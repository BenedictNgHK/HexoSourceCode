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
pipe: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, 
interpreter /lib64/ld-linux-x86-64.so.2, 
BuildID[sha1]=d46958bf74a98420d595d3318c5417fb3d14c260, 
for GNU/Linux 3.2.0, with debug_info, not stripped
```

The last line indicates that this elf file is with debugging information. You can remove it with `strip` command.

```txt
pipe: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, 
interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=d46958bf74a98420d595d3318c5417fb3d14c260, 
for GNU/Linux 3.2.0, stripped
```

## Basic Commands

Debugging a elf file: `gdb filename`.

| Command               | abbr. | Meaning |
| :-------------------: | :-:   | :-      |
| set args              |       |   set command line arguments. For example: ` gdb filename` to enter gdb interface. And `set args a1 a2` to pass arguments to gdb. |
| break `line`          | b     | set breakpoint. `b 20` means set a break point at line 20. You can set multiple breakpoints. |
| run                   | r     | run the program till the next break point. |
| next                  | n     | execute the next statement. If this statement is a function call, `next` command will not go into the function call. It will execute the function call and stop at the next line of the function call. |
| step                  | s     | Go into the function call if you the source code of the function is compiled to the program, otherwise if it is loaded as library, it will not go into the function. |
| print `var`           | p     | print the value in variable `var`. This command can entail with `strcpy`. For example, a char array named `Hello` with length 20. You can execute command `p strcpy(Hello,"Hello World")` then `p Hello`. For `p Hello`, the value of `Hello`, that is **Hello World** will be printed. |
| continue              | c     | continue executing till the next breakpoint. |
| set var `var`=`value` |       | set variable `var` of value `value`. |
| quit                  | q     | quit gdb environment. |
