## VSCode整合前端

### 环境搭建

安装四个常用插件：

- Chinese Language：简体中文
- Live Server：服务器（类似Tomcat）
- Vetur、vue-helper：开发vue工具



创建工作区：

1. 文件 --> 打开文件夹（打开文件要保存的目录）
2. 文件 --> 将工作区另存为（保存到打开的文件夹内）
3. 创建页面
4. 访问页面时，默认访问 5500 端口







### ES6

> ECMAScript6.0，是JavaScript语言的下一代标准，目的是使JavaScript可以用来编写复杂的大型应用程序

模板字符串：使用 ` `` ` 在字符串中插入变量值或表达式（通过 `${变量名}` 获取变量的值）

```javascript
var name = "Tom"
var age = 19
var info = `${name} age is ${age}`
console.log(info)
// 控制台输出：Tom age is 19
```



对象拓展运算符：相当于复制对象的值（通过 `{...对象}` 复制对象的值）

```javascript
var person1 = {name:"Tom",age:19}
var person2 = {...person1}
console.log(person2)
// 控制台输出：与person1值相同
```



箭头函数：简化函数的定义

```javascript
// 原来格式
var res1 = function(a){
    console.log("test1")
    return a
}
console.log(res1("test1"))

// 箭头函数
var res2 = (a) => {
    console.log("test2")
    return a
}
console.log(res2("test2"))
```

- 当函数参数仅有一个时，`()` 可以省略
- 当函数方法体 仅有一句 且 有 `return` 返回结果时，`{}` 、`return` 都可以省略







### VUE

> 需要引入 `vue.js` 文件

基本使用：

```html
<h1 id="唯一标识">
    {{变量名}}
</h1>
<!-- 标签与vue绑定后，通过 {{变量名}} 获取变量的值 -->

<script>
	new Vue({
        el:'#唯一标识',			// 绑定标签
        data:{				// 定义变量
            "变量名":"值"
        },
        methods:{			// 定义方法
          	方法名(){}  
        },
        created(){			// 生命周期方法：在页面渲染之前执行
            debugger		// 设置断点
        },
        mounted(){}			// 生命周期方法：在页面渲染之后执行
    })
</script>
<!-- 若要绑定的标签声明在vue之后，那么需要让vue在页面加载后才执行（windon.onload=function(){}） -->
```



使用vue发送axios请求：

> 需要先引入 `axios.js` 文件

```javascript
new Vue({
    el:"#唯一标识",			// 绑定标签
    created(){			// 生命周期方法：在渲染页面前执行
        this.方法名()
    },
    methods:{			// 定义方法
        方法名(){
            axios.get("请求路径")			// 异步发送get请求
                .then(变量名 => {				// 请求成功执行该方法
                	// 请求成功后，会将内容保存到指定的变量中（内容包括请求头、请求体等）
                	console.info(变量名.data)		// 通过.data，可获取到请求体信息
            	})
                .catch()					// 请求失败执行该方法
        }
    }
})
```









### Node.js

> node.js是用来运行JavaScript代码的（像在HTML中运行JavaScript，需要通过 \<script> 标签来引用）

1. 官网下载 `https://nodejs.org/en/`
2. 傻瓜式安装后，通过 `node -v` 查看版本确认是否成功安装



创建后缀名为 `js` 的文件，该文件中的代码会被当成JavaScript代码执行

- 通过 `node 文件路径.js` 运行该文件



npm：是node.js的包管理工具（相当于Maven），默认在安装了node.js后，就默认集成了npm（通过 `npm -v` 查看版本号）

- `npm init -y`：在当前文件夹下快速生成 `package.json` 文件（该文件相当于Maven的pom.xml文件。-y 表示使用默认值）

- `npm config list`：查看npm的配置信息

- `npm config set registry https://registry.npm.taobao.org`：配置后，以后npm下载依赖时，都从国内的淘宝处下载

- `npm install 依赖名@版本号`：（下载）引入指定依赖（若不指定版本号，则默认使用最新版本）

  `npm install`：根据 `package.json` 文件，引入文件中声明的依赖

  > 依赖会存放到 当前文件夹下的 `node_modules` 文件夹中









### 模块化开发

> 所谓的模块化开发，指的是调用其他node.js文件中的方法

方式一：

````javascript
// 在js文件1中的公共方法
export function 方法1(){}		// 声明export后，该方法才能被其他文件调用
export function 方法2(){}

// js文件2调用js文件1中的方法
import {方法1,方法2} from "文件路径"		// 通过方法名，从其他文件中导入方法
方法1()		// 调用方法
方法2()

// 通过 node 文件2.js 执行方法
````



方式二：

