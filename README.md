# Geo SAM

By Joey and [Fancy](https://github.com/Fanchengyan) from [Cryosphere Lab](https://cryocuhk.github.io/), ESSC, CUHK.

## Introduction

Geo SAM is a QGIS plugin tool that aims to help people segment, delineate or label landforms efficiently when using large-size geospatial raster images. [Segment Anything Model](https://segment-anything.com/) (SAM) is a foundation AI model with the superpower, but the model size is huge, and using it to process images can take a long time, even with a modern GPU. Our tool uses the strategies of encoding image features in advance and trimming the SAM model. The interactive segmentation process can be run in real-time on a laptop by only using a CPU, making it a convenient and efficient tool for dealing with satellite images.

The tool currently only supports preprocessed images (whose features have been generated in advance using a separate program, as the included demo image). We are now building another tool for encoding image features inside QGIS, which will soon be available. So stay tuned.

## Installation

### Install QGIS

You are suggested to install the latest version of [QGIS](https://www.qgis.org/en/site/forusers/download.html) since the plugin has only been tested on the versions newer than QGIS 3.30 (at least ver. 3.28 is recommended).

### Install Library Dependencies

#### For Windows Users

![OsGeo4WShell](./assets/OsGeo4WShell.png)

<!-- <p align="center">
  <img src="./assets/OsGeo4WShell.png" width="100" title="OsGeo4WShell"> -->
  <!-- <img src="./assets/OsGeo4WShell.png" width="100" alt="OsGeo4WShell"> -->
<!-- </p> -->

Open the `OSGeo4W Shell` application from the Start menu, which is a dedicated shell for the QGIS. Then run the following command to install the libraries.

```bash
pip3 install torch torchvision
pip3 install torchgeo
pip3 install segment-anything
```

#### For Mac or Linux Users

Open your own terminal application, and change the directory to the QGIS Python environment.

```bash
# Mac
cd /Applications/QGIS.app/Contents/MacOS/bin
# Linux (Please confirm the python env by "import qgis")
cd /usr/bin
```

To confirm the QGIS Python environment:

```bash
./python3

>>> import qgis
```

Then install the libraries.

```bash
# !important, add ./ to avoid using your default Python in the system
./pip3 install torch torchvision
./pip3 install torchgeo
./pip3 install segment-anything
```

For Linux users, if `pip3` is not found in `/usr/bin`, try the following commands:

```bash
sudo apt-get update
sudo apt-get install python3-pip
```


### Install the GeoSAM Plugin

#### Download the Plugin

Download the [plugin zip file](https://github.com/coolzhao/Geo-SAM/releases/latest), unzip it, and rename the folder as `Geo-SAM` (be aware of undesired nested folders after unzipping).

#### Locate the QGIS Plugin folder

In QGIS, Go to the menu `Settings` > `User Profiles` > `Open active profile folder.`  You'll be taken straight to the profile directory. Under the profile folder, you may find a `python` folder; the `plugins` folder should be right inside the `python` folder (create the `plugins` folder if it does not exist). Put the entire `Geo-SAM` folder inside the `plugins` folder, then restart QGIS. The directory tree structure should be the same as the following.

```txt
python
└── plugins
    └── Geo-SAM
        ├── checkpoint
        ├── ...
        ├── tools
        └── ui
```

Below are some general paths of the plugin folder for your reference.

```bash
# Windows
%APPDATA%\QGIS\QGIS3\profiles\default\python\plugins
# Mac
~/Library/Application\ Support/QGIS/QGIS3/profiles/default/python/plugins
# Linux
~/.local/share/QGIS/QGIS3/profiles/default/python/plugins
```

#### Activate Geo SAM Plugin

After restarting QGIS, you may go to the `Plugins` menu, select `Manage and Install Plugins`, and under `Installed`, you may find the `Geo SAM` plugin; check it to activate the plugin.

![active geo sam](assets/Active_geo_sam.png)

#### Find the Geo SAM Tool

After activating the Geo SAM plugin, you may find the tool under the `Plugins` menu,

<p align="center">
  <img src="assets/Plugin_menu_geo_sam.png" width="350" title="Plugin menu">
</p>

Or somewhere on the toolbar near the Python Plugin.

<p align="center">
  <img src="assets/Toolbar_geo_sam.png" width="350" title="Plugin toolbar">
</p>

## Use the GeoSAM Tool

Click the toolbar icon to open the widget of the tool. You will be shown a demo raster image with thaw slump and small pond landforms for you to try the tool. With a single click on the map, a segmentation result will be generated.

<!-- ![try geo sam](assets/try_geo_sam.png) -->

<p align="center">
  <img src="assets/try_geo_sam.gif" width="500" title="Try Geo SAM">
</p>

A user interface will be shown below.

<!-- ![ui_geo_sam](assets/ui_geo_sam.png) -->

<p align="center">
  <img src="assets/ui_geo_sam.png" width="600" title="Geo SAM UI">
</p>

### Add Points

Click the buttons to select between the `Foreground` and `Background` points. Use `Foreground` points to add areas you desire, and use `Background` points to remove areas you don't want.

### Add Bounding Box (BBox)

Click the `Rectangle` button to activate the BBox tool to draw a rectangle on the map for segmenting a subject.
The BBox tool can be used together with adding points or independently.

### Save Current Results

After adding points and a rectangle for segmenting a subject, you can save the segmentation results by clicking the `Save` button.

### Clear Points and BBox

You can use the `Clear` button to clear the added points and rectangles.

### Undo the last Prompt

You can use the `Undo` button to undo the last points or rectangle Prompt.

### Disable the Tool

You can uncheck the `Enable` button to temporally disable the tool and navigate on the map.

### Load Selected Image Features

The plugin is initialized with features for demo purposes, and you can use the `Feature Folder` selection tool to select the folder that includes the image features you need.

<p align="center">
  <img src="assets/Select_feature_folder.png" width="300" title="Select feature folder">
</p>

After selecting the feature folder, you may press the `Load` button to load the features, and it may take several seconds when you load the folder for the first time. Remember to add the corresponding raster image to the QGIS project.

<p align="center">
  <img src="assets/Load_image_feature.png" width="300" title="Load feature folder">
</p>

## Shortcuts

- `Tab`: loop between 3 prompt types (the cursor will also change to the corresponding types):
  - Foreground Point
  - Background Point
  - Rectangle/BBox
- `C`: clear all prompts in canvas [same as `Clear` button]
- `Z`: undo the last prompt in canvas [same as `Undo` button]
- `S`: save SAM output features into polygon [same as `Save` button]
- `Ctrl+Z` or `command+Z`: undo the last saved features of SAM output


## Tips for Using GeoSAM Tool

- Deal with only **One Subject** each time
- Use **Background Points** to exclude unwanted parts
- Use **Bounding Box (BBox)** to limit the segment polygon boundary
- The **BBox** should cover the entire subject
- Remember to press the `Save` button after the segmentation of the chosen subject

## In Progress

- Image encoder module

## Future Works

- Existing polygon refinement

## Acknowledgement

This repo benefits from [Segment Anything](https://github.com/facebookresearch/segment-anything) and [TorchGeo](https://github.com/microsoft/torchgeo). Thanks for their wonderful work.
