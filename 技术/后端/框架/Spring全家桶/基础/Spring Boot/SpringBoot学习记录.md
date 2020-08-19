## 问题

## 工具

```
#生成https的密钥
keytool -keystore mykeys.jks -genkey -alias tomcat -keyalg RSA
```



## 开发Spring Boot的动机以及Spring Boot的核心

Spring实用，但是他要求开发者提供大量的配置，例如配置项目的依赖会让开发者不得不去考虑依赖的兼容性问题，而一旦产生兼容性问题就会降低生成立，因此就有了开发一个像Spring Boot这样的引导框架的**动机** ，于是Spring Boot就出现了。

Spring Boot减少了Spring当中的一些必须配置，并且避免了由开发人员管理项目依赖而导致的依赖的版本兼容性问题。

那么，他的解决方案是什么呢？

答：Spring boot四核心。



### Spring boot四核心

自动配置

起步依赖

命令行界面

Actuator

这四个核心对Spring的配置进行了简化，并且提供给开发者对应用程序进行监控的能力（Actuator）。**Spring Boot从本质上来讲，其实就是Spring。**



## 测试

### 集成测试自动配置

```java
@RunWith(SpringJUnit4ClassRunner.class)//开始Spring 集成测试
//@ContextConfiguration(classes=AddressBookConfiguration.class)// 加载应用程序上下文
@SpringApplicationConfiguration(classes=AddressBookConfiguration.class)// 加载应用程序上下文更好的方式
public class AddressServiceTests {
	@Autowired
    private AddressService addressService;//注入地址服务
    @Test
    public void testService() {
    	Address address = addressService.findByLastName("Sheman"); 
        assertEquals("P", address.getFirstName());
        assertEquals("Sherman", address.getLastName());
        assertEquals("42 Wallaby Way", address.getAddressLine1()); 
        assertEquals("Sydney", address.getCity());
        assertEquals("New South Wales", address.getState()); 
        assertEquals("2000", address.getPostCode());
    } }
```



### 测试Web应用程序

```java
import static org.hamcrest.Matchers.*;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes = ReadingListApplication.class)
@WebAppConfiguration//开启Web上下文 测试
public class MockMvcWebTests {
    @Autowired
    private WebApplicationContext webContext;//注入WebApplicationContext
    private MockMvc mockMvc;
    
    @Before
    public void setupMockMvc() {
      mockMvc = MockMvcBuilders
          .webAppContextSetup(webContext)
          .build();//设置MockMvc
    } 
    
    @Test
    public void homePage() throws Exception {
        mockMvc.perform(MockMvcRequestBuilders.get("/readingList")) 
            .andExpect(MockMvcResultMatchers.status().isOk()) 
            .andExpect(MockMvcResultMatchers.view().name("readingList")) 
            .andExpect(MockMvcResultMatchers.model().attributeExists("books")) 
            .andExpect(MockMvcResultMatchers.model().attribute(
              "books",Matchers.is(Matchers.empty())
          ));
	}
    
    @Test
	public void postBook() throws Exception {
        mockMvc.perform(post("/readingList")//执行POST请求
        .contentType(MediaType.APPLICATION_FORM_URLENCODED)
        .param("title", "BOOK TITLE")
        .param("author", "BOOK AUTHOR")
        .param("isbn", "1234567890")
        .param("description", "DESCRIPTION"))
        .andExpect(status().is3xxRedirection())
            .andExpect(header().string("Location", 	"/readingList"));
        
        //配置期望的图书
        Book expectedBook = new Book();
        expectedBook.setId(1L);
        expectedBook.setReader("craig");
        expectedBook.setTitle("BOOK TITLE");
        expectedBook.setAuthor("BOOK AUTHOR");
        expectedBook.setIsbn("1234567890");
        expectedBook.setDescription("DESCRIPTION");
        //执行GET请求
        mockMvc.perform(get("/readingList"))
            .andExpect(status().isOk())
			.andExpect(view().name("readingList"))
			.andExpect(model().attributeExists("books"))
			.andExpect(model().attribute("books", hasSize(1)))
			.andExpect(model().attribute("books",
             contains(samePropertyValuesAs(expectedBook))));



   
```



