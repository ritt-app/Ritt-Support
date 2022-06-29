---
layout: default
title: Database format
parent: User Guide
nav_order: 100
permalink: /user-guide/database-format
---

# The Ritt Database
The Ritt database (.ritt files) uses a simple and efficient format to store information. Essentially, it is a compressed text-based [JSON](https://www.json.org/) file. Here, you can learn more about the data structure used by Ritt, which will allow you to write your own code to read and modify the Ritt database file.

# File format and operations

## Decompression

To reduce file size, .ritt files are compressed using [Gzip](https://www.gnu.org/software/gzip/). To decompress, use any supported software such as [7zip](https://www.7-zip.org/). The result is a text file that can be opened with any text editor.


## Interpreting JSON file

## Opening a modified .ritt file


# Ritt data structure

Ritt uses a [directed graph](https://en.wikipedia.org/wiki/Graph_(discrete_mathematics)#Directed_graph) structure that consists of **vertices** and **edges** to store data. 

### Vertices

There are three types of vertices.
1. **Space**. A root node that points to all other nodes.
1. **Link**. A node that *links* to user data such as files, folders and tasks.
1. **Tag**. A node that stores information of a tag.

### Edges

There are five types of directed edges.
1. **Parent**. Together with the **Child** edge, this defines a [directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph) between vertices of the same type. Parent edges of a particular vertex points to the parents of said vertex.
1. **Child**. Similar to above, children edges of a particular vertex points to the children of said vertex.
1. **Space**. Points to the associated Space.
1. **Tag**. Points to the associated Tags.
1. **Link**. Points to the associated Links.

Information of edges are stored with the originating vertices as lists. 

Edges are always bidirectional. For example, if vertex **A** is the parent of vertex **B**, then **A** will have a Child edge pointing towards **B**, and **B** will have a Parent edge pointing towards **A**.

Similarly, if a link **A** as a tag **B**, then **A** will have a *tag* edge pointing towards **B**, and **B** will has a *link* edge pointing towards **A**. 

[//]: # (Add image of Ritt graph here.)