```javascript
// 在js文件1中公共方法
export default{
    方法1(){},
    方法2(){}
}

// js文件2调用js文件1中的方法
import 变量名 from "文件路径"
变量名.方法1()			// 相当于通过对象来调用方法
变量名.方法2()
```



注意：ES6的模块化（功能），无法在Node.js中执行，因此，需要利用Babel转码器，将ES6转为ES5后再运行

- 安装 babel-cli 工具：`npm install --global babel-cli`（安装后通过 `babel --version` 确认安装情况）

- 转码流程：

  1. 在项目的根目录（或要转码的文件所在目录），添加一个 `.babelrc` 配置文件，在文件内添加转码规则：

     ```json
     {
         "presets": ["es2015"],
         "plugins": []
     }
     ```

  2. 引入转码器：`npm install --save-dev babel-preset-es2015`

  3. 进行转码：`babel 文件1.js 文件2.js -d 文件夹`：将指定的js文件转码，并存入到指定的文件夹中

     - 默认转码后的文件名，与转码前的文件名相同
     - 可选择转码 指定目录及其子目录下的所有的js文件（如：`./`，当前目录下所有的js文件都进行转码）











### 前端模版

#### vue-admin-template

> vue-admin-template，是vue-element-admin（后台管理系统集成方案）的基础版（精简版）

##### 部署

1. 进入到 package.json 文件所在目录，通过 `npm install` 命令引入模板所需要的依赖
2. 通过命令 `npm run dev` 启动服务







##### 常用功能

###### 请求地址

修改发送请求的服务器地址：进入 `vue.config.js` 文件，注销掉 `before: require('./mock/mock-server.js')`，配置使用代理来转发请求：

```javascript
proxy: {
    '/dev-api': { 								// 匹配并修改所有以 '/dev-api'开头的请求路径
        target: 'http://ip地址:端口号',			// 目标服务器地址（若上下文路径固定，可在此表示）
        changeOrigin: true, 					// 跨域支持			
        pathRewrite: { 						// 重写上下文路径（去掉路径中开头的'/dev-api'）
            '^/dev-api': ''
        }
    }
}
```





###### 状态码

修改响应成功的状态码：进入 `src/utils/request.js` 文件，修改 `response => {}` 箭头函数中判断code的值

```javascript
if (res.code !== 20000)			// 这里就是当响应码不是20000时，就会认为请求失败
```





###### 自定义请求

`src/api/` 目录下，统一存放请求的接口，在前端页面，我们会通过引入该js文件，再调用内部定义的方法，来实现请求调用：

```javascript
import request from '@/utils/request'           // 间接引入axios

export default{
    // 调用该方法后，会得到一个request，在后续通过.then()可以发送异步请求
    方法名(形参){	// handler处理请求时所需要的参数（在调用该方法时传入参数，在发送请求时携带参数）
        return request({
            url: '/请求路径',
            method: '请求方式（get/post）',
            params: '请求参数'
            // 若需要的是普通的请求参数，则使用 params 传参
            // 若是需要用JSON传递的请求参数，则使用 data 传参
        })
    }
}
```





###### 发送请求

在 `src/views/` 目录下，我们可以自定义vue页面，在页面中发送请求获取数据并进行渲染：

```vue
<!-- 以上是HTML代码 -->
<script>
    import api from '@/api/导入自定义请求接口所在的js文件'
    export default{
        data(){
            return {
                // 在此处定义的变量，可动态渲染（如：通过this.变量名为该变量赋值）
            }
        },
        methods:{			// 定义方法
            方法名(){
                // 确认框
                this.$confirm('此操作将永久删除该记录, 是否继续?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                }).then(()=>{           // 当确认时，会执行该then()；当取消时，会执行catch()
                    return api.接口方法名(形参).then(request=>{ // 调用API，发送异步请求
                        var datas = request.data		// 获取到响应体
                    })    
                }).then(response=>{   // 在确认框中，若是上一个then()返回时，那么就会执行该then()
                    this.$message.success('弹出一条成功的提示信息')
                })
            }
        },
        created(){			// 页面渲染之前执行：调用异步请求方法获取数据
            this.方法名()
        }
    }
</script>
```





###### 自定义路由

路由的定义在 `src/router/index.js` 文件中

```javascript
// 自定义路由
  {
    path: '/基本路径',
    component: Layout,      // 主渲染页面
    meta: {
      title: '标题',
      icon: 'el-icon-s-tools'       // 图标
    },
    alwaysShow: true,       // 当存在子路由时，是否嵌套（可折叠）
    children: [{          // 定义子路由
      path: '具体路径',		// 路由到此的路径：/基础路径 + / + 具体路径
      component: () => import('@/views/自定义的vue页面'),		// 渲染该页面
      meta: { 
        title: '标题', 
        icon: 'el-icon-s-help' 		// 图标
      },
      hidden: true			// 隐藏路由（不显示）
    }]
  }
```













