---
layout: post
title:  "The Ultimate Guide to the GNU Debugger"
date:   2024-10-21 18:00:00 -0400
categories: Tech Rambles
---


 
GDB (GNU Debugger) is an indispensable tool for developers working on software written in C or C++. It has numerous features, including but not limited to:
- Printing the values of variables in your program
- Inspecting the contents of memory
- Investigating different threads of execution within your program
- Investigating different frames within the current thread of execution
- Setting breakpoints
 
Newer versions even have fancier features such as time travel debugging.
 
But as a new user of GDB, these features can be difficult to discover. The user interface doesn't exactly make it obvious what you can or cannot do, and it can be daunting for new users to navigate the debugger. This article aims to both help new users discover commands they may be unaware of while at the same time serving as a reference for experienced users of GDB.
 
 
## Act I: The Basics
 
### Attaching GDB
GDB can be attached to a process in a number of different ways.
 
First, we can start the process with GDB. To do so,
 
```
gdb my_program
```
where `my_program` is the name of your executable. Note: this will attach GDB to the process, but it won't run it immediately. For that, you need to use the `run` command to start running the program. Example output:
 
```
% gdb my_program
GNU gdb (GDB) 15.1
Copyright (C) 2024 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-pc-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.
 
For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from my_program...
(gdb) run
Starting program: /tmp/my_program
```
 
Alternatively, if the process that you want to debug is already running, you can attach GDB with the following command:
 
```
gdb --pid <process_id>
```
 
where `<process-id>` is the process ID of your program. I like to use bash to figure that out for me, with
 
```
gdb --pid $(pgrep my_program)
```
 
 
### Setting Breakpoints
Set breakpoints with
 
```
break <filename>:<line_number>
```
 
or, alternatively
 
```
break <filename>:<function_name>
```
 
To see the currently set breakpoints, run
 
```
info breakpoints
```
 
or simply `i b` for short. In GDB, many of the commonly used commands have one-character abbreviations.
 
To disable a breakpoint, run
```
disable <breakpoint-num>
```
where `<breakpoint-num>` is the number displayed in `info breakpoints` associated with that breakpoint.
 
To re-enable a disabled breakpoint, run
```
enable <breakpoint-num>
```
 
 
#### Conditional Breakpoints
Conditional breakpoints can be set on any variables which are currently in scope. For example, if I want to break whenever the `result` variable is nonzero, I can do so by entering the following command in GDB
 
```
b <filename>:<line_number> if result != 0
```
 
 
### Step, Next, and Continue
After hitting a breakpoint, you can continue the execution of the program in a number of different ways.
 
- If you just want to execute the next line of code, you use the `next` command, or `n`.
- If you want to step into a function and execute the first line of code within that function, use the `step` or `s` command.
- If you want to continue to program execution until the next breakpoint is hit, use the `continue` or `c` command.
 
 
### Inspecting the values of variables
Once program execution is stopped at a particular location such as a breakpoint, you can inspect local variables. To know which variables you have access to, you can run
 
```
info locals
```
 
To print the value of a particular variable, run
```
print my_var
```
 
Alternatively, `print` can be abbreviated `p`. Example:
 
```
p my_var
```
 
 
## Act II: Intermediate GDB
 
### Passing arguments to GDB
Oftentimes, when launching a program with GDB, we want to pass command line arguments to that program. To do so, we can use GDB with the `--args` argument:
 
```
gdb --args my_program --my_flag=1
```
 
In this case, GDB will forward the arguments it is given whenever we start running the program.
 
 
### Call Stacks
To view the call stack of the current thread of execution, use the
 
```
backtrace
```
 
command. Alternatively, use the shorthand `bt`.
 
To jump to a particular frame within the backtrace, you can use
 
```
frame <frame_number>
```
 
where `<frame_number>` is the number corresponding to the frame displayed in the backtrace.
 
 
### Debugging Multiple Threads
By default, when executing the `step` or `next` command, all threads are executed until one of them either hits a breakpoint or the execution of the `step` or `next` command on the current thread is completed. Sometimes, this is not desirable and we want to debug one thread at a time. To force other threads to not advance, use the following command:
 
```
set scheduler-locking step
```
 
