GNU CC\(简称gcc\)是GNU项目中符合ANSI C标准的编译系统，能够编译用C、C++、Object C、Jave等多种语言编写的程序。gcc又可以作为交叉编译工具，它能够在当前CPU平台上为多种不同体系结构的硬件平台开发软件，非常适合在嵌入式领域的开发编译，如常用的arm-linux-gcc交叉编译工具

通常后跟一些选项和文件名来使用GCC编译器。gcc命令的基本用法如下:

gcc \[options\] \[filenames\]

选项指定编译器怎样进行编译。

## 一、gcc编译流程

### 1.预处理-Pre-Processing

`gcc -E test.c-otest.i //.i文件`

### 2.编译-Compiling

`gcc -S test.i -o test.s //.s文件`

### 3.汇编-Assembling//.o文件

`gcc -c test.s -o test.o`

### 4.链接-Linking//bin文件

`gcc test.o -o test`

## 二、gcc工程惯用

### 1.编译

```
gcc -c test.c  //.o文件，汇编
gcc -o test  test.c  //bin可执行文件
gcc test.c  //a.out可执行文件
```

如果是c++直接将gcc改为g++即可。

### 2.常用参数

#### 1）-E参数

-E选项指示编译器仅对输入文件进行预处理。当这个选项被使用时,预处理器的输出被送到标准输出而不是储

存在文件里.

#### 2）-S参数

-S编译选项告诉GCC在为C代码产生了汇编语言文件后停止编译。GCC产生的汇编语言文件的缺省扩展名是.s。

#### 3）-c参数

-c选项告诉GCC仅把源代码编译为目标代码。缺省时GCC建立的目标代码文件有一个.o的扩展名。

#### 4）-o参数

-o编译选项来为将产生的可执行文件用指定的文件名。

#### 5）-O参数

-O选项告诉GCC对源代码进行基本优化。这些优化在大多数情况下都会使程序执行的更快。-O2选项告诉GCC产生尽可能小和尽可能快的代码。如-O2，-O3，-On（n常为0--3）；

-O主要进行跳转和延迟退栈两种优化；

-O2除了完成-O1的优化之外，还进行一些额外的调整工作，如指令调整等。

-O3则包括循环展开和其他一些与处理特性相关的优化工作。

选项将使编译的速度比使用-O时慢，但通常产生的代码执行速度会更快。

如：

```bash
[root@localhost test\]\# gcc test.c -O3
[root@localhost test\]\# gcc -O3 test.c
[root@localhost test\]\# gcc -o tt test.c -O2
[root@localhost test\]\# gcc -O2 -o tt test.c
```

#### 6）调试选项-g和-pg

GCC支持数种调试和剖析选项，常用到的是-g和-pg。

-g选项告诉GCC产生能被GNU调试器使用的调试信息以便调试你的程序。GCC提供了一个很多其他C编译

器里没有的特性,在GCC里你能使-g和-O \(产生优化代码\)联用。

-pg选项告诉GCC在编译好的程序里加入额外的代码。运行程序时,产生gprof用的剖析信息以显示你的程序的耗时情况。

#### 7）-l参数和-L参数

-l参数就是用来指定程序要链接的库，-l参数紧接着就是库名，那么库名跟真正的库文件名有什么关系呢？

就拿数学库来说，他的库名是m，他的库文件名是libm.so，很容易看出，把库文件名的头lib和尾.so去掉就是库名了。

如：

```bash
gcc  xxx.c -lm #(动态数学库)
-lpthread

```

好了现在我们知道怎么得到库名了，比如我们自已要用到一个第三方提供的库名字叫libtest.so，那么我们只要把libtest.so拷贝到/usr/lib里，编译时加上-ltest参数，我们就能用上libtest.so库了（当然要用libtest.so库里的函数，我们还需要与libtest.so配套的头文件）。

放在/lib和/usr/lib和/usr/local/lib里的库直接用-l参数就能链接了，但如果库文件没放在这三个目录里，而是放在其他目录里，这时我们只用-l参数的话，链接还是会出错，出错信息大概是：“/usr/bin/ld: cannot find -lxxx”，也就是链接程序ld在那3个目录里找不到libxxx.so，这时另外一个参数-L就派上用场了，比如常用的X11的库，它放在/usr/X11R6/lib目录下，我们编译时就要用`-L/usr/X11R6/lib -lX11参数，-L参数跟着的是库文件所在的目录名。再比如我们把libtest.so放在`/aaa/bbb/ccc 目录下，那链接参数就是 `-L/aaa/bbb/ccc -ltest`

另外，大部分`libxxxx.so`只是一个链接，以RH9为例，比如libm.so它链接到`/lib/libm.so.x`，`/lib/libm.so.6`又链接到`/lib/libm-2.3.2.so`，如果没有这样的链接，还是会出错，因为ld只会找libxxxx.so，所以如果你要用到xxxx库，而只有libxxxx.so.x或者libxxxx-x.x.x.so，做一个链接就可以了`ln -s libxxxx-x.x.x.so libxxxx.so`

