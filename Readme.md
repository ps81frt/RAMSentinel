# 🛡️ RamSentinel v1.1.0 — Documentation Technique Avancée (Édition Portable)

**RamSentinel** n'est pas un simple "RAM Cleaner". C'est un orchestrateur de ressources de bas niveau conçu pour les ingénieurs système et les utilisateurs exigeants. Il agit comme une interface directe avec le **Memory Manager (VMM)** de Windows, utilisant des primitives système privilégiées pour forcer la réallocation des pages physiques sans les instabilités liées aux méthodes de purge classiques.

---

## 🧬 Architecture Interne & Portabilité

Cette build est un exécutable **Single-File Self-Contained**. 
- **Runtime-Less** : Le CLR .NET 8 est virtualisé et embarqué. Aucune installation de dépendance n'est requise.
- **Native Bridge** : La logique de calcul est déportée dans une couche C++ hautement optimisée, liée statiquement pour éviter les dépendances au MSVC Redistributable.
- **Zero-Footprint** : Aucun service n'est installé. L'application réside entièrement en mémoire après le chargement.

---

## ⚙️ Mécanique d'Optimisation (Vecteurs d'Intervention)

RamSentinel manipule les listes de pages du noyau via des transitions d'état spécifiques :

### 1. Gestion du Working Set (Ensemble de travail)
- **Fonction** : `RS_TrimProcessWorkingSet`
- **Méthode** : Invoque `SetProcessWorkingSetSizeEx` avec les flags `QUOTA_LIMITS_HARDWS_MIN_DISABLE` et `QUOTA_LIMITS_HARDWS_MAX_DISABLE`.
- **Action** : Force le VMM à vider les pages privées non-résidentes des processus vers la liste **Modified Page List**. Cela réduit l'empreinte physique sans suspendre l'exécution, permettant au système de réallouer ces cadres (frames) aux processus prioritaires.

### 2. Auto-Purge Intelligent (Background Monitor)
- **Logique** : Surveillance asynchrone du seuil de saturation du cache.
- **Seuil Configurable** : Permet de définir une limite stricte (ex: 2.0 Go). Dès que la liste Standby dépasse cette valeur, une purge est déclenchée automatiquement.
- **Optimisation Silencieuse** : Conçu pour fonctionner lorsque l'application est réduite, garantissant que la RAM est toujours prête avant même que l'utilisateur ne constate un ralentissement.
- **Protection Thrashing** : Incorpore un cooldown de sécurité pour éviter les purges en boucle qui pourraient solliciter inutilement le processeur.

### 3. Gestion Avancée des Listes de Pages (VMM)
RamSentinel active le privilège `SeProfileSingleProcessPrivilege` pour accéder à `NtSetSystemInformation` :

