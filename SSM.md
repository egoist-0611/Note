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
   		3.多例情况下，不会调用销毁方法
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

> 注解方式实现IOC，依旧需要借助XML文件（如：第三方bean的管理、包扫描与外部文件引用，都需要借助XML文件来配置）

##### 组件管理

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

     




##### 周期方法与作用域

```java
@Scope(scopeName=ConfigurableBeanFactory.SCOPE_PROTOTYPE)	// 作用域：多例
@Component
class 组件名{
    @PostConstruct		// 周期方法：初始化
    public void 方法名(){}
    
    @PreDestroy			// 周期方法：销毁
    public void 方法名(){}
}
/*
	1.单例为默认作用域，格式为：@Scope(scopeName=ConfigurableBeanFactory.SCOPE_SINGLETON)
	2.周期方法依旧要求：public、void、无形参，方法名可随意
	3.多例下，即使绑定了销毁方法，也不会调用
*/
```





##### 自动装配

> 前提：组件全部都由IOC容器进行管理

###### 引用数据类型

使用@Autowired注解实现对引用数据类型的自动装配，可作用于：

1. 属性上

   ```java
   @Autowired
   private 组件类型 属性名
   ```

2. 构造方法上

   ```java
   @Autowired
   public 组件名(组件类型 属性名){}
   // 对构造方法中的形参进行自动装配
   ```

3. set方法上

   ```java
   @Autowired
   public void setXxx(组件类型 属性名){}
   // 对set方法的形参进行自动装配
   ```

自动装配的流程：在IOC容器中查找符合类型的组件对象 --> 将值设置给属性

- 设置@Autowired注解后，默认情况下要求该属性必须被装配，可通过设置required属性值修改默认要求

  ```java
  @Autowired(required=false)		// 默认情况下，required为true
  // 修改为false后，该属性可装配可不装配，若没有符合的装配，则使用默认值（null）
  ```

- 设置@Autowired注解后

  - 先根据要装配的属性的类型进行自动装配

  - 当相同类型的组件存在多个时（如：多态情况下的接口类型，存在多个实现类），则会将属性名作为id值进行查找

    - 通过注解 @Qualifier 来设置通过指定的id来查找，而非通过属性名

      ```java
      @Autowired
      @Qualifier(value="唯一标识id")			// 注解@Qualifier要与注解@Autowired一起使用
      ```

    - 也可以通过注解 @Resource 来设置

      ```java
      @Resource(name="唯一标识id")		// 相当于 @Autowired + @Qualifier 的作用
      /*
      	需要导入依赖：
              <dependency>
                  <groupId>jakarta.annotation</groupId>
                  <artifactId>jakarta.annotation-api</artifactId>
                  <version>2.1.1</version>
              </dependency>
      */
      ```





###### 基本数据类型

为基本数据类型自动赋值的方式有两种：

1. 直接赋值，如：`private int a = 10;`

2. 使用注解 @Value

   ```java
   @Value("值")
   private 数据类型 变量名;
   ```

   > 若是直接为基本属性赋值的话，使用注解@Value是多此一举的。但是，@Value注解在引用了外部配置文件后，可以读取到外部配置文件的数据
   >
   > ```xml
   > <context:component-scan base-package="包路径"/>
   > <!-- 扫描包路径下的组件 -->
   > 
   > <context:property-placeholder location="classpath:配置文件.properties"/>
   > <!-- 引用外部配置文件 -->
   > 
   > @Value("${ key名 : value默认值 }")
   > private 基本数据类型 变量名;
   > <!-- 引用外部配置文件，使用${}获取key所对应的值；当不存在该key时，则使用 value默认值 -->
   > ```







#### 配置类方式实现IOC

##### 组件管理

> 配置类方式实现IOC，无非是在注解方式实现IOC的基础上，将需要用到XML文件的情况，替换为用配置类的方式实现

###### 一、声明配置类

使用@Configuration注解标识该类为配置类：

```java
@Configuration
class 配置类名{}
```





###### 二、替换XML文件

将需要用到XML文件的功能，用相应的注解代替：

1.  包扫描

   ```java
   @Configuration
   @ComponentScan({"包路径1","包路径2"})		// 包扫描（可指定多个包路径）
   class 配置类名{}
   ```

2. 外部文件引入

   ```java
   @Configuration
   @PropertySource(value={"classpath:配置文件1.properties","classpath:配置文件2.properties"})		// 引用外部配置文件（可引用多个配置文件）
   class 配置类名{}
   ```

3. 第三方组件（如：DruidDataSource、JdbcTemplate）

   ```java
   @Configuration
   @PropertySource("classpath:配置文件.properties")		// 引用外部配置文件
   class 配置类名{
       @Value("${配置文件key名}")
       private String username;
   
       @Bean			// 装配第三方组件
       public DruidDataSource dataSource() {
           DruidDataSource dataSource = new DruidDataSource();
           dataSource.setUsername(username);
           return dataSource;
       }
   }
   ```

   - 将方法返回值作为IOC容器的组件的要求：被注解 @Bean 标识

   - 默认情况下，方法名即为组件的唯一标识id，可使用属性 name / value 来重新指定id名

     ```java
     @Configuration
     class 配置类名{
         @Bean(value="唯一id")
         public 组件类型 方法名(){
             return 组件类型对象;
         }
     }
     ```

   - 引用其他组件（如：JdbcTemplate 引用 DruidDataSource）：

     - 若其他组件也是@Bean标识，则可以直接调用该方法（本质也是从IOC容器中获取组件）

       ```java
       @Configuration
       class 配置类名{
           @Bean
           public DruidDataSource dataSource(){
               return new DruidDataSource();
           }
       	
           @Bean
           public JdbcTemplate jdbcTemplate(){
               JdbcTemplate jdbcTemplate = new JdbcTemplate();
               jdbcTemplate.setDataSource(dataSource());		// 直接调用方法
               return jdbcTemplate;
           }
       }
       ```

     - 直接在形参列表处声明需要的组件，IOC容器会自动注入

       ```java
       @Configuration
       class 配置类名{
           @Bean
           public JdbcTemplate jdbcTemplate(DataSource dataSource){ // 直接声明需要的组件
               JdbcTemplate jdbcTemplate = new JdbcTemplate();
               jdbcTemplate.setDataSource(dataSource;		
               return jdbcTemplate;
           }
       }
       ```

       首先根据组件的类型进行自动装配，若没有类型匹配的，则报错

       若存在多个相同类型的组件，则会将形参名作为 唯一标识id 进行匹配





###### 三、创建IOC容器对象

创建IOC容器对象，获取组件：

```java
ApplicationContext context = new AnnotationConfigApplicationContext(配置类.class);
// 直接创建（可使用多态）

AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
context.register(配置类.class);
context.refresh();
// 先创建IOC容器，再加载配置类，最后再刷新（开启IOC和DI管理）
```







##### 整合配置类

使用@Import注解，可以将其他的配置类，整合到一个配置类中，这样在创建IOC容器对象时，只需加载一个配置类即可：

```java
@Import(value={ 配置类1.class , 配置类2.class })
@Configuration
class 配置类名{}
```









#### 整合测试环境

使用@SpringJUnitConfig注解，可以快速帮我们创建IOC容器，并通过自动装配的方式，可以快速获取组件对象

1. 导入相关依赖（junit5测试）：

   ```xml
   <dependency>
       <groupId>org.junit.jupiter</groupId>
       <artifactId>junit-jupiter-api</artifactId>
       <version>5.3.1</version>
   </dependency>
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-test</artifactId>
       <version>6.0.6</version>
   </dependency>
   ```

2. 使用@SpringJUnitConfig标记测试类

   ```java
   @SpringJUnitConfig(locations="配置文件.xml",value=配置类.class)
   // 配置文件使用locatios加载，配置类使用value加载
   class 测试类{
       
       @AutoWired		// 使用组件时，会自动注入，无需getBean获取
       private 组件类型 组件名;
       
       @Test
       public void test(){}
   }
   ```



在Junit5中，提供了两个注解：@BeforeEach、@AfterEach，可以指定：在测试方法执行前 / 执行完成后，执行的代码

```java
@BeforeEach / @AfterEach
public void 方法名(){}
```









### AOP

#### 代理模式

假设我们有个计算器功能：

```java
public class Calculate {
    // 加法
    public int add(int a, int b) {
        // 在此输出形参列表
        int res = a + b;
        // 在此输出运算结果
        return res;
    }
    
	// 减法
    public int minus(int a, int b) {
        return a - b;
    }
}
```

我们想要对其功能进行完善：在调用方法后、返回结果前，先进行日志的输出

若是直接在源代码中添加该功能，就会导致核心代码冗余，而且会多出很多重复的代码，后期更是不便于维护

因此，考虑不修改核心代码，而是将日志信息提取出来，在需要的地方再进行 插入

- 我们可以执行代码时，是通过调用代理，让代理先执行输出部分日志信息，在需要时再去调用核心代码，接着继续完成日志输出，就可以实现以上功能



##### 静态代理

需要我们手动创建代理类，在代理类中调用被指定好的、某种功能的核心代码

```java
public class StaticProxy {
    // 目标对象（静态代理指定了该代理类将只代理Calculate这个类）
    private Calculate target;
    public StaticProxy(Calculate target) {		// 构造方法，来为目标对象赋值
        this.target = target;
    }

    // 代理加法功能
    public int add(int a, int b) {
        // 日志信息
        System.out.println("a=" + a + ",b=" + b);
        int res = target.add(a, b);
        // 日志信息
        System.out.println("result=" + res);
        return res;
    }

    // 代理减法功能
    public int minus(int a, int b) {
        System.out.println("a=" + a + ",b=" + b);
        int res = target.minus(a, b);
        System.out.println("result=" + res);
        return res;
    }
}
```





##### 动态代理

动态代理分为两种：

1. JDK动态代理

   - Java原生

   - 要求目标类必须有接口（根据目标类实现的接口，生成一个对应的实现该接口的代理类）

     > 因此，目标类与代理类实现了相同的接口，生成的代理类只能使用接口类型（多态）来接收

2. Cglib动态代理

   - 第三方，后被整合到Spring中

   - 不要求目标类必须实现接口，而是直接根据目标类，生成一个子类对象作为代理类

     > 因此，目标类与代理类属于继承关系，生成的代理类可以使用目标类类型来接收



基于JDK代理技术实现：

```java
// 代理类实现的接口
public interface Calculate {
    int add(int a, int b);
    int minus(int a, int b);
}

// 代理类
public class CalculateImpl implements Calculate {
	// 加法功能
    public int add(int a, int b) {
        return a + b;
    }
    // 减法功能
    public int minus(int a, int b) {
        return a - b;
    }
}
```

```java
// 代理工厂：随机产生代理类来代理目标对象
public class ProxyFactory {
    private Object target;		// 目标对象
    public ProxyFactory(Object target) {		// 构造方法，来为目标对象赋值
        this.target = target;
    }

    // 获取代理类的方法
    public Object getProxy() {
        ClassLoader classLoader = target.getClass().getClassLoader();	// 获取类加载器
        Class<?>[] interfaces = target.getClass().getInterfaces();		// 获取目标类的接口
        InvocationHandler invocationHandler = new InvocationHandler() {	// 设置执行的内容
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println("参数为：" + Arrays.toString(args));
                Object res = method.invoke(target, args);
                System.out.println("执行结果为：" + res);
                return res;
            }
        };
        
        /* 核心方法：用来创建代理类，其内要求三个参数：
        	1.classLoader：类加载器（根据目标对象来获取类加载器即可）
        	2.interfaces：目标类实现的接口（JDK动态代理要求代理的目标类要有实现的接口）
        	3.invocationHandler：接口，设置：当代理对象调用目标方法时执行的内容（核心功能、日志信息）
        		该方法内仅有一个需要被重写的抽象方法：invoke(proxy,method,args)
        			proxy：随机生成的代理对象
        			method：调用的目标方法的Method对象
        			args：调用的目标方法的形参列表
        		当每次通过代理对象去调用目标方法时，就会执行该重写后的抽象方法
        		而在该抽象方法内，根据method对象，利用反射（传入目标对象、形参列表）即可调用目标方法
        */
        return Proxy.newProxyInstance(classLoader, interfaces, invocationHandler);
    }
}
```

```java
// 调用，实现日志信息的插入
Calculate calculate = new CalculateImpl();		// 创建目标对象
ProxyFactory proxyFactory = new ProxyFactory(calculate);	// 传入目标对象，创建代理工厂
Calculate proxy = (Calculate) proxyFactory.getProxy();		// 随机获取代理对象（JDK动态代理）
proxy.add(10, 20);			// 调用代理对象（代理对象内部会去调用核心功能）
```







#### 概念

OOP：面向对象编程，是纵向编程，比如：在继承父类的方法时，只能完全使用父类的方法，或者完全重写父类的方法，无法做到对方法内部的局部修改

AOP：面向切面编程，是横向编程，可以将代码中重复的某部分代码先抽取到一个公共模块中，再将这部分代码插入到所需要的位置



横切关注点：业务处理的主要流程是核心关注点，与之关系不大的部分则为横切关注点（即：非核心代码的部分）



通知（增强）：每个横切关注点被提取为一个方法，则该方法被称为通知方法。通知方法按照 在目标方法执行的前后 来分类，可分为：

- 前置通知：在目标方法执行前进行增强
- 返回通知：目标方法正常执行后进行增强
- 异常通知：目标方法执行发生异常时进行增强
- 后置通知：目标方法最终执行结束后（无论正常结束还是异常）进行增强
- 环绕通知：使用 try-catch-finally 包裹整个目标方法（包括了以上4种增强）



连接点：所有可以被增强的方法

切入点：被选中进行增强的连接点

切面：切入点和增强相结合



目标：被代理的对象

代理：向目标对象应用通知之后所创建的代理对象



织入：将通知应用到目标上，生成代理对象的过程







