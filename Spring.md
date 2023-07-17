## Spring引入

### 概述

1）Spring框架，是一个分层的、面向切面的轻量级开源框架

- 轻量级：体积结构小；不需要再依赖其他组件
- 框架：已经封装好特定功能的半成品，可以提高开发效率，减少开发周期
- 两个核心模块：IOC 和 AOP
  - IOC：控制反转，将创建对象的过程交给Spring进行管理和创建
  - AOP：面向切面编程，对不连续的代码进行整合，在不修改原代码的基础上增强代码的功能



2）Spring狭义和广义上的理解：

1. 广义上：指的是以Spring  Framework为核心的Spring技术栈，其他如Spring MVC、SpringBoot等以Spring Framework为基础的子项目
2. 狭义上：特指Spring Framework，也称Spring框架。



3）Spring6要求使用 JDK17，Maven版本为3.6以上，且需要设置Maven在进行 complie 是使用的是 JDK17 进行编译（settings.xml）



4）特点：

1. 非侵入式：对本身程序的结构影响很小，不会破坏原有的结构，且能依旧使用原生的代码
2. 控制反转与容器、面向切面编程
3. 组件化：可以使用简单的组件配置组合成一个复杂的应用，由XML或Java注解来组合这些对象
4. 声明式：很多靠编写代码实现的功能，现改为只需声明需求即可由框架代为实现
5. 一站式：在IOC和AOP的基础上，可以整合其他的开源框架或第三方类库



5）五大功能模块

1. Core Container：核心容器，在Spring环境下使用任何功能，都必须基于IOC容器
2. AOP & Aspects：面向切面编程
3. Testing：对 junit 和 TestNG 测试框架的整合
4. Date Access / Integration：对 数据访问 / 集成 的功能
5. Spring MVC：面向Web应用程序的集成功能







### 第一个实例

1. 配置 JDK 环境变量；配置Maven核心配置文件 settings.xml 中 \<profile> 使用 JDK17

2. 在IDEA中，创建一个新Maven项目，创建新的子模块；设置使用的Maven核心程序、读取的settings.xml文件路径、本地仓库地址；修改IDEA中依赖为自动导入；重启IDEA

3. 引入Spring基础依赖：spring-context

4. 创建类，定义属性方法等

   ```java
   public class User {
       public void showName(){
           System.out.println("June!");
       }
   }
   // 在常规情况下，我们需要通过new对象的方式，来获取User对象，再利用对象来调用showName()方法
   // 而使用Spring，我们将不再使用new的方式获取对象，而是通过：
   /* 	
   	1.加载spring的xml配置文件，对xml文件进行解析
   	2.读取xml配置文件的<bean>标签的id属性和class属性的值
   	3.根据id属性定位到class属性所对应的类，并（利用反射，调用类的无参构造器）创建该类的对象
   	4.创建对象后，会被存放到一个Map<String,BeanDefinition> beanDefinitionMap中
   		> key存放的是id，value存放的是类的定义（类的一些描述信息）
   */
   ```

5. 依据Spring的格式要求创建配置文件（.xml，存放到类根路径下）

   ```xml
   <!-- 在New一个xml文件时，可以选择Spring Config格式的xml文件，这样会添加一定的约束 -->
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
       
       <bean id="user" class="com.atguigu.spring1.User"/>
   <!--
   	完成User对象创建所需标签：<bean> 
   		id属性：唯一标识，通过id定位到class属性
   		class属性：要创建的对象所在的类的全类名
   -->
       
   </beans>
   ```

6. 在test测试文件中测试showName()方法的调用

   ```java
   @Test
   public void test1(){
      // 加载spring的xml配置文件
      ApplicationContext context = new ClassPathXmlApplicationContext("Spring1.xml");
      // 调用ApplicationContext对象的getBean(String idName)来创建id所对应的class类的对象
      User user = (User) context.getBean("user");
      user.showName();
   }
   ```

   





## Log4j2日志框架

### 知识点

该框架由3部分构成：

1. 日志信息的优先级
   - 日志信息的优先级由低到高为：TRACE -- DEBUG -- INFO -- WARN -- ERROR -- FATAL
     - TRACE：追踪，相当于追踪程序的指向，是最低的日志级别
     - DEBUG：调试，一般在开发中设置为最低日志级别
     - INFO：信息，输出重要信息
     - WARN：警告，输出警告信息
     - ERROR：错误，输出错误信息
     - FATAL：严重错误
   - 优先级别低的会输出优先级别高的错误信息（如：设置WARN，也会输出ERROR、FATAL），但优先级别高的会屏蔽优先级别低的错误信息（如：设置DEBUG，则TRACE信息不会被输出）
2. 日志信息的输出目的地：指定信息的输出目的地是在控制台上还是存储到文件中
3. 日志信息的输出格式：控制了日志信息的显示格式和内容







### 使用方式

1. 导入依赖：log4j-core、log4j-slf4j2-impl

2. 添加 `log4j2.xml` 配置文件（文件名固定，且必须存放到类根路径下）

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <configuration>
       
       <loggers>
           <!-- 启用DEBUG级别 -->
           <root level="DEBUG">
               <!-- 与下面一一对应 -->
               <appender-ref ref="spring6log"/>
               <appender-ref ref="RollingFile"/>
               <appender-ref ref="log"/>
           </root>
       </loggers>
       
       <appenders>
           
           <!-- 设置日志信息输出到控制台 -->
           <console name="spring6log" target="SYSTEM_OUT">
               <!-- 控制台日志输出的格式 -->
               <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss SSS} [%t] %-3level %logger{1024} - %msg%n"/>
           </console>
   
           <!--
               文件输出日志信息，在每次运行时会情况，由append属性决定（适用于临时测试）
               fileName定义的是文件的保存路径，若文件不存在，会自动创建
           -->
           <File name="log" fileName="G:\Java\WorkPlace\log4j2_log\log.log" append="false">
               <PatternLayout pattern="%d{HH:mm:ss.SSS} %-5level %class{36} %L %M - %msg%xEx%n"/>
           </File>
   
           <!-- 
               文件输出日志信息，会保留所有的信息
   且每次若大小超过size，则这size大小的日志会自动存入按年份-月份建立的文件夹下面，并进行压缩作为存档
               DefaultRolloverStrategy属性设置默认最多同一文件夹下的文件数（默认为7）
           -->
           <RollingFile name="RollingFile" fileName="G:\Java\WorkPlace\log4j2_log\rollingFile.log" filePattern="log/$${date:yyyy-MM}/app-%d{MM-dd-yyyy}-%i.log.gz">
               <PatternLayout pattern="%d{yyyy-MM-dd 'at' HH:mm:ss z} %-5level %class{36} %L %M -%msg%xEx%n"/>
               <SizeBasedTriggeringPolicy size="50MB"/>
               <DefaultRolloverStrategy max="20"/>
           </RollingFile>
       </appenders>
   </configuration>
   ```

3. 在代码中输出日志信息

   ```java
   // 调用LoggerFactory.getLogger(Class printClassName)获取Logger对象
   Logger logger = LoggerFactory.getLogger(UserTest.class);
   // 调用Logger对象.info(String content)来输出日志信息
   logger.info("OK");
   
   // 最终输出的结果包含：printClassName类的全类名 + info输出的信息
   ```







## IOC

### IOC容器

#### 概念

- IOC（Inversion of Control）思想，控制反转设计思想，为了降低程序耦合度，提高程序拓展力

  - 控制反转：

    ① 将对象的创建权利交出去，由第三方容器负责

    ② 将对象与对象之间关系的维护交出去（属性注入），由第三方容器负责

- IOC容器是IOC思想的实现。Spring通过 IOC容器 来管理所有 Java对象的实例化和初始化，控制对象与对象间的依赖关系 ，将由IOC容器管理的Java对象称为Spring Bean（但本质上和new出来的对象没什么区别）

  - 依赖注入：DI（Denpendency Injection），为对象中的属性赋值，是IOC思想实现的体现

- 根据 XML配置文件（BeanDefinition，封装了Bean的定义信息），利用 BeanDefinitionReader 抽取出 Bean的定义信息，将其利用 BeanFactory工厂接口 + 反射 进行实例化和初始化，最终通过BeanFactory 接口的实现类中的方法来获取对象





#### IOC容器创建

IOC容器的创建有两种创建方式：

1. BeanFactory：IoC容器的基本实现，是Spring内部使用的接口，不提供给开发人员使用
2. ApplicationContext：BeanFactory的子接口，内含多个实现类供开发人员使用
   - ClassPathXmlApplicationContext：通过读取类路径下的XML配置文件来创建IoC容器
   - FileSystemXmlApplicationContext：通过读取文件系统路径下的XML配置文件来创建IoC容器
   - ConfigurableApplicationContext：ApplicationContext的子接口（在ApplicationContext接口的基础上，增加了启动、关闭、刷新上下文的能力）
   - WebApplicationContext：在Web环境下创建的IOC容器对象，就是WebApplicationContext类型的

通过 new ApplicationContext接口的实现类的方式，即可获取到IOC容器对象









### 获取Bean对象

ApplicationContext子接口中的方法可以获取到IOC容器中存储的Bean对象：

【Object】`getBean(String idName)`：通过xml配置文件中的id（唯一标识）获取到bean对象

【T】`getBean(Class<T> className)`：通过类（类型）获取到bean对象

【T】`getBean(String idName,Class<T> className)`：通过xml配置文件中的id和类类型获取bean对象



注意：

- 当根据类（类型）获取bean对象时，要求xml配置文件中，仅有一个 \<bean> 标签的class属性指向该类

  ```xml
  <bean id="test1" class="com.atguigu.spring6.Hello"/>
  <bean id="test2" class="com.atguigu.spring6.Hello"/>
  <!-- 通过类（类型）来获取，会报错；但通过id来获取，依然可行 -->
  ```
  
- 当根据类类型来获取bean对象时，可以通过其接口的类型来获取该bean对象，但该接口仅能有一个实现类

  ```java
  // 接口
  public interface Person{}
  
  // 实现类
  public class Student implements Person{}
  
  // 指定创建的bean对象，底层是通过反射，借助无参构造来创建bean对象的
  // 接口是没有构造器的，因此是不能用来指定为bean对象所创建时的实体类类型
  <bean id="test" class="com.atguigu.spring.Student"/>
      
  // 获取bean对象
  ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
  Person bean = context.getBean(Person.class);
  // 通过类型获取bean对象时，是可以通过接口类型来获取其唯一实现类的对象的
  ```

  - 准确来说，只要获取的对象是满足【 对象 instanceof 类型 】且被唯一指向，则都是可以获取到该对象的（如：继承父类、实现接口 等）









### 依赖注入

> 在bean对象全部加载完成后，就会进行依赖注入

#### 注入方式

1）通过属性的setXxx()方法进行赋值：

> 属性和成员变量的区别：
>
> 成员变量中拥有getXxx()、setXxx()方法，当去掉set / get，剩余的部分首字母小写，获取到的即为属性名
>
> 因此可以说，拥有get、set方法的才被称为属性，而属性和那些没有get、set方法的属性，被统称为成员变量

```xml
<!-- 要求：在该类中，要赋值的属性有标准的setXxx()方法可供调用 -->
<bean id="id名" class="全类名">
	<property name="属性名" value="属性值">
