---
title: Header
linkTitle: Header
weight: 2
type: docs
simple_list: true
description: Metadata that is common to all asset types, and may be useful to know before parsing the entire file contents.
---
```
+-----------------------------------------------------------------------------------------------+
|00 01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31|
+-----------+-----------+-----------------------------------------------+-----------+-----------+
|   Magic   | Spec ver. |             Asset UUID version 4              | Type enum | Checksum...
+-----------+-----------+-----------------------------------------------+-----------+-----------
            |
 -----------+
```

```c
struct File_Header {
    uint8_t[4]      magic;                      // String "GALI"
    uint32_t        specification_version;      // Using semantic version - uint8_t major, uint8_t minor, uint16_t patch 
    uint128_t       asset_uuid;                 // UUID version 4
    uint32_t        asset_type;                 // Asset_Type enum backed by a uint32_t
    uint64_t        asset_checksum;             // xxhash XXH3 of the file contents, EXCLUDING the file header.
};
//  36 bytes total
```

### See also

- [UUID specification](https://datatracker.ietf.org/doc/html/rfc4122#section-4.4)
- [Cyan4973/xxhash](https://github.com/Cyan4973/xxHash)

## What's next?

- [Primitive assets](../primitives/)
- [Aggregate assets](../aggregates/)
