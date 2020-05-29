# Tools

All tools require `nodejs` to do so. After installed `nodejs` and cloned the repo, do a `npm install`.

In addition, it requires a Linux Enviorment to do so. If you on Linux, great, if you on Windows, try using `WSL` with `Ubuntu` installed.

>TODO: Make it compatible with Windows.

## Mimic Lib Generator

**BREAKING: THIS TOOLCURRENTLY DOES NOT WORK, PLEASE DO NOT USE IT.**

**Command**: `npm run buildmimic '/path/to/heroesofthestorm/'`

This tool will automatically generates the mimics libs to `./(10)trymemode.stormmap/base.stormdata/ModuleMimicLibs`.

The Mimic Libs will be taked from `heroesdata.stormmod/base.stormmod/TriggerLibs`.

Currently will mimic:

- `GameDataHelperLib.galaxy`
- `GameDataHelperLib_h.galaxy`
- `GameLib.galaxy`
- `GameLib_h.galaxy`
- `HeroesLib.galaxy`
- `HeroesLib_h.galaxy`
- `MapMechanicsLib.galaxy`
- `MapMechanicsLib_h.galaxy`
- `SoundLib.galaxy`
- `SoundLib_h.galaxy`
- `StartingExperienceLib.galaxy`
- `StartingExperienceLib_h.galaxy`
- `SupportLib.galaxy`
- `SupportLib_h.galaxy`
- `UILib.galaxy`
- `UILib_h.galaxy`

### Internal Library Mimics

Mimic Librarys are identical to internal trigger libs (Created by Blizzard). However, due to some restrictions, some functionalities cannot be easily used (for example it will detect whether it is a development build or production build, to prevent normal users accessing the commands.)

The Mimic Libs are from `heroesdata.stormmod/base.stormdata/TriggerLibs` and they are stored in `base.stormdata/ModuleMimicLibs`.

>TODO: Found out what cause LibAIAI.galaxy to crash the map when mimic-ed. Currently exclude that lib from mimicking. 

>Note: To generate your own Mimic Lib, see [Tools Section](#Tools).

Hence, I have create a mimic lib that is identical to internal libs, but modified a few things.


#### Renamed all triggers, variables, function etc.

Well, common sense. To prevent conflict with the origional library, I have renamed the prefix name to `Mimic[LibFileName]`, for example `libSprt_gt_DEBUGShortHeroDeath_Func` will changed to `MimicSupportLib_gt_DEBUGShortHeroDeath_Func`.

#### Changed Game Cheat Detection (Dev/Prod build detect)

Since there are two ways that Blizzard detect whether does the game are in dev mode (maybe by different Blizzard Staff), some of the triggers have them both validated:

    if (!((libCore_gv_dEBUGDebuggingEnabled == true))) {
        return false;
    }

This is most commonly used, however it can be easily bypass it, as `libCore_gv_dEBUGDebuggingEnabled` is just a variable that can be overwritten, which already done in `libDEBUG.galaxy`.

However, another checking method on some triggers can not bypass:

    if (!((GameCheatsEnabled(c_gameCheatCategoryDevelopment) == true))) {
        return false;
    }

Because this `c_gameCheatCategoryDevelopment` is a **constant** that was pre-defined (which cannot be changed and will crash if you "forcefully" do so).

Hence, all the mimic internal libs will change `if (!((GameCheatsEnabled(c_gameCheatCategoryDevelopment) == true))` to `if (!((GameCheatsEnabled(c_gameCheatCategoryDevelopment) == false))` validation check (but not `libCore_gv_dEBUGDebuggingEnabled`).

#### Override `TriggerDebugOutput()` with a custom command `MimicTriggerDebugOutput()`

Since those code are for debug purpose only, if you have played around SC2 Maps, you should come around the `TriggerDebugOutput` function. This is internally, which looks something like this:

![SC2 debug window](https://i.imgur.com/7IofkYI.png)

However, I cannot find a way to get the debug window up despite manually calling the debug window out. This tool is extremely useful for debugging if Heroes can call it out. Well, unfortunately.

Those messages still carry out important infomation about the current state of the trigger call, therefore, I changed the `TriggerDebugOutput` inside the mimic internal libs to a custom function, that will output the message to `c_messageTitleDebug` instead, like how all the chat commands output.

The Lib that countain this command is also generated via the mimic [tool](#Tools), which is `ModulesMimicLibs/MimicTriggerDebugOutput.galaxy`.

#### Prefixing Chat Commands

In order to prevent a chat command have conflict with the original lib (for example both internal and mimic lib listen for the same chat command and execute the same function), I have added a `m` prefix to all chat commands, regardless what characters do they start with.

For example:

Internal Command | Mimicked Command
--- | --- 
`invulnerablestructures` | `minvulnerablestructures`
`BUILDINGSCALING` | `mBUILDINGSCALING`
`-AI All` | `m-AI All`


---

## Finder Tool for `*.s2ma`

**Command**: `npm run s2ma`

>Note: Due to poorly designed *(sigh...)*, it will take a while depends on your processing power.

`*.s2ma` files are the generated libs for Heroes of the Storm. But most importantly it will also contain actual map files as well.

The tool will find all of the `*.s2ma` files and output them to `s2ma/` directory.

To view or extract them, I suggest uses [MPQ Editor](http://www.zezula.net/en/mpq/download.html).

## Automatically generate `GameData.XML`

**Command**: `npm run buildxml`

This tool will automatically generate the `GameData.XML` under `./(10)trymemode.stormmap/base.stormdata`. 

>Note: It **will ignore** any files that does not end with `.xml` (case insensitive) and **does not** validate whether the XML file is valid (syntax error, etc).

---

## Trigger code generator

**DEPRECATED: THIS TOOL WILL BE REMOVED SOON**

**Command**: `npm run triggergenerator`

This tool will help you to generate the codes to create a simple trigger function. Just follow the instructions and you can build a basic trigger code.

You can also get trigger functions from either Starcraft 2 editor (Modules->Triggers->[Add some Events with the UI]->Data->View Script.)