</bean>
```

2）通过构造器为属性赋值

```xml
<!-- 要求：要存在 拥有要赋值的属性 的有参构造器 -->
<bean id="id名" class="全类名">
	<constructor-arg name="属性名" value="属性值"/>
</bean>
```







#### 特殊值注入

1. 注入null值：

   ```xml
   <bean id="id名" class="全类名">
   	<property name="属性名">
       	<null/>
       </property>
   </bean>
   ```

2. 注入特殊字符：

   - 使用转义符：

     ```xml
     <bean id="id名" class="全类名">
     	<property name="属性名" value="&lt;&gt;"/>
     </bean>
     <!-- &lt;表示<符号 &gt;表示>符号 -->
     ```

   - 使用 \<![CDATA[ ]]> 包裹：

     ```xml
     <bean id="id名" class="全类名">
     	<property name="属性名">
         	<value><![CDATA[ 特殊字符 ]]></value>
         </property>
     </bean>
     <!-- 被包裹内部可书写任意特殊字符，都不会产生冲突 -->
     ```
     
     - 由于 \<![CDATA[]]> 是一个特殊的标签，是不能在属性中使用的；而 value 可以作为属性，也可以作为 property 中的一个子标签 \<value>
     - 因此，可以将 \<![CDATA[]]> 声明在 \<value> 标签中







#### 对象注入

- 外部bean引用注入：

  ```xml
  <bean id="id名1" class="全类名"/>
  
  <bean id="id名2" class="全类名">
  	<property name="属性名" ref="id名1"/>
  </bean>
  <!-- 通过ref指定要注入的对象的id值 -->
  ```

- 内部bean直接注入：

  ```xml
  <bean id="id名2" class="全类名">
  	<property name="属性名">
      	<bean class="全类名"/>
      </property>
  </bean>
  <!-- 在<property>标签内部指明要注入的对象 -->
  <!-- <property>内部的<bean>，外部无法读取到 -->
  ```

- 使用级联的方式可以修改实体类属性中的属性值：

  ```xml
  <bean id="id名1" class="实体类全类名"/>
  
  <bean id="id名2" class="全类名">
  	<property name="实体类属性名" ref="id名1"/>
      <property name="实体类属性名.属性" value="属性值"/>
  </bean>
  <!-- 使用级联的方式，在该实体类属性被赋值后，就可以直接通过 .属性 的方式为该实体类中的属性赋值 -->
  <!-- 因为要求该实体类属性必须被赋值后，才能修改属性值，因此不推荐使用 -->
  ```








#### 数组集合属性注入

1）数组类型注入：

```xml
<bean id="id名" class="全类名">
	<property name="属性名">
    	<array>
        	<value>属性值1</value>
            <value>属性值2</value>
        </array>
    </property>
</bean>
<!-- array中的数值当然可以是对象类型，详见下方举例 -->
```



2）List集合类型注入：

```xml
<bean id="id名1" class="全类名"/>

<bean id="id名3" class="全类名">
	<property name="属性名">
    	<list>
            <!-- 属性值1 -->
        	<ref bean="id名1"/>
            <!-- 属性值2 -->
            <bean id="id名2" class="全类名"/>
        </list>
    </property>
</bean>
<!-- 两种方式都可以添加对象属性的值 -->
<!-- 普通数值的方式参考上面 -->
```

- 也可以配置一个集合类型的bean，然后引用该bean对象

  ```xml
  <bean id="id名1" class="全类名">
      <property name="集合类型属性值" ref="id名2"/>
  </bean>
  <util:list id="id名2">
      <value>值1</value>
      <ref bean="值2（引用bean对象的id名）"/>
  </util:list>
  <!-- 需要注意的是：要引入util的命名空间（约束） -->
  ```



3）Map集合类型注入：

```xml
<bean id="id名1" class="全类名"/>

<bean id="id名3" class="全类名">
    <property name="属性名">
    	<map>
            <!-- 属性值1 -->
        	<entry>
            	<key>
                	<value>key值</value>
                </key>
                <ref bean="id名1"/>
            </entry>
            <!-- 属性值2 -->
            <entry>
            	<key>
                	<value>key值</value>
                </key>
                <bean id="id名2" class="全类名"/>
            </entry>
            <!-- 属性值3 -->
            <entry key="key名" value="value值"/>
        </map>
    </property>
</bean>
<!-- 当map中的属性值为普通数值时，依旧是使用<value>标签即可 -->
<!-- entry中的 key-ref/value-ref，用来引用对象类型的id值 -->
```







#### 命名空间注入

```xml
<!-- 使用命名空间前，需要先修改文档类型定义。添加后，p:属性名 才能被正常识别 -->
<!--
	在<beans>标签的文档类型定义中，添加：
		xmlns:p="http://www.springframework.org/schema/p"
-->

<bean id="id名" class="全类名" p:属性名="普通属性值" p:属性名-ref="对象属性所对应的id值"/>
```







#### 引用外部文件注入

```xml
<!-- 使用外部文件引入前，需要先修改文档类型定义。添加后，<context>标签才能被正常识别 -->
<!--
	在<beans>标签的文档类型定义中，添加：
		xmlns:context="http://www.springframework.org/schema/context"
    在 xsi:schemaLocation 末尾追加：（两者要紧挨）
    	http://www.springframework.org/schema/context
    	http://www.springframework.org/schema/context/spring-context.xsd
