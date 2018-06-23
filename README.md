
# HeroesOfTheStorm_TryMode2.0
A modified Try Mode to create a better experience on Heroes of the Storm - Try mode.

## Tools

All tools require `nodejs` to do so. After installed `nodejs` and cloned the repo, do a `npm install`.

### Automatically generate `GameData.XML`

-This tool will automatically generate the `GameData.XML` under `./(10)trymemode.stormmap/base.stormdata/GameData.xml`. Note that it was preset that all XML modifications must be inside the `Mods` folder (`./(10)trymemode.stormmap/base.stormdata/Mods`). Any subdirectories are not limited and will automatically include when generating `GameData.XML`.

To use the tool after created XML files under `Mods` folder, simply use terminal (or CMD) with the command `npm run buildxml`. Also note that it **will ignore** any files that does not end with `.xml` (case insensitive) and will **not** validate whether the XML file is valid (syntax error, etc).


## Notes:

### When Editing Asset Files (.dds)
-Recommended using Photoshop plugin [NVIDIA Texture Tools for Adobe Photoshop](https://developer.nvidia.com/nvidia-texture-tools-adobe-photoshop) to do so. 
When saving, remember to choose `No MIP maps` inside the `MIP Map Generation` section. 
*(Took me a long time to figure it out why the texture is not loading.....)*