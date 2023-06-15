# 基础部分

> Maven的作用：
>
> 1. 管理jar包（jar包的下载与依赖处理）
> 2. 脱离IDE环境进行构建操作



## 下载和配置

1. 下载Maven核心程序

   - 下载官网：`maven.apache.org`

   - Download --> Files --> apache-maven-bin.zip（window版）--> 解压即可

2. 配置Maven本地仓库（存放各类jar包的地方）

   - 核心程序/conf/settings.xml 配置文件

   - 找到被注释的 \<localRepository> 标签，取消注释并在标签内部指定存放的文件的路径

     > 文件会自动创建

3. 修改默认的官方下载镜像

   ```xml
   <!-- 找到<mirrors>标签，在标签内部中，注销掉原本的<mirror>标签，插入代码 -->
   <mirror>
   	<id>nexus-aliyun</id>
       <mirrorOf>central</mirrorOf>
       <name>Nexus aliyun</name>
       <url>http://maven.aliyun.com/nexus/content/groups/public</url>
   </mirror>
   ```

4. 配置JDK使用版本

   ```xml
   <!-- 找到<profiles>标签，在标签内部插入代码 -->
   <profile>
   	<id>jdk-1.8</id>
       <activation>
       	<activeByDefault>true</activeByDefault>
           <jdk>1.8</jdk>
       </activation>
       <properties>
       	<maven.compiler.source>1.8</maven.compiler.source>
           <maven.compiler.target>1.8</maven.compiler.target>
           <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
       </properties>
   </profile>
   ```

5. 配置MAVEN_HOME环境变量

   > Maven是基于JDK来运行的，因此需要配置JAVA_HOME来找到jdk安装的位置

   - 配置MAVEN_HOME：bin目录的上一级（即：核心程序所在目录）
   - 添加PATH环境变量：配置到bin目录（`%MAVEN_HOME%\bin`）
   - 命令行界面输入：`mvn -v` 进行校验









## 知识点

### 命令结构

例如：mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes

- mvn：主命令
- archetype：子命令中的一部分，代表插件的名称
- generate：子命令中的一部分，代表插件定位的目标
- -D：指定属性，属性名与-D紧挨着

> 插件：Maven的核心程序只负责调度，不做具体的工作，而具体的工作都是由插件完成的。
>
> 目标：一个插件可以对应多个目标，而每一个目标与生命周期中的某一个环节对应。
>
> mvn clean package -Dmaven.test.skip=true：进行清理、打包操作，并跳过测试环节







### 仓库

1. 本地仓库：为当前电脑上所有的Maven工程服务

2. 远程仓库：需要联网访问

   - 局域网：自己搭建的Maven私服

   - Internet：

     - 中央仓库
     - 镜像仓库：内容和中央仓库保持一致，但能够分担中央仓库的负担，同时就近能提高用户的访问速度

     > 中央仓库和镜像仓库不要混合使用，有可能会导致jar包来源不纯，从而产生冲突







### 坐标

> Maven是通过三个向量，定位到Maven仓库中唯一一个jar包的

1. groupId：公司或组织的id
   - 通常取值为公司组织或域名的倒序，通常也会指定项目的名称
2. artifactId：一个项目或项目中一个模块的id
   - 通常取值为一个项目下的一个工程名
3. version：版本号
   - 根据所需设定
   - SNAPSHOT：快照版本，正在迭代过程中，是不稳定的版本
   - RELEASE：正式版本

```xml
<!-- 举例 -->
<groupId>javax.servlet</groupId>
<artifactId>servlet-api</artifactId>
<version>2.5</version>

<!-- 此时对应的jar包，在Maven本地仓库中的位置如下 -->
Maven本地仓库根目录\javax\servlet\servlet-api\2.5\servlet-api-2.5.jar
```







### POM

#### POM概念

POM：Project Object Model，即项目对象模型。与DOM（文档对象模型）、BOM（浏览器对象模型）类似，都是模型化思想的体现。

