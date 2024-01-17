---
layout: default
title: Settings - Tags
parent: General
nav_order: 808
permalink: /general/settings-tags
---

# Settings - Tags

The following settings are available under Tags.

### Tag Aggregation Depth
When you navigate to a folder inside Ritt, Ritt scans the files and folders displayed in the main pane and finds any tags that the files and folders are tagged to. These relevant tags are displayed directly under the address bar to allow for quick filtering and navigation.

This setting sets the depth that Ritt scans in order to identify these relevant tags. The default selection is 1. 
- When the **OFF** option is selected, relevant tags will not be displayed. 
- When the **1** option is selected, Ritt scans only the items displayed in the main pane.
- When the **2** option is selected, Ritt scans the items displayed in the main pane plus the items contained in any subfolders.
- When the **3** option is selected, Ritt scans the items displayed in main pane, those contained in subfolders, and items contained in subfolders or subfolders.

In a nutshell, this setting determines how deep Ritt should scan to find tags that are tagged to the items in a selected folder. The maximum depth is **4**.


### Auto expand tag tree
Turned off by default. If this is switched on, on selecting a tag in the tag pane, the tree of children tags will be automatically expanded.

### Display delete confirmation
Turned on by default. On deleting an item (i.e., a file, folder, or tag), a confirmation dialog will be displayed.<br/>
*(This is the same setting as the last setting under Files and Folders.)*