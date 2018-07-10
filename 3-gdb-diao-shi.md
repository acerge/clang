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



