## 一、基础篇

### 1.1 概述

Springboot是Spring提供的一个子项目，用于快速构建Spring应用程序

|      |      |
| ---- | ---- |
|      SpringData| 数据获取 |
| SpringFramework | 核心功能 |
| SpringSecurity | 认证授权 |
| Spring AWQP | 消息传递 |
|Spring Cloud|服务治理|

起步依赖：本质上就是一个Maven坐标，整合了完成一个功能需要的所有坐标

自动配置：遵循约定大约配置的原则，在boot程序启动后，一些bean对象会自动注入到ioc容器，不需要手动声明，简化开发

其他特性：内嵌的Tomcat、Jetty（无需部署WSR文件）、外部化配置、不需要XML配置（properties/yml）

### 1.2 Bean注册

#### 1.2.1 Bean注册

如果要注册的bean对象来自于第三方（不是自定义的），是无法用@Component及衍生注解声明bean的

@Bean 将方法的返回值交给IOC容器管理，成为IOC容器的bean对象     **启动类不推荐**

```java
@Configuration
public class CommonConfig {

    // 注入Country对象
    @Bean
    public Country country() {
        return new Country();
    }

    // 对象默认的名字是：方法名
    // 如果方法的内部需要使用IOC容器中已经存在的bean对象，那么只需要在方法上声明即可，spring会自动注入
    @Bean
    public Province province(Country country) {
        System.out.println("province " + country);
        return new Province();
    }
}

```

@Import 

* 导入配置类
* 导入ImportSelector接口实现类

```java
public class CommonImportSelector implements ImportSelector {
    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        return new String[]{"com.chen.config.CommonConfig"}; // 一般写在配置文件里面
    }
}
```

```java
@SpringBootApplication
//@Import({CommonConfig.class})
@Import(CommonImportSelector.class)
public class SpringbootRegisterApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext context =
                SpringApplication.run(SpringbootRegisterApplication.class, args);
        Country country = context.getBean(Country.class);
        System.out.println(country);

        System.out.println(context.getBean("province"));
    }
}
```

#### 1.2.2  注册条件

SpringBoot提供了设置注册生效条件的注解@Conditional

| 注解                      | 说明                                     |
| ------------------------- | ---------------------------------------- |
| @ConditionalOnProperty    | 配置文件中存在对应的属性，才声明bean     |
| @ConditionalOnMissingBean | 当不存在当前类型的bean是，才声明该bean   |
| @ConditionalOnClass       | 当前环境存在指定的这个类时，才声明该bean |

### 1.3 自动配置原理

 遵循约定大约配置的原则，在boot程序启动后，起步依赖中的一些bean对象会自动注入到ioc容器

> 说一说Springboot的配置原理？
>
> * 在主启动类上添加了SpringBootApplication注解，这个注解组合了EnableAutoConfiguration注解
> * EnableAutoConfiguration注解又组合了Import注解，导入了AutoConfigurationImportSelector类
> * 实现selectImports方法，这个方法经过层层调用，最终读取META-INF目录下的后缀名为imports的文件，当然了boot2.7以前的版本，读取的是spring.factories文件
> * 读取到全类名了之后，会解析注册条件，也就是@Conditional及其衍生注解，把满足注册条件的Bean对象自动注入到IOC容器中

### 1.4 自定义starter

在实际开发中，经常会定义一些公共组件，提供给各个项目团队使用。而在SpringBoot项目中，一般会将这些公共组件封装为SpringBoot的starter



## 二、实战篇

### 2.1 参数校验

使用Spring Validation，对注册接口的参数进行合法性校验

1. 引入Spring Validation起步依赖

   ```xml
   <!-- validation依赖-->
   <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-validation</artifactId>
   </dependency>
   ```

2. 在参数前面添加@Pattern注解

3. 在Controller类上添加@Validated

   ```java
   @RestController
   @RequestMapping("/user")
   @Validated
   public class UserController {

       @Autowired
       private UserService userService;

       @PostMapping("/register")
       public Result register(@Pattern(regexp = "^\\S{5,16}$") String username,
                              @Pattern(regexp = "^\\S{5,16}$") String password) {
           // 查询用户
           User user = userService.findByUsername(username);
           if (user != null) {
               // 用户被占用
               return Result.error("用户名已被占用");
           }
           // 用户名没被占用
           userService.register(username, password);
           return Result.success("成功注册");
       }

   }
   ```

4. 在全局异常处理器中处理参数校验失败的异常

   ```java
   @RestControllerAdvice
   public class GlobalExceptionHandler {

       @ExceptionHandler(Exception.class)
       public Result handleException(Exception e) {
           e.printStackTrace();
           return Result.error(StringUtils.hasLength(e.getMessage()) ? e.getMessage() : "操作失败");
       }
   }
   ```


### 2.2 登录验证

