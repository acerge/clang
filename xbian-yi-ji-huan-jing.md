# 1 cmake find\_package

## 1.1 cmake 的安装目录如下：

```bash
root@server45:/usr/share# tree cmake-3.5/ -d
cmake-3.5/
├── completions
├── editors
│   ├── emacs
│   └── vim
├── Help
│   ├── command
│   ├── generator
│   ├── include
│   ├── manual
│   ├── module
│   ├── policy
│   ├── prop_cache
│   ├── prop_dir
│   ├── prop_gbl
│   ├── prop_inst
│   ├── prop_sf
│   ├── prop_test
│   ├── prop_tgt
│   ├── release
│   └── variable
├── include
├── Modules
│   ├── CMakeAddFortranSubdirectory
│   ├── Compiler
│   ├── CompilerId
│   ├── FindCUDA
│   ├── FortranCInterface
│   │   └── Verify
│   ├── IntelVSImplicitPath
│   ├── Internal
│   └── Platform
└── Templates
    └── Windows

```

## 1.2 cmake Modules目录如下

```bash
root@server45:/usr/share/cmake-3.5/Modules# ll
total 3256
drwxr-xr-x 10 root root 20480 Jul  3 20:50 ./
drwxr-xr-x  8 root root    99 May 23 18:09 ../
-rw-r--r--  1 root root  1053 Mar 24  2016 AddFileDependencies.cmake
-rw-r--r--  1 root root  1450 Sep 27  2016 AutogenInfo.cmake.in
-rw-r--r--  1 root root  1337 Mar 24  2016 BasicConfigVersion-AnyNewerVersion.cmake.in
-rw-r--r--  1 root root  1805 Mar 24  2016 BasicConfigVersion-ExactVersion.cmake.in
-rw-r--r--  1 root root  1691 Mar 24  2016 BasicConfigVersion-SameMajorVersion.cmake.in
-rw-r--r--  1 root root 35271 Mar 24  2016 BundleUtilities.cmake
-rw-r--r--  1 root root  2554 Mar 24  2016 CheckCCompilerFlag.cmake
-rw-r--r--  1 root root  3883 Mar 24  2016 CheckCSourceCompiles.cmake
-rw-r--r--  1 root root  3859 Mar 24  2016 CheckCSourceRuns.cmake
-rw-r--r--  1 root root  2539 Mar 24  2016 CheckCXXCompilerFlag.cmake
-rw-r--r--  1 root root  3914 Mar 24  2016 CheckCXXSourceCompiles.cmake
-rw-r--r--  1 root root  3889 Mar 24  2016 CheckCXXSourceRuns.cmake
-rw-r--r--  1 root root  2109 Mar 24  2016 CheckCXXSymbolExists.cmake
-rw-r--r--  1 root root   702 Mar 24  2016 CheckForPthreads.c
。。。。
```

## 1.3 find\_package\(OpenSSL\)，对应文件如下：

```bash
root@server45:/usr/share/cmake-3.5/Modules# ll *OpenSSL*
-rw-r--r-- 1 root root 14156 Mar 24  2016 FindOpenSSL.cmake 
# cat FindOpenSSL.cmake
# (To distribute this file outside of CMake, substitute the full
#  License text for the above reference.)

if (UNIX)
  find_package(PkgConfig QUIET)
  pkg_check_modules(_OPENSSL QUIET openssl)
endif ()

# Support preference of static libs by adjusting CMAKE_FIND_LIBRARY_SUFFIXES
if(OPENSSL_USE_STATIC_LIBS)
  set(_openssl_ORIG_CMAKE_FIND_LIBRARY_SUFFIXES ${CMAKE_FIND_LIBRARY_SUFFIXES})
  if(WIN32)
    set(CMAKE_FIND_LIBRARY_SUFFIXES .lib .a ${CMAKE_FIND_LIBRARY_SUFFIXES})
  else()
    set(CMAKE_FIND_LIBRARY_SUFFIXES .a )
  endif()
endif()

if (WIN32)
  # http://www.slproweb.com/products/Win32OpenSSL.html
  set(_OPENSSL_ROOT_HINTS
.....
```

## 1.4 总结：按照配置文件中的规则查找包



