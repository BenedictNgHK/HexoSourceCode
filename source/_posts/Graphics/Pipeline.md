---
title: Graphics Pipeline
author: Benedict Ng
categories:
    - [Graphics]
---
[Reference1](https://www.youtube.com/watch?v=_riranMmtvI)
[Reference2](https://www.youtube.com/watch?v=brDJVEPOeY8)
[Reference3](https://stackoverflow.com/questions/832545/what-are-vertex-and-pixel-shaders)


This is a high level overview about graphics pipeline. Some stages are programmable but the others are fixed functions. All the following stages are processed by GPU and with multithreading, which means that they are executed parallelly.

## Input Assembler (Fixed Function)

In this stage, the GPU reads data of each triangle as input from vertex buffer, and its output includes the coordinate, UV, Normal, and ID of each vertex.

## Vertex Shader (Programmable with glsl)

Vertex Shader performs some transformations such as rotation of each vertex passed from Input Assembler

## Tesselation

This stage includes hull shader (fixed function), tesselator (programmable) and domain shader (fixed function). In short, it converts a low poly model to high poly model by introducing more triangles to increase the quality of output image.

## Geometry Shader (Fixed Function)

Takes each transformed primitive (triangle, etc) and can perform calculations on it. This can add new points, take them away or move them as required. This can be used to add or remove levels of detail dynamically from a single base mesh, create mathematical meshes based on a point (for complex particle systems) and other similar tasks

## Rasterization (Fixed Function)

It breaks geometry into fragments for each pixel the triangle overlaps. Its output includes the coordinate of each pixel and the interpolated parameter value.

## Fragment Shader (Programmable with glsl)

It is also referred as pixel shader. It processes each fragment and outputs values such as color

## Color Blending (Fixed Function)

It mixes different fragments corresponding to the same pixel.

## Frame Buffer

The output image

## PS: CPU VS GPU

A graphic card contains GPU and some other outer devices in the motherboard. In contrast with CPU, the GPU has much more physical cores. And each physical core (for GPU and CPU) in a specific time point,  only executes one thread but since it stores context of other threads so this core appears to run different threads parallelly but this "parallelism" is not real, it should be "concurrency" actually. The cores of GPU excels at sequential execution but fails at something like branches, loops, or recursion because they invoke jump instructions. The cache and memory in the motherboard of graphic card handled by GPU are significantly smaller than CPU.
