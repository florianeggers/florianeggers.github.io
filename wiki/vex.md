---
title: VEX
nav_order: 3
---

# VEX
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Channels and Attribs
```
vector2 = chu('Vector2') = u@vector;
vector4 = chp('Vector4') = p@vector;
matrix3 = ch3('Matrix3') = 3@matrix;
matrix4 = ch4('Matrix4') = 4@matrix;
```

## Arrays
Declaring an array **without** variables:
```
int myArray[] = {1, 2, 55};
vector myArray[] = set({1, 5, 3}, {4.6, 66.6, -63.});
```
When declaring an array **with** variables, need to use ```array()```
```
int myArray[] = array(myVar, @ptnum);
vector myArray[] = array(jeff, v@P, {1, 55, 3}});
```
Attribute arrays
```
f[]@myArray = array(f@pscale, 22, -.3);
```

## Random
Using ```rand()``` takes a float seed, ```random()``` only looks at the integer part of the seed:
```
float random = rand(float seed);
float random = random(int seed);
```

## Vector Sampling
Samples Vectors from a seed. First entry of each pair is non-normalized (useful for positions, etc.), second entry is a unit vector (useful for directions, etc.):
```
vector2 disk = sample_circle_uniform(rand(vector2));              // Sample Vector2, length < 1
vector2 diskEdge = sample_circle_edge_uniform(rand(float));       // Sample unit Vector2

vector3 sphere = sample_sphere_uniform(rand(vector));             // Sample Vector3, length < 1
vector3 sphereSurface = sample_direction_uniform(rand(vector2));  // Sample unit Vector3

vector4 hypersphere = sample_hypersphere_uniform(rand(vector4));  // Sample Vector4, length < 1
vector4 orient = sample_orientation_uniform(rand(vector));        // Sample unit Vector4 (Hypersphere)
```
