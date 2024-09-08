---
title: Memory Alignment
author: Benedict Ng
categories:
    - [OS]
---
[Reference](https://en.cppreference.com/w/c/language/object#:~:text=Every%20complete%20object%20type%20has%20a%20property%20called,alignment%20values%20are%20non-negative%20integral%20powers%20of%20two.)

## Rules of Memory Alignment

For each compiler, it has its own default alignment. For gcc its default alignment is 4, and it can be modified with macro `#pragma pack(n)`, n is the targeted alignment and it should be power of 2.

The valid alignment value is the final alignment that the compiler will adopt. Its the smaller one between the default alignment and the member in your `class/struct` with biggest size. It should be power of 2 as well.

The first offset of a member in `struct` is 0, and the offset between each member in `struct` and the first address of the `struct` is the smaller one between the size of the member and valid alignment value. The size of the `struct` also needs to be multiple of the valid alignment value as well.

For embedded `struct`, please treat the inner `struct` as inline in the outer `struct`. This means that just treat it as the members of inner `struct` as inline of the outer `struct`.

For `union`, the memory aligned by the size which is biggest of all members.

For C++, non-virtual functions and static variables will not considered as any member in memory alignment, i.e. just ignore them because compiler will treat them as sth. outside of the `struct/class`. For virtual functions it will only occupies one memory space with size of valid asignment value.

### Example

Considering the following code:

```c++
struct test  
{
    /* data */
    char c;
    int a;
    double ptr; 
    virtual void test(){};
    virtual void test1(){};
    
};

int main()
{
    test t1;
    printf("%ld\n", sizeof(t1))
}
```

If the valid alignment value is 4: `sizeof(t1)=20`. If it is 8, `sizeof(t1)=24`.
