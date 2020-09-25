# Spring

Spring & SpringBoot

## HelloWorld

### SpringBoot 公司版本

开始页面

```
@RestController
@SpringBootApplication
public class DemoApplication {
    @RequestMapping("/")
    String index() {
        String welcome = "Hello, Spring Boot!";
        return welcome;
    }
    public static void main(String[] args) {
        SpringApplication app = new SpringApplication(DemoApplication.class);
        app.setBannerMode(Banner.Mode.OFF);
        app.run(args);
    }
}
```



pom.xml

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>• 读取配置
配置路径：/src/main/resources/application.properties



Bean 文件

    @Component
    public class WelcomeProperties {
        @Value("{project.global.welcome}")
        private String name;
        
        @Value("{project.global.guest}")
        private boolean isGuest;
        
        // Getter Setter
    }


Bean 批量读取    

    @ConfigurationProperties(prefix = "project.global")
    public class ConfigBean {
        private String guest;
        private String welcome;
        
    	  // Getter Setter
    }

读取

    @RestController
    @SpringBootApplication
    @EnableConfigurationProperties({ConfigBean.class}) // 批量读取
    //@ImportResource({"classpath:some-application.xml"})
    public class DemoApplication {
        WelcomeProperties wp;
        ConfigBean cb;
        // 不建议对实例变量使用 Autowired
        // 可以使用 Set 函数或者构造函数
        @Autowired
        public DemoApplication(ConfigBean cb) {
            this.cb = cb;
        }
        
        @Autowired
        void setWelcomeProperties(WelcomeProperties wp) {
            this.wp = wp;
        }
        @RequestMapping("/")
        String index() {
            String welcome = "Hello, Spring Boot!";
            if (wp != null && cb != null) {
                welcome = wp.getWelcome() + " " + welcome + " " + cb.isGuest();
            }
            return welcome;
        }
    }


读取数据库

1. 创建 UserBean

2. *Dao

  ```
  @Mapper
  public interface UserDao {
    @Select("SELECT * FROM user WHERE name = #{name}")
    User findUserByName(@Param("name") String name);
  }
  ```

  

3. Service

  ```
  @Service
  public class UserService {
    @Autowired
    private UserDao userDao;
    public User selectUserByName(String name) {
        return userDao.findUserByName(name);
    }
  }
  ```

  

4. Controller

  ```
  @RestController
  @RequestMapping("/user")
  public class UserController {
    @Autowired
    private UserService userService;
    @RequestMapping("/query")
    public User testQuery() {
        return userService.selectUserByName("Daisy");
    }
  }
  ```

  



代码顺序

config => model => service => controller => mapper => job



1. create table （建表）
2. model/entity （数据库对应实体类）

```
@Data
public class AdArticleVo {
    private Long pageSize;
    private Long pageIndex;
    private String title;
    private String startTime;
    private String endTime;
    private Integer status;

    public Long getPageIndex() {
        return (pageIndex - 1) * pageSize;
    }
}
```

1. model/dto（出参）

```
@Data
public class AdClassPosterDto {
    private Integer id;

    @TableField(typeHandler = JacksonTypeHandler.class)
    private Node theme;
}
```

1. model/vo（入参）

```
// @Data @EqualsAndHashCode(callSuper=false) 
@Data
@EqualsAndHashCode(callSuper = false)
@TableName("t_xx_xxx")
@ApiModel(value = "XxXxx对象", description = "XxXxx表") // api doc
public class XxXxx implements Serializable {
    private static final long serialVersionUID = 1L; // 控制版本是否兼容

    @ApiModelProperty(value = "主键id")
    @TableId("id")
    private Long id;

    @ApiModelProperty(value = "文章标题")
    private String title;
}
```

1. mapper（???接口）

