## Maven

### Maven介绍

Maven是一款 为Java项目 进行 构建管理、依赖管理 的工具

1. 因为Maven是一款工具，因此只要学会其使用即可
2. 只能为Java项目服务
3. 构建管理：
   - 从Java源代码（.java）到运行代码（.class）的过程，则为构建过程
   - 构建过程可用开发工具构建（如 idea、ecplise），也可用Maven进行构建
   - 不同工具构建所要求的项目结构是不同的（如 idea：src web、ecplise：src webcontent、Maven：src webapp）；若使用开发工具来进行项目构建，则当项目进行迁移时，可能会因为项目结构不同而导致失败，因此使用统一的构建工具-Maven来代替开发工具来构建项目（开发工具被用来作为代码提示使用）
   - 使用Maven构建、打包也比较简单
4. 依赖管理：
   - 通过编写Maven的配置文件，即可 自动下载并导入对应的依赖 及 依赖所需的依赖，确保了依赖间的无冲突 和 依赖完整性







### Maven配置

> Maven 3.6.3，支持的最低jdk版本：7
>
> 前提：必须安装配置了Java环境、JAVA_HOME环境变量

1. 下载与安装（绿色免安装）

2. 配置环境变量：MAVEN_HOME（bin目录的上一层）、PATH（`%MAVEN_HOME%\bin`），并使用 `mvn -v` 命令查看是否成功

3. 配置 conf/settings.xml 配置文件（修改默认配置）：

   - 依赖缓存到本地的位置（本地仓库位置）

     ```xml
     <!-- 在settings标签下添加标签 -->
     <localRepository>
     	指定的仓库位置（该目录可不存在，会自动创建）
     </localRepository>
     ```

   - 依赖下载的镜像仓库

     ```xml
     <!-- 在settings-mirrors标签下添加标签 -->
     <mirror>
         <id>nexus-aliyun</id>
         <mirrorOf>central</mirrorOf>
         <name>Nexus aliyun</name>
         <url>http://maven.aliyun.com/nexus/content/groups/public</url>
     </mirror>
     ```

   - 选用编译Java项目所使用的JDK版本

     ```xml
     <!-- 在settings-profiles标签下添加标签 -->
     <profile>
         <id>jdk-17</id>
         <activation>
             <activeByDefault>true</activeByDefault>
             <jdk>17</jdk>
         </activation>
         <properties>
             <maven.compiler.source>17</maven.compiler.source>
             <maven.compiler.target>17</maven.compiler.target>
             <maven.compiler.compilerVersion>17</maven.compiler.compilerVersion>
         </properties>
     </profile>
     ```

4. 在 idea 中指定使用的Maven软件：File --> settings --> Build,Execution,Deployment --> BuildTools --> Maven --> Maven home path（指定Maven软件的存放位置）、Local repository（在读取到settings.xml配置后，会自动变为本地仓库的位置；若不准确，则说明xml文件配置出现问题）







### Maven的GAVP属性

Maven的GAVP属性是指：GroupId、ArtifactId、Version、Packaging，其中GAV是必须在项目创建时指定的，P属性可选（有默认值）。4个属性标识了Maven仓库中的一个项目

- GroupId：如 com.公司.业务线.子业务线，一般为3级，至多为4级
- ArtifactId：如 项目名、产品线名-模块名，一般不重复
- Version：如 1.0.0，一般为 主版本号.次版本号.修订号，其中：
  - 当对产品模块进行了修改（如新增某功能或删除某功能），一般为主版本号修改
  - 当该模块中的某个功能进行了增强或删除（类、接口）时，一般为次版本号修改
  - 当只是对功能的某些bug进行修复时，一般为修订号修改
- Packaging：将项目打包为指定类型的文件：
  - jar（默认值）：普通的Java工程，打包后为.jar结尾的文件
  - war：web工程，打包后为.war结尾的文件
  - pom：不进行打包，用来作为继承的父工程







