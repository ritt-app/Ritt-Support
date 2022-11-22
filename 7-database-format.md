---
layout: default
title: Ritt Database Format
nav_order: 7
permalink: /database-format
---

# Ritt database format
The Ritt database (.ritt files) uses a simple and efficient format to store information. Essentially, it is a compressed plain text file that is written in [JSON](https://www.json.org/) format. Here, you can learn more about the data structure used by Ritt, which will allow you to write your own code to read and modify the Ritt database file.

# Ritt data structure
Ritt uses a [directed graph](https://en.wikipedia.org/wiki/Graph_(discrete_mathematics)#Directed_graph) structure that consists of **vertices** and **edges** to store data. 

### Vertices

There are three types of vertices.
1. **Space**. A root vertex. There is only one space vertex per Ritt database.
1. **Link**. A vertex that *links* to user data such as files, folders and tasks.
1. **Tag**. A vertex that stores information of a tag.

### Edges

There are five types of directed edges.
1. **Parent**. Together with the **Child** edge, this defines a [directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph) between vertices of the same type. Parent edges of a particular vertex point to the parents of the said vertex.
1. **Child**. Similar to above, child edges of a particular vertex point to the children of the said vertex.
1. **Space**. Points to the associated Space.
1. **Tag**. Points to the associated Tags.
1. **Link**. Points to the associated Links.

Information on the edges is stored with the originating vertices as lists. 

Edges are always bidirectional. For example, if vertex **A** is the parent of vertex **B**, then **A** will have a Child edge pointing toward **B**, and **B** will have a Parent edge pointing toward **A**.

Similarly, if link **A** has a tag **B**, then **A** will have a *tag* edge pointing toward **B**, and **B** will have a *link* edge pointing toward **A**. 

### Example

An example graph of a small Ritt database is shown below. The Space, Link and Tag vertices and internal Parent/Child edges (directed arrows) are shown in <span style="color:green">**green**</span>, <span style="color:orange">**orange**</span> and <span style="color:blue">**blue**</span> respectively. Edges connecting two vertices of two different types are shown as non-directed lines in <span style="color:gray">**gray**</span>.

![Small Example Graph](/img/Demo-Small.svg)

The full database file (in plain-text) that generated the above graph can be downloaded [here](/assets/221122_Demo_database_small.ritt).


# File format and operations

## Decompression

To reduce file size, .ritt files are compressed using [Gzip](https://www.gnu.org/software/gzip/). To decompress, use any supported software such as [7zip](https://www.7-zip.org/). The result is a text file that can be opened with any text editor.


## Interpreting the JSON file


### Line 1

Contains favorites and history. See below for an example and the comments for the description of the field.

```javascript
{
    "i": [ // Favorite emojis used a tag icons
        "⚡",
        "👍",
        "🔬",
        "📚",
        "💼"
    ],
    "s": [ // Search history
        "*.pdf",
        "2020"
    ]
}
```

### Line 2

Contains metadata about the graph.

```javascript
{
    "id": "4817f99e-9940-4fb5-94c9-c9c18bf858b0", // Graph unique ID
    "v": "0.13", // Graph specification version
    "l": 324, // Number of vertices in graph
    "s": { // Dictionary of indices of special vertices, typically the space root
        "root_space": 0
    }
}
```

### Line 3 to end

The rest of the file contains information on the vertices, with one vertex per line.

### Example Vertex

```javascript
{
    "p": [ // Indices of Parent vertices
        1
    ],
    "c": [ // Indices of Children vertices
        17
    ],
    "s": [], // Indices of Space vertices
    "t": [], // Indices of Tag vertices
    "l": [], // Indices of Link vertices
    "m": { // Metadata of this vertex
        "t": 2, // Vertex type, see below for details
        "n": "Work", // Name
        "c": { // Content
            "t": 2, // Content type, see below for details
            "id": "373963cd-9d3f-4305-b577-4959888b9a10" // Content ID. Used to identify and track Sources
        },
        "i": "", // Icon for tags
        "a": {} // Attribute dictionary, see below for details
    },
    "i": 14 // Index of this vertex
}
```

### Vertex Type

Type of vertex. See the previous section for details.

| Value | Name  |
| :---- | :---- |
| 0     | Space |
| 1     | Tag   |
| 2     | Link  |

### Content Type

Used in Link vertices to indicate the type of content they hold.

| Value | Name                                      |
| :---- | :---------------------------------------- |
| 0     | None                                      |
| 1     | File                                      |
| 2     | Folder                                    |
| 3     | Task                                      |
| 4     | Task Folder                               |
| 5     | StorageItemPlaceholder (For internal use) |

### Attributes

Some vertices have special attributes, which are stored in key-value pairs in an attribute dictionary.

| Key   | Attribute Name | Description                                        |
| :---- | :------------- | :------------------------------------------------- |
| 2848  | Hidden         | For future implementation of hidden tags.          |
| 3217  | Dashboard      | [Dashboard tags](/tags/dashboard-tags)             |
| 4626  | Badge          | [Badge tags](/tags/tag-icon-and-attributes)        |
| 35528 | ToDoState      | State (completed or not) of a task                 |
| 74140 | Mirror         | [Mirror tags](/tags/creating-a-tag-out-of-an-item) |


## Opening a modified .ritt file

After editing the database file in plain text, there is no need to zip it. Simply ensure that it has the ".ritt" extension and open it with Ritt. Note that Ritt will recompress the file on save.




