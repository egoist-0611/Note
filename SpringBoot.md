## 核心特性

### 介绍

SpringBoot可以让我们快速创建一个独立的、生产级别的应用程序

大多数时候，我们只需编写少量的配置，就可以实现快速整合Spring及第三方技术

1. 内嵌 Tomcat / Jetty / Undertow 等Servlet容器
2. 提供可选的starter启动器，简化依赖的整合
3. 若有需求，有自动配置的Spring及第三方库（约定大于配置，很多配置都有默认选项）
4. 提供生产特性，如监控指标、健康检查、外部文件配置等
5. 无代码生成、无xml

总结：简化开发、简化部署、简化整合、简化配置、简化监控运维









### 快速入门

#### 构建流程

1. 让工程继承于 spring-boot-starter-parent

   ```xml
   <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>3.0.5</version>
   </parent>
   ```

2. 导入相关的启动器（依赖，如：SpringMVC所需要的 spring-boot-starter-web 启动器）

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-web</artifactId>
   </dependency>
   ```

3. 创建主程序（SpringBoot启动入口）

   ```java
   @SpringBootApplication			// 核心注解，表明该类是SpringBoot的启动类
   class 类名{
       public static void main(String[] args){
           SpringApplication.run(类名.class,args);
       }
   }
   // 当执行main方法时，程序会web启动器内置的Tomcat中部署、运行
   ```

4. 正常写业务逻辑、测试

5. 打成可直接运行的jar包

   - 导入插件：spring-boot-maven-plugin

     ```xml
     <build>
         <plugins>
             <plugin>
                 <groupId>org.springframework.boot</groupId>
                 <artifactId>spring-boot-maven-plugin</artifactId>
             </plugin>
         </plugins>
     </build>
     ```

   - 运行maven的package命令进行打包

   - 通过 `java -jar 包名.jar` 运行程序



Spring Initializr：更快速的创建SpringBoot项目







#### 应用分析

1. 依赖处理：
   - 启动器：会将该场景功能所需要的所有依赖进行导入
   - 版本号：SpringBoot项目需要继承父项目：spring-boot-starter-parent，其父项目 spring-boot-dependencies 中，将所有常见的依赖进行了管理。因此，若是在父项目中存在被管理的依赖，我们就不需要再在导入依赖时声明版本号
   
2. 自动配置：

   - 自动装配组件：SpringApplication.run() 运行后，会返回一个IOC容器，该IOC容器中就存放了大多数常用的核心组件（通过 `返回的IOC容器对象.getBeanDefinitionNames()` 可以获取到这些组件的名称）

   - 默认的包扫描规则：SpringBoot默认只会扫描主程序类（@SpringBootApplication标记的类）所在的包及其子包

     > 重新定义扫描路径：
     >
     > - `@SpringBootApplication(scanBasePackages = "包路径")`
     > - @SpringBootApplication 实际上是由 @SpringBootConfiguration、@EnableAutoConfiguration、@ComponentScan 这三个注解复合形成的。我们可以直接使用这三个注解来代替，并使用 `@ComponentScan("包路径")` 来进行重新定义包扫描的路径

   - 已有的默认值配置：配置文件中的很多配置项都是有默认值的（这些配置项都是和某个类的属性匹配的，这些类也被称为配置属性类。比如：ServerProperties 就绑定了所有和 Tomcat服务器配置项 有关的）

   - 按需自动加载：启动器在导入场景所需要的相关依赖后，还会导入一个 spring-boot-starter，是一个所有启动器都有的核心、基本starter。在该starter中，会导入一个 spring-boot-autoconfigure 包，该包下包含所有场景的自动配置类。当导入启动器后，就会根据需要的场景，启用该包下对应的自动配置类











#### 基础使用

##### 常用注解

配置类：

1. `@Configuration`：标示该类是一个配置类，代替xml配置文件（配置类本身也会作为容器中的组件）
2. `@SpringBootConfiguration`：与 @Configuration注解 相同的用法



组件：

1. `@Bean`：使用在方法上，标示该方法返回的值将会作为IOC容器中的组件，替代xml配置文件中的bean标签（组件的id默认为方法名，可使用value属性值指定）
   - 默认情况下，该方式创建的组件是单例，可使用 `@Scope("prototype")` 指定为多例
2. `@Import(Class)`：使用在类上，将指定类型的组件存放于IOC容器中，组件的id默认为全类名（一般用来创建第三方类型的组件）







##### 条件注解

- 当注解指定的条件成立时，就会执行指定行为

- 当声明在类（如配置类）上时，只有当注解要求的内容存在，该配置类才会生效（并添加进IOC容器）

  当声明在方法上（如@Bean修饰时），只有当注解要求的内容存在，该组件才会被添加进IOC容器中



`@ConditionalOnClass(name="全类名")`：当类路径下存在该类时，则会执行指定行为

`ConditionalOnMissingClass(value="全类名")`：当类路径下不存在该类时，才会执行指定行为

> 这里的类路径包括编译时类路径以及运行时类路径
>
> - 编译时类路径：通常指 main/java目录下的类
> - 运行时类路径：通常指 maven引入的依赖（jar包下的类）



`@ConditionalOnBean(name="组件id")`：当IOC容器中存在该组件时，则会执行指定行为

`@ConditionalOnMissingBean(name="组件id")`：当IOC容器中不存在该组件时，才会执行指定行为







##### 属性绑定

`@ConfigurationProperties(prefx="前缀")`：将配置文件的配置项与该类的属性进行绑定

- 使用该注解需要有几个条件：

  ① 该类被添加进IOC容器中（使用@Component注解，或者在配置类中使用 @Bean注解）

  ② 该类提供get、set方法

  ③ 只有当配置项与 `前缀.属性名` 匹配时，才会将配置项的值赋值给属性

- 该注解是可以用在方法上的（如@Bean标注的方法），那么会将该方法返回的组件进行属性绑定



`@EnableConfigurationProperties(类名.class)`：当类上使用了@ConfigurationProperties注解，那么使用该注解就可以直接将该类存入到IOC容器中，并进行属性绑定

- 该注解一般是用于对第三方写好了的组件进行属性绑定







#### 原理讲解

一、导入场景启动器starter

1. 当导入启动器后，会导入相关功能所需要的所有依赖

2. 此外，所有的启动器都会再额外导入一个核心场景启动器：spring-boot-starter。而在这个核心场景启动器中，会引入一个依赖（包）：spring-boot-autoconfigure

3. 在这个包中，存放了所有的场景 需要用到的默认配置。因此，只要能够让这些组件生效（被扫描注册），就能实现快速配置（快速整合功能）

   > 但是，SpringBoot默认只扫描主程序所在的包及其子包，是扫描不到spring-boot-autoconfigure包下的组件的。那么，是怎么让这些组件生效的呢？

二、@SpringBootApplication注解标识的主程序

1. @SpringBootApplication注解，是由三个注解复合而成的：@SpringBootConfiguration、@EnableAutoConfiguration、@ComponentScan
2. @SpringBootConfiguration标示该类是一个配置类，@ComponentScan用来进行包扫描，而@EnableAutoConfiguration，就是开启自动配置（扫描spring-boot-autoconfigure包下的组件）的核心注解
3. 查看@EnableAutoConfiguration注解，可以看到`@Import({AutoConfigurationImportSelector.class})`这个注解。进入AutoConfigurationImportSelector这个类中，里面有个 `getCandidateConfigurations` 方法，该方法会获取到 spring-boot-autoconfigure包 下的 `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`文件 中的142个配置类。@Import注解，目的就是载入这142个配置类（组件）
   - 但是，这142个配置类并不是都会被加载。我们查看配置类，可以发现绝大多数的配置类，都是有条件注解 @ConditionalOnClass 标示的。当我们载入了所需要的类（导入依赖）时，这些配置类满足要求了，才会被加载
   - 通过加载 META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports 文件，实现自动对这些配置类进行加载，这种思想被称为 SPI（Service Provider Interface），用于在应用程序中动态发现和加载组件
4. 在这些配置类中，很多类上又使用了 @EnableConfigurationProperties 进行属性绑定，许多组件的创建，都是依靠读取到的属性进行配置的。因此，通过修改 application.yaml 配置文件，实现对组件的参数的修改

综上，就是SpringBoot自动配置的实现原理















### Web整合

#### 介绍

查看 org.springframework.boot.autoconfigure.AutoConfiguration.imports 配置文件，所有与web开发相关的包：client、embedded、servlet

查看这些包下的类，检查 @EnableConfigurationProperties 注解，获取与application配置文件绑定的配置项：

1. `spring.mvc`：SpringMVC的所有配置
2. `spring.web`：Web场景通用配置
3. `spring.servlet.multipart`：文件上传配置
4. `server`：服务器配置（如：编码方式、端口号等）



SpringBoot提供的默认配置功能：

1. 包含了 `ContentNegotiatingViewResolver` 和 `BeanNameViewResolver` 组件（两个视图解析器），方便视图解析
2. 默认静态资源处理机制（静态资源存放于static文件夹下，即可直接访问）
3. 注册了 `Converter`、`GenericConverter`、`Formatter` 组件，适配常见的数据类型转换和格式要求（如：将application配置文件中的配置项（字符类型）转换为对应绑定的Bean对象的属性的类型）
4. 支持 `HttpMessageConverters`，更方便的返回JSON格式数据
5. 注册了 `MessageCodesResolver` 组件，更好的进行国际化和错误消息处理
6. 支持解析静态 `index.html`
7. 自动使用 `ConfigurableWebBindingInitializer`，实现消息处理、数据绑定、类型转换等功能

> 若是要禁用SpringBoot默认的配置功能，只需创建一个配置类（@Configuration），在配置类上标注 `@EnableWebMvc` 注解
>
> 若是想要使用默认配置，又想要添加自定义的核心组件，那么只需创建一个配置类，让该配置类去实现 `WebMvcConfigurer` 接口，实现相关功能的方法即可









#### WebMvcAutoConfiguration源码分析

##### 整体分析

1. 生效条件：

   ```java
   @AutoConfiguration(after={DispatcherServletAutoConfiguration.class,TaskExecutionAutoConfiguration.class,ValidationAutoConfiguration.class})		// 该注解是让{}内的配置类全部加载完成后，才加载该配置类
   
   @ConditionalOnWebApplication(type = Type.SERVLET)	
   // 只能是web应用才能加载该配置类
   @ConditionalOnClass({Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class})
   // 只能存在{}内的这些类才能加载该配置类
   @ConditionalOnMissingBean({WebMvcConfigurationSupport.class}) 
   /* 不存在该bean时才能加载配置类（@EnableWebMvc可以使底层的自动配置失效的原因）
   	@EnableWebMvc注解会@Import间接引入一个类：WebMvcConfigurationSupport
   	而要让WebMvcAutoConfiguration配置类生效，就要求不能存在WebMvcConfigurationSupport这个类
   */
   
   @AutoConfigureOrder(-2147483638)		// 优先级
   @ImportRuntimeHints({WebResourcesRuntimeHints.class})AutoConfiguration.class})
   ```

2. 存放了两个Filter：

   - HiddenHttpMethodFilter：用于表单提交 PUT、DELETE 请求
   - FormContentFilter：用于表单提交时，发送 PUT、DELETE 请求时，携带请求体
   
3. 导入了 EnableWebMvcConfiguration

   - welcomePageHandlerMapping()：欢迎页功能
   - createExceptionHandlerExceptionResolver：添加默认的异常解析器
   - localeResolver()：国际化解析器
   - mvcConversionService()：数据格式化、数据类型转换
   - mvcValidator()：数据校验功能
   - mvcContentNegotiationManager()：内容协商管理器

4. 导入了 WebMvcAutoConfigurationAdapter

   - 实现 WebMvcConfigurer 底层组件的默认功能

     - configurePathMatch：配置路径匹配规则（如：`/路径/**` 能匹配到 `/路径/路径`、`/路径/路径/路径`）
     - configureContentNegotiation：配置内容协商
     - configureAsyncSupport：配置异步的支持
     - configureDefaultServletHandling：配置默认处理的请求（一般都为 `/`，表示所有请求）
     - addFormatters：添加格式化器（如：application配置文件中的字符转换为Bean对象对应类型属性的值）
     - addInterceptors：添加拦截器
     - addResourceHandlers：添加资源处理器（一般用来添加静态资源处理器）
     - addCorsMappings：添加跨域规则
     - addViewControllers：添加视图控制器（让请求直接跳转到静态页面，无需手写Servlet跳转）
     - configureViewResolvers：配置视图解析规则
     - addArgumentResolvers：添加参数解析器（Servlet参数处理）
     - addReturnValueHandlers：添加返回值处理（Servlet返回值处理）
     - configureMessageConverters：配置消息转换器
     - extendMessageConverters：扩展消息转换器
     - configureHandlerExceptionResolvers：配置异常解析器
     - extendHandlerExceptionResolvers：扩展异常解析器

   - defaultViewResolver()：视图解析器

   - viewResolver()：内容协商解析器

   - requestContextFilter()：可在任意位置获取请求对象

     > ```java
     > ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
     > HttpServletRequest request = attributes.getRequest();
     > HttpServletResponse response = attributes.getResponse();
     > // 在service等地方也能直接获取到请求和响应对象
     > ```

5. 配置了静态资源链规则

6. ProblemDetailsExceptionHandler：内部定义了一些HTTP常见错误（异常）。当SpringMVC异常处理机制无法解决错误时，会先由该handler进行处理，若没有匹配的错误，则再由SpringBoot默认的错误处理机制处理









##### 静态资源处理

###### 源码分析

```java
@Configuration(proxyBeanMethods = false)
@Import({EnableWebMvcConfiguration.class})		// 额外导入了一些其他配置
@EnableConfigurationProperties({WebMvcProperties.class, WebProperties.class})
// 功能的配置与application配置文件中的配置项绑定在一起
@Order(0)
public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer, ServletContextAware {}
```

相关方法：addResourceHandlers()、addResourceHandler()

在 addResourceHandlers 方法中，两个 `addResourceHandler()` 方法添加了两个静态资源路径匹配规则：

1. 当访问 `/webjars/**` 路径时，就去 `classpath:/META-INF/resources/webjars/` 目录下寻找资源
2. 当访问 `/**` 路径时，就到 四个默认的静态资源路径 下寻找资源：
   - `classpath:/META-INF/resources/`
   - `classpath:/resources/`
   - `classpath:/static/`
   - `classpath:/public/`

在 addResourceHandler 方法中，`addResourceHandler()` 方法中添加了缓存规则（浏览器访问静态资源后，会进行缓存。若下次浏览器访问该资源，该资源在服务器中没有发生变化，那么浏览器会直接使用自己缓存的资源）

- `setCachePeriod()`：设置缓存周期（多久时间不使用该资源后会失效），以秒为单位（默认无值）
- `setCacheControl()`：设置HTTP的缓存控制
- `setUseLastModified()`：设置是否使用最后一次修改（当浏览器访问静态资源时，会先获取服务器对该资源的最后一次修改时间，看看是否发生了修改；若没有，则使用缓存。配合 HTTP缓存控制 使用）







###### 配置规则

一、配置静态资源映射规则

```java
this.addResourceHandler(registry, this.mvcProperties.getWebjarsPathPattern(), "classpath:/META-INF/resources/webjars/");
```

查看 getWebjarsPathPattern()，发现该 String webjarsPathPattern 是在 WebMvcProperties 这个类下的，其绑定的application配置文件配置项的前缀为 `spring.mvc`

通过 spring.mvc，我们可以自定义静态资源路径的匹配规则：

1. 更改对webjars资源的路径匹配规则：`spring.mvc.webjars-path-pattern=/路径/**`
2. 更改对静态资源访问的路径匹配规则：`spring.mvc.static-path-pattern=/路径/**`
   - 当访问 `/路径/**` 下的资源时，就去默认的四个静态资源路径下找
   - 通过 `spring.web.resources.static-locations=classpath:/路径1/,classpath:/路径2/` 来设置静态资源的存储位置 



二、配置HTTP缓存控制规则

```java
registration.setCacheControl(this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl());
```

查看 this.resourceProperties，可看到该对象时属于 WebProperties.Resources 类下的，而该类，绑定application配置文件配置项的前缀为 `spring.web`

观察其属性，可以推出 spring.web 可以配置的内容：

1. 配置国际化的区域信息
2. 静态资源策略（Resources）
   - 开启静态资源的映射：`spring.web.resources.add-mapping=true`（默认为true）
   - 设置缓存：
     - `spring.web.resources.cache.use-last-modified=true`：开启 是否对比服务器和浏览器资源的最后一次修改时间是否相同 的功能（默认为true，相同返回304）
     - `spring.web.resources.cache.period=秒数`：浏览器第一次请求服务器，服务器会告诉浏览器将此资源缓存x秒。在此时间内访问该资源时，use-last-modified 为 false，则直接使用缓存；use-last-modified 为 true 时，则会发送一个对比请求，若返回304，则直接使用缓存
     - `spring.web.resources.cache.cachecontrol.max-age=秒数`：与 period 功能相同（更精确：即优先级更高，会覆盖 period 参数）



通过代码也实现配置：

方式一：

1. 创建一个配置类，实现 WebMvcConfigurer 接口

   ```java
   @Configuration
   class 类名 implements WebMvcConfigurer{}
   ```

2. 添加 addResourceHandlers 功能组件

   ```java
   @Override
   public void addResourceHandlers(ResourceHandlerRegistry registry) {
       WebMvcConfigurer.super.addResourceHandlers(registry);
       // 保留默认规则
   
       registry.addResourceHandler("/路径/**")			// 更改对静态资源访问的路径匹配规则
           .addResourceLocations("classpath:/路径/")		// 设置静态资源的存储位置
           .setCacheControl(CacheControl.maxAge(秒数, TimeUnit.SECONDS));   // 设置缓存规则
   }
   ```



方式二：我们直接存放一个新的WebMvcConfigurer组件，该组件中实现了 addResourceHandlers 功能，去替换底层使用的WebMvcConfigurer

```java
@Configuration
class 类名{
    @Bean
    public WebMvcConfigurer webMvcConfigurer(){
        return new WebMvcConfigurer() {
            @Override
            public void addResourceHandlers(ResourceHandlerRegistry registry) {
                WebMvcConfigurer.super.addResourceHandlers(registry);
            }
        };
    }
}
```

- 这样做也能使 WebMvcConfigurer 组件中的功能生效的原因：

  ```java
  public static class EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration implements ResourceLoaderAware {}
  ```

  在 DelegatingWebMvcConfiguration 中，会通过自动注入的方式将 WebMvcConfigurer 注入进来，并在该类中，会通过类似代理的方式，调用 WebMvcConfigurer 中的功能









##### 默认页面跳转

```java
public static class EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration implements ResourceLoaderAware {}
// DelegatingWebMvcConfiguration这个类是继承于 WebMvcConfigurationSupport 这个类的：当我们在IOC容器中已经存在该类时，就不会加载WebMvcAutoConfiguration这个配置类。（但是，我们只有加载了这个配置类后，才会创建该类）
```

相关方法：welcomePageHandlerMapping()

- HandlerMapping：根据请求路径，找对应的Handler（方法）进行处理

当在默认的四个静态资源路径下存在一个 `index.html` 页面，即会将该页面作为项目的默认启动页面



SpringBoot还存在一个隐性规则：当默认的四个静态资源路径下，存在一个 `favicon.ioc` 文件，则会将该文件作为页面的标签图标









##### 路径匹配策略

在设置handler的映射路径时，我们可以在路径中使用如 `*`（匹配多个字符）、`?`（匹配一个字符）等匹配路径。而使用的路径匹配策略，在SpringBoot5.3之后有了些变化：

- SpringBoot5.3之后，使用 `path_pattern_parser` 代替了原版的 `ant_path_matcher`

  对比之下，新版提高了性能、节省了空间
  但是，新版的 `**`（匹配多层路径）只能使用在映射路径的末尾，否则会报错

- 在WebMvcAutoConfiguration类中，通过 `configurePathMatch` 方法可查看源码

- 在application配置文件中，通过 `spring.mvc.pathmatch.matching-strategy=策略` 来进行修改











#### 日志

##### 介绍

日志门面（相当于JDBC）：JCL、SLF4j、jboss-logging

日志实现（相当于mysql驱动包）：Log4j、JUL（java.util.logging）、Log4j2、Logback



Spring底层使用的是 commons-logging 作为日志功能的实现

> 我们调用commons-logging中的API，由commons-logging去调用日志的门面。这样在更换日志门面时，不必再修改源代码

SpringBoot整合了commons-logging的功能，底层支持JUL、Log4j2、Logback等日志实现，默认使用的是 `logback + slf4j` 作为底层日志，且默认会在控制台输出日志信息（也可输出到外部文件中）

- SpringBoot配置日志功能的流程：
  1. 每个启动器，都会有一个核心启动器 spring-boot-starter。在这个核心启动器中，会引入日志的核心功能依赖包：spring-boot-starter-logging
  2. 日志功能是服务器一启动时就会调用的，不再是通过 xxxAutoConfiguration 的机制（通过引入依赖包）实现调用，而是通过 监听器机制（ApplicationListener） 直接启用
  3. 日志的配置也是可以通过修改application.yaml来实现：`logging` 开头的所有配置都是与日志功能相关的







##### 日志格式

SpringBoot默认的日志格式：

日期时间 -> 日志级别 -> 进程ID -> [ 线程名 ] -> 产生日志的类的类名 -> 日志信息

在application配置文件中，通过 `logging.pattern.console=xxx` 可以自定义在控制台输出的日志格式

> 在spring-boot包下additional-spring-configuration-metadata.json文件中可查看默认格式

格式：

- `%clr()`：绿色显示()内的内容
- `%d{yyyy-MM-dd HH:mm:ss}`：输出年月日时分秒
- `%level`：显示日志级别
- `${PID}`：显示进程ID
- `%thread`：显示线程名
- `%logger`：显示产生日志的类的类名
- `%msg`：显示日志信息
- `%n`：换行







##### 日志信息

方式一：

```java
// 1.获取org.slf4j.Logger下的Logger类
Logger logger = LoggerFactory.getLogger(getClass());
	// 一般情况下，一个类中有一个Logger对象即可，因此可以声明为全局变量

// 2.调用Logger对象的info()输出INFO级别的日志信息
logger.info("日志信息 --> {}",变量);
	// 也可使用 trace()、debug() 等输出其他级别的日志信息（需要级别等于或高于当前默认级别才会输出）
	// {}用来输出变量的值
```



方式二：SpringBoot已经帮我们整合了默认的日志功能，我们只需再导入 `lombok` 依赖后便可直接调用

```java
@Controller			// 控制层标识
@Slf4j
class 类名(){
    @GetMapping("/请求路径")
    public 返回值类型 方法名(){
        log.info("日志信息");
    }
}
// 在使用了@Slf4j这个注解后，就会存在 log 这个变量。通过该变量调用 info() 即可输出指定的日志信息
```







##### 日志级别

由低到高分别为：

1. `ALL`：打印所有的日志信息
2. `TRACE`：追踪框架的详细流程（一般不会使用）
3. `DEBUG`：开发调试的细节
4. `INFO`：关键信息
5. `WARN`：警告但并非错误的信息（如版本过时等）
6. `ERROR`：业务错误信息（如各种异常信息）
7. `FATAL`：致命错误信息（如JVM系统崩溃）
8. `OFF`：关闭所有的日志信息

不指定级别时，默认使用的是root指定的级别作为默认级别（默认为 INFO）

在输出日志信息时，会输出级别等于或大于默认级别的信息（如：默认级别为INFO级别，则可以输出INFO、WARN、ERROR、FATAL级别的日志信息）



在application配置文件中，通过 `logging.level.root=日志级别`，可以修改root的默认级别

通过 `logging.level.全类名=日志级别`，可以修改该类使用的日志级别

- `logging.group.组名=全类名1,全类名2`，`logging.level.组名=日志级别`

  将某些类划分为一个组，再为这个组指定日志级别，这样可以快速为某些类指定日志级别







##### 日志输出

在application配置文件中：

1. `logging.file.name=盘符:\\路径\\文件名.log`：指定日志文件生成后存放的路径及其文件名
   - 若不指定路径，则默认存放在：与当前项目同级的目录中
2. `logging.file.path=盘符:\\路径`：将生成的日志文件存放于指定路径下，文件名为spring.log



归档：当天输出到文件中的日志会存储到一个文档中

切割：若每个文档的大小超过指定大小，则会切割出另一个文档

- `logging.logback.rollingpolicy.file-name-pattern`：设置存储的文档的名称

  >默认为：`${LOG_FILE}.%d{yyyy-MM-dd}.%i.gz`
  >
  >- ${LOG_FILE}：文件名
  >- %d{yyyy-MM-dd}：日期
  >- %i：若文档超过指定大小即会进行切割，%i即为文档编号（从0开始计数）

- `logging.logback.rollingpolicy.max-file-size`：设置文档超过指定大小时进行切割（默认为10MB）

- `logging.logback.rollingpolicy.total-size-cap`：设置全部存储的文档的大小超过指定大小后就进行清空（默认为0B，表示不清空）

- `logging.logback.rollingpolicy.max-history`：设置存储的文档的保存天数（默认为7天）









##### 自定义日志系统

SpringBoot底层默认使用的是logback日志系统，但也提供了log4j2日志系统

我们只需在导入依赖时，根据依赖导入的就近原则，手动导入 spring-boot-starter-web，并排除掉 spring-boot-starter-logging 包的导入，然后再导入 `spring-boot-starter-log4j2` 的依赖，那么底层就会自动使用 log4j2作为日志系统

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```

- log4j2的配置，可以与logback使用的配置相同（在application配置文件中修改），也可以直接通过配置 log4j2.xml 配置文件来修改配置

  > 建议将原先的 log4j2.xml 配置文件修改名字为 log4j2-spring.xml，这样SpringBoot可以完全控制日志的初始化



若是还想要用其他的日志系统，导入其xml配置文件即可

建议在导入其他框架时，若有日志系统，使用exclusion排除掉该日志系统，使用SpringBoot提供的默认的即可









#### 多端内容协商

##### 基本实现

在实际开发中，我们会有这样的需求：在获取响应内容时，有些希望接收到的为JSON数据，有些希望接收到的为XML格式的数据。若我们通过映射路径进行区分，我们就需要声明多个handler来处理，且当添加新的格式要求时，又要再次编写源码。多端内容协商就是为了解决这个问题

一、发送请求：

1. 基于请求头 `Accept` 发送请求：

   - Accept: `application/json`：期望接收到JSON格式的数据
   - Accept: `text/xml`：期望接收到XML格式的数据

2. 基于请求参数发送请求：

   - 在application配置文件中，在开启基于请求参数的内容协商功能：

     `spring.mvc.contentnegotiation.favor-parameter=true`（默认为false，即：默认使用请求头的方式来进行内容协商）

   - 在发送请求时，携带请求参数 `format`，参数值为 `json` 或 `xml`

     - 可自定义请求参数：`spring.mvc.contentnegotiation.parameter-name=请求参数名`
   
   > 在获取携带的请求体参数时，需要使用 @RequestBody 注解标识实体类



二、响应数据：

1. 响应JSON格式数据：在handler方法上，添加 `@ResponseBody` 注解

   > 在导入Web场景的启动器时，内部导入的 spring-boot-starter-json 已经整合了JSON转换相关的所有依赖（jackson核心包）。因此标注了这个注解后，就会自动在返回数据时，将数据转换为JSON数据

2. 响应XML格式数据

   > SpringBoot并不支持直接写出XML格式数据。但是，jackson支持将JSON数据转换为XML数据，只需要做些相关配置即可

   - 导入依赖

     ```xml
     <dependency>
         <groupId>com.fasterxml.jackson.dataformat</groupId>
         <artifactId>jackson-dataformat-xml</artifactId>
         <version>2.15.0</version>
     </dependency>
     ```

   - 在handler方法上标记 `@ResponseBody` 注解

   - 在返回类型的类上（数据载体类）标记 `@JacksonXmlRootElement` 注解（可以理解为：根据该类来生成对应的格式（XML格式）的数据）







##### 实现原理

如果handler方法被标记了 @ResponseBody 注解：

1. 项目启动时，会进入到 DispatcherServlet 中的 doDispatch() 中
2. 获取一个 HandlerAdapter 适配器（用来执行目标handler）
3. 执行 handler()，进入到 RequestMappingHandlerAdapter，调用 handleInternal 方法中的 invokeHandlerMethod()
4. 在真正执行handler方法前，会先准备好：
   - HandlerMethodArgumentResolver：参数解析器，确定目标handler所需的每个参数
   - HandlerMethodReturnValueHandler：返回值处理器，确定目标方法的返回值该怎么处理
5. 调用 invokeAndHandle() 中的 invokeForRequest()，去执行目标handler
6. 执行完目标handler后，会返回一个数据载体类对象
7. 执行 handleReturnValue()，在该方法中，执行 selectHandler()，通过循环，找到合适的返回值处理器
8. 最终通过 handleReturnValue()，确定能处理 @ResponseBody注解 的 RequestResponseBodyMethodProcessor
9. 再调用 writeWithMessageConverters()，将最后的结果写出去

得出结论：@ResponseBody 的返回值，是由 HttpMessageConverter 处理的

- writeWithMessageConverters 写出结果时，是通过遍历所有的 MessageConverter，看看哪种支持写出这种数据类型

- 因此，要想实现自定义的内容协商，则需要修改底层的MessageConverter

  通过 WebMvcConfigurer 提供的 `configureMessageConverters` 来修改底层的MessageConverter









##### 自定义内容协商

WebMvcAutoConfiguration中，EnableWebMvcConfiguration内部类间接继承了WebMvcConfigurationSupport，在这个类中，通过addDefaultHttpMessageConverters方法，添加了默认的MessageConverter：

1. ByteArrayHttpMessageConverter：支持字节数据读写
2. StringHttpMessageConverter：支持字符串读写
3. ResourceHttpMessageConverter：支持资源读写
4. ResourceRegionHttpMessageConverter：支持分区资源读写
5. AllEncompassingFormHttpMessageConverter：支持表单 xml / json 读写
6. MappingJackson2HttpMessageConverter：支持请求响应JSON数据读写

SpringBoot提供的MessageConverter有限，仅有用于json或普通数据类型，要额外添加新的内容协商，就需要添加新的HttpMessageConverter



自定义yaml格式的内容协商：

1. 导入依赖

   ```xml
   <dependency>
       <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-yaml</artifactId>
       <version>2.15.0</version>
   </dependency>
   ```
   
2. 在application配置文件中，新增一种内容协商方式：

   ```yaml
   spring:
     mvc:
     	contentnegotiation:
         media-types:
           yaml: text/yaml
   ```

3. 创建一个配置类，继承于 WebMvcConfigurer，重写 `configureMessageConverters` 方法，在方法中添加 自定义的、能够将数据转换为yaml格式的MessageConverter

   ```java
   @Configuration
   class 类名 implements WebMvcConfigurer{
       @Override
       public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
           converters.add(new 类名());
       }
   }
   ```



自定义的、转yaml格式的MessageConverter：

```java
class 类名 extends AbstractHttpMessageConverter<Object>{
	// 这里的泛型是指：支持 将哪种数据类型的数据 转换为yaml
    
/* 要想该MessageConverter支持将数据转换为yaml，就需要和默认提供的MessageConverter一样：
	1.告诉SpringBoot，该MessageConverter是将哪种请求头类型的数据进行转换的
	2.创建要转换的格式所对应的Factory，该Factory就是进行格式转换的（依赖）
	3.创建一个ObjectMapper，添加该Factory（添加格式转换功能）
*/
    private ObjectMapper = null;	// 将该变量提升为全局变量，因为在写出数据时还需用到该变量
    public 类名() {				// 在构造方法中完成对数据的格式转换
        super(new MediaType("text", "yaml", Charset.forName("UTF-8")));		// 第一步
        YAMLFactory yamlFactory = new YAMLFactory();						// 第二步
        this.objectMapper = new ObjectMapper(yamlFactory);					// 第三步
    }
    
    // 可以用来额外判断 该数据类型是否是被支持的数据类型（当返回false时，该数据不会被处理）
    @Override
    protected boolean supports(Class<?> clazz) {
        return true;
    }

    // 处理：从请求体中获取数据并转换为对象
    @Override
    protected Object readInternal(Class<?> clazz, HttpInputMessage inputMessage) throws IOException, HttpMessageNotReadableException {
        return null;
    }

