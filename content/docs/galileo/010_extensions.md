---
title: Extensions
linkTitle: Extensions
weight: 1000
type: docs
simple_list: true
description: Provides application-specific data related to an asset.
---

### Extension Info

```
+-----------------------------------------------------------------------------------------------+
|00 01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31|
+-----------------------------------------------+-----------+-----------------------+-----------+
|                Extension name                 |  Ext ver. |    Data begin index   | Next ext. |
+-----------------------------------------------+-----------+-----------------------+-----------+
```

```c
struct Extension_Info {
    uint8_t[16] extension_name;         // Ascii string, null terminated or max length 16.
    uint32_t    extension_version;
    uint64_t    data_begin_index;       // Index into asset's main data buffer.
    uint32_t    next_extension;         // (index + 1) into extension_info array. Zero indicates no extensions.
}
```

Client implementations of a given asset type **MAY** ignore extension info and data. 
Any behaviour that cannot be safely ignored **MUST** have its own asset type.