*   **Standby List (Cache de lecture)** : Contient des données déjà lues sur disque mais inutilisées. Bien qu'elle soit considérée comme "disponible" par Windows, une liste Standby trop volumineuse et fragmentée impose un coût processeur non négligeable lors de sa réallocation. La purge (`MemoryPurgeStandbyList`) bascule ces pages vers la liste **Free**, supprimant la latence de transition lors du lancement d'applications lourdes.
*   **Modified List (Cache d'écriture)** : Contient des données modifiées en RAM qui n'ont pas encore été écrites sur le disque ou le fichier d'échange (Pagefile). Contrairement à la liste Standby, ces pages **ne peuvent pas** être réutilisées immédiatement par le système. RamSentinel utilise `MemoryFlushModifiedList` pour forcer l'écriture immédiate de ces pages, permettant leur passage en mode Standby puis Free.

| État de la page | Utilisable immédiatement ? | Action RamSentinel |
| :--- | :---: | :--- |
| **Active** | Non | Trim (Working Set) |
| **Modified** | **Non (Doit être écrite)** | **Flush (Forcer l'écriture)** |
| **Standby** | Oui (mais avec latence) | **Purge (Instantanéisation)** |
| **Free / Zero** | Oui | (Cible finale de l'optimisation) |

---

## 📊 Intelligence Artificielle & Diagnostics

### Algorithme de Pression (Multi-Vecteurs)
Le score de pression n'est pas un simple ratio d'utilisation. Il est calculé sur une moyenne glissante :
$$Score = (UsageRAM \times 0.60) + (UsagePagefile \times 0.25) + (CommitRatio \times 0.15)$$
Cela permet de détecter une saturation de la **Commit Charge** (souvent invisible pour l'utilisateur) avant que le système ne commence à tuer des processus (OOM).

### Corrélation d'Événements (Event Logs)
Le moteur surveille les journaux `System` et `Application` pour capturer en temps réel :
- **Event ID 2004 (Resource-Exhaustion-Detector)** : Identification chirurgicale du processus responsable d'une fuite mémoire.
- **Event ID 1000/1001 (Application Error)** : Corrélation entre un pic de pression et un crash applicatif pour diagnostic post-mortem.

---

## 🛡️ Sécurité et Analyse Forensique

Le module VirusTotal utilise une approche sécurisée :
1. **Hachage Local** : L'empreinte SHA-256 est générée localement. Le fichier n'est **jamais** envoyé sur internet.
2. **Analyse de Réputation** : Seul le hash est comparé via l'API REST de VirusTotal.
3. **Identification des Menaces** : Permet de distinguer un processus système légitime d'un mineur de crypto-monnaie ou d'un injecteur de code résidant en mémoire.

---

## 🔔 Système de Notifications (Tray)

RamSentinel utilise le système de **Balloon Tips** de Windows pour communiquer de manière non-intrusive :
- **Alertes Auto-Purge** : Une notification s'affiche lorsque le seuil de cache est atteint et qu'un nettoyage automatique est effectué en arrière-plan.
- **Statut d'Optimisation** : Confirmation visuelle des gains de mémoire après une opération "Smart Optimize".
- **Mode Discret** : L'application disparaît de la barre des tâches lors de la réduction pour se loger dans la zone de notification (System Tray).

---

## 🚀 Spécifications de Déploiement

### Exigences Système
- **OS** : Windows 10 (Build 19041+) / Windows 11.
- **Architecture** : x86-64 (Natif).
- **Privilèges** : **Élévation UAC requise**. Sans droits administrateur, les fonctions `NtSetSystemInformation` (Purge Standby) seront désactivées par le noyau.

### Utilisation en Ligne de Commande (CLI)
Bien que l'interface soit WPF, RamSentinel supporte l'injection de paramètres pour les environnements de maintenance (à venir dans la v1.1).

---

## ❓ F.A.Q : Comprendre la RAM "Standby" vs "Libre"

**L'analogie du Bureau :**

1.  **RAM Libre (Free List)** : C'est la surface de votre bureau qui est **totalement vide**. Vous pouvez y poser un nouveau dossier instantanément sans aucune friction.
2.  **RAM Standby (Cache)** : Ce sont des dossiers que vous avez consultés plus tôt. Windows les laisse sur un coin du bureau "au cas où". 
    *   *L'avantage* : Si vous rouvrez le même dossier, il est déjà là (chargement ultra-rapide).
    *   *Le problème* : Si vous voulez poser un tout nouvel objet très lourd (un jeu AAA, un logiciel de montage), Windows doit d'abord **pousser les vieux dossiers** pour faire de la place. 

Ce petit temps nécessaire pour "pousser les dossiers" s'appelle la **latence de réallocation**. C'est elle qui cause les micro-saccades (*stuttering*) en plein jeu. **RamSentinel** vide ce coin du bureau de manière chirurgicale pour que votre système trouve toujours une surface vide et prête.

---

## 🛠️ Dépannage (Troubleshooting)

### 1. Privilège `SeDebugPrivilege` ou `SeProfileSingleProcess` manquant
RamSentinel tente d'élever ses propres privilèges au démarrage. Si vous voyez une erreur liée aux privilèges :
*   **Solution** : Assurez-vous que l'exécutable est lancé via "Exécuter en tant qu'administrateur".
*   **Cause** : Certains antivirus ou politiques de groupe (GPO) restrictives peuvent bloquer l'acquisition de privilèges de débogage, empêchant le "Trim" des processus système.

### 2. "Erreur d'initialisation NTAPI"
Si les fonctions de purge Standby ne répondent pas :
*   **Cause** : Vous utilisez peut-être une version de Windows familiale avec des restrictions sur les appels `NtSetSystemInformation`.
*   **Solution** : Vérifiez que votre build Windows est supérieure à 19041.

### 3. Faux positifs Antivirus (SmartScreen)
En raison de sa nature (Single-File EXE) et de ses requêtes de droits Administrateur pour manipuler la mémoire :
*   **Solution** : Ajoutez `RamSentinel.exe` aux exclusions de votre antivirus ou cliquez sur "Informations complémentaires" > "Exécuter quand même" sur l'écran bleu SmartScreen.

---

## 📜 Licence, Auteur et Éthique

**Développeur Principal** : ps81frt  
**Dépôt Officiel** : github.com/ps81frt/RAMSentinel

### Licence MIT
Copyright (c) 2024 ps81frt

### Engagement de Transparence
Contrairement aux outils "bloatware", RamSentinel n'effectue aucune télémétrie cachée. 
1. **Zéro Collecte** : Aucune donnée personnelle n'est transmise.
2. **Code Ouvert** : La logique d'optimisation est auditable sur le dépôt source.
3. **Sécurité d'Abord** : Un cooldown de 30 secondes protège le VMM contre les purges excessives (Thrashing).

---
*RamSentinel — L'ingénierie de précision au service de votre système.*