/* 将返回值写出去：调用ObjectMapper类的writeValue(OutputStream,Object)：
	OutputStream：输出流，通过HttpOutputMessage类的getBody()，可以获取到一个输出流对象
	Object：要转换的返回值数据，即Object o
*/
    @Override
    protected void writeInternal(Object o, HttpOutputMessage outputMessage) throws IOException, HttpMessageNotWritableException {
		try (OutputStream os = outputMessage.getBody()) {
            this.objectMapper.writeValue(os, o);
        }
        // 使用了java新特性：try-with，会自动将输出流关闭
    }
}
```









#### Thymeleaf

##### 基本使用

SpringBoot中使用Thymeleaf模版：

1. 引入依赖

   ```xml
   <dependencies>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-thymeleaf</artifactId>
       <version>3.0.5</version>
   </dependencies>
   ```

2. 根据SpringBoot特性：引入场景启动器，即会引入对应的XxxAutoConfiguration

   查看ThymeleafAutoConfiguration，发现EnableConfigurationProperties绑定application配置文件的配置项为 `spring.thymeleaf`

   - 提供的默认配置项：

     - `spring.thymeleaf.prefix`：设置前缀（模版页面默认存放的位置，默认存放于 `classpath:/templates` 文件夹下）
   - `spring.thymeleaf.suffix`：设置后缀（默认后缀名为 `.html`）
     - `spring.thymeleaf.cache`：设置是否开启缓存功能（默认为 `true` 开启状态）
     - `spring.thymeleaf.check-template`：设置开启语法检查（默认为 `true` 开启状态）

3. 只使用 `@Controller` 来标示handler，不能使用 @ResponseBody 注解（使用该注解后，将返回JSON数据，而不会进行页面跳转），handler方法返回值类型为 `String`，返回要跳转的页面的逻辑视图名







##### 核心语法

###### 代码提示

在 \<html> 标签中，添加 `xmlns:th="http://www.thymeleaf.org"`，可以使代码有语法提示





###### 数据获取

`${key名}`：获取存储在共享域中的数据

> ```java
> @GetMapping("/请求路径")
> public String 方法名(Model model){
>     model.addAttribute("key名","value值");		// 在request共享域中存储key-value
>     return "逻辑视图名";
> }
> 
> <h1 th:text="${key名}"></h1>
> // 获取request共享域中的数据
> ```



`@{/路径/文件名}`：使用该方式标示的资源路径，在项目有上下文路径时会添加上

> ```html
> <img src="/路径/文件名"/>
> <!-- 使用原来的方式，在项目有上下文路径时，需要明确指定出上下文路径，否则会找不到资源 -->
> 
> <!-- 在application配置文件中设置项目根路径 -->
> server:
>   servlet:
>     context-path: /根路径
> ```





###### 常用标签

`th:text`：替换标签体内容（纯字符串）

`th:utext`：替换标签体内容（若文本中有标签，会用html的规则运行）

> ```java
> @GetMapping("/请求路径")
> public String 方法名(Model model){
>     String value值 = "<h2>Hello</h2>";
>     model.addAttribute("key名","value值");
>     return "逻辑视图名";
> }
> 
> <h1 th:text="${key名}"></h1>			// value值原样输出
> <h1 th:utext="${key名}"></h1>		// 仅输出Hello
> ```



`th:属性名`：让HTML标签中的属性，能够获取${}的值（动态获取）

> ```html
> <img th:src="@{ ${key名} }" th:style="${key名}"/>
> <!-- 动态决定src的资源路径、img图片样式 -->
> ```



`th:attr`：多个属性的动态指定

> ```html
> <img th:attr="src = @{ ${key名} },style = ${key名}"/>
> <!-- 动态的、连续的设置src资源路径、img图片样式 -->
> ```



`th:if`：当值为false时，标签不显示，true则正常表示

> ```html
> <h1 th:if="false">hello</h1>
> <!-- if为false，则标签不会生效 -->
> ```





###### 常见对象

`#strings`：

