## 概念介绍

### MVC

- MVC是一种软件架构思想，将软件按照 模型、视图、控制器 来划分

- M：Model，模型层，指的是工程中的JavaBean，作用是处理数据

  - JavaBean分为两种：

    实体类Bean：专门存储业务数据的，如Student、User等

    业务处理Bean：指的是Service或Dao对象，是专门用于处理业务逻辑和数据访问的对象

- V：View，视图层，指的是工程中的HTML或JSP等页面，作用是 与用户进行交互、展示数据

- C：Controller，控制层，指的是工程中的Servlet，作用是 接收请求和响应数据

MVC的大体工作流程：

- 用户通过View视图层，发送请求到服务器，请求被Controller控制层接收，并调用相应的Model模型层进行处理，最后再将结果返回到Controller控制层，由Controller将请求处理的结果定位到对应的View视图层，将数据渲染后响应给客户端





### SpringMVC

SpringMVC是Spring的一个子项目，是Spring为表述层提供的一套解决方案

- 三层架构：表述层（展示前台页面和处理后台Servlet）、业务逻辑层、数据访问层

SpringMVC特点：

- 与Spring无缝对接
- 基于原生的Servlet，封装了功能更强大的前端控制器 DispatcherServlet，对请求和响应进行了统一处理
- 对表述层各细分领域中需要解决的问题进行了全方位覆盖，提供全面的解决方案
- 代码简洁，提高开发效率
- 内部组件化程度高，可插拔式组件即插即用
- 性能卓越









## 准备工作

> 以jdk8来进行

1. 创建Maven工程

   - 创建父工程、子工程

   - 修改子工程为web工程

     > `<packaging>web</packaging>`

   - 在 src/main 下添加 webapp 目录，以及 webapp/WEB-INF/web.xml 目录文件

     > project structure --> Facets --> 选择模块 --> + --> Web --> 指定正确路径

   - 设置Maven使用版本、仓库位置，以及设置自动导入Maven依赖
   
2. 引入依赖：spring-webmvc、logback-classic、javax.servlet-api、thymeleaf-spring

   ```xml
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-webmvc</artifactId>
       <version>5.3.1</version>
   </dependency>
   <dependency>
       <groupId>ch.qos.logback</groupId>
       <artifactId>logback-classic</artifactId>
       <version>1.2.3</version>
   </dependency>
   <dependency>
       <groupId>javax.servlet</groupId>
       <artifactId>javax.servlet-api</artifactId>
       <version>3.1.0</version>
       <scope>provided</scope>
   </dependency>
   <dependency>
       <groupId>org.thymeleaf</groupId>
       <artifactId>thymeleaf-spring5</artifactId>
       <version>3.0.12.RELEASE</version>
   </dependency>
   ```

