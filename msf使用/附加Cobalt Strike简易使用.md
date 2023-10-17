# Cobalt Strike简易使用

## 一.介绍

Cobalt Strike是一款内网渗透神器，需要java8环境。

## 二.安装与配置

Cobalt Strike是一款基于服务器的多边渗透工具，既需要在服务端，也需要在客户端部署程序。以下会采用cs4.4和4.0两个破解版本，进行演示。

### 方式一   Linux服务器，windows客户端

将原始文件分别解压到linux服务器和windows客户端上。

在Linux上修改teamserver为可执行权限，执行./teamserver linux的IP172.16.0.101 密码admin

端口可以调整

![](C:\Users\Aurora\OneDrive\桌面\Metasploit\CS0.0.1.png)

在windows可以执行创建快捷方式或者run.vbs

![](C:\Users\Aurora\OneDrive\桌面\Metasploit\C0.0.0.png)

登录

![](C:\Users\Aurora\OneDrive\桌面\Metasploit\CS0.0.2.png)

### 方式二  windows服务器，linux客户端

在此先不展示。

`java -XX:+AggressiveHeap -XX:+UseParallelGC -jar cobaltstrike.jar`

`./strat_linux.sh`

### 插件导入

在上栏Cobalt Strike找到脚本管理器加载cna脚本即可

![](C:\Users\Aurora\OneDrive\桌面\Metasploit\CS0.0.3.png)