1. `toUpperCase(值)`：将值转换为大写表示

   > ```html
   > <h1 th:text="${#strings.toUpperCase(值)}"></h1>
   > <!-- 直接将值转换为大写表示（也可以传入key名，将转换对应的value值） -->
   > ```
   
2. `isEmpty(值)`：判断值是否为空





###### 其他用法

字符串拼接：

1. 在${}中直接使用 `+` 进行拼接

   > ```html
   > <h1 th:text="${'字符串1' + key名 + '字符串2'}"></h1>
   > ```

2. 使用 `||` 包裹${}，被包裹的${}会被转换为key名所对应的value值，其他则为普通字符串显示

   > ```html
   > <h1 th:text="| 字符串1 ${key名} 字符串2 |"></h1>
   > ```



行内写法：`[[]]` 或 `[()]`

> ```html
> <h1>文本：[[ ${key名} ]]</h1>
> <!-- 可以在标签体中动态展示文本，而不用使用th:text去替换标签体内容 -->
> ```



属性优先级：片段 --> 遍历 --> 判断 --> 文本



热启动功能：导入对应的依赖后开启（在代码页面中按 `ctrl`+`f9` 编译即可，无需重启SpringBoot应用，建议只用在Thymeleaf页面中，Java代码中不使用）

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```









##### 常见应用

###### 条件判断

1. `th:if`

   > ```html
   > <h1 th:if="${not #string.isEmpty(值)}"></h1>
   > <!-- 当值不为空时，显示该标签 -->
   > ```

2. `th:switch`、`th:case`

   > ```html
   > <tr th:switch="${值}">
   >        <td th:case="值1"></td>
   >        <td th:case="值2"></td>
   > </tr>
   > <!-- 当值与值1相等时，显示值1标签；当值与值2相等时，显示值2标签；都不匹配时，不显示 -->
   > ```





###### 循环遍历


格式：`th:each="变量名,迭代状态变量:${集合key名}"`

- 每次取出共享域中key所对应的value集合中的一个值，然后存入到变量中

  - 通过 `变量名.属性名`，可以获取对象类型中的属性值（需要提供get/set方法）

- 迭代状态变量：将当前循环的状态信息存入到该变量中（可不获取该变量）

  该变量有以下属性：

  - `index`：当前遍历的索引（从0开始）
  - `count`：当前遍历的索引（从1开始）
  - `size`：要遍历的集合的个数
  - `current`：当前正在遍历的元素对象
  - `even` / `odd`：当前是否是 偶数行 / 奇数行
  - `first` / `last`：当前是否是 第一个元素 / 最后一个元素
  

> ```java
> // 实体类对象
> @NoArgsConstructor
> @AllArgsConstructor
> @Data
> public class Person {
>     private String name;
>     private Integer age;
> }
> 
> // handler方法
> @GetMapping("/for")
> public String 方法名(Model model) {
>     List<Person> list = new ArrayList<>();
>     list.add(new Person("Tom",19));
>     list.add(new Person("Jack",20));
>     list.add(new Person("Rose",18));
>     model.addAttribute("persons",list);
>     return "逻辑视图名";
> }
> 
> // html页面渲染
> <tr th:each="person:${persons}">
>     <td th:text="${person.name}"></td>
>     <td th:text="${person.age}"></td>
> </tr>
> // 取出共享域中存储的List集合，循环将每个元素存入变量中
> ```





###### 变量选择

格式：`th:object`、`*{}`

- 在某个标签中使用th:object来表示某个对象，那么该标签及其子标签下，可使用*{}来直接调用该对象的属性或方法


> ```html
> <tr th:object="${#strings}">
> 	<td th:text="*{isEmpty(true)}"></td>
> </tr>
> <!-- 结果会显示true -->
> ```





###### 模版布局

1. 定义模版：`th:fragment`
2. 调用模板：`th:insert="~{逻辑视图名::模板名}"` / `th:replace="~{逻辑视图名::模版名}"`
   - insert是将模板插入到当前标签内作为子标签；replace是用模板替换当前标签
   - 逻辑视图名指的是当前模板所在的页面的逻辑视图名

> ```html
> <!-- 在a.html页面下的模板 -->
> <div th:fragment="模板名">
>     <a href="xxx"></a>
> </div>
> 
> <!-- 在其他页面中使用模板 -->
> <div th:insert="~{a::模板名}"></div>
> ```









#### 国际化

> 有关的AutoConfiguration：MessageSourceAutoConfiguration

在SpringBoot中使用国际化：`#{key名}`