3. 修改web.xml配置文件：配置SpringMVC的前端控制器

   ```xml
   <servlet>
   	<servlet-name>映射关系名</servlet-name>
       <servlet-class>
           org.springframework.web.servlet.DispatcherServlet
       </servlet-class>
       <!-- 设置SpringMVC配置文件的位置和名称 -->
       <init-param>
       	<param-name>contextConfigLocaltion</param-name>
           <param-value>classpath:配置文件名.xml</param-value>
       </init-param>
       <!-- 设置前端控制器的初始化时间提前 -->
       <load-on-startup>1</load-on-startup>
   </servlet>
   <servlet-mapping>
   	<servlet-name>映射关系名</servlet-name>
       <url-pattern>/</url-pattern>
   </servlet-mapping>
   ```

   - 设置SpringMVC配置文件路径的意义：
     - 默认情况下，SpringMVC的配置文件，是位于WEB-INF下，默认名为`（servlet-name）-servlet.xml`，
     - 但是，一般情况下，我们要将所有的配置文件都存放到 src/main/resources 目录下
   - 设置前端控制器初始化时间的意义：
     - 由于前端控制器会处理所有的请求，因此其初始化时间会较长。将其在第一次访问时再初始化（默认）会极大影响用户体验
   - 请求路径意义：
     - / 所匹配的请求，可以是 /xxx 、.html、.js、.css 等，但不匹配 .jsp 的请求
     - 因为 .jsp 本身也是需要被自己的Servlet处理的，因此不能被前端控制器所处理
     - 若是 /* ，表示匹配所有的请求，就会包含 .jsp
   
4. 在SpringMVC配置文件中，配置Thymeleaf视图解析器

   ```xml
   <bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
       <!-- 设置模板的优先级 -->
       <property name="order" value="1"/>
       <!-- 设置模板的编码 -->
       <property name="characterEncoding" value="utf-8"/>
       <property name="templateEngine">
           <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
               <property name="templateResolver">
                   <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                       <!-- 设置视图前缀 -->
                       <property name="prefix" value="/WEB-INF/前缀路径/"/>
                       <!-- 设置视图后缀 -->
                       <property name="suffix" value="后缀名"/>
                       <property name="templateMode" value="HTML5"/>
                       <property name="characterEncoding" value="utf-8"/>
                   </bean>
               </property>
           </bean>
       </property>
   </bean>
   ```

5. 在SpringMVC配置文件中，将要作为前端控制器的类，注入到IoC容器中（使用 @Controller 注入）

6. 在前端控制器类中，创建方法处理对应请求

   ```java
   @RequestMapping(value="请求路径")
   public String 方法名(){
       return 视图页面;
   }
   ```

   > 1. 浏览器发送请求，若请求地址符合前端控制器url-pattern的处理，则该请求会被前端控制器所拦截。
   > 2. 接着，前端控制器会根据SpringMVC的核心配置文件，找到控制器类，将请求地址和@RequestMapping注解中的value属性值进行匹配，若成功，则该注解标识的方法就是处理请求的方法。
   > 3. 处理请求后，会返回一个字符串类型的视图名称，该视图名称会被视图解析器进行解析后进行渲染，最后转发到对应的视图页面









## @RequestMapping

> SpringMVC前端控制器在收到指定的请求后，会根据映射关系，找到对应的控制器方法来处理请求

### 注解使用位置

- @RequestMapping 标识一个类：映射请求时，请求路径的初始化信息

- @RequestMapping 标识一个方法：映射请求时，请求路径的具体信息

  > 映射请求时，会根据请求路径映射到请求路径的初始化信息，再在初始化信息的基础上，映射到请求路径的具体信息上
  >
  > ```java
  > @Controller			// 注入IoC容器
  > @RequestMapping("/user")	// 请求路径的初始化信息
  > public class 类名{
  >     @RequestMapping("/index")		// 请求路径的具体信息
  >     public String toIndex(){
  >         return "index";
  >     }
  > }
  > 
  > <a th:href="@{/user/index}">链接</a>			// 此时的请求路径
  > ```





### 属性

#### value

- @RequestMapping注解是通过value属性的值，来匹配请求的路径
- value属性值是一个字符串类型数组，表示可以有多个请求路径映射到该值
- value属性值必须设置，且不同的@RequestMapping，若仅设置了value属性，则属性值不能相同



#### method

- method属性值是用来指定接收的请求的请求方式
- method属性值是一个RequestMethod类型的数组，表示可以有多种请求方式映射到该请求路径（RequestMethod.GET 、RequestMethod.POST 等）
- 若能成功映射到请求路径，但请求的方式不满足，则会报405错误
- SpringMVC中，提供了@RequestMapping的派生注解，如：@GetMapping、@PostMapping（相当于method已经确定为GET、POST）
- 目前浏览器只支持 GET 和 POST 的请求方式。若在form表单提交时，method属性中指定了其他的请求方式，则默认按GET请求进行处理



#### params

- params属性值要求：映射到该路径的请求，必须能匹配params属性所有的值
- params属性值是一个字符串类型数组，表示所有的条件必须满足。有四种条件类型：
  - `"参数名"`：要求映射到该路径的请求，必须携带指定的请求参数
  - `"!参数名"`：要求映射到该路径的请求，不能携带指定的请求参数
  - `"参数名=值"`：要求映射到该路径的请求，必须携带指定的请求参数，且请求参数的值必须为指定的值（若值为字符串，则需要添加 `''` ）
  - `"参数名!=值"`：要求映射到该路径的请求，必须携带指定的请求参数，且请求参数的值，必须不等于指定的值
- 若能成功映射到请求路径，但请求参数的条件不满足，则会报400错误



#### headers

- headers属性值要求：映射到该路径的请求，必须能匹配headers属性所有的值
- headers属性值是一个字符串类型数组，表示所有的条件都必须满足。有四种条件类型：
  - `"请求头参数名"`：要求映射到该路径的请求，必须携带指定的请求头参数
  - `"!请求头参数名"`：要求映射到该路径的请求，不能携带指定的请求头参数
  - `"请求头参数名=值"`：要求映射到该路径的请求，必须携带指定的请求头参数，且请求头参数的值必须为指定的值
  - `"请求头参数名!=值"`：要求映射到该路径的请求，必须携带指定的请求头参数，且请求头参数的值，必须不等于指定的值
- 若能成功映射到请求路径，但请求头参数的条件不满足，则会报404错误





### ant风格路径

> 匹配请求路径时，我们可以使用模糊匹配

1. `?`：表示任意单个字符

   ```java
   @RequestMapping("/a?a/index")
   // 【/aaa/index】、【/a-a/index】等都可以生效
   // 一些特殊符号无效，如：?、/
   ```

2. `*`：表示任意0个或多个字符

   ```java
   @RequestMapping("/a*a/index")
   // 【/aaaa/index】、【/aa/index】等都可以生效
   // 一些特殊符号无效，如：?、/
   ```

3. `**`：当 \** 单独占一个目录时（如： `/**/请求路径`），表示任意0层或多层目录结构

   ```java
   @RequestMapping("/**/index")
   // 【/aaaa/index】、【/aa/aa/index】、【/index】等都可以生效
   // 一些特殊符号无效，如：?
   ```

   



### Restful风格下的占位符

> 当发送请求时，若数据是以路径的方式发送，则可以有两种风格：
>
> 1. 原始方式：`/test?id=1`
> 2. restful风格：`/test/1`
>
> 在原始风格中，我们可以通过 key 获取到 value 值
>
> 而在restful风格中，我们就需要通过占位符 {key} 来获取到 value 值

```java
<a th:href="@{/test/1/July}">链接</a>			// 请求路径