-->

<context:property-placeholder location="classpath:类路径下的文件名.properties"/>

<bean id="id名" class="全类名">
	<property name="属性名" value="${properties文件中的key名}"/>
</bean>
<!-- 属性值就是key对应的value值 -->
```







### bean对象的作用域及生命周期

1）作用域：

```xml
<bean id="id名" class="全类名" scope="作用域范围"/>
```

|    作用域范围     |              含义               |    创建对象时机     |
| :---------------: | :-----------------------------: | :-----------------: |
| singleton（默认） |  该bean在IoC容器中仅有一个实例  | IoC容器初始化时创建 |
|     prototype     | 该bean在IoC容器中可以有多个实例 | 获取bean对象时创建  |



2）生命周期：

1. bean对象的创建（调用无参构造器进行创建）

2. 设置bean对象的属性值（依赖注入）

3. 在初始化前，调用后置处理器中的 postProcessBeforeInitialization() 方法

   - 后置处理器类需要实现 `BeanPostProcessor`接口，并且配置到IOC容器中
   - 方法中的参数：bean（当前后置处理器处理的bean对象）、beanName（处理的bean对象的id值），若对bean进行了修改后，则需要将该bean对象进行返回
   - 后置处理器配置后，是针对当前IOC容器中所有的bean都生效的

4. bean对象进行初始化（可指定调用的初始化方法）

   - ```xml
     <bean id="id名" class="全类名" init-method="方法名"/>
     <!-- 自定义方法，然后通过init-method指定其为初始化方法 -->
     ```

5. 在初始化后，调用后置处理器中的 postProcessAfterInitialization() 方法

6. （bean对象创建完毕，可以被使用）

7. bean对象销毁（可指定调用的销毁方法）

   - ```xml
     <bean id="id名" class="全类名" destroy-method="方法名"/>
     <!-- 自定义方法，然后通过init-method指定其为初始化方法 -->
     ```

   - 当IOC容器关闭时，才会调用销毁方法

     > 当作用域为 prototype（多例）时，销毁方法的调用不再由IOC容器管理
     >
     > 通过调用 ApplicationContext 的实现类中的 `close()` 方法来关闭IOC容器








### FactoryBean

- 是一种整合第三方框架的常用机制
- 当 FactoryBean 的实现类被配置为bean时，实际上通过该bean获取到的对象是FactoryBean实现类内部方法 getObject() 所返回的对象

```java
public class A{}

public class B implements FactoryBean<A>{
    
    @Override		// 调用的为getObjec()方法，返回的对象作为被创建的bean对象
    public A getObject() throws Exception {
        return new A();
    }

    @Override		// 用来返回创建的对象的类型
    public Class<?> getObjectType() {
        return null;
    }
}
```

```xml
<bean id="b" class="com.atguigu.B" />
<!-- 
此时明面上创建的是B类的对象，但由于B实现了FactoryBean接口，所以实际上通过id获取的bean对象，是B内部的getObject()方法返回的对象（也可直接通过 方法内部返回的对象的类型 来获取对象）
-->
```









### 自动装配

使bean对象中 类类型或接口类型的属性 自动被注入（匹配 IOC容器 中的某个bean来进行注入）

- autowire 的值为 no、default 时，表示不自动装配，此时该对象中的属性使用默认值

- ```xml
  <bean id="id名" class="全类名" autowire="byType"/>
  <!-- 根据属性的类型（class类类型）来匹配IoC容器中的bean对象，并进行自动装配 -->
  ```

  >当IoC容器中，没有类型匹配的bean对象时，则属性值不装配（即为初始值）
  >
  >当IoC容器中，存在多个类型匹配的bean对象时，会报错

- ```xml
  <bean id="id名" class="全类名" autowire="byName"/>
  <!-- 根据属性的名称（id属性值）来匹配IoC容器中的bean对象，并进行自动装配 -->
  ```









### 注解方式

#### Bean对象的创建

1）将该类标记为需要被自动创建的bean对象

```java
@Component(value="id名")		// 相当于<bean id="id名" class="类的全类名"/>
public class 类名{}

// value不指定时，默认即为 类名首字母小写
```

- @Component：该注解用于描述Spring中的Bean，仅仅表示是容器中的一个组件，并可以表示在任意层次。被标记上该注解的类，会被自动装配到IoC容器中
- @Controller：该注解通常表示在Controller控制层，功能与Component一致
- @Service：该注解通常表示在Service业务层，功能与Component一致
- @Repository：该注解通常表示在Dao数据访问层，功能与Component一致



2）将方法的返回值作为IOC容器中的一个bean对象

```java
@Bean(name="id名")
public 返回值类型 方法名(){}
```

- 若不指定name的值，则默认以方法名作为Bean对象的id名
- 可以指定初始化方法和销毁方法
- 方法可以有参数，参数值会IOC容器创建后，通过属性注入的方式，自动进行值的注入







#### 组件扫描

##### XML文件方式

- 由于Spring默认是不支持注解来自动装配Bean对象的，因此，我们需要在Spring的核心配置文件（xml）中进行相关配置，开启Spring Beans的自动扫描功能

```xml
<!-- 使用组件扫描前，需要先修改文档类型定义。添加后，<context>标签才能被正常识别 -->
<!--
	在<beans>标签的文档类型定义中，添加：
		xmlns:context="http://www.springframework.org/schema/context"
    在 xsi:schemaLocation 末尾追加：（两者要紧挨）
    	http://www.springframework.org/schema/context
    	http://www.springframework.org/schema/context/spring-context.xsd
-->

<context:component-scan base-package="包路径"/>
<!-- 
    开启组件扫描功能后，会自动扫描指定的包及子包下所有的类
    若类使用了@Component注解，则会将该类装配到IoC容器中
	不能存在多个类的类型相同，且id也相同的类（需使用 @Component(value="id名") 来区分）
-->
```

扫描筛选：

1. 排除扫描指定内容：

   ```xml
   <context:component-scan base-package="包路径">
   	<context:exclude-filter type="排除类型" expression="排除类型的全类名"/>
   </context:component-scan>
   <!--
   	type="annotation"：排除指定注解，expression中指定要排除的注解的全类名
   	type="assignable"：排除指定类，expression中指定要排除扫描的类的全类名
   -->
   ```

2. 只扫描指定内容：

   ```xml
   <context:component-scan base-package="包路径" use-default-filters="false">
   	<context:include-filter type="扫描类型" expression="全类名"/>
   </context:component-scan>
   <!-- 
   	user-default-filters="false"：关闭默认扫描规则，默认扫描规则是指定包及包下所有的类
   		排除扫描指定内容时，需要设置 user-default-filters="true"（即默认值）
   		因此，只扫描指定内容，和排除扫描指定内容，是不可以同时使用的
   	type的使用与上面一样
   -->
   ```





##### 配置类方式

- 通过配置类，可以实现让类来实现组件扫描，从而真正意义上的实现全注解，脱离xml配置文件

```java
@Configuration		// 配置类
@ComponentScan("包路径")		// 开启组件扫描
public class 类名{}