1. 默认会根据当前浏览器的语言环境，选择类路径下 `messages_语言_地区.properties` 文件（如：`messages_zh_CN.properties` 中文环境、`messages_en_US.properties` 英文环境）
2. 若没有，则会使用默认文件 `messages.properties`



相关的一些配置项：

- `spring.messages.basename=前缀名`（默认为 `messages`）



在Java代码中，IOC容器中存放了一个 `MessageSource` 组件，通过该组件也可以获取到国际化信息：

```java
@Autowired
MessageSource messageSource;

@GetMapping("/请求路径")
public String 方法名(HttpServletRequest request){
    Locale locale = request.getLocale();		// 获取发送请求的位置信息
	String value值 = messageSource.getMessage("key名",null,locale);// 获取指定key的国际化信息 
    return "逻辑视图名";
}

```









#### 错误处理

SpringBoot的错误处理配置在 ErrorMvcAutoConfiguration 自动配置类中，有两个核心机制：

1. 会自适应处理错误，自动根据配置响应页面或JSON格式数据
2. SpringMVC的异常处理机制会保留。当SpringMVC的异常处理机制没有解决问题时，才会调用SpringBoot进行处理



SpringBoot方面的处理流程：

1. 当SpringMVC处理不了错误时，会将请求转发到 /error 路径下，该请求会被 BasicErrorController 进行处理

   > 可在application配置文件中，通过修改配置项 `server.error.path=/路径`，更改错误转发的路径

2. 在处理之前，会先进行内容协商，确定最终返回的类型，交由对应的方法处理：

   - errorHtml：返回HTML页面
   - error：返回JSON数据



处理规则：

1. HTML页面处理规则：
   - 首先精确匹配，会先获取错误状态码（如：404、500），去 `classpath:/templates/error/` 文件夹下，查找对应的页面（如：404.html、500.html）；若没有，则去 `静态资源目录` 下查找
   - 其次是模糊匹配，会去 `classpath:/templates/error/` 文件夹下查找模糊的错误状态码（如：4xx、5xx）；若没有，则去 `静态资源目录` 下查找
   - 若以上两种没有匹配到页面，则会在 `classpath:/templates/` 目录下查找是否存在 `error.html` 页面
   - 若还是没有，则最终会使用SpringBoot默认提供的 `error`（源码：defaultErrorView 方法）
2. JSON数据处理规则：直接获取错误信息后返回



使用建议：

1. 前后端分离：直接用SpringMVC的异常处理机制（@ControllerAdvice + @ExceptionHandler）

2. 服务端页面渲染：

   - 在 `classpath:/templates/error` 目录下，存放常见的相关错误码页面（精准匹配）；再存放通用的模糊错误码页面（模糊匹配）

   - 核心业务出现错误，应跳转到定制的错误页

     通用业务出现错误，则跳转到 `classpath:/templates/error.html` 页面



此外，SpringBoot会将错误信息存入到一个 Model 中，通过 `${key名}` 我们可以将错误信息在页面中渲染出来









#### 嵌入式容器

> 即Servlet容器，管理、运行Servlet组件的环境，一般指 服务器

相关的自动配置类 ServletWebServerFactoryAutoConfiguration

根据 @EnableConfigurationProperties 注解，查看与 application配置文件的配置项 关联的前缀为：`server`

1. 在使 ServletWebServerFactoryAutoConfiguration 自动配置类生效之前，会先 @Import 引入三大服务器：Tomcat、Jetty、Undertow

   但是，引入的这三个服务器类，不一定都会生效：存在着相关的条件注解（只有引入了依赖才能生效）

   SpringBoot默认在引入 spring-boot-starter-web 包时，就会引入 spring-boot-starter-tomcat。因此，SpringBoot默认将 Tomcat 作为Servlet容器

2. 在引入的三个服务器类时，成功引入时，会在内部添加一个 xxxServletWebServerFactory 的组件。该组件内部有一个方法：getWebServer()，可以获取到Web服务器

3. 当 ServletWebServerApplicationContext IOC容器启动时，会自动调用 onRefresh() 方法，并通过 createWebServer() 创建Web服务器工厂，接着创建Web服务器

总结：在Spring容器（ServletWebServerApplicationContext）启动时，会自动调用 onRefresh()，调用 createWebServer()，通过 getWebServerFactory() 获取到引入服务器类时创建的Web服务器工厂组件，再通过该工厂组件中的 getWebServer()，获取到Servlet容器

更换默认服务器：排除Web启动器自动导入的Tomcat启动器，引入其他服务器启动器









#### 新特性

##### ProblemDetails

在 WebMvcConfiguration 中，有一个内部类 ProblemDetailsErrorHandlingConfiguration，其内部添加了一个 ProblemDetailsExceptionHandler 类型的组件，是一个被 @ControllerAdvice 修饰的 集中处理异常的类

该异常处理类只能处理部分HTTP异常。若出现这些异常，那么SpringBoot支持以 `RFC 7807` 规范来返回错误信息

- RFC 7807 特性：Content-Type为 `application/problem+json`，且支持拓展额外的错误信息进行返回
- 默认情况下，SpringBoot并未使用该功能，通过 `spring.mvc.problemdetails.enabled=true` 开启该功能









#### 其他配置

`spring.banner.location=classpath:banner.txt`：默认情况下，Boot应用在启动时，会在控制台上打印该文件中的内容

`spring.main.banner-mode=off`：关闭打印banner（默认值为 `console`，在控制台上打印）

`spring.application.name="名称"`：配置Boot应用的名称（在运行时显示在日志或其他组件中）

```yaml
spring:
  jackson:
    date-format: yyyy-MM-dd HH:mm:ss
    time-zone: GMT+8
# 配置显示的日期格式
# 在数据库中，我们使用TIMESTAMP类型的时间数据格式来存储数据时，底层是保存的时间戳。在读取数据时，会将数据转换为0时区的数据及其格式，我们需要自定义格式，并且在时区上+8
```











### 数据访问

#### 基本步骤

1. 导入依赖

   ```xml
   <!-- mybatis提供的启动器 -->
   <dependency>
       <groupId>org.mybatis.spring.boot</groupId>
       <artifactId>mybatis-spring-boot-starter</artifactId>
       <version>3.0.1</version>
   </dependency>
   
   <!-- 数据库核心驱动 -->
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>8.0.28</version>
   </dependency>
   ```

2. 在application配置文件中，配置数据源信息

   ```yaml
   spring:
     datasource:
       type: com.zaxxer.hikari.HikariDataSource
       # druid数据源暂时不兼容SpringBoot3，因此使用底层默认提供的数据源
       driver-class-name: com.mysql.cj.jdbc.Driver
       url: jdbc:mysql://IP地址:3306/数据库名
       username: 用户名
       password: 密码
   ```

3. 正常写Mapper接口、映射文件、业务逻辑

4. 在SpringBoot的启动类（@SpringBootApplication注解标识的类）上，指明：扫描哪个包下的所有接口

   ```java
   @MapperScan(basePackages = "Mapper接口所在的包路径")
   @SpringBootApplication
   class 类名{
       public static void main(String[] args) {
           SpringApplication.run(类名.class, args);
       }
   }
   ```

5. 在application配置文件中，指明：Mapper映射文件都存放在哪个目录下

   ```yaml
   mybatis:
     mapper-locations: classpath:/路径/*.xml
   ```

   SpringBoot会根据 \<mapper> 标签中的 `namespace` 属性，自动映射到Mapper接口









#### 源码分析

在导入 mybatis-spring-boot-starter 启动器后，内部会间接引入 spring-boot-starter，进而引入 spring-boot-autoconfigure 包下所有的自动配置类，而 META-INF / org.springframework.boot.autoconfigure.AutoConfiguration.imports 文件将决定会被自动激活的类

- 在mybatis-spring-boot-starter启动器中，会引入 `spring-boot-starter-jdbc`，因此该文件中有关JDBC的5个自动配置类将会被自动激活（满足条件注解要求时）：
  - DataSourceAutoConfiguration：数据源的自动配置（可查看到绑定application配置文件配置项的前缀为：`spring.datasource`）
  - JdbcTemplateAutoConfiguration：给容器中存放了 `JdbcTemplate` 组件（操作数据库对象）
  - JndiDataSourceAutoConfiguration、XADataSourceAutoConfiguration：分布式事务数据源
  - DataSourceTransactionManagerAutoConfiguration：支持事务



在mybatis-spring-boot-starter启动器中，还会引入 `mybatis-spring-boot-autoconfigure`，查看该包下 META-INF / org.springframework.boot.autoconfigure.AutoConfiguration.imports，可以看到MyBatis激活的核心自动配置类：`MybatisAutoConfiguration`

- 该配置类会在 DataSource（数据源）配置完成之后才配置
- 该配置类会往IOC容器中存放两个组件：`sqlSessionFactory`（创建和数据库的会话）、`sqlSessionTemplate`（操作数据库）
- 该配置类与application配置文件配置项绑定的前缀为：`mybatis`



