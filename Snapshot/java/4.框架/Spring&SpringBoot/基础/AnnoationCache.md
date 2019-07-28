1. Java config

@Configuration 表明当前类是一个配置类 

@ImportResource 注入当前配置文件以外的其他配置进来

@ComponentScan 用来扫描bean，默认会扫描当前配置类所在的包，是SpringBoot魔法得以实现的关键组件

@Bean 方法若被标记，则表明返回的是一个bean

@ConfigurationProperties

@PropertySource与@PropertySources



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

   

