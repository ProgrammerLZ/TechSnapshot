[TOC]

# MyBatis Plus 基础篇

## 一、简介

MyBatis 是一个半自动的 ORM 框架。

[MyBatis-Plus](https://github.com/baomidou/mybatis-plus)（简称 MP）是一个 [MyBatis](http://www.mybatis.org/mybatis-3/) 的 <font color='red'>增强工具</font>，在 MyBatis 的基础上**只做增强不做改变，为简化开发、提高效率而生**。

【提示】课程按照当前**最新的版本 3.1.1**。

![1557127576570](assets/1557127576570.png)

### 拥有强大的特性

1）**无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑

2）**损耗小**：启动即会<font color='red'>自动注入基本 CURD</font>，性能基本无损耗，直接面向对象操作

3）**强大的 CRUD 操作**：<font color='red'>内置通用 Mapper、通用 Service</font>，仅仅通过少量配置即可实现单表大部分 CRUD 操作，<font color='red'>更有强大的条件构造器</font>，满足各类使用需求

4）**支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错

5）**支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer2005、SQLServer 等多种数据库

6）**支持主键自动生成**：支持多达<font color='red'> 4 种主键策略</font>（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题

7）**支持 XML 热加载**：Mapper 对应的 XML 支持热加载，对于简单的 CRUD 操作，甚至可以无 XML 启动，<font color='red'>但最新版已经移除了</font>

8）**支持 ActiveRecord 模式**：<font color='red'>支持 ActiveRecord 形式调用</font>，实体类只需继承 Model 类即可进行强大的 CRUD 操作。ActiveRecord 主要是一种数据访问设计模式，可以实现数据对象到关系数据库的映射。

9）**支持自定义全局通用操作**：支持全局通用方法注入 <font color='red'>Write once, use anywhere </font>

10）**支持关键词自动转义**：支持数据库关键词（order、key......）自动转义，还可自定义关键词

11）**内置代码生成器**：采用代码或者 Maven 插件<font color='red'>可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码</font>，支持模板引擎，更有超多自定义配置等您来使用

12）**内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，<font color='red'>写分页等同于普通 List 查询</font>

13）**内置性能分析插件**：可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询

14）**内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

15）**内置 Sql 注入剥离器**：支持 Sql 注入剥离，<font color='red'>有效预防 Sql 注入攻击</font>

## 二、框架结构

![架构02](assets/架构02.png)

## 三、基本开发环境

1. 基于 **Java** 开发，建议 jdk 1.8+ 版本。
2. 需要使用到 **Spring Boot**
3. 需要使用到 **Maven**
4. 需要使用 **MySQL**

### 1. 准备数据

我们需要准备一张 user 表

```sql
DROP TABLE IF EXISTS user;

CREATE TABLE user
(
	id BIGINT(20) NOT NULL COMMENT '主键ID',
	name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
	age INT(11) NULL DEFAULT NULL COMMENT '年龄',
	email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY (id)
);
```

然后给 user 表中添加一些数据

```sql
DELETE FROM user;

INSERT INTO user (id, name, age, email) VALUES
(1, '翠花', 18, 'cuihua@kkb.com'),
(2, '春花', 20, 'chunhua@kkb.com'),
(3, '花花', 28, 'huahua@kkb.com'),
(4, '翠翠', 21, 'cuicui@kkb.com'),
(5, '春春', 24, 'chunchun@kkb.com');
```

### 2. Hello World

> **我们的目的**：通过几个简单的步骤，我们就实现了 User 表的 CRUD 功能，甚至连 XML 文件都不用编写！

#### 第一步：创建一个 Spring Boot 项目

​	推荐，登录 https://start.spring.io 构建并下载一个干净的 Spring Boot 工程。

#### 第二步：编辑 pom.xml 文件添加相关的依赖

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.1.4.RELEASE</version>
</parent>

<dependencies>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.8</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.1.1</version>
    </dependency>    
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
        <version>1.1.10</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.47</version>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

【注意】<font color=red>引入 `MyBatis-Plus` 之后请不要再次引入 `MyBatis` 以及 `MyBatis-Spring`</font>，以避免因版本差异导致的问题。

【如果是 SpringMVC 项目】

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus</artifactId>
    <version>3.1.1</version>
</dependency>
```

#### 第三步：配置 application.yml 文件

编辑 application.yml 配置文件，主要是添加 druid 数据库的相关配置：

```yaml
# DataSource Config
spring:
  datasource:
    # 此处使用 druid 数据源
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/aaa?allowMultiQueries=true&useUnicode=true&characterEncoding=UTF-8
    username: root
    password: 1234
    filters: stat
    maxActive: 20
    initialSize: 1
    maxWait: 60000
    minIdle: 1
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: select 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true
    maxOpenPreparedStatements: 20
```

在 Spring Boot 启动类中添加 `@MapperScan` 注解，用于扫描 Mapper 文件夹：

```java
@SpringBootApplication
@MapperScan("com.kkb.mapper")
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(QuickStartApplication.class, args);
    }

}
```

【注意】如果是 SpringMVC 项目，需要在 ```<bean>``` 标签中配置 MapperScan。

```xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.kkb.mapper"/>
</bean>
```

然后还可以调整 SqlSessionFactory 为 Mybatis-Plus 的 SqlSessionFactory。

```xml
<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
</bean>
```

#### 第四步：创建对应的类

新建 User 类：

```java
@Data
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```

【注意】<font color=red>@Data 是 Lombok 提供的能让代码更加简洁的注解</font>，能够减少大量的模板代码。注解在类上，为类提供读写属性，此外还提供了 equals()、hashCode()、toString() 方法。

如果在 Eclipse 中使用，需要我们手动安装，具体请参考 https://www.projectlombok.org/setup/eclipse

【参考：Lombok 常用注解】

```
@NonNull : 注解在参数上, 如果该类参数为 null , 就会报出异常,  throw new NullPointException(参数名)
@Cleanup : 注释在引用变量前, 自动回收资源 默认调用 close() 方法
@Getter/@Setter : 注解在类上, 为类提供读写属性
@Getter(lazy=true) :
@ToString : 注解在类上, 为类提供 toString() 方法
@EqualsAndHashCode : 注解在类上, 为类提供 equals() 和 hashCode() 方法
@NoArgsConstructor, @RequiredArgsConstructor, @AllArgsConstructor : 注解在类上, 为类提供无参,有指定必须参数, 全参构造函数
@Data : 注解在类上, 为类提供读写属性, 此外还提供了 equals()、hashCode()、toString() 方法
@Value :
@Builder : 注解在类上, 为类提供一个内部的 Builder
@SneakThrows :
@Synchronized : 注解在方法上, 为方法提供同步锁
@Log :
@Log4j : 注解在类上, 为类提供一个属性名为 log 的 log4j 的日志对象
@Slf4j : 注解在类上, 为类提供一个属性名为 log 的 log4j 的日志对象
```

新建 UserMapper 映射接口类：

```java
public interface UserMapper extends BaseMapper<User> {
}
```

#### 第五步：愉快地测试

新建 KKBTest 测试类

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class KKBTest {

    @Autowired
    private UserMapper userMapper;

@Test
	public void testSelect(){
		
		// 此处 null 指的是不用根据参数去查询
		// 可以调用 CRUD 相关的多种方式

		// 1. 查询所有的数据
		List<User> userList = userMapper.selectList(null);
		userList.forEach(user -> System.out.println(user.getName()));
		
		// 2. 根据 id 删除
		userMapper.deleteById(1);
		
		// 3. 添加数据
		User user = new User();
		user.setName("老王");
		user.setEmail("laowang@kkb.com");
		user.setAge(18);
		userMapper.insert(user);
		
		// 4. 更新数据
		user.setName("老王王");
		user.setEmail("laowangwang@kkb.com");
		userMapper.updateById(user);
	}
    
}
```

## 四、常见注解

1. **@TableName**：表名描述
2. **@TableId**：主键注释
3. **@TableField**：字段注解（非主键）
4. **@Version**：乐观锁注解，主要用于标注在字段上
5. **@EnumValue**：通枚举类注解（注解在枚举字段上）
6. **@TableLogic**：表字段逻辑处理注解（逻辑删除）
7. **@SqlParser**：租户注解（支持注解在 mapper 上）
8. **@KeySequence**：序列主键策略，属性有：value、resultMap

### 案例：多表联查

#### 1. 准备数据

##### User 用户表（按之前的）

```sql
CREATE TABLE USER
(
	id BIGINT(20) NOT NULL COMMENT '主键ID',
	NAME VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
	age INT(11) NULL DEFAULT NULL COMMENT '年龄',
	email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY (id)
);

INSERT INTO USER (id, NAME, age, email) VALUES
(1, 'cuihua', 18, 'test1@baomidou.com'),
(2, 'huahua', 20, 'test2@baomidou.com'),
(3, 'cunhua', 28, 'test3@baomidou.com'),
(4, 'laowang', 21, 'test4@baomidou.com'),
(5, 'xiaomei', 24, 'test5@baomidou.com');
```

##### Role 角色表

```sql
CREATE TABLE role (
	id BIGINT(20) NOT NULL COMMENT '主键ID',
	rolecode VARCHAR(100) NULL DEFAULT NULL COMMENT '角色编号',
	rolename VARCHAR(100) NULL DEFAULT NULL COMMENT '角色名字',
	PRIMARY KEY (id)
 );

 INSERT INTO role(id, rolecode, rolename) VALUES 
 (1, '001', '总裁'),
 (2, '002', '院长'),
 (3, '003', '组长');
```