### 创建Maven工程

1. 创建一个新的空项目

2. 创建一个新的模块

   - Name：项目名，默认也作为ArtifactId
   - Localtion：存放在电脑中的位置
   - BuildSystem：构建所使用的工具，选择Maven
   - AdvancedSettings：存放在本地仓库中的位置，即设置GroupId、AritfactId（Version在创建完Maven工程后生成的pom.xml文件中可设置）

3. 在创建完工程后，应确保使用的Maven不是idea默认使用的那个

4. 创建为一个web工程：

   ① 修改 \<packaging> 标签的属性值为war（默认为jar，该标签被省略不写）

   ② 在src\main 目录下，创建 webapp\WEB-INF\web.xml 目录结构（可在 File --> Project Settings --> Modules 中，找到要添加webapp的项目模块，选择Web进行 webapp的创建 和 web.xml文件路径的修改 即可 ）





### Maven依赖处理

#### 一、引入依赖

在pom.xml文件中，添加 \<dependencies> 标签，在内部可添加 \<dependency> 标签，该标签中声明该依赖的GAV信息，即可引入该依赖

- 依赖的GAV信息获取：

  ① https://mvnrepository.com/

  ② 使用idea中的插件：maven-search



在一般情况下，我们会对GAV中的V（Version）进行统一的管理，以便进行维护，因此我们可以将其声明为变量

- 创建 \<properties> 标签，在该标签中自定义子标签，子标签名即为变量名，子标签的值则为变量值
- 在需要使用到该变量的值时，只需使用 `${变量名}` 即可替换为该变量的值
- 一般情况下，我们对变量名的命名方式为：依赖名.version



除了GAV信息外，还可以补充一个标签信息 \<scope>，可以指定该依赖的作用范围（“锦上添花”操作）

- compile：在main、test中编写代码时、在打包和运行时，该依赖都能被使用
- test：只在test中编写代码时，该依赖才会被使用（如：Junit单元测试）
- runtime：在main、test中编写代码时都不会生效，只有打包和运行时该依赖才会被使用（如：反射使用com.mysql.cj.jdbc.Driver）
- provided：在main、test中编写代码时，依赖能被使用，在打包和运行时该依赖失效（如：HttpServlet，Tomcat服务器已提供，在编写代码时需要用到而已）





#### 二、依赖传递与依赖冲突

1. 依赖传递：在导入依赖时，会自动导入依赖所需要的依赖（compile声明的依赖），可以简化依赖的导入，并确保依赖的版本间无冲突

2. 依赖冲突：

   ① 当依赖传递时出现重复依赖时，会终止依赖传递

   ② 当依赖传递时出现版本冲突时，遵循两个原则进行依赖导入，并 终止 后续依赖传递所需的依赖的导入

   - 第一原则：谁的依赖传递链短，谁所需的依赖版本将被导入
   - 第二原则：谁的依赖传递链先在 \<dependencies> 标签中被先声明，谁所需的依赖版本将被导入





#### 三、依赖导入失败

1. 所连接的Maven仓库待机，无法进行依赖的下载
2. 依赖所下载的版本号错误
3. Maven仓库污染，导致无法正确使用现有的依赖，也无法重新下载依赖（一般是依赖下载一半时出现网络故障导致依赖下载一半，变成了 .lastUpdated 文件，只需进入该依赖所存放的目录，删除所有的以 .lastUpdated 结尾的文件，并重新依赖下载即可）







### Maven项目构建

#### 一、Maven命令

> Maven的命令，需要进入到与pom.xml文件同级的目录内（即项目的根目录）进行

`mvn clean`：清理编译或打包的项目结构，删除target文件夹

`mvn compile`：编译项目，生成target文件夹

`mvn test`：进行源码测试

`mvn site`：生成一个关于项目依赖信息介绍的页面

`mvn package`：打包项目，生成 war/jar 文件

