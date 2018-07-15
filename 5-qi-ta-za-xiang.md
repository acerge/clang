## 5.1 GCC 搜索路径

When environment variables and command-line options are used together the compiler searches the directories in the following order:

#### 1） command-line options ‘-I’ and ‘-L’, from left to right

#### 2） directories specified by environment variables, such as C\_INCLUDE\_

PATH and LIBRARY\_PATH

#### 3）default system directories



## 5.2 Coverage testing with gcov

The GNU coverage testing tool gcov analyses the number of times each line of a program is executed during a run. This makes it possible to find areas of the code which are not used, or which are not exercised in testing. When combined with profiling information from gprof the information from coverage testing allows efforts to speed up a program to be concentrated on specific lines of the source code.

```bash
$ gcov cov.c
88.89% of 9 source lines executed in file cov.c
Creating cov.c.gcov
```



