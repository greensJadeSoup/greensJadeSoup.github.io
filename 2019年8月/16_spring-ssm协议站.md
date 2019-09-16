## Springboot注意事项
https://blog.csdn.net/u011753266/article/details/82787922
建立方法参考以上网址
## sql语句
mapper.xml可以不用了，直接@Select标签
## Thymeleaf
页面跳转工具
## @pathvariable和@@RequestParam
都可以拦截网页访问路径中的参数，参考以下
https://blog.csdn.net/qq_37130969/article/details/82628091
> 直接@PathVariable String data即可，不需要@PathVariable("data") String data
## springboot项目启动一会关闭，在pom.xml中添加
```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```
## MYSQL时区错误
mysql出现The server time zone value '�й���׼ʱ��' is unrecogni的解决方法 /mysql-jdbc 6.0 serverTimezone参数详解  
查看以下
https://blog.csdn.net/weixin_43400357/article/details/97916822
## @SpringApplicationConfiguration过期
用@SpringBootTest
## springboot使用log4j
由于org.springframework.boot中自带slf4j，所以在pom文件中要用Log4j替代
```pom

    <dependencies>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-logging</artifactId>
                </exclusion>
            </exclusions>
            <version>2.1.0</version>
        </dependency>

        <!--log4j代替logback-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <artifactId>org.springframework.boot</artifactId>
                    <groupId>spring-boot-starter-logging</groupId>
                </exclusion>
            </exclusions>
        </dependency>
        
        <!--log4j使用-->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.1.1</version>
        </dependency>
```
## log4j不生成文件
有可能是被slf4j替代，配置不生效，也可能是路径错误，要用绝对路径
