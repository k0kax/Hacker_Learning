# Linux的端口与防火墙设置

Linux的端口信息一般保存在etc/services目录下，需要添加执行操作权限后才能查看。

查看端口信息主要使用`lsof`和`netstat`两个命令

## 1.lsof相关命令

参考网页[Linux下 lsof 命令详解 - Linux开发那些事儿 - 博客园 (cnblogs.com)](https://www.cnblogs.com/wanng/p/lsof-cmd.html)

lsof是一个列出当前打开文件信息的小工具。

lsof -h 查看相关信息。

**lsof -i 查看端口运行的信息。**

lsof -i 4/6产看ipv4/v6的信息。

**lsof -i ：xxx端口号 查看具体端口的信息。**

lsof -i xxx进程号 查看相关进程的信息。

lsof -i@ip 查看与某个ip通信的相关文件

lsof -i@ip:port 上个命令具体到某个端口的文件

lsof -t 查看进程



## 2.netstat相关命令

> 参考https://blog.csdn.net/qq_41675254/article/details/85208057

>  参考[netstat命令详解 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/367635200)

-r：--route，显示路由表信息
-g：--groups，显示多重广播功能群组组员名单
-s：--statistics，按照每个协议来分类进行统计。默认的显示IP、IPv6、ICMP、ICMPv6、TCP、TCPv6、UDP和UDPv6 的统计信息。
-M：--masquerade，显示网络内存的集群池统计信息
-v：--verbose，命令显示每个运行中的基于公共数据链路接口的设备驱动程序的统计信息
-W：--wide，不截断IP地址
-n：进制使用域名解析功能。链接以数字形式展示(IP地址)，而不是通过主机名或域名形式展示
-N：--symbolic，解析硬件名称
-e：--extend，显示额外信息
-p：--programs，与链接相关程序名和进程的PID
-t：所有的 tcp 协议的端口
-x：所有的 unix 协议的端口
-u：所有的 udp 协议的端口
-o：--timers，显示计时器
-c：--continuous，每隔一个固定时间，执行netstat命令
-l：--listening，显示所有监听的端口
-a：--all，显示所有链接和监听端口
-F：--fib，显示转发信息库(默认)
-C：--cache，显示路由缓存而不是FIB
-Z：--context，显示套接字的SELinux安全上下文

**netstat -anp：显示系统端口使用情况**可以配可grep过滤命令。
netstat -nupl：UDP类型的端口
netstat -ntpl：TCP类型的端口

netstat -ant:

netstat -na|grep ESTABLISHED|wc -l：统计已连接上的，状态为"established"
netstat -l：只显示所有监听端口
netstat -lt：只显示所有监听**tcp**端口

## 3.关闭端口及服务

端口的开关服务主要通过iptables端口开关和进程关闭来实现

### iptables

打开端口 iptables -A INPUT -p 端口号 -j ACCEPT

关闭端口 iptables -A INPUT -p 端口号 -j DROP

### 关闭进程

找到进程

[6种查看Linux进程占用端口号的方法详解 - 云+社区 - 腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1721588)

lsof -i:端口号

ss -tnlp可以添加过滤信息。

关闭进程kill -9 进程pid

## 4.修改端口

> [Linux修改默认端口 - 玩转机器学习 - 博客园 (cnblogs.com)](https://www.cnblogs.com/shierlou-123/p/13589691.html)







