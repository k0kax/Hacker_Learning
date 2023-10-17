## Windows后渗透的一些模块使用

### 安装应用程序查找

`post/windows/gather/enum_applications`

### 快捷方式查询

`post/windows/gather/dumplinks`

### 最近登录用户

通过数据表读取获得

`post/windows/gather/enum_logg`

### 共享文件夹

`post/windows/gather/enum_share`

### hash值

`post/windows/gather/hashdump`

### usb使用记录

`post/windows/gather/usb_history`

### 其他

一堆有意思的模块存在`post/windows/manage`下酌情使用。

使用info 编号可以查看相关信息。

## 其他2

在获取会话后可以运行以下模块进一步查询数据

`checkvm//虚拟机检查getcountermeasure//脚本检查受害者系统上的安全配置，并可以禁用其他安全措施，例如A/V，防火墙等等`

`getgui//远程桌面`

`get_local_subnets//脚本用于获取受害者的本地子网掩码`

` gettelnet //用于在受害者禁用时启用telnet` 

`hostsedit//由于Windows将首先检查主机文件而不是配置的DNS服务器，它将有助于将流量转向假冒的条目或条目。既可以提供单个条目，也可以为每行包含一个条目的文件提供一系列条目` 

` killav//关闭杀软` 

` remotewinenum //通过wmic向受害者列举系统信息。记下日志的存储位置` 

`scraper//可以获取更多的系统信息，包括整个注册表 `

`winenum//提供了一个非常详细的窗口枚举工具。它会丢弃令牌，哈希等等`

### kiwi域渗透

` creds_all              Retrieve all credentials (parsed)//列举认证信息
    creds_kerberos         Retrieve Kerberos creds (parsed)
    creds_livessp          Retrieve Live SSP creds
    creds_msv              Retrieve LM/NTLM creds (parsed)
    creds_ssp              Retrieve SSP creds
    creds_tspkg            Retrieve TsPkg creds (parsed)
    creds_wdigest          Retrieve WDigest creds (parsed)
    dcsync                 Retrieve user account information via DCSync (unparsed)
    dcsync_ntlm            Retrieve user account NTLM hash, SID and RID via DCSync
    golden_ticket_create   Create a golden kerberos ticket
    kerberos_ticket_list   List all kerberos tickets (unparsed)
    kerberos_ticket_purge  Purge any in-use kerberos tickets
    kerberos_ticket_use    Use a kerberos ticket
    kiwi_cmd               Execute an arbitary mimikatz command (unparsed)
    lsa_dump_sam           Dump LSA SAM (unparsed)
    lsa_dump_secrets       Dump LSA secrets (unparsed)
    password_change        Change the password/hash of a user
    wifi_list              List wifi profiles/creds for the current user
    wifi_list_shared       List shared wifi profiles/creds (requires SYSTEM)`

### python插件

 ` python_execute  Execute a python command string//单指令
    python_import   Import/run a python file or module//多指令
    python_reset    Resets/restarts the Python interpreter//重设`





