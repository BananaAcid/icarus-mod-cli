```
 _                                                    _            _ _
(_) ___ __ _ _ __ _   _ ___       _ __ ___   ___   __| |       ___| (_)
| |/ __/ _` | '__| | | / __|_____| '_ ` _ \ / _ \ / _` |_____ / __| | |
| | (_| (_| | |  | |_| \__ \_____| | | | | | (_) | (_| |_____| (__| | |
|_|\___\__,_|_|   \__,_|___/     |_| |_| |_|\___/ \__,_|      \___|_|_|
```

# icarus-mod-cli

A commandline tool to manage and repack (for weekly updates) mods.

It creates merged mods compatible to [Jimk72/Icarus-Mod-Manager](https://github.com/Jimk72/Icarus_Software), but with a few bells and whistles
- it is terminal only (commandline)
- can be used with dedicated servers 
- most methods can optionally return json - for automation / update scripts (for automated updating of game and servers)
- it supports .EXMOD/.EXMODZ and Laanp Script (nativly)
- merges all folders, IMM looses some folders on merging sometimes (like UI folders)
- supports downloading and searching for mods
- supports an open mod author repo listing indepandant of any mod manager author [here](https://gist.github.com/BananaAcid/5dc703c5cf73a41291bdcee400a8ca38)
- hopefully intuitive to use
- supports Win10+, MacOS, Linux
- uasset merging + patching

> [!IMPORTANT]
> During dev, code is not yet released and not all OS targets build (version < 1.0.0)
>
> Building mods and merging is stable -> making the first windows binary available.
>
> Not yet available:
> - uasset merging
> - native support for merging CrystalFerrai/IcarusModManager mods (.zip)
> - code cleanup
> - ...

## Installation

1. Get the binary from the releases
2. Open your terminal and navigate to the unzipped imc binary
3. Type `imc setup all` ... it will unpack needed files, create a config, download dependencies. See the help below 

## Usage example

```bash
$ imc mods "teleport"
Searching plugins for: teleport
Jimk Multi Teleport | by Jimk72 | v1.1 | week All | file: https://github.com/Jimk72/Icarus_Mods/raw/main/Jimk_MultiTeleport.EXMODZ | modinfo: https://raw.githubusercontent.com/Jimk72/Icarus_Mods/refs/heads/main/modinfo.json | repo: https://github.com/Jimk72/Icarus_Mods
Teleporter | by Jimk72 | v3.0 | week All | file: https://github.com/Jimk72/Icarus_Mods/raw/main/Jimk_Teleporter.EXMODZ | modinfo: https://raw.githubusercontent.com/Jimk72/Icarus_Mods/refs/heads/main/modinfo.json | repo: https://github.com/Jimk72/Icarus_Mods
laanp-MapTeleport | by laanp | v1.0 | week w259 | file: https://github.com/laanp/Icarus_Mods_Separated/releases/download/v1_w259/laanp-MapTeleport_v1_w259_P.pak | modinfo: https://raw.githubusercontent.com/laanp/Icarus_Mods_Separated/refs/heads/main/modinfo.json | repo: https://github.com/laanp/Icarus_Mods_Separated
laanp-WaypointTeleport | by laanp | v1.0 | week w259 | file: https://github.com/laanp/Icarus_Mods_Separated/releases/download/v1_w259/laanp-WaypointTeleport_v1_w259_P.pak | modinfo: https://raw.githubusercontent.com/laanp/Icarus_Mods_Separated/refs/heads/main/modinfo.json | repo: https://github.com/laanp/Icarus_Mods_Separated
```

```bash
$ imc mods "teleport" --json
{"search": "teleport", "mods": [
{"name":"Jimk Multi Teleport","author":"Jimk72","version":"1.1","week":"All","file":"https://github.com/Jimk72/Icarus_Mods/raw/main/Jimk_MultiTeleport.EXMODZ","modinfo":"https://raw.githubusercontent.com/Jimk72/Icarus_Mods/refs/heads/main/modinfo.json","repo":"https://github.com/Jimk72/Icarus_Mods"},
{"name":"Teleporter","author":"Jimk72","version":"3.0","week":"All","file":"https://github.com/Jimk72/Icarus_Mods/raw/main/Jimk_Teleporter.EXMODZ","modinfo":"https://raw.githubusercontent.com/Jimk72/Icarus_Mods/refs/heads/main/modinfo.json","repo":"https://github.com/Jimk72/Icarus_Mods"},
{"name":"laanp-MapTeleport","author":"laanp","version":"1.0","week":"w259","file":"https://github.com/laanp/Icarus_Mods_Separated/releases/download/v1_w259/laanp-MapTeleport_v1_w259_P.pak","modinfo":"https://raw.githubusercontent.com/laanp/Icarus_Mods_Separated/refs/heads/main/modinfo.json","repo":"https://github.com/laanp/Icarus_Mods_Separated"},
{"name":"laanp-WaypointTeleport","author":"laanp","version":"1.0","week":"w259","file":"https://github.com/laanp/Icarus_Mods_Separated/releases/download/v1_w259/laanp-WaypointTeleport_v1_w259_P.pak","modinfo":"https://raw.githubusercontent.com/laanp/Icarus_Mods_Separated/refs/heads/main/modinfo.json","repo":"https://github.com/laanp/Icarus_Mods_Separated"}
], "total": 4}
```


## current (messy) help
`imc --help`
```
 _                                                    _            _ _
(_) ___ __ _ _ __ _   _ ___       _ __ ___   ___   __| |       ___| (_)
| |/ __/ _` | '__| | | / __|_____| '_ ` _ \ / _ \ / _` |_____ / __| | |
| | (_| (_| | |  | |_| \__ \_____| | | | | | (_) | (_| |_____| (__| | |
|_|\___\__,_|_|   \__,_|___/     |_| |_| |_|\___/ \__,_|      \___|_|_| v0.1.60

Complete CLI replicating IcarusModManager PAK repack: reads .EXMOD and Laanp Script, applies JSON changes to data/, builds a mod's /Icarus/Content/Data/, creates a .pak from a single mod or a mod pack with multiple mods

Author: Nabil Redmann (BananaAcid) - https://github.com/BananaAcid/icarus-mod-cli


Usage:
       imc <mod>[ <mod> ...] [--out <dir|pak>] [--week [<n>]] [--deploy] [--data <dir>]        repack mod(s) into a <mod>_P.pak (single) or IMC_Merged_Mod_P.pak (merged; --out <file>.pak)
                             [--force] [--force-uassets]                                       ... optionally force to continue on merge problems (only for merged)
                             [--edit [<id>] [--editid <setid>]]                                ... optionally edit mod(s) before repack
       imc setup 7zip                                                                          download 7-Zip (ZIP extraction fallback; tar/unzip/bsdtar are used when present)
       imc setup repak                                                                         download repak (default pak tool)
       imc setup unrealpak                                                                     download UnrealPak (fallback; primary when config -> force_unrealpak=true)
       imc setup data [--force]                                                                extract current game's data for exmod/laanp/...
       imc setup specs                                                                         write the EXMOD file specification to the temp dir (referenced by the --edit prompt)
       imc setup repos                                                                         update repo sources
       imc setup mods                                                                          reset the modinfo cache and refetch all modinfo files from all repos
       imc setup migrate                                                                       copy icarus mod manager's Extracted_Mods/TempDownload into local dirs and add missing .modinfo.json
       imc setup all [--force]                                                                 run all setup steps (order: 7zip -> repak/unrealpak -> data [--force] -> specs -> repos -> mods -> migrate)
       imc week                                                                                print only the computed current content update week number
       imc repos                                                                               lists repo URLs
       imc data [--json] [--fresh]                                                             list cached game data builds and their weeks
       imc config [--json]                                                                     print the current config: keys, values and descriptions
       imc mods [--description] [--readme] [--json] [--fresh]                                  lists all mods
       imc mods "search_str" [--description] [--readme] [--json] [--fresh]                     fuzzy search for mod
       imc mods download <name> [<name> ...] [--out <dir>]                                     downloads mod(s) by name
       imc mods extract <name> [<name> ...] [--out <dir>] [--unpacker repak|unrealpak]         extract downloaded mod(s) into the extracted dir
       imc mods meta ["*"|"all"|<name> ...] [--force] [--fresh] [--json]                       write missing .modinfo.json/.readme.md sidecars from cached repo modinfos
       imc mods update [--json] [--fresh] [--simple]                                           compare installed mods vs online repos
       imc mods update all                                                                     stub: update every installed mod
       imc mods update <name> [<name> ...]                                                     show old version, re-download, show new version
       imc unpack <file.pak|file.exmodz|file.zip> [--out <dir>] [--unpacker repak|unrealpak]   raw extract mod with repak (default: E:\TEMP_icarus_mods\extracted_optimized)

Options:
  -v, --version           print version
  -h, --help, -?          print this help
  -d, --description       show mod descriptions
  -r, --readme            show mod readme
  -j, --json              output JSON instead of plain text
  -c, --config <file>     load a JSON config overlay - can be used with all commands (deep-merged over the effective config: package.json defaults + config.json store; first occurrence wins; never persisted)
      --deploy            copy resulting .pak into the game mods dir (config icarus_content_mod_path)
      --week [<num>]      deploy into a w<num> subfolder (Friday release cycle; bare --week = the release-aware current content update week - no fetch, a cached steamcmd record that already has the new update counts)
      --data <folder>     use an alternative (unpacked DATA.pak) game data folder (e.g. different version)
  -o, --out <dir>         output target: repack = a directory, or a .pak file (used verbatim); unpack / mods download / mods extract = the output root directory
      --unpacker <tool>   force the pak backend for .pak extraction in unpack / mods extract ("repak"|"unrealpak"; default: repak)
      --force             with setup data / all: force re-extract (overwrite) of game data; pack/merge: bypass merge safety errors (incl. payload conflicts); mods meta: overwrite existing .modinfo.json/.readme.md sidecars; other mods flows: ignored
      --force-uassets     pack/merge only: same as --force, but ONLY for cooked asset binaries (.uasset/.uexp/.ubulk/.umap/.upk) - no uasset merging exists yet, so the later mod's asset wins
      --fresh             with imc mods (list / search / update): ignore caches, refetch (list + per-repo modinfo/readme/merged repo list); with imc data: also refetch the steamcmd game-update check
      --simple            with bare imc mods update ONLY: print just the mods with updates available (warnings show too); "No updates available" when none; rejected with named mods / list / search
      --edit [<id>]       repack: pause before EXMOD patching; writes E:\TEMP_icarus_mods\app_temp\edits\prompt.<id> and waits for
                          PRESS ANY KEY (e = edit in with AI: pick from opencode|opencode mini|baio|claude|codex|agy|aider|goose; ESC/Ctrl+C cancels),
                          files E:\TEMP_icarus_mods\app_temp\edits\continue.<id> / E:\TEMP_icarus_mods\app_temp\edits\cancel.<id>.
                          <id> (12 hex) reuses an existing prompt.<id> and skips straight to the wait
      --editid <setid>    repack: ENFORCE the edit session id (never regenerated; the value must be 12 hex or the run errors).
                          A watcher tool can pre-create E:\TEMP_icarus_mods\app_temp\edits\continue.<setid> /
                          E:\TEMP_icarus_mods\app_temp\edits\cancel.<setid> before the run; EXISTING signal files are not cleared - stale ones resolve the pause.

Repack mod(s) - information:
  EXMODs applied and merged in input order.
  Default out: E:\TEMP_icarus_mods\repacked (config pak_output_dir, relative = under app_temp_dir)
  Merge of several mods: fixed pack name "IMC_Merged_Mod_P.pak" (an --out <file>.pak is used verbatim and its base name is reflected in the merged EXMOD + .md list).
  --out/-o also works for mods download / mods extract / unpack (output root dir). NOTE: files saved to a CUSTOM download dir are not
  auto-discovered by name (name resolution only scans the config downloads dir) - extract/pack them via their archive PATH instead.
  <mod> variations: bare name ("Teleporter" = extracted dir) | path to file.pak/.exmodz/.zip | bare archive name found in downloads
  ... bare name as last try also matches a downloaded *.modinfo.json (exact base or mods[0].name) -> "<base>.exmodz"/"<base>.pak"/"<base>_P.pak"/"<base>.zip"
  (e.g. "Donovan's More Points" -> MorePoints.EXMODZ)

Config file:
  Trying to load config store first: E:\TEMP_icarus_mods\app\config.json
  Second: C:\Users\nabil\.config\icarus-mod-cli\config.json
  See `imc config` for details.

Adding your own mods repo (public repo list):
  Comment at https://gist.github.com/BananaAcid/5dc703c5cf73a41291bdcee400a8ca38
  containing a line like REPO->https://github.com/<user>/<mod-repo> (the next space ends
  the URL). The repo must provide a modinfo.json or toolinfo.json. Repo URLs are picked
  up from the gist comments - setup/repos and "imc mods" list them like any other repo.

Content Update:
  Current content update week: w259 (w1 = Fri 2021-12-10)
```