Mapper接口的代理对象是怎么创建并存入到IOC容器中的？

- 启动类上标记的 `@MapperScan` 注解，是创建代理对象存入到IOC容器中的关键
- 该注解通过 @Import，引入 MapperScannerRegistrar类，该类会：解析包路径下中的每一个类，并为每一个Mapper接口创建代理对象，然后存入到IOC容器中









#### 其他配置

`mybatis.configuration.map-underscore-to-camel-case=true`：开启 驼峰-下划线 映射功能

`debug=true`：开启调试模式（会详细显示启动的自动配置类）











### @SpringBootApplication

@SpringBootApplication标识的类将作为主程序类。该注解是由3个复合注解构成：

1. `@SpringBootConfiguration`：@Configuration 的派生注解，标识容器中的配置类（组件）
2. `@EnableAutoConfiguration`：开启自动配置的核心注解，由2个复合注解构成：
   - `@AutoConfigurationPackage`：扫描主程序类所在的包及其子包
   - `@Import({AutoConfigurationImportSelector.class})`：扫描SPI文件 META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports 下的配置类；加载starter启动器导入的组件
3. `@ComponentScan`：排除加载哪些组件（排除前面已扫描加载了的配置类）













### SpringApplication

SpringApplication是Boot应用启动的核心API

查看run方法的底层源码，发现其写法为：(new SpringApplication(primarySources)).run(args)

因此，我们可以将该方法的调用改为：

```java
@SpringBootApplication
class 类名{
    public static void main(String[] args){
        SpringApplication application = new SpringApplication(类名.class);
        application.run(args);
    }
}
```

该方式我们可以获取到SpringApplication对象，并在调用run()之前，我们可以进行一些配置，比如：`application.setBannerMode(Banner.Mode.OFF)`（关闭在控制台上打印banner的功能）

- 但是，application配置文件的配置，会优先生效



也可以通过FluentAPI来实现：

```java
new SpringApplicationBuilder().main(类名.class).sources(类名.class)
    .bannerMode(Banner.Mode.OFF)		// 关闭在控制台上打印banner的功能
    .run(args);
```











### 事件与监听器

#### 自定义监听器

1. 编写一个类，实现 `SpringApplicationRunListener` 接口
2. 在类路径下创建 `META-INF/spring.factories` 文件，添加 `org.springframework.boot.SpringApplicationRunListener=自定义监听器全类名`

> SpringBoot在 spring-boot.jar 包下，存放了默认的监听器：EventPublishingRunListener









#### 生命周期流程

Boot应用启动，SpringApplication调用run方法，利用底层创建的 BootstrapContext 来引导整个项目启动：

1. 引导：
   - `starting`：当 BootstrapContext 创建完成后，就会调用该方法
   - `environmentPrepared`：环境准备（将启动参数args等绑定到环境变量中，但IOC容器并未创建）

2. 启动：
   - `contextPrepared`：IOC容器创建好（但是主程序配置类还未加载），并关闭引导上下文 BootstrapContext
   - `contextLoaded`：IOC容器加载主程序（配置类），但IOC容器还未刷新（未创建相应的Bean组件）
   - `started`：IOC容器刷新，创建好响应的Bean组件，但未调用 runner
   - `ready`：完成所有的 runner 调用

3. 运行：当 starting 之后，所有的步骤未出现错误，则容器正常running；否则 failed 启动失败



SpringBoot应用启动时，经历启动阶段的 started 时，会进行IOC容器的刷新，在刷新过程中，会先创建BeanFactory，再创建对应的Bean。而 扫描SPI文件中的自动配置类、主程序类所在的包及其子包 这个过程，是在创建BeanFactory阶段的 invokeBeanFactoryPostProcessors 中完成的，但仅仅是确定了要加载的组件；真正加载组件的过程，是在创建Bean阶段的 finishBeanFactoryInitialization 中完成的









#### 监听器接口

`BootstrapRegistryInitializers`：只监听特定阶段 -- 引导阶段，在创建引导上下文 BootstrapContext 时开始触发（starting）

- 可在 `classpath:/META-INF/spring.factories` 文件中添加接口的实现类

  或通过 `SpringApplication` 对象的 `addBootstrapRegistryInitializer()` 进行添加

- 使用场景举例：进行密钥校验授权



`ApplicationContextInitializer`：只监听特定阶段 -- IOC容器初始化（environmentPrepared 与 contextPrepared 之间）

- 可在 `classpath:/META-INF/spring.factories` 文件中添加接口的实现类

  或通过 `SpringApplication` 对象的 `addInitializers()` 进行添加



`ApplicationListener`：监听全阶段 -- 基于事件机制来监听

- 可在 `classpath:/META-INF/spring.factories` 文件中添加接口的实现类

  或通过 `SpringApplication` 对象的 `addListeners()` 进行添加

  也可直接将实现类存入IOC容器中

- 9大事件：

  `ApplicationStartingEvent`：应用启动，但未做任何事

  `ApplicationEnvironmentPrepareEvent`：环境准备好，但IOC容器还未创建

  `ApplicationContextInitializedEvent`：IOC容器创建，但主程序配置类未加载

  `ApplicationPreparedEvent`：组件信息被加载，但未刷新

  `ApplicationStartedEvent`：组件创建完成，runner未被调用

  `AvailabilityChangeEvent`：LivenessState.CORRECT 应用存活（存活探针）

  `ApplicationReadyEvent`：任何runner被调用

  `AvailabilityChangeEvent`：ReadinessState.ACCEPTING_TRAFFIC 应用就绪，可以接收请求（就绪探针）

  `ApplicationFailedEvent`：启动出错



`SpringApplicationRunListener`：监听全阶段，各个阶段都能自定义操作

- 可在 `classpath:/META-INF/spring.factories` 文件中添加接口的实现类



`ApplicationRunner`、`CommandLineRunner`：只监听特定阶段 -- 应用就绪（ready），若卡死应用则不会就绪

- 直接将实现类存入到IOC容器中









#### 监听自定义事件

ApplicationListener可以监听生命周期间的9大事件，也可以监听应用运行中产生的所有事件

应用场景：当我们实现了登录之后，我们希望可以自动推送一些信息，如自动签到。若是使用原先的方法，那么我们就会采用 在登录后调用service层的签到业务，实现自动签到功能；但是，若是我们在之后需要拓展业务，就需要每次都修改原先的controller，非常麻烦。我们希望可以通过 事件功能，在登录成功后自动生成一个事件，并广播出去，其他service（监听器）在收到该事件后，就会调用相应的方法



一、创建事件对象

```java
class 类名 extends ApplicationEvent {

    public 类名(Object source) {
        super(source);
    }
}
// 让一个类继承于ApplicationEvent，并调用其父类构造器，那么这个类就是一个事件类，即ApplicationEvent的子类（在发送事件时，需要用到这么一个类）
// source，往往作为事件的相关信息而存在（如存储用户信息），我们可以在监听事件时获取该值
```



二、创建事件发送对象，并发送事件

```java
@Component			// 存入IOC容器中，方便获取该事件发送对象
class 类名 implements ApplicationEventPublisherAware {
    
    private ApplicationEventPublisher applicationEventPublisher;

    public void 方法名(ApplicationEvent 形参名){
        this.applicationEventPublisher.publishEvent(形参名);
    }

    @Override
    public void setApplicationEventPublisher(ApplicationEventPublisher applicationEventPublisher) {
        this.applicationEventPublisher = applicationEventPublisher;
    }
}
// 让一个类实现ApplicationEventPublisherAware接口，该接口需要实现一个方法：setApplicationEventPublisher
// 该方法会被自动调用，可获取到发送事件的真正对象：ApplicationEventPublisher
// 调用该对象的 publishEvent(ApplicationEvent)，即可真正实现广播一个事件
```



三、自动监听事件

```java
@Service			
// 必须存入IOC容器中才能监听事件（一般在监听到事件后，我们会调用service层的业务方法处理业务，如自动签到）
class 类名 implements ApplicationListener<事件对象类型> {
    @Override
    public void onApplicationEvent(事件对象类型 event) {}
}
// 实现ApplicationListener接口，该接口泛型是指：只监听哪种事件类型
// 当该类型的事件对象触发了事件后，就会调用onApplicationEvent方法，通常会在该方法中调用要处理的业务方法
// event.getSource()，我们可以获取到 存放的事件的相关信息source（如获取用户信息）
```













### 环境切换

SpringBoot支持使用 `@Profile("环境名")` 来标识组件，让这些组件在指定的环境下才会生效

- 组件可以来自 @Configuration 标识的配置类（加载后会存入IOC容器）；可以是通过 @Bean 注解添加的组件；可以是 @Controller 等添加的组件

- 标识的环境名可以任意，但有一个 `default` 的环境是默认会生效的，即：当该Bean被 `@Profile("default")` 标识时，该Bean也会被存入IOC容器

- 可以通过application配置文件配置项修改改默认生效的环境名：`spring.profiles.default=环境名`

- 配置激活指定环境：`spring.profiles.active=环境名1,环境名2`

- 可在Boot应用打成Jar包后，通过命令行的方式 `--spring.profiles.active=环境名` 的方式激活环境

- 还可以包含指定环境，让这些环境无论是否被激活都会生效：`spring.profiles.include=环境名1,环境名2`

- 还可以为环境分组，通过激活该组，从而激活组内所有的环境：

  ````properties
  spring.profiles.group.组名=环境名1,环境名2		// 分组
  spring.profiles.active=组名					// 激活该组
  ````

  

SpringBoot也支持创建多个 `application-环境名.properties` 类型的配置文件，自定义选择激活哪些配置文件：

- 原先的 `application.properties` 作为主配置文件，是在任何情况下都会生效的
- 通过 `spring.profiles.active=环境名` 或命令行 `--spring.profiles.active=环境名` 来选择生效的配置文件（可生效多个配置文件）
- 若激活的配置文件中的配置项，与主配置文件冲突，那么会以激活的配置文件为主；若多个激活的配置文件配置项冲突，则会报错
- 非主配置文件，不能激活其他配置文件



SpringBoot中，多个application配置文件的配置项出现冲突时，遵循以下规则：

- properties格式 和 yaml格式 同时存在时，优先生效 properties格式的
- 通过 `spring.config.import=classpath:/文件名.properties`，可以引入其他文件中的内容作为配置项，但优先级要低于自身文件中设置的配置项
- Boot应用被打成jar包后，可在存放该jar包的位置处放一个新的application配置文件，且以新的配置文件中的配置项作为标准
- 在classpath类路径下，可以有一个文件夹 `config`，该文件夹下的application配置文件的配置项，优先级要高于类路径下的配置文件，且其子文件夹下，优先级更高（仅jar包下的config文件夹下的配置文件能被识别）
- 所有的配置项也可以通过命令行方式传入：`--配置项=值`，优先级最高（相较于前面的几种方式）

总结（配置项加载出现冲突时的优先级）：

- 命令行 > 外包config子目录 > 外包config目录 > 外包根目录 > 内部config目录 > 内部根目录 > 外部引用文件
- 外部环境 > 自身配置
- properties > yaml



`${配置项}`：属性占位符，可以引用配置文件中该配置项的值

- `${配置项:默认值}` 可以让该配置项不存在时，使用默认值，而不至于报错











### 测试整合

1. 导入依赖

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-test</artifactId>
   </dependency>
   ```

2. 使用 `@SpringBootTest` 标识测试类之后，该测试类就具备了 测试SpringBoot容器中所有组件 的功能（即：可直接通过 @Autowired 获取IOC容器中的组件）

   - 要求：@SpringBootTest 标识的测试类，需要与主程序类所在的包路径相同，或是其子包

3. 正常使用 `@Test` 方法，在方法中进行测试



其他可修饰在方法上的注解：

1. `@DisplayName("名")`：更改测试方法运行后，显示的方法名
2. `@BeforeAll`：在所有测试方法执行前，会先执行一次该方法（该方法必须被 `static` 修饰）
3. `@BeforeEach`：在每个测试方法执行前，会先执行该方法



其他测试：

1. `Assertions.assertEquals(期望值,测试值)`：断言，用于判断期望值与测试值是否相等，不相等时报错













### yaml文件

格式：`key: value`

- 当有层级关系时，切换到下一行，并使用两个空格（左侧对齐的话代表在同一级）

使用举例：

```java
class Person{
    String name;	// 字符串
    Integer age;	// 整型
    Boolean like;	// 布尔类型
    Date birthDay;	// 日期类型
    Child child;	// 对象
    List<Dog> dogs;	// List集合（数组）
    Map<String,Cat> cats; // Map集合
}

