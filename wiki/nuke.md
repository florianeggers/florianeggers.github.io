---
title: Nuke
nav_order: 3
---

# Nuke
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Expression reminder
```
frame         = Current frame
input.width   = Image width of first Input
MyNode.color  = Reference to the color field of "MyNode"
root.name     = Filename
date %x       = Date dd/mm/yy
```
When in a text field, use ```[value myExpression]``` format.
For custom Gizmos, use ```parent.knobName``` to reference top level user knobs.
So, for instance, displaying a gizmo level input (called *burnin_comment* in this example) in a text node:
```
comment: [value parent.burnin_comment]
```

## Random tricks
### Sharpening
When sharpening something, use *lin2log* -> *sharpen* -> *log2lin*. Helps with artefacts.

### Blurring Edges
todo