// 相应的，不再读取xml配置文件，而是加载类
ApplicationContext context = new AnnotationConfigApplicationContext(类名.class);
```









#### 属性注入

##### @Autowired的方式

> 根据属性的类型，在IoC容器中找到对应的bean对象，完成属性值注入

1. 直接注入：

   ```java
   @Autowired
   private User user;
   ```

2. set方法中注入：

   ```java
   private User user;
   
   @Autowired
   public void setUser(User user){
       this.user = user;
   }
   ```

3. 构造方法中注入：

   ```java
   class People{
       private User user;
       
       @Autowired		// 当只有一个构造器时，该注入方式的 @Autowired 可以省略
       public People(User user){
        	this.user = user;
       }
   }
   ```

4. 形参上注入：

   ```java
   private User user;
   
   public void set(@Autowired User user){
       this.user = user;
   }
   ```
   
5. 使用 `@Qualifier(value="id名")` 来进行根据id名称的注入

   ```java
   @Autowired
   @Qualifier("user")		// 当仅有一个参数value时，可以省略 value 的书写
   private User user;
   ```

> 默认情况下，使用byType的方式来实现注入
>
> 若存在多个类型匹配的bean，则会自动转换为byName的方式来实现注入
>
> 若不存在任何一个类型匹配的bean，则会抛出 NoSuchBeanDefinitionException 的异常。当我们设置 @Autowired 注解的 required="false"，则不会强制该属性必须完成自动装配；在没有为该属性赋值时，则使用默认值
>
> 若通过byType和byName的方式都无法实现注入，即：存在多个类型相同、且id值与属性名不一致的情况，就可以通过 @Qualifier("id值") 的方式，手动指定 为该属性赋值 的bean的id







##### @Resource的方式

- @Resource是JDK拓展包中的，是属于JDK中的一部分（若是JDK8，则不需要进行依赖引入）。而@Autowired是Spring框架的
- @Resource注解默认是根据名称来进行注入。若没有指明id名，则将属性名作为id名；当属性名作为id名查找不到匹配的，则会自动启用通过类类型来查找

引入依赖：

```xml
<dependencies>
    <dependency>
        <groupId>jakarta.annotation</groupId>
        <artifactId>jakarta.annotation-api</artifactId>
        <version>2.1.1</version>
    </dependency>
</dependencies>
```

```java
@Resource(name="id名")
private User user;
// 没有指定name的值，则按照属性名进行bean对象的查找
// 若根据属性名未找到时，则按照属性的类类型进行查找
```









#### 手写IoC

- 用注解的方式实现bean对象的注入以及属性注入

```java
// IoC容器
private static Map<String, Object> map = new HashMap<>();

// 构造器。传入扫描的包路径，对包路径下的@Bean进行bean对象注入，对@Di进行属性注入
public MyAnnotationApplicationContext(String packageName) {
    // 将包路径的.换成/
    String packagePath = packageName.replace('.', '/');
    // 获取指定资源（包路径）在系统中的绝对路径
    URL url = MyAnnotationApplicationContext.class.getClassLoader().getResource(packagePath);
    String urlPath = url.getPath();
    // 获取统一的绝对路径地址（也就是包的上一个目录之前）
    String resolutePath = urlPath.substring(0, urlPath.length() - packagePath.length());
    // 调用方法，对标记有@Bean注解的，进行bean对象注入
    newBean(urlPath, resolutePath);
    // 调用方法，对标记有@Di注解的，进行属性注入
    diParameter(urlPath, resolutePath);
}

// 属性注入
private void diParameter(String url, String resolutePath) {
    // 获取扫描路径（包路径）下所有的文件或文件夹
    File[] files = new File(url).listFiles();
    for (File file : files) {
        // 若是文件夹，则进行递归
        if (file.isDirectory()) {
            diParameter(file.getPath(), resolutePath);
        } else {
            // 获取文件名
            String fileName = file.getName();
            // 判断文件是否是字节码文件
            if (fileName.endsWith(".class")) {
                // 截取出该类的全类名
                String filePath = file.getPath();
                String classPath = filePath.substring(resolutePath.length() - 1, filePath.length());
                String className = classPath.substring(0, classPath.lastIndexOf(".class")).replace('\\', '.');
                // 根据全类名获取Class对象
                Class clazz = Class.forName(className);
                // 获取该类中的属性
                Field[] fields = clazz.getDeclaredFields();
                for (Field field : fields) {
                    // 判断属性是否被@Di修饰，是，则进行属性注入（运行时注解才能被读取）
                    Di diAnnotation = field.getAnnotation(Di.class);
                    if (diAnnotation != null) {
                        // 判断该类是否是实现类，若是，则改为获取其接口的类名
                        Class[] clazzInterfaces = clazz.getInterfaces();
                        String clazzName = null;
                        if (clazzInterfaces.length != 0) {
                            clazzName = clazzInterfaces[0].getName();
                        } else {
                            clazzName = clazz.getName();
                        }
                        String[] tmp = clazzName.split("\\.");
                        String objNameTmp = tmp[tmp.length - 1];
                        String lastName = objNameTmp.substring(1);
                        String firstName = objNameTmp.substring(0, 1).toLowerCase();
                        // 获取该类的对象
                        Object obj = map.get(firstName + lastName);
                        // 获取要注入进该类的属性的值
                        field.setAccessible(true);
                        Object value = map.get(field.getName());
                        // 注入属性值
                        field.set(obj, value);
                    }
                }
            }
        }
    }
}

// bean对象生成
private void newBean(String url, String resolutePath) {
    // 获取路径下所有的文件或文件夹
    File[] files = new File(url).listFiles();
    for (File file : files) {
        // 若是文件夹则递归
        if (file.isDirectory()) {
            newBean(file.getPath(), resolutePath);
        } else {
            // 获取文件名
            String fileName = file.getName();
            // 若是以.class结尾的字节码文件
            if (fileName.endsWith(".class")) {
                // 获取该字节码文件的全类名
                String filePath = file.getPath();
                String classPath = filePath.substring(resolutePath.length() - 1, filePath.length());
                String className = classPath.substring(0, classPath.lastIndexOf(".class")).replace('\\', '.');
                // 根据全类名获取Class对象
                Class clazz = Class.forName(className);
                // 判断该Class是否被@Bean修饰
                Bean beanAnnotation = (Bean) clazz.getAnnotation(Bean.class);
                if (beanAnnotation != null) {
                    // 若该Class也不是接口
                    if (!clazz.isInterface()) {
                        // 创建该类对象
                        Constructor constructor = clazz.getDeclaredConstructor();
                        constructor.setAccessible(true);
                        Object obj = constructor.newInstance();
                        // 设置id名，若是实现类，则id名设置为其接口名
                        Class[] clazzInterfaces = clazz.getInterfaces();
                        String idNameTmp = null;
                        if (clazzInterfaces.length != 0) {
                            String[] tmp = clazzInterfaces[0].getName().split("\\.");
                            idNameTmp = tmp[tmp.length - 1];
                        } else {
                            idNameTmp = fileName.substring(0, fileName.length() - ".class".length());
                        }
                        String idNameFirst = ((idNameTmp.toCharArray())[0] + "").toLowerCase();
                        String idNameLast = idNameTmp.substring(1);
                        String idName = idNameFirst + idNameLast;
                        // 将bean对象存入IoC容器中
                        map.put(idName, obj);
                    }
                }
            }
        }
    }
}
```









## AOP

### 代理模式

#### 介绍

使用代理的原因：

1. 在方法内部中，除了核心代码，还夹杂了一些非核心代码
2. 若是面向对象思想，则是将非核心代码抽取出来，但该方式仅是适用于抽取一段连续的代码

代理模式：通过一个代理类，在调用该代理类的方法时，间接的去调用目标方法

- 这样既能实现解耦，添加附加功能，又不会对目标核心代码产生影响



场景模拟：

- 接口

```java
public interface Calculator {
    // 加法
    public abstract int add(int a,int b);
    // 减法
    public abstract int minus(int a,int b);
}
```

- 实现类

```java
// 实现功能的实现类
public class CalculatorImpl implements Calculator {
    
    // 加法（目标方法）
    @Override
    public int add(int a, int b) {
		// 核心代码
        return a + b;
    }

    // 减法（目标方法）
    @Override
    public int minus(int a, int b) {
        // 核心代码
        return a - b;
    }
}
```







#### 静态代理

```java
// 代理类要实现与被代理的目标类相同的接口（为了确保代理类中的功能与被代理的目标类的功能一致）
public class CalculatorProxy implements Calculator {

    // 被代理的目标类的对象
    private Calculator target;

    // 提供一个有参构造，对被代理的目标类的对象进行赋值
    public CalculatorProxy(Calculator target) {
        this.target = target;
    }

    // 代理方法：加法
    @Override
    public int add(int a, int b) {
        // 增强代码
        System.out.println("执行加法运算");
        System.out.println("参数值：" + a + "," + b);
        // 核心代码
        int res = target.add(a, b);
        // 增强代码
        System.out.println("执行结果：" + res);
        return res;
    }

