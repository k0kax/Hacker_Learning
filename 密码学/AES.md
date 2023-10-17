#密码学 #AES
## 一、背景

## 二、基本过程

分组加密

明文长度：128bit
密钥长度：128、192、256bit

数据用4 * 4的矩阵表示
1 5   9 13
2 6 10 14
3 7 11 15
4 8 12 16

### 初始变换

明文（4* 4）异或 密钥（4* 4）=A

### 9轮循环运算

#### a.字节代换

上一轮的结果A，对照s盒查找替换，如果数字是19，则替换位第一行第九列，最终得到结果B

#### b.行移位
矩阵第一行不变，第二行左移动一个字节，第三行两个字节，第四行三个字节 
得到C

#### c.列混合
矩阵（一个给定的矩阵）* C
矩阵为固定值
此处乘比较复杂,如图
https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcwMjE5MjAzMzQ2NDM2?x-oss-process=image/format.png

https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcwMjE5MjAzNzQyNTE2?x-oss-process=image/format,png

https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcwMjE5MjA0ODIyNTE3?x-oss-process=image/format,png

https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcwMjE5MjA1NjAxNjgz?x-oss-process=image/format,png

https://img-blog.csdnimg.cn/20210216120331436.png#pic_center
有限域上的域运算
行* 列 

得到结果D

#### d.轮密钥加
D异或子密钥矩阵
子密钥来自原密钥的十轮扩展

##### 密钥扩展
w1 w2  w3 w4 wi
2b  28  ab 09  
7e  ae   f7 cf
15  d2  15 4f
16  a6  88 3c

 i是否为4的倍数
 如果不是4的倍数，wi=w(i-4)异或w(i-1)
 
 如果是4的倍数，wi=w(1-4)异或T(w(i-1))

##### T函数
wi=w[i-4]异或T(w[i-1])

w[i-1]
a1
a2
a3
a4

###### 字循环
将一个字的四个字节循环左移一个字节
a2
a3
a4
a1

###### 字节代换
字循环的结果进行S盒字节代换
b1
a4
c4
d1

###### 轮常量异或
给定的轮常量

b1         01
a4 异或 00
c4          00
d1         00

得出T(W[i-1])



循环9轮
### 1轮最终轮

执行a,b,c


## 三、代码实现

> 参考https://blog.csdn.net/qq_28205153/article/details/55798628