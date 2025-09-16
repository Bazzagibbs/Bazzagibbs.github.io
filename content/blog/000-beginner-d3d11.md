---
date: 2024-12-12
title: Beginner Direct3D11 examples in Odin-lang
description: A series of example programs to introduce concepts of Win32 and Direct3D11 development.
---

Learning material for DirectX in general can be quite hard to find, and when it does exist, it usually
makes heavy use of Microsoft's COM API and smart pointers. Now you have to learn two things instead
of what you really care about. This is a problem for people wanting to use D3D11 from other systems 
programming languages that don't have the same constructs as C++, in this case the Odin programming 
language.

An exception is the excellent resource by [Kevin Moran - Beginner Direct3D11](https://github.com/kevinmoran/BeginnerDirect3D11).
It uses a very simple C-like style of C++ with all the relevant code contained in one or two files per
lesson. I decided it would be useful to make a translation using Odin.

## [Github Repository](https://github.com/Bazzagibbs/odin-beginner-direct3d11)

Click the headings below to open the source code for an example.

To run an example program on your own machine, simply clone the repository and call `odin run <example>`.


#### [0. Opening a Win32 Window](https://github.com/Bazzagibbs/odin-beginner-direct3d11/tree/master/00.%20Opening%20a%20Win32%20Window)

This is a barebones Windows application that displays a window to the user. It receives messages through the `wnd_proc` event handler procedure, and closes when the "X" button.

#### [1. Initializing Direct3D](https://github.com/Bazzagibbs/odin-beginner-direct3d11/tree/master/01.%20Initialising%20Direct3D%2011)

Building upon the previous example, a D3D11 device, context and swapchain is created for the window. The framebuffer is cleared to a specified color to prove it is working.

#### [2. Drawing a Triangle](https://github.com/Bazzagibbs/odin-beginner-direct3d11/tree/master/02.%20Drawing%20a%20Triangle)
![](/blog/beginner-d3d11/02.png)

A vertex buffer, vertex shader and pixel shader are created. Each frame the shaders and buffer are bound before calling `Draw()`.

#### [3. Drawing a Textured Quad](https://github.com/Bazzagibbs/odin-beginner-direct3d11/tree/master/03.%20Drawing%20a%20Textured%20Quad)
![](/blog/beginner-d3d11/03.png)

Each vertex now has texture coordinate data instead of vertex colors. Two triangles are drawn to create a quad, and the pixel shader samples a texture using the provided
texture coordinates.

#### [4. Using a Constant Buffer](https://github.com/Bazzagibbs/odin-beginner-direct3d11/tree/master/04.%20Using%20a%20Constant%20Buffer)
![](/blog/beginner-d3d11/04.gif)

In addition to vertex data, a constant buffer provides data to all invocations of a vertex shader or pixel shader in a draw call.
Here one is used to provide an offset and a single color to all vertices of the triangle.

#### [5. Using a High Precision Timer](https://github.com/Bazzagibbs/odin-beginner-direct3d11/tree/master/05.%20Using%20a%20High%20Precision%20Timer)

Odin's `core:time` package is used to keep track of time instead of an `f32` accumulator.

#### [6. Creating a Depth Buffer](https://github.com/Bazzagibbs/odin-beginner-direct3d11/tree/master/06.%20Creating%20a%20Depth%20Buffer)
![](/blog/beginner-d3d11/06.png)

A depth buffer is used to ensure pixels further away than what has already been drawn do not overwrite the existing color.

#### [7. Drawing a Transparent Textured Quad](https://github.com/Bazzagibbs/odin-beginner-direct3d11/tree/master/07.%20Drawing%20a%20Transparent%20Textured%20Quad)
![](/blog/beginner-d3d11/07.png)

Transparent objects are handled after opaque objects have been drawn, and they require a different blend state to make sure colors are mixed correctly

#### [8. Keyboard Input](https://github.com/Bazzagibbs/odin-beginner-direct3d11/tree/master/08.%20Keyboard%20Input)
![](/blog/beginner-d3d11/08.gif)

Keyboard input is gathered using the window's event handler proc and used to move an object around.

#### [9. 3D Camera](https://github.com/Bazzagibbs/odin-beginner-direct3d11/tree/master/09.%203D%20Camera)
![](/blog/beginner-d3d11/09.gif)

A Model matrix moves the vertices from model->world space, a View matrix moves them from world->view (camera) space, and a Projection matrix moves them into clip space.
This example uses Odin's built-in matrix types and some procs from `core:math/linalg`.

#### [10. Drawing a Cube](https://github.com/Bazzagibbs/odin-beginner-direct3d11/tree/master/10.%20Drawing%20a%20Cube)
![](/blog/beginner-d3d11/10.png)

A cube created from twelve triangles replaces the quads from previous examples.

#### [11. Loading Wavefront .obj](https://github.com/Bazzagibbs/odin-beginner-direct3d11/tree/master/11.%20Loading%20a%20Wavefront%20.obj%20Mesh)
![](/blog/beginner-d3d11/11.png)

Rather than hardcoding vertices into the program, they can be loaded from a mesh file. For simplicity a subset of the Wavefront .OBJ format is used in this example.

Model by [Kenney.nl under the CC0 license](https://kenney.nl/assets/hexagon-kit).

#### [12. Blinn-Phong Shading](https://github.com/Bazzagibbs/odin-beginner-direct3d11/tree/master/12.%20Blinn-Phong%20Shading)
![](/blog/beginner-d3d11/12.png)

The pixel shader performs some simple lighting using a light's direction, the camera's direction and the surface's normal.

#### [Extra 1. High DPI Display Support](https://github.com/Bazzagibbs/odin-beginner-direct3d11/tree/master/98.%20High%20DPI%20Display%20Support)

By default, Windows expects programs to be unaware of high-DPI displays for backwards compatibility as they did not exist when the Win32 API was first written, 
and will scale the window by some amount on these screens to maintain a consistent UI size in legacy apps at the cost of a blurry image.
This example tells Windows that we are aware of such displays and can handle the scaling ourselves. By doing so, Windows will give us the correct
pixel resolution for the window and thus a sharp output.

#### [Extra 2. Removing Global State](https://github.com/Bazzagibbs/odin-beginner-direct3d11/tree/master/99.%20Removing%20Global%20State)

Many applications will benefit from having application state not being stored in global variables, for example when using DLLs for hot-reloading
gameplay code. Here the application state struct is created in `main()` and passed to the window proc as userdata.
