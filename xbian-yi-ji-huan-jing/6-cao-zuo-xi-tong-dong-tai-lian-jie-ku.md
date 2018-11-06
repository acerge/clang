# 5.1 Linux常用动态链接库操作

## 1. 查看动态链接库的搜索顺序

### 1.0 特别说明

1）/usr/lib and /lib are hardcoded IIRC, 系统级的默认search path

2）编译程序时，使用 -L -l，参见[链接](/xbian-yi-ji-huan-jing/4-bian-yi-xiang-guan.md)

3）LD_LIBRARY_PATH 指定特有的搜索路径

4）安装自定义动态库：
```ldconfig -n directory_with_shared_libraries```

5）搜索优先级： 编译参数 -L >> LD_LIBRARY_PATH >> /etc/ld.so.conf

6）/etc/ld.so.conf及相关目录文件变更后，要使用:
```ldconfig -v```来更新

7) 参考[链接](http://tldp.org/HOWTO/Program-Library-HOWTO/shared-libraries.html)


### 1.1 系统默认的动态链接库

# 某ubuntu系统

```bash

fantasy@azgpuserver1:~$ cat /etc/ld.so.conf.d/*.conf
/usr/local/cuda-8.0/targets/x86_64-linux/lib
# libc default configuration
/usr/local/lib
# Multiarch support
/lib/x86_64-linux-gnu
/usr/lib/x86_64-linux-gnu
/usr/lib/nvidia-384
/usr/lib32/nvidia-384
/usr/lib/nvidia-384
/usr/lib32/nvidia-384
# Legacy biarch compatibility support
/lib32
/usr/lib32
fantasy@azgpuserver1:~$ ls /etc/ld.so.conf.d/
cuda-8-0.conf            i386-linux-gnu_GL.conf  x86_64-linux-gnu.conf      x86_64-linux-gnu_GL.conf
i386-linux-gnu_EGL.conf  libc.conf               x86_64-linux-gnu_EGL.conf  zz_i386-biarch-compat.conf

```
### X. 几个命令

#### X.1 pmap - report memory map of a process

命令可以看出当前进程使用的哪些动态链接库及其内存地址映射

#### X.2 ldd - print shared object dependencies

```bash

fantasy@azgpuserver1:~$ ldd /lib/x86_64-linux-gnu/libcrypt-2.23.so 
        linux-vdso.so.1 =>  (0x00007fffccbfb000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f9d07668000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f9d07c6a000)

```

#### X.3 ldconfig - config or print share lib 

```bash

# 1) print current used lib in cache
/sbin/ldconfig -p
# 2) refresh config
/sbin/ldconfig -v

```