## SpringSecurity应用实例

### 基本介绍

> 案例背景：SpringBoot 2.3.6.RELEASE + JDK8

导入依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!-- 旧版本的SpringSecurity需要依赖Web环境 -->
```



认证流程：（查看 UsernamePasswordAuthenticationFilter）

1. 在用户提交登录请求时，会将请求数据封装到 Authentication 中（实现类：UsernamePasswordAuthenticationToken）
2. 调用 authenticate() 进行认证（ProviderManager.class）
   - 在该方法中，又调用了 provider.authenticate()（AbstractUserDetailsAuthenticationProvider.class）委托认证
   - 在委托认证过程中，调用了 this.retrieveUser()，从中调用了 loadUserByUsername()，去数据库中了查询用户信息，并返回 UserDetails
   - 通过 PasswordEncoder，对比 UserDetails 中的密码，与 Authentication 提交的密码是否一致，若是一致，则返回 Authentication 对象
3. 最后，通过 SecurityContextHolder.getContext()、setAuthentication() ，保存该 Authentication 对象到上下文中







### 流程介绍

自定义用户认证大体流程：

1. 自定义拦截器1：我们设置该拦截器先执行，并且设置当拦截到的资源是登录请求时，就走该拦截器进行认证。在该拦截器中，我们要获取发送过来的用户信息，与数据库中的数据进行身份和权限的认证。在认证成功时，生成token响应给客户端，并将权限信息进行缓存（如Redis）（也可在认证失败时做出对应响应）

   - 身份和权限的认证，是通过 UserDetailsService 中的 loadUserByUsername() 返回的 UserDetails 来进行的。因此，通过自定义 UserDetailsService，在 loadUserByUsername() 中查询数据库中指定用户数据，并在查询不到数据时抛出异常；在查询到数据时返回一个含有该用户信息的 UserDetails，与请求表单中的用户信息进行校验

     > 表单提交的密码，是采用 BCryptPasswordEncoder 进行加密的，要确保与 UserDetails 中的密码，两者的加密方式相同

2. 自定义拦截器2：当我们访问除登录外的其他资源时，走该拦截器进行认证。在该拦截器中，要获取客户端发送的token进行认证。认证成功时，从缓存中获取该用户所拥有的权限，再放行该请求

3. 创建一个配置类，开启身份和权限的认证；定义拦截路径规则，将我们的两个拦截器进行应用；替换底层使用的 UserDetailsService 认证过程（还可替换 对表单密码的加密方式）

4. 设置每个 handler 被调用时所需要的权限







### 实现过程

#### 自定义拦截器1

```java
public class login拦截器 extends UsernamePasswordAuthenticationFilter {
   
