---
title: Python
nav_order: 5
parent: Houdini
---

# Python
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Syntax Reminder
HDA stuff:
```python
hou.phm().myFunction()  # Call python module functions with a button
hou.pwd()               # Get this Python module's parent (?)
kwargs["node"]          # reference to the current node. Same as hou.node('.') ?
kwargs["node"].hdaModule().myFunction() # Call function in Python Module from other places (such as OnLoaded)
```

Combining *if* and *for* loops to populate a list in one line.
```python
nodes = [c for c in hou.node('/obj').children() if 'myfilter' in c.name()]
```

Replace multiple substrings:
```python
# Dict of replacements:
replacements = {'original' : 'replacewith', '|' : '/'}
# loop over each replacement pair and replace sequentially:
for o, r in replacements.items():
    myString = myString.replace(o, r)
```

## Groups selection dropdown (within HDA)
The group SOP (and many others) have this handy little button which allows to interactively select stuff in the viewport. It uses action script on the string parameter and uses
```python
import soputils
kwargs['geometrytype'] = kwargs['node'].parmTuple('grouptype')
kwargs['inputindex'] = 0
kwargs['ordered'] = kwargs['node'].parm('ordered').eval()
soputils.selectGroupParm(kwargs)
```
Let's say you have an HDA and want to promote a group SOP to the HDA's interface (By dragging it from the 'from nodes' section in the parm interface). It might spit this out:
```python
'NoneType' object has no attribute 'eval'
```
It's looking for the the missing ordered and grouptype controls from the original Group SOP. Can be fixed by either promoting those controls as well, or hardcoding the missing arguments:
```python
kwargs['geometrytype'] = hou.geometryType.Points # Or any other type
kwargs['ordered'] = 1 # 1 or 0
```
Now, if your HDA does not have any inputs, the ```kwargs['inputindex'] = 0``` will cause problems because it's looking for an input. Unfortunately, ```soputils.selectGroupParm(kwargs)```needs this inputindex argument and therefore errors out:
*Not enough inputs connected to select this group.*
This can be fixed by first referencing the actual GroupSOP instead of the HDA:
```python
import soputils;
path = kwargs['node'].path()+'/myGroupSOP' # myGroupSop being the node the selection takes place on
kwargs['node'] = hou.node(path)
kwargs['geometrytype'] = kwargs['node'].parmTuple('grouptype')
kwargs['inputindex'] = 0
kwargs['ordered'] = kwargs['node'].parm('ordered').eval()
soputils.selectGroupParm(kwargs)
```
Yay!

## JSON
Importing data from json. Don't have a good grasp on this yet, but for now:
```python
jsf = hou.pwd().parm('jsonfile').eval() #get json file path from parameter
    with open(jsf) as jsonfile:
        data = json.load(jsf)
        for d in data:
            myList.append(d['jsonDataEntryName'])
            surnames.append(d['surname'])
```

## Viewport & Flipbooks
Came across the issue of wanting to flipbook a sequence of multiple ABC cameras. The Switcher OBJ doesn't work in that case, so here's some python magic to do it as a shelf tool.

Setting a viewport camera:
```python
myCamera = hou.node("pathToMyCamera/cam")
hou.ui.paneTabOfType(hou.paneTabType.SceneViewer).curViewport().setCamera(myCamera)
```

Set up and run flipbooks:
```python
mySceneViewer = hou.ui.paneTabOfType(hou.paneTabType.SceneViewer)         # Get the scene viewer
myFlipbookSettings = mySceneViewer.flipbookSettings().stash()             # make a copy of the flipbook settings
myFlipbookSettings.frameRange((1001, 1200))                               # change flipbook settings
mySceneViewer.flipbook(mySceneViewer.curViewport(), myFlipbookSettings)   # run flipbook
```

## ToolUtils
$HFS/python3.7libs/toolutils.py
