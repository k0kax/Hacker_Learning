# Ruby初探

> ​	Metasploit是由Ruby语言编写的框架，Ruby简单易学，可以用`irb`打开交互

## 简易计算

`┌─[root@aurora-vmwarevirtualplatform]─[/home/aurora]
irb(main):001:0> 1+1
irb(main):002:0> 2*595
irb(main):003:0> 958956897/232
=> 243343
=> 364554`

## 赋值

`irb(main):008:0> a= 'hasjshjjfhsd'
=> "hasjshjjfhsd"
irb(main):009:0> b='sdasfdsf'
=> "sdasfdsf"
irb(main):010:0> a+b
=> "hasjshjjfhsdsdasfdsf"
irb(main):011:0> c=a+b
=> "hasjshjjfhsdsdasfdsf"`

## 定义方法

`irb(main):012:1* def xorops(a,b)//方法名（变量）
irb(main):013:1*   res=a^b //异或运算
irb(main):014:1*   return res//返回值
irb(main):015:0> end//结束
=> :xorops
irb(main):016:0> xorops(1123,8485448)
=> 8486443`

## 输出

`irb(main):017:0> puts('hello,world')
hello,world
=> nil//空
irb(main):018:0> print('hello,world')
hello,world=> nil
irb(main):019:0> puts 'hello,world'
hello,world
=> nil`

Metasploit的输出框架存在，`print_line. print_good.   print_status.  print_erro`函数，下面将`http_version`修改代码，查看输出函数。使用模块后，直接`edit`进行编辑，同样也可以用`vim`进行编辑修改。Ruby代码以`rb`为文件名。