    // 构造器方法
    public LoginAuthenticationFilter(AuthenticationManager authenticationManager,
                                     StringRedisTemplate redisTemplate) {
// 该构造器关键就是获取并设置这个AuthenticationManager（在设置该拦截器时会传递进来），从而可以调用内部的方法进行认证
        this.setAuthenticationManager(manager);
        
        // 获取传入的RedisTemplate
        this.redisTemplate = redisTemplate;
        
        // 设置该过滤器拦截的登录的接口，以及指定拦截的是哪种请求方式
        this.setRequiresAuthenticationRequestMatcher
                (new AntPathRequestMatcher("/接口handler地址", "POST"));
        
        // 是否只拦截POST请求
        this.setPostOnly(false);
    }

    
    // 该方法用来获取表单信息，并调用委托认证
    @Override
    public Authentication attemptAuthentication(HttpServletRequest request,
                                                HttpServletResponse response)
            throws AuthenticationException {
        
        try {
            // 读取输入流，从而获取到表单提交的信息
            LoginVo vo = new ObjectMapper()
                    .readValue(request.getInputStream(), 封装表单数据的实体类对象.class);
            
           // 将表单提交的信息封装为一个Authentication，用于与底层返回的UserDetails进行数据对比校验
            Authentication authenticationToken =
                    new UsernamePasswordAuthenticationToken
                            (vo.getUsername(), vo.getPassword());
            
            // 调用方法进行委托认证
            return this.getAuthenticationManager().authenticate(authenticationToken);
            
        } catch (Exception e) {
            throw new RuntimeException("读取表单提交信息失败：" + e.getMessage());
        }
    }
    
    
/*
	认证所需要的UserDetails，是在我们自定义的MyUserDetailsService中获取的
	而MyUserDetails，是我们自定义的实现了UserDetails接口的实现类
*/

    
    // 认证成功时执行该方法，响应成功状态码，并响应token信息
    @Override
    protected void successfulAuthentication(HttpServletRequest request,
                                            HttpServletResponse response,
                                            FilterChain chain,
                                            Authentication authResult)
            throws IOException, ServletException {
        
        // 获取到认证时用的UserDetails
        MyUserDetails myUserDetails = (MyUserDetails) authResult.getPrincipal();
        
        /* 获取在UserDetails内部保存的用户对象，从而生成token（建议封装进响应信息中）*/
        
        // 将信息响应给前端
        response.setStatus(HttpStatus.OK.value());
        response.setContentType(MediaType.APPLICATION_JSON_UTF8_VALUE);
        new ObjectMapper().writeValue(response.getWriter(), "响应token数据");
        
        // 将用户的权限（List集合，转成JSON字符串后）缓存到Redis中
        redisTemplate.opsForValue().
                set(user.getUsername(), 
                        JSON.toJSONString(myUserDetails.getAuthorities()));
    }

    
    // 认证失败时执行该方法，响应失败信息
    @Override
    protected void unsuccessfulAuthentication(HttpServletRequest request,
                                              HttpServletResponse response,
                                              AuthenticationException failed)
            throws IOException, ServletException {
        
        // 获取失败的原因（再响应出去）
        String failMsg = failed.getCause().getMessage();
    }
}
```

- 自定义的MyUserDetailsService：

  ```java
  public class MyUserDetailsService extends UserDetailsService {
      
      // 在该方法内进行数据库查询，返回的UserDetails将作为 与表单提交的用户信息 进行比较的数据
      @Override
      public UserDetails loadUserByUsername(String username) 
              throws UsernameNotFoundException {
          
          /* 根据用户名查询数据库用户信息，查询不到的话抛出异常 */
          
  // 返回一个UserDetails（MyUserDetails继承于User，而User又是UserDetails的实现类）。其中，返回的用户对象用于 与表单提交的用户信息做对比认证，而用户权限集合在后面获取后，可用来决定用户是否有权限操作（用户权限：new SimpleGrantedAuthority("权限名")，是GrantedAuthority的实现类）
          return new MyUserDetails(数据库用户对象, 用户权限集合);
      }
  }
  ```

  - 自定义的MyUserDetails：

    ```java
    public class MyUserDetails extends User {
        
        public MyUserDetails(数据库用户对象,
                             Collection<? extends GrantedAuthority> authorities) {
            super(user.getUsername(), user.getPassword(), authorities);
    /* 要将数据库用户对象保存起来，之后可通过获取到该MyUserDetails，从而来获取该数据库用户对象，进而取得用户的一些信息（用于生成token） */
        }
    }
    ```







#### 自定义拦截器2

```java
public class token拦截器 extends OncePerRequestFilter {

    // 拦截认证是否有token
    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
            throws ServletException, IOException {
        
        // 判断请求头中是否存在token
        String token = request.getHeader("token");
        
        if (!StringUtils.isEmpty(token)) {
            
            /* 获取token中的用户信息 */
            
            // 根据用户名从Redis中获取保存的用户权限
            String jsonData = redisTemplate.opsForValue().get("用户名");
            // 将JSON字符串转换为List集合，每个元素以Map格式存储
            List<Map> authsMap = JSON.parseArray(jsonData, Map.class);
            // 重新定义一个存储权限的GrantedAuthority集合，将从Redis中读取到的权限存放进去
            List<GrantedAuthority> auths = new ArrayList<>();
            for (Map map : authsMap) {
                String auth = (String) map.get("authority");
                auths.add(new SimpleGrantedAuthority(auth));
            }
            
// 生成Authentication认证信息，并在上下文中保存信息，之后，当该信息中的用户再次进行资源访问时，将不再进行认证，且在调用handler时，若有权限要求，则会进行权限的校验
            SecurityContextHolder.getContext()
                    .setAuthentication
                            (new UsernamePasswordAuthenticationToken
                                    ("用户名", "密码", auth));
            
            // 认证成功，放行请求
            filterChain.doFilter(request, response);
            return;
        }
        
        /*  没有token时，认证失败 */
    }
}
```







#### 配置类

```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private MyUserDetailsService userDetailsService;

    // 调用该方法后，可获取到一个AuthenticationManager（login拦截器所需）
    @Override
    protected AuthenticationManager authenticationManager() throws Exception {
        return super.authenticationManager();
    }

    // 添加一个密码加密器
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
    
    // 定义哪些请求不需要进行拦截
    @Override
    public void configure(WebSecurity web) throws Exception {
        web.ignoring().antMatchers("/favicon.ico",
                "/swagger-resources/**", "/webjars/**", "/v2/**",
                "/swagger-ui.html/**", "/doc.html", "/v3/**");
    }
    
    // 配置自定义的loadUserByUsername认证
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService);
    }

    // 配置拦截认证规则
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http    // 关闭csrf跨站请求伪造
                .csrf().disable()
                // 允许跨域
                .cors()
                .and()
                // 所有请求都需要经过认证
                .authorizeRequests().anyRequest().authenticated()
                .and()
