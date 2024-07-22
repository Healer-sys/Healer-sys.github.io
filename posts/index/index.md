# 总结一些Gie经验



# [Git撤销对远程仓库的push&amp;commit提交](https://www.cnblogs.com/chaoxiZ/p/9714085.html)

## 撤销push

1. 执行 git log查看日志，获取需要回退的版本号 

![img](https://img2018.cnblogs.com/blog/788599/201809/788599-20180927164303193-2084393469.png)

2. 执行 git reset –-soft &lt;版本号&gt; ，如 git reset --soft 4f5e9a90edeadcc45d85f43bd861a837fa7ce4c7 ，重置至指定版本的提交，达到撤销提交的目的

然后执行 git log 查看

![img](https://img2018.cnblogs.com/blog/788599/201809/788599-20180927164827547-451137005.png)

此时，已重置至指定版本的提交，log中已经没有了需要撤销的提交

&gt;  git reset 命令分为两种： git reset –-soft 与 git reset –-hard ，区别是：
&gt;
&gt;    前者表示只是改变了HEAD的指向，本地代码不会变化，我们使用git status依然可以看到，同时也可以git commit提交。后者直接回改变本地源码，不仅仅指向变化了，代码也回到了那个版本时的代码。

3. 执行 git push origin 分支名 –-force ，强制提交当前版本号。

至此，撤销push提交完成。

## 撤销commit

1. 执行 git log 查看需要撤销的commit的前面一个提交版本的id；
2. 执行 git reset --hard commit_id ，该commit_id为需要撤销的commit的提交的前面一个提交的版本，即需要恢复到的提交的id，重置至指定版本的提交，达到撤销提交的目的
3. 执行 git log 查看，commit提交已撤销
# 添加多个远程仓库
## 修改config文件

1. 定位到.git/config


![image-20240715102010000](https://cdn.jsdelivr.net/gh/Healer-sys/ImageHub/Image/202407151021974.png)

&gt;  [remote &#34;origin&#34;] 远程：
&gt;
&gt;  url：推送地址
&gt;
&gt;  fetch：拉取地址
2. 添加remote里面的url，如gitee仓库


![image-20240715102946107](https://cdn.jsdelivr.net/gh/Healer-sys/ImageHub/Image/202407151030982.png)

3. push同步提交

```shell
git push origin
```

![image-20240715103429622](https://cdn.jsdelivr.net/gh/Healer-sys/ImageHub/Image/202407151034985.png)
4. 至此，添加多个远程仓库完成。

# 查看分支和切换分支
## 查看分支
```shell
	git branch -a
```
![image-20240719223802363](https://cdn.jsdelivr.net/gh/Healer-sys/ImageHub/Image/202407192238748.png)
## 切换分支

```shell
git checkout branchName
```
![image-20240719230135038](https://cdn.jsdelivr.net/gh/Healer-sys/ImageHub/Image/202407192301212.png)

# [git拉取、推送所有分支及标签](https://www.cnblogs.com/xiaojiluben/p/15880248.html)

&gt; 从origin1推送到origin2

```bash
git push origin2 &#39;refs/remotes/origin1/*:refs/heads/*&#39; 

# 推送后带有后缀demo01 
git push origin2&#39;refs/remotes/origin1/*:refs/heads/demo01/*&#39; 

# 推送指定分支 
git push origin2 &#39;refs/remotes/origin1/dev:refs/heads/dev&#39;
```

&gt; 拉取所有分支到本地

```bash
git branch -r | grep -v &#39;\-&gt;&#39; | while read remote; do git branch --track &#34;${remote#origin/}&#34; &#34;$remote&#34;; done
git fetch --all
git pull --all
```
![image-20240719225022333](https://cdn.jsdelivr.net/gh/Healer-sys/ImageHub/Image/202407192250216.png)
&gt; 拉取所有标签到本地

```sql
git fetch origin --prune
```

&gt; 切换远程仓库
&gt; 推送所有分支

```css
git push --mirror
```

&gt; 推送所有标签

```css
git push origin --tags
```

&gt; git迁移脚本

```shell
#!/bin/bash

export oldUrl=$1
export newUrl=$2
export repoName=$3

printf &#34;oldUrl: %s\nnewUrl: %s\nrepoName: %s\n&#34; $oldUrl $newUrl $repoName

[[ -z &#34;${oldUrl}&#34; ]] &amp;&amp; echo &#34;不能为空&#34; &amp;&amp; exit
[[ -z &#34;${newUrl}&#34; ]] &amp;&amp; echo &#34;不能为空&#34; &amp;&amp; exit
[[ -z &#34;${repoName}&#34; ]] &amp;&amp; echo &#34;不能为空&#34; &amp;&amp; exit

printf &#34;克隆原仓库&#34;
cd repo || exit
git.exe clone --progress -v &#34;${oldUrl}&#34;
cd ${repoName} || exit
#git branch -r | grep -v &#39;\-&gt;&#39; | while read remote; do git branch --track &#34;${remote#origin/}&#34; &#34;$remote&#34;; done
#git fetch --all
#git pull --all
#git fetch origin --prune


# 原仓库
# dev -&gt; dev-origin

# git remote change new repo
echo &#34;增加新仓库&#34;
git remote rename origin origin-old
git remote add origin &#34;${newUrl}&#34;

echo &#34;拉取所有仓库分支&#34;
git fetch origin
git fetch origin-old

echo &#34;删除本地dev分支&#34;
git branch -D dev





# origin/dev -&gt; origin/dev-bak
echo &#34;备份分支&#34;
git checkout -b dev-origin origin-old/dev
git checkout -b dev-yanshi4-11 origin-old/yanshi4-11
git checkout -b dev-bak origin/dev

echo &#34;新仓库切出dev分支&#34;
git checkout -b dev origin/dev

echo &#34;复制ci配置到临时文件&#34;
mkdir -p ../tmp/ || exit
cp .gitlab-ci.yml Dockerfile ../tmp/ || exit

echo &#34;切回旧仓库dev分支&#34;
git checkout master
git branch -D dev
git checkout -b dev origin-old/yanshi4-11

echo &#34;配置ci配置&#34;
mv -f ../tmp/* ./ || exit
mv -f ../tmp/.gitlab-ci.yml ./  || exit
git add .
git commit -m &#34;ci适配&#34;

echo &#34;删除旧仓库远程配置，防止误删&#34;
git remote remove origin-old


echo &#34;推送所有分支&#34;
git push --progress &#34;origin&#34; dev-origin
git push --progress &#34;origin&#34; dev-yanshi4-11
git push --progress &#34;origin&#34; dev-bak
git push --force --progress &#34;origin&#34; dev:dev
```


---

> 作者: 莫德飞  
> URL: http://localhost:1313/posts/index/  

