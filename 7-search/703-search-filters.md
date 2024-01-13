---
layout: default
title: Search filters
parent: Search
nav_order: 703
permalink: /search/search-filters
---

# Search filters

Using the search bar, it is possible to filter search results by properties such as size, date, file type, and number of tags.

- Use the **AND** and **NOT** operators to add filters to your existing search line.

{: .note-title }
> Example searches:
>
> Tag1 **and** <mark style="background-color: #FFF0EE">size>5mb</mark> **not** <mark style="background-color: #FFF0EE">task</mark>
>
> "*.txt" **and** (<mark style="background-color: #FFF0EE">size<30kb</mark> **or** <mark style="background-color: #FFF0EE">created<5hrs</mark>)
> 
> <mark style="background-color: #FFF0EE">modified>2weeks</mark> **and** <mark style="background-color: #FFF0EE">untagged</mark> *(this search will be based on the current context)*

Note: Ensure that there are **no spaces** when using these filters (e.g., type "size<5mb" instead of "size < 5 mb").

|**Supported search filter**|**Description**|**Example(s)**|
|size|Filter based on file size<br/><br/>Supported units: **b, kb, mb, gb, tb, pb, eb**<br/>If a range is used (with a dash "-"), both the lower and upper bounds are included.|- size<5mb<br/>- size>=10gb<br/>- size:10kb-1mb<br/>- size:1-10mb|
|modified<br/>created<br/>accessed|Filter based on how long ago item was created, modified, or accessed<br/><br/>Supported units: **s, min(s), hr(s), day(s), week(s), month(s), year(s)**<br/>Range includes both lower and upper bounds|- modified>5days<br/>- modified<1year<br/>- created:1week-2months<br/>- created:3min-10h<br/>- accessed:10-100s|
|tags|Filter based on number of tags<br/><br/>Range includes both lower and upper bounds|- tags<=2<br/>- tags=5<br/>- tags:3-7<br/>- tags>10 and Tag1|
|tagged|Filter that returns items with one or more tags||
|untagged|Filter that returns items without any tags||
|file|Filter that returns files only|
|folder|Filter that returns folders only||
|task|Filter that returns tasks only||
|complete|Filter that returns completed tasks only||
|incomplete|Filter that returns incomplete tasks only||