---
title: Pipe System Call
author: Benedict Ng
categories:
    - [OS]
---

Pipe is a mechanism of inter-process communication (IPC). There are 2 types of pipe: unnamed pipe and named pipe. For unnamed pipe, they can only communicate between parent and child processes or sibling processes. For named pipe, it can communicate between random processes. The content produced at one end of pipe will be consumed at the other end of pipe (producer-consumer model).

Using pipe to read command output to the terminal:

```c
#include <stdio.h>
#include <string.h>
int main(void)
{
    char scree_str[1000];
    memset(scree_str, '\0', 1000);
    FILE * content = popen("ls", "r");
    if(content != NULL)
    {
        fread(scree_str, sizeof(char), 999, content);
        printf("%s", scree_str);
        pclose(content);
    }
    
    return 0;
}
```