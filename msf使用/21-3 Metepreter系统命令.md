# Meterpreter-系统命令

   clearev       Clear the event log//清除系统日志，主要为了防止留下日志记录
    drop_token    Relinquishes any active impersonation token.
    execute       Execute a command
    getenv        Get one or more environment variable values
    getpid        Get the current process identifier//获取当前的进程标识符。
    getprivs      Attempt to enable all privileges available to the current process//获取当前用户进程随对应的权限
    getsid        Get the SID of the user that the server is running as//获取当前用户的sid
    getuid        Get the user that the server is running as//获取当前服务的用户信息
    kill          Terminate a process//关闭进程 kill 进程pid 即可关闭某个进程。
    localtime     Displays the target system local date and time//获取靶机所在的时区时间
    pgrep         Filter processes by name//过滤命令，常常配合 | grep 过滤内容。
    pkill         Terminate processes by name//类似于kill,根据程序名关闭程序。
    ps            List running processes//列出当前的进程。相当于windows的任务控制器。
    reboot        Reboots the remote computer//重启
    reg           Modify and interact with the remote registry//注册表相关操作操作。这方面命令比较多。
    rev2self      Calls RevertToSelf() on the remote machine
    shell         Drop into a system command shell//进入shell，进入目标的命令行，不同系统的编码方式存在差异会造成乱码，如果linux转win需要进入shell输入chcp 65001进行编码方式调节。

shutdown      Shuts down the remote computer//关机
    steal_token   Attempts to steal an impersonation token from the target process
    suspend       Suspends or resumes a list of processes//挂起或恢复进程。

​                    suspend xxpid挂起程序  无法使用。

​                    suspend -r  xxpid恢复程序
​    sysinfo       Gets information about the remote system, such as OS//获取系统信息



















