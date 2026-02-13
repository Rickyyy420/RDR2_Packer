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

- `mainmods.json` – core mods required for the pack  
- `pack.json` – user‑defined mods, optional mods, and custom installs  

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

---

## <span style="color:#005cc5">How the Installer Works</span>

### 1. Load JSON
The installer loads:
