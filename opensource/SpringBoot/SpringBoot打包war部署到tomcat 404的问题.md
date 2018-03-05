#SpringBoot打包war部署到tomcat 404的问题

##问题
将spring boot 打包war放到tomcat启动，没有报错，但是没有初始化spring boot项目
访问项目的时候404报错，

## 原因
maven 配置jdk版本和tomcat 版本不一致

```
<build>
  <plugins>
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
    </plugin>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.1</version>
      <configuration>
      <!--和服务器jdk版本一致即可-->
        <source>1.8</source>
        <target>1.8</target>
      </configuration>
    </plugin>
  </plugins>
</build>
```