    // 代理方法：减法
    @Override
    public int minus(int a, int b) {
        // 增强代码
        System.out.println("执行减法运算");
        System.out.println("参数值：" + a + "," + b);
        // 核心代码
        int res = target.minus(a, b);
        // 增强代码
        System.out.println("执行结果：" + res);
        return res;
    }
}
```

> 创建一个代理类，代理计算器的加减功能：
>
> 1. 通过代理类中的属性和有参构造，获取到被代理的目标类的对象，利用该对象来调用核心代
> 2. 在核心代码执行前或执行后，进行些额外的操作







#### 动态代理

动态代理的动态性体现在：让其动态的为我们生成一个代理类

- 动态代理分为：JDK动态代理、cglib动态代理
- 当目标类有接口时，可以使用JDK动态代理或cglib动态代理；当目标类没有实现接口时，只能使用cglib动态代理
- JDK动态代理生成的代理类，会被存放到com.sun.proxy包下，类名为$proxyX（X表示随机数字），并且和目标类实现相同的接口；cglib动态代理生成的代理类，会与目标类存放在同一个包下，且会继承目标类

JDK动态代理：该技术的代理类和目标类会实现相同的接口，因此被代理的目标类就必须实现接口

cglib动态代理：通过继承被代理的目标类来实现代理

AspectJ：AOP思想的一种实现，将代理逻辑，写入 被代理的目标类编译后得到的字节码文件中，从而实现的动态代理（本质上是静态代理），而Spring只是借用了其中的注解



【Object】（static）`Proxy.newProxyInstance(ClassLoader,Class[],InvocationHandler)`：实现动态代理的关键方法，返回一个动态生成的代理类对象

- ClassLoader：用来加载要被代理的目标类的类加载器
- Class[]：要被代理的目标类所实现的所有接口（确保代理类所能实现的功能与被代理的目标类的功能一致）
- InvocationHandler：接口，对（被调用的）被代理的目标类所实现的所有接口中的某个抽象方法 进行实现。实现后，调用该方法时，实际上调用的是内部的抽象方法 invoke()
  - invoke()方法有三个参数：
    - Object proxy：代理对象
    - Method method：所实现的方法（利用反射可调用指定对象中的该方法）
    - Object[] args：所实现的方法所需要的参数



```java
// 通过该类，动态获取到一个代理类对象
// 通过该代理类对象（因为实现了与被代理的目标类相同的接口）来间接调用目标方法
public class ProxyFactory {
    
    // 被代理的目标类的对象
    private Object target;

    // 通过提供一个有参构造器，为被代理的目标类的对象进行赋值
    public ProxyFactory(Object target) {
        this.target = target;
    }

    // 自定义方法，通过该方法来获取到一个动态生成的代理类对象
    public Object getProxyObj() {
        
        // 获取Proxy.newProxyInstance()方法所需要的参数
        // 加载被代理的目标类的类加载器
        ClassLoader classLoader = this.getClass().getClassLoader();
        // 被代理的目标类所实现的所有接口，用于确保代理类的功能与被代理的目标类的功能一致
        Class[] interfaces = target.getClass().getInterfaces();
        // 调用某个功能（即接口中的方法）时，代理类会去实现该方法，并间接去调用目标方法
        InvocationHandler invocationHandler = new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println("调用方法：" + method.getName());
                int res = (int) method.invoke(target, args);
                System.out.println("执行结果：" + res);
                return res;
            }
        };
        
        // 该方法调用后，就会动态的为我们生成一个代理类对象
        return Proxy.newProxyInstance(classLoader, interfaces, invocationHandler);
    }
}

/* 
该代理类对象返回值时Object类型
由于我们已经确定了被代理的目标类的类型，而在动态生成代理类时，我们需要实现与被代理的目标类相同的接口
因此，我们可以直接将该Object类型的返回值，直接强转为实现的接口类型，就可以实现调用（代理）某个功能（方法）
*/
```







### 概念介绍

- AOP是一种面向切面的设计思想，是对面向对象编程的一种补充和完善。
- 提取非核心代码，封装并在对应位置执行。即：在不修改源代码的情况下，给程序动态且统一的添加额外功能



横切关注点：从目标方法中抽取出来的非核心代码



增强（通知）：将横切关注点中的内容封装为一个方法，此方法称为通知方法

- 通知可根据核心代码的执行顺序，分为五种类型：
  - 前置通知：在核心代码执行前执行
  - 返回通知：在核心代码成功执行后 执行
  - 异常通知：在核心代码出现异常时执行
  - 后置通知：在核心代码最终执行结束后执行
  - 环绕通知：上面四种通知都要执行（在try-catch-finally中执行）



切面：封装通知方法的类



目标：被代理的对象

代理：根据目标对象，应用通知后，所创建的代理对象



连接点：抽取非核心代码的位置（即横切关注点的位置）

切入点：定位连接点的方式







### 基于注解的AOP实现

> 基于IOC所需依赖的基础上，引入依赖：spring-aspects

1）将应用通知的目标类的对象存入到IOC容器中



2）开启aspectj自动代理（自动代理目标对象）

```xml
<!-- 在开启aspectj自动代理前，需要先修改文档类型定义。添加后，<aop>标签才能被识别 -->
<!-- 
    在<beans>标签的文档类型定义中，添加：
        xmlns:aop="http://www.springframework.org/schema/aop"
    在 xsi:schemaLocation 末尾追加：（两者要紧挨）
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
-->

<!-- 开启自动代理 -->
<aop:aspectj-autoproxy/>
```



3）设置切面类以及通知方法，并通过切入点表达式设置通知方法作用的位置

```java
@Aspect			// 标记该类为切面类
@Component		// 需要将切面类也存放进IoC容器中，需要确保开启组件扫描
public class 类名{
    
// 前置通知：在被代理的目标方法执行前执行
    @Before(value="切入点表达式")
    public void 方法名(){}
    
    
// 返回通知：在被代理的目标方法成功结束后执行
    // 可以设置获取代理的方法调用后的返回值：returning="返回值变量名"
    // 若设置了获取返回值后，则该方法需添加形参，形参名应与返回值变量名相同
    @AfterReturning(value="切入点表达式",returning="返回值变量名")
    public void 方法名(返回值类型 返回值变量名){}
    
    
// 异常通知：在被代理的目标方法异常结束后执行
    // 可以设置获取代理的方法出现异常后的异常信息：throwing="异常变量名"
    // 若设置了获取异常信息后，则该方法需添加Exception或Throwable类型的形参，形参名应与异常变量名相同
    @AfterThrowing(value="切入点表达式",throwing="异常变量名")
    public void 方法名(Throwable 异常变量名){}
    
    
// 后置通知：在被代理的目标方法最终结束后执行
    @After(value="切入点表达式")
    public void 方法名(){}
    
    
// 环绕通知：上面四种通知都要执行
    @Around(value="切入点表达式")
    public Object 方法名(ProceedingJoinPoint 变量名){
        
        Object 返回值变量名;
        
        try{
            
        // 此处执行代码，相当于前置通知
            
            返回值变量名 = 变量名.proceed();
            // 去执行目标方法，并接收返回的结果
            // 若目标方法的执行结果是有返回值的，则该通知方法必须将该目标方法执行后的结果返回
            // 与动态代理有相同的原因：代理类所代理方法，其结构上是一致的（为了确保实现的功能相同）
    		
        // 此处执行代码，相当于返回通知
            
        }catch(Throwable e){
    	
        // 此处执行代码，相当于异常通知

        }finally{
    		
        // 此处执行代码，相当于后置通知
        
        }
    }
    
