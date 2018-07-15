## 5.1 GCC 搜索路径

When environment variables and command-line options are used together the compiler searches the directories in the following order:

#### 1） command-line options ‘-I’ and ‘-L’, from left to right

#### 2） directories specified by environment variables, such as C\_INCLUDE\_

PATH and LIBRARY\_PATH

#### 3）default system directories

## 5.2 Coverage testing with gcov

The GNU coverage testing tool gcov analyses the number of times each  
 line of a program is executed during a run. This makes it possible to  
 find areas of the code which are not used, or which are not exercised  
 in testing. When combined with profiling information from gprof the  
 information from coverage testing allows efforts to speed up a program to  
 be concentrated on specific lines of the source code.

```bash
$ gcov cov.c
88.89% of 9 source lines executed in file cov.c
Creating cov.c.gcov
```

## 5.3 Identifying files

When a source file has been compiled to an object file or executable the options used to compile it are no longer obvious. The file command looks at the contents of an object file or executable and determines some of its characteristics, such as whether it was compiled with dynamic or static linking.

For example, here is the result of the file command for a typical executable:

`$ file a.out`

`a.out: ELF 32-bit LSB executable, Intel 80386,`

`version 1 (SYSV), dynamically linked (uses shared libs), not stripped`

## 5.4 Examining the symbol table

As described earlier in the discussion of debugging, executables and object

files can contain a symbol table \(see Chapter 5 \[Compiling for debugging\],

page 41\). This table stores the location of functions and variables by

name, and can be displayed with the nm command:

```
$ nm  a.out
08048334 t Letext
08049498 ? _DYNAMIC
08049570 ? _GLOBAL_OFFSET_TABLE_
........
080483f0 T main
08049590 b object.11
0804948c d p.3
U printf@GLIBC_2.0
```



