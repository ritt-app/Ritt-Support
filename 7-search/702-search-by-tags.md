---
layout: default
title: Search by tags
parent: Search
nav_order: 702
permalink: /search/search-by-tags
---

# Search by tags

- To search by tags, start typing the name of the tag in the search bar.
- A list of suggested tags will appear. Select the required tag using the Tab key, or Up and Down arrow keys.
- For tags with spaces in the name, it is recommended to start typing with a foward slash "/".

## Using logical operators

- It is possible to customize your search by using logical operators (**AND**, **OR**, and **NOT**) and parantheses.

{: .note-title }
> Example search:
>
> (Tag1 **or** Tag2) **and** Tag3 **not** Tag4

The above searches the entire Ritt database for items that are tagged to Tag1 and Tag3, or Tag2 and Tag3, but not to Tag4.

- If you would like to perform a search based on the current context, start the search using an operator

{: .note-title }
> Example search:
>
> **not** (Tag1 **or** Tag2)

The above will return results that are from the current context, which are not tagged to Tag1 or Tag2.
