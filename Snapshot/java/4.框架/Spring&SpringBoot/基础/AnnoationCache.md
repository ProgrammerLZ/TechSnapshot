1. Java config

* ```java
  @Configuration 表明当前类是一个配置类
  @ImportResource 注入当前配置文件以外的其他配置进来
  @ComponentScan 用来扫描bean
  @Bean 方法若被标记，则表明返回的是一个bean
  @ConfigurationProperties
  ```

2. 定义相关

   ```
   @Compnent/@Respository/@Service
   @Controller/@RestController
   @RequestMapping
   ```

3. 注入相关

   ```
   @Autowired
   @Qualifier 根据名字进行注入
   @Resource 根据名字进行注入
   @Value
   ```

   

