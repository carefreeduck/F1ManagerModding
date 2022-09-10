# F1Manager 2022 Modding Guide
This is a guide to making numerical change mods to the game F1 Manager 2022.

## Requirements
- FModel.zip included in this repository

## Preparing Modding Files
Follow Unpacking guide found [here](https://github.com/carefreeduck/F1ManagerModding/blob/main/Packing.md).

## Modding Tyre Wear of Highest Pace Mode
To change numerical numbers, first you need to figure out which file they are located in. We will use a modified version of FModel to locate numbers and make changes to them. In this guide we will change tyre wear ratio of pace modes. From inspecting files, I found that we have these numbers defined in `RaceSimDataAsset.uasset`:

![Tyre wear numbers](https://i.imgur.com/TOzXfmp.png)

Although it doesn't say which number is for which pace mode, easy to conclude it is in pace mode order from highest to lowest and the Normal mode is not included, meaning it is 1.
First copy `RaceSimDataAsset.uasset` and `RaceSimDataAsset.uexp` files located under `ModdingFiles\Extracted\F1Manager22\Content\RaceSim` to the folder you configured FModel to look at. If you own the game through Steam and FModel could detect it, it will already have a configuration for you. If that is the case, copy these files to your game folder into `steamapps\common\F1Manager2022\F1Manager22\Content\Paks`.
If FModel couldn't detect your game or if you prefer it to use a different folder, you can add your own configuration:

![Adding new configuration](https://i.imgur.com/0Srqmen.png)

First click on the little button marked as 1. This will expand bottom section. You can put anything you want as the name. Directory should be where you want to put your files. Then click on the button marked as 2. This will add the configuration. Now you can use it.

Once you configure FModel and start it, you will see `<Local files..>` option with other pak files if your folder has pak files too.

![Game archives](https://i.imgur.com/fvY5BkH.png)

Double click on `<Local files..>` and locate to `RaceSimDataAsset.uasset` by locating through directory tree. You can double click on the file to see content.

![File contents](https://i.imgur.com/cO4a9VA.png)

You can see the `TyreWearPaceMode` numbers we are interested in (I also had some other files in the folder as you can see on the left. Don't be alarmed if those aren't shown in yours.). Now we want to export editable numbers from this file to a text file so we can change it.

![Export and import](https://i.imgur.com/4jMpusE.png)

Right click on `RaceSimDataAsset.uasset` and click on `Export Value Map`, marked as 1. Pick a location when asked and save the file. This will save a `.json` file. If you don't know what JSON is, don't worry. We can edit this file with any text editor. Now open this file in a text editor (I recommend [Notepad++](https://notepad-plus-plus.org/downloads/)).

![Edited json file](https://i.imgur.com/s9kTdyn.png)

As you can see I opened the json file in text editor and changed pace values and saved the file. Now we need to import this file. Right click on `RaceSimDataAsset.uasset` and click on `Import Value Map`, marked as 2 in above image. If you did everything correctly, you will see logs at the bottom of FModel saying our changes are applied.

![Logs](https://i.imgur.com/3yFH0XG.png)

If you don't see logs, click on the little button marked as 1.

Our files are ready to be repacked and used as a mod. First copy them into `ModdingFiles\F1Manager22\Content\RaceSim`. You will need to create these folders if they don't already exist. Note how the folder structure is same with extracted. Once you copied the files, you need to change `pack_list.txt` to list all the files you want to be packed. The downloaded file in this repository already has lines for this example. After that you can follow the guide [here](https://github.com/carefreeduck/F1ManagerModding/blob/main/Packing.md) for repacking.

## Can I change only numbers?
Modified FModel currently supports only numbers. I plan to add support for more in the future.