`mvn install`（部署）：打包后上传到Maven本地仓库（本地部署）

`mvn deploy`（部署）：打包后上传到Maven私服仓库（私服部署）

- 部署操作只有是jar包形式才能进行





#### 二、构建命令周期

- 构建命令周期可以简化的触发构建命令

三大周期：

1. 清理：只有 clean 操作
2. 构建：包含 compile、test、package、install / deploy 操作
3. 报告：只有 site 操作

在触发 一个周期内 后面的命令 时，会自动执行该周期中 该命令前的命令





#### 三、导入构建插件

> 若出现所要打包的版本与构建插件不兼容时，需要重新指定构建所使用的插件

在pom.xml文件中，使用 \<build> 标签可指定构建相关的内容

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-war-plugin</artifactId>
            <version>3.2.2</version>
        </plugin>
    </plugins>
</build>
```







### Maven继承与聚合

在创建完Maven工程后，设置 \<packaging> 标签的值为pom，该工程则会作为父工程

在子工程中，会添加 \<parent> 标签，声明父工程的GAV信息，而子工程自身的GV信息，也会继承父工程的信息（也可自定义）

- 在父工程中引入的依赖，子工程都会继承，但可能出现不需要使用的依赖，子工程也进行了继承
- 因此，可以使用 \<dependenciesManagement> 标签来进行依赖的声明，在子工程需要使用时，再进行依赖的导入

```xml
<dependenciesManagement>
    <dependencies>
        <dependency>
            <!-- 依赖的GAV信息声明 -->
        </dependency>
    </dependencies>
</dependenciesManagement>
```

> 当子工程需要应用该依赖时，直接声明依赖所需的GA信息即可，下载的依赖即为父工程中所指定的版本的依赖；当子工程自己声明了版本信息时，则会导入子工程所使用的版本的依赖



在父工程中，会添加 \<modules> 标签，该标签中的子标签 \<module> 中的值为 子工程的ArtifactId

被聚合的子工程，在进行父工程的Maven构建命令时，会自动对聚合的子工程进行构建













## Spring

### IOC

#### 概念介绍

组件：组件就是可以被 复用 的Java对象（组件一定是对象；对象不一定能复用，因此不一定是组件）

Spring就充当了组件的管理角色：

1. 自动创建组件（对象）
2. 存储组件对象
3. 自动进行对象赋值
4. 自动对组件的生命周期进行管理

> 而我们只需配置好Spring的配置文件，告诉它应该怎么装配组件即可



组件交给Spring管理的优势：

1. 降低了组件间的耦合性
2. 提高了代码的可重用性和可维护性
3. 方便配置和管理组件
4. 将组件交由Spring来管理，可以享受Spring框架的其他功能（如：AOP、TX声明式事务）



IOC：控制反转，将本由程序员创建Java对象的权利，转移给IOC容器，由IOC容器帮我们创建对象

DI：依赖注入，通过配置文件告诉IOC容器我们需要某个对象被赋值，IOC容器会自动替我们完成值的注入







#### IOC实现

##### 一、导入依赖

> 在实现IOC功能前，需要先导入基础依赖

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>6.0.6</version>
</dependency>
```





##### 二、编写配置文件

> 编写配置文件，声明哪些组件需要被管理以及DI注入关系

1. 组件可以通过无参构造器直接创建并进行装配

   ```xml
   <bean id="唯一标识" class="类的全类名"/>
   <!-- 
   	1.通过唯一标识可获取到该组件，因此不能重复
   	2.若类提供了无参构造器，则可以直接通过该类的全类名，创建该类的对象，并交由IOC容器进行管理
   -->
   ```

2. 组件只能通过静态工厂的静态方法来获取，并装配到IOC容器中

   ```xml
   <bean id="唯一标识" class="静态工厂类的全类名" factory-method="返回实例的静态方法名"/>
   <!-- 
   	通过该静态工厂类的全类名，定位到该工厂中获取实例的静态方法，再通过调用该静态方法来获取到所需要被IOC容器装配的实例对象
   -->
   ```

