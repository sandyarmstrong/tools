# Windows Application Developer Tools
By: Christian Gunderman

## Introduction:

This project aims to provide a small, coherent, well documented set of scripts and lightweight tools for improving productivity for developers. It aims to solve a number of common problems, including backup, build, error navigation, file search, patching of installs, F5 debugging of arbitrary solutions, and killing of processes.

It was originally created to improve productivity of developers building Visual Studio. ❤

### Install: [Self extracting installer](https://github.com/gundermanc/tools/releases).
Auto-updates on subsequent usages.

### Use:

#### Launch Within Any Terminal
- In Powershell: `tools.ps1`
- In Developer Command Prompt: `tools.cmd`.

#### 'Building'
- Clone the repository recursively
- Update submodules
- Run Build.cmd
- Build produces 'StandaloneInstaller.bat' with packaged scripts.

#### Debugging
- Clone repo
- Run `src/Tools.bat` to test inline without installing. I recommend https://code.visualstudio.com/ with the PowerShell extension for editing.

#### Configure Patching
Patching can be used to update an installed VS or application with experimental bits. For robustness, patch
makes backups of each file, takes a hash of the original files, and kills any lock processes,
guaranteeing that patch always succeeds, and revert never breaks VS after an update.

- Open your 'scratch' (tool configuration) directory by typing 'nvgo scratch'
- Specify the VS instance to patch for this session by:
  - `vsget`: lists VS instances on your box
  - `vspatch [instance number]`: sets the VS instance you wish to use as your target instance (if patching VS extensions).
- You can create a new patch profile with `ptedit [profile name]`. This gets stored in your scratch directory.

##### Creating patch profiles
You can create a new patch profile with `ptedit [profile name]`. This gets stored in your scratch directory and looks like the sample below.
Patch profiles support embedded Powershell variables, including `$env:` for environment variables. Commands is an array of Powershell commands
that are run on patch.

Must first set `$env:PatchTargetDir` to directory you want to patch. Use `vspatch [install number]` to patch a VS install.

```json
{
    "sourceDirectory": "directory to copy from",
    "files": {

        "relative path to source file": "relative path to destination file",
    },
    "commands": [
        "Start-Process 'myproc.exe' -Wait -ArgumentList 'args'"
    ]
}
```

#### Start VS
- Find VS instances with `vsget`
- Launch desired instance with `vsstart [instance number]`.
- Drop into Developer Command prompt with `vscmd [instance number]`
- Navigate to VS install directory with `vspath [instance number]`

#### Patch and Restore
- Follow steps above for 'Configuring' your machine and profile.
- Patch your install using `ptapply [profile name]`. The originally installed files are backed up and can be restored at any time.
- Check if your currently selected VS instance has been patched with `ptstatus`.
- Unpatch a specific profile using `ptrevert [profile name]`.

#### F5 debug your project
- Follow steps above for 'Configuring' your machine and profile.
- Ensure that 'Patch and Restore' from above work.
- Run `ptF5 [instance number of VS instance to use to debug] [path to solution file] [profile name]

This injects a post-build-event into your project that enables F5 debugging by running your patch profile on 'Start Debugging'
and auto-attaches the debugger.

NOTE: for robustness, patch makes backups of each file, takes a hash of the original files, and kills any lock processes,
guaranteeing that patch always succeeds, and revert never breaks VS after an update.

### More aliases

#### Find
Aliases for finding files and text:
- findp [path]: Searches subdirectory for paths containing [path].
- findif [text]: Searches subdirectory for files containing [text].

#### MSBuild
Defines some aliases for MSBuild with a clickable GUI error list
that makes navigating MSBuild spew more pleasant.
- msbbuild: Build with typical settings
- msbclean: Clean project
- msbrestore: Perform Nuget restore.

#### Patching applications
Defines some simple commands for backing up, patching, and restoring
installed applications based on a configuration file. This script features
automatic killing of locking processes as well as hash-verification, ensuring
that patch and revert are reliable. It has also experimental support for F5
debugging any VS solution via patch profiles.

Use with Visual Studio
'vspatch' command to patch a VS install.
- ptedit [profile]: Opens the selected profile for editing.
- ptget [profile]: Gets a list of profiles.
- ptapply [profile]: Applies the selected profile.
- ptstatus [profile]: Checks the current target application for patched files.
- ptrevert [profile]: Reverts the current profile's patched binaries.
- ptF5 [vsinstance] [solutionpath] [profile]: Launches the specified instance of VS with the specified solution + profile configured for one-click (F5) debugging.

#### Process Snapshot
Defines some convenient scripts and aliases for killing groups of processes
to help get you unblocked if a build executable is locking a file.
- pbunlock [fileName]: Kills all processes that are locking a file.
- pbldmp [profile] [fileName]: Dumps a list of all processes locking the given file into a profile.
- pbget: Gets a list of Process Baseline profiles that you have saved.
- pbnew [profile]: Dumps all active processes to a named profile or 'Default' profile if not specified.
- pbnstop [profile]: Stops all executables except those reference by the given profile or 'Default' profile if not specified.
- pbstop [profile]: Stops all executables referenced by the given profile or 'Default' profile if not specified.
- pbedit [profile]: Opens a named profile for manual editing.

#### Navigation aliases
Defines aliases for navigating and opening explorer in named paths.
- nvedit: Opens the list of aliases for editing.
- nvget: Lists all defined aliases.
- nvgo: `cd`s to the given alias or prompts you to define it if undefined.
- nvnew: Defines or redefines an alias.
- nve: Launches a named location in file explorer.

#### Visual Studio
Aliases for launching Visual Studio installs developer command prompts.
- vsget: Lists all VS installs and their [instance] number.
- vsstart [instance]: Starts a specific VS instance by its number.
- vsreset [instance]: Wipes a specific VS instance by its number.
- vsconfig [instance]: Updates configuration timestamp on a specific VS.
- vspath [instance]: Opens installation path on a specific VS.
- vscmd [instance]: Launches developer command prompt for a specific VS.
- vspatch [instance]: Selects an instance of VS as the target application for patching.

## ChangeLog
- 7/17/2019 - Support environment variables in patch paths and define $env:GitRoot for the root of the current repo.
- 6/7/2019 - Fixed killing of locking processes when doing revert. Fixed overwriting of backup when revert fails. Print release notes on startup. Init submodule on build.
- 5/10/2019 - Add 'scratch' to default nav locations.
- 5/4/2019 - Auto updater.
- 5/3/2019 - Fixed dev prompts, fixed some bugs with F5, improved patching reliability, aliases for listing processes locking a file.
- 4/5/2019  - Kills processes locking files during patch operation, verifies hashes on restore, less likely to accidentally delete backups on exception, and enables F5 debugging.
- 3/23/2019 - Fix revert when patch is run multiple times and add navigation aliases.
- 3/20/2019 - Wait for VS config tasks to complete and store scratch outside of install directory.
- 3/19/2019 - Added patching aliases.
- 3/18/2019 - Added many tools and README.