#### 注解实现AOP

##### 基本实现

1. 导入依赖

   > AOP功能实现的依赖，有spring-context（IOC、DI所需）、spring-aop（在导入spring-context时已通过依赖传递导入）、spring-aspects、aspectj（在导入spring-aspects时已通过依赖传递导入）

   ```xml
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>6.0.6</version>
   </dependency>
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-aspects</artifactId>
       <version>6.0.6</version>
   </dependency>
   ```

2. 正常编写核心业务组件，并交由IOC容器进行管理

3. 编写IOC的配置文件或配置类

4. 定义一个增强类，在增强类中定义增强方法（根据要增强的功能在插入的目标方法的不同位置，决定增强方法的个数，如：前置增强、后置增强等）

   - 前置通知（@Before）、返回通知（@AfterReturning）、异常通知（@AfterThrowing）、后置通知（@After）、环绕通知（@Around）

   - 获取目标方法中的信息（方法名、形参等）

     - 若想要获取目标方法的信息，只需要在增强方法的参数位置处，添加一个 JoinPoint 类型的形参即可

       ```java
       @通知类型("execution(切入点表达式)")
       public void 方法名(JoinPoint 形参名){
       /*
       	【Object】JoinPoint对象.getTarget()：获取目标类对象
       	【Signature】JoinPoint对象.getSignature()：获取目标方法的信息
           	【int】Signature对象.getModifiers()：获取目标方法的返回值类型
           	【String】Signature对象.getName()：获取目标方法的方法名
           【Object[]】JoinPoint对象.getArgs()：获取目标方法的形参列表
       */
       }
       ```

     - 若想要获取目标方法的执行结果信息，则需要在返回通知（正常执行结束）中，设置一个Object类型的形参，并在@AfterReturning注解中，使用 returning属性 绑定该形参名即可获取到

       ```java
       @AfterReturning(value="execution(切入点表达式)",returning="对应形参名")
       public void 方法名(Object 对应形参名){}
       ```

     - 若想要获取目标方法执行过程中，出现的异常信息，则需要在异常通知（出现异常）中，设置一个Throwable类型的形参，并在@AfterThrowing注解中，使用 throwing属性 绑定该形参名即可获取到

       ```java
       @AfterThrowing(value="execution(切入点表达式)",throwing="对应形参名")
       public void 方法名(Throwable 对应形参名){}
       ```

5. 对增强方法进行配置（定义切入点，格式：`(value="execution(① ② ③.④.⑤(⑥))")`）

   - ① 访问修饰符，如`public`

   - ② 方法的返回值参数类型，如`void`

     > 当：任意访问修饰符、任意返回值参数类型，①、② 可整合为一个 *

   - ③ 包路径

     > 具体路径：如`com.atguigu.controller.impl`
     >
     > 单层模糊：如`com.*.controller.impl`
     >
     > 多层模糊：如`com..impl`（不能以`..`开头，可以为`*..`）

   - ④ 类的名称

     > 具体名称：`Calculator`
     >
     > 模糊名称：`*`
     >
     > 部分模糊：`Cal*`、`*or`

   - ⑤ 方法名（同类的名称）

   - (⑥) 形参列表

     > 没有参数：`()`
     >
     > 有具体参数：如`(int)`、`(String,int)`
     >
     > 模糊参数：`(..)`（可以没有或多个参数）
     >
     > 部分模糊：`(String..)`（第一个参数为String）、`(..int)`、`(String..int)`

6. 将增强类加入IOC容器，并定义该类具有切面

   ```java
   @Component		// 标记该类被IOC容器管理
   @Aspect			// 标记该类具有切面（具有切入点，且作用了增强方法）
   class 增强类{}
   ```

7. 开启对aspect注解的支持的功能

   ```java
   // 在xml文件中配置：
   <aop:aspectj-autoproxy />
   
   // 在配置类中配置：
   @Configuration		// 定义该类为配置类
   @EnableAspectJAutoProxy			// 开启对aspect注解的使用
   class 配置类{}
   ```

8. 最后，在创建IOC容器对象，获取代理对象时：

   - 若进行增强的目标类有实现接口，则底层使用的是JDK动态代理（实现接口），只能通过接口类型在IOC容器中获取代理对象并存放

   - 若进行增强的目标类没有实现接口，则底层使用的是cgLib动态代理（子类继承），可以通过目标类的类型来获取代理对象并存放

     > 可以说，若是使用了AOP，则最终在IOC容器中，存储的是代理对象，而非目标对象





##### 其他要点

###### 切点表达式的复用

定义一个方法，在方法上方使用 @Pointcut 注解定义要复用的切入点表达式，在需要使用的地方引用该方法即可

```java
@Pointcut("execution(切入点表达式)")
public void 方法名(){}

@通知类型("方法名()")
public void 通知方法名(){}
```

> 建议将切入点表达式方法重新定义在一个类中，这样后期便于维护（只需在引用方法时，使用方法所在的类的全类名，即可正确引用）





###### 环绕通知

> 环绕通知的要点：有个 ProceedingJoinPoint 类型的参数，是 JoinPoint 的子类，可以执行目标方法。相当于要我们自己去执行目标方法，并将目标方法使用 try-catch-finally 包裹起来，在对应的位置实现4种通知类型

```java
@Around("execution(切入点表达式)")
public Object 方法名(ProceedingJoinPoint 形参名) {
    Object[] args = 形参名.getArgs();		// 调用父类的方法：获取目标方法的形参列表
    Object res;			// 接收目标方法执行后返回的结果
    try {
        // 此处相当于前置通知
        res = joinPoint.proceed(args);		// 执行目标方法（可能需要传入参数）
        // 此处相当于返回通知
    } catch (Throwable e) {
		// 此处相当于异常通知
        throw new RuntimeException(e);	// 需要手动抛出异常，否则目标方法出现异常时不会结束执行
    } finally {
        // 此处相当于后置通知
    }
    return res;			// 最终需要将目标方法执行后的结果给返回
}
```





###### 指定优先级

使用 @Order 注解来指定多个切面（通知方法）间执行的优先级：值越小，优先级越高，前置越先执行，后置越慢执行

```java
@Component		// 让该增强类被IOC容器管理
@Aspect			// 标记该类具有切面
@Order(整型值)		// 指定优先级
class 增强类{}
```







#### XML实现AOP

> 定义目标类以及增强类，并存入到IOC容器中

```xml
<aop:config>
    <!-- 定义切入点表达式方法 -->
	<aop:pointcut id="切入点表达式方法名" expression="execution(切入点表达式)"/>
    
    <!-- 指定切面（具有切点并进行了增强），并指定该切面（通知方法）的优先级 -->
    <aop:aspect ref="增强类id" order="整型值">
        <aop:before method="前置通知方法名" pointcut-ref="切入点表达式方法id"/>
        <aop:after-returning method="返回通知方法名" pointcut="execution(切入点表达式)" returning="对应的形参名"/>
    </aop:aspect>
</aop:config>
```









#### 声明式事务

##### 概念

声明式事务是指用注解或XML配置的方式来控制事务的提交和回滚

由于编写事务的代码是重复且单一的，因此可以考虑将这些非核心业务代码，通过AOP进行抽取

但是，spring为我们提供了一种更简单的方式来实现声明式事务，而不需要使用AOP来实现



spring为我们提供了各种事务操作的通知方法，而不需要我们自己写

但由于各类数据库进行事务的操作不同，又考虑到框架要具备兼容性，所以并不是直接将各种数据库不同的事务调用直接写入增强方法中，而是定义了一个接口，在接口中调用了统一的事务操作方法（该接口被称为事务管理器）

该接口有不同的实现类，这些实现类就代表了不同的数据库对事务的操作，只要配置了实现类，在调用事务方法时，就会去执行不同实现类所对应的数据库的事务操作方法



因此，我们的操作很简单：

1. 选择合适的事务管理器的实现类，加入到IOC容器中
2. 指定哪些地方需要增强（添加事务）



继承关系：

- TransactionManager：spring5.2之后出现的接口，作为事务管理器（存放于 spring-tx 包下）

- PlatformTransactionManager：spring5.2之后作为TransactionManager的子接口，真正提供了事务的操作方法

- DataSourceTransactionManager：事务管理器接口的实现类，主要是对MyBatis、JDBC、JdbcTemplate的操作（存放于 spring-jdbc 包下）

  HibernateTransactionManager：事务管理器接口的实现类，主要是对Hibernate的操作（存放于 spring-orm 包下）







##### 实现

> 使用配置类的方式实现

1. 将合适的事务管理器的实现类添加到IOC容器中

   ```java
   // jdbcTemplate、mybatis、jdbc等使用的都是DataSourceTransactionManager
   @Bean
   public TransactionManager transactionManager(DataSource dataSource) {
       DataSourceTransactionManager manager = new DataSourceTransactionManager();
       manager.setDataSource(dataSource);
       // DataSourceTransactionManager内部是基于连接池进行事务的操作的，因此需要DataSource对象
       return manager;
   }
   ```

2. 在需要的地方使用 @Transactional 注解添加事务

   ```java
   @Transactional	  // 添加在方法上时，表示当前方法有事务；添加在类上时，表示该类下所有的方法都有事务
   public 返回值类型 方法名(){}
   ```

3. 在配置类上开启对事务的注解功能的支持

   ```java
   @Configuration		// 表明该类是一个配置类
   @ComponentScan("包路径")		// 开启注解扫描
   @EnableTransactionManagement		// 开启事务注解的支持
   class 配置类{}
   ```







##### @Transactional属性

1. readOnly：只读，默认值为false，设置为 true 后，事务管理的内容只能查询，不能修改

   ```java
   @Transactional(readOnly=true)
   public 返回值类型 方法名(){}
   ```

   - 添加只读功能，可以提高查询效率
   - 查询操作是不会用到事务。但是，一般情况下，我们会在类上进行@Transactional注解的标识，就会导致原本只是用来查询的方法，可能能够被修改
   - 方法上的@Transactional注解，会 覆盖 掉类上标识的@Transactional注解

2. timeout：超时时间，默认值为-1（永不超时），可指定等待的秒数；若超时，则回滚事务，并释放资源

   ```java
   @Transactional(timeout=3)			// 等待3s
   public 返回值类型 方法名(){}
   ```
   
3. rollbackFor：控制哪些异常会进行回滚（默认情况下，只会回滚RuntimeException）

   noRollbackFor：控制哪些异常不进行回滚（默认情况下为null）

   ```java
   @Transactional(rollbackFor=异常类型.class , noRollbackFor=异常类型.class)
   public 返回值类型 方法名(){}
   ```

   - 异常体系：Throwable --> Error / Exception --> RuntimeException、IOException

4. isolation：设置事务的隔离级别（默认为 Isolation.DEFAULT）

   ```java
   @Transactional(isolation = Isolation.READ_COMMITTED)
   ```

   - 读未提交：事务可以读取已经提交的数据，容易产生脏读、不可重复读、幻读
   - 读已提交：事务只能读取已经提交的数据，可以避免脏读，可能产生不可重复读、幻读（oracle默认，推荐设置）
   - 可重复读：在一个事务中，相同的查询将返回相同的结果集，不管其他事务对数据进行了什么修改。可以避免脏读和不可重复读，可能出现幻读（mysql默认）
   - 串行化：完全禁止并发，只允许一个事务执行完毕后才能执行另一个事务，可以避免所有问题，但效率低

5. propagation：设置事务的传播行为（在父方法中调用了其他方法时对事务的处理）

   - Propagation.REQUIRED：父方法中有事务，则会加入到父方法的事务中（最终都会是在同一个事务中，是默认值，推荐使用）
   - Propagation.REQUIRES_NEW：每个方法都独立自己的事务，不论父方法中是否有事务

   ```java
   @Transactional(propagation = Propagation.REQUIRED)
   public 返回值类型 方法1() {
       // 正确代码
   }
   
   @Transactional(propagation = Propagation.REQUIRED)
   public 返回值类型 方法2() {
       // 异常代码
   }
   
   @Transactional
   public 返回值类型 父方法() {
       方法1();
       方法2();
   }
   /*
   	由父方法调用了方法1和方法2
           方法1成功执行，事务提交
           方法2出现异常，事务回滚
           定义Propagation.REQUIRED的事务传播行为后，会将当前事务归并入父方法的事务中
           因此整体事务回滚，即：方法1事务成功，但也进行回滚
   */
   ```













## MyBatis

> 基本实现步骤：
>
> 1. 导入依赖
> 2. 创建Mapper接口及其映射文件
> 3. 创建核心配置文件，载入Mapper映射文件
> 4. 加载核心配置文件，执行CRUD操作指令



### 一、依赖导入

```xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.7</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.28</version>
</dependency>
<!-- mybatis底层使用jdbc驱动实现，不需要再导入连接池，mybatis自带 -->
```









### 二、Mapper接口和映射文件

#### 基本实现

> 要先准备好数据载体类

原先方式：xxxDao接口（规定方法）、xxxDaoImpl实现类（具体实现）

mybatis结构：xxxMapper接口（规定方法）、xxxMapper.xml文件（对应mapper接口的具体SQL实现）



mybatis有两种方式实现对数据库的操作：ibatis1.x / 2.x、mybatis3.x

1. ibatis方式：不要求写接口

   ```xml
   <mapper namespace="字符串1">
   	<select id="字符串2" resultType="数据载体全类名">
       	查找类型的sql语句
       </select>
   </mapper>
   <!-- 
   	ibatis方式并不要求我们写接口
   	namespace也不需要指定接口全类名（任意字符串即可）
   	书写SQL语句的crud标签中的id也不需要指定为接口的方法名（任意字符串即可）
   -->
   ```

2. mybatis方式：要求有接口

   ```xml
   <mapper namespace="mapper接口全类名">
   	<select id="接口中的方法名" resultType="数据载体全类名">
       	查找类型的sql语句
       </select>
   </mapper>
   <!-- 接口中不能有重载方法（CRUD标签的id是根据方法名进行定位的） -->
   ```