手工来写链接参数总是很麻烦的，还好很多库开发包提供了生成链接参数的程序，名字一般叫xxxx-config，一般放在/usr/bin目录下，比如gtk1.2的链接参数生成程序是gtk-config，执行gtk-config --libs就能得到以下输出`-L/usr/lib -L/usr/X11R6/lib -lgtk -lgdk -rdynamic -lgmodule -lglib -ldl -lXi -lXext -lX11 -lm`，这就是编译一个gtk1.2程序所需的gtk链接参数，xxx-config除了--libs参数外还有一个参数是--cflags用来生成头文

件包含目录的，也就是-I参数，在下面我们将会讲到。你可以试试执行gtk-config --libs --cflags，看看输出结果。

现在的问题就是怎样用这些输出结果了，最笨的方法就是复制粘贴或者照抄，聪明的办法是在编译命令行里加入这个\`xxxx-config --libs --cflags\`，比如编译一个gtk程序：gcc gtktest.c \`gtk-config --libs --cflags\`这样就差不多了。注意\`不是单引号，而是1键左边那个键。

除了xxx-config以外，现在新的开发包一般都用pkg-config来生成链接参数，使用方法跟xxx-config类似，但xxx-config是针对特定的开发包，但pkg-config包含很多开发包的链接参数的生成，用pkg-config --list-all命令可以列出所支持的所有开发包，pkg-config的用法就是pkg-config pagName --libs --cflags，其中pagName是包名，是pkg-config--list-all里列出名单中的一个，比如gtk1.2的名字就是gtk+，pkg-config gtk+ --libs --cflags的作用跟gtk-config --libs --cflags是一样的。比如：

```bash
gcc gtktest.c `pkg-config gtk+ --libs --cflags`
```

#### 8）-include和-I参数

-include用来包含头文件，但一般情况下包含头文件都在源码里用＃i nclude xxxxxx实现，-include参数很少用。-I参数是用来指定头文件目录，/usr/include目录一般是不用指定的，gcc知道去那里找，但是如果头文件不在/usr/icnclude里我们就要用-I参数指定了，比如头文件放在/myinclude目录里，那编译命令行就要加上-I/myinclude参数了，如果不加你会得到一个"xxxx.h: No such file or directory"的错误。-I参数可以用相对路径，比如头文件在当前目录，可以用-I.来指定。上面我们提到的--cflags参数就是用来生成-I参数的。

#### 9）-Wall、-w和-v参数

-Wall打印出gcc提供的警告信息

-w关闭所有警告信息

-v列出所有编译步骤

## 四.几个相关的环境变量

PKG\_CONFIG\_PATH：用来指定pkg-config用到的pc文件的路径，默认是/usr/lib/pkgconfig，pc文件是文本文件，扩展名是.pc，里面定义开发包的安装路径，Libs参数和Cflags参数等等。

**CC：**用来指定c编译器。

**CXX：**用来指定cxx编译器。

**LIBS**：跟上面的--libs作用差不多。

**CFLAGS**: 跟上面的--cflags作用差不多。

**CC，CXX，LIBS，CFLAGS**手动编译时一般用不上，在做configure时有时用到，一般情况下不用管。

环境变量设定方法：exportENV\_NAME=xxxxxxxxxxxxxxxxx

## 五.关于交叉编译

交叉编译通俗地讲就是在一种平台上编译出能运行在体系结构不同的另一种平台上，比如在我们地PC平台\(X86 CPU\)上编译出能运行在arm CPU平台上的程序，编译得到的程序在X86 CPU平台上是不能运行的，必须放到armCPU平台上才能运行。当然两个平台用的都是linux。这种方法在异平台移植和嵌入式开发时用得非常普遍。相对与交叉编译，我们平常做的编译就叫本地编译，也就是在当前平台编译，编译得到的程序也是在本地执行。用来编译这种程序的编译器就叫交叉编译器，相对来说，用来做本地编译的就叫本地编译器，一般用的都是gcc，但这种gcc跟本地的gcc编译器是不一样的，需要在编译gcc时用特定的configure参数才能得到支持交叉编译的gcc。为了不跟本地编译器混淆，交叉编译器的名字一般都有前缀，比如armc-xxxx-linux-gnu-gcc，arm-xxxx-linux-gnu- g++等等

交叉编译器的使用方法

使用方法跟本地的gcc差不多，但有一点特殊的是：必须用-L和-I参数指定编译器用arm系统的库和头文件，不能用本地\(X86\)的库（头文件有时可以用本地的）。

例子：

`arm-xxxx-linux-gnu-gcc test.c -L/path/to/sparcLib -I/path/to/armInclude`

## 六、man gcc部分

GCC\(1\)GNUGCC\(1\)

NAME

gcc - GNU project C and C++ compiler

SYNOPSIS

gcc \[-c \| -S \| -E\] \[-std=standard\]

\[-g\] \[-pg\] \[-Olevel\]

\[-Wwarn...\] \[-pedantic\]

\[-Idir...\] \[-Ldir...\]

\[-Dmacro\[=defn\]...\] \[-Umacro\]

\[-foption...\] \[-mmachine-option...\]

\[-o outfile\] infile...

Only the most useful options are listed here; see below for the remain-

der.g++ accepts mostly the same options as gcc.

