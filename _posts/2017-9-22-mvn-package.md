---
title: mvn package javax.crypto does not exist
date: 2017-9-22 15:30:09
categories:
- Other
comments: true
---

# 问题现象
有一次同事提交了代码Jenkins平台自动打包失败，而同事在本地是Eclipse开发环境使用```mvn tomcat7:run``` 是可以成功运行项目的。   
区别是Jenkins平台使用```mvn clean package```打包。错误说明是挺明显的**package javax.crypto does not exist**，但是在开发环境中没有任何问题，build path中包含了jre目录自然能够引用到jce.jar中的类。

```text
[INFO] 49 errors
[INFO] -------------------------------------------------------------
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary:
[INFO]
[INFO] services-consumer .......................... SUCCESS [  0.874 s]
[INFO] services-consumer-rest ..................... FAILURE [  2.240 s]
[INFO] job-consumer ............................... SKIPPED
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 34.749 s
[INFO] Finished at: 2017-09-22T15:39:10+08:00
[INFO] Final Memory: 127M/430M
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:2.3.2:compile (default-compile) on project services-consumer-rest: Compilation failure: Compilation failure:
[ERROR] /Users/ervices-consumer-rest/src/main/java/com/test/portal/utils/RSAUtils.java:[18,19] error: package javax.crypto does not exist
[ERROR] /Users/services/services-consumer-rest/src/main/java/com/test/portal/utils/DesUtil.java:[11,19] error: package javax.crypto does not exist
```

报错的RSAUtils.java文件内容，包引用部分

```java
import javax.crypto.BadPaddingException;
import javax.crypto.Cipher;
import javax.crypto.IllegalBlockSizeException;
import javax.crypto.NoSuchPaddingException;
import javax.crypto.SecretKey;
import javax.crypto.SecretKeyFactory;
import javax.crypto.spec.DESKeySpec;
import javax.crypto.spec.IvParameterSpec;
```

# 问题分析

1. 怀疑Jenkins平台问题
为了验证这个怀疑，我同步了最新的代码在本地使用```mvn clean package``` 得到同样的错误信息，证明这个怀疑不成立。

2. Maven编译环境JDK问题
既然IDE开发环境中编译时没有问题的，而使用```mvn compile``` ```package``` ```install``` 都报错很容易联想到mvn的运行时环境中没有引用到jce.jar。

***<font color="#FF0099"> 那么问题就变成了Maven是如何指定编译环境的？ </font>***

请参见官方文档 http://maven.apache.org/plugins/maven-compiler-plugin/index.html

# 解决方法   
maven 是通过插件来管理编译的，所以在pom.xml的```maven-compiler-plugin```可以设定编译器的lib包路径，
添加上${java.home}/lib/jce.jar

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>  
      <artifactId>maven-compiler-plugin</artifactId>  
      <version>${maven_compiler_plugin_version}</version>  
      <configuration>
        <source>1.7</source>  
        <target>1.7</target>  
        <compilerArguments>
          <bootclasspath>${java.home}/lib/rt.jar:${java.home}/lib/jce.jar</bootclasspath>
        </compilerArguments>
      </configuration>
    </plugin>
  </plugins>
</build>

```
