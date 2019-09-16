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
