#vulnhub #缓冲区溢出 #工具使用 #immunity_debugger #汇编 #寄存器 #溢出 #漏洞利用
## 一、介绍

vulnhub一款以缓冲区溢出漏洞为主的靶机
## 二、安装与环境
下载导入即可，网络连接设置为net模式
环境
* 攻击机：kali
* 靶机：brainpan
* 中间机：win10
## 三、基本流程
### 1.主机存活扫描
```bash
nmap -sn 192.168.17.0/24

Starting Nmap 7.92 ( https://nmap.org ) at 2023-05-07 07:33 EDT
Nmap scan report for 192.168.17.1
Host is up (0.00067s latency).
MAC Address: 00:50:56:C0:00:08 (VMware)
Nmap scan report for 192.168.17.2
Host is up (0.00022s latency).
MAC Address: 00:50:56:F2:CB:C5 (VMware)
Nmap scan report for 192.168.17.128
Host is up (0.00021s latency).
MAC Address: 00:0C:29:82:40:95 (VMware)
Nmap scan report for 192.168.17.129
Host is up (0.00025s latency).
MAC Address: 00:0C:29:CA:9B:56 (VMware)
Nmap scan report for 192.168.17.254
Host is up (0.00017s latency).
MAC Address: 00:50:56:F5:94:9F (VMware)
Nmap scan report for 192.168.17.130
Host is up.
Nmap done: 256 IP addresses (6 hosts up) scanned in 10.21 seconds
                                                                        
```
130为kali
128为win10
129为brainpan

### 2.基本信息扫描
***端口开放扫描***
```bash
nmap --min-rate 10000 -p- 192.168.17.129

┌──(root㉿kali)-[~]
└─# nmap --min-rate 10000 -p-  192.168.17.129
Starting Nmap 7.92 ( https://nmap.org ) at 2023-05-07 07:36 EDT
Nmap scan report for 192.168.17.129
Host is up (0.0041s latency).
Not shown: 65533 closed tcp ports (reset)
PORT      STATE SERVICE
9999/tcp  open  abyss
10000/tcp open  snet-sensor-mgmt
MAC Address: 00:0C:29:CA:9B:56 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 5.54 seconds

```

发现开放9999、10000端口


***端口服务扫描***
```bash
┌──(root㉿kali)-[~]
└─# nmap -sT -sV -O -p9999,10000 192.168.17.129     
Starting Nmap 7.92 ( https://nmap.org ) at 2023-05-07 07:38 EDT
Nmap scan report for 192.168.17.129
Host is up (0.00079s latency).

PORT      STATE SERVICE VERSION
9999/tcp  open  abyss?
10000/tcp open  http    SimpleHTTPServer 0.6 (Python 2.7.3)
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port9999-TCP:V=7.92%I=7%D=5/7%Time=64578DCD%P=x86_64-pc-linux-gnu%r(NUL
SF:L,298,"_\|\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20_\|\x20\x20\x20\x20\
SF:x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\n_\|_\|_\|\x20\x20\x20\x20_\|\x20\x20_\|_\|\x20\x20\x20\x20_\|_\|_\|\
SF:x20\x20\x20\x20\x20\x20_\|_\|_\|\x20\x20\x20\x20_\|_\|_\|\x20\x20\x20\x
SF:20\x20\x20_\|_\|_\|\x20\x20_\|_\|_\|\x20\x20\n_\|\x20\x20\x20\x20_\|\x2
SF:0\x20_\|_\|\x20\x20\x20\x20\x20\x20_\|\x20\x20\x20\x20_\|\x20\x20_\|\x2
SF:0\x20_\|\x20\x20\x20\x20_\|\x20\x20_\|\x20\x20\x20\x20_\|\x20\x20_\|\x2
SF:0\x20\x20\x20_\|\x20\x20_\|\x20\x20\x20\x20_\|\n_\|\x20\x20\x20\x20_\|\
SF:x20\x20_\|\x20\x20\x20\x20\x20\x20\x20\x20_\|\x20\x20\x20\x20_\|\x20\x2
SF:0_\|\x20\x20_\|\x20\x20\x20\x20_\|\x20\x20_\|\x20\x20\x20\x20_\|\x20\x2
SF:0_\|\x20\x20\x20\x20_\|\x20\x20_\|\x20\x20\x20\x20_\|\n_\|_\|_\|\x20\x2
SF:0\x20\x20_\|\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20_\|_\|_\|\x20\x20_\
SF:|\x20\x20_\|\x20\x20\x20\x20_\|\x20\x20_\|_\|_\|\x20\x20\x20\x20\x20\x2
SF:0_\|_\|_\|\x20\x20_\|\x20\x20\x20\x20_\|\n\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20_\|\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\n\x20\x20\x20\x20\x20\x20\x20
SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x
SF:20\x20_\|\n\n\[________________________\x20WELCOME\x20TO\x20BRAINPAN\x2
SF:0_________________________\]\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20ENTER\x2
SF:0THE\x20PASSWORD\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\n\n\x
SF:20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20\x20\x20\x20\x20\x20>>\x20");
MAC Address: 00:0C:29:CA:9B:56 (VMware)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 2.6.X|3.X
OS CPE: cpe:/o:linux:linux_kernel:2.6 cpe:/o:linux:linux_kernel:3
OS details: Linux 2.6.32 - 3.10
Network Distance: 1 hop

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 38.55 seconds

```
发现10000是python搭建的简单web服务

