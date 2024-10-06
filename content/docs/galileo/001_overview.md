---
title: Overview
weight: 1
type: docs
simple_list: true
---

## File data

Galileo assets **MUST** be encoded in little-endian binary.

## File structure

A Galileo asset **MUST** contain a 36 byte [`File_Header`](../002_header) and a variable length `Asset_Body`.

```c
struct Galileo_File {
    File_Header     header;     // 36 bytes
    Asset_Body      body;       // variable length
};
```

The `File_Header` contains metadata applicable to any asset type, while the `Asset_Body` contains information specific to its asset type.

All asset types fall under two categories: [`primitive assets`](../primitives/) and [`aggregate assets`](../aggregates). 

- `Primitives` are the lowest-level, "raw data" assets such as `meshes`, `images` and `audio` files. These are self contained and **MAY** be
implemented individually.

- `Aggregates` are higher-level collections of other assets - they contain references to other asset files, as well as augmenting data.
Where specified, `aggregate asset` implementations **MUST** also implement any possible asset types that can be referenced.


## What's next?

- [File Header](../002_header)
- [Primitive assets](../primitives/)
- [Aggregate assets](../aggregates/)