#### 详细配置

##### 传入参数与赋值

```xml
<mapper namespace="接口全类名">
    <select id="接口方法名" resultType="数据载体类型">
        SELECT * FROM student WHERE id = 值
    </select>
</mapper>
```

在值位置处，可以使用两种方式进行赋值：

1. `#{key}`：使用填充占位符的方式进行赋值（即：先通过 where id = ? ，再进行占位符填充）
2. `${key}`：使用字符串拼接的方式进行赋值（即："where id = " + id，通过将id替换为参数值）

在一般情况下，推荐使用 #{key} 的方式进行参数的传递，可以有效防止SQL注入问题。但是，当我们需要进行如列名、表名等的动态传入时，#{key} 赋值的方式无法实现，就可以使用 ${key} 来实现



key是在调用接口方法时传入的参数，不同情况所对应的key的值不同：

1. 当传入的是单个值，如 int、Double 等基本或引用类型时，key的值可以是任意值

   ```java
   public abstract 数据载体类型 接口方法名(Integer id);
   
   <select id="接口方法名" resultType="数据载体全类名">
   	SELECT * FROM 表名 WHERE id = #{任意值}
   </select>
   ```

2. 当传入的是数据载体类对象时，key的值要求为实体类中与之对应的属性名

   ```java
   public abstract int 接口方法名(Employee employee);
   
   <insert id="接口方法名">
   	INSERT INTO employee(emp_name,emp_age) VALUES(#{empName},#{empAge})
   </insert>
   ```

3. 当传入的是Map集合时，key的值要求为Map集合中与之对应的key

   ```java
   public abstract int 接口方法名(Map map);
   
   <insert id="接口方法名">
   	INSERT INTO employee(name,salary) VALUES(#{name},#{salary})
   </insert>
   ```

4. 当传入的是List集合时，可以通过固定的值获取，但建议使用@Param注解的方式来获取

5. 当传入的是多个值时，有两种方式设置key的值：

   - 使用 @Param 注解的方式

     ```java
     数据载体类型 接口方法名(@Param("字符串1")Integer age,@Param("字符串2")Double salary);
     
     <select id="接口方法名" resultType="数据载体全类名">
     	SELECT * FROM 表名 WHERE age = #{字符串1} AND salary = #{字符串2}
     </select>
     ```

   - 使用默认值的方式

     ```java
     数据载体类型 接口方法名(Integer age,Double salary);
     
     <select id="接口方法名" resultType="数据载体全类名">
     	SELECT * FROM 表名 WHERE age = #{arg0} AND salary = #{arg1}
     	// 也可以使用 param1、param2 代替 arg0、arg1
     </select>
     ```





##### 返回值

> 进行增删改操作时，返回值为int类型，即影响的行数，是固定的，因此在CRUD标签中不需要再额外去指定返回值的数据类型

###### resultType

当返回值类型为单个的简单类型（如：基本数据类型、包装类、自定义实体类、容器等）时，使用resultType属性指定返回值的数据类型

```xml
<select id="接口方法名" resultType="返回值类型全类名">
	查询的sql语句
</select>
```

- 返回值类型全类名是通用写法，也可以使用别名

  - mybatis默认为我们提供了72种常用的数据类型的别名，通常为：

    ① 基本数据类型：最前面加 `_` ，如 int（_int）、double（\_double）

    ② 包装数据类型、集合等容器类型：大写转小写，如 Integer（integer）、Map（map）

  - 若没有想要的，可以写全类名或者在核心配置文件中自定义别名

- 自定义实体类返回时，返回的是数据载体类类型，要求查询结果中的列名与在载体类中的属性名要对应

  - 可以在查询时使用列的别名来统一
  - 在核心配置文件中，开启 下划线转驼峰的自动映射功能（emp_name --> empName，若满足该规则，则会自动将该列的结果映射到实体类的属性中）
  - 使用resultMap实现

- 返回的是Map容器类型时，列名将作为key，列的结果将作为value

  - 当查询的结果没有对应的实体类类型存储，又需要获取到列名时，就可以使用Map作为返回值类型

- 要返回List容器类型时，resultType中的值为 List集合所使用的泛型的全类名

  - 在mybatis中，底层使用的依旧是ibatis的查询方式，而ibatis的查询中，无论是查询单条数据还是查询多条数据，底层都是调用 selectList 查询所有的方式（只是：若使用的是查询一条数据，则在查询到多条数据后会报错）





###### 主键回显

> 要求我们传入想要插入的数据时，是以实体类类型传入的

1. 若回显的主键为自增长主键

   ```xml
   public abstract int 接口方法名(数据载体类型 变量名);
   
   <insert id="接口方法名" useGeneratedKeys="true" keyColumn="主键列名" keyProperty="实体类对应属性名">
       插入类型的sql语句
   </insert>
   ```

   - usseGenneratedKeys 表示想要获取插入后自增长的主键

     keyColumn 指定哪个字段为自增长列

     keyProperty 是将获取到的自增长数值存储到 传入的那个实体类（接口形参）所对应的属性中

2. 若回显的主键并非自增长的主键

   - 非自增长的主键，就说明主键是要我们自己维护，确保主键的唯一性，那么就没有存在主键回显这一说法。因此，回显的是mybatis帮我们生成的主键

   ```xml
   public abstract int 接口方法名(数据载体类型 变量名);
   
   <insert id="接口方法名">
   	<selectKey order="BEFORE" resultType="返回值类型" keyProperty="实体类对应的属性名">
       	SELECT REPLACE(UUID(),'-','')
       </selectKey>
       插入类型的sql语句
   </insert>
   ```

   - \<selectKey> 是指定额外执行的SQL语句

     ​	order属性来指定 先执行（BEFORE）还是后执行（AFTER）

     ​	resultType属性用来指定 该sql语句执行后的返回值

     ​	keyProperty属性用来指定 存储该返回值的数据载体类所对应的属性名

     相当于我们自己维护主键的语句，交由了mybatis来实现：

     ```java
     String primaryKey = UUID.randomUUID().toString().replaceAll("-","");
     数据载体类型.主键对应属性名 = primaryKey;
     // 在原先，我们插入新数据时，就需要自己设定唯一的主键
     // mybatis帮我们先执行了一条SQL语句，生成主键，然后将主键设置进实体类的属性中，再通过获取实体类的属性来插入新数据
     ```





###### resultMap

> resultMap标签，用来自定义映射关系，还可以实现深层次的映射

一、自定义映射

```xml
<resultMap id="唯一id" type="返回值类型（数据载体类）的全类名">
    <id column="主键字段" property="对应实体类属性名"/>		<!-- 用来指定主键的映射关系 -->
    <result column="字段名" property="对应实体类属性名"/>	<!-- 用来指定其他字段的映射关系 -->
</resultMap>

<select id="接口方法名" resultMap="唯一id">
    查询类型的sql语句
</select>

<!-- 若result标签中的字段名是与属性名相同（或是开启了下划线转驼峰功能后可对应），则result标签可直接省略（深层次映射时还需额外开启autoMappingBehavior的功能） -->
```



二、多表查询

进行多表查询时，和原先不同的是，我们还需关注3点：

1. 多表查询的SQL语句的编写

2. 如何设计存储数据的数据载体类

   - 数据载体类的设计技巧（以客户表和订单表为例）：

     - 对于客户表而言，一个客户可以有多个订单（对多），因此客户实体类中应该有属性：订单集合

       对于订单表而言，一个订单对应一个客户（对一），因此订单实体类中应该有属性：客户对象

     - 当发生了多表查询时，才去设计（修改）实体类属性，否则，不要提前设计表间的关系

     - 无论是多少张表进行关联，实体类间都是两两考虑

     - 在查询时，只需关注本次查询所需的属性（比如：当需要查询订单和对应的客户时，就不要再去关注客户中的订单集合）

3. 自定义实体类与结果集的映射关系

   - 一对一：查询指定订单及其客户信息

     ```xml
     <resultMap id="唯一id" type="返回值类型的全类名">
         <id column="主键字段" property="实体类中对应的属性名"/>
         <result column="其他字段" property="实体类中对应的属性名"/>
         <association property="实体类中对象类型的属性" javaType="该对象的全类名">
             <id column="关联的表的主键字段" property="关联的表对应的数据载体类所对应的属性名"/>
             <result column="关联的表的其他字段" property="关联的表对应的数据载体类所对应的属性名"/>
         </association>
     </resultMap>
     <select id="接口方法名" resultMap="唯一id">
         多表查询的sql语句
     </select>
     
     <!--
     	订单实体类中，会存在一个客户对象类型的属性，是另一张表所对应的数据载体类
     	通过association标签，借助该属性，将另一张表查询到的数据，存放到其所对应的实体类的属性中
     -->
     ```

   - 多对一：查询所有客户及其订单信息

     ```xml
     <resultMap id="唯一id" type="返回值类型的全类名">
         <id column="主键字段" property="实体类中对应的属性名"/>
       <result column="其他字段" property="实体类中对应的属性名"/>
         <collection property="实体类中list集合类型的属性" ofType="list集合的泛型的全类名">
             <id column="关联的表的主键字段" property="关联的表对应的数据载体类所对应的属性名"/>
             <result column="关联的表的其他字段" property="关联的表对应的数据载体类所对应的属性名"/>
         </collection>
     </resultMap>
     <select id="接口方法名" resultMap="唯一id">
         多表查询的sql语句
     </select>
     
     <!-- 
     	客户实体类中，会存在一个List集合类型的属性，存放客户所有的订单，其泛型是另一张表的数据载体类
     	通过collection标签，将另一张表查询到的每条数据，先存放于对应的实体类的属性中，再存放于该集合属性中
     -->
     ```

     





##### 动态SQL语句

> 当：SELECT * FROM 表名 WHERE xxx = xxx AND yyy = yyy
>
> 若传入参数值xxx或yyy，则进行条件判断，否则，就按默认情况（不添加条件）进行SQL语句的执行，这种情况下，就需要使用动态的SQL语句

###### if 标签

判断是否进行SQL语句的拼接（若属性 test 的条件成立的话，则会进行SQL语句的拼接）

```xml
<select id="接口方法名" resultType="返回值类型全类名">
	SELECT * FROM 表名 WHERE
    <if test="条件1">
    	xxx = xxx
    </if>
    <if test="条件2">
    	AND yyy = yyy
    </if>
</select>
```

- 若要在test中获取传入的形参值，直接通过 key 即可获取到（#{key}中的key）
- 常用条件符号：`and`、`or`、`&gt;`（>，不推荐，低版本mybatis可能出错）、`&lt;`（<）、`!=`、`null`

> 以上拼接SQL语句的方式存在一定问题：
>
> - 若条件1不满足，则会拼接条件2，出现 WHERE AND yyy=yyy 的错误语句
> - 若条件1、2都不满足，则不拼接SQL语句，出现 SELECT * FROM 表名 WHERE 的错误语句
>
> 因此，推荐配合 where标签 一起使用





###### where 标签

- 当拼接SQL语句时，若条件满足，需要添加WHERE关键字时，会自动添加
- 自动去掉拼接的SQL语句前后多余的 AND、OR 关键字

```xml
<select id="接口方法名" resultType="返回值类型全类名">
    SELECT * FROM student
    <where>
        <if test="条件1">
            xxx = xxx
        </if>
        <if test="条件2">
            AND yyy = yyy
        </if>
    </where>
</select>
<!--
	若条件1、条件2不满足，则不会添加WHERE关键字（不拼接SQL语句）
	若条件1不满足、条件2满足，则会添加WHERE关键字，并且会删除条件2多余的AND关键字、拼接条件2中的SQL语句
-->
```





###### set 标签

set标签主要用于update更新类型的SQL语句，主要有两个功能：

- 自动添加set关键字（但是若没有条件成立，则无论添加还是不添加set关键字，都是错误的SQL语句）
- 自动去除拼接的SQL语句前后 多余的 `,`

```xml
<update id="接口方法名">
    UPDATE 表名
    <set>
        <if test="条件1">
            xxx = xxx ,
        </if>
        <if test="条件2">
            yyy = yyy
        </if>
    </set>
</update>
<!--
	若条件1和条件2不满足，则不会添加set关键字（不拼接SQL语句）
·	若条件1满足，条件2不满足，则会添加set关键字，拼接条件1对应的SQL语句，并删除末尾多余的 ,
-->
```





###### trim 标签

```xml
<select id="接口方法名" resultType="返回值类型全类名">
    SELECT * FROM 表名
    <trim prefix="WHERE" prefixOverrides="AND | OR">
        <if test="条件1">
            xxx = xxx
        </if>
        <if test="条件2">
            AND yyy = yyy
        </if>
    </trim>
</select>
```

- prefix属性：当有进行SQL语句拼接时，则会在开始拼接SQL语句之前，添加prefix指定的关键字

  suffix属性：与之类似，当有进行SQL语句拼接时，则会在拼接完所有SQL语句之后，添加suffix指定的关键字

- prefixOverrides属性：当在开始拼接SQL语句时，若开头存在该字符串，则会删除

  suffixOverrides属性：与之类似，当在拼接完所有SQL语句后，若末尾存在该字符串，则会删除

  > 用 | 可以一次性匹配多个字符串





###### choose 标签

- 相当于 if - else if - else

```xml
<select id="接口方法名" resultType="返回值类型全类名">
    SELECT * FROM 表名 WHERE
    <choose>
        <when test="条件1">
            xxx = xxx
        </when>
        <when test="条件2">
            yyy = yyy
        </when>
        <otherwise>
            zzz = zzz
        </otherwise>
    </choose>
</select>
<!-- 在所有条件中，一定会执行一个 -->
```





###### foreach 标签

