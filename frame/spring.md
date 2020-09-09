# Spring



## HelloWorld



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