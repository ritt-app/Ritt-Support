---
layout: default
title: Release Notes
nav_order: 12
permalink: /release-notes
---

# Release Notes

## V1.2.2 (Oct 2023)
- Aggregating and displaying tags relevant to items in view by searching to a certain, customizeable depth (default = 2 i.e. items in view and subfolder items)
- Many new shortcut added, including one for tag entry ('T'). Now able to quickly tag items using only the keyboard. (suggested by u/AskingQuestionns)
- Previous search entries can be pinned to the searchbar for quick access
- Filter by tags after performing search

## V1.2.1 (Sep 2023)
- Interactive preview of 3D models (suggested by zendor)
- Preview of markdown files, including support for syntax highlighting and math
- Improved preview of pdfs, svg, images (zoom and pan support)
- Alternative method of tagging: drag files/folders and drop them onto a tag (suggested by UnderPL)
- Ability to quickly add new tags by mouseover buttons and context menu options (suggested by UnderPL)
- Improved search experience with demarcated search tokens

## V1.2 (Jul 2023)
- New Details pane. Shows item metadata, preview, tags, and linked items. Allows file-to-file and file-to-task linking.
- More convenient tagging in Details pane. You can create new tags when tagging items.
- Notes for files/folders. Displayed and edited in the Details pane.
- New network graph visualization of all tags and tagged items.
- Auto backup of local database. You can easily restore to a previous database version.
- Added option to export tags to sidecar files for archival purposes.
- Introduction of Ritt Pro and Lite tiers. All current users of Ritt Cloud are automatically promoted to the Pro tier for free (for a limited time). 


## V1.1 (May 2023)
- The launch of our dedicated cloud service: Ritt Cloud. Ritt Cloud lets you sync your tags and tasks across multiple devices with ease.
- Search filters such as 'task' and 'tagged' now recursively search within folders. For example, searching for 'task' from Home will return all tasks in your database.
- Organize Settings by category and merged several small menu items such as Help and About into Settings.
- Modernized in-app notification UI.

## V1.0 (Mar 2023)
- Advanced search that supports combining tags with logical operations (AND, OR, NOT) and filters (e.g. size>5mb or modified<3days)
- Related tags displayed next to currently active tags
- Group by tag combinations (instead of just individual tags)
- Can assign hotkeys (0-9) to up to 10 tags for quick tagging

## V0.9 (Feb 2023)
- Track the history of changes to tags and tasks to allow reverting to an earlier state.
- Display context menu item from Explorer in Ritt. This lets you use your favorite tools such as 7zip directly from Ritt.
- Recognize Windows shortcut files (.lnk and .url).
- Migration from text-based format to SQLite database for better scalability. Backward compatible with the old format.
- Direct download and install option (.msi) with auto-update. See our website for more details.
- Several critical bug fixes.

## V0.8 (Jan 2023)
- Migrate from UWP to Windows App SDK 1.2. Able to launch any file now (e.g. .py, .xaml).
- Integrate folder tree and tag tree in the left panel. Drag to insert and expand any folder.
- Can link files or folders with another folder by dragging from Explorer and dropping on Ritt while holding the ALT key. Similar to shortcuts, but with backlinks.
- Simplify the first launch experience. Ritt launches with a default database and all changes are automatically saved.
- Improve the performance of viewing large folders and moving/coping files. Uses system UI to display progress.
- Improve the reliability of tracking file movements, renames, and deletions in Explorer.
- Integrate out-of-the-box with QuickLook for previewing files.
- Refresh the general UI.

## V0.7 (Oct 2022)
- Privacy: Ritt operates completely offline.
- Related tags flyout
- New tag attributes: Mirror, Dashboard. Enable new use cases for Ritt
- Incremental refresh when adding or deleting an item
- Bug fixed: Reduced performance after long use, among others


## V0.6
- Task management
- Relevant tags suggestion
- Active tags bar
- Display badge next to tagged items

## V0.5
- Improved search experience
- Navigate to tag and do tagging directly from the search prompt
- Improved Previewer
- Added sort function
- Added autosave and home button

## V0.4
- Greatly simplified UI
- The left panel of Sources is removed to reduce visual clutter and transfer focus to the tag trees
- Drag and drop files/folders onto the tag tree to create tags
- Option to show contents of tagged folders, when browsing by tags
- Option to disable file/folder thumbnails to improve performance
- Bug fixes, including a memory leak issue