// 设置登录认证时使用的filter（该filter的创建，需要传入一个AuthenticationManager，通过调用重写的方法获取）
                .addFilter(new login拦截器(authenticationManager()))
// 设置其他认证（token认证）时使用的filter，并指定该filter是在UsernamePasswordAuthenticationFilter（登录过滤）之后才进行拦截的
                .addFilterAfter(token拦截器对象, 
                        UsernamePasswordAuthenticationFilter.class);

        // 禁用session
        http.sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);
    }
}
```











## Activiti7

工作流，就是业务上一个完整的审批流程（如员工请假）；而工作流引擎，是指通过计算机对业务流程的自动化执行管理（Activiti就是一个工作流引擎）

工作流引擎的一个规则：当前节点处理后，会始终自动读取下一个节点。工作流引擎会用一张表来记录当前节点，当该节点被处理后，会从表中删除，再自动读取下一个节点后存储到表中，依此类推，从而实现流程的自动推进。



### 依赖引入与配置

1. 导入依赖：

   ```xml
   <dependency>
       <groupId>org.activiti</groupId>
       <artifactId>activiti-spring-boot-starter</artifactId>
       <version>7.1.0.M6</version>
       <exclusions>
           <exclusion>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
           </exclusion>
       </exclusions>
   </dependency>
   <!-- 注意：activiti的运行是依赖数据库的 -->
   ```

   - Activiti 默认会集成 Spring Security，因为要求设置审批人时必须是系统用户操作

2. 修改配置：

   ```yaml
   spring:
     activiti:
       database-schema-update: true			# 需要的表不存在时，自动创建
       db-history-used: true					# 使用历史表
       history-level: full						# 历史表保存哪些信息
       check-process-definitions: true			# 校验流程文件
   ```

> 以上配置完成后，运行时，就会在数据库中创建需要的表（25张）





### 工具创建流程图

部署工具：

1. 解压 activiti.zip，获取 activiti-explorer.war，存放到 Tomcat 的 webapps 目录下（Tomcat8，JDK8）
2. 运行Tomcat（bin/startup.bat），访问：localhost:8080/activiti-explorer，默认登录账号密码：kermit

创建流程图：

1. 流程 --> 流程设计工作区 --> 新建模型 --> （名称） --> Activiti Modeler --> 创建

2. 创建一个请假流程：（将事件拖拽到工作区）

   ① Start Events（开始事件：表示一个流程的开始）：选择 Start event

   ② Activities（活动：表示一个任务）：选择 User task，指定该任务的处理用户Assignments（name可设置当前任务的名称）

   ③ End Events（结束事件：流程的结束）：选择 End event

3. 点击工作区空白处 --> Process identifier 设置流程key

4. 保存并退出工作区

5. 处理模型 --> 导出模型（bpmn20.xml），右键流程图片 --> 保存图片

6. 在 resources 目录下创建一个 process 文件夹，用来存放 bpmn20.xml 和流程图片





### 流程定义的部署

部署流程定义有两种方式：单个文件部署、压缩包部署（将 bpmn20.xml和图片 打包成 zip 后再部署）

1. 单个文件部署：

   ```java
   // 1.通过DI注入的方式获取到RepositoryService对象
   @Autowired
   RepositoryService repositoryService;
   
   // 2.部署流程
   Deployment deploy = repositoryService.createDeployment()
                   .addClasspathResource("process/流程文件.bpmn20.xml")
                   .addClasspathResource("process/流程图.png")
                   .name("自定义流程名")
                   .deploy();
   
   /*
   	确保类路径下的png文件能被扫描并加载
   	部署后，会获取到一个Deployment信息对象，可调用其方法：
   		① getId()：获取 部署后生成的流程id
   		② getName()：获取自定义的流程名
   */
   ```

2. ZIP文件部署：

   ```java
   // 1.通过DI注入的方式获取到RepositoryService对象
   @Autowired
   RepositoryService repositoryService;
   
   // 2.获取ZIP文件的输入流
   InputStream is = this.getClass().getClassLoader().getResourceAsStream("文件路径.zip");
   ZipInputStream zipIs = new ZipInputStream(is);
   
   // 3.部署流程
   repositoryService.createDeployment().addZipInputStream(zipIs).deploy();
   ```

部署流程时，会操作activiti中的3张表：

- act_ge_bytearray：流程资源表
- act_re_deployment：流程定义部署表，在每次进行流程操作时都会增加一条记录
- act_re_procdef：流程定义表，部署每个新的流程时，会在表中增加一条记录





### 创建和启动流程实例

```java
@Autowired				// 通过DI注入RuntimeService操作对象
RuntimeService runtimeService;

