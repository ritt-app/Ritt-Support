---
layout: default
title: Internal Scripting
parent: Scripting
nav_order: 1101
permalink: /scripting-api/internal-scripting
---

# Internal scripting
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>


# Preamble

Execute custom scripts written in Javascript or VBScript within Ritt to perform advanced automation. Most of the examples in this documentation are written in  Javascript. 

## Definitions
- **Datum**: A file, folder or task to be organized by Ritt

## Writing scripts
Click on the Internal Scripting button <img class="ui-icon" src="../img/Btn-Code.svg">, then the <img class="ui-icon" src="../img/Btn-Add.svg"> button to add a new script. While hovering over a script, click on the <img class="ui-icon" src="../img/Btn-Edit.svg"> button to start editing. Set the name, description and language of your script. You can edit the script in Ritt or click on the <img class="ui-icon" src="../img/Btn-OpenFile.svg"> button on open the script in your text editor. To view the location of all scripts, click on the Open script folder <img class="ui-icon" src="../img/Btn-E2B4.svg"> button.

## Executing scripts
While hovering over a script, click on the <img class="ui-icon" src="../img/Btn-Play.svg"> button to run it.

{: .warning}
The script is executed without any further confirmation. The effects of your script (e.g. file move, deletion) may be irreversible.

## Example
The following script will tag currently selected items based on its file type and size.


<button class="button selected" id="showJSExample">Javascript</button>
<button class="button" id="showVBSExample">VBScript</button>

{: .exampleJS}
```js 
// Create sets of extensions
const imageExts = new Set([".jpg", ".jpeg", ".png", ".gif", ".bmp", ".tiff", ".tif", ".webp"]);
const videoExts = new Set([ ".mp4", ".mkv", ".avi", ".mov", ".wmv", ".flv", ".webm", ".m4v", ".3gp"]);

// Define large file as more than 100MB
const largeFileBytes = 100 * 1024 * 1024;

// All interactions with Ritt are brokered via the "Asset" class
// Get the currently selected items in the Ritt main panel
const selectedItems = Asset.GetSelectedDatums();
for (const item of selectedItems){
    // Get existing tags
    const oldTags = new Set(item.Tags.map(x => x.Name));
	
    // Get the extension
    const ext = item.Extension.toLowerCase();
    // Attach tags based on extension
    if (imageExts.has(ext)){
        item.Attach("Image");
    }        
    if (videoExts.has(ext)){
        item.Attach("Video");
    }

    // Get file size
    const fileInfo = new FileInfo(item.Path);
    if (fileInfo.Exists){
        const fileSizeInBytes = fileInfo.Length;
        if (fileSizeInBytes == 0){
            item.Attach("Empty");
        }
        if (fileSizeInBytes >= largeFileBytes){
            item.Attach("Large");
        }   
    }

    // Find added tags
    const newTags = new Set(item.Tags.map(x => x.Name));
    const addedTags = newTags.difference(oldTags);
    if (addedTags.size > 0){
        Asset.Log(`"${item.Name}": added tags: ${[...addedTags].join(", ")}`);
    } 
}

// Save changes to database
Asset.Save();
```

{: .exampleVBS .hidden}
```vb
' Define image and video extensions as dictionaries
Set imageExts = CreateObject("Scripting.Dictionary")
imageExts.Add ".jpg", True
imageExts.Add ".jpeg", True
imageExts.Add ".png", True
imageExts.Add ".gif", True
imageExts.Add ".bmp", True
imageExts.Add ".tiff", True
imageExts.Add ".tif", True
imageExts.Add ".webp", True

Set videoExts = CreateObject("Scripting.Dictionary")
videoExts.Add ".mp4", True
videoExts.Add ".mkv", True
videoExts.Add ".avi", True
videoExts.Add ".mov", True
videoExts.Add ".wmv", True
videoExts.Add ".flv", True
videoExts.Add ".webm", True
videoExts.Add ".m4v", True
videoExts.Add ".3gp", True

' Define large file threshold (100MB)
largeFileBytes = 100 * 1024 * 1024

' Get selected items from Asset
Set selectedItems = Asset.GetSelectedDatums()
Set fso = CreateObject("Scripting.FileSystemObject")

For Each item In selectedItems

    ' Create dictionary for old tags
    Set oldTags = CreateObject("Scripting.Dictionary")
    For Each tag In item.Tags
        oldTags(tag.Name) = True
    Next

    ' Get extension and make lowercase
    ext = LCase(item.Extension)

    ' Attach tags based on extension
    If imageExts.Exists(ext) Then
        item.Attach("Image")
    End If
    If videoExts.Exists(ext) Then
        item.Attach("Video")
    End If

    ' Get file size
    If fso.FileExists(item.Path) Then
        Set file1 = fso.GetFile(item.Path)
        fileSizeInBytes = file1.Size
        If fileSizeInBytes = 0 Then
            item.Attach("Empty")
        End If
        If fileSizeInBytes >= largeFileBytes Then
            item.Attach("Large")
        End If
    End If    

    ' Create dictionary for new tags
    Set newTags = CreateObject("Scripting.Dictionary")
    For Each tag In item.Tags
        newTags(tag.Name) = True
    Next

    ' Compute added tags (newTags - oldTags)
    Set addedTags = CreateObject("Scripting.Dictionary")
    For Each key In newTags
        If Not oldTags.Exists(key) Then
            addedTags.Add key, True
        End If
    Next

    ' Log added tags if any
    If addedTags.Count > 0 Then
        tagList = ""
        For Each key In addedTags
            If tagList <> "" Then tagList = tagList & ", "
            tagList = tagList & key
        Next
        Asset.Log("""" & item.Name & """: added tags: " & tagList)
    End If

Next

' Save changes
Asset.Save()
```


