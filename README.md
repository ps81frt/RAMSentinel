# RamSentinel

> Outil professionnel d'analyse, d'optimisation et de forensique mémoire RAM pour Windows — interface WPF native (.NET 8) avec moteur natif C++ haute performance.  
> Auteur : [ps81frt](https://github.com/ps81frt) · Dépôt source : [github.com/ps81frt/ramsentinel_source](https://github.com/ps81frt/ramsentinel_source)

---

## 📊 Statut

![Platform](https://img.shields.io/badge/platform-Windows%2010%2F11%20x64-brightgreen)
![Language C#](https://img.shields.io/badge/C%23-94.8%25-blueviolet)
![Language C++](https://img.shields.io/badge/C%2B%2B-0.9%25-blue)
![Language PowerShell](https://img.shields.io/badge/PowerShell-4.3%25-blue)
![Framework](https://img.shields.io/badge/.NET-8.0--windows-purple)
![License](https://img.shields.io/badge/license-MIT-green)
![Stars](https://img.shields.io/github/stars/ps81frt/ramsentinel_source)
![Forks](https://img.shields.io/github/forks/ps81frt/ramsentinel_source)
![Watchers](https://img.shields.io/github/watchers/ps81frt/ramsentinel_source)
![Contributors](https://img.shields.io/github/contributors/ps81frt/ramsentinel_source)
![Issues](https://img.shields.io/github/issues/ps81frt/ramsentinel_source)
![Last commit](https://img.shields.io/github/last-commit/ps81frt/ramsentinel_source)
![Repo size](https://img.shields.io/github/repo-size/ps81frt/ramsentinel_source)
![Views](https://komarev.com/ghpvc/?username=ps81frt&repo=ramsentinel_source)

<!-- Décommenter après la première release publiée :
![Release](https://img.shields.io/github/v/release/ps81frt/ramsentinel_source)
![Downloads](https://img.shields.io/github/downloads/ps81frt/ramsentinel_source/total)
[![Download](https://img.shields.io/badge/Download-latest-blue)](https://github.com/ps81frt/ramsentinel_source/releases/latest)
-->

---

## Version

**Version actuelle : 2.0.0**  
Historique complet disponible dans [CHANGELOG.md](https://github.com/ps81frt/ramsentinel_source/blob/main/CHANGELOG.md).

---

## Présentation du projet

RamSentinel est une application Windows de surveillance et d'optimisation de la mémoire RAM orientée usage avancé — techniciens, power users, analystes SOC et intervenants forensiques. Elle expose en temps réel l'état complet du sous-système mémoire Windows : pression, standby, processus suspects, fuites, injections de DLL, hollowing de processus, connexions réseau par PID, et bien plus.



Le projet ne fait **aucune modification système permanente** — aucun service installé, aucune entrée de registre créée. Il peut fonctionner en mode portable (`.exe` auto-contenu) sans installation.


### Cas d'usage typiques

- **Surveillance RAM temps réel** : pression mémoire, standby list, processus par consommation, score de danger composite.
- **Optimisation gaming ou production** : trim des working sets, flush des pages modifiées, purge de la Standby List, Gaming Mode en un clic.
- **Forensique et réponse à incident** : inspection mémoire par processus (carte mémoire, modules chargés, connexions réseau, anti-hollowing, détection d'injection DLL), export rapports HTML ou JSON.
- **Diagnostic de fuites mémoire** : surveillance continue du working set avec historique 10 min, calcul du taux de croissance, alertes par niveau de sévérité.
- **Vérification d'intégrité** : hash SHA-256 de l'exécutable portable, vérification Authenticode des modules chargés, scan VirusTotal intégré.

---

## Table des matières

- [Architecture globale](#architecture-globale)
- [Structure détaillée du dépôt](#structure-détaillée-du-dépôt)
  - [Racine](#racine)
- [Vérification d'intégrité du portable](#vérification-dintégrité-du-portable)
- [Interface utilisateur](#interface-utilisateur)
- [Actions globales](#actions-globales)
- [App/Views/Popups/](#appviewspopups)
- [Menu contextuel — clic droit sur un processus](#menu-contextuel--clic-droit-sur-un-processus)
  - [CR-01 — Carte mémoire](#cr-01--carte-mémoire-memorymapwindow)
  - [CR-02 — Working Set détaillé](#cr-02--working-set-détaillé-workingsetdetailwindow)
  - [CR-03 — Modules / DLL chargés](#cr-03--modules--dll-chargés-loadedmoduleswindow)
  - [CR-04 — Fichiers mappés](#cr-04--fichiers-mappés-mappedfileswindow)
  - [CR-05 — Segments Heap](#cr-05--segments-heap-heapsegmentswindow)
  - [CR-06 — Commit vs Private](#cr-06--commit-vs-private-commitprivatewindow)
  - [CR-07 — Page Faults](#cr-07--page-faults-pagefaultwindow)
  - [CR-08 — Suspendre / Reprendre](#cr-08--suspendre--reprendre-suspendresumewindow)
  - [CR-09 — MiniDump](#cr-09--minidump-minidumpwindow)
  - [CR-10 — Handles](#cr-10--handles-handlelistwindow)
  - [CR-11 — Threads](#cr-11--threads-threadlistwindow)
  - [CR-12 — Priorité](#cr-12--priorité-prioritywindow)
  - [CR-13 — Affinité CPU](#cr-13--affinité-cpu-affinitywindow)
  - [CR-14 — Token de sécurité](#cr-14--token-de-sécurité-securitytokenwindow)
  - [CR-15 — Authenticode](#cr-15--authenticode-authenticodewindow)
  - [CR-16 — Arbre de processus](#cr-16--arbre-de-processus-processtreewindow)
  - [CR-17 — Connexions réseau](#cr-17--connexions-réseau-networkconnectionswindow)
  - [CR-18 — Strings mémoire](#cr-18--strings-mémoire-memorystringswindow)
  - [CR-19 — Score de dangerosité](#cr-19--score-de-dangerosité-dangerscorewindow)
  - [CR-20 — Détection de fuite mémoire](#cr-20--détection-de-fuite-mémoire-leakdetectionwindow)
  - [CR-21 — Historique Working Set](#cr-21--historique-working-set-wshistorywindow)
  - [CR-22 — Export Forensic JSON](#cr-22--export-forensic-json-forensicexportwindow)
  - [CR-23 — Gaming Mode](#cr-23--gaming-mode-gamingmodewindow)
  - [CR-25 — Anti-Hollowing](#cr-25--anti-hollowing-antihollowingwindow)
- [VirusTotal](#virustotal)
- [Rapports HTML forensiques](#rapports-html-forensiques)
- [Fichiers créés sur la machine](#fichiers-créés-sur-la-machine)
- [Nettoyage après intervention](#nettoyage-après-intervention)
- [Architecture technique détaillée](#architecture-technique-détaillée)
- [Compatibilité](#compatibilité)
- [FAQ / Dépannage](#faq--dépannage)
- [Gestion des erreurs](#gestion-des-erreurs)
- [Contributions](#contributions)
- [⚠️ Avertissement antivirus](#️-avertissement-antivirus)
- [Licence](#licence)
- [Auteur / Contact](#auteur--contact)

---

## Architecture globale

```
┌──────────────────────────────────────────────────────────────────────┐
│                         RamSentinel v2.0                             │
│                                                                      │
│  ┌─────────────────────────────────────────────────────────────────┐ │
│  │                    Couche WPF / C# (.NET 8)                     │ │
│  │                                                                 │ │
│  │  MainWindow ──► MainViewModel ──► MemoryAnalyzer               │ │
│  │       │               │                  │                     │ │
│  │  Panneau gauche   25 RelayCommands   RefreshAsync()            │ │
│  │  Liste processus  (CR-01..CR-25)     └──► WsHistoryService     │ │
│  │  Barre d'état                        └──► DangerScoreService   │ │
│  │                                      └──► LeakDetectionService │ │
│  │  Services transverses :                                         │ │
│  │   LoggingService · CrashLogService · ForensicExportService     │ │
│  │   GamingModeService · VirusTotalService · HtmlReport*          │ │
│  │                                                                 │ │
│  │  25 Fenêtres Popup (CR-01..CR-25) — XAML + code-behind         │ │
│  └───────────────────────────┬─────────────────────────────────────┘ │
│                              │ P/Invoke                              │
│  ┌───────────────────────────▼─────────────────────────────────────┐ │
│  │              RamSentinelCore.dll  (C++ x64 natif)               │ │
│  │                                                                 │ │
│  │  RS_EmptyStandbyList · VirtualQueryEx  · GetMappedFileName      │ │
│  │  NtSuspendProcess    · NtResumeProcess · QueryWorkingSetEx      │ │
│  │  NtQuerySystemInformation            · MiniDumpWriteDump        │ │
│  └─────────────────────────────────────────────────────────────────┘ │
│                                                                      │
│  Sortie fichiers :                                                   │
│   %LOCALAPPDATA%\RamSentinel\Reports\   ← Rapports HTML / JSON      │
│   %LOCALAPPDATA%\RamSentinel\Logs\      ← Journaux d'exécution      │
│   %LOCALAPPDATA%\RamSentinel\CrashLogs\ ← Crash logs               │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Structure détaillée du dépôt

### Racine

```
RAMSentinel_Portable/
├── ram2.png                  # Capture écran menu contextuel
├── RamSentinel.exe           # EXE auto-contenu — aucune dépendance externe
├── RamSentinel.exe.sha256    # Hash au format certutil-compatible
├── Readme.md                 # Manuel utilisateur portable
├── rm1.png                   # Capture écran interface principale
└── SHA256.txt                # Hash SHA-256 de référence (format lisible)
```

---

## Vérification d'intégrité du portable

```powershell
# Via certutil (Windows natif)
certutil -hashfile .\RamSentinel.exe SHA256

# Via PowerShell
(Get-FileHash .\RamSentinel.exe -Algorithm SHA256).Hash
```

Comparer le résultat avec le contenu de `SHA256.txt` ou `RamSentinel.exe.sha256`.

---

## Interface utilisateur

L'interface est organisée en trois zones principales.

**Panneau latéral gauche** — affiche en permanence : RAM totale / utilisée / disponible / Standby / modifiée, score de pression mémoire, boutons d'action globale, section VirusTotal API, badges de statut (🔴 fuites, 🟡 suspects) et événements système récents.

**Liste centrale des processus** — DataGrid temps réel : PID, nom, chemin, consommation mémoire (Working Set), score de danger 0–100, indicateurs de fuite et de suspicion. Un clic droit ouvre le menu contextuel complet.

**Barre d'état** — affiche les messages de résultat des actions, erreurs d'accès et confirmations d'opérations.

---

## Actions globales

Ces boutons sont disponibles en permanence dans le panneau latéral gauche, indépendamment de tout processus sélectionné. Ils agissent sur l'ensemble du système ou sur la liste complète des processus.

---

### Rafraîchir

Déclenche manuellement un cycle complet de collecte. `MemoryAnalyzer.Refresh()` appelle `NativeCore.RS_GetMemorySnapshot()` pour reconstruire les métriques RAM (total, utilisé, disponible, Standby estimé, score de pression via `RS_GetMemoryPressureScore()`). La liste des processus est reconstruite via `NativeCore.RS_GetProcessList()`, qui inclut pour chaque processus son nom, PID, Working Set, Private Bytes, Peak WS, Virtual Size, Paged Pool, Non-Paged Pool et handle count. Le token de sécurité est interrogé pour afficher l'utilisateur propriétaire. Les badges fuites et suspects dans le panneau gauche sont recalculés.

À utiliser immédiatement après une action d'optimisation (Trim, Vider Standby, Gaming Mode) pour constater l'effet sans attendre le cycle automatique de ~5 secondes.

---

### Trim Top

Réduit le Working Set des processus les plus consommateurs via `SetProcessWorkingSetSizeEx(handle, -1, -1, 0)` — les paramètres `-1, -1` indiquent au kernel de réduire le Working Set au minimum possible, forçant les pages inactives à sortir du jeu de travail physique. Seuls les processus marqués `CanTrim = true` par le moteur natif sont traités.

Les processus continuent de tourner normalement et reprennent leurs pages depuis le cache Standby ou le disque dès qu'ils en ont besoin. Aucun processus n'est tué ni suspendu.

---

### Trim Sélection

Même opération que Trim Top (`SetProcessWorkingSetSizeEx` avec `-1, -1`) mais ciblée sur le seul processus sélectionné dans la DataGrid. Accessible aussi via le menu contextuel → **Forcer le Trim**.

---

### Auto-purge Standby

Purge automatique périodique de la Standby List, déclenchée selon deux conditions configurables dans `SettingsService` (`settings.json`) :

- `AutoPurgeIntervalMinutes` — intervalle minimum entre deux purges (défaut : 30 min). Le timer ne déclenche pas si le délai n'est pas écoulé.
- `AutoPurgePressureThreshold` — seuil de pression RAM en % (défaut : 85%). La purge n'est exécutée que si la pression courante dépasse ce seuil ET que l'intervalle est écoulé. Les deux conditions sont cumulatives.

**Ce que fait la purge :** `NativeCore.RS_EmptyStandbyList()` appelle `NtSetSystemInformation` avec la classe `SystemMemoryListInformation` et la commande `MemoryPurgeStandbyList` — instruction directe au kernel de vider la Standby List et de transférer ces pages vers la liste Free. Les pages Modified (non encore écrites sur le pagefile) sont d'abord flushées avant d'être libérées. L'effet est visible immédiatement dans `PhysicalMemoryWindow` : Standby diminue, Free augmente.

**Quand l'utiliser :** Systèmes avec peu de RAM totale où la Standby croît jusqu'à saturation, ou environnements de production où la pression dépasse 85% régulièrement sans que les processus en aient réellement besoin (ex : SQL Server ou JVM qui garde des caches larges). À éviter sur les systèmes avec I/O disque lente — forcer le vidage du cache Standby oblige le kernel à relire depuis le disque ce qu'il avait gardé en mémoire.

**Droits requis :** Administrateur obligatoire — `NtSetSystemInformation` avec `SystemMemoryListInformation` est bloqué par le kernel sans `SeIncreaseBasePriorityPrivilege` / `SeProfileSingleProcessPrivilege`. Sans élévation, `RS_EmptyStandbyList()` retourne `false` et le statut est loggé. Le bouton reste disponible mais l'opération échoue silencieusement avec message dans la barre d'état.

---

### Gaming Mode

Appelle `GamingModeService.ActivateAsync()` qui effectue dans l'ordre :

1. Pour chaque processus de la liste dont le nom n'est pas dans la whitelist (`SettingsService.Settings.GamingModeWhitelist`) et dont `CanTrim` est `true` : `OpenProcess(PROCESS_SET_QUOTA | PROCESS_QUERY_INFORMATION)` puis `SetProcessWorkingSetSizeEx(handle, -1, -1, 0)`.
2. `NativeCore.RS_EmptyStandbyList()` — purge de la Standby List via `NtSetSystemInformation`.

La whitelist par défaut protège : `explorer`, `dwm`, `csrss`, `lsass`, `winlogon`, `services`, `svchost`, `ntoskrnl`, `audiodg`, `nvcontainer`, `nvidia`, `amdow`, `igfx`, `steam`, `gameoverlayui`, `discord`. Elle est configurable dans les paramètres (`settings.json`).

La fenêtre de résultat (`GamingModeWindow`, CR-23) affiche le nombre de processus trimmés, les Mo libérés estimés (différence de Working Set avant/après par processus), si la purge Standby a réussi, et les éventuelles erreurs d'accès.

---

### Export historique CSV

Exporte l'historique de pression mémoire enregistré par `MemoryAnalyzer` depuis le démarrage de l'application. `MemoryAnalyzer` maintient un buffer circulaire de 300 snapshots (5 minutes à raison d'un toutes les secondes), chacun contenant : horodatage, RAM totale, utilisée, disponible, Standby, page file total/utilisé, commit total/limite, usage %, score de pression. L'export CSV ouvre une boîte de dialogue de sauvegarde et écrit ces colonnes ligne par ligne.

---

## Menu contextuel — clic droit sur un processus

Un clic droit sur n'importe quel processus de la liste ouvre un menu hiérarchique organisé en groupes. Toutes les fenêtres s'ouvrent en popup non-bloquant — plusieurs peuvent être ouvertes simultanément pendant que la liste continue de se rafraîchir.

---
#### App/Views/Popups/

Chaque popup correspond à une commande CR et s'ouvre via le menu contextuel de la liste des processus.

| Fenêtre | Commande | Fonction |
|---|---|---|
| `MemoryMapWindow` | CR-01 | Carte mémoire virtuelle — `VirtualQueryEx` : adresse, taille, type, protection, fichier mappé |
| `WorkingSetDetailWindow` | CR-02 | Décomposition Working Set : Private / Shareable / Shared via `QueryWorkingSetEx` |
| `LoadedModulesWindow` | CR-03 | DLL chargées — adresse de base, taille, chemin, signature Authenticode |
| `MappedFilesWindow` | CR-04 | Régions `MEM_MAPPED` — résolution chemin via `GetMappedFileName` |
| `HeapSegmentsWindow` | CR-05 | Estimation des segments heap (régions mémoire privées) |
| `CommitPrivateWindow` | CR-06 | Commit vs Private — barres de progression comparatives |
| `PageFaultWindow` | CR-07 | Compteurs de fautes de page hard/soft avec taux en temps réel (1 sec) |
| `SuspendResumeWindow` | CR-08 | Suspendre / Reprendre processus (`NtSuspendProcess` / `NtResumeProcess`) |
| `MiniDumpWindow` | CR-09 | Création de dump mémoire Normal/Full via `MiniDumpWriteDump` |
| `HandleListWindow` | CR-10 | Nombre total de handles ouverts par le processus |
| `ThreadListWindow` | CR-11 | Liste des threads — TID, priorité, temps CPU user/kernel |
| `PriorityWindow` | CR-12 | Modification de la classe de priorité via `SetPriorityClass` (Idle → Realtime) |
| `AffinityWindow` | CR-13 | Masque d'affinité CPU — CheckBox par cœur, jusqu'à 64 cœurs |
| `SecurityTokenWindow` | CR-14 | Token de sécurité — utilisateur, domaine, élévation, liste des privilèges activés |
| `AuthenticodeWindow` | CR-15 | Vérification signature `WinVerifyTrust` — sujet, émetteur, empreinte, dates |
| `ProcessTreeWindow` | CR-16 | Arbre hiérarchique parent/enfant avec consommation mémoire |
| `NetworkConnectionsWindow` | CR-17 | Connexions TCP/UDP associées au PID avec résolution DNS |
| `MemoryStringsWindow` | CR-18 | Scan mémoire live — URLs, chemins, emails, clés (catégories + regex) |
| `DangerScoreWindow` | CR-19 | Score danger 0–100 — 5 facteurs : VT, modules unsigned, réseau, WS growth, fenêtre absente |
| `LeakDetectionWindow` | CR-20 | Détection fuite — taux croissance Mo/min, niveaux de sévérité colorés |
| `WsHistoryWindow` | CR-21 | Historique Working Set — 120 points à 5 sec (~10 min), Total/Private/Shareable |
| `ForensicExportWindow` | CR-22 | Export JSON forensique complet du processus |
| `GamingModeWindow` | CR-23 | Gaming Mode — trim + purge standby en lot (accessible aussi depuis panneau gauche) |
| `AntiHollowingWindow` | CR-25 | Détection process hollowing — comparaison image disque vs sections en mémoire |
| `KernelDriversWindow` | — | Liste tous les pilotes kernel chargés via `EnumDeviceDrivers` + `GetDeviceDriverBaseName` / `GetDeviceDriverFileName`. Affiche nom, adresse de base (hex), taille (lue sur le fichier disque via `FileInfo`), pages estimées. Filtre texte en temps réel (nom, chemin, adresse hex). Bouton de recherche crash : accepte une adresse hex brute (`0xFFFF...`) ou un format `module.sys+0xoffset` (résolu en adresse absolue via la base du driver). Export CSV. Menu contextuel : copier nom / adresse / chemin, ouvrir dans l'Explorateur. Ouvre les sous-vues : KernelAddressSpaceWindow, KernelDriverPageMapWindow, KernelPhysicalMapWindow |
| `KernelAddressSpaceWindow` | — | Vue Canvas : représente chaque driver comme un bloc coloré positionné proportionnellement à son adresse de base dans l'espace kernel (`baseAddress - minBase / totalSpan × width`). Taille du bloc proportionnelle à la taille du fichier. Tooltip : nom, plage d'adresses, pages, chemin. Affiche la plage complète `min(BaseAddress) — max(BaseAddress + Size)` et le total de pages estimé |
| `KernelDriverPageMapWindow` | — | Même rendu Canvas que KernelAddressSpace mais les pilotes sont ordonnés par adresse de base croissante. Affiche en légende le total de pages et la plage d'adresses. La DataGrid liste chaque pilote avec ses détails |
| `KernelPhysicalMapWindow` | — | Approximation de la disposition physique : les drivers sont répartis séquentiellement dans l'espace physique total (`TotalBytes / pageSize` pages). Chaque driver occupe une plage PFN (`pfnStart` à `pfnEnd`) calculée en fonction de son nombre de pages. Le Canvas affiche chaque plage colorée proportionnellement au volume de RAM total. Tooltip : nom, PFN start/end, plage d'adresses physiques |
| `PhysicalPageMapWindow` | — | Vue page-level par processus : `GetProcessPageMap(pid, maxPages=1024)` parcourt `VirtualQueryEx` sur les régions `Commit`, puis échantillonne les pages avec `QueryWorkingSetEx` par blocs de 128 pour tester la résidence (`VirtualAttributes & 0x1`). Affiche une grille de cellules colorées (vert = Private résidente, bleu = Mapped/Image résidente, gris = non résidente). Statistiques : pages échantillonnées, % résidentes, top sources. Export CSV |
| `PhysicalMemoryWindow` | — | Vue de l'état physique complet de la RAM système, indépendante de tout processus. Appelle `GetPhysicalMemoryBreakdown()` via `NtQuerySystemInformation(SystemPerformanceInformation)` — lecture directe des compteurs kernel. Cinq valeurs affichées avec pourcentage et barre proportionnelle (`GridLength Star`) : **Total** (RAM physique installée), **Active** (pages en Working Set d'un processus actif — coût de reprise = 0), **Standby** (pages libérées mais encore en cache RAM — récupérables sans I/O disque, recyclables immédiatement par le kernel), **Modified** (pages modifiées non encore écrites sur le pagefile — dette d'écriture, doivent être flushées avant réaffectation, cible directe de `RS_EmptyStandbyList`), **Free** (pages vraiment disponibles immédiatement). Affiche aussi le **score de pression mémoire** (`PressureDisplay`). Bouton Rafraîchir : re-appel complet de `GetPhysicalMemoryBreakdown()` — utile pour mesurer l'effet immédiat d'une purge Standby ou d'un trim de Working Set. Point clé : la Standby n'est pas de la RAM perdue, c'est du cache kernel ; la Modified est la dette à vider ; seule la Free est réellement libre sans aucun coût. |
| `MappedFilesWindow` | CR-04 | Filtre exclusif des régions `MEM_MAPPED` avec chemin disque résolu. Appliqué sur les résultats bruts de `GetMemoryRegions` : seules les régions dont `Type == "Mapped"` ET `MappedFile` non vide (résolution via `GetMappedFileName`, device path normalisé en chemin Win32) sont retenues. Colonnes : adresse de base (hex), taille octets + format lisible, chemin disque complet. Cibles normales : fichiers de bases de données SQLite/Chromium (`.db`), fichiers de configuration mmap, DLL de ressources, fichiers de données jeux ou navigateurs. Signal forensique : tout fichier mappé depuis `%TEMP%`, `%APPDATA%`, chemin sans extension dans un répertoire utilisateur — technique d'injection de payload sans `LoadLibrary` détectable, le fichier PE est mappé directement comme section anonyme. |

---
## CR-01 — Carte mémoire (`MemoryMapWindow`)

**Ce que l'utilisateur voit**

Un tableau de toutes les régions mémoire virtuelles du processus, avec pour chaque ligne : adresse de base (hex), taille en octets et en format lisible, état, type, protection, et chemin du fichier mappé si applicable. En haut de la fenêtre, un résumé de l'état physique global de la machine (Active / Standby / Free / Modified avec pourcentages).

**Ce que l'utilisateur peut faire**

- **Exporter en CSV** — toutes les régions avec leurs colonnes complètes
- **Copier l'adresse** d'une région sélectionnée (clic droit sur la ligne)
- **Rechercher une adresse crash** — saisir une adresse hex brute (`0xFFFF800012345678`) ou un format `module.sys+0xoffset` pour identifier le driver kernel correspondant ; ouvre `KernelDriversWindow` pré-rempli
- **Utiliser l'adresse sélectionnée** comme adresse de recherche crash directement depuis la ligne (menu clic droit → Rechercher adresse)
- **Ouvrir la vue physique** (`PhysicalMemoryWindow`) — état complet Active / Standby / Modified / Free de la RAM système via `NtQuerySystemInformation(SystemPerformanceInformation)`. Standby = cache kernel réutilisable sans I/O. Modified = dette d'écriture pagefile. Free = RAM vraiment disponible. Barre de répartition proportionnelle + score de pression. Bouton Rafraîchir pour mesurer l'effet d'une purge immédiate.
- **Ouvrir les pilotes kernel** (`KernelDriversWindow`) — liste complète des drivers kernel chargés via `EnumDeviceDrivers` + `GetDeviceDriverBaseName/FileName`. Nom, adresse de base (hex), taille (lue sur le fichier disque via `FileInfo`), pages estimées. Filtre texte temps réel sur nom / chemin / adresse hex. Recherche crash : accepte `0xFFFF800012345678` ou `ntoskrnl.exe+0x1A2B3` (résolu en adresse absolue via la base du driver). Sous-vues : `KernelAddressSpaceWindow` (canvas proportionnel d'adressage kernel), `KernelDriverPageMapWindow` (même canvas trié par adresse croissante avec DataGrid), `KernelPhysicalMapWindow` (approximation PFN — répartition séquentielle dans l'espace physique total calculée depuis `TotalBytes / pageSize`). Export CSV des drivers ou du résultat de matching crash. Menu contextuel : copier nom / adresse / chemin, ouvrir dans l'Explorateur.
- **Ouvrir la cartographie page-level du processus** (`PhysicalPageMapWindow`) — `GetProcessPageMap(pid)` parcourt `VirtualQueryEx` sur les régions `Commit` et échantillonne jusqu'à 1024 pages via `QueryWorkingSetEx` par blocs de 128. Bit `VirtualAttributes & 0x1` = page résidente. Grille de cellules colorées : vert (Private résidente), bleu (Mapped/Image résidente), gris (non résidente). Statistiques : total échantillonné, % résidentes, top sources par type de région. Export CSV.
- **Ouvrir les fichiers mappés** (`MappedFilesWindow`, CR-04) — filtre des régions `MEM_MAPPED` avec chemin disque résolu via `GetMappedFileName`. Signal forensique si mappé depuis `%TEMP%` ou `%APPDATA%` — technique d'injection sans `LoadLibrary` détectable.

**Données collectées**

`VirtualQueryEx` en boucle sur l'espace d'adressage jusqu'à épuisement de `0x0` à `0x7FFFFFFFFFFFFFFF`. Chaque `MEMORY_BASIC_INFORMATION` expose : `State` (`MEM_COMMIT` / `MEM_RESERVE` / `MEM_FREE`), `Type` (`MEM_PRIVATE` / `MEM_MAPPED` / `MEM_IMAGE`), `Protect` décodé en flags lisibles (`PAGE_READONLY` → `R`, `PAGE_READWRITE` → `RW`, `PAGE_EXECUTE_READ` → `RX`, `PAGE_EXECUTE_READWRITE` → `RWX`, `PAGE_NOACCESS`, `PAGE_WRITECOPY` + modificateurs `PAGE_GUARD`, `PAGE_NOCACHE`, `PAGE_WRITECOMBINE`). `BaseAddress` en hex 16 chiffres. `RegionSize` en octets et format lisible. Pour les régions `MEM_MAPPED`, `GetMappedFileName` résout le device path (`\Device\HarddiskVolume3\...`) normalisé en chemin Win32 (`C:\...`). Le résumé physique en haut de fenêtre est collecté séparément via `GetPhysicalMemoryBreakdown()` à l'ouverture de la fenêtre.

---

## CR-02 — Working Set détaillé (`WorkingSetDetailWindow`)

**Ce que l'utilisateur voit**

Trois compteurs distincts — Private, Shareable, Shared — exprimés en octets et en pourcentage du Working Set total. Plus le pic historique de Working Set et le compteur total de fautes de page.

**Ce que l'utilisateur peut faire**

Lire la décomposition et comprendre si un processus consomme de la RAM qui lui est exclusive (Private) ou s'il tire principalement sur des pages partagées (code de DLL, fichiers mappés).

**Données collectées**

`GetProcessMemoryInfo` (`PROCESS_MEMORY_COUNTERS_EX`) pour le pic WS et `PageFaultCount`. La décomposition est calculée depuis `VirtualQueryEx` : `Commit+Private` → Private, `Commit+Image` → Shareable (pages de code DLL), `Commit+Mapped` → Shared. `PrivateUsage` (Private Bytes) vient de `PROCESS_MEMORY_COUNTERS_EX.PrivateUsage`.

---

## CR-03 — Modules / DLL chargés (`LoadedModulesWindow`)

**Ce que l'utilisateur voit**

La liste de tous les modules chargés dans l'espace d'adressage du processus, triée par taille décroissante. Pour chaque module : nom, adresse de base (hex), taille en mémoire, chemin complet sur disque, statut de signature (Signé / Non signé / Suspect), et nom du signataire si disponible.

**Ce que l'utilisateur peut faire**

Repérer immédiatement un module non signé ou chargé depuis un répertoire inhabituel (`%TEMP%`, `%APPDATA%`, `%LOCALAPPDATA%`), signalé en rouge comme `IsSuspect`.

**Données collectées**

`EnumProcessModules` pour la liste des handles de modules, `GetModuleFileNameEx` pour le chemin, `GetModuleInformation` pour l'adresse de base (`lpBaseOfDll`) et la taille (`SizeOfImage`). La signature est vérifiée rapidement via `X509Certificate` (sans appel `WinVerifyTrust` complet). `IsSuspect = true` si non signé **ou** si le chemin commence par `%TEMP%`, `%APPDATA%` ou `%LOCALAPPDATA%`.

---

## CR-04 — Fichiers mappés (`MappedFilesWindow`)

**Ce que l'utilisateur voit**

La liste de tous les fichiers actuellement mappés en mémoire par le processus. Pour chaque entrée : adresse de base (hex), taille en octets et format lisible, chemin disque complet normalisé (device path `\Device\HarddiskVolume3\...` → chemin Win32 `C:\...`). Les cibles normales incluent les bases de données Chromium/SQLite (`.db`, `.sqlite`), les fichiers de configuration mmap, les DLL de ressources pures (`.mui`, `.dll` de localisation), et les fichiers de données jeux ou navigateurs.

**Ce que l'utilisateur peut faire**

Identifier des fichiers mappés inattendus — un payload mappé depuis `%TEMP%`, `%APPDATA%`, ou un fichier sans extension dans un répertoire utilisateur est un signal d'alerte forensique fort. Technique classique d'injection sans `LoadLibrary` : le PE est mappé manuellement comme section `MEM_MAPPED` anonyme, il n'apparaît pas dans `EnumProcessModules` (CR-03) mais est visible ici. Croiser avec CR-03 (modules) et CR-25 (anti-hollowing) pour confirmer une injection.

**Données collectées**

Post-filtre sur les résultats bruts de `GetMemoryRegions` (mêmes données que CR-01) : seules les régions dont `Type == "Mapped"` (flag `MEM_MAPPED` dans `MEMORY_BASIC_INFORMATION.Type`) ET dont `GetMappedFileName` retourne un chemin non vide sont retenues. `GetMappedFileName` appelle le kernel via `NtQueryVirtualMemory(MemoryMappedFilenameInformation)` pour résoudre le device path de la section mappée. Les régions `MEM_IMAGE` (DLL chargées via `LoadLibrary`) sont exclues de cette vue — elles apparaissent dans CR-03.

---

## CR-05 — Segments Heap (`HeapSegmentsWindow`)

**Ce que l'utilisateur voit**

Une liste de segments mémoire considérés comme actifs, avec adresse et taille. Tous sont marqués `BUSY`.

**Ce que l'utilisateur peut faire**

Estimer le volume d'allocations heap privées actives et détecter une fragmentation excessive.

**Données collectées**

Approximation : toutes les régions `Commit + Private` issues de `VirtualQueryEx` sont listées comme segments heap. Il n'y a pas d'accès à la structure interne du heap Windows — c'est une estimation par exclusion, documentée comme telle dans le code.

---

## CR-06 — Commit vs Private (`CommitPrivateWindow`)

**Ce que l'utilisateur voit**

Deux valeurs comparées côte à côte avec des barres de progression : octets committés (mémoire réservée auprès du système) et Private Bytes (mémoire physique réellement consommée).

**Ce que l'utilisateur peut faire**

Détecter un écart anormal entre commit et private — un processus qui commit beaucoup sans consommer autant en physique peut indiquer de la réservation spéculative ou des mappings virtuels inactifs.

**Données collectées**

`GetProcessMemoryInfo` (`PROCESS_MEMORY_COUNTERS_EX`) : `PagefileUsage` → octets committés, `PrivateUsage` → Private Bytes. La limite de commit globale n'est pas disponible par processus et n'est pas affichée.

---

## CR-07 — Page Faults (`PageFaultWindow`)

**Ce que l'utilisateur voit**

Le compteur total de fautes de page depuis le démarrage du processus, et un taux calculé en fautes par seconde. La fenêtre se rafraîchit toutes les secondes.

**Ce que l'utilisateur peut faire**

Détecter un processus en train de swapper activement — un taux élevé et croissant de fautes de page explique des ralentissements et indique une pression mémoire réelle sur ce processus.

**Données collectées**

`GetProcessMemoryInfo` (`PROCESS_MEMORY_COUNTERS_EX.PageFaultCount`). Le taux est calculé par différence entre deux lectures séparées par `elapsedSeconds`. Limite : `GetProcessMemoryInfo` ne distingue pas hard faults (lecture depuis disque) et soft faults (page déjà en RAM) — le champ `HardFaultsPerSec` est explicitement à `0` dans le code ; distinguer les deux nécessiterait des Performance Counters séparés.

---

## CR-08 — Suspendre / Reprendre (`SuspendResumeWindow`)

**Ce que l'utilisateur voit**

Deux boutons : Suspendre et Reprendre. L'état courant du processus est indiqué. Si le processus est protégé système, un message d'erreur s'affiche sans qu'aucun appel kernel ne soit fait.

**Ce que l'utilisateur peut faire**

Geler l'exécution complète d'un processus suspect le temps de l'analyser — il reste visible dans la liste, consomme toujours sa mémoire, mais n'exécute plus aucune instruction et ne peut plus communiquer vers l'extérieur ni effacer des traces.

**Données collectées / API utilisées**

`NtSuspendProcess` / `NtResumeProcess` depuis `ntdll.dll`, après `OpenProcess(PROCESS_SUSPEND_RESUME)`. Si ce droit est refusé, fallback sur `PROCESS_ALL_ACCESS`. Liste bloquée dure dans le code : `lsass.exe`, `csrss.exe`, `winlogon.exe`, `services.exe`, `smss.exe`, `wininit.exe`, `system` — une tentative lève `InvalidOperationException` avant tout appel kernel. Le NTSTATUS est vérifié : valeur négative = erreur loggée avec code hex.

---

## CR-09 — MiniDump (`MiniDumpWindow`)

**Ce que l'utilisateur voit**

Deux modes de dump à choisir — Normal et Full — avec une barre de progression pendant la création. Une boîte de dialogue de sauvegarde permet de choisir l'emplacement du fichier `.dmp`.

**Ce que l'utilisateur peut faire**

- **Mode Normal** : dump léger (métadonnées + stacks de threads). Rapide, quelques Mo. Adapté à l'analyse de crash.
- **Mode Full** : dump complet de toute la mémoire privée du processus. Peut faire plusieurs Go. Nécessaire pour l'analyse forensique complète. Analysable avec WinDbg, Visual Studio Debugger, ou tout outil DFIR compatible.
- **Annuler** la création en cours.

**API utilisée**

`MiniDumpWriteDump` depuis `dbghelp.dll`. Processus ouvert avec `PROCESS_QUERY_INFORMATION | PROCESS_VM_READ | 0x001F0000`. Constantes exposées : `MINIDUMP_NORMAL (0x0)`, `MINIDUMP_WITH_FULL_MEMORY (0x2)`. Opération async avec `CancellationToken` et `IProgress<int>` (report à 30% à l'ouverture du fichier, 100% à la fin).

---

## CR-10 — Handles (`HandleListWindow`)

**Ce que l'utilisateur voit**

Le nombre total de handles ouverts par le processus à l'instant du dernier rafraîchissement.

**Ce que l'utilisateur peut faire**

Repérer un processus avec un nombre anormalement élevé de handles — fuite de handles, comportement malveillant qui ouvre des ressources sans les fermer.

**Données collectées**

`HandleCount` issu du snapshot `NativeCore.RS_GetProcessList`. Seul le compteur total est disponible — il n'y a pas d'énumération des handles individuels dans cette vue.

---

## CR-11 — Threads (`ThreadListWindow`)

**Ce que l'utilisateur voit**

La liste de tous les threads du processus avec pour chacun : TID, priorité de base, temps CPU consommé en mode utilisateur et en mode noyau.

**Ce que l'utilisateur peut faire**

Repérer un thread qui monopolise du CPU, détecter un nombre anormal de threads pour un processus qui n'en devrait avoir que quelques-uns, ou identifier des threads système injectés.

**Données collectées**

`CreateToolhelp32Snapshot(TH32CS_SNAPTHREAD, 0)` — snapshot global de tous les threads système, filtré sur `th32OwnerProcessID`. `Thread32First` / `Thread32Next` pour l'énumération. Pour chaque thread : `OpenThread(THREAD_QUERY_INFORMATION)` + `GetThreadTimes` pour les temps CPU kernel et user, stockés en ticks 100ns.

---

## CR-12 — Priorité (`PriorityWindow`)

**Ce que l'utilisateur voit**

La classe de priorité courante du processus et six boutons de choix.

**Ce que l'utilisateur peut faire**

Modifier la classe de priorité parmi six niveaux : `Idle`, `BelowNormal`, `Normal`, `AboveNormal`, `High`, `Realtime`. Utile pour brider un processus gourmand en CPU ou au contraire prioriser une application critique. ⚠️ `Realtime` peut rendre le système instable en privant le scheduler des cœurs nécessaires aux processus système.

**API utilisée**

`GetPriorityClass` pour lecture, `SetPriorityClass` pour écriture. Processus ouvert avec `0x0200 (PROCESS_SET_INFORMATION) | PROCESS_QUERY_INFORMATION`. Constantes : `IDLE(0x40)`, `BELOW_NORMAL(0x4000)`, `NORMAL(0x20)`, `ABOVE_NORMAL(0x8000)`, `HIGH(0x80)`, `REALTIME(0x100)`.

---

## CR-13 — Affinité CPU (`AffinityWindow`)

**Ce que l'utilisateur voit**

Une CheckBox par cœur logique (jusqu'à 64), avec les cœurs actuellement autorisés cochés et les cœurs du système disponibles affichés.

**Ce que l'utilisateur peut faire**

Restreindre ou étendre les cœurs sur lesquels le processus est autorisé à s'exécuter. Utile pour isoler un processus sur des cœurs dédiés, libérer des cœurs pour d'autres applications, ou analyser un comportement multi-thread en limitant la parallélisation.

**API utilisée**

`GetProcessAffinityMask` pour lire `ProcessAffinityMask` et `SystemAffinityMask`. `GetSystemInfo` pour `dwNumberOfProcessors`. `SetProcessAffinityMask` pour appliquer. Processus ouvert avec `0x0200 | PROCESS_QUERY_INFORMATION`.

---

## CR-14 — Token de sécurité (`SecurityTokenWindow`)

**Ce que l'utilisateur voit**

En haut : utilisateur (`DOMAINE\nom`), SID, niveau d'intégrité, type d'élévation. En dessous : liste des groupes du token avec leurs attributs, puis liste complète des privilèges avec leur état (Enabled / Disabled).

**Ce que l'utilisateur peut faire**

Savoir immédiatement si un processus tourne en SYSTEM, s'il a `SeDebugPrivilege` activé, s'il présente une élévation suspecte (Full sans raison), ou si des groupes inattendus figurent dans son token.

**Données collectées**

`OpenProcessToken(TOKEN_QUERY)` puis `GetTokenInformation` sur cinq classes : `TokenUser` (SID résolu via `LookupAccountSid`, retry × 4), `TokenElevationType` (1=Default / 2=Full / 3=Limited), `TokenIntegrityLevel` (SID mappé : `S-1-16-16384`=System, `S-1-16-12288`=High, `S-1-16-8192`=Medium, `S-1-16-4096`=Low), `TokenGroups` (liste SID + attributs), `TokenPrivileges` (noms via `LookupPrivilegeName`, état si bit `SE_PRIVILEGE_ENABLED` positionné).

---

## CR-15 — Authenticode (`AuthenticodeWindow`)

**Ce que l'utilisateur voit**

Statut de signature (valide / révoqué / expiré / non signé), sujet du certificat, émetteur (CA), empreinte SHA-1, dates de début et de fin de validité.

**Ce que l'utilisateur peut faire**

Vérifier instantanément si le binaire d'un processus est signé par un éditeur légitime. Un exécutable non signé ou avec un certificat expiré dans un répertoire système est un signal d'alerte fort.

**API utilisée**

`WinVerifyTrust` avec GUID `00AAC56B-CD44-11d0-8CC2-00C04FC295EE` (WINTRUST_ACTION_GENERIC_VERIFY_V2), `WTD_UI_NONE`, `WTD_REVOKE_NONE`, flags `WTD_CACHE_ONLY_URL_RETRIEVAL | WTD_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT` (pas d'appel réseau). Codes retour : `0x0`=valide, `0x800B010C`=révoqué, `0x800B0101`=expiré. Métadonnées du certificat extraites via `X509Certificate2` (sujet, émetteur, empreinte, dates).

---

## CR-16 — Arbre de processus (`ProcessTreeWindow`)

**Ce que l'utilisateur voit**

L'arbre hiérarchique complet de tous les processus du système, avec le processus sélectionné mis en évidence dans cet arbre. Chaque nœud affiche le nom, le PID, et le Working Set courant.

**Ce que l'utilisateur peut faire**

Vérifier qui a lancé ce processus et quels processus il a lui-même lancé. Détecter une hiérarchie suspecte — par exemple `cmd.exe` enfant de `winword.exe`, ou un processus légitime lancé par un parent inhabituel (technique classique de persistence malveillante).

**Données collectées**

`CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0)` + `Process32First` / `Process32Next` → map `{PID → ProcessTreeNode}` de tous les processus avec leur `th32ParentProcessID`. `BuildTree()` construit récursivement l'arbre à partir du `rootPid`. Working Set de chaque nœud via `Process.GetProcessById().WorkingSet64`.

---

## CR-17 — Connexions réseau (`NetworkConnectionsWindow`)

**Ce que l'utilisateur voit**

La liste de toutes les connexions TCP et sockets UDP du processus. Pour TCP : IP locale, port local, IP distante, port distant, état (ESTABLISHED, LISTEN, TIME_WAIT…), hostname résolu si l'IP est publique. Pour UDP : IP locale et port local uniquement, état `LISTEN`. Les connexions vers des IPs publiques sont signalées visuellement.

**Ce que l'utilisateur peut faire**

Détecter immédiatement un processus qui communique avec l'extérieur — exfiltration de données, C2, beacon. Les IPs publiques sont identifiées et leur hostname résolu automatiquement.

**Données collectées**

`GetExtendedTcpTable(TCP_TABLE_OWNER_PID_ALL, AF_INET)` et `GetExtendedUdpTable(UDP_TABLE_OWNER_PID, AF_INET)` filtrés sur le PID. Ports en big-endian, swappés à la lecture. IPs publiques détectées en excluant `10.x`, `172.16-31.x`, `192.168.x`, `127.x`. Résolution DNS via `Dns.GetHostEntry` pour les IPs publiques uniquement.

---

## CR-18 — Strings mémoire (`MemoryStringsWindow`)

**Ce que l'utilisateur voit**

Une liste de chaînes de caractères extraites de la mémoire live du processus, avec pour chacune : adresse mémoire, encodage (ASCII ou UTF-16), longueur, valeur (tronquée à 200 caractères), et catégorie automatique.

**Ce que l'utilisateur peut faire**

- **Filtrer par catégorie** : `URL` (regex `https?://`), `Path` (regex `[A-Za-z]:\\...`), `Email`, `Key` (chaîne hex ≥ 32 caractères), `Other`
- **Définir une longueur minimale** de chaîne (défaut : 4 caractères)
- Chercher des IOCs directement en mémoire — URLs de C2, chemins de fichiers, clés de chiffrement, adresses email

**Données collectées**

`VirtualQueryEx` pour lister les régions `Commit`. Régions > 50 Mo ignorées. `ReadProcessMemory` sur chaque région. Extraction des chaînes ASCII (byte par byte) et UTF-16 (2 bytes). Chaque chaîne imprimable (`0x20`–`0x7E`) d'au moins `minLength` caractères est catégorisée. Maximum 10 000 résultats. Opération async avec `CancellationToken` et `IProgress<int>`.

---

## CR-19 — Score de dangerosité (`DangerScoreWindow`)

**Ce que l'utilisateur voit**

Un score de 0 à 100 avec code couleur (vert ≤ 30 / orange 31–60 / rouge > 60), et le détail de chaque facteur avec sa contribution individuelle au score total.

**Ce que l'utilisateur peut faire**

Comprendre **pourquoi** un processus est considéré suspect — quel facteur précis a fait monter le score — et décider d'investiguer plus loin avec les autres CR.

**Calcul du score** (cache 60 secondes par PID)

| Facteur | Points max | Condition |
|---|---|---|
| VT positifs | 40 | `vtPositives × 5`, plafonné à 40. Nécessite clé VT configurée et analyse préalable |
| Modules non signés | 20 | `unsignedCount × 4` depuis `GetLoadedModules`, plafonné à 20 |
| Modules suspects | 8 | Au moins un module `IsSuspect` dans la liste des modules |
| Croissance WS | 15 | `wsGrowthDetected = true` passé par le ViewModel (basé sur `LeakDetectionService`) |
| Pas de fenêtre | 10 | `MainWindowHandle == IntPtr.Zero` ET `Description` vide |

---

## CR-20 — Détection de fuite mémoire (`LeakDetectionWindow`)

**Ce que l'utilisateur voit**

Le taux de croissance du Working Set en Mo/min, le statut de fuite (Normal / Suspect / Critique), et depuis combien de minutes la fuite a été détectée.

**Ce que l'utilisateur peut faire**

Confirmer ou infirmer une fuite mémoire sur un processus précis, voir depuis combien de temps elle dure, et décider d'une action (dump, kill, escalade).

**Fonctionnement interne**

`LeakDetectionService` maintient un buffer circulaire de **20 points** par PID. Minimum 5 points requis pour l'analyse. Pente calculée par **régression linéaire** (moindres carrés) sur les paires `(secondes depuis premier sample, Mo)`. Pente en Mo/sec convertie en Mo/min. Seuil de détection : `> 10 Mo/min`. Timestamp de première détection mémorisé dans `_detectedSince` — supprimé si la croissance redescend sous le seuil.

---

## CR-21 — Historique Working Set (`WsHistoryWindow`)

**Ce que l'utilisateur voit**

Une courbe temporelle du Working Set sur les ~10 dernières minutes, avec trois séries : Total, Private, Shareable.

**Ce que l'utilisateur peut faire**

Visualiser si un processus a un comportement mémoire stable, cyclique (pic et retour), ou en croissance continue. Corréler un pic de consommation avec un événement connu.

**Fonctionnement interne**

`WsHistoryService` reçoit un appel `Record(pid, total, private, shareable)` à chaque cycle de rafraîchissement du ViewModel (~5 sec). Buffer circulaire de **120 points maximum** par PID (`ConcurrentDictionary<int, WsHistory>`, lock par PID) → fenêtre de ~10 minutes. `Prune()` nettoie les PIDs absents de la liste active.

---

## CR-22 — Export Forensic JSON (`ForensicExportWindow`)

**Ce que l'utilisateur voit**

Un bouton de déclenchement. À la fin de l'export, le chemin du fichier JSON généré est affiché, et le rapport HTML s'ouvre automatiquement dans le navigateur.

**Ce que l'utilisateur peut faire**

Exporter toutes les données d'inspection du processus en JSON structuré pour traitement externe (scripts SOC, SIEM, outils DFIR). Consulter le rapport HTML interactif (dark theme, sidebar sticky, sections navigables, compatible impression).

**Ce qui est généré**

`ForensicExportService.ExportAsync()` collecte en async : métadonnées process, résumé mémoire, régions mémoire, fichiers mappés, modules chargés, threads, connexions réseau, token de sécurité, score de danger, détection de fuite. Deux fichiers dans `%LOCALAPPDATA%\RamSentinel\Reports\` : `forensic_{nom}_{pid}_{timestamp}.json` et `forensic_{nom}_{pid}_{timestamp}.html`. Le HTML s'ouvre via `Process.Start(UseShellExecute = true)`.

---

## CR-23 — Gaming Mode (`GamingModeWindow`)

**Ce que l'utilisateur voit**

Un bouton d'activation et, après exécution, un rapport : nombre de processus trimmés, Mo libérés estimés, succès ou échec de la purge Standby, liste des erreurs d'accès par processus.

**Ce que l'utilisateur peut faire**

Lancer ou arrêter le Gaming Mode depuis le menu contextuel sans quitter l'inspection d'un processus. Identique au bouton du panneau gauche.

**Ce qui s'exécute**

`GamingModeService.ActivateAsync()` : pour chaque processus non whitelisté et `CanTrim = true` → `SetProcessWorkingSetSizeEx(handle, -1, -1, 0)`. Puis `NativeCore.RS_EmptyStandbyList()`. Les Mo libérés sont estimés par différence `WorkingSet64` avant/après pour chaque processus trimmé.

---

## CR-25 — Anti-Hollowing (`AntiHollowingWindow`)

**Ce que l'utilisateur voit**

Un tableau des sections PE analysées (`.text`, `.data`, `.rdata`) avec pour chacune : nom de section, adresse virtuelle, taille sur disque, hash SHA-256 sur disque, hash SHA-256 en mémoire, et statut `OK` ou `⚠ DIVERGENT` en rouge.

**Ce que l'utilisateur peut faire**

Détecter si le code d'un processus a été remplacé en mémoire (process hollowing) ou modifié après chargement (hooking inline, patching). Une divergence de hash sur `.text` est le signal le plus fort.

**Fonctionnement interne**

Lecture du PE sur disque (`File.ReadAllBytes`). Parse de l'en-tête : offset PE depuis `0x3C`, `NumberOfSections` depuis `peOffset+6`, `SizeOfOptionalHeader` depuis `peOffset+20`. Pour chaque section `.text` / `.data` / `.rdata` : hash SHA-256 des octets bruts sur disque (raw offset + raw size), hash SHA-256 des octets lus via `ReadProcessMemory` à `imageBase + VirtualAddress`. L'adresse de base du module est trouvée dans `GetLoadedModules` par correspondance de nom de fichier. Toute divergence → `IsDivergent = true`.

---

## VirusTotal

La vérification VirusTotal est entièrement optionnelle et ne bloque aucune fonction si la clé est absente.

**Configurer la clé API** : dans la section `VIRUSTOTAL (API)` du panneau gauche, saisir la clé dans le champ dédié, puis `💾 Enregistrer`. Alternativement, `📂` charge une clé depuis un fichier existant.

**Utiliser VirusTotal** : sélectionner un processus → clic droit → `Vérifier sur VirusTotal`. L'application calcule le SHA-256 du binaire local puis interroge l'API si la clé est présente.

**Effacer la clé après usage** (recommandé avant de rendre une machine cliente) : le bouton `🗑️` supprime le fichier de clé, le pointeur `vt_api_path.txt` et la clé en mémoire — aucune trace laissée.

---

## Rapports HTML forensiques

Générés dans `%LOCALAPPDATA%\RamSentinel\Reports\`, ouverts automatiquement dans le navigateur. Produits par le service `ForensicExportService` composé de 4 modules (`HtmlReportStyles.cs`, `HtmlReportScripts.cs`, `IndividualReportBuilders.cs`, `ForensicExportService.cs`).

**Rapport individuel** (processus sélectionné) — 9 sections :

- Métadonnées : PID, chemin, ligne de commande, parent, heure de démarrage
- Résumé mémoire : Working Set, Private Bytes, Commit, Page Faults, nombre de régions
- Carte mémoire des régions virtuelles (protection, type, fichiers mappés)
- Fichiers mappés
- Arbre parent/enfant
- Modules chargés et statut Authenticode
- Threads du processus
- Connexions réseau associées au PID
- Sécurité, token, score de danger, détection de fuite

**Rapport système** — état global :

- RAM totale / utilisée / disponible / Standby / modifiée
- Score de pression mémoire
- Nombre total de processus / suspects / en fuite
- Total et moyenne des handles
- Top processus par RAM et par handles

Les deux rapports utilisent un dark theme GitHub-inspired : sidebar sticky, badges colorés, responsive, compatible impression.

---

## Fichiers créés sur la machine

| Emplacement | Contenu | Créé par |
|---|---|---|
| `%LOCALAPPDATA%\RamSentinel\Logs\` | Journaux d'exécution horodatés | `LoggingService` |
| `%LOCALAPPDATA%\RamSentinel\Reports\` | Rapports HTML et JSON | `ForensicExportService` |
| `%LOCALAPPDATA%\RamSentinel\CrashLogs\` | Crash logs en cas d'exception non gérée | `CrashLogService` |
| `%LOCALAPPDATA%\RamSentinel\vt_api_path.txt` | Pointeur vers le fichier de clé VirusTotal | `VirusTotalService` |

Aucun service Windows installé. Aucune entrée de registre créée.

---

## Nettoyage après intervention

```powershell
# 1. Effacer la clé VirusTotal via le bouton 🗑️ dans l'UI
# 2. Fermer RamSentinel
# 3. Supprimer le dossier de données
Remove-Item "$env:LOCALAPPDATA\RamSentinel" -Recurse -Force
```

---

## Architecture technique détaillée

### Moteur natif — RamSentinelCore.dll

La DLL C++ est compilée en Release|x64 via Visual Studio Build Tools. Elle encapsule les appels aux APIs Windows non accessibles en managed .NET (NtDll, Kernel32, DbgHelp, WinTrust) et les expose via P/Invoke à la couche C#. En build standard, le `.csproj` copie la DLL dans le dossier de sortie via une condition `<Content>` différenciée par `$(Configuration)`. En mode portable, la DLL est embarquée dans l'EXE via `IncludeNativeLibrariesForSelfExtract=true`.

### Couche C# — gestion des exceptions globales

`App.xaml.cs` enregistre trois gestionnaires au démarrage : `DispatcherUnhandledException` (thread UI — messagebox + crash log), `AppDomain.CurrentDomain.UnhandledException` (exceptions fatales hors thread UI), et `TaskScheduler.UnobservedTaskException` (Tasks fire-and-forget — marquées `SetObserved()` pour éviter le crash silencieux).

### Pattern architectural MVVM

Les vues XAML ne contiennent que du code-behind minimal de binding. Toute la logique est dans `MainViewModel`. Chaque commande CR est un `RelayCommand` qui instancie et affiche la fenêtre popup correspondante en lui passant le `ProcessInfo` sélectionné. Les services sont injectés ou accédés en singleton.

### Thème et styles WPF

`App.xaml` définit via `TargetType` sans clé les styles globaux de `DataGrid`, `DataGridCell`, `DataGridRow`, `DataGridColumnHeader` et `ScrollBar`. Les interactions hover/selected utilisent des `DynamicResource` cohérents avec le thème. La `ScrollBar` est entièrement retemplated : 6px de largeur, CornerRadius 3, fond transparent.

---

## Compatibilité

- Windows 10 / Windows 11 — architecture x64 obligatoire
- .NET 8.0 runtime inclus dans l'EXE portable (aucune installation requise)
- `System.Management` v8.0.0 (NuGet) — seule dépendance externe
- High DPI : `SystemAware` déclaré dans le `.csproj`
- Fonctionne sans droits administrateur pour la lecture et l'affichage ; certaines actions d'optimisation nécessitent une élévation

---

## FAQ / Dépannage

**L'application est signalée par l'antivirus.**  
Faux positif — voir section [Avertissement antivirus](#️-avertissement-antivirus).

**`Accès refusé` sur certaines actions.**  
Relancer l'application en tant qu'administrateur (clic droit → Exécuter en tant qu'administrateur).

**`Chemin d'accès invalide` dans le menu contextuel.**  
Le binaire du processus n'est pas accessible depuis l'espace utilisateur ou Windows ne renvoie pas de chemin exploitable (processus système protégés).

**VirusTotal indisponible.**  
Vérifier la présence de la clé API dans le panneau VT, l'accès Internet, et les limites de quota du compte utilisé.

**La DLL native n'est pas trouvée au démarrage.**  
En build standard, vérifier que `Core\x64\Release\RamSentinelCore.dll` existe (le build C++ doit avoir été effectué via `buildNET.ps1`). En mode portable, la DLL est intégrée dans l'EXE — ce problème ne peut pas survenir.

**Le build C++ échoue avec `vcvarsall.bat non trouvé`.**  
Installer Visual Studio 2022 Build Tools avec le workload **Desktop development with C++** depuis [visualstudio.microsoft.com](https://visualstudio.microsoft.com/fr/downloads/).

**Score de danger toujours à 0.**  
Le calcul nécessite un rafraîchissement complet. Sans clé VT, le facteur VirusTotal est absent mais les 4 autres facteurs fonctionnent sans connexion.

**Historique Working Set vide dans CR-21.**  
`WsHistoryService` enregistre un snapshot à chaque cycle de rafraîchissement (~5 sec). Attendre au moins 30 secondes après le démarrage.

---

## Gestion des erreurs

- Chaque service est encapsulé dans des blocs `try/catch` typés (`Win32Exception`, `UnauthorizedAccessException`, `FileNotFoundException`) avec logging contextué.
- Les erreurs d'accès refusé à des processus protégés sont silencieusement ignorées au niveau module — l'exécution continue pour les autres processus.
- Les exceptions non gérées sont capturées globalement par `App.xaml.cs`, loggées et affichées via messagebox sans crash du processus.
- Les Tasks fire-and-forget sont surveillées via `TaskScheduler.UnobservedTaskException` pour éviter les crashes silencieux.
- Les crash logs sont écrits dans `%LOCALAPPDATA%\RamSentinel\CrashLogs\` avec contexte d'origine.

---

## Contributions

Les contributions sont les bienvenues. Pour signaler un bug, proposer une amélioration ou soumettre une nouvelle fenêtre d'inspection :

1. **Fork** le dépôt
2. Créer une branche : `git checkout -b feature/nom-contribution`
3. Committer les changements : `git commit -m 'Ajout CR-26 : description'`
4. Push : `git push origin feature/nom-contribution`
5. Ouvrir une **Pull Request**

### Idées de contributions

- Nouvelles fenêtres d'inspection (CR-26+) : VAD tree, strings kernel, sections PE détaillées
- Export STIX / MISP / CSV des données forensiques
- Tests unitaires (xUnit + Moq pour les services C#)
- Refactoring injection de dépendances (DI container)
- Support thème clair (LightTheme réel)
- Pipeline CI/CD GitHub Actions (build + test automatisé)
- Traduction de l'interface en anglais

---

## ⚠️ Avertissement antivirus

Microsoft Defender et d'autres antivirus peuvent signaler ce programme comme suspect. Il s'agit d'un **faux positif** sans lien avec un quelconque logiciel malveillant.

Ce type de détection est courant pour les applications qui accèdent aux APIs mémoire bas niveau de Windows (`NtQuerySystemInformation`, `VirtualQueryEx`, `MiniDumpWriteDump`), s'exécutent avec des droits étendus, ou sont téléchargées depuis Internet (Mark-of-the-Web).

> Defender ne signale pas ce programme au moment de la publication, mais ses systèmes de détection par apprentissage automatique peuvent le signaler dans les jours suivant une nouvelle release.

❗ **Si Defender bloque l'exécution :**

```powershell
# Ajouter une exclusion sur le dossier du portable
Add-MpPreference -ExclusionPath "$env:USERPROFILE\Downloads\RAMSentinel_Portable"

# Ou débloquer uniquement l'exécutable (supprime le Mark-of-the-Web)
Unblock-File .\RamSentinel.exe
```

Pour soumettre un signalement de faux positif :
- **Microsoft Defender** : https://www.microsoft.com/en-us/wdsi/filesubmission
- **VirusTotal** : https://www.virustotal.com/gui/home/upload

---

## Auteur / Contact

**Auteur :** ps81frt  
**GitHub :** <https://github.com/ps81frt>  
**Dépôt source :** <https://github.com/ps81frt/ramsentinel_source>  
**Releases (portable) :** <https://github.com/ps81frt/RAMSentinel/releases>  
**Issues :** <https://github.com/ps81frt/RAMSentinel/issues>

Pour les rapports de bugs, ouvrir une issue sur le dépôt GitHub.  
Pour les divulgations de sécurité, contacter l'auteur directement via GitHub.

---

<div align="center">

Développé par **[ps81frt](https://github.com/ps81frt)**

[![GitHub](https://img.shields.io/badge/GitHub-ps81frt%2Framsentinel__source-181717?logo=github)](https://github.com/ps81frt/ramsentinel_source)

</div>
