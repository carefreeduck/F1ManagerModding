# F1Manager 2022 Modding Guide
This is a guide to making numerical change mods to the game F1 Manager 2022.

## Requirements
- ModdingFiles.zip included in this repository
- Unreal Engine 4.27 (Can uninstall after acquiring UnrealPak)
- Asset Editor by kaiheilos https://github.com/kaiheilos/Utilities
- WinHex https://www.x-ways.net/winhex/

## Preparing Modding Files
1. Extract ModdingFiles.zip content to anywhere you want.
2. Install Unreal Engine 4.27 and open `UE_4.27\Engine\Binaries\Win64`. From here you will need to copy some files to `ModdingFiles\UnrealPak\2\3`. Here is the list of all the files needed:
   - UnrealPak.exe
   - UnrealPak.modules
   - UnrealPak-Analytics.dll
   - UnrealPak-BuildSettings.dll
   - UnrealPak-Core.dll
   - UnrealPak-CoreUObject.dll
   - UnrealPak-DerivedDataCache.dll
   - UnrealPak-Json.dll
   - UnrealPak-PakFile.dll
   - UnrealPak-PakFileUtilities.dll
   - UnrealPak-Projects.dll
   - UnrealPak-RSA.dll
   - UnrealPak-SSL.dll
   - UnrealPak-TraceLog.dll
3. Open game folder and copy `pakchunk0-WindowsNoEditor.pak` under `F1Manager2022\F1Manager22\Content\Paks` into `ModdingFiles`.
4. Drag and drop `pakchunk0-WindowsNoEditor.pak` onto `unpack.cmd`. If Windows asks if you want to run, click on Run Anyway. Note that this step is required to grab engine config required for packing.
5. A console window will open. Before doing anything check `ModdingFiles\extract_list.txt` to see if it has an error or list of thousands of files. If it has a list of files hit enter and let it extract files.
6. Once the extracting is done delete `ModdingFiles\Engine`. Move `Engine` folder under `Extracted` to `ModdingFiles`.

## Modding Tyre Wear of Highest Pace Mode
To change numerical numbers, first you need to figure out which file they are located in. I recommend [FModel](https://github.com/4sval/FModel) for quickly checking files. In this guide we will change tyre wear ratio of highest pace mode. From inspecting files, I found that we have these numbers defined in `RaceSimDataAsset.uasset`:

![Tyre wear numbers](https://i.imgur.com/TOzXfmp.png)

Although it doesn't say which number is for which pace mode, easy to conclude it is in pace mode order from highest to lowest and the Normal mode is not included, meaning it is 1. So we need to change 1.1. To do that we need to find location of this number in `RaceSimDataAsset.uexp` file (If you would like to know more about .uasset and .uexp files, check end of this guide).
First copy `RaceSimDataAsset.uasset` and `RaceSimDataAsset.uexp` files located under `ModdingFiles\Extracted\F1Manager22\Content\RaceSim` to `ModdingFiles\F1Manager22\Content\RaceSim`. You will need to create folders with same names under `ModdingFiles`.
Run AssetEditor.exe downloaded earlier. Go to `File->Open` and open `RaceSimDataAsset.uasset`. From left expand `Code Block` and click on `Block 1: RaceSimDataAsset`. This will show numerical values in the file on the right. Find `TyreWearPaceMode` values and note the offset on the left:

![Offset](https://i.imgur.com/FlAbeds.png)

Open WinHex downloaded earlier. Drag and drop `RaceSimDataAsset.uexp` you copied earlier into WinHex. This will show us the file content in hex values. Do not be scared, it is not that hard! First go to `View->Show->Data Interpreter` from top. Then go to `Options->Data Interpreter...`. From the screen that opened, untick everything and tick `Float (=Single, 32 bit)` and then click `OK`. This small window opened will let us see the numbers starting from an index.

Before moving on, check `Offset` on the left of your WinHex window and if it shows letters with numbers, click on it once. This will change offset from hexadecimal to decimal, the numbers we are most familiar with.

Remember we noted offset of the value we wanted to change as 373. Now we need to find this offset. From top go to `Navigation->Go to Offset...`. In the window opened, type 373 and click OK (make sure relative to is selected as `beginning`). This will take us to the location our tyre wear number is. You can see now that our Data Interpreter window is showing us the original number:

![Data Interpreter](https://i.imgur.com/s7T0HdZ.png)

Now click on the text box in Data Interpreter and type 3 and hit enter. This will change the original value to 3 and you can see changed hex values in blue:

![Data Interpreter](https://i.imgur.com/9KAstEQ.png)

Now you can do CTRL + S and save your file. When asked if you sure, click `Yes`.

Our files are ready to be repacked and used as a mod. Remember we copied files to under `ModdingFiles` keeping the same folder structure. Now open `ModdingFiles\pack_list.txt`. As you can see, for this example I already prepared this file. It lists all the files that should be included in the pack with the path ***relative to*** the UnrealPak.exe file.

To repack files, simply drag and drop `pack_list.txt` file onto `pack.cmd`. If Windows asks if you want to run, click on Run Anyway. A console window will open. If it doesn't show any errors, hit enter. You will see a new file called `mod.pak` is created under `ModdingFiles`. Copy this file into game files, under `F1Manager2022\F1Manager22\Content\Paks` and rename it to `pakchunk0-WindowsNoEditor_1_P.pak`.

Now your mod is ready and you can start the game, do a quick practice run and check it :)

## Understanding .uasset and .uexp files
The way Unreal Engine works is .uasset files keep information about data used by the game and .uexp files keep the data itself. So to explain using our mod example, .uasset tells us that there is a number that is called `TyreWearPaceMode` and it is a floating point number. The file also tells us where this number is located (offset). But to change the number itself, we need to change .uexp file, where it is actually located.

## Can I change only numbers?
You can change anything as long as you can modify the files in the correct way. As explained before, .uasset keeps information about data while .uexp keeps the data itself. Changing numbers is easy because their data size doesn't change. If you wanted to change something else and ended up with a different data size, you would need to change all the offsets coming after that one and file size information kept in .uasset file. There are programs people developed to make this easier but unfortunately I couldn't find one that works on this engine version (4.27).

## Credits
- mesagrator for their tutorial (https://gbatemp.net/threads/how-to-unpack-and-repack-unreal-engine-4-files.531784/)
- Zwiejus for providing AES key (https://old.reddit.com/r/F1Manager/comments/x5pnbl/how_to_unpack_f1_manager_22_pak_files/)