ProcessInstance processInstance = runtimeService.startProcessInstanceByKey("流程key");
/* 
	根据流程key，创建该流程的实例（根据流程图创建该流程的实例）
	返回一个ProcessInstance信息对象，可调用其方法：
		① getProcessDefinitionId()：获取部署的流程id
		② getId()：获取创建的流程实例id
		③ getActivityId()：获取当前活动id
*/
```

创建流程实例后会操作的数据表：

- act_hi_actinst：流程实例创建的执行记录（历史记录）

- act_hi_identitylink：执行了流程任务的用户（历史记录）

- act_hi_procinst：流程实例的创建历史（历史记录）

- act_hi_taskinst：流程任务（历史记录）

- act_ru_execution：流程执行信息

- act_ru_identitylink：流程实例信息

  > PROC_INST_ID：创建的流程实例ID

- act_ru_task：任务信息





### 用户任务查询与处理

```java
@Autowired			// 通过DI注入TaskService操作对象
TaskService taskService;

List<Task> list = taskService.createTaskQuery()
                .taskAssignee("任务处理用户Assignments")	// 或者通过processInstanceId来查任务
                .list();		// 返回所有；若查询一条，可使用 singleResult()
/*
	查询指定的处理任务用户 所有未处理的任务，返回一个集合，元素类型为Task，可调用其方法：
		① getProcessInstanceId()：获取创建的流程实例的id
		② getId()：任务id
		③ getAssignee()：任务处理人
		④ getName()：任务名称
*/

for (Task task : list) {
    taskService.complete(task.getId());			// 根据任务id，处理任务
}
// 任务处理完成，流程会自动转入下一任务
```







### 其他

> 流程定义的部署，相当于创建了一个Java类；而流程实例的创建，就是相当于该Java类创建了一个对象

#### 流程定义删除

```java
@Autowired			// 注入操作对象
RepositoryService repositoryService;

repositoryService.deleteDeployment("流程定义部署后的ID",true);
// 删除流程定义，true表示该流程定义下的所有流程实例也会跟着删除（默认有流程实例的流程定义无法被删除）
```





#### 流程实例的挂起与激活

> 例如在进行系统维护时，我们需要将流程进行暂停，禁止用户进行流程处理

方式一：通过流程定义，将全部的流程实例进行挂起或激活

```java
@Autowired			// 注入操作对象
RepositoryService repositoryService;

// 根据流程key获取单个流程定义对象
ProcessDefinition definition = repositoryService.createProcessDefinitionQuery()
                .processDefinitionKey("流程key")
                .singleResult();

// 通过调用 isSuspended() 判断当前流程定义的状态（true表示暂停，可以激活；false表示活跃，可以挂起）
if (definition.isSuspended()) {
    repositoryService.activateProcessDefinitionById(definition.getId(), true, null);
    // 通过流程定义部署时的id，激活流程下的所有实例（true表示确认激活，null为时间点）
} else {
    repositoryService.suspendProcessDefinitionById(definition.getId(), true, null);
    // 通过流程定义部署时的id，挂起流程下的所有实例（true表示确认挂起，null为时间点）
}
```

方式二：通过流程实例对象，激活或挂起该流程实例

```java
@Autowired			// 注入操作对象
RuntimeService runtimeService;

// 根据流程实例的id，获取流程实例对象
ProcessInstance processInstance = runtimeService.createProcessInstanceQuery()
    .processInstanceId("流程实例id")
    .singleResult();

// 通过调用 isSuspended() 判断当前流程实例的状态（true表示暂停，可以激活；false表示活跃，可以挂起）
if (processInstance.isSuspended()) {
    runtimeService.activateProcessInstanceById("流程实例id");
    // 通过流程实例的id，激活该流程实例
} else {
    runtimeService.suspendProcessInstanceById(processId);
    // 通过流程实例的id，挂起该流程实例
}
```





#### 用户历史任务查询

```java
@Autowired			// 注入操作对象
HistoryService historyService;

List<HistoricTaskInstance> list = historyService.createHistoricTaskInstanceQuery()
                .taskAssignee("任务处理用户Assignments")
                .list();

