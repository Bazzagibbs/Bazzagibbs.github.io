---
title: Coordinates for rendering
weight: 10
description: Transforming coordinates from world space to other spaces.
---

## World coordinates

World coordinates are **right-handed, +Z forward, +Y up, -X right**.

Why?
- Want right handed coords
- glTF default, easy import
- Y-up intuitive for gravity


## Projection and Clip space

Vulkan's NDC space is defined as [x: -1, y: -1] top-left, [x: 1, y: 1] bottom-right, and z depth in the range [0, 1].

Callisto uses a reversed-depth, infinite far plane perspective projection.
- Better distribution of floating point precision
- No far plane clipping artifacts

Because of the reversed depth, shaders must use `Greater` depth test (higher depth is closer).


To get from Callisto coords to Vulkan coords, the x and y axes must be flipped.

```go
perspective :: proc(fovy, aspect, near: f32) -> (mat: Matrix4x4) {
    ep :: math.pow(2, -20)          // ep is a small value to prevent float rounding errors 
    g := 1 / math.tan(0.5 * fovy)
    
    // Column-major matrix
    //
    // -g/s     0       0       0
    //  0      -g       0       0
    //  0       0       ep      near * (1-ep) 
    //  0       0       1       0

    mat[0, 0] = -g / aspect
    mat[1, 1] = -g
    mat[2, 2] = ep
    mat[2, 3] = 1
    mat[3, 2] = near * (1 - ep)


    return mat
}
```