class Child{
    String name;
    List<String> text;
}

class Cat{
    String name;
    Integer age;
}

class Dog{
    String name;
    Integer age;
}
```

1. properties文件表示：

   ```properties
   person.name=Tom
   person.age=19
   person.like=false
   
   person.birthDay=2021/10/21 19:20:21
   # 日期时间，/分隔年月日，:分隔时分秒，之间用空格隔开
   
   person.child.name=Jack
   # 普通对象，用.进入下一层
   person.child.text[0]=abc
   person.child.text[1]=def
   # 对象中有List集合（数组），[下标] 表示集合元素
   
   person.dogs[0].name=KK
   person.dogs[0].age=2
   person.dogs[1].name=LL
   person.dogs[1].age=1
   
   person.cats.c1.name=JJ
   person.cats.c1.age=2
   person.cats.c2.name=OO
   person.cats.c2.age=1
   # Map集合，与List不同的是：key值非固定。使用.（一层级别）来表示key即可
   ```

2. yaml（yml）文件表示：

   ```yaml
   person:
     name: Tom
     age: 20
     like: false
     birthDay: 2019/10/01 10:10:10
     child:	# 普通对象，下一级表示
       name: July
       text: [ abc,def ]	# List集合（数组），可使用：[值1,值2] 来表示数组的元素
     dogs:		# List集合（数组），还可以：下一级，并使用 - 来代表对应的一个元素
       - name: YY
         age: 1
       - name: UU
         age: 2
     cats:		# Map集合，key下一级表示
       c1:		# value，也是下一级表示
         name: II
         age: 2
       c2: { name: PP,age: 3 }		# 普通对象，还可以直接通过{key: value}来表示
   ```

   - 其他细节：

     - birthDay，可以使用birth-day，即：`xxYy` 可以替换为 `xx-yy`，效果是一样的

     - value的值可以用 `''`（单引号）或 `""`（双引号），双引号会对如 \n、\t 等进行转义，而单引号则不会（和不用效果相同）

     - 当值为 `|` ，在下一级书写value时，会将value的格式完整保留（如：换行、缩进等格式都会存在）

       当值为 `>` ，在下一级书写value时，若value的每行都不存在缩进，则不会换行，而是将换行该为空格隔开（有缩进的该行会原样输出）

     - 使用 --- 可以为当前yaml文件进行文档分割（每个文档区内容独立，在收起来后更便于阅读）











### 自定义starter

自定义starter，本质上还是将代码进行抽取，存放在一个model中，并根据需要引入代码所需要的依赖，且不存在主程序类，然后通过 \<dependency> 引入该starter即可

但是，通常情况下直接引入依赖并不能成功调用该starter中的组件。原因：主程序类只会扫描其所在的包及其子包，若starter中的组件不在这些包范围内，则不会被扫描到

解决办法：

1. 最基本解决：编写一个 xxxAutoConfiguration 配置类，在该配置类中，通过 `@Import` 注解，将这些不能被扫描到的组件添加进来，再在主程序类中，通过 @Import 注解，将 xxxAutoConfiguration 引入进来即可
2. 自动加载方式：SpringBoot应用在启动时，会自动加载SPI文件（`classpath:/META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`），我们只需要在该文件中添加上 xxxAutoConfiguration 配置类的全类名，就会自动载入该配置类组件，从而引入该配置类中 @Import 标识的组件













## 场景整合

### 环境搭建

1. 安装Docker

   ```shell
   yum install -y yum-utils
   
   yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
   
   yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   
   systemctl enable docker --now		# 启动Docker服务
   docker ps			# 测试是否安装成功
   ```

2. 配置环境

   - 先创建一个文件夹，在该文件夹中存放配置文件

     ```shell
     mkdir prod
     cd prod
     ```

   - 编写 prometheus 配置文件：prometheus.yml（设置采集数据的对象）

     ```yaml
     # 全局配置
     global:
       scrape_interval: 15s 			# 采集指标的时间间隔 
       evaluation_interval: 15s 		# 规则评估执行间隔
       
     # 采集指标的任务配置  
     scrape_configs:
       # 采集Prometheus数据
       - job_name: 'prometheus'		# 展示的标题名
         static_configs:
           - targets: ['localhost:9090']		# 采集数据的地址
       
       # 采集Redis数据
       - job_name: 'redis'   
         static_configs:
           - targets: ['redis:6379']			# 本机docker，可以直接使用该方式访问
       
       # 采集Kafka数据
       - job_name: 'kafka'   
         static_configs:
           - targets: ['kafka:9092']
     ```

   - 编写 docker-compose.yml 文件，因为我们安装了docker-compose-plugin这个插件，因此支持我们安装镜像时，支持通过yml文件来进行安装（安装：redis、prometheus、kafka / kafka-ui、grafana、mysql）

     ```yaml
     version: "3"
     services:
       redis:
         image: redis
         ports:
           - "6379:6379"
       prometheus:
         image: prom/prometheus
         volumes:
           - /prod:/etc/prometheus		# 挂载配置（默认加载/etc/prometheus下的配置文件）
         ports:
           - "9090:9090"
       kafka:
         image: wurstmeister/kafka
         ports:
           - "9092:9092"
         environment:
           KAFKA_ADVERTISED_HOST_NAME: kafka
           KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
       zookeeper:
         image: wurstmeister/zookeeper
         ports:
           - "2181:2181"
       kafka-ui:
         image: provectuslabs/kafka-ui
         ports:
           - "8080:8080"
         environment:
           KAFKA_CLUSTERS_0_NAME: local
           KAFKA_CLUSTERS_0_BOOTSTRAPSERVERS: kafka:9092
           KAFKA_CLUSTERS_0_ZOOKEEPER: zookeeper:2181
       grafana:
         image: grafana/grafana
         ports:
           - "3000:3000"
       mysql:
         image: mysql
         environment:
           MYSQL_ROOT_PASSWORD: password
         ports:
           - "3306:3306"
     ```

3. 拉取镜像、创建容器并启动：`docker compose -f docker-compose.yml up -d`

   > `docker compose logs -f 服务名`：查看服务的日志信息（如：kafka）









### Redis整合

#### 基本使用

1. 导入starter启动器

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-data-redis</artifactId>
   </dependency>
   ```

2. 导入启动器后，就相当于导入相应的 XxxAutoConfiguration。查看 RedisAutoConfiguration，@EnableConfigurationProperties 注解可看出关联application配置文件配置项的前缀为 `spring.data.redis`

   添加配置项：连接的redis主机地址

   ```properties
   spring.data.redis.host=ip地址
   ```

3. 查看RedisAutoConfiguration配置类，发现为我们提供了两个核心 Bean：`RedisTemplate` / `StringRedisTemplate`，这两个 Bean 都是可以用来对redis进行操作的组件，主要区别在于：StringRedisTemplate 已经处理了序列化问题（以字符串格式存储于redis服务器中），且其key、value在存储时都要以字符串格式存储

   ```java
   @Autowired				// 该组件已被装配，直接DI注入使用即可
   StringRedisTemplate redisTemplate;
   
   redisTemplate.opsForValue().increment("key名");
   String res = redisTemplate.opsForValue().get("key名");
   // 通过 StringRedisTemplate对象.opsForXxx() 获取一个xxx对应的数据类型的操作对象，通过该对象调用相应的方法进行操作即可
   ```

   常用的opsForXxx()有：

   - opsForValue()：value值为string普通字符串

     - `increment("key名")`：对指定的key进行 +1 操作
     - `set("key名","value值")`：设置key-value
     - 【String】`get("key名")`：获取指定的key的value值

   - opsForList()：value值为list列表

     - `leftPush("key名","value值")`：左插一个值
     - `leftPop("key名")`：弹出最左边的一个值

   - opsForSet()：value值为set集合

     - `add("key名","value值1","value值2")`：为指定集合添加元素
     - 【Boolean】`isMember("key名","value值")`：判断指定集合中的元素是否存在

   - opsForZSet()：value值为zset有序集合

     - `add("key名","value值",double)`：为有序集合添加新元素（将按double值的大小进行排序）

     - 【ZSetOperations.TypedTuple\<String>】`popMax("key名")`：弹出有序集合中double值最大的

       > 通过返回的对象，调用：
       >
       > ​	`getValue()`：获取value值
       >
       > ​	`getScore()`：获取double值

   - opsForHash()：value值为hash哈希

     - `put("key名","map-key名","map-value值")`：为哈希map添加新元素
     - `get("key名","map-key名")`：获取哈希中指定map-key名对应的map-value值







#### 自定义RedisTemplate

StringRedisTemplate，在储存key-value，其值只能为String字符串，不能存储对象，我们考虑使用 `RedisTemplate<Object,Object>`，只要对象所属的类实现了序列化接口（`Serializable`），就可以被存储

但是，查看 `RedisTemplate` 源码，发现：由于RedisTemplate处理序列化的方式是采用底层默认的 JdkSerializationRedisSerializer 来处理，因此，在redis数据库中存储的形式看似为乱码，影响在数据库中查看

考虑将序列化的方式改为 JSON ，就需要将默认的处理序列化方式替换掉

查看 `RedisAutoConfiguration`，发现在创建 RedisTemplate 时，是要求IOC容器中没有 redisTemplate 组件时才会创建，因此，可以选择使用 @Bean 注解添加一个自定义的 redisTemplate 组件，在该组件中修改默认使用的序列化方式

 ```java
// 在配置类中添加该组件
@Bean
public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
    RedisTemplate<Object, Object> template = new RedisTemplate<>();
    template.setConnectionFactory(redisConnectionFactory);
    
    template.setDefaultSerializer(new GenericJackson2JsonRedisSerializer());
    // 改用JSON方式处理序列化问题，那么要被存储的对象也不再需要实现序列化接口了，因为都将以JSON格式存储
    
    return template;
}
 ```

> 保存对象到Redis时，是将对象转换为JSON字符串存储到Redis中的；当获取数据时，再转换为Java对象。在反序列化为Java对象时，需要注意该对象要有无参构造器，否则会报错







#### 其他配置

SpringBoot底层默认有两种操作redis服务器的客户端：lettuce（默认）、jedis



更改默认使用的客户端：在引入的starter中，排除lettuce，重新引入jedis，且修改配置项：`spring.data.redis.client-type=jedis`



修改其他参数：

- `spring.data.redis.jedis.pool.enabled=true`：开启连接池功能
- `spring.data.redis.jedis.pool.max-active=10`：自定义连接池连接数量











### 接口文档

> 生成接口对应的文档信息，方便前端对接时使用

#### springdoc-openapi

1. 导入依赖

   ```xml
   <dependency>
       <groupId>org.springdoc</groupId>
       <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
       <version>2.1.0</version>
   </dependency>
   <!-- 适用SpringBoot3.x -->
   ```

2. 在导入依赖后，Boot应用运行时就会生成相应的接口文档，访问地址为 `/swagger-ui/index.html`

3. 功能注解：

   - `@Tag(name="标题信息")`：标识在类上，一般用于描述该controller的作用（name属性必须携带）
   - `@Operation(summary="标题信息")`：标识在方法上，一般用于描述该handler方法的作用
   - `@Parameter(description="描述信息")`：标记在形参上，一般用于描述该形参的作用
   - `@Schema(description="描述信息")`：标记在实体类或其属性上，一般用于描述该实体类的作用及其属性的含义

- 将多个controller进行分页显示：

  ```java
  @Bean
  public GroupedOpenApi 方法名(){
      return GroupedOpenApi.builder().group("页名")
          .pathsToMatch("/请求路径/**").build();
      // 将这个请求路径下所有的handler存放在一个页面中
  }
  // 每存放这么一个Bean组件，就会多一个对应的页名，通过页名切换页面，可以看到对应的请求路径下所有匹配的handler
  ```

  







#### knife4j

1. 导入依赖

   ```xml
   <dependency>
       <groupId>com.github.xiaoymin</groupId>
       <artifactId>knife4j-spring-boot-starter</artifactId>
       <version>3.0.3</version>
   </dependency>
   <!-- 使用SpringBoot2.x -->
   ```

2. 在导入依赖后，Boot应用运行时就会生成相应的接口文档，访问地址为 `/doc.html`

3. 功能注解：

   - `@Api(tags = "标题名称")`：标识在类上，一般用于描述该controller的作用
   - `@ApiOperation("标题信息")`：标识在方法上，一般用于描述该handler方法的作用
   - `@ApiParam("描述信息")`：标记在形参上，一般用于描述该形参的作用