    return 返回值变量名;
}
```

- 通知的执行顺序：

  - Spring5.3x以前：前置通知 --> 目标方法 --> 后置通知 --> 返回通知或异常通知
  - Spring5.3x以后：前置通知 --> 目标方法 --> 返回通知或异常通知 -->  后置通知

- 切面的优先级：决定切面中通知方法的执行顺序

  - 使用`@Order(整型值)`注解，可控制切面的优先级：数值越小，优先级越高

- 切入点表达式：

  `execution(代理的方法的权限修饰符 返回值类型 方法所在的类的全类名.方法名(参数列表))`

  - 可用一个 * 表示任意的权限修饰符和返回值类型

    可用 * 来表示任意的一级包名或类名

    可用 * 表示方法名任意，用 get* 表示方法名以 get开头

    可用 (..) 表示形参列表任意

- 复用切入点表达式

  ```java
  @Pointcut("切入点表达式")
  public void 方法名(){}
  
  // 后续需要引用该切入点表达式，则直接通过 方法名()（当前类中使用） 或 全类名.方法名()（其他类中使用） 来引用
  ```
  
- JoinPoint类型参数：可获取横切关注点（抽取非核心代码）所在的方法的信息

  - 【Signature】`JoinPoint对象.getSignature()`：获取横切关注点所在的方法的签名（声明）信息
    - 【String】`Signature对象.getName()`：获取签名信息中的方法名
  - 【Object[]】`JoinPoint对象.getArgs()`：获取横切关注点所在的方法的参数列表
  
- ProceedingJoinPoint类型参数：可调用目标方法（环绕通知中使用）

  - 【String】`ProceedingJoinPoint对象.getSignature().getName()`：获取横切关注点所在的方法的方法名
  - 【Object[]】`ProceedingJoinPoint对象.getArgs()`：获取横切关注点所在的方法的参数列表
  - 【Object】`ProceedingJoinPoint对象.proceed()`：执行目标方法，并获取其返回值



4）需要注意的是：

1. AOP底层使用的是静态代理的方式，为目标类生成了代理对象（看起来实现的方式为动态代理）。与代理类似：若直接调用目标方法，是没有代理效果的
2. 因此，虽然我们将目标类的bean对象存入了IOC容器中，但是，由于该对象应用了通知（被代理），因此，是无法直接通过 getBean() 来获取到该bean对象的（但与代理不同的是：直接调用目标对象，会抛出异常NoSuchBeanDefinitionException）
3. 但是，与代理一样，会实现与目标类相同的接口。因此，我们可以通过获取目标类所实现的接口的Class，来获取到bean对象，从而调用被通知的方法







### 基于XML的AOP实现

1）引入依赖



2）定义切面类以及增强方法



3）配置XML文件

```xml
<!-- 在开启aspectj自动代理前，需要先修改文档类型定义。添加后，<aop>标签才能被识别 -->
<!-- 
    在<beans>标签的文档类型定义中，添加：
        xmlns:aop="http://www.springframework.org/schema/aop"
    在 xsi:schemaLocation 末尾追加：（两者要紧挨）
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
-->

<!-- 首先，需先确保IoC容器已创建，并且bean对象已被注入 -->
<aop:config>
    
    <!-- 配置切面类 -->
    <aop:aspect ref="切面类的bean对象id名">
        
        <!-- 配置切入点 -->
        <aop:pointcut id="切入点id名" expression="切入点表达式"/>
    	
        <!-- 配置五种通知类型 -->
        <!-- 前置通知，其他也类似 -->
    	<aop:before method="切面类中的方法" pointcut-ref="切入点id名"/>
    </aop:aspect>
</aop:config>
```









## 整合Junit

> 在进行单元测试时，我们还需要通过ApplicationContext来获取到bean对象，这些步骤可以依赖Spring的整合功能而省略

1）引入依赖：spring-test、junit-jupiter-api（Junit5）/ junit（Junit4）



2）在测试类上声明注解

```java
// Junit5：
	// 方式一
@SpringJUnitConfig(locations = "classpath:xml文件路径")
public class TestJunit5{
    @Autowired
    属性类型 属性名;
}

	// 方式二
@ExtendWith({SpringExtension.class})
@ContextConfiguration("classpath:xml文件路径")
public class TestJunit5{
    @Autowired
    属性类型 属性名;
}

// Junit4
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:test3.xml")
public class TestJunit4{
    @Autowired
    属性类型 属性名;
}
```

- 根据注解，读取xml配置文件后，扫描并注入所有的bean对象，然后进行整合，再注入进属性中，就可以直接通过属性调用，而不需要ApplicationContext对象来获取bean对象









## JdbcTemplate

> 封装 JDBC（对数据库的增删改查）操作

### 获取对象

1）引入依赖：spring-jdbc、mysql-connector-java、druid



2）引用外部properties文件，创建连接池对象

```xml
<!-- 使用外部文件引入前，需要先修改文档类型定义。添加后，<context>标签才能被正常识别 -->
<!--
	在<beans>标签的文档类型定义中，添加：
		xmlns:context="http://www.springframework.org/schema/context"
    在 xsi:schemaLocation 末尾追加：（两者要紧挨）
    	http://www.springframework.org/schema/context
    	http://www.springframework.org/schema/context/spring-context.xsd
-->

<!-- 引入外部properties文件 -->
<context:property-placeholder location="classpath:文件名.properties"/>

<!-- 创建druid数据库连接池对象 -->
<bean id="druid连接池对象id名" class="com.alibaba.druid.pool.DruidDataSource">
	<property name="driverClassName" value="${ properties文件对应的key }"/>
    <property name="url" value="${ properties文件对应的key }"/>
    <property name="username" value="${ properties文件对应的key }"/>
    <property name="password" value="${ properties文件对应的key }"/>
</bean>
```



3）创建JdbcTemplate对象，绑定数据库连接池

```xml
<bean id="JdbcTemplate对象id名" class="org.springframework.jdbc.core.JdbcTemplate">
	<property name="dataSource" ref="druid连接池对象id名"/>
</bean>
```





### 增删改查

【int】`JdbcTemplate对象.update(String sql,Object...args)`：进行增删改等操作，返回影响行数

- String sql：预编译的SQL语句
- Object...args：填充占位符



【T】`JdbcTemplate对象.queryForObject(String sql,RowMapper<T>,Object...args)`：查询操作，返回一条数据

【List\<T>】`JdbcTemplate对象.query(String sql,RowMapper<T>,Object...args)`：查询操作，返回List\<T>数据载体集合

- String sql：预编译的SQL语句
- RowMapper\<T>：接口，需要重写其内部方法 `T mapRow(ResultSet rs, int rowNum)`
  - T：封装结果集的类类型
  - ResultSet rs：第n行的结果集（相当于已直接调用了 next() ）
  - int rowNum：值为 n-1
- Object...args：填充占位符

> RowMapper参数值，可以使用其实现类：`BeanPropertyRowMapper<>(Class)`
>
> - 该类可以自动替我们将结果集封装到指定的数据载体类中（由Class指定）
> - 但是需要提供无参构造器，以及要封装的字段所对应的get、set方法



【T】`JdbcTemplate对象.queryForObject(String sql,Class<T>)`：查询操作，返回单个结果

- String sql：预编译的SQL语句
- Class\<T>：包装类的Class对象

> 查询如：COUNT(*)、MAX(salary) 等返回单个结果的









## 事务

### 全注解实现

1）创建配置类，开启扫描注入，开启事务功能

```java
@Configuration		// 声明当前类为配置类
@ComponentScan("包路径")		// 开启扫描注入功能
@EnableTransactionManagement		// 开启事务管理功能
public class 类名{}
```

> ```xml
> <tx:annotation-driven transaction-manager="事务管理器id名"/>
> <!-- 与 @EnableTransactionManagement 功能一样，开启事务管理功能 -->
> 
> <!-- 使用<tx>前，需要先修改文档类型定义。添加后，<tx>标签才能被正常识别 -->
> <!-- 
>     在<beans>标签的文档类型定义中，添加：
>         xmlns:tx="http://www.springframework.org/schema/tx"
>     在 xsi:schemaLocation 末尾追加：（两者要紧挨）
>         http://www.springframework.org/schema/tx
>         http://www.springframework.org/schema/tx/spring-tx.xsd
> -->
> ```



2）创建数据源DataSource对象，配置事务管理器，创建JdbcTemplate对象

```java
// 创建数据源
@Bean
public DataSource getDataSource() {
    InputStream is = this.getClass().getClassLoader().getResourceAsStream("类路径下的properties文件");
    Properties properties = new Properties();
    properties.load(is);
    DruidDataSource druidDataSource = new DruidDataSource();
    druidDataSource.setDriverClassName(properties.getProperty("driverClass值"));
    druidDataSource.setUrl(properties.getProperty("url值"));
    druidDataSource.setUsername(properties.getProperty("username值"));
    druidDataSource.setPassword(properties.getProperty("password值"));
    return druidDataSource;
}

// 配置事务管理器，绑定数据源
@Bean
public DataSourceTransactionManager dataSourceTransactionManager(DataSource dataSource) {
    DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
    dataSourceTransactionManager.setDataSource(dataSource);
    return dataSourceTransactionManager;
}