模型化思想：POM表示将一个工程给抽象为一个模型，再用程序中的对象，来描述这个模型，然后我们就可以用程序来管理该项目了。

POM的理念就集中在pom.xml配置文件中。





#### pom.xml文件

1. project：根标签，代表对当前工程进行配置、管理

2. modelVersion：从Maven2开始，就是4.0.0，代表当前pom.xml文件所采用的标签结构

3. groupId：坐标向量之一，代表公司或组织开发的某一个项目

   artifactId：坐标向量之一，代表项目下的某一个模块（工程）

   version：坐标向量之一，代表当前模块的版本

4. packaging：打包方式

   - jar：生成jar包。说明当前工程是一个Java工程
   - war：生成war包。说明当前工程是一个Web工程
   - pom：说明当前工程是用来管理其他工程的

5. properties：定义Maven中的属性

   - \<project.build.sourceEncoding>：构建过程中，读取源码时使用的字符集

   - 自定义属性：

     ```xml
     <属性名>属性值</属性名>
     <!-- 在后续需要使用该属性时，通过 ${属性名} 的方式即可调用 -->
     ```

6. dependencies：配置依赖信息

   - dependency：配置一个具体的依赖的信息
     - groupId、artifactId、version：坐标信息（要导入哪个jar包，就配置其所在的坐标信息即可）
     - scope：依赖可用范围
   
7. build：对项目构建的一些自定义内容

   - finalName：当前工程构建后使用的最终名称

   - plugins：配置编译信息

     - plugin：配置具体的编译信息

       ```xml
       <plugin>
           <groupId>org.apache.maven.plugins</groupId>
           <artifactId>maven-compiler-plugin</artifactId>
           <version>3.8.1</version>
           <configuration>
               <compilerArgs>
                   <arg>-parameters</arg>
               </compilerArgs>
           </configuration>
       </plugin>
       <!-- 设置Maven项目在编译Java代码时，取出方法中的参数名 -->
       ```

       







### 约定目录结构

> 为了使构建过程更加自动化，因此就要使用约定目录结构。

#### Java工程

src目录：存放源码的目录

- main：主体程序存放目录
  - java：存放java源代码
    - package包目录结构
  - resources：存放配置文件
- test：测试程序存放目录
  - java：测试java代码
    - package包目录结构

target目录：存放构建操作所输出的结果

- classes：主程序编译结果
- test-classes：测试程序编译结果
- surefire-reports：测试生成的报告





#### Web工程

src目录：存放源码的目录

- main：前端代码存放目录
  - webapp：存放WEB-INF等目录
  - java：servlet存放目录
    - package包目录结构
- test：测试程序存放目录
  - java：测试java代码
    - package包目录结构

target目录：存放构建操作所输出的结果

- 项目名.war：打包后的Web工程（同名的文件夹为war解压后的文件）







### 依赖

#### 添加依赖

1. 让Web工程依赖于Java工程

   ```xml
   <!-- 在pom.xml文件中，添加依赖关系 -->
   <dependency>
   	<groupId>com.atguigu.maven</groupId>
   	<artifactId>Project01</artifactId>
       <version>1.0-SNAPSHOT</version>
       <scope>compile</scope>
       <!-- 可选，默认即为compile -->
   </dependency>
   <!-- 添加完依赖后，应该重新打包，生成的war包中才能有所依赖的jar包 -->
   ```

2. 查看是否在war包中包含Java工程中的jar包：`target/artifactId目录/WEB-INF/lib/` 目录下

3. 查看当前工程所有的依赖关系：`mvn dependency:list`、`mvn dependency:tree`（树形查看）





#### 依赖范围

在pom.xml文件中，配置依赖关系时，使用不同的 \<srope> 表示不同的依赖范围

- 可选的依赖范围有：compile（默认）、test、provided、system、runtime、import

