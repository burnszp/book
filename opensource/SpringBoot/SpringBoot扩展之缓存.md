# SpringBoot扩展之缓存

## 配置pom.xml

```xml
<dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-cache</artifactId>
          <version>${spring-boot-version}</version>
</dependency>
```

## ApplicationStart启用缓存注解:@EnableCaching
```java
@SpringBootApplication
@EnableCaching
public class ApplicationStart extends SpringBootServletInitializer {

    public static void main(String[] args) {
        SpringApplication.run(ApplicationStart.class, args);
    }
}
```

## Repository增加缓存注解：
```java
@Repository
@CacheConfig
public interface CityRepository extends PagingAndSortingRepository<City, Long> {
    @Cacheable(cacheNames = "findByIdCountry")
    List<City> findByIdCountry(Long idCountry);
    @Cacheable(cacheNames = "countByIdCountry")
    int countByIdCountry(Long id);
    @Cacheable(cacheNames = "findByName")
    City findByName(String cityName);
}
```
