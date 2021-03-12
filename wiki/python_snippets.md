---
title: Python Snippets
nav_order: 6
parent: Houdini
---

# Python Snippets
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Shelf tools
### Fetch OUT Nodes
Lets the user choose one or more OUT Nulls to be imported into the current subnet via an object merge. The OUT nodes need to be nulls and follow the "OUT_" naming convention.
```python
# Tested in h18
new = []
try:
    parent = hou.node(hou.ui.paneTabOfType(hou.paneTabType.NetworkEditor).pwd().path())
except:
    print 'No Network pane?'

# Dialog selection
def selection():
    obj = hou.ui.selectNode(initial_node=None, node_type_filter=hou.nodeTypeFilter.Obj, title='Select OBJ', multiple_select=False)
    if obj:
        collect(obj)

# Collect Nodes
def collect(obj):
    outs = [c for c in hou.node(obj).children() if 'OUT_' in c.name()]
    names = [n.name() for n in outs]
    key = hou.ui.selectFromList(names, exclusive=False, title='Select nodes to fetch', column_header="OUT Nodes", clear_on_cancel=True)
    for k in key:
        create(outs[k].path(), names[k])

# Create new nodes
def create(path, name):
    parent = hou.node(hou.ui.paneTabOfType(hou.paneTabType.NetworkEditor).pwd().path())
    node = parent.createNode('object_merge', node_name=name.replace('OUT_', 'IN_'))
    node.setColor(hou.Color((0.302,0.525,0.114)))
    node.parm('objpath1').set(path)
    node.parm('xformtype').set(1)
    new.append(node)


def layout():
        parent.layoutChildren(items=(new), horizontal_spacing=-1.0, vertical_spacing=0)

selection()
```
## To Do
- [ ] Abuse Tag for Notes on Nodes
