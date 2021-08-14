---
title: VEX
nav_order: 3
parent: Houdini
---

# VEX
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Cheat Sheet
```
-------------------- BASICS ----------------------------------
vector2 = chu('Vector2') = u@vector;
vector4 = chp('Vector4') = p@vector;
matrix3 = ch3('Matrix3') = 3@matrix;
matrix4 = ch4('Matrix4') = 4@matrix;


-------------------- ARRAYS ----------------------------------
int myArray[] = {1, 2, 55};                                       // Array without variables
vector myArray[] = set({1, 5, 3}, {4.6, 66.6, -63.});

int myArray[] = array(myVar, @ptnum);                             // With variables, use array()
vector myArray[] = array(jeff, v@P, {1, 55, 3}});

f[]@myArray = array(f@pscale, 22, -.3);                           // Attribute arrays


-------------------- RANDOM ----------------------------------
float random = rand(float seed);                                  // "rand" uses float seed
float random = random(int seed);                                  // "random" uses integer seed


-------------------- SAMPLE VECTORS ---------------------------
vector2 disk = sample_circle_uniform(rand(vector2));              // Sample Vector2, length < 1
vector2 diskEdge = sample_circle_edge_uniform(rand(float));       // Sample unit Vector2

vector3 sphere = sample_sphere_uniform(rand(vector));             // Sample Vector3, length < 1
vector3 sphereSurface = sample_direction_uniform(rand(vector2));  // Sample unit Vector3

vector4 hypersphere = sample_hypersphere_uniform(rand(vector4));  // Sample Vector4, length < 1
vector4 orient = sample_orientation_uniform(rand(vector));        // Sample unit Vector4 (Hypersphere)

-------------------- PAD ZERO ---------------------------
s@attrib = sprintf('%04d',@MyAttrib);
```
