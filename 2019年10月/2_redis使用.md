## redis独自使用
+构建实体类User implements Serializable 
```java
private static final long serialVersionUID = 3529219554011221820L;
private String name;
private Integer age;
```
+ RedisConfig类继承
```java
/**
   * 配置自定义redisTemplate
   *
   * @return
   */
  @Bean
  RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {

      RedisTemplate<String, Object> template = new RedisTemplate<>();
      template.setConnectionFactory(redisConnectionFactory);

      //使用Jackson2JsonRedisSerializer来序列化和反序列化redis的value值
      Jackson2JsonRedisSerializer serializer = new Jackson2JsonRedisSerializer(Object.class);

      ObjectMapper mapper = new ObjectMapper();
      mapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
      mapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
      serializer.setObjectMapper(mapper);

      template.setValueSerializer(serializer);
      //使用StringRedisSerializer来序列化和反序列化redis的key值
      template.setKeySerializer(new StringRedisSerializer());
      template.setHashKeySerializer(new StringRedisSerializer());
      template.setHashValueSerializer(serializer);
      template.afterPropertiesSet();
      return template;
  }
```
+ 配置application.yml添加
```java
server:
  port: 8080
spring:
  redis:
    host: 127.0.0.1
    port: 6379
    # 密码 没有则可以不填
    password:
    # 如果使用的jedis 则将lettuce改成jedis即可
    database: 1
    lettuce:
      pool:
        # 最大活跃链接数 默认8
        max-active: 5
        # 最大空闲连接数 默认8
        max-idle: 10
        # 最小空闲连接数 默认0
        min-idle: 0
```
+ 测试类
```java
// redis存储数据
String key = "name";
redisTemplate.opsForValue().set(key, "qiuwei");
// 获取数据
String value = (String) redisTemplate.opsForValue().get(key);
System.out.println("key:" + key + " ,value：" + value);

User user = new User("qw",12);

String userKey = "qiuwei";
redisTemplate.opsForValue().set(userKey, user);
User newUser = (User) redisTemplate.opsForValue().get(userKey);
System.out.println("key:" + userKey + " ,value：" + newUser);

```
+ 参考https://www.cnblogs.com/uptothesky/p/8215117.html
