# RDR2_Packer
to start this is a WIP and will come with qol and other updates/patches at my descretion, im a solo dev so updates take time to push but i hope for this to one day be a wabbajack alternitive for rdr2 and gta games, for now we start with rdr2 below is the requirements for the packer and the steps to use/setup the scripts


## requiremetns
- rdr2 obviously clean and un modded
- the most recent python ( downloaded via microsoft store for me )
- python requests ( CTRL + R and type "pip install requests" )
- time and the ability to read carefully 



# RDR2 Modpack Installer

A fully automated, JSON‑driven mod installation system for Red Dead Redemption 2.  
Supports LML mods, ASI root mods, hybrid mods, custom installs, flattening rules, secondary folders, and Nexus API downloads.

---

## <span style="color:#d73a49">Overview</span>

This installer reads two configuration files:

- mainmods.json – core mods required for the pack  
- pack.json – user‑defined mods, optional mods, and custom installs  

Each mod entry defines:

- Where to download it  
- How to extract it  
- How to install it  
- Whether to flatten folders  
- Whether it contains an ASI file  
- Whether it includes secondary folders  

The installer handles:

- Nexus API downloads  
- ZIP extraction  
- LML folder detection  
- ASI extraction  
- Root installs  
- Custom installs  
- Blacklists and regex blacklists  
- Flattening modes  
- Hybrid LML+ASI mods  
- Secondary folder installs  



## <span style="color:#005cc5">How the Installer Works</span>

### 1. Load JSON  
The installer loads mainmods.json and pack.json.  
Each entry becomes a Mod object with fields such as type, folder, flatten, asi_file, secondary, blacklist, and regex_blacklist.

### 2. Download or Locate ZIP  
The installer:

- Checks for an existing ZIP in RDR2/downloads  
- Downloads via Nexus API if allowed  
- Falls back to direct URL  
- Falls back to manual drag‑and‑drop  

### 3. Extract ZIP  
ZIPs are extracted into:

_temp/<zipname>_

### 4. Install Based on Type

#### type: "root"  
Installs files into the RDR2 root directory.

#### type: "lml"  
Installs LML folder into RDR2/lml/.

#### type: "custom"  
Uses only the secondary rules.

#### Hybrid LML + ASI  
If asi_file is provided, the installer:

- Searches recursively for the ASI  
- Moves only that file to the RDR2 root  
- Excludes its parent folder from installation  
- Installs LML normally  
- Applies flatten rules to remaining root files  

This prevents wrapper folders like “Open This Folder” from being copied.


## <span style="color:#6f42c1">Flatten Modes</span>

Flattening controls how folder structures are handled.

### flatten: "none"  
Preserves folder structure exactly.

### flatten: "top"  
Moves the contents of the top‑level folder into the destination.  
Example:

MyMod/  
    file1.asi  
    folderA/  

Becomes:

RDR2/  
    file1.asi  
    folderA/  

### flatten: "all"  
Flattens every file from every subfolder into the destination.

### flatten: ["folder1", "folder2"]  
Flattens only the listed folders.

---

## <span style="color:#22863a">Hybrid LML + ASI Mods</span>

Some mods contain:

- An LML folder  
- An ASI file  
- A wrapper folder such as “Open This Folder”  

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
2. Installs it into RDR2/lml/  
3. Searches for DynamicWorld.asi  
4. Moves only that file to RDR2 root  
5. Excludes its parent folder  
6. Applies flatten rules to remaining root files  

---

## <span style="color:#b08800">Root Mods</span>

Root mods install directly into the RDR2 directory.

Example:

{
    "name": "Rampage Trainer",
    "nexus_url": "https://www.nexusmods.com/reddeadredemption2/mods/233",
    "type": "root",
    "flatten": "top"
}

Rampage ZIP structure:

Rampage_1.0.1491.50/  
    Rampage.asi  
    RampageFiles/  
    README.txt  

With flatten: "top", the installer produces:

RDR2/  
    Rampage.asi  
    RampageFiles/  
    README.txt  

---

## <span style="color:#d73a49">Secondary Installs</span>

Secondary installs allow flattening or moving specific folders after the main install.

Example:

"secondary": [
    {
        "folder": "OptionalAddons",
        "target": "root"
    }
]

Secondary installs are used for:

- Optional folders  
- Add‑ons  
- Custom mod structures  

---

## <span style="color:#005cc5">Full JSON Examples</span>

### LML‑only mod

{
    "name": "Photorealistic Lighting",
    "type": "lml",
    "nexus_url": "https://example.com",
    "flatten": "none"
}

### Root ASI mod

{
    "name": "Rampage Trainer",
    "type": "root",
    "nexus_url": "https://example.com",
    "flatten": "top"
}

### Hybrid LML + ASI mod

{
    "name": "A Dynamic World",
    "type": "lml",
    "nexus_url": "https://example.com",
    "flatten": "top",
    "asi_file": "DynamicWorld.asi"
}

### Custom mod with secondary folders

{
    "name": "Custom Mod",
    "type": "custom",
    "secondary": [
        {
            "folder": "Data",
            "target": "root"
        }
    ]
}

---

## <span style="color:#6f42c1">How to Add a New Mod</span>

1. Open pack.json  
2. Add a new entry:

{
    "name": "My Mod",
    "type": "root",
    "nexus_url": "https://example.com",
    "flatten": "top"
}

3. Save the file  
4. Run the installer  
5. The mod installs automatically  

---

## <span style="color:#22863a">How to Run the Installer</span>

1. Place installer.py, modclass.py, installers.py, and JSON files in the same directory  
2. Run:

python installer.py

3. Select your RDR2 directory  
4. Enter your Nexus API key  
5. The installer handles everything else  

---

## <span style="color:#b08800">Troubleshooting</span>

### A folder appears in the RDR2 root  
Use flatten: "top".

### ASI file not installed  
Add asi_file: "MyMod.asi".

### LML folder not detected  
Specify folder: "SomeParentFolder".

### Mod requires manual drag‑and‑drop  
Set manual: true.

---

## <span style="color:#d73a49">Conclusion</span>

This installer is designed to handle every mod structure RDR2 modders encounter:

- Wrapper folders  
- Hybrid mods  
- ASI files  
- LML folders  
- Optional add‑ons  
- Custom installs  
- Flattening rules  

It is fully extensible, predictable, and easy to maintain.