@RequestMapping("/test/{id}/{name}")		// 映射路径，1对应id，July对应name
public String test(@PathVariable("id")int id,@PathVariable("name")String name){
// @PathVariable("请求参数名")参数类型 变量名		-->		获取请求参数
    return null;
}
```

> 占位符必须对应，请求路径才会正确映射









## 获取请求参数

### ServletAPI获取

> 前端控制器在处理控制器方法时，若发现控制器方法中，需要如：HttpServletRequest等 参数时，依旧会提供其对象
>
> - 不推荐使用

直接通过 `HttpServletRequest对象.getParameter("请求参数名")` 的方式获取参数值

```java
@RequestMapping("请求路径")
public String 方法名(HttpServletRequest request){
    String 变量名 = request.getParameter("请求参数名");
}
```





### 控制器方法的形参获取

> 在控制器方法的形参位置处，设置和请求参数同名的形参。当浏览器发送请求，并能映射到该路径时，前端控制器会自动将请求参数的值赋值到所对应的形参中

```java
@RequestMapping("请求路径")
public String 方法名(参数类型 请求参数名) {}
```

- 若请求参数中有多个同名的参数（如复选框），此时，获取到的多个请求参数的值之间，使用了 `,` 隔开（也可以选择使用数组来接收参数值）
- 若成功映射到该请求路径，但请求参数没有传递时，则参数值为默认值null





### @RequestParam注解获取

#### @RequestParam

@RequestParam 是用来：为请求参数和控制器方法的形参创建映射关系

> 当请求参数名与控制器方法形参名不一致时，就获取不到参数值。因此，可以使用@RequestParam，将 控制器参数 与 要获取的请求参数 之间绑定起来

```java
@RequestMapping("请求路径")
public String 方法名(@RequestParam(value="请求参数名")参数类型 形参名){}
```

@RequestParam 其他参数值：

- required：设置 是否必须传输此请求参数
  -  true（默认值）：当获取不到该请求参数、且没有设置默认值时，报400错误
  - false：若获取不到该请求参数，则该形参值为默认值null
- defaultValue：设置默认值。当获取不到请求参数，或是请求参数的值为空字符串（`""`），则形参值为该值



#### @RequestHeader

@RequestHeader 是用来：获取请求头中的参数值

- 和@RequestParam拥有一样的参数
- 和@RequestParam有所不同的是，想要获取请求头参数，是必须使用@RequestHeader来获取的

```java
@RequestMapping("请求路径")
public String 方法名(@RequestHeader("请求头参数") String 形参名) {}
```



#### @CookieValue

@RequestHeader 是用来：获取请求头中的Cookie值

- 和@RequestParam拥有一样的参数
- 和@RequestParam有所不同的是，想要获取Cookie值，是必须使用@CookieValue来获取的

```java
@RequestMapping("请求路径")
public String 方法名(@CookieValue("cookie的key名") String 形参名) {}
```





### 实体类直接获取

可以在控制器方法的形参位置处设置一个实体类类型的形参。此时，若传输过来的请求参数，与该实体类中的属性对应，则会自动为该实体类中的属性赋值

```java
class 实体类名{
    private 数据类型 属性名;
    // 经测试，set方法必须提供，否则无法赋值。但最好：无参、get、set等都提供
}

