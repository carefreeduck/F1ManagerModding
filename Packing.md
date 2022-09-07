# F1Manager 2022 How to Unpack/pack Files
This is a guide to unpacking and packing game files of the game F1 Manager 2022.

## Requirements
- ModdingFiles.zip included in this repository
- Unreal Engine 4.27 (Can uninstall after acquiring UnrealPak)

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

## Unpacking
1. Open game folder and copy all the files with `.pak` extension under `F1Manager2022\F1Manager22\Content\Paks` into `ModdingFiles`.
4. Drag and drop `pakchunk0-WindowsNoEditor.pak` onto `unpack.cmd`. If Windows asks if you want to run, click on Run Anyway. Note that this step is required to grab engine config required for packing.
5. A console window will open. Before doing anything check `ModdingFiles\extract_list.txt` to see if it has an error or list of thousands of files. If it has a list of files hit enter and let it extract files.
6. Do this for each of the remaining `.pak` files.
7. Once the extracting is done delete `ModdingFiles\Engine`. Move `Engine` folder under `Extracted` to `ModdingFiles`.

## Packing
1. Copy changed files into `ModdingFiles` keeping the same folder structure.
2. Open `ModdingFiles\pack_list.txt`. As you can see, this file already has an example. It lists all the files that should be included in the pack with the path ***relative to*** the UnrealPak.exe file.
3. Drag and drop `pack_list.txt` file onto `pack.cmd`. If Windows asks if you want to run, click on Run Anyway.
4. A console window will open. If it doesn't show any errors, hit enter.
5. You will see a new file called `mod.pak` is created under `ModdingFiles`.
6. Copy this file into game files, under `F1Manager2022\F1Manager22\Content\Paks` and rename it to `pakchunk0-WindowsNoEditor_1_P.pak`.

## Credits
- mesagrator for their tutorial (https://gbatemp.net/threads/how-to-unpack-and-repack-unreal-engine-4-files.531784/)
- Zwiejus for providing AES key (https://old.reddit.com/r/F1Manager/comments/x5pnbl/how_to_unpack_f1_manager_22_pak_files/)