3. 组件只能通过非静态工厂的非静态方法来获取，并最后装配到IOC容器中

   ```xml
   <bean id="非静态工厂类的唯一标识" class="非静态工厂的全类名"/>
   <bean id="唯一标识" factory-bean="非静态工厂类的唯一标识" factory-method="返回实例的非静态方法名"/>
   <!-- 
   	1.非静态方法无法直接被调用，需要先获取到非静态工厂类的实例
   	2.获取到非静态工厂类的实例后，通过调用非静态方法来获取到所需要被IOC容器装配的实例对象
   -->
   ```

4. 提供了有参构造器来创建组件，在装配到IOC容器的同时，进行了DI注入

   > 引用和被引用的组件，必须全部处于IOC容器中
   >
   > IOC容器是一个高级容器，内部会会先创建完所有bean对象，再进行属性赋值（DI注入），因此被引用的bean对象，无需先被创建

   ```xml
   <!-- 提供的是单个构造参数的有参构造器 -->
   <bean id="唯一标识" class="类的全类名">
   	<constructor-arg value="值"/>
   </bean>
   
   <!-- 提供的是多个构造参数的有参构造器 -->
   <bean id="唯一标识" class="类的全类名">
   	<constructor-arg name="参数名1" value="值"/>
       <constructor-arg name="参数名2" value="值"/>
       <!-- 
   		1.若按构造参数的顺序为参数赋值，则name属性可以省略 
   		2.可以将name换成 index ，通过下角标（从0开始）的方式按特定顺序为构造参数赋值
   	-->
   </bean>
   
   
   <!--
   	当赋值的构造参数为引用数据类型时，则需要先将被引用的对象存放于IOC容器中，再通过 ref 来进行引用
   	
   -->
   <bean id="被引用类的唯一标识" class="类的全类名"/>
   <bean id="引用类的唯一标识" class="类的全类名">
   	<constructor-arg name="参数名" ref="被引用类的唯一标识"/>
   </bean>
   ```

   - 通过set方法的方式进行DI注入

     ```xml
     <bean id="唯一标识" class="类的全类名">
     	<property name="（属性名）" value="值"/>	
     </bean>
     <!--
     	1.name所指的是setXxx方法 去掉set并首字母小写 的值（也就是调用该set方法）来给指定的属性赋值
     	2.若被赋值的为属性为引用数据类型，则同样要先将被引用的对象存放于IOC容器中，再使用 ref 来引用该bean对象，代替value的使用
     -->
     ```

     



##### 三、指定配置文件

> 实例化IOC容器对象，指定读取的配置文件，根据配置文件所声明的bean和DI关系进行组件的自动创建

IOC容器的创建规则：

- BeanFactory接口中，规定了IOC容器的基本动作
- ApplicationContext接口继承了BeanFactory接口，对IOC容器的动作进行了拓展（如：AOP、Web、国际化等）
- 而IOC容器的创建，是根据配置文件的不同形式，选择对应的方式来创建的：

|               实现类               |                          要求                          |
| :--------------------------------: | :----------------------------------------------------: |
|   ClassPathXmlApplicationContext   |   配置文件是XML格式，且位于类路径（如：resources）下   |
|  FileSystemXmlApplicationContext   | 配置文件是XML格式，位于系统中的某个盘符下（如：D:\xx） |
|       WebApplicationContext        |             Web项目所使用的IOC容器创建方式             |
| AnnotationConfigApplicationContext |               配置文件是Java类（配置类）               |



若XML文件存放于 resources 目录下，则在编译后，会存放在target的classes目录（也就是类路径）下，可以直接通过ClassPathXmlApplicationContext的方式获取该IOC容器对象

