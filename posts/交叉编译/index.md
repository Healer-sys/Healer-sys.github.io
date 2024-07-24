# 交叉编译



## 1. 查看交叉编译工具链的搜索路径
```bash
/usr/lib/gcc-cross/aarch64-linux-gnu/11/../../../../aarch64-linux-gnu/bin/ld --verbose | grep SEARCH_DIR
```
## 2. 添加交叉编译工具链的搜索路径

### 方法一
```bash
    # 添加链接库的搜索路径
    gcc -L/Your_Path/lib ....
```

### 方法二
添加变量
```bash
    # 程序编译时搜索的库路径，即指定gcc编译需要链接动态链接库的目录
    export LIBRARY_PATH=/Your_Path/lib:$LIBRARY_PATH

    # 编译时搜索的头文件路径
    export C_INCLUDE_PATH=/Your_Path/lib:$C_INCLUDE_PATH

    # 程序运行时搜索的库路径
    export LD_LIBRARY_PATH=/Your_Path/lib:$LD_LIBRARY_PATH
```

### 终极绝招，修改链接器缓存
使用GCC编译动态链接库的项目时，在其他目录下执行很可以出现找不到动态链接库的问题。

这种情况多发生在动态链接库是自己开发的情况下，原因就是程序运行时找不到去何处加载动态链接库。

可能会说在编译时指定了链接的目录啊！编译时指定的 -L的目录，只是在程序链接成可执行文件时使用的。程序执行时动态链接库加载不到动态链接库。


解决办法有两种，第一程序链接时指定链接库的位置，就是使用-wl,-rpath=&lt;link_path&gt;参数，&lt;link_path&gt;就是链接库的路径。如：
```bash
gcc -o foo foo.c -L. -lfoo -Wl,-rpath=./
```
上面就是指定了链接的位置在当前目录，这种情况只有在当前目录执行./foo时，才是可以正确使用的。一般情况我们使用如下格式：
```bash
gcc -o foo foo.c -L$(prefix)/lib -lfoo -Wl,-rpath=$(prefix)/lib
```
第二种方式就是，将链接库的目录添加到/etc/ld.so.conf文件中或者添加到/etc/ld.so.conf.d/*.conf中，然后使用ldconfig进行更新，进行动态链接库的运行时动态绑定。如：
添加文件/etc/ld.so.conf.d/foo.conf，内容如下：

/your path/lib
然后执行如下命令：
```bash
    ldconfig
```


参考文章:
    [Re: linking with -Wl,-rpath and $(prefix)](https://gcc.gnu.org/ml/gcc-help/2005-12/msg00017.html)


---

> 作者: 莫德飞  
> URL: http://localhost:1313/posts/%E4%BA%A4%E5%8F%89%E7%BC%96%E8%AF%91/  

