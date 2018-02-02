# spring boot入门

## maven依赖

```xml
<properties>
       <spring-boot-version>1.5.9.RELEASE</spring-boot-version>
       <junit-version>4.12</junit-version>
       <springframework-version>4.3.5.RELEASE</springframework-version>
       <swagger-version>2.2.2</swagger-version>
       <mysql-version>5.1.24</mysql-version>
       <spring-data-jpa-version>2.0.2.RELEASE</spring-data-jpa-version>
</properties>
<dependencies>

     <!--spring boot -->
     <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-web</artifactId>
         <version>${spring-boot-version}</version>
     </dependency>
     <!-- spring data jpa-->
     <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-data-jpa</artifactId>
         <version>${spring-boot-version}</version>
     </dependency>  
     <!--mysql-->
     <dependency>
         <groupId>mysql</groupId>
         <artifactId>mysql-connector-java</artifactId>
         <version>${mysql-version}</version>
     </dependency>
     <!--mongo-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-data-mongodb</artifactId>
           <version>${spring-boot-version}</version>
       </dependency>
     <!-- swagger2 start -->
      <dependency>
          <groupId>io.springfox</groupId>
          <artifactId>springfox-swagger2</artifactId>
          <version>${swagger-version}</version>
      </dependency>
      <dependency>
          <groupId>io.springfox</groupId>
          <artifactId>springfox-swagger-ui</artifactId>
          <version>${swagger-version}</version>
      </dependency>
     <!-- test start -->
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-test</artifactId>
          <version>${springframework-version}</version>
          <scope>test</scope>
      </dependency>
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>${junit-version}</version>
          <scope>test</scope>
      </dependency>
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-test</artifactId>
          <version>${spring-boot-version}</version>
          <scope>test</scope>
      </dependency>
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>RELEASE</version>
          <scope>test</scope>
      </dependency>
</dependencies>     
<build>
      <finalName>ROOT</finalName>
      <plugins>
          <plugin>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-maven-plugin</artifactId>
              <configuration>
                  <fork>true</fork>
              </configuration>
          </plugin>
          <plugin>
              <artifactId>maven-compiler-plugin</artifactId>
              <configuration>
                  <source>1.8</source>
                  <target>1.8</target>
              </configuration>
          </plugin>
          <plugin>
              <artifactId>maven-war-plugin</artifactId>
              <version>2.6</version>
              <configuration>
                  <!--如果想在没有web.xml文件的情况下构建WAR，请设置为false。-->
                  <failOnMissingWebXml>false</failOnMissingWebXml>
              </configuration>
          </plugin>
      </plugins>
</build>
```

## 配置

```properties
server.port=8001
logging.level.root=INFO
## mongodb配置
spring.data.mongodb.uri=mongodb://localhost:27017/test
## mysql配置
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://db3:7749/zhihui
spring.datasource.username=zhihui
spring.datasource.password=123456@zhi
spring.jpa.database-platform=org.hibernate.dialect.MySQL5Dialect

```


## 启动类
```java
@SpringBootApplication
public class ApplicationStart extends SpringBootServletInitializer {

    public static void main(String[] args) {
        SpringApplication.run(ApplicationStart.class, args);
    }
}
```
## 常用配置
通过@Configuration配置类扩展功能

### CORS跨域访问
```java
@Configuration
public class CORSConfiguration {
    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurerAdapter() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**")
                        .allowedHeaders("*")
                        .allowedMethods("*")
                        .allowedOrigins("*");
            }
        };
    }
}
```
### Swagger在线文档生成
```java
@Configuration
@EnableSwagger2
public class Swagger2Configuration {
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("cn.enilu.elm.api.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("文档标题")
                .description("文档描述")
                .termsOfServiceUrl("www.enilu.cn")
                .contact("eniluzt@qq.com")
                .version("1.0")
                .build();
    }
}
```


### 读取application.properties中自定义的配置
比如在application.properties增加如下配置
```properties
api.qq.get.location=http://apis.map.qq.com/ws/location/v1/ip
api.qq.search.place=http://apis.map.qq.com/ws/place/v1/search
## 图片地址
img.dir=/data/springboot-elm/img/
```
可以通过构建java实体类读取配置文件，然后在使用的地方注入该java实体即可。
- java配置实体

```java
@Component
public class AppConfiguration {
    @Value("${api.qq.get.location}")
    private String apiQqGetLocation;
    @Value("${api.qq.search.place}")
    private String apiQqSearchPlace;   
    @Value("${img.dir}")
    private String imgDir;
    setter...
    getter...
}
```

- 调用放的调用方法

```java
@Autowired
private AppConfiguration appConfiguration;
 void test(){
   String imgDir = appConfiguration.getImgDir();
 }
```