```
public interface AdArticleMapper extends BaseMapper<AdArticle> {
    //查询文章条数
    int articleCount(@Param("vo") AdArticleVo vo);

    //查询文章列表
    List<AdArticle> articleList(@Param("vo") AdArticleVo vo);

    //根据标签id查询对应标签下的文章列表
    List<AdArticle> getArticleByLabel(@Param("id") Long id, @Param("pageIndex") Long pageIndex, @Param("pageSize") Long pageSize);

    //品牌学院-查询文章数量（分页总页数）
    int articleListByLabelCount(@Param("id") Long id);
}
```

1. service（服务接口）

```
public interface AdArticleService extends IService<AdArticle> {
    int articleCount(AdArticleVo vo);
}
```

1. serviceImpl（服务实现）

```
// ServiceImpl<mapper, entity> 顺序不能错
public class AdArticleServiceImpl extends ServiceImpl<AdArticleMapper, AdArticle> implements AdArticleService {
    @Resource
    private AdArticleMapper adArticleMapper;
    @Resource
    private AdLabelService adLabelService;


    @Override
    public int articleCount(AdArticleVo vo) {
        return 0;
    }
}
```

1. controller（业务逻辑）

```
@RestController
@RequestMapping("/ad")
public class AdArticleController {


    @Resource
    private AdArticleService adArticleService;


    /**
     * 文章管理-查询文章列表接口
     * @param vo
     * @return
     */
    @PostMapping("/articleList")
    public RestResult articleList(@RequestBody AdArticleVo vo){
        int restCount = adArticleService.articleCount(vo);
        List<AdArticle> list = adArticleService.articleList(vo);
        Map<String,Object> map = new HashMap<String,Object>();
        map.put("restCount",restCount);
        map.put("list",list);
        return RestResult.success(map);
    }
}
```

1. model/enum（状态/类型/错误信息）

```
@AllArgsConstructor
public enum XxXxxEnum implements IError {
    ADD_EDIT_AD_ERROR(3000, "新增编辑轮播图失败"),
    UPDATE_AD_STATUS_ERROR(3001, "修改轮播图状态失败"),
    ADD_SEARCH_HOT_ERROR(3002, "新增热词失败"),
    ADD_EDIT_ARTICLE_ERROR(3003, "新增编辑文章失败"),
    @Getter
    private int code; // 状态码
    @Getter
    private String msg; // Message 返回消息
}
```

1. job（定时任务）

```
@JobHandler(value="xxXxxJobHandler")
@Component
public class XxXxxJobHandler extends IJobHandler {

    @Autowired
    private XxXxxService adRotaryPlantingMapService;

    @Override
    public ReturnT<String> execute(String param) throws Exception {
        XxlJobLogger.log("XXL-JOB,更新轮播图状态开始");
        XxXxxService.scheduleUpdownShelf();//定时任务修改状态（自动下架）
        XxXxxService.scheduleUpperShelf();//定时任务修改状态（自动上架）
        XxlJobLogger.log("XXL-JOB,更新轮播图状态结束");
        return SUCCESS;
    }
}
```







## 概述

Spring是一个软件设计层面的，企业级开发框架。优势在于可以将应用程序分层，开发者可以自主选择组件。

- MVC：Struts2，Spring MVC
- ORMapping：Hibernate，MyBatis，Spring Data



企业级项目特点

- 大规模：用户数量多，数据规模大，功能模块多
- 性能和安全要求高
- 业务复杂
- 灵活多变



![image-20200917101549640](/Users/strk2333/Documents/Workspace/Doc/roadmap/frame/spring_assets/image-20200917101549640.png)



优点

- 低侵入式设计
- 独立于各种应用服务器
- 依赖注入特性将组件关系透明化，降低了耦合度
- 面向切面编程特性允许将通用任务进行集中式处理
- 与第三方框架的良好整合









































## IoC DI





































## AOP

































## 注解

### @Autowired

1. 修饰变量
2. 修饰构造方法（单一参数，多参数）
3. 修饰 set 方法
4. 修饰普通方法