1. 直接创建IOC容器并指定配置文件

   ```java
   ApplicationContext 变量名 = new ClassPathXmlApplicationContext("类路径下的配置文件.xml");
   // 多态的形式，可同时指定多个配置文件
   ```

2. 先创建IOC容器，再指定配置文件，再刷新（装配组件并进行DI注入）

   ```java
   ClassPathXmlApplicationContext 变量名 = new ClassPathXmlApplicationContext();
   // 先创建IOC容器（接口中没有设置配置文件的方法，因此不能使用多态）
   
   变量名.setConfigLocations("类路径下的配置文件名.xml");
   // 再进行配置文件的设置
   
   变量名.refresh();
   // 刷新，进行IOC和DI的调用
   
   // 第一种方法本质上也是该方法的简化实现
   ```

   



##### 四、获取组件

> 通过IOC容器对象来获取存放在IOC容器中的组件

```java
ApplicationContext context = new ClassPathXmlApplicationContext("配置文件.xml");
```

1. 直接通过bean对象的id值来获取组件

   ```java
   Object 变量名 = context.getBean("唯一id值");
   // 该方式获取到的组件对象为Object类型，需要强转
   ```

2. 根据 bean对象的id值、bean对象的class 来获取组件

   ```java
   对象类型 变量名 = context.getBean("唯一id值",对象类型.class);
   // 可直接获取到组件对象
   ```

3. 直接通过bean对象的class来获取组件

   ```java
   对象类型 变量名 = context.getBean(对象类型.class);
   /* 
   	1.可直接获取到组件对象
   	2.要求在IOC容器中，仅能有一个该类型的bean
   	3.在配置文件中，要被管理的组件所对应的class，一定是能够被创建的对象（实现类）；但在获取组件时，可以通过接口的类型来获取到实现类的bean对象（前提是该接口的实现类仅有这一个）
   */
   ```

   





#### IOC容器特性

##### 组件初始化与销毁

Servlet有三个周期方法：init、service、destroy

- 当Tomcat创建了Servlet对象时，会自动调用Servlet的init（初始化）方法
- 每次进行业务时，Tomcat就会调用Servlet的service（服务）方法
- 当Tomcat要销毁Servlet对象时，会自动调用Servlet的destroy（销毁）方法



IOC容器作为高级容器，除了可以实例化组件，还可以对组件的周期进行管理（组件的初始化和销毁）

1. 定义初始化和销毁方法

   ```java
   // 定义了一个方法，将作为组件的初始化方法
   public void init(){}
   
   // 定义了一个方法，将作为组件的销毁方法
   public void destroy(){}
   
   /* 
   	定义组件的初始化和销毁方法，要求：
   		1.必须是 public 修饰，必须为 void 返回值，必须是 无参数 的
   		2.方法名可以自定义
   */
   ```

2. 定义组件时，指明组件存在初始化和销毁方法

   ```xml
   <bean id="唯一标识" class="全类名" init-method="初始化方法名" destroy-method="销毁方法名"/>
   <!-- 为组件定义了初始化和销毁方法后，IOC容器就会在对应的时间点调用相应的方法 -->
   ```

- 调用组件的初始化和销毁方法的时间点：

  ```java
  ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("配置文件")
  // 在创建IOC容器时，就会调用组件的初始化方法
      
  context.close()
  /* 
  	1.在关闭IOC容器时，就会调用组件的销毁方法
  	2.只有在正常关闭IOC容器时，IOC容器才会有时间去调用组件的销毁方法；若直接销毁了IOC容器对象的话，是不会调用组件的销毁方法的
  */
  ```

  



##### 组件作用域

IOC容器创建组件时，是根据配置文件进行创建的

而IOC容器时Java代码编写的，因此，配置文件中的组件信息（如id、class、init-method等）就会被封装到一个类中（BeanDefinition），根据类的信息来创建组件对象

