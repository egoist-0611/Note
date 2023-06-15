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









## XXX















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