组件可为空 @Autowired(required=false)





### @Component

加入容器控制



自定义id @Component("userService")



其他语义定制注解

- @Controller
- @Service
- @Repository (dao)



### @Configuration

标记为配置文件



### @ComponentScan

扫描所在包下的所有类

指定扫描范围

- @ComponentScan("com.xxx.xxx")
- @ComponentScan(basePackages = {"com.xxx.xxx", "com.xxx.yyy"})
- @ComponentScan(basePackages = {UserController.class, UserService.class, UserDao.class})



### @RunWith(SpringJUnit4ClassRunner.class)





### @ContextConfiguration

- 注解配置 @ContextConfiguration(classes=AppConfig.class)
- XML配置 @ContextConfiguration("classpath:applicationContext.xml")



### @Test



### @Qualifier @Primary

- 处理装配歧义
  - @Primary
  - 使用 @Qualifier
    - @Qualifier("")
    - @Component("name")
    - 使用默认首字母小写的变量名
    - 装配使用 @Resource(name="") 或 @Autowired @Qualifier("")



### @Resource

@Autowired @Qualifier("userService") = @Resource(name="userService")

Autowired, Qualifier 是 Spring 标准，Resource 是 javax.annotation.Resource, JDK的标准









### @Bean

```
// 放在 @Configuration 里
@Bean
public UserService userService(@Qulifier("name") UserDao userDao) {
	// UserDao userDao = userDao(); // 拦截，返回已存在对象
	return new UserService(userDao); // 构造方法注入，也可以用setter, 甚至任意方法注入
}
```



装配第三方类

消除歧义方法同上



### @Scope

配置 bean 作用域

1. 在 bean 声明类上配置
2. 在 bean 声明变量上配置

```
@Scope("prototype")
@Scope(scopeName="prototype")
@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
```



### @Lazy

延迟加载，只和 Scope 为 Singleton 的匹配

1. 在 bean 声明类上配置
2. 在 bean 声明变量上配置



### @PostConstruct @PreDestory

初始化方法，销毁方法







注：没有关于工厂模式的 Spring 注释



## XML配置

resources 下新增 applicationContext.xml

