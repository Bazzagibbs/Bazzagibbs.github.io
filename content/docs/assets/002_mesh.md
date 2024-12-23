---
title: Mesh
description: A primitive asset for storing vertex data.
weight: 2
draft: true
---

## Mesh overview

Mesh assets store an object's vertex data, and how the vertices are connected to form triangles.

This page is currently being rewritten to support the new asset format.

<!--
## Mesh body
```
+------------------------+-------------------------+--------------------------+
|        40 bytes        |       n * 64 bytes      |       m * 32 bytes       |
|      Mesh manifest     | Vertex group info array |   Extension info array   |
+------------------------+-------------------------+--------------------------+
| Buffer
+-----------------------------------------------------------------------------
                                      ...
 -----------------------------------------------------------------------------+
                                                                              |
 -----------------------------------------------------------------------------+
```

```c
struct Asset_Body_Mesh {
    Mesh_Manifest           manifest;               // 40 bytes
    Vertex_Group_Info       []vertex_group_infos;   // Count provided in manifest. 55 bytes each.
    Extension_Info          []extension_infos;      // Count provided in manifest. 32 bytes each.
    uint8_t                 []buffer;               // Size provided in manifest.
};
```

A `mesh` asset body **MUST** contain a 40 byte [`Mesh_Manifest`](#manifest), followed by a variable length 
array of 55 byte [`Vertex_Group_Info`](#vertex-group-info) structs. `Mesh` assets **MAY** include a variable 
length array of 32 byte [`Extension_Info`](../../extensions) structs.

The count of `Vertex_Group_Info` structs and `Extension_Info` structs **MUST** be provided in the manifest.

### Manifest
```
+-----------------------------------------------------------------------------------------------+
|00 01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31|
+-----------+-----------+-----------+-----------+-----------+-----------+-----------+-----------+
|                              Bounding Box                             | VG count  | Ext count |
+-----------+-----------+-----------+-----------+-----------+-----------+-----------+-----------+
|      Buffer size      | Next ext. |
+-----------------------+-----------+
```

```c
struct Mesh_Manifest {
    float32_t[6]    bounding_box;           // center.xyz, extents.xyz. Extents are half of box dimensions.
    uint32_t        vertex_group_count;
    uint32_t        extension_count;
    uint64_t        buffer_size;

    uint32_t        next_mesh_extension;    // (index + 1) into extension info array. Zero indicates no extensions.
};
```

A `mesh` **MUST** contain one or more `vertex groups`. 
Each `vertex group` **SHOULD** correspond to one draw call in the renderer, and **MAY** be used to draw different parts of a `mesh` with a different material or shader.

A `vertex group` **MUST** contain sub-buffer offsets for each `vertex attribute`.

### Vertex Group Info

```
+-----------------------------------------------------------------------------------------------+
|00 01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31|
+-----------+-----------+-----------+-----------+-----------+-----------+-----------+-----------+
|                              Bounding Box                             |  Buf slice begin idx  |
+-----------+-----------+-----------+-----------+-----------+-----------+--+--+--+--+-----------+
|    Buf slice size     |       Idx count       |      Vert count       |TC|CL|JW|EX| Next ext. |
+-----------+-----------+-----------+-----------+-----------+-----------+--+--+--+--+-----------+
```

```c
struct Vertex_Group_Info {
    float32_t[6]    bounding_box;               // center.xyz, extents.xyz. Extents are half of box dimensions.
    uint64_t        buffer_slice_begin_index;
    uint64_t        buffer_slice_size;
    
    uint32_t        index_count;
    uint32_t        vertex_count;

    uint8_t         texcoord_channel_count;
    uint8_t         color_channel_count;
    uint8_t         joint_weight_channel_count;
    uint8_t         extension_attribute_channel_count;
    
    uint32_t        next_attribute_extension;   // (index + 1) into extension_info array. Zero indicates no extensions.
};
```

A `vertex group` **MUST** have an index buffer. Its count **MUST** be a multiple of 3.


A `vertex group` **MUST** have the following attributes:
- `float32_t[3]     position`
- `float32_t[3]     normal`
- `float32_t[4]     tangent`

A `vertex group` **MAY** have zero or more channels of the following attributes:
- `uint16_t[2]      texcoord` (in unorm format)
- `uint8_t[4]       color`    (in RGBA format)
- `uint16_t[4]      joints`
- `uint16_t[4]      weights`
- application-specific extension attributes

The number of `joint` and `weight` channels for a `vertex group` **MUST** be equal.

### Buffer

All `vertex attribute` data for a given `vertex group` **MUST** be contiguous and de-interleaved.

For any attributes with multiple channels, all channels of the same attribute type **MUST** be contiguous and de-interleaved.

```
+==== Vert group 0 ====+
|  Index list          |
+----------------------+
|  Position            |
+----------------------+
|  Normal              |
+----------------------+
|  Tangent             |
+----------------------+
|  ? Texcoord[0]       |
+----------------------+
|  ? Texcoord[1]       |
+----------------------+
|  ? Color[0]          |
+----------------------+
|  ? Color[1]          |
+----------------------+
|  ? Joints[0]         |
+----------------------+
|  ? Joints[1]         |
+----------------------+
|  ? Weights[0]        |
+----------------------+
|  ? Weights[1]        |
+----------------------+
|  ? Ext attributes[0] |
+==== Vert group 1=====+
|  Index list          |

           ...         
|                      |
+======================+
|  ? Extension data[0] |
+----------------------+
```

### Reference implementation

A reference implementation can be found in the [Callisto repository](https://github.com/Bazzagibbs/callisto/blob/master/asset/mesh.odin).
-->