/*
	查询指定的处理任务用户 所有已处理的任务，返回一个集合，元素类型为Task，可调用其方法：
		① getProcessInstanceId()：获取创建的流程实例的id
		② getId()：任务id
		③ getAssignee()：任务处理人
		④ getName()：任务名称
*/
```





#### 任务分配

> 指定流程中处理任务的负责人

方式一：固定分配。在创建流程图时，通过Assignments属性指定固定的任务处理人

方式二：表达式分配。在指定Assignments时，值为变量或某个组件的方法

- Assignments值为变量：`${变量名}`（流程变量）

  ```java
  @Autowired				// 注入操作对象
  RuntimeService runtimeService;
  
  Map<String, Object> map = new HashMap<>();
  map.put("变量名1", 负责人1);
  map.put("变量名2", 负责人2);
  runtimeService.startProcessInstanceByKey("流程key", map);
  // 在创建流程实例时传入一个map，指定好每个任务的处理人（key为变量名，value为具体的处理人）
  /*
  	① 也可以在创建流程实例时，只先指定负责人1，当进行负责人1的任务处理时，再指定负责人2：
  		taskService.complete("任务ID",Map<String,Object>)
  	② 还可以通过流程实例ID指定负责人：
  		runtimeService.setVariables("流程实例ID",Map<String,Object>)
  */
  ```

- Assignments值为某个组件的方法：`${组件.方法名(值)}`

  ```java
  // 在容器中添加组件。比如：
  @Component
  public class User {
      public String setMaskUser(int type) {
          if (type == 1) {
              return "zhangsan";
          }
          return "lisi";
      }
  }
  // 在创建流程实例时，就会根据Assignments的值，调用组件中的方法，得到返回的信息，该信息就将作为任务的负责人
  ```

方式三：监听器分配

```java
// 不设置Assignments属性，而是通过 Task listeners 属性，设置一个监听器的全类名，并让该监听器在创建流程实例后自动触发执行。而在该监听器中，要定义好任务处理人的指定规则
/*
	指定监听器的执行时期：
		Create：任务创建后触发
		Assignment：任务分配后触发
		Delete：任务完成后触发
		All：所有事件发生时都触发
*/
public class 监听器名 implements TaskListener {
    @Override
    public void notify(DelegateTask delegateTask) {
        // 通过判断在流程图中所设置的name任务名称，来确定该任务的处理人
        if (delegateTask.getName().equals("任务名称")) {
            delegateTask.setAssignee("负责人1");
        } else {
            delegateTask.setAssignee("负责人2");
        }
    }
}
```





#### 任务组

> 当任务处理人存在问题而无法进行任务处理时，就可以通过添加候选人的方式，让候选人获取任务进行处理
>
> 在创建流程图时，通过Assignments属性中的 Candidate users 值来指定候选人名单

```java
@Autowired				// 注入操作对象
RepositoryService repositoryService;
@Autowired
RuntimeService runtimeService;
@Autowired
TaskService taskService;

// 部署流程定义
repositoryService.createDeployment()
                .addClasspathResource("process/流程图文件名.bpmn20.xml")
                .name("自定义流程名")
                .deploy();
// 创建和启动流程实例
runtimeService.startProcessInstanceByKey("流程key");

// 查询当前流程任务中是否存在指定候选人
Task task = taskService.createTaskQuery().taskCandidateUser("候选人").singleResult();
// 如果存在该候选人，则将该任务交由指定候选人处理（注意：当任务被候选人1提取后，则候选人2就没有机会了）
if(task != null){
    taskService.claim(task.getId(),"候选人");
}

// 候选人提取任务后，就成为任务的处理人Assignments，通过查询任务处理人，查看待办任务，进行任务执行
List<Task> tasks = taskService.createTaskQuery().taskAssignee("负责人").list();
for (Task t : tasks) {
    taskService.complete(t.getId());
}
```





#### 网关

> 网关相当于判断，决定流程接下来的走向

排他网关：只有一种流程会被选择（通常与流程变量使用）

- 流程图中设置：Gateways --> Exclusive gateway，拖到工作区后，可以指向下一个子流程，选择指向的线，在 Flow condition 中设置流程变量（设置条件，若满足该条件，则流程将走向该指向的子流程）
- 比如：请假小于3天，交由部门处理，再交由人事处理；请假大于3天，交由经理处理，再交由人事处理

并行网关：两边流程都被处理后，才会执行子流程

- 流程图中设置：Gateways --> Parallel gateway，拖到工作区后，可以指向下一个子流程，最终让所有子流程指向另一个并行网关（当子流程都被处理后，流程才会继续）
- 比如：请假经过了部门和经理的处理后，才会交由人事处理





#### 结束流程实例

> 通过 让流程实例的任务完成后 直接走到结束事件节点 来实现

```java
@Autowired				// 注入操作对象
TaskService taskService;

