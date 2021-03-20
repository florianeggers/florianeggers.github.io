---
title: Misc
nav_order: 7
parent: Houdini
---

# Misc
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Expressions
```python
opinputpath("../.", 0)    // Node inputs
centroid(0, D_X)          // centroid
bbox(0, type)             // bbox size

types: D_XMIN, D_YMIN, D_ZMIN, D_XMAX, D_YMAX, D_ZMAX, D_XSIZE, D_YSIZE, or D_ZSIZE
```

## Fake cluster setup
In case wedging or PDG is not supported on the farm, here's a dirty fake clustering setup:
In this example, control all cluster parms through a node called **CTRL** with a parm called **cluster**.
Then, use ROP Geometry nodes named **myName_0**, **myName_1**, etc. using this Pre-Render Script:
```
opparm ../CTRL cluster `opdigits(".")
```
Now you can submitt all those ROPs and the prerender script will set **cluster** on **CTRL** according to the suffix of each node name.