### 测试运行中的应用程序（随机端口）

```java
@RunWith(SpringJUnit4ClassRunner.class)
    @SpringApplicationConfiguration(
          classes=ReadingListApplication.class)
    @WebIntegrationTest(randomPort=true)//在服务器中运行测试
    public class SimpleWebTest {
        
         @Value("${local.server.port}")
   		 private int port;

          @Test(expected=HttpClientErrorException.class)
          public void pageNotFound() {
            try {
              RestTemplate rest = new RestTemplate();//这是一个测试rest端点的一个好用的工具
              rest.getForObject(
                 "http://localhost:{port}/bogusPage", String.class,port);
              fail("Should result in HTTP 404");
            } catch (HttpClientErrorException e) { 
                        assertEquals(HttpStatus.NOT_FOUND, e.getStatusCode()); 
                        throw e;
            } 
          }
}
```



### 使用Selenium测试HTML页面

```java
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes=ReadingListApplication.class)
@WebIntegrationTest(randomPort=true)//用随机端口启动
public class ServerWebTests {
    
    private static FirefoxDriver browser;
    @Value("${local.server.port}")//注入端口号
    private int port;
      
    @BeforeClass
      public static void openBrowser() {
        browser = new FirefoxDriver();
        browser.manage().timeouts()
       .implicitlyWait(10, TimeUnit.SECONDS);//配置Firefox驱动
     }

      @AfterClass
      public static void closeBrowser() {
          browser.quit();
      }
    
    @Test
    public void addBookToEmptyList() {
        String baseUrl = "http://localhost:" + port;
        browser.get(baseUrl);//获取主页
        assertEquals("You have no books in your book list",
                     browser.findElementByTagName("div").getText());//判断图书列表是否为空
        browser.findElementByName("title").sendKeys("BOOK TITLE");
		browser.findElementByName("author").sendKeys("BOOK AUTHOR");
		browser.findElementByName("isbn").sendKeys("1234567890");
		browser.findElementByName("description").sendKeys("DESCRIPTION");
		browser.findElementByTagName("form").submit();//填充并发送表单
        
        WebElement dl = browser.findElementByCssSelector("dt.bookHeadline");
 		assertEquals("BOOK TITLE by BOOK AUTHOR (ISBN: 1234567890)",dl.getText());
		WebElement dt = browser.findElementByCssSelector("dd.bookDescription");
  		assertEquals("DESCRIPTION", dt.getText());//判断列表中是 否包含新书
    }
 
```



## 部署

1. 将maven中的<packaging>结点改为jar。
2. 替换开发时候的H2数据库（视情况可以省略此步骤）

可以使用一个DataSource类来进行配置。

```java
@Bean
@Profile("production")//注意这里，只有在生产环境的时候才会有这个Bean
public DataSource dataSource() {
    DataSource ds = new DataSource(); ds.setDriverClassName("org.postgresql.Driver"); 		  	  ds.setUrl("jdbc:postgresql://localhost:5432/readinglist");
    ds.setUsername("habuma");
  	ds.setPassword("password");
 	return ds;
}
```

也可以在Application.yml中配置。

```yaml
spring:
      profiles: production
      datasource:
        url: jdbc:postgresql://localhost:5432/readinglist
        username: habuma
        password: password
	  jpa:
	  database-platform: org.hibernate.dialect.PostgreSQLDialect
```



3. 继承**SpringBootServletInitializer**，来指定Spring的配置类。SpringBootServletInitializer是一个支持
   Spring Boot的Spring WebApplicationInitializer实现。

```java
public class ReadingListServletInitializer extends SpringBootServletInitializer{

    
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(ReadinglistApplication.class);
    }
}
```



4. 将应用的jar包或者是war包上传到云服务器。若是使用jar包，则需要在服务器上将此应用程序运行起来。

```shell
nohup java -jar readinglist.jar --server.port=8090 &
```



* 没有看到各个starter中有spring.factories这个文件啊