##### Permission 权限表

```sql
CREATE TABLE permission (
	id BIGINT(20) NOT NULL COMMENT '主键ID',
	permissioncode VARCHAR(100) NULL DEFAULT NULL COMMENT '权限编号',
	permissionname VARCHAR(100) NULL DEFAULT NULL COMMENT '权限名字',
	path VARCHAR(100) NULL DEFAULT NULL COMMENT '映射路径',
	PRIMARY KEY (id)
 );

INSERT INTO permission(id, permissioncode, permissionname) VALUES 
 (1, '111', '随便乱来'),
 (2, '222', '偶尔乱来'),
 (3, '333', '小小乱来');
```

##### UserRole 用户角色关联表

```sql
CREATE TABLE userrole (
	id BIGINT(20) NOT NULL COMMENT '主键ID',
	username VARCHAR(100) NULL DEFAULT NULL COMMENT '用户名',
	rolecode VARCHAR(100) NULL DEFAULT NULL COMMENT '角色编号',
	PRIMARY KEY (id)
 );

 INSERT INTO userrole(id, username, rolecode) VALUES 
 (1, 'cuihua', '001'),
 (2, 'chunhua', '003'),
 (3, 'huahua', '002');
```

##### RolePermission 角色权限关联表

```sql
CREATE TABLE rolepermission (
	id BIGINT(20) NOT NULL COMMENT '主键ID',
	rolecode VARCHAR(100) NULL DEFAULT NULL COMMENT '角色编号',
	permissioncode VARCHAR(100) NULL DEFAULT NULL COMMENT '权限编号',
	PRIMARY KEY (id)
 );

 INSERT INTO rolepermission(id, rolecode, permissioncode) VALUES 
 (1, '001', '111'),
 (2, '002', '222'),
 (3, '003', '333');
```

#### 2. 创建实体类

文件路径：com.kkb.pojo

##### User 类

```java
@Data
@TableName("user")
@ApiModel("用户类")
public class User implements Serializable {

    @ApiModelProperty(name = "id", value = "ID 主键")
    @TableId(type = IdType.AUTO)
    private String id;

    @ApiModelProperty(name = "name", value = "用户名")
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private String name;

    @ApiModelProperty(name = "age", value = "年龄")
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Integer age;

    @ApiModelProperty(name = "email", value = "邮箱")
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private String email;

    @TableField(exist = false)
    private Set<Role> roles = new HashSet<>();

}
```

【简单理解】代码中 @ApiModel 和 @ApiModelProperty 注解出自于 [Swagger](https://swagger.io/) 这块 API 简化开发开源框架，感兴趣的同学可以去了解一下。

在 Springboot 中使用，需要在 pom.xml 文件中引入其依赖。

```xml
<dependency>
    <groupId>com.spring4all</groupId>
    <artifactId>swagger-spring-boot-starter</artifactId>
    <version>1.7.0.RELEASE</version>
</dependency>
```
另外，也可以单独引入它的依赖，可自己指定对应或最新版本。

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>lastest version</version>
</dependency>

<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>lastest version</version>
</dependency>
```

##### Role 类

```java
@Data
@TableName("role")
@ApiModel("角色类")
public class Role implements Serializable {

    @ApiModelProperty(name = "id", value = "ID 主键")
    @TableId(type = IdType.AUTO)
    private String id;

    @ApiModelProperty(name = "roleCode", value = "角色编号")
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private String roleCode;

    @ApiModelProperty(name = "roleName", value = "角色名")
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private String roleName;

    @TableField(exist = false)
    private Set<Permission> permissions = new HashSet<>();

}
```

##### Permission 类

```java
@Data
@TableName("permission")
@ApiModel("权限类")
public class Permission implements Serializable {

    @ApiModelProperty(name = "id", value = "ID 主键")
    @TableId(type = IdType.AUTO)
    private String id;

    @ApiModelProperty(name = "permissionCode", value = "权限编号")
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private String permissionCode;

    @ApiModelProperty(name = "permissionName", value = "权限名")
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private String permissionName;

    @ApiModelProperty(name = "path", value = "映射路径")
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private String path;

}
```

##### UserRole 类

```java
@Data
@TableName("userRole")
@ApiModel("用户角色关系类")
public class UserRole implements Serializable{

    @ApiModelProperty(name = "id", value = "ID 主键")
    @TableId(type = IdType.AUTO)
    private String id;

    @ApiModelProperty(name = "username", value = "用户名")
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private String username;

    @ApiModelProperty(name = "roleCode", value = "角色编号")
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Integer roleCode;

}
```

##### RolePermission 类

```java
@Data
@TableName("rolePermission")
@ApiModel("角色权限关系类")
public class RolePermission implements Serializable{

    @ApiModelProperty(name = "id", value = "ID 主键")
    @TableId(type = IdType.AUTO)
    private String id;

    @ApiModelProperty(name = "roleCode", value = "角色编号")
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Integer roleCode;

    @ApiModelProperty(name = "permission", value = "权限编号")
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private String permissionCode;

}
```

#### 3. 创建 Mapper 接口类

文件路径：com.kkb.mapper

##### UserMapper 接口类

```java
public interface UserMapper extends BaseMapper<User> {
	
    @Select("select * from user where name = #{name}")
    public User findUserByName(String username);

    // 获取用户所拥有的角色
    @Select("select * from  role where rolecode in(select rolecode from userrole where username = #{userName})")
    public Set<Role> getUserRoles(String username);

}
```

##### RoleMapper 接口类

```java
public interface RoleMapper extends BaseMapper<User> {

    // 获取角色所拥有权限
    @Select("select * from permission where permissioncode in (select permissioncode from rolepermission where rolecode = #{roleCode})")
    public Set<Permission> getRolePermissions(String roleCode);

}
```

#### 4. 在启动类中扫描 mapper

```java
@SpringBootApplication
@MapperScan("com.kkb.mapper")
public class App {

    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }

}
```

#### 5. 编辑 application.yml 文件

```yaml
mybatis-plus:
   type-aliases-package: com.kkb.pojo
   configuration:
    map-underscore-to-camel-case: true  
```

#### 6. 一起愉快地测试

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class Demo {
	
    @Autowired
    private UserMapper userMapper;

    @Autowired
    private RoleMapper roleMapper;

    @Test
    public void testUserRole() {

        User user = userMapper.findUserByName("cuihua");
        Set<Role> roles = userMapper.getUserRoles("cuihua");

        for (Role role : roles) {
                role.setPermissions(roleMapper.
				getRolePermissions(role.getRoleCode()));
            }

        user.setRoles(roles);
        System.out.println(user);
    }
}
```



# MyBatis Plus 核心功能篇

## 一、代码生成器

**AutoGenerator** 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、Mapper、Mapper XML、Service、Controller 等各个模块的代码，极大的提升了开发效率。

```java
// 演示例子，执行 main 方法控制台输入模块表名回车自动生成对应项目目录中
public class CodeGenerator {

    /**
     * <p>
     * 读取控制台内容
     * </p>
     */
    public static String scanner(String tip) {
        Scanner scanner = new Scanner(System.in);
        StringBuilder help = new StringBuilder();
        help.append("请输入" + tip + "：");
        System.out.println(help.toString());
        if (scanner.hasNext()) {
            String ipt = scanner.next();
            if (StringUtils.isNotEmpty(ipt)) {
                return ipt;
            }
        }
        throw new MybatisPlusException("请输入正确的" + tip + "！");
    }

    public static void main(String[] args) {
        // 代码生成器
        AutoGenerator mpg = new AutoGenerator();

        // 全局配置
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        gc.setOutputDir(projectPath + "/src/main/java");
        gc.setAuthor("jobob");
        gc.setOpen(false);
        // gc.setSwagger2(true); 实体属性 Swagger2 注解
        mpg.setGlobalConfig(gc);

        // 数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/ant?useUnicode=true&useSSL=false&characterEncoding=utf8");
        // dsc.setSchemaName("public");
        dsc.setDriverName("com.mysql.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("密码");
        mpg.setDataSource(dsc);

        // 包配置
        PackageConfig pc = new PackageConfig();
        pc.setModuleName(scanner("模块名"));
        pc.setParent("com.baomidou.ant");
        mpg.setPackageInfo(pc);

        // 自定义配置
        InjectionConfig cfg = new InjectionConfig() {
            @Override
            public void initMap() {
                // to do nothing
            }
        };

        // 如果模板引擎是 freemarker
        String templatePath = "/templates/mapper.xml.ftl";
        // 如果模板引擎是 velocity
        // String templatePath = "/templates/mapper.xml.vm";

        // 自定义输出配置
        List<FileOutConfig> focList = new ArrayList<>();
        // 自定义配置会被优先输出
        focList.add(new FileOutConfig(templatePath) {
            @Override
            public String outputFile(TableInfo tableInfo) {
                // 自定义输出文件名 ， 如果你 Entity 设置了前后缀、此处注意 xml 的名称会跟着发生变化！！
                return projectPath + "/src/main/resources/mapper/" + pc.getModuleName()
                        + "/" + tableInfo.getEntityName() + "Mapper" + StringPool.DOT_XML;
            }
        });
        /*
        cfg.setFileCreate(new IFileCreate() {
            @Override
            public boolean isCreate(ConfigBuilder configBuilder, FileType fileType, String filePath) {
                // 判断自定义文件夹是否需要创建
                checkDir("调用默认方法创建的目录");
                return false;
            }
        });
        */
        cfg.setFileOutConfigList(focList);
        mpg.setCfg(cfg);

        // 配置模板
        TemplateConfig templateConfig = new TemplateConfig();

        // 配置自定义输出模板
        //指定自定义模板路径，注意不要带上.ftl/.vm, 会根据使用的模板引擎自动识别
        // templateConfig.setEntity("templates/entity2.java");
        // templateConfig.setService();
        // templateConfig.setController();

        templateConfig.setXml(null);
        mpg.setTemplate(templateConfig);

        // 策略配置
        StrategyConfig strategy = new StrategyConfig();
        strategy.setNaming(NamingStrategy.underline_to_camel);
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
        strategy.setSuperEntityClass("com.baomidou.ant.common.BaseEntity");
        strategy.setEntityLombokModel(true);
        strategy.setRestControllerStyle(true);
        strategy.setSuperControllerClass("com.baomidou.ant.common.BaseController");
        strategy.setInclude(scanner("表名，多个英文逗号分割").split(","));
        strategy.setSuperEntityColumns("id");
        strategy.setControllerMappingHyphenStyle(true);
        strategy.setTablePrefix(pc.getModuleName() + "_");
        mpg.setStrategy(strategy);
        mpg.setTemplateEngine(new FreemarkerTemplateEngine());
        mpg.execute();
    }

}
```

