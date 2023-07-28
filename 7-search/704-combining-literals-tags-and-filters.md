---
layout: default
title: Combining literals, tags and filters
parent: Search
nav_order: 704
permalink: /search/combining-literals-tags-and-filters
---

# Combining literals, tags and filters in the search bar
v1.2
{: .label .label-purple}

- The following are examples of searches that combine literals, tags and filters. It is good practice to enclose the literal by quotation marks ("").

{: .note-title }
> Example searches:
>
> Tag1 **and** "meeting" **and** <mark style="background-color: #FFF0EE">created<3days</mark>
>
> Tag2 **and** (<mark style="background-color: #FFF0EE">task</mark> **or** <mark style="background-color: #FFF0EE">file</mark>) **and** "update"

<br/>
The following combinations are currently not supported:
- Combining two literals (e.g., file1 **or** file2)
- Combining a literal and a filter