```xml
<select id="getStudentsByAge" resultType="student">
    SELECT * FROM student WHERE age IN
    <foreach collection="ages" open="(" separator="," close=")" item="age">
        #{age}
    </foreach>
</select>
<!-- 结果：SELECT * FROM student WHERE age IN (xx,yy,zz) -->
```

- collection属性：要迭代的属性（List集合）

  item属性：将迭代的属性中的每一个值，每次取出来后存放于该变量中

  open属性：在最开始迭代前先拼接的内容

  close属性：在所有迭代结束后拼接的内容

  separator属性：在每次迭代结束后拼接的内容（最后一次迭代结束时，不会拼接）

- 若迭代的是集合类型的对象，取出每个对象后，通过 `对象.属性名` 可以获取到对象的属性值

- 迭代整条SQL语句时，需要在配置DataSource的url中，指定可以叠加多条SQL语句

  ```xml
  jdbc:mysql:///库名?allowMultiQueries=true
  ```

  



###### sql 标签与 include 标签

- 用来复用SQL语句

```xml
<sql id="唯一标识">
    要被复用的SQL语句
</sql>

<select id="接口方法名" resultType="返回值类型全类名">
    <include refid="唯一标识"/>
</select>
<!-- include声明的位置，将会被替换为要被复用的SQL语句 -->
```











### 三、核心配置文件

#### 基本要求

````xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!-- 约束，限定能使用的标签 -->

<configuration>  
<!-- 配置mybatis执行sql的数据库环境，可以配置多个环境，但实际运行的，只会是default属性所指定的 -->
    <environments default="environment标签的唯一id">
        <environment id="唯一id"> 
            
            <!-- 配置mybatis内置的事务管理器，type属性可以有两个值：
            ① MANAGED（不开启事务）；② JDBC（自动开启事务，但要求自己提交事务）-->
            <transactionManager type="JDBC"/>
            
			<!-- 设置数据源啊：POOLED 表示让mybatis帮我们维护连接（即：使用连接池的方式）
 							UNPOOLED 表示每次都是新建连接 -->
            <dataSource type="POOLED">
                <property name="driver" value="jdbc的driver值"/>
                <property name="url" value="jdbc的url值"/>
                <property name="username" value="jdbc的username值"/>
                <property name="password" value="jdbc的password值"/>
            </dataSource>
        </environment>
    </environments>
    
<!-- mappers用来指定mapper映射文件的位置，可以指定多个映射文件 -->
    <mappers>
        <mapper resource="以配置文件的路径为基准进行路径的配置"/>
        <package name="包路径"/>
        <!-- 
			可以引入包路径下的所有mapper映射文件，但是有2个要求：
				1.mapper接口与其映射文件的名字必须相同
				2.最终打包后的位置要相同（java、resources打包后，都是位于classes目录下）
					要确保打包后的位置相同，可以：
					① 将XML文件也放入接口所在的包下（但这种方式默认不会将XML文件也一起打包，需要配置）
					② 在resources下创建与接口所在包同名的文件夹结构（创建多层文件夹时用 / 分隔）
 		-->
    </mappers>
</configuration>
````





#### 其他配置

1. settings：进行一些额外功能的配置，声明于environments标签前

   - 添加日志输出功能（在控制台中输出日志信息）

     ```xml
     <setting name="logImpl" value="STDOUT_LOGGING"/>
     ```
     
   - 开启 返回数据载体类型数据时 下划线转驼峰后的自动映射功能（emp_name --> empName，若匹配，则会自动将该列的结果存储到该实体类的属性中）

     ```xml
     <setting name="mapUnderscoreToCamelCase" value="true"/>
     ```
     
   - 开启 resultMap 标签中的子标签 result 在深层次嵌套的情况下，指定的字段和结果集能否进行自动映射 的功能（设置后，无论是单层次还是深层次的result标签，只要能满足自动映射，则会自动映射）

     ```xml
     <setting name="autoMappingBehavior" value="FULL"/>
     ```

2. typeAliases：设置执行SQL语句后，返回值的类型（自定义类、数据载体类）的别名

   ```xml
   <typeAliases>
       <!-- 设置单个自定义类的别名 -->
       <typeAlias type="要设置别名的类的全类名" alias="别名"/>
       
       <!-- 设置该包路径下，所有的类的别名为：类名但首字母小写 -->
       <package name="包路径"/>
   </typeAliases>
   ```

   - 若是为包路径下所有的类设置别名，但其中有些类不想使用默认的别名，而是设置为其他别名，可以使用 @Alias 注解来设置

     ```java
     @Alias("别名")
     class 包路径下的类{}
     ```
   
3. plugins：添加插件的功能，可以拦截SQL语句（相当于拦截器），声明于environments标签之前









### 四、执行CRUD操作指令

1. 创建SqlSessionFactory对象

   - 通过读取核心配置文件，创建输入流，再通过SqlSessionFactoryBuild对象的方法 `build(InputStream)` ，载入输入流来创建该对象

   ```java
   InputStream is = Resources.getResourceAsStream("配置文件.xml");
   // 读取核心配置文件，创建输入流
   
   SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(is);
   // 载入输入流，创建SqlSessionFactoryBuild对象
   ```

2. 使用SqlSessionFactory对象来开启一个会话

   ```java
   SqlSession session = sessionFactory.openSession();
   // openSession()中，可以传入boolean类型的参数，表示是否：在每次进行增删改操作后自动提交事务
   ```



3.定位到sql语句并执行

- 若使用ibtatis技术，则无需创建代理对象，直接通过会话定位到对应的CRUD标签，进而执行相应的SQL语句

  ```java
  Object obj = session.selectOne("字符串1.字符串2",Object);
  /*
  	String（形参1）：字符串1指的是namespace的值，字符串2指的是CRUD标签的id值
  	Object（形参2）：执行sql语句时传入的参数
  	返回值类型为Object（不确定），可强转为sql语句执行后的结果的类型
  	
  	缺点：
  		1.定位字符串（形参1）可能写错
  		2.执行sql语句所传入的参数只能有一个（要传入多个的话只能先包装后再传入）
  		3.返回值为Object类型，还需强转
  */
  ```

- 若使用mybatis技术，则会先通过会话获取到一个代理对象（JDK动态代理，实现与Mapper相同类型的接口），再通过代理对象调用目标方法，从而触发对应的CRUD标签所对应的SQL语句

  ```java
  接口类型 变量名 = session.getMapper(Mapper接口类型.class);
  变量名.接口方法名();
  ```

>无论是mybatis还是ibatis，底层都是根据namespace和CRUD标签的id来定位到要SQL语句的
>
>- ibatis是直接通过自定义的字符串1（namespace）和字符串2（CRUD标签中的id）来定位到SQL语句的
>- mybatis是通过定义接口和方法，在调用方法时，底层是通过代理对象，将接口的全类名作为namespace，将方法名作为CRUD标签的id，使用与ibatis相同的方式定位到SQL语句的
>
>因此，ibatis和mybatis底层使用的原理是一样的，只是mybatis使用代理替我们进行了一系列的封装（包括整合SQL语句执行时所需的参数），使用起来更方便



4. 提交事务并关闭会话

   ```java
   session.commit();			// 提交事务，查询操作不需要
   session.close();			// 关闭会话
   ```









### 其他

#### 分页插件

>SELECT * FROM 表名 LIMIT x,y
>
>- x：偏移量，值为 (当前页码 - 1)*页面显示条数
>
>  y：页面显示条数

1. 导入依赖

   ```xml
   <dependency>
       <groupId>com.github.pagehelper</groupId>
       <artifactId>pagehelper</artifactId>
       <version>5.3.1</version>
   </dependency>
   ```

2. 在核心配置文件中添加插件

   ```xml
   <plugins>
       <plugin interceptor="com.github.pagehelper.PageInterceptor">
           <!-- 指定是用mysql语法 -->        
           <property name="helperDialect" value="mysql"/>
       </plugin>
   </plugins>
   ```

3. 正常进行SQL语句的编写，但不需要使用LIMIT进行分页，在SQL语句末尾也不要添加 ; 结束（调用分页插件执行时，会自动添加LIMIT关键字进行查询）

4. 进行分页规则设定，将查询后的数据进行分页

   ```java
   PageHelper.startPage(获取的页码,每页显示条目数);
   // 在进行查询之前，需要先进行分页规则的设定
   
   // 获取查询数据，一般会返回List集合
   
   PageInfo<数据载体类型> pageInfo = new PageInfo<>(查询的结果集);
   // 将查询结果作为参数，由PageInfo进行分页
       
   // 调用PageInfo对象的方法，获取分页后的数据
   List<数据载体类> list = pageInfo.getList();		// 获取指定页码中的数据
   int pages = pageInfo.getPages();			// 获取数据的总分页数
   long pageNum = pageInfo.getTotal();			// 获取数据的总条目数
   ```

   





#### ORM思想与逆向工程

Java代码中，使用的是面向对象编程思想，是将内容进行封装，我们直接调用即可

Sql代码中，使用的多是面向过程编程思想，实现的内容是要我们自己写代码的

当两者结合，就需要有一端进行妥协。JDBC技术可以看出，Java代码中还需我们自己手动写SQL语句

因此，我们尝试使用面向对象的方式来进行SQL语句的操作，这就是ORM思想

ORM思想实现的框架，大体分为两类：

- 全自动ORM框架（如 hibernate）：提供了CRUD方法，并自动生成了SQL语句

- 半自动ORM框架（如 mybatis）：提供了CRUD方法，但需要我们自己写SQL语句

  - 虽然我们自己写SQL语句更灵活，但一些简单的SQL语句也无需总是自己写，因此就想在半自动的基础上，添加一些全自动的功能，这种方式就被称为 逆向工程

    > 需要注意的是，mybatis只支持对单表进行逆向工程













## SpringMVC

### 介绍

前端发送请求，能拦截请求、获取请求参数的，只有 Servlet 和 Filter 有这样的功能

SpringMVC，就是对Servlet进行了封装的框架

SpringMVC主要的功能，就是对前端发送的请求参数、响应给前端的数据，进行了简化处理

SpringMVC相较于其他前端处理器的最大优点，就是与Spring无缝对接



SpringMVC内部的工作流程：

1. 当前端发送了一个请求时，会交由DispatcherServlet处理

2. DispatcherServlet接收到请求时，并不会直接去调用请求所对应的方法（handler处理器），而是通过HandlerMapping，判断该请求所对应的方法是否存在

3. 在确认了请求所对应的方法存在后，会将request对象交由HandlerAdapter处理，HandlerAdapter会将request内部包含的原始参数提取出来，转换为handler所需的参数，并去调用handler

   handler处理完成后，会将响应的数据返回给HandlerAdapter，HandlerAdapter会将返回的数据封装到response对象中，再响应给DispatcherServlet

- 若接收到的请求是一个页面，则会将该请求发送给视图解析器，由视图解析器对逻辑视图进行拼接，访问完整的页面，再由视图解析器对页面进行返回

DispatcherServlet：处理全部请求

HandlerMapping：缓存了handler方法地址

HandlerAdapter：参数处理和包装响应

视图解析器：简化视图查找









### 基本实现

#### 一、导入依赖

```xml
<!-- 需要用到SpringIOC容器 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>6.0.6</version>
</dependency>

<!-- SpringMVC核心依赖 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>6.0.6</version>
</dependency>

<!-- Spring WEB MVC 6 中，将Servlet API迁移到了该依赖中 -->
<dependency>
    <groupId>jakarta.platform</groupId>
    <artifactId>jakarta.jakartaee-web-api</artifactId>
    <version>9.1.0</version>
    <scope>provided</scope>
</dependency>
```







#### 二、编写Handler

首先，需要先将Handler所在的类加入到IOC容器中

```java
@Controller				// 包扫描后，会将该类交由IOC容器管理
class 类名{}
```



设置方法所对应的请求地址（根据请求调用相对应的方法）

```java
@RequestMapping(value="请求路径")
public 返回值类型 方法名(){}
// 相较于@WebServlet，@RequestMapping所要求的请求路径，不是必须以 / 开头的
```

