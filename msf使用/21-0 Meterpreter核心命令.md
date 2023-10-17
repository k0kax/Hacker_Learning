# Meterpreter-核心命令

> `meterpreter`会话是在获取部分权限后解锁。具体命令并不并不是很多，依据获取权限的大小，可以使用的命令也存在差异，权限越高，解锁的命令更多，操作空间也更大。

## 基础命令

` Command       Description`
`    -------       -----------`
  `  ?             Help menu//帮助菜单
    banner        Display an awesome metasploit banner//版本及载荷的数量信息
    cd            Change the current working directory//打开文件夹
    color         Toggle color//切换颜色
    connect       Communicate with a host//连接主机
    debug         Display information useful for debugging//调试有用信息
    exit          Exit the console//推出命令行
    features      Display the list of not yet released features that can be opted in to//
    get           Gets the value of a context-specific variable
    getg          Gets the value of a global variable
    grep          Grep the output of another command//过滤命令
    help          Help menu//帮助菜单
    history       Show command history
    load          Load a framework plugin//加载插件
    quit          Exit the console//推出命令行
    repeat        Repeat a list of commands
    route         Route traffic through a session//路由信息
    save          Saves the active datastores//
    sessions      Dump session listings and display information about sessions//查看sessions
    set           Sets a context-specific variable to a value//设置相关参数
    setg          Sets a global variable to a value//设置全局参数*************************************************重点
    sleep         Do nothing for the specified number of seconds
    spool         Write console output into a file as well the screen
    threads       View and manipulate background threads//线程
    tips          Show a list of useful productivity tips//展示细节操作
    unload        Unload a framework plugin//推出插件
    unset         Unsets one or more context-specific variables
    unsetg        Unsets one or more global variables
    version       Show the framework and console library version numbers//展示版本`







## 核心命令

在其最基本的使用，·`meterpreter` ·是一个 Linux 终端在受害者的计算机上。这样，我们的许多基本的Linux命令可以用在`meterpreter`甚至是在一个窗口或其他操作系统。

这里有一些核心的命令可以用在`meterpreter`。

1. ? – 帮助菜单

2. background – 将当前会话移动到背景也可以CTRL+Z +y置于后台

3. `bgkill` – 杀死一个背景` meterpreter` 脚本

4. `bglist `– 提供所有正在运行的后台脚本的列表

5. `bgrun` – 作为一个后台线程运行脚本

6. channel – 显示活动频道 通道管理，可以开启多个shell通道

   `meterpreter > channel`

   ```linux
   -c   Close the given channel.//关闭channel
   -h   Help menu.
   -i   Interact with the given channel.//切换channel
   -k   Close the given channel.
   -K   Close all channels.
   -l   List active channels.//列出channel
   -r   Read from the given channel.
   -w   Write to the given channel.
   ```

   进入shell后可以使用ctrl+z置于后台配合channel命令进行操作。

7. close – 关闭通道

8. exit – 终止` meterpreter `会话

9. help – 帮助菜单

10. interact – 与通道进行交互

11. `irb `– 进入 Ruby 脚本模式

12. migrate – 移动到一个指定的 PID 的活动进程 

    ![](C:\Users\Aurora\OneDrive\桌面\Metasploit\21.0.1.png)

    控制进程（1096）如果太明显会被轻易发现，可以迁移到其他进程下。直接输入命令 `migrate 1828不易被发现的进程`

13. quit – 终止 `meterpreter `会话

14. read – 从通道读取数据

15. run – 执行以后它选定的 `meterpreter` 脚本

    运行虚拟机检测`run post/windows/gather/chekvm`

    ![](C:\Users\Aurora\OneDrive\桌面\Metasploit\21.0.2.png)

16. use – 加载 `meterpreter` 的扩展

17. write – 将数据写入到一个通道