|          | main目录 | test目录 | 开发过程 | 部署到服务器 |
| :------: | :------: | :------: | :------: | :----------: |
| compile  |   有效   |   有效   |   有效   |     有效     |
|   test   |   无效   |   有效   |   有效   |     无效     |
| provided |   有效   |   有效   |   有效   |     无效     |

> main目录、test目录：表示在该目录下的java类，是否能够使用该依赖关系所导入的jar包
>
> 开发过程、部署到服务器：表示在开发过程中能否使用该jar包；部署到服务器后，能否使用该jar包

- compile：通常使用的第三方jar包，在项目中实际运行时用到的jar包，都会是compile范围的
- test：测试过程中使用的jar包
- provided：开发过程中需要用到的jar包，但不参与部署到服务器上，因为服务器上已有同类的jar包，要避免与服务器中的jar包产生冲突





#### 依赖传递

jar包A被jar包B依赖，jar包B被jar包C依赖，那么，只要jar包A的依赖范围为compile，则在jar包C中，就可以引用jar包A，这就是依赖传递

- 若存在jar包D依赖jar包C，那么jar包D中也可以引用jar包A
- 可以使用 `mvn dependency:tree`、`mvn dependency:list` 查看当前工程都依赖了哪些jar包





#### 依赖排除

若jar包A依赖了jar包B，jar包A又依赖了jar包C，由于依赖的传递性，就有可能jar包B中某个依赖的jar包 与 jar包C中某个依赖的jar包产生了冲突。那么，我们就需要：在jar包A 对jar包B/C 的依赖中，排除掉对jar包B/C的该jar包的依赖。

```xml
<!-- 在依赖的jar包中，排除 对该jar包中某个由依赖传递过来 的依赖 -->
<exclusions>
	<exclusion>
        <!-- 填写不需要被传递的依赖的jar包 -->
    	<groupId>XXX</groupId>
        <artifactId>XXX</artifactId>
    </exclusion>
</exclusions>
```

> jar包A排除了 由于依赖传递导致的对jar包B/C的某个jar包的依赖，jar包B/C对该被A排除的jar包的依赖依然不会发生变化。







### 继承

> 在父工程中对子工程依赖信息进行管理，可以实现一处修改、处处生效

1. 要想让工程成为父工程，就要在创建好工程后，修改其pom.xml文件中的 \<packaging> 标签值为`pom`

   > 打包方式为pom的工程，不用于写业务代码，而是用于管理其他Maven工程

2. 在该工程下创建的工程即为子工程

3. 当在父工程中创建了子工程时：

   - 父工程会多出一个标签：\<modules>，用于聚合的配置

     > 聚合：在父工程中进行聚合配置后，只需要父工程进行install，无需再去考虑install的先后顺序

   - 子工程会多出一个标签：\<parent>，用来记录父工程的信息

   - 若子工程中的 \<groupId>、\<version> 与父工程的一致时，子工程中的可以省略

4. 父工程中添加统一管理的依赖信息：

   ```xml
   <dependencyManagement>
   	<dependencies>
           <!-- 要统一管理的依赖的信息 -->
       	<dependency>
           	<groupId>XXX</groupId>
               <artifactId>XXX</artifactId>
               <version>XXX</version>
           </dependency>
       </dependencies>
   </dependencyManagement>
   ```

   - 当子工程使用被父工程统一管理的依赖时，子工程中该依赖的 \<version> 可以不需要指明，默认使用父工程中统一管理所使用的 \<version>（若指明，则会覆盖父工程中声明的 \<version>）







### 生命周期

> 执行生命周期中的其中一个环节，都会从该生命周期中的第一个环节执行

生命周期分为：

1. Clean：进行清理相关的操作
2. Site：生成站点相关信息（生成一个类似于说明文档的静态页面）
3. Default：主要的构建过程（如：compile、test、package、install等）









## 操作

> 若在操作过程中没有所需要的插件，则会进行下载，并保存到Maven本地仓库中

### 命令模式下

#### 项目创建

##### Java工程

