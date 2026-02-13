# RDR2 Packer / Modpack Installer

A JSON‑driven, fully automated mod installation system for Red Dead Redemption 2.  
Supports LML mods, ASI mods, hybrid installs, flattening rules, Nexus API downloads, and custom mod structures.

This project is a work in progress. Updates and improvements will be released over time.

---

## Requirements

- A clean, unmodded installation of Red Dead Redemption 2  
- Latest version of Python 3  
- Python `requests` package  
  Install using: `pip install requests`  
- Ability to follow instructions carefully  

---

## How Users Install the Provided Modpack

1. Download and extract the modpack folder.  
2. Open the folder in File Explorer.  
3. Run the installer using `launcher.bat`.  
4. When prompted:  
   - Select your RDR2 installation directory  
   - Enter your Nexus API key
  
5. youll be prompted to manually download 3 files and a exe into the window, make sure to do so

7. The installer will automatically:  
   - Download required mods  
   - Extract archives  
   - Install LML mods  
   - Install ASI mods  
   - Handle hybrid mods  
   - Apply flattening rules  
   - Install optional or secondary folders  
8. Launch RDR2 when installation is complete.

---

## Overview

The installer reads two JSON files:

- `mainmods.json` – core mods required for the pack  
- `pack.json` – user‑defined mods, optional mods, and custom installs  

Each mod entry defines:

- Where to download it  
- How to extract it  
- How to install it  
- Whether to flatten folders  
- Whether it includes an ASI file  
- Whether it contains secondary folders  

The installer automatically handles:

- Nexus API downloads  
- ZIP, RAR, 7z, and TAR extraction  
- LML folder detection  
- ASI extraction and placement  
- Root installs  
- Hybrid LML + ASI mods  
- Custom installs  
- Blacklists and regex blacklists  
- Flattening rules  
- Secondary folder installs  

---

## How the Installer Works

### 1. Load JSON  
The installer loads `mainmods.json` and `pack.json`.  
Each entry becomes a Mod object with fields such as `type`, `folder`, `flatten`, `asi_file`, `secondary`, `blacklist`, and `regex_blacklist`.

### 2. Download or Locate ZIP  
The installer:  
- Checks for an existing ZIP in `RDR2/downloads`  
- Downloads via Nexus API if allowed  
- Falls back to direct URL  
- Falls back to manual drag‑and‑drop  

### 3. Extract ZIP  
ZIPs are extracted into `_temp/<zipname>`.

### 4. Install Based on Type

#### type: `root`  
Installs files into the RDR2 root directory.

#### type: `lml`  
Installs LML folder into `RDR2/lml/`.

#### type: `custom`  
Uses only the secondary rules.

#### Hybrid LML + ASI  
If `asi_file` is provided, the installer:  
- Searches recursively for the ASI  
- Moves only that file to the RDR2 root  
- Excludes its parent folder  
- Installs LML normally  
- Applies flatten rules to remaining files  

---

## Flatten Modes

### flatten: `none`  
Preserves folder structure exactly.

### flatten: `top`  
Moves the contents of the top‑level folder into the destination.

### flatten: `all`  
Flattens every file from every subfolder into the destination.

### flatten: `[ "folder1", "folder2" ]`  
Flattens only the listed folders.

---

## Hybrid LML + ASI Mods

Some mods contain both an LML folder and an ASI file inside a wrapper folder.

Example JSON:

{  
    "name": "A Dynamic World",  
    "nexus_url": "https://www.nexusmods.com/reddeadredemption2/mods/7190",  
    "type": "lml",  
    "flatten": "top",  
    "asi_file": "DynamicWorld.asi"  
}

Installer behavior:

1. Detects LML folder  
2. Installs it into `RDR2/lml/`  
3. Searches for the ASI file  
4. Moves only that file to RDR2 root  
5. Excludes its parent folder  
6. Applies flatten rules to remaining files  

---

## Root Mods

Root mods install directly into the RDR2 directory.

Example:

{  
    "name": "Rampage Trainer",  
    "nexus_url": "https://www.nexusmods.com/reddeadredemption2/mods/233",  
    "type": "root",  
    "flatten": "top"  
}

---

## Secondary Installs

Secondary installs allow flattening or moving specific folders after the main install.

Example:

"secondary": [  
    {  
        "folder": "OptionalAddons",  
        "target": "root"  
    }  
]

Used for:

- Optional folders  
- Add‑ons  
- Custom mod structures  

---

## How to Add a New Mod

1. Open `pack.json`.  
2. Add a new entry:

{  
    "name": "My Mod",  
    "type": "root",  
    "nexus_url": "https://example.com",  
    "flatten": "top"  
}

3. Save the file.  
4. Run the installer.  
5. The mod installs automatically.

---

## How to Run the Installer (Developer)

1. Place `installer.py`, `modclass.py`, `installers.py`, and JSON files in the same directory.  
2. Run `python installer.py`.  
3. Select your RDR2 directory.  
4. Enter your Nexus API key.  
5. The installer handles everything else.

---

## Troubleshooting

**A folder appears in the RDR2 root**  
Use `flatten: "top"`.

**ASI file not installed**  
Add `asi_file: "MyMod.asi"`.

**LML folder not detected**  
Specify `folder: "SomeParentFolder"`.

**Mod requires manual drag‑and‑drop**  
Set `manual: true`.

---

## Conclusion

This installer is designed to handle every mod structure RDR2 modders encounter:

- Wrapper folders  
- Hybrid mods  
- ASI files  
- LML folders  
- Optional add‑ons  
- Custom installs  
- Flattening rules  

It is fully extensible, predictable, and easy to maintain.

---

If this STILL doesn’t paste correctly, tell me **exactly what GitHub is doing wrong** and I’ll fix it line‑by‑line.