@RequestMapping("请求路径")
public String 方法名(实体类类型 形参名) {}
```

- 若有请求参数，实体类中无该属性，则该请求参数需额外获取
- 若无请求参数，实体类中有该属性，则该实体类中的属性值为默认值









## 设置字符集

> GET请求中：
>
> - 在TomCat8之前的配置文件server.xml中，通过\<Connector>标签中的 URIEncoding 属性，可以直接设置默认编码集
> - 在TomCat8及之后，已不需要额外配置，默认编码集即为UTF-8

设置获取参数的字符集，不能仅仅通过获取参数HttpServletRequest来进行设置

- 因为前端控制器内部已经帮我们获取了请求参数，而要想设置获取到的请求参数的字符集，是需要在获取请求参数前，就设置好字符集的

要想解决字符编码问题，就需要在前端控制器获取参数前，就将字符集进行设置

- 请求的处理流程：Listener（如：ServletContextListener 监听上下文，但是仅在服务器初始化时执行一次，不能处理每次请求）--> Filter --> Servlet（DispatcherServlet 前端控制器）

因此，就要考虑使用Filter进行编码设置

SpringMVC提供了一个编码过滤器 CharacterEncodingFilter，可以用来进行编码设置，但需要先在 web.xml 中进行注册，然后进行配置：

```xml
<filter>
    <filter-name>映射关系名</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>

<!-- 
   深入源码，找寻doFilter方法，可看到doFilter中调用了CharacterEncodingFilter.doFilterInternal() 
   doFilterInternal中，判断了encoding，encoding即为编码集，默认值为null
   设置了encoding后，由于没有设置request.getCharacterEncoding()的值，默认为null，所以可以设置编码
-->	
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
    
<!--
	继续查看，发现若想要设置响应编码集，则必须设置属性forceResponseEncoding=true
-->
    <init-param>
        <param-name>forceResponseEncoding</param-name>
        <param-value>true</param-value>
    </init-param>    
</filter>

