# Meterpreter-网络命令

1. arp缓存 Display the host ARP cache

   直接输入`arp`即可获取arp缓存列表，可以查看局域网通信的相关信息，包括ip mac地址

2.   getproxy 代理   Display the current proxy configuration

3. ​    ifconfig   网络接口信息   Display interfaces ，包括一些内外网的信息。

4. ​    ipconfig      Display interfaces

5. ​    netstat    网络连接信息   Display the network connections

6. __portfwd 端口重定向 __ Forward a local port to a remote service

   `Usage: portfwd [-h] [add添加 | delete 删除| list列表 | flush删除] [args]

   ​      -h   Help banner.
   ​    -i   Index of the port forward entry to interact with (see the "list" command).
   ​    -l   Forward: local port to listen on. Reverse: local port to connect to.//本地端口号
   ​    -L   Forward: local host to listen on (optional). Reverse: local host to connect to.本地监听及远程连接的端口
   ​    -p   Forward: remote port to connect to. Reverse: remote port to listen on.//远程端口号
   ​    -r   Forward: remote host to connect to.//远程ip
   ​    -R   Indicates a reverse port forward.//取消端口重定向  `

   `portfwd add  -I 3389 -p 3389 -r 192.168.0.100`将本机的3389端口与远程主机的3389端口连接。

   相关内容见附加篇。

7. ​    resolve       Resolve a set of host names on the target

   

8.   **route路由信息   **      View and modify the routing table

   路由交换主要用于内网渗透，从拿下的一个靶机攻击其他内网内存在的机器。
   
   route 输出路由表，建立会话后，将sessions 置于后台，即可进行以下操作。
   
   添加/删除 路由 add/delete 子网 子网掩码 网关
   
   1.添加会话  `route add 172.16.0.0/24 sessionid`
   
   ![](C:\Users\Aurora\OneDrive\桌面\Metasploit\21.2.0.png)
   
   2.使用arp进行局域网主机发现
   
   
   
   ![](C:\Users\Aurora\OneDrive\桌面\Metasploit\21.2.1.png)
   
   ![](C:\Users\Aurora\OneDrive\桌面\Metasploit\21.2.2.png)

3.之后就可以通过win2k8对内网做进一步扫描和利用。

4.使用hushdump，获取哈希值

![](C:\Users\Aurora\OneDrive\桌面\Metasploit\21.2.3.png)

5.使用`exploit/windows/smb/psexec`设置好哈希，运行，即可获取另一张网卡权限。

![](C:\Users\Aurora\OneDrive\桌面\Metasploit\21.2.4.png)
