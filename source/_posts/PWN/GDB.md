---
title: GDB Debugger
author: Benedict Ng
categories:
    - [PWN, Tools]
---
## Compiling

Add `-g` option to gcc to preserve debugging info.

```shell
gcc -o target target.c -g
```

Using `file` command to check if the elf file contains debugging information.

```txt
pipe: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), 
dynamically linked, 
interpreter /lib64/ld-linux-x86-64.so.2, 
BuildID[sha1]=d46958bf74a98420d595d3318c5417fb3d14c260, 
for GNU/Linux 3.2.0, with debug_info, not stripped
```

The last line indicates that this elf file is with debugging information. You can remove it with `strip` command.

```txt
pipe: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), 
dynamically linked, 
interpreter /lib64/ld-linux-x86-64.so.2, 
BuildID[sha1]=d46958bf74a98420d595d3318c5417fb3d14c260, 
for GNU/Linux 3.2.0, stripped
```

## Basic Commands

Debugging a elf file: `gdb filename`.

| Command               | abbr. | Meaning |
| :-------------------: | :-:   | :-      |
| set args              |       |   set command line arguments. For example: ` gdb filename` to enter gdb interface. And `set args a1 a2` to pass arguments to gdb. |
| break `line`          | b     | set breakpoint. `b 20` means set a break point at line 20. You can set multiple breakpoints. The `line` is identical with the line number in the source file (ignoring comments). |
| run                   | r     | run the program till the next break point. |
| next                  | n     | execute the next statement. If this statement is a function call, `next` command will not go into the function call. It will execute the function call and stop at the next line of the function call. |
| step                  | s     | Go into the function call if you the source code of the function is compiled to the program, otherwise if it is loaded as library, it will not go into the function. |
| print `var`           | p     | print the value in variable `var`. This command can entail with `strcpy`. For example, a char array named `Hello` with length 20. You can execute command `p strcpy(Hello,"Hello World")` then `p Hello`. For `p Hello`, the value of `Hello`, that is **Hello World** will be printed. |
| continue              | c     | continue executing till the next breakpoint. |
| set var `var`=`value` |       | set variable `var` of value `value`. |
| backtrace             | bt    | Check the stack frame of called function. |
| info `type`           |       | Check the information of specific type. For example, `info b` checks the information of all set breakpoints. |
| quit                  | q     | quit gdb environment. |

## Core Dump

Core is a file used by gdb to track segmentaion faults.

1. Use `ulimit` command to check & modify the parameters of each process of current user.

`ulimit -a`: Check parameters.
`ulimit -c unlimited`: Set file size of core file unlimited.

2. Then use command `gcc -g -o target src.c` to compile an executable file with debug info.
3.  Execute the compiled file.
4.  Check `/proc/sys/kernel/core_pattern` file to see where is the core file. The location can be modified.
5.  `gdb /path/to/executalbe /path/to/core` to debug the segmentation faults.

## Debugging Running Program

1. Run the `target` executable file. You can run it at background or open a new terminal for debugging.
2. `ps -ef | grep target` to get the running process PID.
3. `gdb target -p PID` to debug the program. The running process will be blocked for debugging.

## Debugging Multi-process Program

### Debugging Parent | Child Process: `set follow-fork-mode [parent | child]`

After the program is compiled, you debug it as usual. By default, the process you are debugging is parent process. If the breakpoint is set after the `fork()` function, the child process will continue running till exit and the parent process will still be blocked for debugging.  You can change it with command `set follow-fork-mode child` to debug the child process. When you are debugging the child process, the procedure looks quite similar: the parent process will continue running till exit and the child process will still be blocked for debugging.

### Debugging Mode: `set detach-on-fork [on | off]`

By default, this setting is `on`, which means other process will continue executing while you are debugging. If it is set to `off`, the other process will be hung up.

## Switching between Processes

The gdb command `info inferiors` shows all executing PIDs for the debugged program. Use command `inferior  PID` to switch to other process.

## Debugging Multi-threading Program

Just compile the program as usual (don't forget `-g` for debugging and `-lpthread` to link the library). Then use command `ps -aL | grep target` to check the thread IDs of your `target` program. You can also check the relationship between main thread and other sub-threads with command `pstree -p main_thread_ID`.

### Switching between Threads

In gdb command line, you can use `info threads` to check the all threads information. The thread marked with `*` is the current thread. Use command `thread thread_id` to switch between threads.

### Debugging Mode: `set scheduler-locking [off | on]`

By default this setting is `off`, which means when you are debugging specific thread other threads will continue executing as well. You can toggle it to `on` to hang up other threads.

### Apply GDB Command to Specific Thread or All Threads: `thread apply thread_id cmd`/`thread apply all cmd`