<filter-mapping>
    <filter-name>映射关系名</filter-name>
    <url-pattern>/*</url-pattern>
    <!-- 设置编码集应作用于所有的请求上，因此使用 /* 代表所有请求 -->
</filter-mapping>
```









## 域对象中保存数据

### request作用域

#### 利用ServletAPI

直接通过 `HttpServletRequest对象.setAttribute(String keyName,Object value)` 的方式将值存入到request作用域

```java
@RequestMapping("请求路径")
public String 方法名(HttpServletRequest request){
    request.setAttribute("keyName", value);
}

<p th:text="${keyName}"></p>
// 在前端页面中，使用thymeleaf语法，直接通过 ${keyName} 获取对应的value值
```





#### 利用ModelAndView

- ModelAndView具有 Model 和 View 的功能
  - Model主要用于项请求域中共享数据
  - View主要用于设置视图，实现页面渲染和跳转
  
- 观察源码：在方法栈中，可以发现调用了DispatcherServlet类中的doDispatch方法。而该方法在内部调用其他方法后，结果是返回一个ModelAndView对象

  因此可以得出结论：无论是哪种方式向域对象中保存数据，结果都会重新封装为一个ModelAndView对象

```java
@RequestMapping("请求路径")
public ModelAndView 方法名() {
    
    // 1.创建ModelAndView对象
    ModelAndView mav = new ModelAndView();
    
    // 2.向ModelAndView对象中设置请求域
    mav.addObject("keyName", value);
    
    // 3.设置跳转渲染的视图名称
    mav.setViewName("视图名称");
    
    // 4.返回ModelAndView对象
    return mav;
    
}

// 使用了ModelAndView，若想要共享数据起作用，则方法的返回值就必须为ModelAndView
```





#### 利用Model / Map / ModelMap

1）使用Model向request域对象中共享数据：

```java
@RequestMapping("请求路径")
public String 方法名(Model 形参名) {			// 传递Model类型参数
    形参名.addAttribute("keyName", value);
}
```



2）使用Map向request域对象中共享数据：

```java
@RequestMapping("请求路径")
public String 方法名(Map<String,Object> 形参名) {			// 传递Map类型参数
    形参名.put("keyName", value);
}
```



3）使用ModelMap向request域对象中共享数据：

```java
@RequestMapping("请求路径")
public String 方法名(ModelMap 形参名) {			// 传递ModelMap类型参数
    形参名.addAttribute("keyName", value);
}
```



Model、Map、ModelMap 三者间的关系：

- ```java
  public class ModelMap extends LinkedHashMap<String, Object> {}
  public class LinkedHashMap<K,V> extends HashMap<K,V> implements Map<K,V>
  ```

- ```java
  public class BindingAwareModelMap extends ExtendedModelMap {}
  public class ExtendedModelMap extends ModelMap implements Model {}
  ```

所以，BindingAwareModelMap 是 Map、Model 的实现类，是 ModelMap 的子类，而传入Model、Map、ModelMap类型，从本质上可以看做是传入了BindingAwareModelMap类型







### Session作用域

> 使用ServletAPI处理较为简单

```java
@RequestMapping("请求路径")
public String 方法名(HttpSession 形参名){
    形参名.setAttribute("keyName",value);
}
```







### Application作用域

> 使用ServletAPI处理较为简单

```java
@RequestMapping("请求路径")
public String 方法名(HttpSession 形参名){
    ServletContext 变量名 = 形参名.getServletContext();
    变量名.setAttribute("keyName",value);
}
```









## 视图处理

> 视图的作用是渲染数据，将模型Model中的数据展示给用户
>
> SpringMVC中的视图是View接口，大体可分为三种技术：转发、重定向、模板引擎
>
> 通过是否被前缀修饰来确定调用哪个技术

### ThymeleafView

> 模版引擎技术
>
> 若使用了视图技术Thymeleaf，且在SpringMVC的配置文件中，配置Thymeleaf的视图解析器；当视图经过该解析器解析后，得到的就是ThymeleafView

- 当控制器方法中，返回的视图名称没有任何前缀修饰时，此时的视图名称会被SpringMVC配置文件中所配置的视图解析器解析，视图名称拼接上视图前缀和后缀，得到最终路径，最终通过转发的方式实现跳转

```java
@RequestMapping("请求路径")
public String 方法名(){
    return "视图名称";
}
```

> 观察源码：
>
> 1. 找到方法栈中，DispatcherServlet中的方法doDispatch，可知返回的视图名称，会重新封装为一个ModelAndView
> 2. 接受到mv属性值后，继续往下走，会发现调用了processDispatchResult方法，进入方法内部
> 3. 在方法内部进行了一系列判断后，会调用render方法，进入方法内部
> 4. 在执行完resolveViewName方法后，就会得到一个封装了要调用的视图技术的View对象







### InternalResourceView

> 转发视图技术

- 当控制器方法中返回的视图名称以 `forward:` 为前缀时，此时视图名称不会被SpringMVC配置文件中的视图解析器所解析，而是创建InternalResourceView视图，将前缀 forward: 去掉，将剩余部分作为最终路径，以转发的方式实现跳转

```java
@RequestMapping("请求路径")
public String 方法名() {
    return "forward:转发路径";
}
```

- 若要转发的页面需要经过Thymeleaf渲染，则需要借助servlet来处理







### RedirectView

> 重定向视图技术

- 当控制器方法中返回的视图名称，以 `redirect:` 为前缀时，则此时的视图名称并不会被SpringMVC配置文件中所配置的视图解析器所解析，而是创建RedirectView视图，将前缀 redirect: 去掉，将剩余部分作为最终路径，以重定向的方式实现跳转

```java
@RequestMapping("请求路径")
public String 方法名(){
    return "redirect:重定向路径";
}
```

- 若要重定向的路径在WEB-INF下，则需要借助servlet来处理才能访问
- 若要重定向的页面需要经过Thymeleaf渲染，则需要借助servlet来处理







### 视图控制器

当控制器方法仅仅是用来实现页面跳转（即：只返回了视图名称），而没有其他处理逻辑时，则可以使用 \<mvc:view-controller> 标签来代替该控制器方法

```xml
<mvc:view-controller path="请求的映射路径" view-name="返回的视图名称"/>
```

但是，只要设置了该标签，就会导致控制器方法中，所有的请求映射关系全部失效。

若想要使控制器方法内的其他请求映射关系依旧生效，则需要额外添加标签 \<mvc:annotation:driven/> 来开启SpringMVC的注解驱动

```xml
<mvc:annotation-driven/>
```











## RESTFul

### 介绍

- RESTFul是一种风格，提倡URL地址使用统一的风格设计：从前到后所有单词以 / 分隔，不再使用 ?key=value 的方式携带请求参数，而是将要发送给服务器的数据作为URL地址的一部分。能保证整体风格的一致性

- 使用HTTP协议的四种请求方式来代表不同的操作： GET（查）、POST（增）、PUT（改）、DELETE（删）

  | 操作 |     传统方式     |       REST风格       |
  | :--: | :--------------: | :------------------: |
  |  查  | getUserById?id=1 |  （GET请求）user/1   |
  |  增  |     addUser      |   （POST请求）user   |
  |  删  | deleteUser?id=1  | （DELETE请求）user/1 |
  |  改  |    updateUser    |   （PUT请求）user    |







### 发送 DELETE / PUT 请求

- 默认情况下，浏览器是不支持发送 DELETE / PUT 请求的
- 因此，我们可以借助SpringMVC为我们提供的过滤器 HiddenHttpMethodFilter ，来设置发送DELETE或PUT请求

```xml
<!-- 配置web.xml配置文件 -->
<filter>
    <filter-name>映射关系名</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>映射关系名</filter-name>
    <url-pattern>/*</url-pattern>
    <!-- /* 拦截所有的请求，因为所有的请求都有可能要使用 DELETE/PUT的请求方式 -->
</filter-mapping>
```

> 仅仅配置 HiddenHttpMethodFilter 过滤器是不够的。观察源码：
>
> - 可以看到 doFilterInternal方法 中执行了放行方法，因此仔细分析该方法
> - 若想要执行后续代码，则 `"POST".equals(request.getMethod())` 必须成立，因此，必须使用 `POST`
> - 从 `request.getParameter(this.methodParam)` 可以知道，会获取一个名为 `_method` 的请求参数；观察后续代码，可以发现：只要该 _method 参数的值为【delete/put/patch】其中的一个，就会新创建一个该 _method 为请求方式的Request对象

因此，还需要的额外条件：

1. 发送请求的方式，必须为 POST
2. 携带请求参数 _method，值可为 delete/put/patch

```html
<form action="请求路径" method="post">
    <input type="hidden" name="_method" value="delete或put或patch">
</form>
```

> 还有一个需要注意的：
>
> - 由于配置 HiddenHttpMethodFilter过滤器 后，源码中会获取请求参数，会间接导致 CharacterEncodingFilter过滤器 的字符集编码设置失效。因此，CharacterEncodingFilter过滤器 的设置应该在 HiddenHttpMethodFilter过滤器 的设置之前









## 访问静态资源

> TomCat的配置文件web.xml中，有一个默认的 DefaultServlet ，是默认的用来处理请求的Servlet
>
> 该Servlet处理的是 / 下的所有资源，与我们所配置的SpringMVC的 DispatcherServlet 产生了冲突
>
> 因此，根据就近原则，工程中的 web.xml 下的 DispatcherServlet 生效，而默认的 DefaultServlet 失效
>
> 从而使我们所有的请求，都是 DispatcherServlet 在处理；当我们没有添加映射关系时，就会出现404错误
>
> 而静态资源的访问，是不需要添加映射关系的
>
> 我们就需要想办法让默认的 DefaultServlet 来处理访问静态资源的请求

```xml
<mvc:default-servlet-handler/>
```

- 添加该标签后，就会启用默认的 DefaultServlet
- 当 SpringMVC 中的 DispatcherServlet（根据映射关系）找不到资源时，就会交给 DefaultServlet 来处理
- 只有当 DefaultServlet （在 / 下进行查找）也找不到资源时，才会由 DefaultServlet 报出404错误









## HttpMessageConverter

> 报文信息转换器，用来将请求报文转换为Java对象，或将Java对象转换为响应报文

### 请求转换

#### @RequestBody

> 用于获取请求体
>
> 需要注意的是，POST才有请求体

```java
@RequestMapping("请求路径")
public String 方法名(@RequestBody String 形参名){}
```

- 将获取到的请求体，存放到字符串类型的变量中





#### RequestEntity

> 可用于获取请求报文

```java
@RequestMapping("请求路径")
public String 方法名(RequestEntity<String> 形参名) {
    形参名.getHeaders();			// 获取请求头信息
    形参名.getBody();				// 获取请求体信息
}
```

> 比较值得注意的字段如：referer，获取页面跳转来源。可以用来分页时，删除数据后依旧跳转到原来页面







### 响应转换

#### @ResponseBody

> 将内容响应给客户端

1）响应普通文本内容

```java
@RequestMapping("请求路径")
@ResponseBody
public 返回值类型 方法名() {
    return 内容;
}
```

- 在添加了@ResponseBody注解后，返回的内容就不再是视图名称，而是要响应给客户端的文本内容



2）响应JSON对象

在满足了一定条件后，响应的java对象就会被自动转换为json对象，再响应给客户端

1. 引入依赖：jackson-databind

   ```xml
   <dependency>
       <groupId>com.fasterxml.jackson.core</groupId>
       <artifactId>jackson-databind</artifactId>
       <version>2.12.3</version>
   </dependency>
   ```

2. 开启mvc的注解驱动

   ```xml
   <mvc:annotation-driven/>
   ```

3. 在控制器方法上声明了注解：`@ResponseBody`

4. 将java对象直接作为返回值（经测试，该java对象中，至少需要提供 get、set 方法的）

   ```java
   @RequestMapping("请求路径")
   @ResponseBody
   public T 方法名(){
       return new T();
   }
   ```



3）@RestController：SpringMVC提供的一个复合注解，标识在控制器的类上，就相当于添加了 @Controller 注解，且该类中的所有方法都添加了 @ResponseBody 注解





#### ResponseEntity

> 用来设置响应报文

##### 文件下载

实现基本步骤：

1. 获取文件部署后存放的路径
2. 读取文件内容，并将文件内容作为响应体
3. 设置响应头和响应状态码
4. 响应报文

```java
<a th:href="@{请求路径}"></a>

@RequestMapping("请求路径")
public ResponseEntity<byte[]> 方法名(HttpSession session) throws IOException {
    // 获取上下文对象，用于获取指定资源部署在服务器上后，存放的路径
    ServletContext servletContext = session.getServletContext();
    
    // 获取资源存放在服务器上的路径
    String realPath = servletContext.getRealPath("文件路径");
    
    // 获取文件输入流，读取文件内容
    FileInputStream fis = new FileInputStream(realPath);
    
    // 创建数组，大小为：文件输入流中的总字节数
    byte[] file = new byte[fis.available()];
    
    // 将文件输入流中的内容读取后，存放到数组中
    fis.read(file);
    
    // 创建请求头
    MultiValueMap<String, String> headers = new HttpHeaders();
    headers.add("Content-Disposition", "attachment;filename=文件名");
    
    // 设置响应状态码
    HttpStatus status = HttpStatus.OK;
    
    // 创建响应报文对象
    ResponseEntity<byte[]> response = new ResponseEntity<>(file, headers, status);
    
    // 关闭资源
    fis.close();
    
    // 响应报文
    return response;
}
```







##### 文件上传

- 文件上传只能为POST或PUT请求，且必须指定 enctype 类型为 `multipart/form-data`

  ```html
  <form th:action="@{请求路径}" method="post" enctype="multipart/form-data">
      <input type="file" name="picture">
      <input type="submit" value="fileUp">
  </form>
  ```

- 文件上传功能的实现，需要引入依赖：commons-fileupload

  ```xml
  <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.1</version>
  </dependency>
  ```

- 通过请求参数名，可以获取到一个 MultipartFile 类型的形参。但是，默认上传的文件的类型是无法封装到MultipartFile类型中的，因此，我们需要在SpringMVC的配置文件中，配置文件上传解析器

  ```xml
  <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"/>
  ```

  > 该文件上传解析器，是通过id来获取的，因此，id值固定

- 将上传的文件重命名，并转存到指定路径

  ```java
  @RequestMapping("请求路径")
  public String 方法名(MultipartFile picture, HttpSession session) throws IOException {
      // 获取上传的文件的文件名
      String originalFilename = picture.getOriginalFilename();
      
      // 获取文件的后缀名
      String suffixName = originalFilename.substring(originalFilename.lastIndexOf("."));
      
      // 获取文件在部署到服务器上时，存放的路径（若路径不存在，则创建）
      ServletContext servletContext = session.getServletContext();
      String realPath = servletContext.getRealPath("文件路径");
      
      // 根据获取到的路径，判断路径是否存在，不存在则创建
      File file = new File(realPath);
      if (!file.exists()) {
          file.mkdir();
      }
      
      // 利用UUID起随机名：上传文件名重复时，读取文件内容后，会覆盖掉原文件的内容
      String randomName = UUID.randomUUID().toString().replaceAll("-", "");
      
      // 要转存到的 最终路径/文件名
      String finalPath = realPath + File.separator + randomName + suffixName;
      
      // 调用MultipartFile中的方法，转存文件
      picture.transferTo(new File(finalPath));
      
     	// 跳转到成功页面
      return "视图名称";
  }
  ```









## xxx









## Thymeleaf语法

`@{}`：在匹配到绝对路径时，会自动补充上下文路径（Application Context）

- 路径按解析方式，可分为：浏览器解析、服务器解析

  - 浏览器解析：页面中的所有超链接，都是由浏览器进行解析

    绝对路径：协议://IP地址:端口号/

    相对路径：当前文件目录

  - 服务器解析：

    绝对路径：协议://IP地址:端口号/上下文路径/

    相对路径：当前文件目录



传递请求参数的两种方式：

1. 直接 `?参数名=值&参数名=值`
2. 通过 `(参数名=值,参数名=值)` （若值为字符串，则要添加 `''` ）



`th:field`：可用在单选、复选框等。将该属性获取到的值，与原属性value的值进行比对，若为true，则会设置当前 单选框/复选框 为选中状态