---
layout: default
title: Ritt Database Format
nav_order: 12
permalink: /database-format
---

# Ritt database format
The Ritt database (.ritt files) uses the [SQlite](https://www.sqlite.org/) file format. It can be opened with any SQLite database viewer, such as [SQLite Viewer Web App](https://sqliteviewer.app/). There are several tables, the most important of which are ```RittMetadata``` and  ```RittMainGraph```. The rest of the tables are used for caching.

### RittMetadata table
There are four rows in this table, with IDs 100 to 400. The first row stores previously used tag icons ("if") and previous searches ("sh"). The second row stores metadata related to the graph such as ID and version. The third row contains the graph hash and the fourth row contains the last edit time.

### RittMainGraph table
Each row of this table contains one vertex of the Ritt graph. The information of each vertex, such as its name, type, and neighboring vertices is expressed as a [JSON](https://www.json.org/) string. 

# Ritt Graph
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

### Example graph

An example graph of a small Ritt database is shown below. The Space, Link and Tag vertices and internal Parent/Child edges (directed arrows) are shown in <span style="color:green">**green**</span>, <span style="color:orange">**orange**</span> and <span style="color:blue">**blue**</span> respectively. Edges connecting two vertices of two different types are shown as non-directed lines in <span style="color:gray">**gray**</span>.

![Small Example Graph](/img/Demo-Small.svg)

The full database file (in plain-text) that generated the above graph can be downloaded [here](/assets/221122_Demo_database_small.ritt).

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

| Key   | Attribute Name | Description                                 |
|:------|:---------------|:--------------------------------------------|
| 2848  | Hidden         | Hidden tags.                                |
| 3217  | Encapulate     | [Encapsulating tags](/tags/dashboard-tags)  |
| 4626  | Badge          | [Badge tags](/tags/tag-icon-and-attributes) |
| 4712  | Separator      | Tag tree separator                          |
| 5183  | HotKey         | Tagging hotkey                              |
| 35528 | ToDoState      | State (completed or not) of a task          |
| 50258 | DateCreated    | Date created                                |
| 54234 | DateModified   | Date modified                               |





