# 配置环境变量



# 配置linux的环境变量

## 第一级，临时环境变量
1. 临时环境变量，仅当前目录下的终端有效，但是当终端关闭的时候，就会失效。
```bash
    export PATH=$PATH:‘your path’
```
2. 在目录下，添加到配置文件，当终端关闭的时候，乃然有效，重新打开会自动生效。
1. 创建文件
```bash
    vim .bashrc
```
2. 添加内容，在底部添加以下内容，然后保存退出。
```bash
    export PATH=$PATH:‘your path’
```
3. 添加内容之后，配置还没有生效，需要刷新配置文件，可以重新打开终端，或者执行以下命令
```bash
    source .bashrc 
    # 或者 source等同于 .(点)
    . .bashrc
```
## 第二级，用户级的永久环境变量
1. 在根目录下，创建配置文件，即Home目录下的.bashrc文件，可以理解成根目录到Home目录，都是用户级的配置文件。同理可以配置单个用户的环境变量。在Home目录下的用户目录创建.bashrc文件，添加内容，然后刷新配置文件。
2. 创建文件
```bash
    # 全部用户生效
    cd home
    vim .bashrc
    或者
    vim ~/.bashrc
    # 单个用户生效
    vim /home/用户名/.bashrc
```
3. 在 .bashrc 文件的最后加入环境变量
```bash
    export PATH=$PATH:‘your path’
```
4. 保存并退出,刷新配置文件
```bash
    :wq
    source .bashrc
```
## 第三级，系统级的永久环境变量
1. 系统级的环境变量，可以理解成全局的配置文件，所有用户都可以使用。
2. 创建文件
```bash
    vim /etc/profile
```
3. 在 /etc/profile 文件的最后加入环境变量
```bash
    export PATH=$PATH:‘your path’
```
4. 保存并退出,刷新配置文件
```bash
    :wq
    source /etc/profile
```

# bb
    
    $的意思是取变量器，比如$PATH，表示取PATH这个变量的值，$PATH=/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
    PATH=$PATH:，可以理解为在内容后追加
    可以用命令：echo $PATH，来显示环境变量的值
    export

    source .bashrc可以重复使用，用完可以看看export有什么变化


---

> 作者: 莫德飞  
> URL: http://localhost:1313/posts/%E9%85%8D%E7%BD%AE%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F/  

