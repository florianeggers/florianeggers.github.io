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
# I usually use OBJ_fetch for the icon
new = []
try:
    parent = hou.node(hou.ui.paneTabOfType(hou.paneTabType.NetworkEditor).pwd().path())
except:
    print ('No Network pane?')

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

### Versioned Caching Nodes
Adds linked  ROP OUT and file nodes after the selected node. The ROP Geo node has versioning and commenting built in. Comments are stored in tags on the node (and thus hip file) itself, so not much use in a studio environment.
```python
# I usually use SOP_filecache for the icon

anchor = hou.selectedNodes()[0]

# Ask for opname + ask for ROP or SOP
gui = hou.ui.readInput("Name of cache. CACHE_ prefix will be added", buttons=("SOP context", "OUT context", "Cancel", ), title="Create cache nodes", close_choice=2)
ropContext = gui[0]
ropName = gui[1].replace(" ", "_")

# Create rop
cacheOUT = anchor.parent().createNode("rop_geometry", node_name="CACHE_" + ropName)

# create input
cacheIN = anchor.parent().createNode("file", node_name="CACHE_IN_" + ropName)    

# Connect rop
cacheOUT.setInput(0, anchor, 0)

# Color
color = hou.Color((0.322, 0.259, 0.58))
cacheOUT.setColor(color)
cacheIN.setColor(color)

# Align
cacheOUT.setPosition([anchor.position()[0], anchor.position()[1] - 1])
cacheIN.setPosition([anchor.position()[0], anchor.position()[1] - 2])

# Versioning-----------------------------------------------
# create note tagss
noteDict = {}
for i in range(99):
    noteDict["version_note_" + str(i)] =  ""

# create new parms
parms = cacheOUT.parmTemplateGroup()

parmVersion = hou.IntParmTemplate("version_v", "Version", 1, default_value=(1, 1, 1), min=1, min_is_strict=True, join_with_next=True, script_callback_language=hou.scriptLanguage.Python)
parmVersionUp = hou.ButtonParmTemplate("version_up", "+", script_callback_language=hou.scriptLanguage.Python)
parmNote = hou.StringParmTemplate("version_note", "Note", 1, join_with_next=True, tags=noteDict, script_callback_language=hou.scriptLanguage.Python)

# TODO set callbacks

newParms = [parmVersion, parmVersionUp, parmNote]

folder = hou.FolderParmTemplate("folder_version", "Version Control", newParms, folder_type=hou.folderType.Simple)

# apply parms
parms.insertBefore((0,), folder)
cacheOUT.setParmTemplateGroup(parms)

# set out
cacheOUT.parm("sopoutput").set('$CACHE/projects/mountainimpact_03/geo/`opname("..")`/$OS/v`padzero(3, ch("./version_v"))`/$OS.$F.bgeo.sc')

# set in
cacheIN.parm("file").set('`chs("../' + cacheOUT.name() + '/sopoutput")')
cacheIN.parm("missingframe").set(1)
```


## To Do
- [x] Abuse Tag for Notes on Nodes