- @RequestMapping支持精准地址匹配，也支持模糊地址匹配

  - `*` 表示一层的任意字符串，如：/user/* --> /user/a、/user/b

    `**` 表示任意层的任意字符串，如：/user/** --> /user/a、/user/a/b

- 类上也可以添加@RequestMapping注解

  - ```java
    @RequestMapping("请求路径1")
    class 类名{
        @RequestMapping("请求路径2")
        public 返回值类型 方法名(){}
    }
    ```

    此时若想访问请求路径2的方法时，实际上的访问路径为：请求路径1/请求路径2（类地址 + 方法地址）

  - 若是想将类上的地址作为方法的地址，那么方法只需声明@RequestMapping注解即可

- @RequestMapping可以指定只允许哪些请求方式的访问

  - 默认情况下，只要请求地址匹配，所有的请求方式都可以访问

  - ```java
    @RequestMapping(value="请求地址",method={RequestMethod.GET})
    public 返回值类型 方法名(){}
    /* 此时该方法只能由：地址匹配、请求方式为GET的，才能访问
    常用的请求方式有：RequestMethod.POST、RequestMethod.DELETE、RequestMethod.PUT
    请求地址匹配、但不符合请求方式的，会出现405错误
    */
    ```

  - @RequestMapping与请求方式结合，衍生出了一些注解，如：@GetMapping("请求地址")、@PostMapping("请求地址")，但只能运用于方法上







#### 三、创建配置类

```java
@Configuration		// 标记该类为配置类
@ComponentScan("包路径")		// 包扫描
class 类名{
    // 将HandlerMapping、HandlerAdapter加入到IOC容器中（添加@EnableWebMvc注解后，就不需要再配置）
    @Bean
    public RequestMappingHandlerMapping handlerMapping(){
        return new RequestMappingHandlerMapping();
    }
    @Bean
    public RequestMappingHandlerAdapter handlerAdapter(){
        return new RequestMappingHandlerAdapter();
    }
}
```







#### 四、加载IOC容器

> 让一个类继承AbstractAnnotationConfigDispatcherServletInitializer抽象类，并实现其抽象方法
>
> 当Web项目被加载时，会自动加载指定的配置类，创建IOC容器，还可以设置DispatcherServlet可以处理的请求地址

```java
class 类名 extends AbstractAnnotationConfigDispatcherServletInitializer{
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[0];
    }
	// 指定要加载的SpringMVC配置类
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{配置类.class};
    }
	// 指定DispatcherServlet能处理的请求
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
        // /表示处理所有的请求
    }
}
```

1. AbstractAnnotationConfigDispatcherServletInitializer底层中，实现了父类接口WebApplicationInitializer，该接口中有一个onStartup的抽象方法，会在Web项目被加载时自动调用
2. onStartup的实现方法中，会去调用registerDispatcherServlet方法，内部会接着去调用createServletApplicationContext方法
   - 在createServletApplicationContext方法内部，会使用AnnotationConfigWebApplicationContext加载配置类的方式，读取getServletConfigClasses方法中的配置类，创建IOC容器
3. 同时，在registerDispatcherServlet方法中，会调用addMapping方法，获取getServletMappings中的映射地址进行设置









### 两大功能

#### 参数处理

> 发送过来的请求参数一般有三种：param普通参数（key=value&key=value）、路径参数（/value1/value2/value3）、JSON格式对象（{key:value,key:value}）

##### 获取参数

###### Param参数

1. 通过相同参数名直接接收

   ```java
   @RequestMapping("请求路径")
   public 返回值类型 方法名(参数类名 参数名){}
   /* 两个特点：
   	1.方法形参名与请求参数名相同时，就可直接获取到请求参数
   	2.请求参数可以传递，也可以不传递
   */
   ```

2. 通过@RequestParam注解获取指定参数

   ```java
   @RequestMapping("请求路径")
   public 返回值类型 方法名(@RequestParam(value="形参名")形参类型 形参名){}
   /* 获取指定的请求参数，获取不到时会报错（400错误）
   	当获取的请求参数的名与形参名一致时，value可以省略不写
   	通过required属性，设置其值为false，可以在获取不到参数时依旧不会报错（默认为true）
   		通过defaultValue属性，设置请求参数的默认值，在获取不到请求参数时，使用默认值
   */
   ```

3. 用一个形参变量接收多个值：使用List集合，配合@RequestParam注解接值

   ```java
   @RequestMapping("请求路径")
   public 返回值类型 方法名(@RequestParam("参数名")List 参数名){}
   /* 若是不使用@RequestParam注解，则会将一个请求参数的值存放到形参中，从而出现类型不匹配的错误 */
   ```

4. 使用实体类对象接收参数值

   ```java
   @RequestMapping("请求路径")
   public 返回值类型 方法名(实体类类型 变量名){}
   /* 要求：实体类中的属性必须提供对应的get、set方法 */
   ```

   



###### 路径参数

```java
@RequestMapping("/{形参名1}/{形参名2}")
public 返回值类型 方法名(@PathVariable 参数类型 形参1,@PathVariable 参数类型 形参2){}
/* 使用{}来表示该路径是参数值，在形参处要获取的
	@PathVariable中，属性value用来指定获取的路径参数（当形参名与路径参数名一致时，value可省略）
		属性required用来指定该参数值是否必须传递
*/
```





###### JSON参数

1. 编写实体类（Java对象作为JSON格式数据的载体）

2. 使用@RequestBody注解标识实体类形参，接收JSON格式数据

   ```java
   @RequestMapping(value="请求路径",method=RequestMethod.POST)
   public 返回值类型 方法名(@RequestBody 实体类型 参数名){}
   /* Java原生的API中，仅支持Param和路径参数的获取，并不支持Json格式，因此，仅通过添加@RequestBody注解，是无法获取到请求体中的Json数据的 */
   ```

3. 导入依赖：jackson

   ```xml
   <dependency>
       <groupId>com.fasterxml.jackson.core</groupId>
       <artifactId>jackson-databind</artifactId>
       <version>2.15.0</version>
   </dependency>
   ```

4. 为HandlerAdapter配置JSON转换器

   ```java
   @EnableWebMvc		// 为HandlerAdapter配置JSON转换器
   @Configuration		// 配置类标识
   class 配置类名{}
   
   /* 实际上，@EnableWebMvc注解，相当于XML配置文件中的annotation-driven标签
   	查看spring-webmvc包，其下的spring.handlers文件，进行MvcNamespaceHandler类中，查看annotation-driven所对应的类，该类会自动调用parse方法
   	在方法内部可以看到，会重新创建一个RootBeanDefinition的IOC容器，加载RequestMappingHandlerMapping和RequestMappingHandlerAdapter，并后续通过addRequestBodyAdvice方法，配置JSON转换器
   	因此，当配置了@EnableWebMvc注解后，已经不需要再额外创建HandlerMapping、HandlerAdapter了
   */
   ```

   





##### 获取请求头与Cookie值

```java
@RequestMapping("请求地址")
public 返回值类型 方法名(@CookieValue("cookie的key值")String 变量名){}
// 获取Cookie值

@RequestMapping("请求地址")
public 返回值类型 方法名(@RequestHeader("请求头中某个数据的key值")String 变量名){}
// 获取请求头中key所对应的value值
```







##### 获取原生对象

像常用的ServletRequest、ServletResponse、HttpSession、输入输出流，都可以直接通过属性名获取

```java
@RequestMapping("请求地址")
public 返回值类型 方法名(HttpServletRequest request){}	// 常用的原生对象，都可以直接获取
/* ServletContext的获取方法比较特殊，有两种方式：
	1.通过request或session的getServletContext方法获取
	2.在程序启动时，servletContext对象会被自动装载如IOC容器中，通过DI自动注入即可获取到该对象
		@Autowired
		private ServletContext context;
*/
```







##### 共享域对象

对于共享域的操作，直接通过获取原生HttpServletRequest来操作即可

而request方式的共享域，SpringMVC额外提供了四种方式储存：Model、ModelMap、Map、ModelAndView









#### 返回处理

##### 页面跳转

页面跳转可分为两步骤：

1. 编写视图解析器（以jsp视图解析器为例）

   ```java
   @Configuration				// 设置该类为配置类
   @ComponentScan("包路径")		// 开启扫描
   @EnableWebMvc				// 配置jsp转换器，配置 HandlerMapping、HandlerAdapter
   // 让配置类实现该接口：该接口提供了一些快速配置功能的方法
   class 配置类 implements WebMvcConfigurer{
       // 配置JSP视图解析器
       @Override
       public void configureViewResolvers(ViewResolverRegistry registry) {
           registry.jsp("视图前缀","视图后缀");
       }
   }
   ```

2. 编写页面，设置映射方法，跳转到对应页面

   ```java
   @RequestMapping("请求路径")
   public String 方法名(){
       return "逻辑视图名";
   }
   // 映射方法中，唯一要注意的是不能添加@ResponseBody注解（该注解会将返回内容直接响应给前端）
   ```







##### 转发与重定向

```java
@RequestMapping("请求路径")
public String 方法名(){
    return "forward:请求路径";
}
/* 实现转发的两个要点：
	1.返回值为字符串，且在返回的请求路径前添加 forward:
	2.不能添加@ResponseBody注解
*/

@RequestMapping("请求路径")
public String 方法名(){
    return "redirect:请求路径";
}
/* 实现重定向的两个要点：
	1.返回值为字符串，且在返回的请求路径前添加 redirect:
	2.不能添加@ResponseBody注解
*/
```

- 转发只能实现项目下的资源转发，转发路径忽略 applicationContext（上下文路径）
- 重定向可以是项目下的资源，也可以是其他外部资源，是属于二次请求，请求路径需要是全地址（即包括applicationContext在内的路径）。但是，SpringMVC替我们进行了封装，因此我们无需再书写applicationContext上下文路径







##### 返回JSON数据

返回JSON数据的主要步骤：

1. 导入依赖：jackson

   ```xml
   <dependency>
       <groupId>com.fasterxml.jackson.core</groupId>
       <artifactId>jackson-databind</artifactId>
       <version>2.15.0</version>
   </dependency>
   ```

2. 配置JSON转换器

   ```java
   @Configuration		// 标记该类为配置类
   @EnableWebMvc		// 开启JSON转换器
   class 配置类名{}
   ```

3. 编写数据载体类，声明上 @ResponseBody 注解，最后将该Java实体类对象直接返回

   ```java
   @RequestMapping("请求路径")
   @ResponseBody
   public 实体类类型 方法名(){}
   ```

   - 返回的类型无论是实体类类型还是List集合类型，JSON都有对应的格式接收
   - @ResponseBody 表明将数据直接放入响应体中进行返回，不会使用视图解析器，也不会进行转发或重定向
   - @ResponseBody 注解也可以声明在类上。与 @Controller 注解一起使用时，可以使用 @RestController 注解代替







##### 静态资源访问

在配置了DispatcherServlet后，所有的访问，都会被交给HandlerMapping进行查找，查找不到访问路径时，就会报404错误。而静态资源，是可以直接访问的，并不需要通过handler来进行处理

只需在配置类中，配置一个功能，让访问不能被HandlerMapping查找到时，交由DefaultServletHandler来查找；都查找不到时，再报404错误

```java
@Configuration		// 标识该类为配置类
class 配置类 implements WebMvcConfigurer{
    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }
}
```

- 该效果，相当于在XML配置文件中设置 default-servlet-handler
- 通过引入的依赖 webmvc，其内META-INT下的properties文件 spring.handlers 中，可以看到default-servlet-handler所对应的类 DefaultServletHandlerBeanDefinitionParser，进去看到parse方法中，通过DefaultServletHttpRequestHandler又创建了一个IOC容器，而该类中的handleRequest方法，rd.forward(request, response) 已经很明显的表明了该配置可以实现静态资源访问的原理：
  - 底层是使用了转发功能，直接去项目中查找资源，若能查到说明存在资源，直接返回；若查找不到，则说明不存在资源，报404错误









### 其他内容

#### RestFul风格

客户端与服务器进行交互，客户端发送request请求，服务端进行response响应

在请求与响应的过程中，使用的是Http协议

Http协议要求：请求的地址（URL）、请求方式、传递参数

若是没有一套标准，则在请求方式上，可能会存在各种各样的请求方式；而传递参数方面，也可能使用不同的方式传递（如param、地址传递、json）

RestFul就是这么一套Http协议规范的标准和风格：规定了请求路径的设计、参数的传递、请求方式的选择





RestFul风格的两大体现：

1. URL回归到名词形式表示

   > 如：/mvc/getUser?id=1  --> /mvc/user/1
   >
   > URL只用名词表示，不再使用动词

2. 用指定的请求方式来表示要进行的操作

   - GET：查询操作

     PUT：修改操作

     POST：新增操作

     DELETE：删除操作

需要注意的是，并不是所有的请求参数都是路径传递

- 一般情况下，当请求的内容返回单一资源时（如根据id查询一个用户），使用路径传递；但是，当请求的内容返回的是资源集合时（如模糊查询），这时候就要考虑使用param参数传递
- @RequestMapping中的请求路径是不能有重复的。因此，当不得不出现重复的请求路径时，其中的一方的请求路径可以添加动词来区分

总结：

1. GET、DELETE方式没有请求体，只能 路径传参 或 param 传参；若内容仅有一个，则路径传参；若内容有多个，则param传参
2. POST、PUT方式有请求体，一般都是直接使用 JSON 传参







#### 声明式异常处理

> 声明式异常即：不在编写代码的过程中进行异常处理，而是将异常处理声明在一个类中，当出现异常时，就会去该类中查找对应的处理异常的handler

1. 定义该类为异常处理类

   ```java
   @ControllerAdvice			// 定义该类为全局异常处理类，当出现异常时，执行该类下的handler
   class 异常处理类{}
   ```

   - 声明为 @ControllerAdvice 注解的话，handler返回值可以是逻辑视图名，也可以是进行转发或重定向
   - 声明为 @RestControllerAdvice 注解的话，则handler处理后，响应的为 JSON字符串（相当于 @ControllerAdvice + @ResponseBody 结合）

2. 将handler作为指定异常的处理方法

   ```java
   @ExceptionHandler(异常类.class)
   public 返回值类型 方法名(异常类类型 变量名){}
   ```

   - 使用 @ExceptionHandler 指定该方法为对应异常的处理方法（当出现对应异常时，会执行该方法）

     可以匹配精准的异常类型，从而执行对应方法；若没有精准匹配，则其父类异常所对应的方法也可以被执行

   - 在参数位置处，可以获取到异常对象，从而获取异常信息

3. 让该异常处理类加入IOC容器中（被包扫描到）







#### 拦截器

>与过滤器Filter相似，拦截器也有拦截请求的功能
>
>不同的是，过滤器是每个JavaWeb都有的，则拦截器是SpringMVC特有的
>
>过滤器在SpringMVC中，是在请求交由DispatcherServlet处理前进行拦截的，因此可能无法做到精细化拦截（如登录校验等），而拦截器是在handler执行前、后等进行拦截的，能更好的把控

拦截器的配置分为：

1. 创建一个类，实现HandlerInterceptor接口

   ```java
   class 类名 implements HandlerInterceptor{
       // 会在DispatcherServlet调用handler前进行拦截 
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           // 返回true，则放行；返回false，拦截请求（并且后续的拦截全都将不会执行到）
           // Object handler参数为：要调用的handler方法
           return true;
       }
   
       // 会在handler响应给DispatcherServlet前进行拦截
       @Override
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
       }
   
       // 会在视图处理器响应给DispatcherServlet前进行拦截
       @Override
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
       }
   }
   ```

2. 在配置类中，通过实现WebMvcConfigurer接口，开启拦截器功能，并配置上指定拦截器

   ```java
   @Configuration		// 配置类标识
   class 配置类 implements WebMvcConfigurer{
       @Override
       public void addInterceptors(InterceptorRegistry registry) {
           registry.addInterceptor(拦截器对象);
       }
   }
   ```

   - 设置仅拦截部分请求或不拦截部分请求：

     `registry.addInterceptor(拦截器对象).addPathPatterns("/拦截路径").excludePathPatterns("/不拦截路径")`

     addPathPatterns方法与excludePathPatterns方法为链式调用

     拦截路径同样可以使用模糊路径：* 表示一层任意字符串，** 表示多层任意字符串



拦截器的先后执行顺序：先被addInterceptor注册的拦截器，其拦截器处于外层：

1. 外层拦截器preHandler被调用，接着是内层拦截器preHandler被调用；
2. 内层拦截器postHandle被调用，接着是外层拦截器postHandle被调用；
3. 内层拦截器afterCompletion被调用，接着是外层拦截器afterCompletion被调用







#### 校验注解

校验注解是用来快速对请求参数的校验的。该套注解使用的是JSR303规范，底层是hibernate实现的，但SpringMVC支持对该套注解的解析

使用方式：

1. 导入依赖

   ```xml
   <dependency>
       <groupId>org.hibernate.validator</groupId>
       <artifactId>hibernate-validator</artifactId>
       <version>8.0.0.Final</version>
   </dependency>
   <dependency>
       <groupId>org.hibernate.validator</groupId>
       <artifactId>hibernate-validator-annotation-processor</artifactId>
       <version>8.0.0.Final</version>
   </dependency>
   ```

2. 在实体类的属性中使用注解

   - 常用的注解：

     1. @Null：标记的属性值必须为null

        @NotNull：标记的属性值不能为null

        - @NotEmpty：标记的集合属性值的长度不能为0
        - @NotBlank：标记的字符串属性值的内容不为`""`或`" "`，且不为null

     2. @AssertTrue：标记的属性值必须为true

        @AssertFalse：标记的属性值必须为false

     3. @Min(value)、@DecimalMin(value)：标记的属性值不能小于value

        @Max(value)、@DecimalMax(value)：标记的属性值不能大于value

     4. @Size(max,min)：标记的属性值只能在max和min的范围内

     5. @Past：标记的属性值为日期类型，且必须是过去的时间

        @Future：标记的属性值为日期类型，且必须是未来的时间

     6. @Pattern(value)：标记的属性值必须满足指定的正则表达式规则

     7. @Email：标记的属性值的格式必须满足邮箱地址的格式

     8. @Length：标记的字符串属性的值长度必须在指定范围内

     9. @Range：标记的属性值必须在指定的范围内

3. 在接收请求参数的形参位置处，声明一个实体类类型对象，并使用@Validated注解开启注解校验功能

   ```java
   @RequestMapping("请求路径")
   public 返回值类型 方法名(@Validated 实体类类型 变量名){}
   // 这样就会对传入进来的实体类中的属性进行规则校验。若不满足规则，则会向前端抛出异常
   ```

   - 若不满足规则，则会直接向前端抛出异常。若是想自定义抛出的异常信息或对异常处理，则可以在校验的实体类变量后，声明一个绑定属性BindingResult

     ```java
     @RequestMapping("请求路径")
     public 返回值类型 方法名(@Validated 实体类类型 变量名,BindingResult result){
     	if(result.hasErrors()){
             return 自定义异常信息;
         }
     }
     /* 需要注意：
     	1.BindingResult必须紧挨着实体类变量，否则不会生效
     	2.结果应该返回自定义的异常信息，否则不会抛出异常信息
     */
     ```









#### 同源策略

假设有前端页面：http://test.com.8080，想发送请求到后台：https://test.com:8080，这时候，浏览器会触发其安全策略进行阻止：同源策略

- 默认情况下， 浏览器仅支持同源（同组织）的资源进行互相访问

同源的判断依旧：协议、IP、端口号（全部相同即同源）

若想要实现访问后台数据，只需让后台告诉浏览器，不要拦截该前端的请求，让其正常访问即可

```java
@CrossOrigin		// 允许浏览器让非同源的进行访问
// 声明在类上时，表示该类下所有的请求都可以访问；声明在方法上时，只有方法对应的请求才可以访问
@Controller			// 控制层组件标识
class 类名{}
```







#### 令牌

> 令牌（Token）是一种代表资源的访问权限或进行身份校验的字符串或数字，是一种规范，而 JWT（JSON Web Token）是其中的一种具体实现

当用户将登录信息发生给服务器时，服务器会将信息加密后，生成对应的Token后返回给客户端

客户端会将收到的Token进行保存。在下次发送请求时，就会携带Token

服务器收到Token后，会进行解密和认证，判断其有效性，若有效才会返回数据

- Cookies在出现跨域时，是无法使用的；因此，考虑在请求头中携带Token，通过Token来实现认证等功能



JWT由三部分组成：header（头部）、payload（载荷）、signature（签名）

因此，JWT可以携带很多信息，一般有：

1. 有效时间：确保Token的时效性
2. 签名秘钥：防止被窃取后，被其他人随意解析
3. 用户标识信息：通过解析Token，就可以指定Token对应的具体的用户是谁

```java
// 工具类示例：
public class JwtHelp {
    private static long tokenExistTime = 1000 * 60 * 60;       // 定义token有效时间：60分钟内
    private static String tokenSignKey = "123456";          // 定义token生成或解析时的秘钥

