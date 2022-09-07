# F1Manager 2022 Modding Guide
This is a guide to making numerical change mods to the game F1 Manager 2022.

## Requirements
- Asset Editor by kaiheilos https://github.com/kaiheilos/Utilities
- WinHex https://www.x-ways.net/winhex/

## Preparing Modding Files
Follow Unpacking guide found [here](https://github.com/carefreeduck/F1ManagerModding/blob/main/Packing.md).

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

Our files are ready to be repacked and used as a mod. You can follow the guide [here](https://github.com/carefreeduck/F1ManagerModding/blob/main/Packing.md) for repacking.

## Understanding .uasset and .uexp files
The way Unreal Engine works is .uasset files keep information about data used by the game and .uexp files keep the data itself. So to explain using our mod example, .uasset tells us that there is a number that is called `TyreWearPaceMode` and it is a floating point number. The file also tells us where this number is located (offset). But to change the number itself, we need to change .uexp file, where it is actually located.

## Can I change only numbers?
You can change anything as long as you can modify the files in the correct way. As explained before, .uasset keeps information about data while .uexp keeps the data itself. Changing numbers is easy because their data size doesn't change. If you wanted to change something else and ended up with a different data size, you would need to change all the offsets coming after that one and file size information kept in .uasset file. There are programs people developed to make this easier but unfortunately I couldn't find one that works on this engine version (4.27).