在BeanDefinition类中，是根据属性 scope 来决定创建的组件对象的个数的。scope的值可分为两个：

|                   |                   含义                    |     创建对象的时机      |
| :---------------: | :---------------------------------------: | :---------------------: |
| singleton（默认） | 该组件在IOC容器中始终仅有一个实例（单例） |    在IOC容器初始化时    |
|     prototype     |  该组件在IOC容器中可以有多个实例（多例）  | 在获取组件（getBean）时 |

> 在WebApplicationContext环境下，会有另外两个作用域：
>
> |         |         含义         | 创建对象的时机 |
> | :-----: | :------------------: | :------------: |
> | request | 请求范围内有效的实例 |   每次请求时   |
> | session | 会话范围内有效的实例 |   每次会话时   |

使用方式：

```xml
<bean id="唯一标识" class="全类名" scope="作用域值"/>
```





##### FactoryBean简化

组件被IOC容器管理创建时，有两种方式：

- 通过构造器创建
- 通过工厂模式创建

而通过工厂模式创建时，需要我们指定工厂类，以及用来创建组件的工厂方法

FactoryBean是一个被定义好的标准接口（工厂类），当我们将该接口的实现类添加到配置文件时，就会自动执行相应的方法（getObject），从而获取到方法所返回的组件

1. FactoryBean实现类的创建

   ```java
   class 实现类 implements FactoryBean<组件类型>{
       @Override
       public 组件类型 getObject() throws Exception {
           return new 组件类型();
           // 当配置文件中装配了该实现类时，会自动调用该方法，并将该方法的返回值作为组件
       }
   
       @Override
       public Class<?> getObjectType() {
           return 组件类型.class;
           // 返回组件的class
       }
       
       @Override
       public boolean isSingleton() {
           return FactoryBean.super.isSingleton();
           // 有默认值，默认创建的组件即为单例模式，因此该方法可不重写
       }
   }
   ```

2. 定义配置文件

   ```xml
   <bean id="唯一标识" class="实现类全类名"/>
   ```

3. 获取组件

   ```java
   ApplicationContext context = new ClassPathXmlApplicationContext("配置文件名.xml");
   组件类型 bean = context.getBean("唯一标识",组件类型.class);
   // 需要注意的是：除了 调用方法返回的组件会被装配到IOC容器 外，FactoryBean工厂的实现类自身也会被装配到IOC容器中，其唯一标识为：在定义组件的唯一标识前+&（即：&唯一标识）
   ```

   

FactoryBean一般被第三方整合时使用：提供一个实现类继承FactoryBean接口，当我们装配该实现类时，就会自动获取到想要让我们获取到的组件（即内部进行了封装）









#### 案例

##### 一、装配JdbcTemplate组件

1. 导入依赖

   ```xml
   <!-- IOC容器 -->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>6.0.6</version>
   </dependency>
   
   <!-- Mysql驱动 -->
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>8.0.28</version>
   </dependency>
   
   <!-- Druid连接池 -->
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>druid</artifactId>
       <version>1.2.8</version>
   </dependency>
   
   <!-- JdbcTemplate -->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-jdbc</artifactId>
       <version>6.0.6</version>
   </dependency>
   ```

