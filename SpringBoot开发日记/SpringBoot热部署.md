使用IDEA平台进行部署

修改pom.xml
添加
```xml
<!--热部署，进行动态文件目录生成-->  
<dependency>  
<groupId>org.springframework.boot</groupId>  
<artifactId>spring-boot-devtools</artifactId>  
<optional>true</optional>  
</dependency>
```

修改application.properties，添加
```
spring.devtools.restart.enabled=true  
spring.devtools.restart.additional-paths=src/main/java
```

修改IDEA

设置/编译器/自动构建项目

![[../../Pasted image 20230710171023.png]]
设置/高级设置/

![[../../Pasted image 20230710165715.png]]