    // 该方法用来创建token
    public static String createToken(Long userId, String username) {
        return Jwts.builder()
                // 定义token过期时间（当前时间+指定时间=过期的时间点）
                .setExpiration(new Date(System.currentTimeMillis() + tokenExistTime))
                // 生成的token中包含的信息，也是token的主体部分
                .claim("userId", userId).claim("username", username)
                // 编码压缩
                .signWith(SignatureAlgorithm.HS512, tokenSignKey)
                .compressWith(CompressionCodecs.GZIP).compact();
    }

    // 该方法用来获取token中的内容
    public static Long getUserId(String token) {
        Jws<Claims> jws = Jwts.parser()
                .setSigningKey(tokenSignKey).parseClaimsJws(token);	  // 根据秘钥解析token
        return ((Integer) jws.getBody().get("userId")).longValue();	// 获取token中的指定信息
    }

    // 该方法用来获取token中的内容
    public static String getUsername(String token) {
        Jws<Claims> jws = Jwts.parser().setSigningKey(tokenSignKey).parseClaimsJws(token);
        return (String) jws.getBody().get("username");
    }
```













## SSM整合

### 整合分析

1. 整合为两个IOC容器：Web IOC容器、Root IOC容器
   - Web IOC容器将作为子容器，主要装配Controller层组件，如：HandlerMapping、HandlerAdapter ...
   - Root IOC容器将作为父容器，主要装配Service层、Mapper层的组件，如：AOP、TX相关组价；DataSource ...
2. 创建IOC容器的所需的配置类至少要有两个。但一般根据每层架构创建相应的配置类：由Web IOC容器加载controller层配置类；由Root IOC容器加载service层、mapper层配置类





### 基本步骤

#### 一、导入依赖

```xml
	<!-- Spring -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>6.0.6</version>
</dependency>
<dependency>
    <groupId>jakarta.annotation</groupId>
    <artifactId>jakarta.annotation-api</artifactId>
    <version>2.1.1</version>
</dependency>
<!-- AOP -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>6.0.6</version>
</dependency>
<!-- TX事务 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>6.0.6</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>6.0.6</version>
</dependency>

	<!-- SpringMVC -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>6.0.6</version>
</dependency>
<dependency>
    <groupId>jakarta.platform</groupId>
    <artifactId>jakarta.jakartaee-web-api</artifactId>
    <version>9.1.0</version>
</dependency>
<!-- JSON -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.15.0</version>
</dependency>
<!-- 注解校验 -->
<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>8.0.0.Final</version>
</dependency>
<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator-annotation-processor</artifactId>
    <version>8.0.0.Final</version>
</dependency>

	<!-- MyBatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.7</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.28</version>
</dependency>
<!-- 分页插件 -->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.3.1</version>
</dependency>

	<!-- 整合所需 -->
<!-- Spring容器 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>6.0.6</version>
</dependency>
<!-- 整合MyBatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>3.0.2</version>
</dependency>
<!-- 数据库连接池 -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.8</version>
</dependency>
<!-- log4j日志功能（还需要添加log4j配置文件） -->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.3</version>
</dependency>
```







#### 二、配置类

##### controller层

1. 扫描controller层组件
2. 配置全局异常处理类
3. 配置HandlerAdapter、HandlerMapping、JSON转换器
4. 设置静态资源处理
5. 配置视图解析器
6. 配置拦截器

````java
@Configuration		// 配置类标识
@ComponentScan({"controller层组件所在包", "异常处理类所在包"})		// 组件扫描
@EnableWebMvc		// 配置HandlerAdapter、HandlerMapping、JSON转换器
public class 配置类名 implements WebMvcConfigurer {
    // 配置静态资源处理
    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }
	// 配置视图解析器
    @Override
    public void configureViewResolvers(ViewResolverRegistry registry) {
        registry.jsp("前缀", ".后缀");		// 配置JSP视图解析器
    }
	// 配置拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
    	registry.addInterceptor(拦截器对象);
    }
}
````





##### service层

1. 扫描service层组件
2. 扫描增强类组件；开启功能：对AOP注解的支持
3. 配置事务管理器的实现；开启功能：对声明式事务注解的支持

```java
@Configuration			// 配置类标识
@ComponentScan({"service层组件所在包","增强类所在的包"})		// 组件扫描
@EnableAspectJAutoProxy		// 开启对AOP注解支持的功能
@EnableTransactionManagement		// 开启对声明式事务注解支持的功能
public class 配置类名 {
    // 配置事务管理器的底层实现
    @Bean
    public TransactionManager transactionManager(DataSource dataSource) {
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(dataSource);
        return transactionManager;
    }
}
```





##### mapper层

对于mapper中的哪些组件需要交由IOC容器进行管理：

- SqlSessionFactoryBuilder：该类仅仅是用来创建SqlSessionFactory，仅使用一次，后续也无需再次使用，因此无需添加进IOC容器中
- SqlSessionFactory：该类会缓存下配置文件的信息，应该在运行期间一直存在，因此应存入IOC容器中
- SqlSession：每个线程应当有自己的SqlSession实例，不能被共享，因此不要添加进IOC容器中
- Mapper映射器：理论上，我们仅需通过getMapper获取代理对象即可，是无需存入IOC容器的。但是，若不存入IOC容器中，则只能从IOC容器中获取SqlSessionFactory，再从中获取SqlSession，再调用方法获取代理对象，此过程过于繁琐，因此应该将Mapper映射器加入IOC容器中

总结：交由IOC容器管理的有：SqlSessionFactory、Mapper映射器



而配置类的方式有两种方式：

1. 配置类 + XML文件：

   - XML配置的方式与原先一样（如：别名、插件等），但是：

     数据源不需要配置，而是自己在配置类中指定（使用Druid连接池）

     Mapper接口扫描不需要配置，而是自己在配置类中指定（将Mapper接口创建的代理对象交由IOC容器管理）

   - 除去这两个内容，就仅需在配置类中配置SqlSessionFactory对象，使其交由IOC容器管理即可

   ```java
   @Configuration			// 配置类标识
   public class 配置类名 {
   // SqlSessionFactoryBean即是SqlSessionFactory的FactoryBean，是mybatis提供的快速创建SqlSessionFactory的类，通过引入mybatis-spring依赖即可调用该类
       @Bean
       public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource) {
           SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
           sqlSessionFactoryBean.setDataSource(dataSource);
           Resource resource = new ClassPathResource("xml配置文件名");
           sqlSessionFactoryBean.setConfigLocation(resource);
           // SqlSessionFactory是用来加载配置文件的，因此需要读取配置文件（通过流方式加载）
       	// 在配置文件中，没有配置数据源，因此还需在SqlSessionFactory中主动设置数据源
           return sqlSessionFactoryBean;
       }
       
