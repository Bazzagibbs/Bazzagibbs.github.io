---
title: Assets
weight: 10
description: Game data stored on the disk.
---

Assets are classified as either Primitive or Aggregate. 
Primitive assets **MUST** contain only information about themselves, while aggregate assets **MAY** contain references to other assets.

Planned primitives: 
- `Image`
- `Mesh`
- `Audio`
- `Input action map`
- `Shader`

Planned aggregates:
- `Material` (shader + images)
- `Model` (mesh + materials)
- `Construct` (fixed-length transform hierarchy)
- `Level`


## File format

This section is a work in progress. I may end up implementing a custom format that requires very little processing to deserialize.

In the meantime, I'm using [Concise Binary Object Representation (CBOR)](https://cbor.io/) for serialization, 
as it provides some desired attributes to the asset files:
- Named fields to avoid data loss when struct definitions change
- Small file size
- Existing Odin core library


## File loading

Inspiration from Timothy Cain's video, [Arcanum "dat" files](https://youtu.be/VYw4ln0jxUY), development builds use a raw OS file system, or "loose assets", 
while release builds use packed bundles.
- data locality on the disk
- "seek" is much faster than "open"
- avoid file descriptor limits
- less overhead for many tiny assets (block alignment, OS file nodes)
- easy patches/modding with sequential overriding of the asset database
- more efficient compression