2. 装配JdbcTemplate组件

   JdbcTemplate提供了无参构造器来创建对象：

   ```java
   JdbcTemplate template = new JdbcTemplate();
   ```

   但在创建了JdbcTemplate的对象后，需要指定其绑定的数据源：

   ```java
   template.setDataSource(数据源对象);
   ```

   数据源对象可以借助Druid来实现：

   ```java
   DruidDataSource dataSource = new DruidDataSource();
   dataSource.setUrl("url");
   dataSource.setDriverClassName("driver");
   dataSource.setUsername("username");
   dataSource.setPassword("password");
   ```

   将其交由IOC容器来进行组件管理：

   ```xml
   <context:property-placeholder location="classpath:类路径下配置文件.properties"/>
   <!-- 导入外部properties文件（若导入多个文件，可使用 , 隔开） -->
   
   <bean id="数据源唯一标识" class="com.alibaba.druid.pool.DruidDataSource">
       <property name="url" value="${读取properties文件中的url}"/>
       <property name="driverClassName" value="${读取properties文件中的driver}"/>
       <property name="username" value="${读取properties文件中的username}"/>
       <property name="password" value="${读取properties文件中的password}"/>
   </bean>
   <!-- 创建数据源对象（JdbcTemplate所需） -->
   
   <bean id="jdbcTemplate唯一标识" class="org.springframework.jdbc.core.JdbcTemplate">
       <property name="dataSource" ref="数据源唯一标识"/>
   </bean>
   <!-- 配置JdbcTemplate组件 -->
   ```





##### 二、JdbcTemplate的使用

- 进行数据的增删改操作

  ```java
  int rows = JdbcTemplate对象.update(String sql,Object...args);
  /*
  	sql：预编译的sql语句
  	args：填充占位符
  	返回影响的行数
  */
  ```

- 进行单条数据的查询操作

  ```java
  数据载体类型 变量名=JdbcTemplate对象.queryForObject(String sql,new RowMapper<数据载体类型>(){
      @Override
      public 数据载体类型 mapRow(ResultSet rs, int rowNum) throws SQLException {
          // rs是结果集，rowNum是查询的条目数
          // 将结果集取出来，存放到数据载体中，最后返回
          return 数据载体对象;
      }
  }, Object...args);
  /*
  	sql：预编译的sql语句
  	args：填充占位符
  	RowMapper：处理结果集与数据载体间的映射关系（若映射关系相互对应（结果集的列名与数据载体类中属性名互相一致），则可以直接使用其实现类BeanPropertyRowMapper来快速进行数据装配，使用方法如下案例）
  */
  ```

- 进行多条数据的查询操作

  ```java
  List<数据载体类型> 变量名 = JdbcTemplate对象.query(String sql, new BeanPropertyRowMapper<>(数据载体类型.class),Object...args);
  /*
  	sql：预编译的SQL语句
  	args：填充占位符
  	BeanPropertyRowMapper：RowMapper的实现类，若结果集中列的名与数据载体类的属性名一致，则可以直接将结果集中的数据存放到对应的属性中
  */
  ```

  





#### 注解方式实现IOC

注解方式实现IOC容器管理，分为两步骤：

1. 标记哪些组件要被IOC容器所管理

   用于标记的注解共有4个

   - @Component：用于表示一个普通的bean组件
   - @Controller：用于表示控制层的组件
   - @Service：用于表示业务层的组件
   - @Repository：用于表示数据访问层（DAO）的组件

   其注解的所有功能相同，只是代表含义不同

   - 默认情况下，被标记的组件的默认id为当前类的首字母小写，可以通过注解的value属性值修改默认的id值

     ```java
     @Component(value="唯一id值")
     // 当属性值仅有value一个时，value= 可以省略
     ```

2. 开启注解扫描指定路径，将该路径下所有被标记的组件交由IOC容器管理

   ```xml
   <context:component-scan base-package="包路径"/>
   <!-- 指定包路径下的所有被标记了注解的组件，会被IOC容器所管理 -->
   ```

   - 可以指定扫描的包路径下，不扫描被哪些注解标记的类

     ```xml
     <context:component-scan base-package="包路径">
     	<context:exclude-filter type="annotation" expression="不扫描的注解的全类名"/>
     </context:component-scan>
     ```

   - 也可以指定不扫描包路径下所有的注解，而只扫描被哪些注解标记的类

     ```xml
     <context:component-scan base-package="包路径" use-default-filters="false">
     	<context:include-filter type="annotation" expression="要扫描的注解的全类名"/>
     </context:component-scan>
     ```

     

   