   // MapperScannerConfigurer实际上是实现将Mapper代理对象存入IOC容器的FactoryBean
   // 当指定mapper接口与映射文件所在的包时，会将该包路径下所有的mapper接口，创建出其对应的代理对象，并存入IOC容器中
       @Bean
       public MapperScannerConfigurer mapperScannerConfigurer() {
           MapperScannerConfigurer configurer = new MapperScannerConfigurer();
           configurer.setBasePackage("mapper接口与映射文件所在的相同的包名");
           return configurer;
       }
   }
   ```

   - DataSource只需读取外部配置文件，并创建DruidDataSource即可

     ```java
     @Configuration		// 配置类标识
     @PropertySource("classpath:properties文件名")		// 引用外部文件
     public class 配置类名 {
         @Value("${jdbc.url}")
         private String url;
         @Value("${jdbc.driver}")
         private String driver;
         @Value("${jdbc.username}")
         private String username;
         @Value("${jdbc.password}")
         private String password;
         
         @Bean
         public DataSource dataSource() {
             DruidDataSource dataSource = new DruidDataSource();
             dataSource.setUrl(url);
             dataSource.setDriverClassName(driver);
             dataSource.setUsername(username);
             dataSource.setPassword(password);
             return dataSource;
         }
     }
     ```

     若是DataSource的创建，与SqlSessionFactoryBean的创建处于相同的配置类中，会出现问题：

     - SqlSessionFactory的创建是由mybatis自己封装的FactoryBean实现的，会先于外部文件读取

       因此，当SqlSessionFactory设置数据源时，DataSource的属性值还全为null

     - 解决方法也很简单：将数据源提取到另一个配置类中

2. 完全配置类：

   - 完全配置类的方式与XML配置文件方式只有一个不同点：

     完全配置类不再让SqlSessionFactoryBean去读取配置文件，而是自己添加配置

   ```java
   @Bean
   public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource) {
       SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
       sqlSessionFactoryBean.setDataSource(dataSource);
         
       org.apache.ibatis.session.Configuration configuration = new org.apache.ibatis.session.Configuration();
       configuration.setMapUnderscoreToCamelCase(true);	// 下划线转驼峰
       configuration.setLogImpl(Slf4jImpl.class);			// 控制台输出日志信息
       configuration.setAutoMappingBehavior(AutoMappingBehavior.FULL);
       // 自动深层次结果集映射到属性
       
       sqlSessionFactoryBean.setConfiguration(configuration);	
   // 设置settings标签中的内容：开启功能。需要一个Configuration类型的形参（其内部进行功能开启）
   
       sqlSessionFactoryBean.setTypeAliasesPackage("com.atguigu.pojo");
   // 设置typeAliases标签中的内容：返回值类型别名
   
       PageInterceptor pageInterceptor = new PageInterceptor();
       Properties properties = new Properties();
       properties.setProperty("helperDialect","mysql");	// 在Properties中设置好数据库类型
       pageInterceptor.setProperties(properties);
       // 设置分页插件处理的数据库类型，需要的是Properties类型的参数
       sqlSessionFactoryBean.addPlugins(pageInterceptor);
   // 设置plugings标签中的内容：添加插件（添加分页插件）
   
       return sqlSessionFactoryBean;
   }
   ```







##### 容器初始化类

声明一个类，继承AbstractAnnotationConfigDispatcherServletInitializer接口，并重写其内的抽象方法

```java
public class InitClass extends AbstractAnnotationConfigDispatcherServletInitializer {
    // 用该方法来初始化service层、mapper层的IOC容器
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{ service层配置类.class , mapper层配置类.class };
    }

    // 用该方法来初始化controller层的IOC容器
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{ controller层配置类.class };
    }

    // 设置DispatcherServlet拦截的请求路径
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```













## SpringBoot

> 使用SpringBoot，可以实现只需编写少量配置，就可快速整合Spring及第三方技术
>
> - 约定大于配置（只要按照约定，即可使用默认配置）

### 配置文件

SpringBoot是集中式管理配置的，在统一一个配置文件中进行参数设置或参数声明

位于resources文件夹下，且必须命名为 `application`（-xxx） `.properties` 或 `.yaml`（`.yml`）

- -xxx：SpringBoot允许有多个起了别名的配置文件（如 `application-xxx.yml`）。在项目运行时，启动的是没有别名的主配置文件；而在主配置文件中，可以激活其他配置文件的运行（多环境配置）

SpringBoot提供了很多配置参数，有固定key的，且很多具有默认值。若没有需要的配置参数，可自定义key，并在Java类中，通过@Value或@ConfigurationProperties注解去读取value值并赋值到属性中



#### properties文件与yaml文件

properties文件中key与value的形式：key=value

```properties
server.port=80			
# 设置服务器默认端口号
server.servlet.context-path=/boot
# 设置项目根路径（contextPath）

person.teacher.user.username=Tom
person.teacher.user.password=123
# 自定义key-value
```

-  为了保证key不重复，需要叠加多层结构，会使得key特别长



yaml文件中key与value的形式：

```yaml
server:
	port: 80			# 设置服务器默认端口号
	servlet:
		context-path: /boot		# 设置项目根路径
		
person:
	teacher:
		user:
			username: Tom
			password: 123
	student:
		user:
			username: Jack
			password: 123
# 自定义key-value
```

- 格式注意：

  - 每层结构前是一个 缩进（第一层结构不需要）
  - value值前有一个 空格（非集合）

- 优点：

  - 多层次结构，简洁清晰

  - 可以添加注解，且支持集合

    ```yaml
    person:
    	user:
    		- teacher
    		- student
    # Java类的属性使用集合接收，且只能使用@ConfigurationProperties注解接值
    ```





#### 读取配置文件key

> 读取配置文件中key的值赋值给属性：（需要将该组件交由IOC容器管理）

方式一：在属性上使用@value注解

```java
@Component				// 组件标识
@PropertySource("classpath:外部文件.properties")		// 引用外部文件
class 类名{
    @Value("${key}")
    private 属性类型 变量名;
    // key必须写全，且只能读取单个值，不能读取yml中的集合    
}
```



方式二：在类上使用@ConfigurationProperties注解 

```java
@Component		// 组件的标识
@ConfigurationProperties(prefix="前缀key")		// 批量配置文件key的读取
class 类名{
    private 数据类型 后缀key1;
    private 数据类型 后缀key2;
    // 必须提供get、set方法
}
/* prefix中的前缀key是指前面的n层结构key，后缀key是指最后一层的key
   为所有匹配的key且 后缀key = 变量名 的赋值，但需要属性提供get、set方法
   可为yml配置文件中的集合类型赋值 */
```





#### 多环境配置

配置文件被允许起别名（如 application-xxx.yml），这些配置文件并不会被主动加载，而是在主配置文件application.yaml中激活后才会被加载

我们就可以多写些配置文件，有测试用的，有真正部署时候用的，这时候选择需要的激活使用就可以了

并且我们不用把所有的配置信息都写在一个配置文件中，可以将一些抽取出来存放在另一个配置文件中，只需要在主配置文件中激活使用就可以了

```yml
spring:
	profiles:
		active: xxx,yyy
# xxx、yyy即为配置文件的别名，可以指定多个配置文件
# 若激活外部的配置文件时，与主配置文件的key冲突时，使用的是外部配置文件的value值
```









### 整合SpringMVC

1. 创建一个工程，声明其父工程

   ```xml
   <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>3.0.5</version>
   </parent>
   ```

2. 引用启动器依赖

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-web</artifactId>
   </dependency>
   <!-- 不需要写版本：父工程中已经声明 -->
   ```

   - 启动器：对某个功能所需要的依赖进行了整合和配置（简化依赖管理、自动配置）

   - 官方提供的启动器命名为：spring-boot-starter-*

     第三方提供的启动器命名为：*-spring-boot-starter

3. 创建一个类作为启动类，提供main方法启动项目

   ```java
   @SpringBootApplication			// 启动类标识
   class 类名{
       public static void main(String[] args){
           SpringApplication.run(类名.class,args);
       }
   }
   ```

   - @SpringBootApplication注解的含义：
     - 配置类：该注解的@SpringBootConfiguration注解，是继承于@Configuration注解的，因此该注解可以表示该类是一个配置类（因此该类中可以正常书写@Bean）
     - 自动加载其他配置类：该注解的@EnableAutoConfiguration注解，可以让其自动加载其他配置类
     - 组件扫描：该注解的@ComponentScan，可以让 该类所在的包及其子包下所有的组件 被扫描装配
   - SpringApplication.run()的作用：
     - 创建IOC容器，加载配置文件
     - 启动内置的Web服务器（如Tomcat）

4. 正常写controller层handler方法







### 整合MyBatis

1. 导入依赖

   ```xml
   <!-- JdbcTemplate、事务实现类相关的启动器 -->
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-jdbc</artifactId>
   </dependency>
   <!-- Druid启动器 -->
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>druid-spring-boot-3-starter</artifactId>
       <version>1.2.18</version>
   </dependency>
   <!-- MyBatis启动器 -->
   <dependency>
       <groupId>org.mybatis.spring.boot</groupId>
       <artifactId>mybatis-spring-boot-starter</artifactId>
       <version>3.0.1</version>
   </dependency>
   <!-- mysql驱动 -->
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>8.0.28</version>
   </dependency>
   ```

2. 编写application.yaml配置文件：

   ```yaml
   # 连接池配置
   spring:
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource		# 指定连接池的类型
       druid:
         url: jdbc:mysql://IP地址:端口号/数据库
         username: 用户名
         password: 密码
         driver-class-name: com.mysql.cj.jdbc.Driver
         
   # mybatis配置
   mybatis:
   	mapper-locations: classpath:/映射文件路径/*.xml
   	type-aliases-package: 包路径	# 为数据载体类起别名
   	configuration:
   		map-underscore-to-camel-case: true		# 下划线自动转驼峰的功能
   		auto-mapping-behavior: full				# 深层次结果集自动映射到对应属性的功能
   		log-impl: org.apache.ibatis.logging.slf4j.Slf4jImpl		# slf日志输出功能
   		# org.apache.ibatis.logging.stdout.StdOutImpl 控制台日志输出功能
   ```

3. 在启动类上指定mapper接口的位置

   ```java
   @SpringBootApplication				// 启动类标识，且有三个功能
   @MapperScan("mapper接口所在包")		// mapper接口扫描
   class 类名{
       public static void main(String[] args){
           SpringApplication.run(类名.class,args);
       }
   }
   ```

   - 在SpringBoot中，mapper接口与映射文件无需在编译后处于同个包下（相同包路径）：相当于是将mapper接口一个个引入，而不是使用包引入的方式

4. 直接通过DI注入获取mapper接口的代理对象，进行数据库操作即可

   > 若是使用JdbcTemplate来进行数据库操作，SpringBoot也会自动帮我们将JdbcTemplate存入IOC容器中。我们还是可直接通过DI注入获取到该对象，然后正常进行操作

5. 但是，由于SpringBoot3对Druid的兼容性不是很到位，因此我们还需额外再添加一个配置文件，否则会一直报错（该文件要存放于 `resouces类路径/META-INF/spring` 下，命名为：`org.springframework.boot.autoconfigure.AutoConfiguration.imports`）

   ```xml
   com.alibaba.druid.spring.boot3.autoconfigure.DruidDataSourceAutoConfigure
   <!-- 文件内容 -->
   ```







### 其他整合

#### 服务器配置

```yaml
server:
	port: 80	# 配置使用的端口号（默认为8080）
	servlet:
		context-path: /路径		# 配置项目的根路径（contextPath）
```





#### 静态资源

> 静态资源的访问设置，只需在application.yaml中配置一下即可

```yaml
spring:
	web:
		resources:
			static-locations: classpath:/文件夹名/		# 配置静态资源存储文件夹
# SpringBoot为我们提供了默认的静态资源存储文件夹，若我们创建该文件夹，并在其下存放资源，该资源会被视为静态资源，是可以直接访问的。默认的文件夹有：
#								classpath:/META-INF/resources/
#								classpath:/rsources/
#								classpath:/static/
#								classpath:/public/
# 访问时，不需要指定文件夹名（如 http://localhost/boot/static/test.png）
#						http://localhost/boot/test.png
# 一旦自己指定了默认的静态资源存储文件夹，那么SpringBoot提供的默认文件夹路径将全部失效
```





#### 拦截器

若需要用到拦截器，则只需要像SSM中使用即可（创建配置类、实现WebMvcConfigurer等等）

- 需要注意的是，只有在 启动类所在的包及其子包下 的配置类，才会被自动加载





#### 声明式事务

声明式事务只需要引入依赖，然后在需要用的地方使用@Transactional注解标识即可

> 不再需要创建事务管理器，也不再需要开启对事务注解支持的功能了

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```





#### AOP

1. 导入依赖

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-aop</artifactId>
   </dependency>
   ```

2. 定义增强类，直接声明增强方法即可，会自动应用在切入点上

   ```java
   @Component		// 存入IOC容器
   @Aspect			// 增强类标识
   class 类名{
       @增强类型("execution(切入点表达式)")
       public 返回值类型 方法名(){}
   }
   ```

   > 不需要再去开启对@Aspect注解支持的功能了





#### 声明式异常处理

- 像原先那样做即可（声明异常处理类、异常处理方法等）





#### 文件上传

- 直接添加 `MultipartFile` 形参即可，会自动注入





#### 测试环境

1. 引入依赖

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-test</artifactId>
   </dependency>
   ```

2. 在测试类上使用 @SpringBootTest 注解

   - 使用该注解后，会自动进行创建IOC容器等操作。我们只需通过DI注入获取我们需要的组件，在@Test方法中进行测试即可
   - 该测试类要确保位于启动类所在的包（或其子包）下







### 打包与部署

> 普通web项目，直接打成war包，存放在Tomcat的webapps目录下即可

SpringBoot内置了服务器软件，打包后会是jar包，通过执行命令即可部署

1. 添加插件

   ```xml
   <build>
       <plugins>
           <plugin>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-maven-plugin</artifactId>
           </plugin>
       </plugins>
   </build>
   <!-- 不添加该插件的话，会无法找到程序入口 -->
   ```

2. 执行命令：`java -jar 包名.jar`

   - 可以在 -jar 后，使用 -D 临时添加或修改一些application.yaml中的参数（重启失效）

     `-Dserver.port=端口号`：指定运行的端口号

     `-Dspring.profiles.active=xxx,yyy`：指定激活的 application-xxx.yaml 外部配置文件











## MyBatis-plus

mybatis-plus是在mybatis的基础上进行增强，可以自动生成 **单表** 的CRUD操作，并且提供了丰富的条件拼接方式，是一款全自动ORM类型的持久层框架

### 一、导入依赖

```xml
<!-- mybatis-plus启动器（导入后，不需要再额外导入mybatis启动器） -->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.5.3.1</version>
</dependency>
<!-- Druid启动器 -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-3-starter</artifactId>
    <version>1.2.18</version>