在指定的Maven工作目录下创建Java工程：`mvn archetype:generate`

> - Choose a number or apply filter：默认即可（回车）
> - Define value for property 'groupId'：定义groupId
> - Define value for property 'artifactId'：定义artifactId
> - Define value for property 'version' 1.0-SNAPSHOT:：定义version。默认即可（回车）
> - Define value for property 'package'：定义package。默认即可（回车）
> - Y：确认信息。默认即可（回车）
>
> 出现 BUILD SUCCESS 即代表工程创建成功，该工程就会出现在我们指定的Maven工作目录下
>
> 调整：Maven默认生成的工程，对junit单元测试依赖的为3.8.1老版本，因此应该修改为较合适的4.12版本。其中，自动生成的App.java和AppTest.java类可以删除



##### Web工程

在与Java工程同级的目录下创建工程：`mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes -DarchetypeArtifactId=maven-archetype-webapp -DarchetypeVersion=1.4`

> 同样需要指定groupId、artifactId、version、package，并确认信息





#### 项目构建

> 需要进入到pom.xml配置文件所在的目录，才能执行构建命令

1. 清理：删除上一次构建的结果，为下一次构建做准备

   - `mvn clean`（效果相当于删除target目录）

2. 编译：将Java源程序编译成 .class 字节码文件

   - 编译主程序：`mvn compile`
   - 编译测试程序：`mvn test-compile`

3. 测试：运行提前准备好的测试程序

   - `mvn test`（重新编译测试程序后，再进行测试）

4. 报告：对上一步的测试结果生成一个全面的信息介绍

5. 打包：jar包（Java工程）、war包（Web工程）
   
- `mvn package`（重新测试，测试成功后才会进行打包）
  
6. 安装：将一个Maven工程打包所生成的jar包或war包存入到Maven仓库中

   - `mvn install`（存放在`groupId目录/artifactId目录/version目录`下，并将对应的pom.xml文件改名为 artifactId-version.pom 文件）

7. 部署：
   - 部署jar包：将jar包部署到Nexus私服服务器上

   - 部署war包：将war包部署到Tomcat服务器上

     > 将.war或解压后的文件夹，放到`Tomcat目录/webapps/`目录下







### IDEA中

#### 项目创建

1. New Project --> Maven --> Advanced Settings 中设置groupId、artifactId --> Create

2. 配置：

   - 自动导入依赖：File --> Settings --> Build,Execution,Deployment --> Build Tools ==> Reload project after changes in the build scripts 中选项改为 Any changes

   - 修改Maven核心程序：File --> Settings --> Build,Execution,Deployment -->Build Tools ==> Maven --> Maven home path 修改为 bin所在目录的上一级

     > 若读取的settings.xml文件和本地仓库所在位置不准确时，需要打开Overwide手动指定

3. 创建子工程：

   - New一个新的Module，指明 artifactId 即可。
   - 创建了子工程后，父工程中的pom.xml文件中，会自动生成 \<packaging> 以及 \<modules>，子工程的pom.xml文件中，会自动生成 \<parent>

4. 创建Web工程：

   - 创建一个新的子工程
   
   - 修改该工程的pom.xml文件，添加 \<packaging> 类型为 `war` 的标签
   
   - File --> Project Structure --> Facets --> Web（没有的话就手动添加）--> Deployment Descriptors（添加web.xml文件）--> Web Resource Directories（创建webapp文件）--> OK
   
     > 通常，webapp目录在 src/main 下，web.xml在 webapp/WEB-INF 下







#### 项目构建

三种方式执行Maven命令：

1. 右边工具栏 --> Maven --> 选择项目 --> Lifecycle 或 Plugins --> 双击要执行的命令即可
2. 右边工具栏 --> Maven --> m --> 输入要执行的maven命令 --> Project选择项目 --> 回车执行命令
3. 右击pom.xml文件 --> Open In --> Terminal --> 在打开的终端窗口中执行maven命令