// 创建JdbcTemplate对象
@Bean
public JdbcTemplate jdbcTemplate(DataSource dataSource) {
    JdbcTemplate jdbcTemplate = new JdbcTemplate();
    jdbcTemplate.setDataSource(dataSource);
    return jdbcTemplate;
}
```



3）开启事务

```java
@Transactional			// 开启事务，根据事务管理器，对指定的数据源进行事务管理
public class 类名{}
```

- @Transactional修饰类时，该类中所有的方法都存在事务

  修饰方法时，仅该方法存在事务

@Transactional属性介绍：

- transactionManager = "事务管理器id名"
- readOnly = true：只读，只能进行查询操作，不能进行增删改等操作
- timeout = 秒数：以秒为单位，方法执行超时时，自动回滚
- noRollbackFor、noRollbackForClassName：对指定的异常不进行回滚
- propagation：设置传播行为
  - propagation = Propagation.REQUIRED（默认）：有两层事务，外层事务与内层事务。这种情况下：只要内层事务出现异常，内外层事务都会进行回滚（只有外层事务全部执行完，且没有出现过异常，事务才不会回滚）
  - propagation = Propagation.REQUIRES_NEW：有两层事务，外层事务与内层事务。这种情况下：内层事务出现异常时，会进行回滚，但外层事务已执行完毕的，不会进行回滚









### XML文件实现

1）开启扫描注入功能

```xml
<context:component-scan base-package="包路径"/>
```



2）引入properties文件，创建DataSource数据源，创建JdbcTemplate对象

```xml
<!-- 引入properties文件 -->
<context:property-placeholder location="jdbc.properties"/>

<!-- 创建DataSource数据源 -->
<bean id="DataSource数据源id名" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="${driverClass值}"/>
    <property name="url" value="${url值}"/>
    <property name="username" value="${username值}"/>
    <property name="password" value="${password值}"/>
</bean>

<!-- 创建JdbcTemplate对象 -->
<bean id="JdbcTemplate对象id名" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="DataSource数据源id名"/>
</bean>
```



3）配置事务管理器

```xml
<bean id="事务管理器id名" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="DataSource数据源id名"/>
</bean>
```



4）使用xml配置文件开启事务管理，实质上是使用AOP思想，根据切入点，在指定的方法集上进行增强（事务管理）

```xml
<tx:advice id="要被事务管理的方法集id名" transaction-manager="事务管理器id名">
    <tx:attributes>
        <tx:method name="*"/>	<!-- 这些标签代表：要被管理的方法的方法名 -->
    </tx:attributes>
</tx:advice>

<aop:config>
    <aop:pointcut id="切入点id名" expression="切入点表达式"/>
    <aop:advisor advice-ref="要被事务管理的方法集id名" pointcut-ref="切入点id名"/>
</aop:config>
<!-- 使用到了AOP，就需要引入对应的依赖 -->
```









## 资源处理

### Resource接口

> 依赖于spring-context，位于 org.springframework.core.io包 下，用于抽象对低级资源的访问

接口中的一些常用方法：

- 【URI】`getURI()`：获取文件的URI地址
- 【String】`getFileName()`：获取文件的文件名
- 【String】`getDescription()`：获取文件的信息
- 【InputStream】`getInputStream()`：获取文件的输入流对象



接口的实现类：

|        类名        |               获取实例                |                          描述                           |
| :----------------: | :-----------------------------------: | :-----------------------------------------------------: |
|    UrlResource     |    `new UrlResource(String path)`     | 通过URL的绝对路径来操作网络资源（如 http / ftp / file） |
| ClassPathResource  | `new ClassPathResource(String path)`  |                 操作类路径下的资源文件                  |
| FileSystemResource | `new FileSystemResource(String path)` |                操作文件系统中的资源文件                 |







### ResourceLoader接口

> 该接口的实现类的实例，是一个Resource对象

该接口中仅有一个方法：

【Resource】`getResource(String path)`：根据调用该方法的实现类的类型，获取到一个对应的Resource实现类的实例

- ApplicationContext的所有实现类都实现了ResourceLoader接口，因此通过ApplicationContext实现类的对象，调用该方法可以获取到对应的Resource对象

  ```java
  ApplicationContext context = new FileSystemXmlApplicationContext();
  Resource resource = context.getResource("系统文件资源路径");
  // 此时得到的，就是Resource的实现类FileSystemResource类型的对象
  
  ApplicationContext context = new ClassPathXmlApplicationContext();
  Resource resource = context.getResource("类路径下资源");
  // 此时得到的，就是Resource的实现类ClassPathResource类型的对象
  ```

> 因此，我们一般不会直接调用Resource实现类，而是通过该方法来获取需要的Resource实现类







### ResourceLoaderAware接口

- 当实现了该接口的实现类的实例，被部署到了IoC容器中，那么Spring容器会将自身当成ResourceLoader，作为其方法`setResourceLoader(ResourceLoader)`的参数

  ```java
  // ResourceLoaderAware接口的实现类
  public class LoaderAware implements ResourceLoaderAware {
      private ResourceLoader resourceLoader;
  
      @Override
      public void setResourceLoader(ResourceLoader resourceLoader) {
          this.resourceLoader = resourceLoader;
      }
  
      // get方法
      public ResourceLoader getResourceLoader() {
          return resourceLoader;
      }
  }
  
  
  <!-- 将实现类部署到IoC容器中 -->
  <bean id="loaderAware" class="com.atguigu.test1.LoaderAware"/>
  
   
  // 测试
  public static void main(String[] args) {
      ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
      
      LoaderAware loaderAware = context.getBean("loaderAware",LoaderAware.class);
      ResourceLoader resourceLoader = loaderAware.getResourceLoader();
      
      System.out.println(context==resourceLoader);		// true
      // ApplicationContext本身也是实现了ResourceLoader接口的
      // 对比确定，Spring容器确实作为了setResourceLoader方法的参数
  }
  ```







### 其他知识点

#### classpath通配符

> 在填写资源路径时，是可以明确指定资源是位于哪个路径下的，如：
>
> ClassPathXmlApplicationContext("classpath:xxx.xml")
>
> 就意味着锁定资源在classpath（类路径）下

- `classpath*:xxx.xml`：表示匹配类路径下，所有的名为xxx.xml的配置文件，将它们分别加载，最后合并为一个ApplicationContext（仅在ApplicationContext中生效，Resource中无效）

  > 同时，文件名也可以为`xxx*.xml`，那么就会加载以xxx开头的所有xml文件









## 国际化

### Java中实现国际化

1. 创建配置文件

   配置文件命名要求：basename_language_country.properties

   - basename：基础名字，可随意，但不同语言的配置文件的basename要求相同
   - language：语言
   - country：国家

   > 如：messages_zh_CN.properties

2. 利用 java.util.ResourceBundle 类，来获取配置文件中的内容

   - 先获取到ResourceBundle对象：

     【ResourceBundle】（static）`ResourceBundle.getBundle(String baseName,Locale)`

     - String baseName：配置文件的basename
     - Locale：位于java.util包下，通过构造器 `Locale(String language,String country)` 来指定文件所属的语言以及环境

   - 通过ResourceBundle对象，读取配置文件中指定的key所对应的value值

     【String】`ResourceBundle对象.getString(String keyName)`

```java
// 配置文件：test_zh_CN.properties
title=这里是中国

// 配置文件：test_en_GB.properties
title=This is English
    
