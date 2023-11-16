# xxe漏洞

> 参考：[XXE(XML External Entity attack)XML外部实体注入攻击 - FreeBuf网络安全行业门户](https://www.freebuf.com/column/181064.html)

## 原理

程序在设计之初，外部的xml实体没有进行严格的控制，导致攻击者可以将恶意代码嵌入xml进行攻击。

<?xml version="1.0" encoding="utf-8"?>

<!DOCTYPE a [    <!ENTITY % name SYSTEM "file:///etc/passwd">

    <?xml version="1.0" encoding="utf-8"?>//xml声明   
    <!DOCTYPE a [<!ENTITY note "jjjjjjjj"> ]>//DTD命令
    <name>&note;</name>//XML部分
    
    
    