### 远程调用

#### 介绍

本地过程调用：所有的方法都在同一个JVM中运行，可以相互调用

远程过程调用：通过连接对方服务器，之间进行请求响应交互，来实现调用



API（Application Programming Interface）：接口，远程提供功能

SDK（Software Development Kit）：工具包，导入Jar包后调用功能



开发过程中，我们会经常调用别人写好的功能：

1. 若是内部微服务，可通过依赖cloud、注册中心等进行调用
2. 若是对外暴露的API，可通过发送HTTP请求，遵循外部规定的协议进行调用。一般有三种调用方式：
   - RestTemplate（原生的调用）
   - WebClient（响应式编程调用）
   - HttpServiceProxyFactory（代理对象调用）







#### WebClient

> WebClient进行远程天气查询API调用

1. 导入依赖

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-webflux</artifactId>
   </dependency>
   ```

2. 创建WebClient，响应式调用API

   ```java
   WebClient client = WebClient.create();
   // 创建WebClient对象
   
   Map<String,String> param = new HashMap<>();
   param.put("key名","value值");
   // 将请求参数存入Map集合中
   
   Mono<String> mono = client.get()		// 请求类型：get请求
       
       .uri("https://ali-weather.showapi.com/area-to-weather?area={key名}",param)
   // uri(String,Map)：设置请求的地址，String是请求路径，可通过 ? 携带请求参数，{key名} 是占位符，最终取值为传入的Map对应的value值
       
       .accept(MediaType.APPLICATION_JSON)			// 响应的数据类型：JSON
       
       .header("Authorization","APPCODE a2b61c9b156949799408bda9fe17f225")
       // 该远程API要求：请求头中携带Authorization字段，APPCODE后跟的是授权码
       
       .retrieve()				// 发送请求
       
       .bodyToMono(String.class);			// 取出响应的请求体，返回类型为 Mono<String>
   ```









#### HttpServiceProxyFactory

1. 导入依赖

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-webflux</artifactId>
   </dependency>
   ```

2. 创建要被代理的接口

   ```java
   interface 接口名 {
       @GetExchange(url="https://ali-weather.showapi.com/area-to-weather",
                    accept = "application/json")
       // 发送一个get请求，向指定url发送请求，接收的数据类型为JSON
       
       Mono<String> getWeather(@RequestParam("area")String area,，
                               @RequestHeader("Authorization")String pass);
   // 被@GetExchange注解标识后，@RequestParam接收到的参数会作为请求参数；@RequestHeader接收到的参数会作为请求头信息
   }
   ```

3. 创建客户端

   ```java
   WebClient 客户端对象 = WebClient.builder()
   /*    .baseUrl("https://ali-weather.showapi.com")
   // 若@GetExchange注解中，url属性只设置了/area-to-weather，则需要补上基础路径
   	  .defaultHeader("Authorization","APPCODE 2d4b0a9525b842c88f033f09f2e")
   // 默认携带的请求头，每个用户的APPCODE是固定的，因此可以考虑在这里表示，就不用再@RequestHeader传入
   */   
       .codecs(clientCodecConfigurer -> {
               clientCodecConfigurer.defaultCodecs().maxInMemorySize(256 * 1024 * 1024);
           })
   // 响应的数据量可能会超出，因此要设置大点
       
       .build();
   ```

4. 创建代理对象的工厂

   ```java
   HttpServiceProxyFactory 代理工厂对象 = HttpServiceProxyFactory
       .builder(WebClientAdapter.forClient(客户端对象))
       .build();
   ```

5. 用代理对象工厂创建代理对象，利用代理对象调用代理方法，实现远程API调用

   ```java
   WeatherInterface weatherAPI = 代理工厂对象.createClient(WeatherInterface.class);
   // 根据要被代理的接口的类型，创建对应的代理对象
   
   Mono<String> mono = weatherAPI.getWeather(area,"APPCODE 2d4b0a9525b842c88f033f09f2e");
   // 利用代理对象调用被代理的方法，传入需要的参数，实现远程API调用
   ```

> 我们可以将 创建客户端、创建代理对象工厂 这两步骤合并，返回一个 HttpServiceProxyFactory 代理工厂对象的组件，这样我们可以直接通过工厂对象创建对应的代理类











### 消息队列

#### 介绍

功能描述：与 自定义事件+监听器 有异曲同工之妙（）

1. 【异步】

   存在需求：在注册账号后，发送短信提醒，并邮件发送新手大礼包

   - 正常流程：注册账号（1s） --> 发送短信（0.5s） --> 发送邮件（0.5s）==》2s
   - 消息队列：注册账号（1s） --> 响应注册成功信息（0.2s）/ 异步发送短信、邮件 ==》 1.2s

2. 【解耦】

   正常情况下：下订单 --> 调用接口 --> 访问库存，当接口发生改变时，就需要调整两边的接口调用

   消息队列：下订单 --> 发送信息告诉消息队列 --> 库存监听消息并处理

3. 【削峰】、【缓冲】

   正常情况下：发送请求，直接处理。当信息量过大时，可能会导致服务器宕机

   消息队列：将请求发送给消息队列，服务器监听消息队列，按能力处理消息



模式介绍：

1. 点对点模式：生产者发送消息给消息队列，消费者从消息队列中获取并处理消息，之后从消息队列中删除

2. 发布订阅模式：

   - 生产者发送消息，存储在消息队列中某个主题（`Topic`）下

   - 消费者A订阅该主题，从中读取2条消息，记录偏移量为2

   - 消费者B订阅该主题，从中读取3条信息，记录偏移量为3

     > 消息不会从消息队列中删除



可将不同主题下消息进行分区和副本：

1. 分区：将海量消息进行分散存储在不同的消息队列中

   > 比如：新闻主题下的消息，原本是全部存储在消息队列服务器A中的；现将该主题下的消息，通过特定算法，一部分会存储在另一个消息队列服务器B中

2. 副本：每个分区的备份

   > 比如：将A消息队列中的消息，备份到B消息队列中。当A消息队列宕机后，可用B消息队列中的备份来代替



消费者可被划分为一个组内

- 相同组内的消费者，对该主题下消息的处理属于 竞争关系：消息被消费者A处理后，消费者B就不能再处理该消息了
- 不同组内的消费者，对该主题下消息的处理属于 发布/订阅关系：消息被消费组1内的消费者A处理后，消费组2内的消费者A，也是可以处理该消息的







#### 发送消息

> 若提示无法找到主机，请在 `C:\Windows\System32\drivers\etc\hosts` 文件中，添加：`IP地址 kafka`

1. 导入依赖

   ```xml
   <dependency>
       <groupId>org.springframework.kafka</groupId>
       <artifactId>spring-kafka</artifactId>
   </dependency>
   ```

   导入相关依赖后，就会导入其AutoConfiguration自动配置类，观察KafkaAutoConfiguration自动配置类：

   - application配置文件配置项前缀：`spring.kafka`
   - 添加了两个重要Bean：`KafkaTemplate`（收发消息）、`KafkaAdmin`（维护主题）

2. 配置消息队列：`spring.kafka.bootstrap-servers=IP地址:端口号`

3. 通过DI获取KafkaTemplate组件，调用其 `send()` 发送消息给消息队列

   ```java
   @Autowired
   private KafkaTemplate kafkaTemplate;
   // DI注入kafka组件
   
   StopWatch stopWatch = new StopWatch();// 创建一个秒表计时对象，用于统计发送给消息队列所用的时间
   stopWatch.start();			// 秒表开始计时
   
   CompletableFuture[] futures = new CompletableFuture[10000];
   // 创建一个存储消息对象的数组，存储 循环发送的1w条消息后 的消息对象
   for (int i = 0; i < 10000; i++) {
       futures[i] = kafkaTemplate.send("主题名", 消息key, 消息value);
       // 发送消息给消息队列，并将消息对象存储在数组中
   }
   CompletableFuture.allOf(futures).join();
   // 让消息数组中的所有消息对象的发送过程，都加入到主线程中
   
   /* 发送消息的过程，是异步过程。将所有消息对象存储在数组中，通过allof整合这个数组，再通过join一次性将所有消息发送的过程加入到主线程中，那么就会等所有消息都发送完后，主线程才会继续执行 */
   
   stopWatch.stop();		// 停止秒表计时
   long times = stopWatch.getTotalTimeMillis();		// 获取总共花费的时间（ms毫秒数）
   ```

   > 发送对象消息：
   >
   > - 默认情况下，send()发送消息时，消息队列只能存储 发送的消息value为String类型的，不能是对象类型
   > - 通过 `spring.kafka.producer.value-serializer=org.springframework.kafka.support.serializer.JsonSerializer` 可设置消息队列能存储JSON格式数据（默认值为 org.apache.kafka.common.serialization.StringSerializer）
   >
   > ```java
   > CompletableFuture future1 = kafkaTemplate.send("主题名", "person1", new Person());
   > CompletableFuture future2 = kafkaTemplate.send("主题名", "person2", "person");
   > future1.join();
   > future2.join();
   > // String类型和对象类型，都能被成功存储在消息队列中
   > ```

 

创建主题：

```java
@Bean
public NewTopic 方法名() {
    return TopicBuilder.name("主题名")
        .partitions(分区数).replicas(副本数)
        .compact().build();
}
```







#### 监听消息

1. 使用 `@EnableKafka` 注解标识启动类（开启kafka注解驱动功能）

2. 在组件的某个方法上标识 `@KafkaListener` 注解后，该方法就会自动监听消息

   ```java
   @Component			// 组件
   class 类名{
       
       @KafkaListener(topics="主题名",groupId="消费组名")
   	// 监听该主题下的消息，该消费者属于指定消费组
       
   /*
   	该方式只能监听之后的消息，之前存放于消息队列中的消息不能获取到
       重新定义@KafkaListener注解，使其能够获取到之前存放于消息队列中的消息：
           @KafkaListener(groupId = "消费组名",topicPartitions = {
                   @TopicPartition(topic = "主题名",partitionOffsets = {
                           @PartitionOffset(partition = "分区ID",initialOffset = "偏移量")
                   })
           })
           偏移量是指：从那个位置开始读取消息（之前读取不到消息队列中的消息，因为默认偏移量是末尾值）
   */
       public void 方法名(ConsumerRecord record){		// 该参数用于获取消息的详细信息
           String topic = record.topic();		// 获取消息所处的主题
           Object key = record.key();			// 获取消息key
           Object value = record.value();		// 获取消息value
       }
   }
   ```











### Spring Security

#### 介绍

Spring Security 安全架构实现的三大功能：

1. 用户认证（用户是否登录）
2. 权限校验（用户是否具有权限）
3. 攻击防护



原理：Servlet的三大组件：Servlet、Filter、Listener。客户端发送请求到服务器，期间可能会经历Filter进行过滤，而Spring Security就是利用 `FilterChainProxy` 封装了一些列的Filter，从而实现了安全拦截功能









#### 使用

> SpringBoot 3.0.5 +JDK17

