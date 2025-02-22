---
title: Configuring Doorstop
nav_order: 1
parent: Guides
grand_parent: Home
---

# Configuring Doorstop

![Static Badge](https://img.shields.io/badge/Skill_Level-Beginner-blue?style=for-the-badge)  
![Static Badge](https://img.shields.io/badge/Estimated_Time-5_Minutes-blue?style=for-the-badge)

{: .warning}
> The developers of the game have stopped stripping the game assemblies so this guide is obsolete and following it will just add extra space to your game install. It has been left up for historical purposes but will not provide any benefit to follow these instructions

{: .note}
> This guide assumes you've already installed BepInEx following the instructions from their repository. You specifically want the [latest version 5 LTS](https://github.com/BepInEx/BepInEx/releases/tag/v5.4.23.2)

<hr/>
Table of Contents

- [Acquire Unstripped Assemblies](#acquire-unstripped-assemblies)
- [Extract The Unstripped Assemblies](#extract-the-unstripped-assemblies)
- [Edit Doorstop Config](#edit-doorstop-config)

## Acquire Unstripped Assemblies

Since Obenseuer uses `Unity 2019.4.40f` we'll need the core files and libraries for that specific version.

- [Core Files](https://unity.bepinex.dev/corlibs/2019.4.40.zip)
- [Libraries](https://unity.bepinex.dev/libraries/2019.4.40.zip)

## Extract The Unstripped Assemblies

The files are both named after the unity version so your browser likely named one `2019.4.40.zip` and the other `2019.4.40(1).zip`, that's okay, it doesn't matter which order you extract them in anyway so you'll be fine.

- Right click the game in your steam library and go to manage -> browse local files
- Make a new folder here called `unstripped` and extract the contents of both archives directly inside with no additional folders. You did it right if you can see `Microsoft.Csharp.dll` and `Mono.Posix.dll` inside your unstripped folders (among others)

## Edit Doorstop Config

Go back to the game directory and open `doorstop_config.ini` and find a section like the following

```ini
# Options specific to running under Unity Mono runtime
[UnityMono]

# Overrides default Mono DLL search path
# Sometimes it is needed to instruct Mono to seek its assemblies from a different path
# (e.g. mscorlib is stripped in original game)
# This option causes Mono to seek mscorlib and core libraries from a different folder before Managed
# Original Managed folder is added as a secondary folder in the search path
# To specify multiple paths, separate them with semicolons (;)
dll_search_path_override =
```

By default Doorstop doesn't have a search path override so you'll need to add the unstripped folder you just made to this

```ini
dll_search_path_override = unstripped
```

Great job! Now BepInEx won't crash on startup anymore! Have fun using Obenseuer mods made for Bepin!