// 获取指定流程的BpmnModel对象
BpmnModel bpmnModel = repositoryService.getBpmnModel("流程定义id");
// 获取该流程的所有结束事件节点
List<EndEvent> endEventList = bpmnModel.getMainProcess().findFlowElementsOfType(EndEvent.class);
// 若是并行流程任务的话，则可能会没有结束事件节点
if (!CollectionUtils.isEmpty(endEventList)) {
    // 获取该流程的结束事件节点
    FlowNode endFlowNode = endEventList.get(0);
    // 获取当前流程的任务节点
    FlowNode currentFlowNode = (FlowNode) bpmnModel.getMainProcess().getFlowElement("流程key");
    // 删除当前任务节点后续的走向
    currentFlowNode.getOutgoingFlows().clear();
    // 创建新的走向
    SequenceFlow newSequenceFlow = new SequenceFlow();
    newSequenceFlow.setId("newSequenceFlowId");     // 设置走向的id
    newSequenceFlow.setSourceFlowElement(currentFlowNode);      // 设置走向的起始节点
    newSequenceFlow.setTargetFlowElement(endFlowNode);      // 设置走向的最终节点
    // 设置当前流程任务节点之后的新走向
    List<SequenceFlow> list = new ArrayList<>();
    list.add(newSequenceFlow);
    currentFlowNode.setOutgoingFlows(list);
    // 完成任务后，流程将走向结束事件节点，从而结束流程
    taskService.complete("流程实例任务的id");
}
```











## 整合微信

### 推送公众号菜单选项

1. 申请测试公众号：https://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=sandbox/login

2. 引入依赖：weixin-java-mp，该依赖是导入一个工具，可以让我们更方便的推送菜单

   ````xml
   <dependency>
       <groupId>com.github.binarywang</groupId>
       <artifactId>weixin-java-mp</artifactId>
       <version>4.1.0</version>
   </dependency>
   ````

3. 注入两个组件：

   ```java
   @Bean
   public WxMpConfigStorage wxMpConfigStorage() {
       WxMpDefaultConfigImpl wxMpConfigStorage = new WxMpDefaultConfigImpl();
       wxMpConfigStorage.setAppId("appID值");
       wxMpConfigStorage.setSecret("appsecret值");
       return wxMpConfigStorage;
   }
   
   @Bean
   public WxMpService wxMpService() {
       WxMpService wxMpService = new WxMpServiceImpl();
       wxMpService.setWxMpConfigStorage(wxMpConfigStorage());
       return wxMpService;
   }
   ```

4. 编写推送接口（查看接口文档决定返回的JSON格式数据：https://developers.weixin.qq.com/doc/offiaccount/Custom_Menus/Creating_Custom-Defined_Menu.html）

   ````java
   // 创建一个JSONObject，通过put()，可像Map一样添加数据；通过toJSONString()可转换为JSON数据
   JSONObject jsonData = new JSONObject();
   
   // 创建一个JSONArray，通过add()，可添加多个 封装了要响应的菜单数据的JSONObject
   JSONArray menuJson = new JSONArray();
   
   // 注入之前添加的组件WxMpService，通过调用其方法推送菜单
   wxMpService.getMenuService().menuCreate("满足格式的json数据");
   
   /* 最终返回一个button（数组），内部的子元素有这些属性：name、type
   	根据type为【key】还是【view】，决定是否还需要url属性
   	根据是否有子层菜单，决定是否需要sub_button属性
   */
   ````









### 推送消息

1. 新增测试模板 --> 模板内容（举例）：`{{first.DATA}} 审批编号：{{keyword1.DATA}} 提交时间：{{keyword2.DATA}} {{content.DATA}}`

   添加后，会生成模板ID

2. Java代码实现：

   ```java
   WxMpTemplateMessage templateMessage = WxMpTemplateMessage.builder()
                       .toUser("要推送的用户的openId")
                       .templateId("消息模板的id") 
                       .url("点击消息时跳转的地址")
                       .build();
   
   templateMessage.addData(new WxMpTemplateData("first", "内容"));
   templateMessage.addData(new WxMpTemplateData("keyword1", "内容");
   templateMessage.addData(new WxMpTemplateData("keyword2", "内容");
   templateMessage.addData(new WxMpTemplateData("content", "内容"));
          
   // 利用之前推送公众号菜单时添加的两个Bean，来推送消息
   wxMpService.getTemplateMsgService().sendTemplateMsg(templateMessage);
   ```







