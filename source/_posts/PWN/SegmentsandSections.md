---
title: Segments and Sections
author: Benedict Ng
categories:
    - [PWN, Theory]
---
[Reference Link](./https://medium.com/@johnehk86/66-what-are-memory-and-sections-text-data-bss-rodata-etc-e134bd5b9ccd)

## Segments

Segments usually describe the layout of `.elf` files in memory. In a C program the memory layout is as follows:

![Memory Layout](./SegmentsandSections/memoryLayoutC.jpg)

So each the segments are different parts of the memory space e.g. Stack/Heap. **Note: uninitialized data (.bss) and initialized data (.data) should be regarded as a whole: data segment. They are sections by themselves.** This memory layout is for virtual address. (visible to programmers). About the actually layout in physical memory, it is handled by OS using paging technique to mapping virtual address to physical address by page table. In a 32 bits OS, memory space for each process is 4GB (2^32). However 1 GB is preserved for OS kernel and shared between process, only about 3GB are exclusive for each process.

[Memory Segments](https://www.youtube.com/watch?v=m1UzSfgjA4Y)

- Stack Segment:

  Stack is preserved for local variables and function calls. It grows from high address to low address. The stack segment are exclusive for each thread for multi-threads programs (this is a special case because other segments are shared between threads).

**Note: Between Stack and Heap there is a memory mapping segment used to store data of dynamic linked libraries.**

- Heap Segment:

  Heap is dynamically allocated memory (malloc(), calloc()) and its lifetime is managed by user (use free() to deallocate). It grows form low address to high address.
- Data Segment:

  Data Segment has 2 sections: uninitialized data section (.bss) and initialized (.data) section.

1. .bss: It contains uninitialized static and global variables and they last during the whole lifetime of the program. These variables will be initialized as 0.
2. .data: It contains initialized static and global variables and they last during the whole lifetime of the program. The variables in this section can be constant if you added `const` specifier in initialization.

```c
#include <stdio.h>
const char * text = "Hello World";  // In this case, the variable text is constant 
int val;
int main()
{
    printf("%d\n%s\n", val, text); // val will be 0 here
    return 0;
}
```

- Text Segment: It is also referred as code segment. It contains execution instructions for programs or const variables and its content cannot be modified during program execution. It is placed in the lower address so it will not be overwritten by heap or stack.

## Sections

In contrast with segments, sections describe the layout of `.elf` files in disk. The most frequently used sections in PWN is .data and .bss as mentioned before. Now let's analyze a demo program.

```c
#include<stdio.h>

int global; //.bss section
char * str = "Hello World!"; // .data section

int sum(int x, int y);

int main()
{

	sum(1, 2);
	void * ptr = malloc(0x100);
	read(0, ptr, 0x100); // input "deadbeef" here
	return 0;
}

int sum(int x, int y)
{
	int ret = x + y;
}
```

Since `t` is the return value and `ptr` is local variable, they should be laid in stack segment. The heap segment stores the value `deadfeef` from the keyboard input. The `global` and `str` are both global variables. Since `global` is uninitialized variable, it should be initialized as 0 and stored in .bss section; and for `str`, since it is initialized, so it is stored in `.data` section. Both of them belongs to data segment. The text segment stores constant and instructions, so its content should be `main`, `sum` and `Hello World!`.

About `x` and `y`: its related with the CPU architecture. In x86 architecture, they should be stored in stack segment. In amd64, they are stored in registers.
