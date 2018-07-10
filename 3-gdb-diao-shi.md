# 3.1 enable core dump

Check the ouput of`ulimit -c`, if it output 0, this is why you don't have core dumped. Use `ulimit -c unlimited`

Also check:

```
$ sysctl kernel.core_pattern
```

You can test it by:

```
sleep 10 & killall -SIGSEGV sleep
```

# 3.2 GDB调试

[https://sourceware.org/gdb/current/onlinedocs/gdb/](https://sourceware.org/gdb/current/onlinedocs/gdb/)

[http://www.cnblogs.com/kingcat/archive/2012/07/13/2589810.html](http://www.cnblogs.com/kingcat/archive/2012/07/13/2589810.html)

[http://valgrind.org/](http://valgrind.org/) Valgrind is an instrumentation framework for building dynamic analysis tools.