// 测试
public void test1(){
    ResourceBundle bundle = ResourceBundle.getBundle("test", new Locale("zh","CN"));
    String title = bundle.getString("title");
    System.out.println(title);		// 这里是中国
}
public void test2(){
    ResourceBundle bundle = ResourceBundle.getBundle("test", new Locale("en","GB"));
    String title = bundle.getString("title");
    System.out.println(title);		// This is English
}
```







### Spring中实现国际化

> Spring中的国际化，是通过 org.springframework.context包 下的MessageSource接口来规范的
>
> - ResourceBundleMessageSource是该接口的一个实现类，是基于java的ResourceBundle基础类实现的

1. 引入spring-context依赖，创建配置文件

   ```xml
   <!-- 配置文件：test_zh_CN.properties -->
   title=你好，这里是{0}，现在是{1}
   
   <!-- 配置文件：test_en_GB.properties -->
   title=Hello,this is {0},now it is {1}
   
   <!-- {0}、{1}是动态填充的值 -->
   ```

2. 注入ResourceBundleMessageSource对象

   ```xml
   <bean id="id名" class="org.springframework.context.support.ResourceBundleMessageSource">
       <!-- 设置读取的配置文件的basename -->
       <property name="basename" value="test"/>
       <!-- 设置读取配置文件时，使用的编码集 -->
       <property name="defaultEncoding" value="utf-8"/>
   </bean>
   ```

3. 获取配置文件中的内容

   ```java
   // 加载Spring配置文件
   ApplicationContext context = new ClassPathXmlApplicationContext("配置文件.xml");
   
   // 设置动态填充的值
   Object[] parameters = {"中国", LocalDateTime.now()};
   
   // 调用方法获取key所对应的value值
   String title = context.getMessage(String keyName, parameters, Locale);
   ```











## 数据校验

### 实现接口方式

> 通过实现 org.springframework.validation.Validator 接口的方式实现

1. 引入依赖：hibernate-validator、jakarta.el、spring-context

2. 创建要被校验的类

3. 创建一个校验器（让类实现Validator接口）

   ```java
   public class 校验器名 implements Validator {
   
       // 该方法用于：判断是否是要进行校验的类
       @Override
       public boolean supports(Class<?> clazz) {
           return 被校验的类名.class.equals(clazz);
       }
    
       // 该方法用于：设置校验规则
       @Override
       public void validate(Object target, Errors errors) {	
       // target是要被校验的目标对象，errors是Errors对象
          
   		// 根据需要进行逻辑判断。若不满足需要的，则进行报错
   		errors.rejectValue("出错属性名","错误码","错误信息");
       }
   }
   ```

4. 测试

   ```java
   // 1.创建一个DataBinder对象，并在创建对象时，指定要被校验的数据对象
   DataBinder binder = new DataBinder(Object);
   
   // 2.设置使用的校验器
   binder.setValidator(Validator);
   
   // 3.执行校验
   binder.validate();
   
   // 4.获取校验结果
   BindingResult res = binder.getBindingResult();
   
   // 5.获取校验结果中，所有的错误信息
   List<ObjectError> errors = res.getAllErrors();
   ```








### 注解方式

1. 创建LocalValidatorFactoryBean对象

   ```java
   // 采用配置类的方式实现
   @Configuration
   @ComponentScan("包路径")
   public class 类名{
       
       // 注入LocalValidatorFactoryBean对象
       @Bean
       public LocalvalidatorFactoryBean 方法名(){
           return new LocalvalidatorFactoryBean();
       }
   }
   ```

2. 创建需要被校验的数据载体类，为需要被校验的属性设置对应的注解

   ```java
   public class 被校验的类名{
       @注解
       属性名 属性值;
   }
   ```

   常用的注解：

   - `@NotNull`：限制必须不为null
   - `@NotEmpty`：只作用于字符串，限制字符串不能为空，且长度不能为0
   - `@NotBlank`：只作用于字符串，限制字符串不能为空，且被去除首尾空格后也不为空
   - `@DecimalMax(value)`、`Max(value)`：限制必须为一个不大于指定值的数字
   - `@DecimalMin(value)`、`Min(value)`：限制必须为一个不小于指定值的数字
   - `@Pattern(value)`：限制必须符合指定的正则表达式要求
   - `@Size(max,min)`：限制字符串长度必须在min到max之间
   - `@Email`：验证注解的元素值是Email

3. 进行校验

   方式一：jakarta.validation.Validator 下的Validator

   ```java
   // 加载配置类
   ApplicationContext context = new AnnotationConfigApplicationContext(Class);
   
   // 获取LocalValidatorFactoryBean对象
   Validator v = context.getBean("id名", Validator.class);
   
   // 调用方法，校验数据，返回一个Set类型的集合，内部是所有的错误信息
   Set<ConstraintViolation<User>> res = v.validate(new User());
   
   // 判断Set集合是否为空：
   	// 当集合为空，说明没有错误信息，则数据满足需求
   	// 当集合不为空，则说明有错误信息，数据有不满足需求的
   res.isEmpty();
   ```

   方式二：org.springframework.validation.Validator 下的Validator

   ```java
   // 加载配置类
   ApplicationContext context = new AnnotationConfigApplicationContext(Class);
   
   // 获取LocalValidatorFactoryBean对象
   Validator v = context.getBean("id名", Validator.class);
   
   // 创建：要被校验的类 的异常对象
   BindException bindException = new BindException(要被校验的类的对象,String name);
   
   // 校验数据，将错误信息保存到异常对象中
   v.validate(要被校验的类的对象,bindException);
   
   // 判断是否有错误信息：当有错误信息，返回true，否则返回false
   bindException.hasErrors();
   ```







### 方法方式

1. 创建MethodValidationPostProcessor对象

   ```java
   // 采用配置类的方式实现
   @Configuration
   @ComponentScan("包路径")
   public class 类名{
       
       // 注入MethodValidationPostProcessor对象
       @Bean
       public MethodValidationPostProcessor 方法名(){
           return new MethodValidationPostProcessor();
       }
   }
   ```

2. 创建需要被校验的数据载体类，为需要被校验的属性设置对应的注解

3. 创建一个类，提供方法来校验数据是否符合要求

   ```java
   @Component			// 注入到IoC容器中
   @Validated			// 表明：该类中的方法参数需要被校验
   public class 类名 {
       // 方法校验
       public 返回值类型 方法名(@Valid 要被校验的类的对象) {}
   }
   ```







### 自定义注解

1. 创建自定义注解

   ```java
   @Target({ElementType.METHOD,ElementType.FIELD})		// 元注解，指定可使用该注解的地方
   @Retention(RetentionPolicy.RUNTIME)				// 表明该注解属于运行时注解
   @Documented									// 指定被多次调用时，调用的方法
   @Constraint(validatedBy = {校验器类.class})		// 指定该注解使用的校验规则
   public @interface 注解名{
       // 指定默认的错误提示信息
       String message() default "不能含有空格";
   
       Class<?>[] groups() default {};
       Class<? extends Payload>[] payload() default {};
   
       // 当被多次调用时，执行以下的内容
       @Target({ElementType.METHOD, ElementType.FIELD})
       @Retention(RetentionPolicy.RUNTIME)
       @Documented
       public @interface List {
           注解名[] value();
       }
   }
   ```

2. 定义校验器类

   ```java
   public class 类名 implements ConstraintValidator<注解名,校验值类型>{
       
       // 重写接口中的方法，在该方法内定义校验规则
       @Override
       public boolean isValid(校验值类型 value, ConstraintValidatorContext Context) {
           // value是被校验的值
      	 // context.getDefaultConstraintMessageTemplate() 方法可以获取到默认的错误提示信息
       }
   }
   ```

   









## AOT

1. JIT：Just In Time，动态编译（实时编译），边运行边编译
   - 在程序运行时，动态生成代码
   - 启动速度较慢，编译时会占用运行时资源
2. AOT：Ahead Of Time，运行前编译（提前编译）
   - 可以把源代码直接转换为机器码，启动快，内存占用低
   - 在运行时不能优化，且程序安装时间长，产物不能跨平台运行











## 常用依赖

spring-context

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.23</version>
</dependency>
```



log4j2

```xml
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.19.0</version>
</dependency>
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-slf4j2-impl</artifactId>
    <version>2.19.0</version>
    <scope>test</scope>
</dependency>
```



spring-aspects

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>5.3.1</version>
</dependency>
```



Junit整合

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>6.0.3</version>
    <scope>test</scope>
</dependency>

<!-- Junit5使用 -->
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-api</artifactId>
    <version>5.9.0</version>
    <scope>test</scope>
</dependency>

<!-- Junit4使用 -->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
```



JdbcTemplate

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>6.0.3</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.30</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.15</version>
</dependency>
```



Validator

```xml
<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>8.0.0.Final</version>
</dependency>
<dependency>
    <groupId>org.glassfish</groupId>
    <artifactId>jakarta.el</artifactId>
    <version>4.0.2</version>
    <scope>test</scope>
</dependency>
```