</dependency>
<!-- mysql驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.28</version>
</dependency>
```





### 二、继承接口

> mybatis-plus不再需要我们去创建mapper映射文件了，我们只需调用其封装好的方法，即可实现CRUD操作

#### mapper层

- 需要在启动类上添加@MapperScan注解，扫描mapper接口所在的包

```java
interface mapper接口 extends BaseMapper<数据载体类型>{}
// mybatis-plus将根据传入的数据载体类名，对对应的表进行操作
```

insert操作：

【int】`insert(数据载体对象)`：插入数据（传入的数据载体对象，会填充上新插入的数据的主键值）



delete操作：

【int】`deleteById(Long)`：根据id删除

【int】`deleteByMap(Map)`：根据条件删除

> 在Map中封装条件，如：age=10 ---> Map（key=age，value=10）



update操作：

【int】`updateById(数据载体对象)`：根据数据载体类中id属性值，修改对应id的数据

【int】`update(数据载体对象,Wrapper)`：根据Wrapper条件，修改对应的数据

> 当传入的实体类的某个属性值为null时，将不对表中对应的列的值进行修改



select操作：

【数据载体对象】`selectById(Long)`：根据id查找对应数据

> 在使用该方法时，若报该方法不能使用的问题（无法确定绑定的主键），则需要在实体类的主键上添加@TableId注解

【数据载体对象】`selectBatchIds(Collection)`：根据多个id（集合）查找对应数据

【数据载体对象】`selectOne(Wrapper)`：根据条件查询单条记录

【List<数据载体类型>】`selectList(Wrapper)`：查询所有符合条件的数据

【Long】`selectCount(Wrapper)`：查询符合条件的数据的条目数









#### service层

- service层实现类需要存入IOC容器中

在三层架构中，若是进行数据操作，service层可以解决的，那么就直接调用到service层解决即可，不需要再去调用mapper层

service层对比mapper层，mapper层适合复杂的CRUD操作，而service层适合进行批量的数据操作，且service层的CRUD方法默认是添加了事务的

但service层中的方法，在底层还是调用BaseMapper接口中的方法

1. 让service层的接口继承IService接口

   ```java
   interface service接口 extends IService<数据载体类型>{}
   ```

2. 让service层的实现，继承ServiceImpl实现类，实现service层接口

   ```java
   @Service		// 存入IOC容器中
   class service实现类 extends ServiceImpl<继承BaseMapper的mapper接口类型,数据载体类型> implements service接口{}
   /* 
   	IService接口中，仅提供了一半的CRUD方法的实现
   	当我们实现了service接口后，就要求我们实现IService接口中的方法
   	而ServiceImpl实现类中，提供了IService接口中的另一半CRUD方法的实现
   */
   // 一般情况下，我们都是通过service接口类型来调用service层的方法。若不继承IService接口，那么只有一半CRUD方法，且service接口中也无法调用这些方法
   
   // 在ServiceImpl底层中，根据泛型类型自动注入了BaseMapper。而我们调用的方法，实际上都是通过BaseMapper来调用
   ```

save操作：

【boolean】`saveBatch(Collection)`：批量（集合）插入数据

【boolean】`saveOrUpdate(数据载体对象)`：保存或修改数据

> 当载体对象有id属性值，则为修改数据，否则为保存数据



remove操作：

【boolean】`removeById(Long)`：根据id删除数据



update操作：

【boolean】`updateById(数据载体对象)`：根据传入的数据载体对象的id属性值，修改对应id的数据

> 当传入的属性值为null时，不对该属性所对应的列的值进行修改



get操作：

【数据载体类型】`getById(Long)`：根据id进行查询

【List<数据载体类型>】`list()`：查询所有数据

【List<数据载体类型>】`list(Wrapper)`：查询所有符合条件的







### 三、其他操作

#### 配置参数

mybatis-plus的application.yaml参数配置，与mybatis大差不差，只有些许不同：

1. mybatis-plus可以不需要指定mapper映射文件的位置，默认会引入 `resources/mapper/**` 路径下的所有mapper映射文件

2. 设置一些全局变量

   ```yaml
   mybatis-plus:
     global-config:
       db-config:
         table-prefix: 前缀
         # 设置后，所有的BaseMapper<数据载体类>所对应的表名不再是数据载体类的类名 ，而是 前缀+类名
         id-type: 主键策略			# 设置全部数据载体类使用的主键策略
         logic-delete-field: 值		# 设置已被逻辑删除 所用的值（默认值为1）
         logic-not-delete-field: 值		# 设置未被逻辑删除 所用的值（默认值为0）
         logic-delete-field: 属性名		# 设置所有的数据载体都共同使用该属性来表示数据的状态
     configuration:
       log-impl: org.apache.ibatis.logging.stdout.StdOutImpl		# 控制台输出日志信息
   ```









#### 分页查询

在启动类中添加mybatis-plus插件（分页插件）的组件

```java
@Bean
public MybatisPlusInterceptor plusInterceptor(){
    // MybatisPlusInterceptor是mybatis-plus的插件集合，将需要使用的插件加入到该集合中即可
    MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
    // 添加分页插件（指定数据库类型为mysql）
    interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
    return interceptor;
}
```

- 若是可以直接调用BaseMapper接口的 `selectPage(Page,Wrapper)` 方法进行分页

  - Page：是IPage接口的实现，用来指定分页信息（当前页码、每页显示条数），并存放分页后的信息
  - Wrapper：封装查询条件

  ```java
  Page<数据载体类型> page = new Page<>(当前页码,每页显示条目数);
  接口对象.selectPage(page,查询条件);
  // 查询并分页后，结果集会存放在page对象中
  long 变量名 = page.getCurrent();		// 获取当前页码
  long 变量名 = page.getSize();			// 获取每页显示条目数
  List<数据载体类型> 变量名 = page.getRecords();		// 获取当前页数据
  long 变量名 = page.getTotal();			// 获取总的条目数
  long 变量名 = page.getPages();			// 获取总页数
  ```

- 若不能直接调用BaseMapper接口的方法进行分页，需自定义查询语句的情况：

  1. 在mapper接口中定义方法时，该方法需要传入 `IPage<数据载体类型>` 类型的参数，且返回值类型应设置为 `IPage<数据载体类型>` 类型

     > 若mapper接口中自定义的分页查询方法，还需要传入其他参数，那么建议加上@Param注解

  2. 在mapper映射文件中，resultType设置的返回值类型为 数据载体类型，且SQL语句不能以 `;` 结尾
  
  > 满足以上条件后，当调用该方法、进行自定义的SQL语句查询后，返回的是分页了的查询结果









#### Wrapper条件构造

Wrapper：条件构造抽象类，是顶端父类

- QueryWrapper是AbstractWrapper（Wrapper子类）的子类，封装了 增删改查 的条件
- UpdateWrapper也是AbstractWrapper的子类，封装了 改 的条件

AbstractLambdaWrapper也是AbstractWrapper的资料，使用的是Lambda语法，其子类有 LambdaQueryWrapper、LambdaUpdateWrapper



##### QueryWrapper

1. 创建QueryWrapper对象

   ```java
   QueryWrapper<数据载体类> 变量名 = new QueryWrapper<>();
   ```

2. 链式调用方法拼接条件

   ```java
   like("列名","模糊字符");		// 模糊查询
   between("列名",范围1,范围2);	// 范围指定
   isNotNull("列名");			// 不为空
   isNull("列名");				// 为空
   gt("列名",值);				// 所有列的值大于指定值
   
   select("列名");				// 只查询某一列
   
   orderByAsc("列名");			// 查询后，结果按指定的列的值升序排序
   orderByDesc("列名");			// 查询后，结果按指定的列的值降序排序
   
   or();				// 接下来的那个条件，使用or拼接（默认条件都为and拼接）
   ```

3. 调用BaseMapper接口中的CRUD方法，传入Wrapper参数，获取满足条件的结果

- 若要实现动态SQL语句（条件满足才拼接条件）：

  - 链式调用的每个方法，都有一个重载方法，其内多了一个boolean类型的形参，当该值为true时，才会拼接上条件，否则不会拼接条件。如：

    ```java
    String name = null;
    QueryWrapper<数据载体类型> wrapper = new QueryWrapper<>();
    wrapper.like(StringUtils.isNotBlank(name),"class","A");
    // 判断name的值是否为空，若不是，则拼接条件
    List<数据载体类> 变量名 = mapper接口对象.selectList(wrapper);
    ```





##### UpdateWrapper

QueryWrapper可以进行条件修改，但是：

1. 需要准备实体类存放要修改的数据
2. 不能将值修改为null（存放的值为null时，不会对该属性所对应的列的值进行修改）

UpdateWrapper就可以做到无需准备实体类就可以设置列的值，还可以设置为null值

```java
UpdateWrapper<数据类型载体> wrapper = new UpdateWrapper<>();

wrapper.isNull("列名").set("列名",值)
// 设置列名为空的列名的值（可以继续链式调用set方法设置多个列）

mapper接口对象.update(null,wrapper);
```





##### Lambda

LambdaQueryWrapper、LambdaUpdateWrapper的使用，与QueryWrapper、UpdateWrapper的使用是一样的，只是在链式调用方法时，传递的不再是String类型的列名，而是使用方法引用的方式（会自动根据引用的方法，推断出对应的列名）

> 使用方法引用的方式，就不容易写错列名了

```java
LambdaQueryWrapper<数据载体类型> wrapper = new LambdaQueryWrapper();
wrapper.like(数据载体类::get方法名,"值");
// 方法引用的规则，详见Java基础
```







#### 结果集映射

##### @TableName

当我们在mapper接口中继承了BaseMapper接口时，需要指定BaseMapper接口的泛型，而该泛型即为数据载体类。同时，该数据载体类的类名，在忽略大小写后，将作为进行CRUD操作的表名

```java
@TableName("表名")
class 数据载体类{}
// 若不使用@TableName指明该数据载体类所对应的表的名字，则默认为类名
// 可以在application.yaml配置文件中，批量指定表名的前缀
```





##### @TableId

1. 当表中的主键名与属性名不一致时，手动指定属性对应的主键
2. 设置插入数据时，主键的生成方式（主键策略）

```java
@TableName("表名")
class 数据载体类{
    @TableId(value="主键列名",type=主键策略)
    private 数据类型 变量名;
}
// 可以在application.yaml配置文件中，设置所有的数据载体类使用的主键策略
```

- 主键策略的两种值：

  - AUTO：让数据库去维护主键（数据库的自增长策略）

    > 使用该方式，必须确保主键有自增长

  - ASSIGN_ID（默认）：根据雪花算法，生成一个随机的、不重复的Long类型的数字（与UUID类似）

    > 使用该方式，数据库主键应设置为bigint或varchar(64)类型，而数据载体类用Long类型去接收





##### @TableField

1. 当表中的列名与属性名不一致时，手动指定属性对应的列名
2. 设置该属性是否是对应了表中的字段

```java
@TableName("表名")
class 数据载体类{
    @TableField(value="列名",exist=true)
    private 数据类型 变量名;    
}
// true表示该属性对应表中的一个字段
```







#### 逻辑删除

删除数据时，并不是真正的将数据从表中删除，而是在创建表时，额外创建一个int类型的字段，用来表示数据的状态

当我们删除数据时，将该数据的状态置为1，表示该数据已被删除；而默认情况下，未被删除的数据的状态为0

这样，后期若是想要恢复之前删除的数据，也是可以实现的

mybatis-plus帮我们对数据状态列进行了维护：当我们进行删除操作时，会自动将删除语句改为修改语句（数据状态置为1）；当我们进行CRUD操作时，会自动只对未被删除的数据（数据状态为0的）进行操作

1. 在数据库中创建一个表示数据状态的字段（一般用int类型，指定默认值、默认数据状态）

2. 在数据载体类中创建对应属性，并使用@TableLogic标识，表示用来存储数据状态，且交由mybatis-plus维护

   ```java
   class 类名{
       @TableLogic
       private Integer 列名;
   }
   // 可以在application.yaml配置文件中，指定所有的表中的某个属性，是作为逻辑删除字段的。也可以在配置文件中，指定表示数据状态的值
   ```







#### 乐观锁

乐观锁和悲观锁是两种解决并发问题的思路：

- 悲观锁：在整个数据访问的过程中，将共享资源锁定，确保其他进程不能同时修改资源（实现：锁机制）

- 乐观锁：在修改数据时，进行冲突检测，没有出现冲突的话，则修改资源（实现：版本号）

  > 版本号：为数据添加一个版本号字段，每次修改数据前，确认当前的版本号是否与最新版本号一致，若一致才会进行数据的修改

版本号实现步骤：

1. 为表中的数据添加一个版本号字段（一般使用 int类型，且一般设置默认值为1，从1开始累加）

2. 在启动类中添加版本号更新的插件

   ```java
   @SpringBootApplication		// 启动类
   class 类名{
   	public static void main(String[] args){
   		SpringApplication.run(类名.class,args);
   	}
       
       @Bean
       public MybatisPlusInterceptor mybatisPlusInterceptor() {
           // mybatis-plus插件集合（拦截器）
           MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
           // 添加版本号更新插件
           interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
           return interceptor;
       }
   }
   ```

3. 在数据载体类中，创建与版本号列名对应的属性，并使用@Version注解标识

   ```java
   class 数据载体类{
       @Version
       private Integer 列名;
   }
   ```

4. 正常进行更新操作即可：每次修改数据前，自动查询版本号是否为最新的版本号，若不是，则不会修改数据；每次修改数据后，会自动将当前的版本号+1

   > mybatis-plus提供的自动版本号更新，仅适用于BaseMapper接口所提供的 update、updateById 方法







#### 防误删

- 防止全表误删或全表更新操作（直接抛出异常）

只需添加对应插件（拦截器）即可

```java
@SpringBootApplication			// 启动类
class 类名{
    public static void main(String[] args){
        SpringApplication.run(类名.class,args);
    }
    
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        // 添加拦截器
        interceptor.addInnerInterceptor(new BlockAttackInnerInterceptor());
        return interceptor;
    }
}
```







#### MyBatisX逆向工程

1. IDEA中，下载安装插件 MyBatisX

2. IDEA右边工具栏，Database连接数据库（首次连接时，需要下载MySQL驱动文件）

3. 连接数据库成功后，选择要逆向生成的表，右键 --> MybaitsX-Generator

   module path：选择逆向的项目

   base package：选择生成mapper、service、pojo后存放的包路径

   relative package：指定数据载体类所在的包（pojo）

   ignore table prefix：忽略掉表的某些前缀，来生成pojo类名

4. 下一步

   annotation：Mybatis-plus 3

   options：Model

   template：mybatis-plus 3

5. 完成，检查效果，进一步完善补充