### 1. 具体使用

#### 1）添加依赖

MyBatis-Plus 从 `3.0.3` 之后移除了代码生成器与模板引擎的默认依赖，<font color='red'>需要手动添加相关依赖</font>：

- 添加 <font color='red'>代码生成器</font> 依赖

  ```xml
  <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-generator</artifactId>
      <version>3.1.1</version>
  </dependency>
  ```

- 添加 <font color='red'>模板引擎</font> 依赖，MyBatis-Plus 支持 Velocity（默认）、Freemarker、Beetl，用户可以选择自己熟悉的模板引擎，如果都用不爽，还可以采用自定义模板引擎。

  **Velocity（默认）**：

  ```xml
  <dependency>
      <groupId>org.apache.velocity</groupId>
      <artifactId>velocity-engine-core</artifactId>
      <version>2.1</version>
  </dependency>
  ```

  **Freemarker**：

  ```xml
  <dependency>
      <groupId>org.freemarker</groupId>
      <artifactId>freemarker</artifactId>
      <version>2.3.28</version>
  </dependency>
  ```

  **Beetl**：

  ```xml
  <dependency>
      <groupId>com.ibeetl</groupId>
      <artifactId>beetl</artifactId>
      <version>3.0.0.M1</version>
  </dependency>
  ```

  【注意】如果您选择了非默认引擎，<font color=red>需要在 AutoGenerator 中 设置模板引擎</font>。

  ```java
  AutoGenerator generator = new AutoGenerator();
  
  // set freemarker engine
  generator.setTemplateEngine(new FreemarkerTemplateEngine());
  
  // set beetl engine
  generator.setTemplateEngine(new BeetlTemplateEngine());
  
  // set custom engine (reference class is your custom engine class)
  generator.setTemplateEngine(new CustomTemplateEngine());
  
  // other config
  ...
  ```

#### 2）编写配置

MyBatis-Plus 的代码生成器提供了<font color=red>大量的自定义参数</font>供用户选择，能够满足绝大部分人的使用需求。

- 配置 GlobalConfig

  ```java
  GlobalConfig globalConfig = new GlobalConfig();
  globalConfig.setOutputDir(System.getProperty("user.dir") + "/src/main/java");
  globalConfig.setAuthor("jobob");
  globalConfig.setOpen(false);
  ```

- 配置 DataSourceConfig

  ```java
  DataSourceConfig dataSourceConfig = new DataSourceConfig();
  dataSourceConfig.setUrl("jdbc:mysql://localhost:3306/ant?useUnicode=true&useSSL=false&characterEncoding=utf8");
  dataSourceConfig.setDriverName("com.mysql.jdbc.Driver");
  dataSourceConfig.setUsername("root");
  dataSourceConfig.setPassword("password");
  ```

- 配置 DataSourceConfig

  ```java
  DataSourceConfig dataSourceConfig = new DataSourceConfig();
  dataSourceConfig.setUrl("jdbc:mysql://localhost:3306/ant?useUnicode=true&useSSL=false&characterEncoding=utf8");
  dataSourceConfig.setDriverName("com.mysql.jdbc.Driver");
  dataSourceConfig.setUsername("root");
  dataSourceConfig.setPassword("password");
  ```

### 2. 自定义模版引擎

请继承类 com.baomidou.mybatisplus.generator.engine.AbstractTemplateEngine

### 3. 自定义代码模版

```java
// 指定自定义模板路径, 位置：/resources/templates/entity2.java.ftl(或者是.vm)
// 注意不要带上.ftl(或者是.vm), 会根据使用的模板引擎自动识别
TemplateConfig templateConfig = new TemplateConfig()
    .setEntity("templates/entity2.java");

AutoGenerator mpg = new AutoGenerator();
// 配置自定义模板
mpg.setTemplate(templateConfig);
```

### 4. 自定义属性注入

```java
InjectionConfig injectionConfig = new InjectionConfig() {
    //自定义属性注入:abc
    //在.ftl(或者是.vm)模板中，通过${cfg.abc}获取属性
    @Override
    public void initMap() {
        Map<String, Object> map = new HashMap<>();
        map.put("abc", this.getConfig().getGlobalConfig().getAuthor() + "-mp");
        this.setMap(map);
    }
};
AutoGenerator mpg = new AutoGenerator();
//配置自定义属性注入
mpg.setCfg(injectionConfig);
```

```xml
entity2.java.ftl
自定义属性注入abc=${cfg.abc}

entity2.java.vm
自定义属性注入abc=$!{cfg.abc}
```

## 二、CRUD 接口

### 1. Mapper CRUD 接口

【简单说明】

