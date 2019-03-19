# PowerShell Tools Self Extractor
(C) 2019 Christian Gunderman
Contact Email: gundermanc@gmail.com

## Introduction:
This repo is the current and future home of a self-extracting PowerShell
tools archive that contains a number of useful scripts and aliases.

## 'Building'
- Clone the repository recursively
- Run Build.cmd
- Build produces 'StandaloneInstaller.bat' with packaged scripts.

## 'Installing'
Installation extracts tools to your user directory and creates a shortcut on the desktop
and the Start Menu as well as registering a 'tools' command to your user path.
- Run 'StandaloneInstaller.bat'
- Run Install-Tools

## Tools
### Find
Aliases for finding files and text:
- findp [path]: Searches subdirectory for paths containing [path].
- findif [text]: Searches subdirectory for files containing [text].

### MSBuild
Defines some aliases for MSBuild as well as a clickable GUI error list
that makes navigating MSBuild spew more pleasant.
- msbbuild: Build with typical settings
- msbebuild: Build with typical settings and pipe the output into a GUI error list.
- msbclean: Clean project
- msbrestore: Perform Nuget restore.

### Process Snapshot
Defines some convenient scripts and aliases for killing groups of processes
to help get you unblocked if a build executable is locking a file.
- pbget: Gets a list of Process Baseline profiles that you have saved.
- pbnew [profile]: Dumps all active processes to a named profile or 'Default' profile if not specified.
- pbnstop [profile]: Stops all executables except those reference by the given profile or 'Default' profile if not specified.
- pbstop [profile]: Stops all executables referenced by the given profile or 'Default' profile if not specified.
- pbedit [profile]: Opens a named profile for manual editing.

### Visual Studio
Aliases for launching Visual Studio installs developer command prompts.
- vsget: Lists all VS installs and their [instance] number.
- vsstart [instance]: Starts a specific VS instance by its number.
- vsreset [instance]: Wipes a specific VS instance by its number.
- vsconfig [instance]: Updates configuration timestamp on a specific VS.
- vspath [instance]: Opens installation path on a specific VS.
- vscmd [instance]: Launches developer command prompt for a specific VS.

## ChangeLog
- 3/18/2019 - Added many tools and README.