More info on this particular command and its implications can be found at [GDB Thread Stops Documentation](https://sourceware.org/gdb/current/onlinedocs/gdb#Thread-Stops).
 
To print all current threads, use the
 
```
info threads
```
command. Switching to a particular thread of execution can be accomplished with
 
```
thread <thread_num>
```
 
 
### Debugging a Core File
On Unix-like systems, when a program crashes, it may generate a core file. This file contains the process memory at the time of the crash and can be inspected to gain clues on why the crash occurred. Use the following command to debug a core file with gdb:
 
```
gdb path/to/the/binary path/to/the/core/dump/file
```
After doing so, you will be able to examine the values of variables, inspect threads, backtraces, and program memory as usual, but you will not be able to execute the program.
 
 
## Act III: Advanced Topics
 
### Inspecting Contents of Memory
Earlier, we discussed how to print the values of variables within the current scope using the `print` command. However, when programming in C, there are many cases were we want to print out a contiguous chunk of memory. In these cases, you can use the `x` command to examine memory.
 
For example, to examine 38 bytes formatted as hex located starting as the memory address 0x80008000 is
```
x /38xb 0x80008000
```
There are a few things to unpack here, so we'll break it down piece by piece.
- `x` is the examine command.
- `/` allows us to specify the number of bytes and formatting, which follows.
- 38 specifies the quantity of units to output (in this case, bytes).
- `x` specifies the output format of hex. Another option here is `t` for binary.
- `b` specifies the unit size. Alternatives are `w` for words (four bytes) and `g` for giant words (eight bytes).
- `0x80008000` is the memory address that we are examining. You can also pass expressions here, as long as the expression evaluates to a valid memory address.
 
Phew! That is a lot, but this command is a must when working with custom binary storage formats.
 
 
### Catch `exit` Syscall
 
The following can be used to catch a process just before it is exiting, which can be useful for figuring out why it’s exiting.
 
```
catch syscall 60
catch syscall 231
```
Note: These syscalls are OS-specific.
 
 
### Debugging Calls to `fork`
Sometimes, a process that you are debugging will call the `fork` command. This command launches a new process. By default, GDB will continue debugging the parent process, but you may want to debug the child process instead. To do so, you can execute
 
```
set follow-fork-mode child
```
 
To reset the default behavior, use
 
```
set follow-fork-mode parent
```
 
And finally, to show the current `follow-fork-mode`, use
 
```
show follow-fork-mode
```
 
### Signals
On Unix systems, a [signal](https://man7.org/linux/man-pages/man7/signal.7.html) is a message defined by the operating system which can be sent or raised by processes. By default, GDB halts at some signals (like SIGSEGV, the signal raised whenever the process attempts to access out-of-bounds memory) and does nothing for some other signals. The command
 
```
info signals
```
will show which signals exist and what behavior GDB will take when these signals are encountered.
 
Sometimes, it may be useful to disable certain signals so that your debugging process is not interrupted by them. To disable a particular signal from causing gdb to halt, for example `SIGSTOP`, run:
 
```
handle SIGSTOP noprint
```
 
### Max Output Length
By default, GDB imposes a maximum length for strings which are printed out. The default is 200 characters. If you want to print anything longer, you can run
 
```
set print elements <length>
```
 
Alternatively, if you want to disable this limit entirely, run
 
```
set print elements 0
```
 
**Warning:** Not setting a hard limit can cause instability
 
 
### Debugging A Sandboxed Process
Some processes you may want to debug may be `sandboxed`. In other words, they may be running in a different process, network, IPC, mount, or user namespace. GDB can be run within the sandbox with the following commands:
 
```
sudo nsenter -t $(pgrep my_program) -m -u -i -n -p
gdb --pid $(pgrep my_program) # Note: this PID may be different than the previous one
```
 
### Setting Hardware Watchpoints
In some cases, you don't want to set a breakpoint at a particular location. Instead, you want the process execution to stop when a particular value changes. This can be accomplished with hardware watchpoints in GDB. To watch the value of a variable for changes, execute
 
```
watch my_var
```
 
 
## Miscellaneous Features and Commands
 
### Window Layouts
The default GDB window layout works well under most circumstances, but there are some alternatives which I find useful:
 
* `layout split`
* `layout reg` - display hardware registers and values
* `layout asm` - show current assembly instructions
* `layout src` - show source code (if available)
 
To switch focus between panes, use `<Ctrl> + x, o`.
 
 
### Pretty Printing
By default, GDB packs as much information it can into the available space. When examining a complicated struct, this can sometimes be hard to look at. Thankfully, you can enable pretty-printing with
```
set print pretty on
```
which gratuitously spaces out this information.
 
 
### Printing Types
Sometimes, you may not know what the type of a variable you are inspecting is. Printing a variable reveals it's type, but if the type definition is not known to you, it may not help you much. With GDB, you can execute
 
```
ptype <type_name>
```
to get the type information. For example:
 
```
(gdb) ptype uint64_t
type = unsigned long
```
 
 
## How to Discover Additional Features
There are so many additional features that I don't know about or didn't document in this article. One way to discover additional features within GDB is to use the
 
```
help all
```
command, which shows all available commands as well as a short description.
 
 
## Additional Resources
The full GDB user manual can be found at [https://sourceware.org/gdb/current/onlinedocs/gdb](https://sourceware.org/gdb/current/onlinedocs/gdb).
