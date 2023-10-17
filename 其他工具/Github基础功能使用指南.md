# Github简单使用

### 一、本地连接github
##### 1.下载git客户端
比较简单在此不做演示
##### 2.生成ssh密钥
使用git  bash输入
`ssh-keygen -t ed25519 -C "你的github邮箱地址"`
一直回车即可，当然也可以自定义一些rsa加密的参数

会在用户目录/.ssh生成公钥和私钥
##### 3.上传ssh密钥到github
将公钥id_rsa .pub的内容复制到github网站，Setting/SSH and GPG keys 复制添加密钥即可
##### 4.配置~/.ssh config文件
先执行第五步，如果出现端口异常，则需要挂梯子或者修改本文件。
在~/.shh下编辑（没有就新建config.config）如下内容
```config
Host github.com
User git
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
Port 443
```
##### 5.设置git config及测试连接
输入如下代码设置config
```git
$ git config --global user.name  "name"//自定义用户名
$ git config --global user.email "youxiang@qq.com"//用户邮箱

```
输入如下代码测试连接：
`$ ssh -T git@github.com`
输入yes持久连接即可

### 二、新建仓库

#### 1、新建远程仓库
在github新建Repositories，选择公开（或私立），设置开源协议，新建即可
下面的步骤，github会给提示相关的代码

#### 2、新建本地仓库及连接
找到一个需要建立本地仓库的文件夹
用gitbash打开
##### a.初始化本地仓库
`git init`
##### b.与远程仓库建立连接
`git status`查看仓库状态
在github仓库找到仓库地址,与本地仓库建立连接
`git remote add origin https://github.com/k0kax/myZinx.git`
##### c.将代码添加到暂存区
`git add .`这是将所有文件放到暂存区
```git
$ git add .
warning: in the working copy of 'Github使用指南.md', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'go.mod', LF will be replaced by CRLF the next time Git touches it
```

`git status`查看状态
```git
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   .idea/.gitignore
        new file:   .idea/modules.xml
        new file:   .idea/myZinx.iml
        new file:   "Github\344\275\277\347\224\250\346\214\207\345\215\227.md"
        new file:   go.mod


```

##### 4.提交到仓库
`git commit -m "first commit(此处为提交信息)"`
```git
[master (root-commit) 20abd9f] first commit
 5 files changed, 82 insertions(+)
 create mode 100644 .idea/.gitignore
 create mode 100644 .idea/modules.xml
 create mode 100644 .idea/myZinx.iml
 create mode 100644 "Github\344\275\277\347\224\250\346\214\207\345\215\227.md"
 create mode 100644 go.mod


```



### 三、代码更新

##### 1.同步本地仓库-->远程仓库

将本地仓库与远程仓库同步，把本地代码push上传到github远程仓库中
````git
git push origin master
````

```git
$ git push origin master
Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Delta compression using up to 16 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (8/8), 1.91 KiB | 1.91 MiB/s, done.
Total 8 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/k0kax/myZinx.git
 * [new branch]      master -> master
```
***问题处理***
这一步很容易出问题，常出现以下报错
```git
 fatal: unable to access 'https://github.com/k0kax/myZinx.git/': 
 Failed to connect to github.com port 443 after 21057 ms: Couldn't 
 connect to server
```
参考解决[方案](https://blog.csdn.net/good_good_xiu/article/details/118567249)
取消github的https和http，或者添加本地代理
```git
//取消http代理
git config --global --unset http.proxy
//取消https代理 
git config --global --unset https.proxy
```
在此推荐使用代理工具[FastGithub](https://github.com/dotnetcore/FastGithub)


***更新远程仓库代码需要：***
* 加入暂存区`git add .`
* 然后commit `git commit -m "commit"`
* 最后上传`git push origin master`


##### 2.同步远程仓库--->本地仓库
远程仓库同步到本地仓库，需要从远程pull下来
~~git pull origin master~~ git pull强制覆盖本地的代码方式,且这个东西贼几把难用，推荐以下方法：
`git fetch --all`

然后，你有两个选择：
`git reset --hard origin/master`

或者如果你在其他分支上：
`git reset --hard origin/<branch_name>`


##### 3.其他的东西
其他的东西如git clone这个比较简单，在此不做演示，仓库分支，log等一些东西目前用不上，在此也不做讨论

##### 4.参考资料
> https://zhuanlan.zhihu.com/p/369486197     
> https://blog.csdn.net/qq_36330643/article/details/87923045


