---
title: VEX
parent: Houdini
---

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
vector myArray[] = {{1, 5, 3}, {4.6, 66.6, -63.}};
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
Declaring an array **without** variables:
```
int myArray[] = {1, 2, 55};
vector myArray[] = {{1, 5, 3}, {4.6, 66.6, -63.}};
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