1. 导入依赖

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-security</artifactId>
   </dependency>
   ```

   导入依赖后，就会导入对应的AutoConfiguration。查看 `SecurityAutoConfiguration`、`SecurityFilterAutoConfiguration`：

   - 观察 @EnableConfigurationProperties 注解，application配置文件配置项的前缀为：`spring.security`

   - @Import 了另一个配置文件：SpringBootWebSecurityConfiguration，观察该组件：

     - 添加了一个 `SecurityFilterChain` 组件，该组件就是 FilterChainProxy，默认提供了一套拦截规则：

       所有的请求都需要认证（登录）、开启默认表单登录的功能（未经登录时的请求会被转发到默认提供的登录页：用户名user，密码控制台随机生成）、开启httpbasic方式登录的功能

     - `@EnableWebSecurity` 注解内部，有引入了 WebSecurityConfiguration（web安全配置）、HttpSecurityConfiguration（Http安全配置）、@EnableGlobalAuthentication（AuthenticationConfiguration）（全局认证配置）等配置文件

2. 配置认证规则（仿照底层提供的 SecurityFilterChain）

   ```java
   @Bean
   SecurityFilterChain 方法名(HttpSecurity http) throws Exception {
       // 配置认证规则
       http.authorizeHttpRequests(request -> {		// lambda表达式，参考 Customizer 接口
           request.requestMatchers("/").permitAll()	// 允许 /（首页）的访问
               .anyRequest().authenticated();			// 其他请求需要经过认证
       });
       
       // 配置表单登录页面
       http.formLogin(login -> {					// lambda表达式，参考 Customizer 接口   
           	login.loginPage("/handler请求路径").permitAll()
           // 配置了thymeleaf后，请求就会交由thymeleaf解析，那么就会进行认证，因此需要允许该请求通过
           // 解析并访问handler，之后跳转到自定义的表单登录页面，在该页面中进行身份认证
                   
                   .loginProcessingUrl("/表单提交页面");
           		// <form th:action="@{/表单提交页面}" method="post"></form>
   // 表单最终提交的位置，是loginProcessingUrl指定的位置，那么该认证就会交由UserDetailsService处理
           });
       return http.build();
   }
   ```

3. 自定义用户信息校验

   > 通过添加一个UserDetailsService组件，可以让其去数据库中查询是否存在该用户，保存成功登录的信息（即通过认证）

   ```java
   @Bean
   public UserDetailsService userDetailsService(PasswordEncoder passwordEncoder) {
       return new UserDetailsService() {
           @Override
           public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
               // loadUserByUsername在表单提交后会自动调用，可以获取到表单提交的username
               // 通过username，可以传入service层方法，去数据库中根据用户名查询用户
               // 查询到用户后，若用户不存在，则抛出指定异常：UsernameNotFoundException
               if (用户对象 == null) {
                   throw new UsernameNotFoundException("用户不存在");
               }
    	// 用户存在，则将用户的信息保存到UserDetails中，表单提交的数据会与UserDetails中的数据进行比较
           // 当数据一致时，则通过认证（需要注意：表单提交的密码是加密过后的密码）
               return User.withUsername(username)
                   .password(passwordEncoder.encode(用户对象.getPassword()))
                   .authorities("权限")		// 权限必须有，且不能为空
                   .build();
           }
       };
   }
   
   @Bean		// 密码加密器
   public PasswordEncoder passwordEncoder() {
       return new BCryptPasswordEncoder();
   }
   ```

4. 自定义方法权限

   ① 在启动类上添加 `@EnableMethodSecurity` 注解（开启Spring Security注解功能）

   ② 在需要的handler方法上添加权限注解：

   - `@PreAuthorize("hasAuthority('权限')")`：在执行方法前，校验当前登录的用户是否有权限操作
   - `@PostAuthorize("hasAuthority('权限')")`：在方法返回内容前校验当前登录的用户是否有权限操作











### 可观测性

>对线上的应用进行观测、监控、预警等：
>
>- 健康状况：组件状态、存活状态等
>- 运行指标：CPU / 内存 占用率，垃圾回收情况、吞吐量、响应成功率等
>- 链路追踪

#### 基本使用

导入依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

导入依赖后，访问 `/actuator` 页面，就会有一些基础的指标信息

要想暴露所有的指标，需要在application配置文件中配置：

```yaml
management:
  endpoints:
    web:
      exposure:
        include: '*'
    enabled-by-default: true
# 以Web的方式暴露出所有的指标
```





#### 自定义健康状况与运行指标

自定义健康状况：

```java
@Component
class 类名 extends AbstractHealthIndicator {
    @Override
    protected void doHealthCheck(Health.Builder builder) throws Exception {
        // 去调用监控目标对象中，我们所自定义返回值的方法，若正常返回指定值，则说明为正常状态
        if (返回值 == 满足值) {
            builder.up().withDetail("key名", "value值").build();
            // 构建up状态的健康信息，withDetail可以用来自定义一些其他信息
        } else {
            builder.down().build();
            // 构建down状态的健康信息
        }
    }
}
```

- 访问 `/actuator/health` 查看健康状态信息
- 默认只能查看整体的健康状态，通过 `management.endpoint.health.show-details=always` 开启查看所有监控的健康状态



自定义运行指标（监控方法调用次数）：

```java
@Controller
class 类名{
    private Counter counter = null;
    // 在我们要监控的方法所处的类中，创建一个无参构造器
    	// SpringBoot无参构造器有一个特性：需要组件时，会自动注入
	public 类名(MeterRegistry meterRegistry){
        counter = meterRegistry.counter("指标名");
    }
    
    // 后续在我们需要监控的方法中，调用 counter.increment()，那么该方法每执行一次，count数量就+1
    // 访问 /actuator/metrics/指标名 进行查看
}
```







#### 面板监控

> Prometheus+Grafana实现

由Prometheus时序数据库，每隔5s从SpringBoot中抓取数据，再由Grafana从Prometheus获取时序数据，以面板方式展示出来

导入依赖：

```xml
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

导入依赖后，prometheus就会开始抓取数据，访问 `/actuator/prometheus` 可查看数据



案例：将boot应用上传并部署在Linux服务器，并配置监控面板

1. 将boot应用打包（且确保运行时端口不冲突），并上传到Linux服务器

   `yum install -y lrzsz`：安装一个上传文件工具（也可xshell文件传输）

   `rz`：运行该命令后，选择要上传的Jar即可

2. Linux下载安装JDK17

   - `wget https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.tar.gz`：下载JDK

   - `mkdir /opt/java`：创建一个存放解压后的Java的文件夹

   - `tar -zxf jdk-17_linux-x64_bin.tar.gz -C /opt/java`：解压并存放到指定文件夹下

   - `vi /etc/profile` 配置环境变量：

     ```shell
     export JAVA_HOME=/opt/java/jdk-17.0.9		# 到java的bin文件夹所在的那层目录
     export PATH=$PATH:$JAVA_HOME/bin
     ```

   - `source /etc/profile`：让环境变量生效

3. 后台运行boot应用：`nohup java -jar jar包 &> nohup.log &`

   > `nohup java -jar jar包 &`：该命令即可后台运行boot应用，但会实时生成日志
   >
   > `&> 文件名`：让日志输出也后台运行，且保存到指定位置处

4. 修改prometheus配置文件（之前环境搭建时的 `/prod/prometheus.yml`），让其监控boot应用的数据指标

   ```yml
   global:
     scrape_interval: 15s
    evaluation_interval: 15s
   scrape_configs:
     - job_name: 'prometheus'
       static_configs:
         - targets: ['localhost:9090']
     - job_name: 'redis'
       static_configs:
         - targets: ['redis:6379']
     - job_name: 'kafka'
       static_configs:
         - targets: ['kafka:9092']
   
   # 采集自定义boot应用的指标信息：/actuator/prometheus页面
     - job_name: '显示名称'
       metrics_path: '/actuator/prometheus'		# 从SpringBoot中抓取数据
       static_configs:
         - targets: ['IP地址:端口号']				# boot应用部署的服务器IP、端口
   ```
   
   访问Prometheus界面：`http://IP地址:9090`，Status --> Targets，若自己定义的显示名称的状态为UP，表明正常抓取到数据（若非正常启动，建议查看Prometheus的日志）
   
5. 访问Grafana界面：`http://IP地址:3000`，菜单栏 --> Connections --> Data sources --> Prometheus，设置Connection，然后保存测试

   成功之后，Build a dashboard --> Import dashboard --> Load加载ID：12900 --> 选择刚才创建Prometheus --> Import

   > Load 加载的是界面样式，可从 `https://grafana.com/grafana/dashboards/` 网站中，查找SpringBoot，只要兼容Prometheus即可使用









### AOT

#### 基本介绍

AOT（Ahead-of-Time）：提前编译，在程序执行前，全部编译为机器码

JIT（Just in Time）：即时编译，程序边编译边运行



编译：源代码（如.c、.java）转化为机器码的过程。编译有两种方式：

1. Complier编译器：AOT原理，提前编译。机器执行速度快，因为源代码一次性转换；开发效率慢，要耗费大量时间进行编译调试
2. Interpreter解释器：JIT原理，即时编译。机器执行速度慢，源代码一行一行转换后才执行；开发效率快，逐行执行可以进行快速调试



Java是 半编译半解释型语言，基本执行流程：

- .java 文件被 javac，经过词法、语法、语义等的分析，转换为 .class 文件
- .class 文件在JVM中执行时：
  - 先判断缓存区中是否存在已编译的机器码，若存在，则直接调用
  - 当不存在机器码时，会进行热点探测，检测当前要解释执行的代码是否已达到热点阈值：若没有，则解释运行；若已达到热点阈值，则会将代码编译为机器码，保存到缓存区中，供下次直接调用
    - 在编译为机器码前，会先判断是否进行同步编译：若是，则线程阻塞，编译为机器码，再调用；若不是，则此次先继续解释执行，同时开启另一个线程进行编译，后续再使用缓存区中存放的机器码

JVM内部的编译器有两种：Client Compiler 和 Server Compiler

- Client Compiler：注重启动速度和局部的优化
- Server Compiler：注重全局优化，性能更好，但会进行更多的全局分析，所以启动速度较慢







#### GraalVM

##### 介绍

Cloud Native云原生 态下，Java编译执行的特点就突出了问题：Java若是要用jar，就会先解释执行，只有热点代码才会被编译，初始启动速度就会显得特别慢。大型的云平台，要求应用要能够秒启动，对应用启动效率要求极高

因此，我们希望Java应用也能够被提前编译为机器码，可以随时急速启动，一旦启动就是最佳的运行状态。且被编译为机器码后，还可以无需安装Java环境（机器码可以在对应操作系统的机器上直接运行）

GraalVM 利用 native-image 工具，在满足一定的编译环境后，会将应用打包成适配本机平台的可执行文件（原生镜像、本地镜像、机器码）





##### Window环境配置

1. 安装window平台所需要的编译环境：VisualStudio中，安装好C++环境、Window桌面开发环境

   >当存在一个 Native Tools 的黑色命令窗，且打开后能正常显示命令行，则说明编译环境安装成功

2. 解压graalvm，配置环境变量：JAVA_HOME：graalvm所在目录（bin文件夹所在的那层目录）

   > 当使用 java -version 显示的为 GraalVM 时，说明成功

3. 安装 native-image 工具：在工具所在目录下运行命令 `gu install --file 文件名.jar`

   > 当使用命令 `native-image --help` 没有报错时，则说明成功安装





##### Linux环境配置

1. 安装Linux平台所需要的编译环境：`yum install -y gcc glibc-devel zlib-devel`

2. 安装Java JDK17：

   - `wget https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.tar.gz`：下载JDK
   - `mkdir /opt/java`：创建一个存放解压后的Java的文件夹
   - `tar -zxf jdk-17_linux-x64_bin.tar.gz -C /opt/java`：解压并存放到指定文件夹下

3. 安装GraalVM：

   - 解压并存放于 /opt/java 目录下

   - `vi /etc/profile` 配置环境变量：

     ```shell
     export JAVA_HOME=/opt/java/graalvm-java17		# 到GraalVM的bin文件夹所在的那层目录
     export PATH=$PATH:$JAVA_HOME/bin
     ```

   - `source /etc/profile`：让环境变量生效

   - `java --version` 查看是否显示 GraalVM

4. 安装 native-image 工具：在工具所在目录下运行命令 `gu install --file 文件名.jar`

   > 当使用命令 `native-image --help` 没有报错时，则说明成功安装





##### 镜像生成

使用Native Tools工具运行程序：`native-image -cp 文件路径.jar 主类全类名 -o 生成文件名`

- 在应用的创建上，需要有所调整：选择使用 GraalVM 的JDK
- 应用打包时，若是普通Java程序（非Boot应用），需要指定程序的入口：进入jar包中，在 `MANIFEST.MF` 文件末尾添加 `Main-Class: 主类全类名`

> 也可以通过 .class 文件（target/classes 目录下）来进行镜像生成：`native-image -cp classes层目录路径 主类全类名 -o 生成的文件名`

并非所有的Java程序都能支持镜像生成的：

- 动态能力损失（如反射），需要提前告知GraalVM反射所使用的方法、构造器
- 配置文件丢失：需要提前告知GraalVM配置文件在哪













## 其他知识点

### Lombok

用于简化JavaBean的开发（可自动生成构造器、get/set方法等）

1. 导入依赖

   ```xml
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.30</version>
   </dependency>
   ```

2. 在需要使用的载体类上使用对应注解

   - `@Data`：自动生成get/set方法
   - `@NoArgsConstructor`：自动生成无参构造器
   - `@AllArgsConstructor`：自动生成全参（所有参数）构造器









### 常用方法

`StringUtils.isEmpty(String)`：判断字符串是否为 null 或 ""

`CollectionUtils.isEmpty(Collection)`：判断集合内是否有元素

`BeanUtils.copyProperties(Object resource,Object target)`：将 resource 实体类中的属性值复制到 target 实体类中





