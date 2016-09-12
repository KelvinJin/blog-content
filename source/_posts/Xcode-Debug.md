---
title: Xcode Debug
permalink: xcode-debug
id: 4
updated: '2015-01-27 18:51:58'
date: 2015-01-24 18:26:39
tags:
---

From today, I'll note down some of the debugging technologies that are used when I develop iOS applications.

First, a very good [article](http://eli.thegreenplace.net/2011/01/23/how-debuggers-work-part-1.html) that explains the basic knowledge of how the debugger actually works. The most insteresting command in this article is `ptrace`, a function that allows the parent process to communicate with the child process.

For breakpoints, the magic happens when we use the `int 3` (`0xcc`) instruction. Since 1 byte is the shortest instruction in `x86` architecture, we can guarantee that the target program will stop right on where we break it.
> To set a breakpoint at some target address in the traced process, the debugger does the following:  
1. Remember the data stored at the target address  
2. Replace the first byte at the target address with the `int 3` instruction

After the CPU stops the target process:
> 1. Replace the `int 3` instruction at the target address with the original instruction  
2. Roll the instruction pointer of the traced process back by one. This is needed because the instruction pointer now points after the `int 3`, having already executed it.  
2. Allow the user to interact with the process in some way, since the process is still halted at the desired target address. This is the part where your debugger lets you peek at variable values, the call stack and so on.  
4. When the user wants to keep running, the debugger will take care of placing the breakpoint back (since it was removed in step 1) at the target address, unless the user asked to cancel the breakpoint.

Moreover, if you want to print out some debug information when you encounter some issues, normally you would consider to write `NSLog` or `println` directly into the code. However, a more convenient way is to add a breakpoint and associate some actions with it.