***漏洞扫描***
```bash
nmap --script vuln -p9999,10000 192.168.17.129

┌──(root㉿kali)-[~]
└─# nmap --script vuln -p9999,10000 192.168.17.129
Starting Nmap 7.92 ( https://nmap.org ) at 2023-05-07 07:45 EDT
Nmap scan report for 192.168.17.129
Host is up (0.00082s latency).

PORT      STATE SERVICE
9999/tcp  open  abyss
10000/tcp open  snet-sensor-mgmt
MAC Address: 00:0C:29:CA:9B:56 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 10.89 seconds


```

没发现什么东西

### 3.目录爆破

使用dirsearch进行目录爆破
`
```bash
┌──(root㉿kali)-[~]
└─# dirsearch  -u http://192.168.17.129:10000

  _|. _ _  _  _  _ _|_    v0.4.2
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 30 | Wordlist size: 10927

Output File: /root/.dirsearch/reports/192.168.17.129-10000/_23-05-07_07-52-22.txt

Error Log: /root/.dirsearch/logs/errors-23-05-07_07-52-22.log

Target: http://192.168.17.129:10000/

[07:52:34] Starting: 
[07:53:58] 200 -  230B  - /bin/                                                                               
[07:53:58] 301 -    0B  - /bin  ->  /bin/                                    
[07:54:07] 200 -  215B  - /index.html                                        
                                                                             
Task Completed

```

只发现了一个bin目录
### 4.网站
首页
```html
┌──(root㉿kali)-[~]
└─# curl http://192.168.17.129:10000
<html>
<body bgcolor="ffffff">
<center>
<!-- infographic from http://www.veracode.com/blog/2012/03/safe-coding-and-software-security-infographic/ -->
<img src="soss-infographic-final.png">
</center>
</body>
</html>

```


只有一张图片
bin目录
```html
┌──(root㉿kali)-[~]
└─# curl http://192.168.17.129:10000/bin/
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 3.2 Final//EN"><html>
<title>Directory listing for /bin/</title>
<body>
<h2>Directory listing for /bin/</h2>
<hr>
<ul>
<li><a href="brainpan.exe">brainpan.exe</a>
</ul>
<hr>
</body>
</html>
          
```

发现一个程序,下载下来
```shell
wget http://192.168.17.129:10000/bin/brainpan.exe
```

9999端口
![[Pasted image 20230507200946.png]]

不知道这是干啥的
### 5.程序分析

下载下来的是个exe程序，先对程序进行安全性测试




