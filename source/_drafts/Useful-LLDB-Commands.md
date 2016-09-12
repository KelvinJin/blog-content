---
title: Useful LLDB Commands
permalink: useful-lldb-commands
id: 38
updated: '2016-02-12 14:22:22'
tags:
---

- `(lldb) image lookup -rn [SEARCHSTRING]`: Search for a certain string in the memory.
> Note: Be aware that the image lookup LLDB command will list methods that are implemented in memory. When applying this to a particular class, this does not take into account subclassing where other methods inherit from superclasses. That is, the lookup of this command will omit any methods only declared in superclasses provided the subclasses do not override the super method.

```bash
(lldb) rb 'DVTBezelAlertPanel\ initWithIcon:message:'
Breakpoint 1: 2 locations.
(lldb) br l
...
```