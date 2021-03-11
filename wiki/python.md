---
title: Python
nav_order: 4
---
1. TOC
{:toc}

## Syntax Reminder
HDA stuff:
```Python
hou.phm().myFunction()  # Call python module functions with a button
hou.pwd()               # Get this Python module's parent (?)
kwargs["node"]          # reference to the current node. Same as hou.node('.') ?
kwargs["node"].hdaModule().myFunction() # Call function in Python Module from other places (such as OnLoaded)
```

Combining if an for loops to populate a list in one line.
```Python
nodes = [c for c in hou.node('/obj').children() if 'myfilter' in c.name()]
```

Replace multiple substrings:
```Python
# Dict of replacements:
replacements = {'original' : 'replacewith', '|' : '/'}
# loop over each replacement pair and replace sequentially:
for o, r in replacements.items():
    myString = myString.replace(o, r)
```

## Groups selection dropdown (within HDA)
The group SOP (and many others) have this handy little button which allows to interactively select stuff in the viewport. It uses action script on the string parameter and uses
```Python
import soputils
kwargs['geometrytype'] = kwargs['node'].parmTuple('grouptype')
kwargs['inputindex'] = 0
kwargs['ordered'] = kwargs['node'].parm('ordered').eval()
soputils.selectGroupParm(kwargs)
```
Let's say you have an HDA and want to promote a group SOP to the HDA's interface (By dragging it from the 'from nodes' section in the parm interface). It might spit this out:
```Python
'NoneType' object has no attribute 'eval'
```
It's looking for the the missing ordered and grouptype controls from the original Group SOP. Can be fixed by either promoting those controls as well, or hardcoding the missing arguments:
```Python
kwargs['geometrytype'] = hou.geometryType.Points # Or any other type
kwargs['ordered'] = 1 # 1 or 0
```
Now, if your HDA does not have any inputs, the ```kwargs['inputindex'] = 0``` will cause problems because it's looking for an input. Unfortunately, ```soputils.selectGroupParm(kwargs)```needs this inputindex argument and therefore errors out:
*Not enough inputs connected to select this group.*
This can be fixed by first referencing the actual GroupSOP instead of the HDA:
```Python
import soputils;
path = kwargs['node'].path()+'/myGroupSOP' # myGroupSop being the node the selection takes place
kwargs['node'] = hou.node(path)
kwargs['geometrytype'] = kwargs['node'].parmTuple('grouptype')
kwargs['inputindex'] = 0
kwargs['ordered'] = kwargs['node'].parm('ordered').eval()
soputils.selectGroupParm(kwargs)
```
Yay!

## JSON
Importing data from json. Don't have a good grasp on this yet, but for now:
```Python
jsf = hou.pwd().parm('jsonfile').eval() #get json file path from parameter
    with open(jsf) as jsonfile:
        data = json.load(jsf)
        for d in data:
            myList.append(d['jsonDataEntryName'])
            surnames.append(d['surname'])
```