## Console
The console is a developer tool to help debug your script and log any information. You can print to console by calling ```Asset.Log()```. Any uncaught errors are automatically logged in the console. View the console by clicking the <img class="ui-icon" src="../img/Btn-ChevronRightSmall.svg"> button at the bottom right corner.

# Exposed Classes
There are several C# classes that are accessible to the user.

1. ```Asset``` class
1. ```File``` class
1. ```FileInfo```  class
1. ```Directory``` class

## ```Asset``` class
All interactions with Ritt are brokered via the ```Asset``` class. It has two main purposes:
1. It provides important static methods such as saving changes and refreshing the UI.
2. It's instances wraps around internal Ritt objects such as tags, files and folders and tasks, allowing the user to access properties of, as well as manipulate, the underlying objects.

## ```File```, ```FileInfo``` and ```Directory``` classes
These .NET APIs are exposed to allow to user to perform file and directory manipulations such as reading, moving, creating and deleting files or folders. The full list of available APIs can be found in the following links:
- [File Class](https://learn.microsoft.com/en-us/dotnet/api/system.io.file) 
- [FileInfo Class](https://learn.microsoft.com/en-us/dotnet/api/system.io.fileinfo)
- [Directory Class](https://learn.microsoft.com/en-us/dotnet/api/system.io.directory)

Here are some commonly used methods.

**Create** all directories and subdirectories in the specified path unless they already exist.
```js
Directory.CreateDirectory("path\to\new\folder");
```

**Open** a text file, **read** all the text in the file, and then closes the file.
```js
const readText = File.ReadAllText("path\to\file");
```

**Create** a new file, write the contents to the file, and then closes the file. If the target file already exists, it is truncated and overwritten.
```js
const contents = "Hello!"
File.WriteAllText("path\to\file", contents);
```

**Get the names of files** (including their paths) or subdirectories in the specified directory.
```js
const files = Directory.GetFiles("path\to\folder");
const folders = Directory.GetDirectories("path\to\folder");
```

**Copy** an existing file to a new file. Overwriting a file of the same name is allowed.
```js
const overwrite = true;
File.Copy("path\to\existing\file", "path\to\new\file", overwrite);
```

**Move** a file or a directory and its contents to a new location. This is also used to rename a file or directory.
```js
// Move an existing file
File.Move("path\to\existing\file", "path\to\new\file");
// Move an existing file or directory
Directory.Move("path\to\existing\file\or\directory", "path\to\new\file\or\directory");
```

**Delete** the specified file, specified directory, and optionally any subdirectories.
```js
File.Delete("path\to\existing\file");
// Specify whether or not to delete folder contents i.e. files and subfolders
// If false, this method will only delete an empty folder
const deleteFolderContents = true;
Directory.Delete("path\to\existing\directory", deleteFolderContents)
```

## ```Asset``` arrays
Many ```Asset``` methods return an array of ```Asset``` objects. For example, ```files``` in the following code is an array.

```js
const files = myFolder.Children;
```

For Javascript scripts, this is a native [Javascript array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array). For VBScript scripts, this is a [.NET array](https://learn.microsoft.com/en-us/dotnet/api/system.array).

# Properties of the ```Asset``` class
```Asset``` objects are wrappers around internal Ritt objects such as tags, files and folders and tasks. 

## Get and Set name
Get and Set the name of a file, folder, tag or task. The contents of a task is its name. When renaming a file or folder, any disallowed characters \(\\/\:\*?\"\<\>\|\) will be automatically replaced by an underscore \(\_\).
```js
const filename = myFile.Name;
const tagname = myTag.Name;
myFile.Name = "A new name";
myTask.Name = "Reply to Mark's email";
```

## Get path
Get the path of a file or folder as a string.
```js
const path = myFile.Path;
```

## Get extension
Get the extension of a file, including the period (".").
```js
// "cars.png" -> ".png"
// "cars.old.png" -> ".png"
// "cars" -> ""
const ext = myFile.Extension;
```

## Get parent
Get the parent ```Asset``` object. This applies for both tags and files/folders.
```js
const parent = myAsset.Parent;
```

## Get children
Get the children of a folder or tag as an ```Asset``` array.
```js
const files = myFolder.Children;
const childrenTags = myTopLevelTag.Children;
```

## Get and Set notes
Get and Set the Notes of a file/folder/task or tag
```js
const notes = myFile.Notes;
myTag.Notes = "A short description";
```

## Get and Set alias
Get and Set the [alias](/tags/tag-alias) of a tag
```js
const alias = myTag.Alias;
myTag.Alias = "Another name for this tag";
```

## Get tags

Get tags attached to a datum. You can get either direct or [inherited](/tags/inherited-tags) tags, or both.
```js
// Direct tags are those directly attached to a datum
const directTags = myFile.DirectTags;

// Inherited tags are those attached to a parent folder
const inheritedTags = myFile.InheritedTags;

// Get both direct and inherited tags
const myTags = myFile.Tags;
```

## Get datums
Get the datums attached to a tag, or [linked items](/tasks/linking-items-to-tasks) attached to a datum.
```js
const datums = myTag.Datums;
const linked = myFile.Datums;
```

# Methods of the ```Asset``` class
Access application level commands by calling the static or instance methods of the ```Asset``` class. 

Methods that are invoked by the ```Asset``` class are **static** methods. E.g.
```js
Asset.Save();
```

Methods that by invoked by an ```Asset``` object are **instance** methods. E.g.
```js
myFile.Attach("Important");
```

## Save changes 

Save changes made to the Ritt database.
```js
Asset.Save();
```

## Refresh
Refresh UI to view changes.
```js
Asset.Refresh();
```

## Print to console
Print a string to the console. View the console by clicking the ">" button at the bottom right corner.
```js
Asset.Log("Hello!");
```

## Get displayed datums
Get current datums that are displayed in the Main Pane of Ritt as an ```Asset``` array. In Ritt, a datum is a file, folder or task.
```js
const currentDatums = Asset.GetDisplayedDatums();
```


## Get selected datums
Get datums that are selected in the Main Pane of Ritt as an ```Asset``` array.
```js
const selectedDatums = Asset.GetSelectedDatums();
```

## Get activated tags
Get tags that are activated (selected in the Tag pane) as an ```Asset``` array. Positive tags are tags that are activated for inclusion (default) and negative tags are those that are activated for exclusion (shown as red, crossed-out).
```js
const positiveTags = Asset.GetPositiveTags();
const negativeTags = Asset.GetNegativeTags();
```

## Get activated datums
Get datums that are activated as an ```Asset``` array. For example, if you are navigated to the Desktop folder, then that folder is considered "activated".
```js
const activatedDatums = Asset.GetActivatedDatums();
```


## Get tag root
Get an ```Asset``` object representing the tag root. The tag root is a tag that is not shown in the UI. Its children are top level tags show in the tag tree panel.
```js
const tagRoot = Asset.GetTagRoot();
```

## Get all tags
Get an ```Asset``` array containing all tags in the tag tree.
```js
const allTags = Asset.GetAllTags();
```


## Get tags by name
Get tags with that exact name (case-sensitive) as an ```Asset``` array. Since tags with the same name are allowed in Ritt, multiple tags could be returned.
```js
const tags = Asset.GetTagsByName("In Progress");
```

## Find tags
Performs a fuzzy search for tags by name or alias. This uses the same internal search engine as Ritt's search bar. Returns an ```Asset``` array.
```js
const tags = Asset.FindTags("In Progress");
```

## Create tags
Create children tags under a parent tag, and get them as an ```Asset``` array. Top-level tags can be created under the tag root.
```js
const childrenTags = parentTag.CreateTags(["ProjectA", "ProjectB"]);
const topLevelTags = Asset.GetTagRoot().CreateTags(["ProjectA", "ProjectB"]);
```

## Get datum by path
Get a datum by its absolute path. Returns an ```Asset``` array.
```js
const myFile = Asset.GetDatumByPath(String.raw`C:\Users\MyName\Documents\Hello123.txt`);
```

## Attach and unattach tags
Attach/unattach tags to datums. This can be called on datum or tag objects.
```js
// myFile, myTag etc are all Asset objects
myFile.Attach(myTag);
myFile.Attach([myTagA, myTagB]);

myFile.Unattach(myTag);
myFile.Unattach([myTagA, myTagB]);

// The reverse also works
myTag.Attach([myFileA, myFileB]);
myTag.Unattach([myFileA, myFileB]);
```

Attach tags to a datum using strings. Ritt will search for all existing tags with that **exact** name (case-sensitive) and attach them to the datum. If not found, new tags under the tag root are created and attached. This can only be called on datum ```Asset``` objects.
```js
myFile.Attach("Important");
myFile.Attach(["Important", "To Do"]);

myFile.Unattach("myTag");
myFile.Unattach(["myTagA", "myTagB"]);
```

## Unattach all tags
Unattach all direct tags from a file or folder.
```js
myFile.UnattachAllTags();
```

## Copy datums or tags to a new destination
Copy one or more datums or tags to a new parent. Note that the target and destination type (i.e. datum or tag) must match. Returns the copied datums or tags.
```js
// Copy myTargetFile to a new location: myDestinationFolder
// copiedFile is an Asset object
const copiedFile = Asset.CopyTo(myTargetFile, myDestinationFolder);
// Copy many files to myDestinationFolder
// copiedFiles is an array of Asset objects
const copiedFiles = Asset.CopyTo([myFile1, myFile2, myFile3], myDestinationFolder);

// Copy myTargetTag to a new parent tag: myDestinationTag
// copiedTag is an Asset object
const copiedTag = Asset.CopyTo(myTargetTag, myDestinationTag);
// Copy many tags to myDestinationTag
// copiedTags is an array of Asset objects
const copiedTags = Asset.CopyTo([myTag1, myTag2, myTag3], myDestinationTag);
```

## Move datums or tags
Move one or more datums or tags to a new parent. Note that the target and destination type (i.e. datum or tag) must match.
```js
// Move myTargetFile to a new location: myDestinationFolder
Asset.MoveTo(myTargetFile, myDestinationFolder);
// Move many files to myDestinationFolder
Asset.MoveTo([myFile1, myFile2, myFile3], myDestinationFolder);

// Move myTargetTag to a new parent tag: myDestinationTag
Asset.MoveTo(myTargetTag, myDestinationTag);
// Move many tags to myDestinationTag
Asset.MoveTo([myTag1, myTag2, myTag3], myDestinationTag);
```

## Duplicate datums or tags
Duplicate datums or tags.  All child datums/tags are duplicated recursively. Attachment to tags are copied e.g. a duplicate file will have the same tags as the original.
```js
// Static method
const duplicatedFiles = Asset.Duplicate([myFile1, myFile2, myFile3]);
const duplicatedTags = Asset.Duplicate([myTag1, myTag2, myTag3]);

// Instance method
const duplicatedFile = myFile.Duplicate();
const duplicatedTag = myTag.Duplicate();
```

## Merge tags
Merge two or more tags as one. This operation will merge the attached datum and children tags of the subsequent tags into the first one. Only the name, attributes and other properties of the first tag be remain after the merge.
```js
// Static method
// Note that myTagB, myTagC are merged into myTagA
// The icon, alias, notes and attributes of myTagB, myTagC will be lost 
const mergedTag = Asset.MergeTags([myTagA, myTagB, myTagC]);

// Instance method
// Note that myTagB, myTagC are merged into myTagA
// The icon, alias, notes and attributes of myTagB, myTagC will be lost 
myTagA.MergeWith([myTagB, myTagC]);
```

## Delete datums or tags
Delete datums (files, folder, tasks) or tags. 

**Parameters**
- ```permanent```: If ```true```, files and folders are permanently deleted. Otherwise, they are moved to the Recycle Bin. Defaults to ```false```. This has no effects on tasks and tags.

```js
// Static method
// Decides if files/folders are permanently deleted or moved to the Recycle Bin
const permanent = true;
Asset.Delete([myFileA, myFileB, myTagA], permanent);

// Instance method
myFile.Delete(permanent);
myTag.Delete();
```


<script>
document.addEventListener("DOMContentLoaded", function () {
  document.getElementById("showJSExample").addEventListener("click", function () {
    document.querySelector(".exampleJS").classList.remove("hidden");
    document.querySelector(".exampleVBS").classList.add("hidden");
    document.getElementById("showJSExample").classList.add("selected");
    document.getElementById("showVBSExample").classList.remove("selected");
  });
  document.getElementById("showVBSExample").addEventListener("click", function () {
    document.querySelector(".exampleJS").classList.add("hidden");
    document.querySelector(".exampleVBS").classList.remove("hidden");
    document.getElementById("showJSExample").classList.remove("selected");
    document.getElementById("showVBSExample").classList.add("selected");
  });
});
</script>

<style>
.hidden {
  display: none;
}
</style>