```xml
<!-- 相当于 @Configuration, xmlns:c c 名称空间 -->
<beans 
       xmlns="https://www.springframework.org/schema/beans"
       xmlns:c="https://www.springframework.org/schema/c"
       xmlns:p="https://www.springframework.org/schema/p"
       xmlns:util="https://www.springframework.org/schema/util"
       xsi:schemaLocation="https://www.springframework.org/schema/beans
                           https://www.springframework.org/schema/spring-beans
                           https://www.springframework.org/schema/util
                           https://www.springframework.org/schema/spring-util.xsd">
  <!-- 组件扫描范围 -->
  <context:component-scan base-package="com.nogi.dao" />
  <context:component-scan base-package="com.nogi.service" />
  
  
  <!--------------------------------------------------------------->
  <!-- bean -->
  <!-- name 多个别名, 没有name，id，自动生成类似 com.nogi.service#0 的id -->
  <bean id="nanase" name="xxx yyy,zzz;ttt" class="com.nogi.entify.Member" />
  
  
  <!--------------------------------------------------------------->
  <!-- 依赖注入1 -->
  <!-- 构造函数 -->
  <bean id="jingu2017" class="com.nogi.entity.Live">
    <constructor-arg ref="nanase" />
    <constructor-arg (name="location")
                     (index="0")
                     (type="java.lang.String")
                     value="Tokyo Jingu" />
    <constructor-arg value="2017-08-22" />
    <constructor-arg name="memberList">
      <list> <!-- list / set(LinkedHashSet) / array -->
        <value>nanase</value> <!-- 基本类型 -->
        <value>renka</value>
        <value>yoda</value>
        <value>ten</value>
        <value>momoko</value>
        
        <ref bean="nanase" /> <!-- 实例注入 -->
        <ref bean="shiraishi" />
      </list>
      <map>
      	<entry key="01" value="ayame" />
      	<entry key="02" value="kaki" />
      	<entry key="03" value-ref="saku" />
      </map>
    </constructor-arg>
  </bean>
  
  
  <!--------------------------------------------------------------->
  <!-- 依赖注入2 -->
  <!-- c名称空间 xxx 为构造函数参数名，可以用 _0 表示第一个参数 -->
  <!-- c名称空间，无法进行集合装配 -->
  <bean id="nigemizu" 
        class="com.nogi.entity.Song" 
        c:_0-ref="yoda"
        c:_1-ref="momoko"
        c:-ref="nanase"
        c:fullName="nigemizu" /> <!-- 加 -ref 为注入实例 -->
  
  
  <!--------------------------------------------------------------->
  <!-- 属性注入1 -->
  <!-- property 注入，需要类中属性有 set 方法 -->
  <bean id="route246" class="com.nogi.entity.Song">
    <property name="title" value="Route246" /> <!-- setTitle v setTitle1 x -->
    <property name="center" ref="asuka" />
    <property name="memberList">
      <array>
      	<value ref="asuka" />
      	<value ref="ikuta" />
      	<value ref="miona" />
      	<value ref="renka" />
      </array>
    </property>
  </bean>
  
  
  <!--------------------------------------------------------------->
  <!-- util名称空间, 支持list, map, set... -->
  <util-list id="aozoraMemberList">
    <ref beans="ikuta" />
    <ref beans="ikoma" />
    <ref beans="minami" />
  </util-list>
  
  
  <!--------------------------------------------------------------->
  <!-- 属性注入2 -->
  <!-- p名称空间 -->
  <bean id="aozora" 
        class="com.nogi.entity.Song" 
        p:fullName="nandomenoaozora" 
        p:center-ref="ikuta"
        p:memberList-ref="aozoraMemberList">
  </bean>
  
  
  <!--------------------------------------------------------------->
  <!-- bean 作用域 -->
  <!-- singleton 获取实例时，返回单实例。初始化创建实例 -->
  <!-- prototype 获取实例时，创建新实例。初始化不创建实例 -->
  <bean id="staff" class="com.nogi.entity.Staff" scope="prototype" />
  
  
  <!--------------------------------------------------------------->
  <!-- 延迟加载 -->
  <!-- 获取实例时，创建实例 -->
  <bean id="cameraman" class="com.nogi.entity.CameraMan" lazy-init="true" />
  
  
  <!--------------------------------------------------------------->
  <!-- 初始化方法，销毁方法 -->
  <bean id="soundman" 
        class="com.nogi.entity.SoundMan" 
        init-method="init"
        destroy-method="destory" />
  
  
  <!--------------------------------------------------------------->
  <!-- 工厂方法 -->
  <!-- 静态工厂方法 -->
  <bean id="schedule1"
        class="com.nogi.entity.Manager"
        factory-method="createSchedule" />
  
  <bean id="manager2"
        class="com.nogi.entity.Manager" />
  
  <!-- 实例工厂方法 -->
  <bean id="schedule2"
        factory-bean="manager2"
        factory-method="createSchedule" />
  
</beans>
```



```java
@ContextConfiguration("classpath:applicationContext.xml") // 注解

... = new ClassPathXmlApplicationContext("applicationContext.xml"); // XML
Xxx xxx = context.getBean(Xxx.class);
// context.getBean("xxx.xxx.xxx.Xxx#0");
// context.getBean("xxx01");

```



```xml
<dependencies>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>4.3.13.RELEASE</version>
  </dependency>
  
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>4.3.13.RELEASE</version>
  </dependency>
  
  <!-- 还需要导入 log4j.properties -->
  <dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
  </dependency>
  
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
  </dependency>
</dependencies>
```