- 通用 CRUD 封装 [BaseMapper](https://gitee.com/baomidou/mybatis-plus/blob/3.0/mybatis-plus-core/src/main/java/com/baomidou/mybatisplus/core/mapper/BaseMapper.java) 接口，为 `Mybatis-Plus` 启动时自动解析实体表关系映射转换为 `Mybatis` 内部对象注入容器
- 泛型 `T` 为任意实体对象
- 参数 `Serializable` 为任意类型主键 `Mybatis-Plus` 不推荐使用复合主键约定每一张表都有自己的唯一 `id` 主键
- 对象 `Wrapper` 为 [条件构造器](https://mp.baomidou.com/guide/wrapper.html)

#### insert

```java
/**
 * 插入一条记录
 *
 * @param entity 实体对象
 * @return 插入成功记录数
 */
int insert(T entity);
```

#### deleteById

```java
/**
 * 根据 ID 删除
 *
 * @param id 主键ID
 * @return 删除成功记录数
 */
int deleteById(Serializable id);
```

#### deleteByMap

```java
/**
 * 根据 columnMap 条件，删除记录
 *
 * @param columnMap 表字段 map 对象
 * @return 删除成功记录数
 */
int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);
```

#### delete

```java
/**
 * 根据 entity 条件，删除记录
 *
 * @param wrapper 实体对象封装操作类（可以为 null）
 * @return 删除成功记录数
 */
int delete(@Param(Constants.WRAPPER) Wrapper<T> wrapper);
```

#### deleteBatchIds

```java
/**
 * 删除（根据ID 批量删除）
 *
 * @param idList 主键ID列表(不能为 null 以及 empty)
 * @return 删除成功记录数
 */
int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);
```

#### updateById

```java
/**
 * 根据 ID 修改
 *
 * @param entity 实体对象
 * @return 修改成功记录数
 */
int updateById(@Param(Constants.ENTITY) T entity);
```

#### update

```java
/**
 * 根据 whereEntity 条件，更新记录
 *
 * @param entity        实体对象 (set 条件值,可为 null)
 * @param updateWrapper 实体对象封装操作类（可以为 null,里面的 entity 用于生成 where 语句）
 * @return 修改成功记录数
 */
int update(@Param(Constants.ENTITY) T entity, @Param(Constants.WRAPPER) Wrapper<T> updateWrapper);
```

#### selectById

```java
/**
 * 根据 ID 查询
 *
 * @param id 主键ID
 * @return 实体
 */
T selectById(Serializable id);
```

#### selectBatchIds

```java
/**
 * 查询（根据ID 批量查询）
 *
 * @param idList 主键ID列表(不能为 null 以及 empty)
 * @return 实体集合
 */
List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);
```

#### selectByMap

```java
/**
 * 查询（根据 columnMap 条件）
 *
 * @param columnMap 表字段 map 对象
 * @return 实体集合
 */
List<T> selectByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);
```

#### selectOne

```java
/**
 * 根据 entity 条件，查询一条记录
 *
 * @param queryWrapper 实体对象
 * @return 实体
 */
T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
```

#### selectCount

```java
/**
 * 根据 Wrapper 条件，查询总记录数
 *
 * @param queryWrapper 实体对象
 * @return 满足条件记录数
 */
Integer selectCount(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
```

#### selectList

```java
/**
 * 根据 entity 条件，查询全部记录
 *
 * @param queryWrapper 实体对象封装操作类（可以为 null）
 * @return 实体集合
 */
List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
```

#### selectMaps

```java
/**
 * 根据 Wrapper 条件，查询全部记录
 *
 * @param queryWrapper 实体对象封装操作类（可以为 null）
 * @return 字段映射对象 Map 集合
 */
List<Map<String, Object>> selectMaps(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
```

#### selectObjs

```java
/**
 * 根据 Wrapper 条件，查询全部记录
 * 注意： 只返回第一个字段的值
 *
 * @param queryWrapper 实体对象封装操作类（可以为 null）
 * @return 字段映射对象集合
 */
List<Object> selectObjs(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
```

#### selectPage

```java
/**
 * 根据 entity 条件，查询全部记录（并翻页）
 *
 * @param page         分页查询条件（可以为 RowBounds.DEFAULT）
 * @param queryWrapper 实体对象封装操作类（可以为 null）
 * @return 实体分页对象
 */
IPage<T> selectPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
```

#### selectMapsPage

```java
/**
 * 根据 Wrapper 条件，查询全部记录（并翻页）
 *
 * @param page         分页查询条件
 * @param queryWrapper 实体对象封装操作类
 * @return 字段映射对象 Map 分页对象
 */
IPage<Map<String, Object>> selectMapsPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
```

#### 【简单示例】

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class Demo {
	
    @Autowired
    private UserMapper userMapper;

    @Test
    public void testSelect(){

        // 1.  添加数据（一定要保证表的主键是自增状态）
        /*User user = new User();
        user.setName("老王");
        user.setEmail("laowang@kkb.com");
        user.setAge(18);
        userMapper.insert(user);*/

        // 2. 根据 id 删除
        // userMapper.deleteById(6);

        // 3. 根据 columnMap 条件，删除记录
        // Maps 属于 com.google.common.collect.Maps 包，来自于 Guava
        // Guava 中文是石榴的意思，该项目是 Google 的一个开源项目，包含许多 Google 核心的 Java 常用库
        /*Map<String, Object> columnMap = Maps.newHashMap();
        columnMap.put("id", 3);

        userMapper.deleteByMap(columnMap);*/

        // 4. 查询所有数据
        // 不指定条件，直接传递 null
        /*List<User> userList = userMapper.selectList(null);
        userList.forEach(user -> System.out.println("用户：" + user));*/

        // 5. 批量查询
        List<String> idList = new ArrayList<String>();
        idList.add("1");
        idList.add("2");
        List<User> userList = userMapper.selectBatchIds(idList);
        userList.forEach(user -> System.out.println("用户：" + user));

    }
}
```



### 2. Service CRUD 接口

【简单说明】

- 通用 Service CRUD 封装 [IService](https://gitee.com/baomidou/mybatis-plus/blob/3.0/mybatis-plus-extension/src/main/java/com/baomidou/mybatisplus/extension/service/IService.java) 接口，进一步封装 CRUD 采用 `get 查询单行` `remove 删除 list 查询集合` `page 分页` 前缀命名方式区分 `Mapper` 层避免混淆。
- 泛型 `T` 为任意实体对象
- 建议如果存在自定义通用 Service 方法的可能，请创建自己的 `IBaseService` 继承 `Mybatis-Plus` 提供的基类
- 对象 `Wrapper` 为 [条件构造器](https://mp.baomidou.com/guide/wrapper.html)

#### save

```java
/**
 * 插入一条记录（选择字段，策略插入）
 *
 * @param entity 实体对象
 */
boolean save(T entity);
```

#### saveBatch

```java
/**
 * 插入（批量）
 *
 * @param entity 实体对象
 */
boolean saveBatch(T entity);
```

#### saveBatch

```java
/**
* 插入（批量）
* 
* @param entityList 实体对象集合
* @param batchSize  插入批次数量
 */
boolean saveBatch(Collection<T> entityList, int batchSize);
```

#### saveOrUpdateBatch

```java
/**
 * 批量修改插入
 *
 * @param entityList 实体对象集合
 */
boolean saveOrUpdateBatch(Collection<T> entityList);
```

#### saveOrUpdateBatch

```java
/**
 * 批量修改插入
 *
 * @param entityList 实体对象集合
 * @param batchSize  每次的数量
 */
boolean saveOrUpdateBatch(Collection<T> entityList, int batchSize);
```

#### removeById

```java
/**
 * 根据 ID 删除
 *
 * @param id 主键ID
 */
boolean removeById(Serializable id);
```

#### removeByMap

```java
/**
 * 根据 columnMap 条件，删除记录
 *
 * @param columnMap 表字段 map 对象
 */
boolean removeByMap(Map<String, Object> columnMap);
```

#### remove

```java
/**
 * 根据 entity 条件，删除记录
 *
 * @param queryWrapper 实体包装类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
boolean remove(Wrapper<T> queryWrapper);
```

#### removeByIds

```java
/**
 * 删除（根据ID 批量删除）
 *
 * @param idList 主键ID列表
 */
boolean removeByIds(Collection<? extends Serializable> idList);
```

#### updateById

```java
/**
 * 根据 ID 选择修改
 *
 * @param entity 实体对象
 */
boolean updateById(T entity);
```

#### update

```java
/**
 * 根据 whereEntity 条件，更新记录
 *
 * @param entity        实体对象
 * @param updateWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.update.UpdateWrapper}
 */
boolean update(T entity, Wrapper<T> updateWrapper);
```

#### updateBatchById

```java
/**
 * 根据ID 批量更新
 *
 * @param entityList 实体对象集合
 * @param batchSize  更新批次数量
 */
boolean updateBatchById(Collection<T> entityList, int batchSize);
```

#### saveOrUpdate

```java
/**
 * TableId 注解存在更新记录，否插入一条记录
 *
 * @param entity 实体对象
 */
boolean saveOrUpdate(T entity);
```

#### getById

```java
/**
 * 根据 ID 查询
 *
 * @param id 主键ID
 */
T getById(Serializable id);
```

#### listByIds

```java
/**
 * <p>
 * 查询（根据ID 批量查询）
 * </p>
 *
 * @param idList 主键ID列表
 */
Collection<T> listByIds(Collection<? extends Serializable> idList);
```

#### listByMap

```java
/**
 * 查询（根据 columnMap 条件）
 *
 * @param columnMap 表字段 map 对象
 */
Collection<T> listByMap(Map<String, Object> columnMap);
```

#### getOne

```java
/**
 * 根据 Wrapper，查询一条记录
 *
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 * @param throwEx      有多个 result 是否抛出异常
 */
T getOne(Wrapper<T> queryWrapper, boolean throwEx);
```

#### getMap

```java
/**
 * 根据 Wrapper，查询一条记录
 *
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
Map<String, Object> getMap(Wrapper<T> queryWrapper);
```

#### getObj

```java
/**
 * 根据 Wrapper，查询一条记录
 *
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
Object getObj(Wrapper<T> queryWrapper);
```

#### count

```java
/**
 * 根据 Wrapper 条件，查询总记录数
 *
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
int count(Wrapper<T> queryWrapper);
```

#### list

```java
/**
 * 查询列表
 *
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
List<T> list(Wrapper<T> queryWrapper);
```

#### page

```java
/**
 * 翻页查询
 *
 * @param page         翻页对象
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
IPage<T> page(IPage<T> page, Wrapper<T> queryWrapper);
```

#### listMaps

```java
/**
 * 查询列表
 *
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
List<Map<String, Object>> listMaps(Wrapper<T> queryWrapper);
```

#### listObjs

```java
/**
 * 根据 Wrapper 条件，查询全部记录
 *
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
List<Object> listObjs(Wrapper<T> queryWrapper);
```

#### pageMaps

```java
/**
 * 翻页查询
 *
 * @param page         翻页对象
 * @param queryWrapper 实体对象封装操作类 {@link com.baomidou.mybatisplus.core.conditions.query.QueryWrapper}
 */
IPage<Map<String, Object>> pageMaps(IPage<T> page, Wrapper<T> queryWrapper);
```

### 3. mapper 层 选装件

选装件位于 `com.baomidou.mybatisplus.extension.injector.methods.additional` 包下 需要配合 [Sql 注入器](https://mp.baomidou.com/guide/sql-injector.html) 使用。

```java
int insertBatchSomeColumn(List<T> entityList);
```

```java
int deleteByIdWithFill(T entity);
```

## 三、条件构造器

【简单说明】

- 以下出现的第一个入参 `boolean condition ` 表示该条件**是否**加入最后生成的 sql 中
- 以下代码块内的多个方法均为从上往下补全个别 `boolean` 类型的入参，默认为 `true`
- 以下出现的泛型 `Param` 均为 `Wrapper` 的子类实例（均具有`AbstractWrapper` 的所有方法）
- 以下方法在入参中出现的 `R` 为泛型，在普通 wrapper 中是 `String`，在LambdaWrapper 中是 **函数**（例：`Entity::getId`,`Entity` 为实体类,`getId`为字段 `id` 的 **getMethod**）
- 以下方法入参中的 `R column` 均表示数据库字段，当 `R` 为 `String` 时则为数据库字段名（**字段名是数据库关键字的自己用转义符包裹**），而不是实体类数据字段名。
- 以下举例均为使用普通 wrapper，入参为 `Map` 和 `List` 的均以`json`形式表现!
- 使用中如果入参的 `Map` 或者 `List` 为**空**，则不会加入最后生成的 sql 中。
- 有任何疑问就点开源码看，另外还可以补充下 [lambda 函数](https://www.jianshu.com/p/613a6118e2e0)。

【注意】不支持以及不赞成在 RPC 调用中把 Wrapper 进行传输

1. wrapper 很重。
2. 传输 wrapper 可以类比为你的 controller 用 map 接收值。
3. 正确的 RPC 调用姿势是写一个 DTO 进行传输，被调用方再根据 DTO 执行相应的操作。

### 1. AbstractWrapper

QueryWrapper(LambdaQueryWrapper) 和 UpdateWrapper(LambdaUpdateWrapper) 的父类
用于生成 sql 的 where 条件, entity 属性也用于生成 sql 的 where 条件

注意：entity 生成的 where 条件与 使用各个 api 生成的 where 条件**没有任何关联行为**。

#### allEq

```java
allEq(Map<R, V> params)
allEq(Map<R, V> params, boolean null2IsNull)
allEq(boolean condition, Map<R, V> params, boolean null2IsNull)
```

- 全部 [eq](https://mp.baomidou.com/guide/wrapper.html#eq) （或个别 [isNull](https://mp.baomidou.com/guide/wrapper.html#isnull)）

个别参数说明:

`params` : `key` 为数据库字段名，`value` 为字段值
`null2IsNull` : 为`true`则在`map`的`value`为`null`时调用 [isNull](https://mp.baomidou.com/guide/wrapper.html#isnull) 方法,为`false`时则忽略`value`为`null`的

- 例1: `allEq({id:1,name:"老王",age:null})`--->`id = 1 and name = '老王' and age is null`
- 例2: `allEq({id:1,name:"老王",age:null}, false)`--->`id = 1 and name = '老王'`

```java
allEq(BiPredicate<R, V> filter, Map<R, V> params)
allEq(BiPredicate<R, V> filter, Map<R, V> params, boolean null2IsNull)
allEq(boolean condition, BiPredicate<R, V> filter, Map<R, V> params, boolean null2IsNull) 
```

个别参数说明:

`filter` : 过滤函数,是否允许字段传入比对条件中
`params` 与 `null2IsNull` : 同上

- 例1: `allEq((k,v) -> k.indexOf("a") > 0, {id:1,name:"老王",age:null})`--->`name = '老王' and age is null`
- 例2: `allEq((k,v) -> k.indexOf("a") > 0, {id:1,name:"老王",age:null}, false)`--->`name = '老王'`

#### eq

```java
eq(R column, Object val)
eq(boolean condition, R column, Object val)
```

- 等于 =
- 例: `eq("name", "老王")`--->`name = '老王'`

#### ne

```java
ne(R column, Object val)
ne(boolean condition, R column, Object val)
```

- 不等于 <>
- 例: `ne("name", "老王")`--->`name <> '老王'`

#### gt

```java
gt(R column, Object val)
gt(boolean condition, R column, Object val)
```

- 大于 >
- 例: `gt("age", 18)`--->`age > 18`

#### ge

```java
ge(R column, Object val)
ge(boolean condition, R column, Object val)
```

- 大于等于 >=
- 例: `ge("age", 18)`--->`age >= 18`

#### lt

```java
lt(R column, Object val)
lt(boolean condition, R column, Object val)
```

- 小于 <
- 例: `lt("age", 18)`--->`age < 18`

#### le

```java
le(R column, Object val)
le(boolean condition, R column, Object val)
```

- 小于等于 <=
- 例: `le("age", 18)`--->`age <= 18`

#### between

```java
between(R column, Object val1, Object val2)
between(boolean condition, R column, Object val1, Object val2)
```

- BETWEEN 值1 AND 值2
- 例: `between("age", 18, 30)`--->`age between 18 and 30`

#### notBetween

```java
notBetween(R column, Object val1, Object val2)
notBetween(boolean condition, R column, Object val1, Object val2)
```

- NOT BETWEEN 值1 AND 值2
- 例: `notBetween("age", 18, 30)`--->`age not between 18 and 30`

#### like

```java
like(R column, Object val)
like(boolean condition, R column, Object val)
```

- LIKE '%值%'
- 例: `like("name", "王")`--->`name like '%王%'`

#### notLike

```java
notLike(R column, Object val)
notLike(boolean condition, R column, Object val)
```

- NOT LIKE '%值%'
- 例: `notLike("name", "王")`--->`name not like '%王%'`

#### likeLeft

```java
likeLeft(R column, Object val)
likeLeft(boolean condition, R column, Object val)
```

- LIKE '%值'
- 例: `likeLeft("name", "王")`--->`name like '%王'`

#### likeRight

```java
likeRight(R column, Object val)
likeRight(boolean condition, R column, Object val)
```

- LIKE '值%'
- 例: `likeRight("name", "王")`--->`name like '王%'`

#### isNull

```java
isNull(R column)
isNull(boolean condition, R column)
```

- 字段 IS NULL
- 例: `isNull("name")`--->`name is null`

#### isNotNull

```java
isNotNull(R column)
isNotNull(boolean condition, R column)
```

- 字段 IS NULL
- 例: `isNotNull("name")`--->`name is not null`

#### in

```java
in(R column, Collection<?> value)
in(boolean condition, R column, Collection<?> value)
```

- 字段 IN (value.get(0), value.get(1), ...)
- 例: `in("age",{1,2,3})`--->`age in (1,2,3)`

```java
in(R column, Object... values)
in(boolean condition, R column, Object... values)
```

- 字段 IN (v0, v1, ...)
- 例: `in("age", 1, 2, 3)`--->`age in (1,2,3)`

#### notIn

```java
notIn(R column, Collection<?> value)
notIn(boolean condition, R column, Collection<?> value)
```

- 字段 IN (value.get(0), value.get(1), ...)
- 例: `notIn("age",{1,2,3})`--->`age not in (1,2,3)`

 

```java
notIn(R column, Object... values)
notIn(boolean condition, R column, Object... values)
```

- 字段 NOT IN (v0, v1, ...)
- 例: `notIn("age", 1, 2, 3)`--->`age not in (1,2,3)`

#### inSql

```java
inSql(R column, String inValue)
inSql(boolean condition, R column, String inValue)
```

- 字段 IN ( sql语句 )
- 例: `inSql("age", "1,2,3,4,5,6")`--->`age in (1,2,3,4,5,6)`
- 例: `inSql("id", "select id from table where id < 3")`--->`id in (select id from table where id < 3)`

#### notInSql

```java
notInSql(R column, String inValue)
notInSql(boolean condition, R column, String inValue)
```

- 字段 NOT IN ( sql语句 )
- 例: `notInSql("age", "1,2,3,4,5,6")`--->`age not in (1,2,3,4,5,6)`
- 例: `notInSql("id", "select id from table where id < 3")`--->`age not in (select id from table where id < 3)`

#### groupBy

```java
groupBy(R... columns)
groupBy(boolean condition, R... columns)
```

- 分组：GROUP BY 字段, ...
- 例: `groupBy("id", "name")`--->`group by id,name`

#### orderByAsc

```java
orderByAsc(R... columns)
orderByAsc(boolean condition, R... columns)
```

- 排序：ORDER BY 字段, ... ASC
- 例: `orderByAsc("id", "name")`--->`order by id ASC,name ASC`

#### orderByDesc

```java
orderByDesc(R... columns)
orderByDesc(boolean condition, R... columns)
```

- 排序：ORDER BY 字段, ... DESC
- 例: `orderByDesc("id", "name")`--->`order by id DESC,name DESC`

#### orderBy

```java
orderBy(boolean condition, boolean isAsc, R... columns)
```

- 排序：ORDER BY 字段, ...
- 例: `orderBy(true, true, "id", "name")`--->`order by id ASC,name ASC`

#### having

```java
having(String sqlHaving, Object... params)
having(boolean condition, String sqlHaving, Object... params)
```

- HAVING ( sql语句 )
- 例: `having("sum(age) > 10")`--->`having sum(age) > 10`
- 例: `having("sum(age) > {0}", 11)`--->`having sum(age) > 11`

#### or

```java
or()
or(boolean condition)
```

- 拼接 OR

注意事项:

主动调用`or`表示紧接着下一个**方法**不是用`and`连接!(不调用`or`则默认为使用`and`连接)

- 例: `eq("id",1).or().eq("name","老王")`--->`id = 1 or name = '老王'`

 

```java
or(Function<Param, Param> func)
or(boolean condition, Function<Param, Param> func)
```

- OR 嵌套
- 例: `or(i -> i.eq("name", "李白").ne("status", "活着"))`--->`or (name = '李白' and status <> '活着')`

#### and

```java
and(Function<Param, Param> func)
and(boolean condition, Function<Param, Param> func)
```

- AND 嵌套
- 例: `and(i -> i.eq("name", "李白").ne("status", "活着"))`--->`and (name = '李白' and status <> '活着')`

#### nested

```java
nested(Function<Param, Param> func)
nested(boolean condition, Function<Param, Param> func)
```

- 正常嵌套 不带 AND 或者 OR
- 例: `nested(i -> i.eq("name", "李白").ne("status", "活着"))`--->`(name = '李白' and status <> '活着')`

#### apply

```java
apply(String applySql, Object... params)
apply(boolean condition, String applySql, Object... params)
```

- 拼接 sql

注意事项:

该方法可用于数据库**函数** 动态入参的`params`对应前面`applySql`内部的`{index}`部分.这样是不会有sql注入风险的,反之会有!

- 例: `apply("id = 1")`--->`id = 1`
- 例: `apply("date_format(dateColumn,'%Y-%m-%d') = '2008-08-08'")`--->`date_format(dateColumn,'%Y-%m-%d') = '2008-08-08'")`
- 例: `apply("date_format(dateColumn,'%Y-%m-%d') = {0}", "2008-08-08")`--->`date_format(dateColumn,'%Y-%m-%d') = '2008-08-08'")`

#### last

```java
last(String lastSql)
last(boolean condition, String lastSql)
```

- 无视优化规则直接拼接到 sql 的最后

注意事项：只能调用一次，多次调用以最后一次为准，有 sql 注入的风险，请谨慎使用。

- 例：`last("limit 1")`

#### exists 

```java
exists(String existsSql)
exists(boolean condition, String existsSql)
```

- 拼接 EXISTS ( sql语句 )
- 例: `exists("select id from table where age = 1")`--->`exists (select id from table where age = 1)`

#### notExists

```java
notExists(String notExistsSql)
notExists(boolean condition, String notExistsSql)
```

- 拼接 NOT EXISTS ( sql语句 )
- 例: `notExists("select id from table where age = 1")`--->`not exists (select id from table where age = 1)`

### 2. QueryWrapper

【简单说明】

继承自 AbstractWrapper，自身的内部属性 entity 也用于生成 where 条件
及 LambdaQueryWrapper，可以通过 new QueryWrapper().lambda() 方法获取。

#### select 

```java
select(String... sqlSelect)
select(Predicate<TableFieldInfo> predicate)
select(Class<T> entityClass, Predicate<TableFieldInfo> predicate)
```

- 设置查询字段

【简单说明】

以上方分法为两类。
第二类方法为：过滤查询字段（主键除外），入参不包含 class 的调用前需要`wrapper` 内的 `entity` 属性有值， 这两类方法重复调用以最后一次为准。

- 例: `select("id", "name", "age")`
- 例: `select(i -> i.getProperty().startsWith("test"))`

【简单示例】

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class Demo {
	
    @Autowired
    private UserMapper userMapper;

    @Test
    public void testSelect2(){
        IPage<User> page = selectUserPage();

        List<User> userList = page.getRecords();
        userList.forEach(user -> System.out.println("用户：" + user));
    }

    // 查询年龄在 20-30 名字为 cuihua 的用户
    public  IPage<User>  selectUserPage() {

        Page page = new Page<User>(1, 20);
		QueryWrapper<User> queryWrapper = new QueryWrapper<User>().between("age", 18, 20).eq("name", "cuihua");
		return userMapper.selectPage(page, queryWrapper);
	}
}
```



### 3. UpdateWrapper

说明:

继承自 `AbstractWrapper` ,自身的内部属性 `entity` 也用于生成 where 条件
及 `LambdaUpdateWrapper`, 可以通过 `new UpdateWrapper().lambda()` 方法获取!

#### set

```java
set(String column, Object val)
set(boolean condition, String column, Object val)
```

- SQL SET 字段
- 例: `set("name", "老李头")`
- 例: `set("name", "")`--->数据库字段值变为**空字符串**
- 例: `set("name", null)`--->数据库字段值变为`null`

#### setSql

```java
setSql(String sql)
```

- 设置 SET 部分 SQL
- 例: `set("name = '老李头')`

#### lambda

- 获取 `LambdaWrapper`
  在`QueryWrapper`中是获取`LambdaQueryWrapper`
  在`UpdateWrapper`中是获取`LambdaUpdateWrapper`

### 4. 使用 Wrapper 自定义 SQL

【需求来源】

在使用了 `mybatis-plus` 之后，自定义 SQL 的同时也想使用 `Wrapper` 的便利应该怎么办？ 在 `mybatis-plus` 版本 `3.0.7` 得到了完美解决 版本需要大于或等于`3.0.7`， 以下两种方案取其一即可。

#### Service.java

```java
mysqlMapper.getAll(Wrappers.<MysqlData>lambdaQuery().eq(MysqlData::getGroup, 1));
```

#### 方案一 注解方式 Mapper.java

```java
@Select("select * from mysql_data ${ew.customSqlSegment}")
List<MysqlData> getAll(@Param(Constants.WRAPPER) Wrapper wrapper);
```

#### 方案二 XML形式 Mapper.xml

```xml
<select id="getAll" resultType="MysqlData">
	SELECT * FROM mysql_data ${ew.customSqlSegment}
</select>
```

## 四、分页插件

### 1. 简单示例

```xml
<!-- spring xml 方式 -->
<plugins>
    <plugin interceptor="com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor">
        <property name="sqlParser" ref="自定义解析类、可以没有" />
        <property name="dialectClazz" value="自定义方言类、可以没有" />
    </plugin>
</plugins>
```

```java
//Spring boot 方式
@EnableTransactionManagement
@Configuration
@MapperScan("com.baomidou.cloud.service.*.mapper*")
public class MybatisPlusConfig {

    /**
     * 分页插件
     */
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }
}
```

### 2. XML 自定义分页

1）UserMapper 类

```java
public interface UserMapper{//可以继承或者不继承BaseMapper
    /**
     * <p>
     * 查询 : 根据state状态查询用户列表，分页显示
     * 注意!!: 如果入参是有多个,需要加注解指定参数名才能在xml中取值
     * </p>
     *
     * @param page 分页对象,xml中可以从里面进行取值,传递参数 Page 即自动分页,必须放在第一位(你可以继承Page实现自己的分页对象)
     * @param state 状态
     * @return 分页对象
     */
    IPage<User> selectPageVo(Page page, @Param("state") Integer state);
}
```

2）UserMapper.xml 等同于编写一个普通的 list 查询，mybatis-plus 自动分页。

```xml
<select id="selectPageVo" resultType="com.baomidou.cloud.entity.UserVo">
    SELECT id,name FROM user WHERE state=#{state}
</select>
```

3）UserServiceImpl 类，调用分页方法。

```java
public IPage<User> selectUserPage(Page<User> page, Integer state) {
    // 不进行 count sql 优化，解决 MP 无法自动优化 SQL 问题，这时候你需要自己查询 count 部分
    // page.setOptimizeCountSql(false);
    // 当 total 为非 0 时(默认为 0),分页插件不会进行 count 查询
    // 要点!! 分页返回的对象与传入的对象是同一个
    return userMapper.selectPageVo(page, state);
}
```

### 3. Sequence 主键



实体主键支持Sequence

- Oracle 等数据库主键策略配置 Sequence

- GlobalConfiguration 配置 KeyGenerator

```java
  GlobalConfiguration gc = new GlobalConfiguration();
  gc.setKeyGenerator(new OracleKeyGenerator());
```

- mybatis-plus-boot-starter [配置参考](https://mp.baomidou.com/config)

```java
@Bean
public OracleKeyGenerator oracleKeyGenerator(){
  return new OracleKeyGenerator();
}
```

- 实体类配置主键 Sequence，

  指定主键 @TableId(type=IdType.INPUT) // 不能使用AUTO

```java
@TableName("TEST_SEQUSER")
@KeySequence("SEQ_TEST")//类注解
public class TestSequser{
  @TableId(value = "ID", type = IdType.INPUT)
  private Long id;

}
```

- 支持父类定义 @KeySequence, 子类使用，这样就可以几个表共用一个Sequence

```java
@KeySequence("SEQ_TEST")
public abstract class Parent{

}

public class Child extends Parent{

}
```

以上步骤就可以使用Sequence当主键了。

Spring MVC：xml配置，请参考 https://mp.baomidou.com/install

#### 如何使用 Sequence 作为主键，但是实体主键类型是 String

也就是说，表的主键是varchar2, 但是需要从sequence中取值

- 1.实体定义@KeySequence 注解clazz指定类型String.class
- 2.实体定义主键的类型String

```java
@KeySequence(value = "SEQ_ORACLE_STRING_KEY", clazz = String.class)
public class YourEntity{
    
    @TableId(value = "ID_STR", type = IdType.INPUT)
    private String idStr;
    ...
}
```

- 3.正常配置GlobalConfiguration.keyGenerator

```java
@Bean
public GlobalConfiguration globalConfiguration() {
    GlobalConfiguration conf = new GlobalConfiguration();
    conf.setKeyGenerator(new OracleKeyGenerator());
    return conf;
}
```



# MyBatis Plus 常用插件

## 一、逻辑删除

在 Spring Boot 中的 application.yml 文件中配置。如果我们保持默认，则此配置就不用写了。

```yaml
mybatis-plus:
  global-config:
    db-config:
      logic-delete-value: 1 # 逻辑已删除值(默认为 1)
      logic-not-delete-value: 0 # 逻辑未删除值(默认为 0)
```

然后在实体类上，添加 @TableLogic 注解。

```java
@TableLogic
private Integer deleted;
```

【注意】使用 mp 自带方法删除和查找都会附带逻辑删除功能 ，但自己写的 xml 不会。

> java 逻辑删除简单理解就是“删除”，它是为了方便数据恢复和保护数据本身价值等等的一种方案。如果这个数据你需要再去查出来，就不应该使用逻辑删除，而是以一个状态去表示。比如，员工离职、帐号锁定等都应该是一个状态字段，不适合使用逻辑删除。如果是要查找删除数据，则需要单独手写 SQL 来实现，比如需要查看过往所有数据的统计汇总信息。

## 二、通用枚举

主要是为了让 MyBatis 更优雅的使用枚举属性，抛弃那些超繁琐的配置。

从 3.1.1 版本开始，直接使用默认枚举类会更好。

推荐的配置方式：

1）使用 IEnum 接口，推荐配置 defaultEnumTypeHandler 

2）使用注解枚举处理，推荐配置 typeEnumsPackage

3）注解枚举处理与 IEnum 接口，推荐配置 typeEnumsPackage

4）与原生枚举混用，需配置 defaultEnumTypeHandler 与 typeEnumsPackage

### 1. 使用通用枚举属性的两种方式

#### 方式一：使用 @EnumValue 注解枚举属性

```java
public enum GradeEnum {

    PRIMARY(1, "小学"),  SECONDORY(2, "中学"),  HIGH(3, "高中");

    GradeEnum(int code, String descp) {
        this.code = code;
        this.descp = descp;
    }

    @EnumValue//标记数据库存的值是code
    private final int code;
    //。。。
}
```

#### 方式二：枚举属性，实现 IEnum 接口

```java
public enum AgeEnum implements IEnum<Integer> {
    ONE(1, "一岁"),
    TWO(2, "二岁"),
    THREE(3, "三岁");
    
    private int value;
    private String desc;
    
    @Override
    public Integer getValue() {
        return this.value;
    }
}
```

实体属性使用枚举类型

```java
public class User{
    /**
     * 名字
     * 数据库字段: name varchar(20)
     */
    private String name;
    
    /**
     * 年龄，IEnum接口的枚举处理
     * 数据库字段：age INT(3)
     */
    private AgeEnum age;
        
        
    /**
     * 年级，原生枚举（带{@link com.baomidou.mybatisplus.annotation.EnumValue}):
     * 数据库字段：grade INT(2)
     */
    private GradeEnum grade;
}
```

#### 2. 配置扫描通用枚举

编辑 Spring Boot 项目 applicaton.yml 文件

```text
mybatis-plus:
    # 支持统配符 * 或者 ; 分割
    typeEnumsPackage: com.kkb.entity.enums
```

#### 3. JSON 序列化处理

##### 1）Jackson

在需要响应描述字段的 get 方法上添加 @JsonValue 注解即可

##### 2）Fastjson

全局处理方式

```text
FastJsonConfig config = new FastJsonConfig();

// 设置WriteEnumUsingToString	config.setSerializerFeatures(SerializerFeature.WriteEnumUsingToString);

converter.setFastJsonConfig(config);
```

局部处理方式

```text
@JSONField(serialzeFeatures= SerializerFeature.WriteEnumUsingToString)
private UserStatus status;
```

二选一，然后在枚举中重写 toString() 方法。

## 三、 自动填充功能

在使用的时候，需要实现元对象处理器接口：com.baomidou.mybatisplus.core.handlers.MetaObjectHandler

### 1. 配置 @TableField 注解

注解填充字段 `@TableField(.. fill = FieldFill.INSERT)` 生成器策略部分也可以配置。

```java
public class User {

    // 注意！这里需要标记为填充字段
    @TableField(.. fill = FieldFill.INSERT)
    private String fillField;

    ....
}
```

自定义实现类，需要实现 MetaObjectHandler 接口，填充处理器在 Spring Boot 中使用的时候，需要再加上 @Component 注解。

必须使用父类的 setFieldValByName() 或者 setInsertFieldValByName / setUpdateFieldValByName 方法，否则不会根据注解中的 FiledFill.xxx 来区分。

```java
@Component
public class MyMetaObjectHandler implements MetaObjectHandler {

    private static final Logger LOGGER = LoggerFactory.getLogger(MyMetaObjectHandler.class);

    @Override
    public void insertFill(MetaObject metaObject) {
        LOGGER.info("start insert fill ....");
        this.setFieldValByName("operator", "Jerry", metaObject);//版本号3.0.6以及之前的版本
        //this.setInsertFieldValByName("operator", "Jerry", metaObject);//@since 快照：3.0.7.2-SNAPSHOT， @since 正式版暂未发布3.0.7
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        LOGGER.info("start update fill ....");
        this.setFieldValByName("operator", "Tom", metaObject);
        //this.setUpdateFieldValByName("operator", "Tom", metaObject);//@since 快照：3.0.7.2-SNAPSHOT， @since 正式版暂未发布3.0.7
    }
}
```

```java
public enum FieldFill {
    /**
     * 默认不处理
     */
    DEFAULT,
    /**
     * 插入填充字段
     */
    INSERT,
    /**
     * 更新填充字段
     */
    UPDATE,
    /**
     * 插入和更新填充字段
     */
    INSERT_UPDATE
}
```

## 四、SQL 注入器



**注入器配置**：全局配置 `sqlInjector` 用于注入 `ISqlInjector` 接口的子类，实现自定义方法注入。

自定义的通用方法可以实现接口 `ISqlInjector` ，也可以继承抽象类 `AbstractSqlInjector` 注入通用方法 `SQL 语句`， 然后继承 `BaseMapper` 添加自定义方法，全局配置 `sqlInjector` 注入 MP 会自动将类所有方法注入到 `mybatis` 容器中。

```java
public interface ISqlInjector {

    /**
     * 检查SQL是否注入(已经注入过不再注入)
     *
     * @param builderAssistant mapper 信息
     * @param mapperClass      mapper 接口的 class 对象
     */
    void inspectInject(MapperBuilderAssistant builderAssistant, Class<?> mapperClass);
}
```

## 五、攻击 SQL 阻断解析器

主要作用：阻止恶意的全表更新删除。

```java
@Bean
public PaginationInterceptor paginationInterceptor() {
    PaginationInterceptor paginationInterceptor = new PaginationInterceptor();

    ...

    List<ISqlParser> sqlParserList = new ArrayList<>();
    // 攻击 SQL 阻断解析器、加入解析链
    sqlParserList.add(new BlockAttackSqlParser());
    paginationInterceptor.setSqlParserList(sqlParserList);

    ...

    return paginationInterceptor;
}
```

## 六、性能分析插件

性能分析拦截器，用于输出每条 SQL 语句及其执行时间。如果对 SQL 的打印效果要求较高，可以使用下面的【执行SQL分析打印】插件。

先配置 ```<plugins>``` 标签

```xml
<plugins>
    ....

    <!-- SQL 执行性能分析，开发环境使用，线上不推荐。 maxTime 指的是 sql 最大执行时长 -->
    <plugin interceptor="com.baomidou.mybatisplus.extension.plugins.PerformanceInterceptor">
        <property name="maxTime" value="100" />
        <!--SQL是否格式化 默认false-->
        <property name="format" value="true" />
    </plugin>
</plugins>
```

然后，在 Spring Boot 中使用

```java
//Spring boot方式
@EnableTransactionManagement
@Configuration
@MapperScan("com.baomidou.cloud.service.*.mapper*")
public class MybatisPlusConfig {

    /**
     * SQL执行效率插件
     */
    @Bean
    @Profile({"dev","test"})// 设置 dev test 环境开启
    public PerformanceInterceptor performanceInterceptor() {
        return new PerformanceInterceptor();
    }
}
```

**参数说明**

- 参数：maxTime SQL 执行最大时长，超过自动停止运行，有助于发现问题。

- 参数：format SQL SQL是否格式化，默认false。

  

**【**注意】**该插件只用于开发环境，不建议生产环境使用。**

## 七、执行 SQL 分析打印



在 3.1.0+ 版本，该功能依赖 `p6spy` 组件，完美的输出打印 SQL 及执行时长。

1）在 Maven 中引入 p6spy 依赖

```xml
<dependency>
  <groupId>p6spy</groupId>
  <artifactId>p6spy</artifactId>
  <version>最新版本</version>
</dependency>
```

2）配置 application.yml 文件

```xml
spring:
  datasource:
    driver-class-name: com.p6spy.engine.spy.P6SpyDriver
    url: jdbc:p6spy:h2:mem:test
    ...
```

3）配置 spy.properties 文件

```xml
module.log=com.p6spy.engine.logging.P6LogFactory,com.p6spy.engine.outage.P6OutageFactory
# 自定义日志打印
logMessageFormat=com.baomidou.mybatisplus.extension.p6spy.P6SpyLogger
#日志输出到控制台
appender=com.baomidou.mybatisplus.extension.p6spy.StdoutLogger
# 使用日志系统记录 sql
#appender=com.p6spy.engine.spy.appender.Slf4JLogger
# 设置 p6spy driver 代理
deregisterdrivers=true
# 取消JDBC URL前缀
useprefix=true
# 配置记录 Log 例外,可去掉的结果集有error,info,batch,debug,statement,commit,rollback,result,resultset.
excludecategories=info,debug,result,batch,resultset
# 日期格式
dateformat=yyyy-MM-dd HH:mm:ss
# 实际驱动可多个
#driverlist=org.h2.Driver
# 是否开启慢SQL记录
outagedetection=true
# 慢SQL记录标准 2 秒
outagedetectioninterval=2
```

【注意】

​	**driver-class-name** 为 p6spy 提供的驱动类。

​	url 前缀为 jdbc:p6spy 跟着冒号为对应数据库连接地址。

​	❤ 此插件有性能损耗，不建议生产环境使用。

## 八、乐观锁插件

###  1. 主要适用场景

**意图**：当要更新一条记录的时候，希望这条记录没有被别人更新

**乐观锁实现方式**：

- 取出记录时，获取当前 version
- 更新时，带上这个 version
- 执行更新时， set version = newVersion where version = oldVersion
- 如果version不对，就更新失败

### 2. 使用方式

#### 插件配置

在 Spring Boot 中的配置

```java
@Bean
public OptimisticLockerInterceptor optimisticLockerInterceptor() {
    return new OptimisticLockerInterceptor();
}
```

2）一定要在实体字段上使用 @Version 注解

```java
@Version
private Integer version;
```

##### 特别说明

- **支持的数据类型：int, Integer, long, Long, Date, Timestamp, LocalDateTime**
- 整数类型下 `newVersion = oldVersion + 1`
- `newVersion` 会回写到 `entity` 中
- 仅支持 `updateById(id)` 与 `update(entity, wrapper)` 方法
- **在 update(entity, wrapper) 方法下, wrapper 不能复用!!!**

##### 示例

编写 Java 代码，具体如下：

```java
int id = 100;
int version = 2;

User u = new User();
u.setId(id);
u.setVersion(version);
u.setXXX(xxx);

if(userService.updateById(u)){
    System.out.println("Update successfully");
}else{
    System.out.println("Update failed due to modified by others");
}
```

上面所对应的 SQL 原理非常简单：

```sql
update tbl_user set name = 'update',version = 3 where id = 100 and version = 2
```

## 九、动态数据源

![img](https://s1.ax1x.com/2018/07/31/PwPVAA.png)

### 1. 快速了解

#### 1. 简介

dynamic-datasource-spring-boot-starter：一个基于 Springboot 的快速继承多数据源的启动器。

github 地址：https://github.com/baomidou/dynamic-datasource-spring-boot-starter

#### 2. 优势

关于动态数据源的切换，核心只有两种。

1）构建多套环境，优势是方便控制也容易集成一些简单的分布式事务，缺点是非动态同时代码量较多，配置难度大。

2）基于spring提供原生的 `AbstractRoutingDataSource` ，参考一些文档自己实现切换。

如果你的数据源较少，场景不复杂，选择以上任意一种都可以。**如果你需要更多特性，请尝试本动态数据源。**

```java
1）数据源分组，适用于多种场景、纯粹多库、读写分离、一主多从、混合模式。

2）简单集成 Druid 数据源监控多数据源，简单集成 Mybatis-Plus 简化单表，简单集成 P6sy 格式化 sql，简单集成 Jndi 数据源。

3）简化 Druid 和 HikariCp 配置，提供全局参数配置。

4）提供自定义数据源来源（默认使用 yml 或 properties 配置）。

5）项目启动后能动态增减数据源。

6）使用 spel 动态参数解析数据源，如从 session，header 和参数中获取数据源。（多租户架构神器）

7）多层数据源嵌套切换。（一个业务ServiceA调用ServiceB，ServiceB调用ServiceC，每个Service都是不同的数据源）

8）使用正则匹配或spel表达式来切换数据源（实验性功能）。
```

#### 3. 劣势

不能使用多数据源事务（同一个数据源下能使用事务），网上其他方案也都不能提供。

如果你需要使用到分布式事务，那么你的架构应该到了微服务化的时候了。

#### 4. 约定

1）本框架只做 **切换数据源** 这件核心的事情，并**不限制你的具体操作**，切换了数据源可以做任何 CRUD。

2）配置文件所有以下划线 `_` 分割的数据源 **首部** 即为组的名称，相同组名称的数据源会放在一个组下。

3）切换数据源即可是组名，也可是具体数据源名称，切换时默认采用负载均衡机制切换。

4）默认的数据源名称为 **master** ，你可以通过spring.datasource.dynamic.primary修改。

5）方法上的注解优先于类上注解。

#### 5. 建议

强烈建议在 **主从模式** 下遵循普遍的规则，以便他人能更轻易理解你的代码。

主数据库 **建议** 只执行 `INSERT` `UPDATE` `DELETE` 操作。

从数据库 **建议** 只执行 `SELECT` 操作。

### 2. 具体使用

1）引入依赖

```xml
<dependency>
  <groupId>com.baomidou</groupId>
  <artifactId>dynamic-datasource-spring-boot-starter</artifactId>
  <version>${version}</version>
</dependency>
```

2）配置数据源

```yaml
spring:
  datasource:
    dynamic:
      primary: master #设置默认的数据源或者数据源组,默认值即为master
      datasource:
        master:
          username: root
          password: 123456
          driver-class-name: com.mysql.jdbc.Driver
          url: jdbc:mysql://xx.xx.xx.xx:3306/dynamic
        slave_1:
          username: root
          password: 123456
          driver-class-name: com.mysql.jdbc.Driver
          url: jdbc:mysql://xx.xx.xx.xx:3307/dynamic
        slave_2:
          username: root
          password: 123456
          driver-class-name: com.mysql.jdbc.Driver
          url: jdbc:mysql://xx.xx.xx.xx:3308/dynamic
       #......省略
       #以上会配置一个默认库master，一个组slave下有两个子库slave_1,slave_2
```

```yaml
# 多主多从   纯粹多库（记得设置primary）  混合配置
spring:       spring:                spring:
  datasource:   datasource:            datasource:
    dynamic:      dynamic:               dynamic:
      datasource:   datasource:            datasource:
        master_1:     mysql:                  master:
        master_2:     oracle:                 slave_1:
        slave_1:      sqlserver:              slave_2:
        slave_2:      postgresql:             oracle_1:
        slave_3:      h2:                     oracle_2:
```

3）使用 @DS 切换数据源。

**@DS** 可以注解在方法上和类上，**同时存在方法注解优先于类上注解**。

【注意】注解在 service 实现或 mapper 接口方法上，但强烈不建议同时在 service和 mapper 注解。 (可能会有问题) 

|     注解      |                   结果                   |
| :-----------: | :--------------------------------------: |
|    没有@DS    |                默认数据源                |
| @DS("dsName") | dsName可以为组名也可以为具体某个库的名称 |

```java
@Service
@DS("slave")
public class UserServiceImpl implements UserService {

  @Autowired
  private JdbcTemplate jdbcTemplate;

  public List<Map<String, Object>> selectAll() {
    return  jdbcTemplate.queryForList("select * from user");
  }
  
  @Override
  @DS("slave_1")
  public List<Map<String, Object>> selectByCondition() {
    return  jdbcTemplate.queryForList("select * from user where age >10");
  }
}
```

在 mybatis 环境下也可注解在 mapper 接口层。

```java
@DS("slave")
public interface UserMapper {

  @Insert("INSERT INTO user (name,age) values (#{name},#{age})")
  boolean addUser(@Param("name") String name, @Param("age") Integer age);

  @Update("UPDATE user set name=#{name}, age=#{age} where id =#{id}")
  boolean updateUser(@Param("id") Integer id, @Param("name") String name, @Param("age") Integer age);

  @Delete("DELETE from user where id =#{id}")
  boolean deleteUser(@Param("id") Integer id);

  @Select("SELECT * FROM user")
  @DS("slave_1")
  List<User> selectAll();
}
```

另可参考更多更细致的内容：https://gitee.com/baomidou/dynamic-datasource-spring-boot-starter/wikis/pages

## 十、多租户 SQL 解析器

这里配合 **分页拦截器** 使用， spring boot 例子配置如下。

```java
@Bean
public PaginationInterceptor paginationInterceptor() {
    PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
    paginationInterceptor.setLocalPage(true);// 开启 PageHelper 的支持
    /*
     * 【测试多租户】 SQL 解析处理拦截器<br>
     * 这里固定写成住户 1 实际情况你可以从cookie读取，因此数据看不到 【 麻花藤 】 这条记录（ 注意观察 SQL ）<br>
     */
    List<ISqlParser> sqlParserList = new ArrayList<>();
    TenantSqlParser tenantSqlParser = new TenantSqlParser();
    tenantSqlParser.setTenantHandler(new TenantHandler() {
        @Override
        public Expression getTenantId() {
            return new LongValue(1L);
        }

        @Override
        public String getTenantIdColumn() {
            return "tenant_id";
        }

        @Override
        public boolean doTableFilter(String tableName) {
            // 这里可以判断是否过滤表
            /*
            if ("user".equals(tableName)) {
                return true;
            }*/
            return false;
        }
    });
    sqlParserList.add(tenantSqlParser);
    paginationInterceptor.setSqlParserList(sqlParserList);
    paginationInterceptor.setSqlParserFilter(new ISqlParserFilter() {
        @Override
        public boolean doFilter(MetaObject metaObject) {
            MappedStatement ms = SqlParserHelper.getMappedStatement(metaObject);
            // 过滤自定义查询此时无租户信息约束【 麻花藤 】出现
            if ("com.baomidou.springboot.mapper.UserMapper.selectListBySQL".equals(ms.getId())) {
                return true;
            }
            return false;
        }
    });
    return paginationInterceptor;
}
```

## 十一、动态表明 SQL 解析

实现 ITableNameHandler 接口注入到 DynamicTableNameParser 处理器链中，将动态表名解析器注入到 MP 解析链。

【注意】原理为解析替换设定表名为处理器的返回表名，表名建议可以定义复杂一些避免误替换。例如：真实表名为 user 设定为 mp_dt_user 处理器替换为 user_2019 等。

## 十二、MyBatisX 快速开发插件

MybatisX 是一款基于 IDEA 的快速开发插件，为效率而生。

安装方法：打开 IDEA，进入 File -> Settings -> Plugins -> Browse Repositories，输入 `mybatisx` 搜索并安装。

项目码云地址：https://gitee.com/baomidou/MybatisX

功能：

1）Java 与 XML 调回跳转

2）Mapper 方法自动生成 MXL













