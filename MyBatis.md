# MyBatis

## 概念

MyBatis（原名 iBatis），是一个基于Java的持久层框架，包括了SQLMaps和DAO（Data Access Objects）

- 支持定制化SQL（自定义SQL语句）、存储过程以及高级映射

- 避免了几乎所有的JDBC代码和手动设置参数、获取结果集

- 可以使用简单的XML或注解用于配置和原始映射，将接口和Java的pojo映射成数据库中的记录

- 半自动的ORM（Object Relation Mapping对象关系映射）框架

  - 对象：Java实体类对象
  - 关系：关系型数据库
  - 映射：两者关联

  | Java | 数据库 |
  | :--: | :----: |
  |  类  |   表   |
  | 属性 |  字段  |
  | 对象 | 行数据 |

  







## 第一个测试

1. 创建新的Maven父工程、子工程

2. 修改使用的Maven版本、配置文件、仓库地址，修改Maven依赖为自动导入

3. 导入依赖：mybatis、junit、mysql-connector-java

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
   <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.12</version>
       <scope>test</scope>
   </dependency>
   ```

4. 创建MyBatis的核心配置文件（一般起名为：mybatis-config.xml）

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC"/>	
               <!-- 表示使用JDBC的方式管理事务，即：事务的提交与回滚需要手动执行 -->
               <dataSource type="POOLED">
                   <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://localhost:3306/数据库名"/>
                   <property name="username" value="账号"/>
                   <property name="password" value="密码"/>
               </dataSource>
           </environment>
       </environments>
       <mappers>
           <mapper resource="在系统资源下的映射文件的路径"/>
       </mappers>
   </configuration>
   ```

5. 创建pojo类，并创建对应的映射接口（相当于原先的DAO接口；但不再叫DAO，而是改为Mapper）

   ```java
   public interface 接口名(){
       public abstract 返回值类型 方法名();
   }
   ```

6. 创建映射接口对应的映射文件

   - 映射文件一般以 Mapper.xml 结尾
   - 一个映射文件对应了一个映射接口，也就是对应一个pojo类进行操作、对应一张表进行操作
   - 映射文件用于编写SQL语句，访问并操作表中的数据
   - 由于一张表对应一个映射文件，因此可能会有很多映射文件，因此，一般在resources目录下重新创建一个目录（mappers）来存放映射文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="接口全类名">
       <操作 id="接口中方法名">
           SQL语句
       </操作>
   </mapper>
   
   <!-- 
   	MyBatis体现的是面向接口编程：
    		1.接口相当于原先的DAO，但不需要提供接口的实现类
   		2.调用接口中的方法时，会找到映射文件中，接口全类名与其namespace对应的mapper
   		3.根据调用的方法名，找到mapper中某个操作的id值，与方法名对应的
   		4.执行该操作中的SQL语句
   -->
   ```

7. 测试功能

   - 通过加载核心配置文件，获取一个SqlSession对象后，进行操作
     - SqlSession：代表Java程序和数据库之间的会话（HttpSession：Java程序和浏览器之间的会话）

   ```java
   // 1.加载核心配置文件：调用org.apache.ibatis.io.Resources下的方法
   InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
   
   // 2.获取SqlSession对象
   SqlSession sqlSession = new SqlSessionFactoryBuilder().build(is).openSession();
   // openSession(true)：表示开启自动提交事务功能
   
   // 3.获取接口对象（底层使用动态代理方式创建对象）
   接口类型 mapper = sqlSession.getMapper(接口名.class);
   
   // 4.获取接口方法调用后的返回结果
   返回值类型 res = mapper.方法名();
   
   // 5.提交事务（JDBC下需要手动提交事务）
   sqlSession.commit();
   ```









## log4j日志功能

1. 引入依赖：

   ```xml
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   ```

2. 创建 log4j.xml 配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
   <log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
       <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
           <param name="Encoding" value="UTF-8"/>
           <layout class="org.apache.log4j.PatternLayout">
               <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS} %m (%F:%L) \n"/>
           </layout>
       </appender>
       <logger name="java.sql">
           <level value="debug"/>
       </logger>
       <logger name="org.apache.ibatis">
           <level value="info"/>
       </logger>
       <root>
           <level value="debug"/>
           <appender-ref ref="STDOUT"/>
       </root>
   </log4j:configuration>
   ```

   







## 增删改查操作

> 调用接口的方法，来执行SQL语句。因此，实现功能时，因从接口来开始实现

### 增删改

- 接口（XxxMapper）

  ```java
  public interface 接口名(){
      public int insert方法名();
      
      public int delete方法名();
      
      public int update方法名();
  }
  ```

  - SQL中，增删改操作所返回的为影响的行数。因此，接口中的方法返回值类型可以为int，也可以为void（不接收返回值参数）



- 映射文件（XxxMapper.xml）

  ```xml
  <mapper namespace="接口的全类名">  
      <insert id="insert方法名">
          INSERT INTO <!-- SQL语句 -->
      </insert>
      
      <delete id="delete方法名">
          DELETE FROM <!-- SQL语句 -->
      </delete>
  
  	<update id="update方法名">
      	UPDATE <!-- SQL语句 -->
      </update>
  </mapper>
  ```

  - 被调用的接口的方法，通过 namespace 找到对应的映射关系，再根据方法名，找到 id 所对应操作，执行其中的SQL语句









### 查询

- 接口（XxxMapper）

  ```java
  public interface 接口名(){
      public 数据载体类型 select方法名();
  }
  ```

  - 当返回多条数据时，则可使用 `List<数据载体类型>` 来表示



- 映射文件（XxxMapper.xml）

  ```xml
  <mapper namespace="接口的全类名">  
      <select id="insert方法名" resultType="数据载体类的全类名">
          SELECT <!-- SQL语句 -->
      </select>
  </mapper>
  ```

  - resultType 的作用是：指定查询到的结果，所封装到的数据载体的类型
  - 可以使用 resultType，也可以使用 resultMap，两者的区别是：
    - resultType 是默认的映射关系，即：数据载体属性名与字段名需保持一致
    - resultMap 可以自定义映射关系









## 核心配置文件

MyBatis的核心配置文件中标签的定义，是需要遵循一定的顺序的

1. properties：引入properties文件

   ```xml
   <properties resource="外部文件名.properties"/>
   <!-- 使用 ${key名} 获取对应的value值 -->
   ```

2. typeAliases：设置select查询结果后，存储的数据载体的类型的别名

   ```xml
   <typeAliases>
       
       <typeAlias type="数据载体的全类名" alias="别名"/>
       <!-- 
           设置某个具体的数据载体的别名
               ① 当alias不指定时，默认使用 type数据载体的类名 作为别名
               ② 别名不区分大小写
       -->
       
       <package name="包全路径"/>
       <!-- 以包为单位，指定的包路径下所有的类都 使用默认的别名（即：使用类名作为别名） -->
   
   </typeAliases>
   ```

3. environments：配置多个连接数据库的环境

   - default：设置默认使用的数据库环境，值为具体的数据库环境的 id值

   ```xml
   <environments default="environment的id">
       
       <environment id="唯一标识">
       <!-- 配置具体的数据库环境 -->
           
           <transactionManager type="JDBC"/>
           <!-- 
   			设置事务的管理方式
   				type：有两种方式
   					① JDBC：使用JDBC原生的事务管理方式，事务的提交回滚需要手动执行
   					② MANAGED：被管理（如：SSM整合时，被Spring管理）
   		-->
           
           <dataSource type="POOLED">
           <!--
   			配置数据源
   				type：有三种类型
   					① POOLED：表示使用数据库连接池，缓存数据库连接
   					② UNPOOLED：表示不使用数据库连接池
   					③ JNDI：表示使用上下文中的数据源
   		-->
               
               <property name="driver" value="值"/>
               <property name="url" value="值"/>
               <property name="username" value="值"/>
               <property name="password" value="值"/>
           </dataSource>
       </environment>
   </environments>
   ```

4. mappers：引入映射文件

   ```xml
   <mappers>
       
       <mapper resource="映射文件资源路径"/>
       <!-- 引入具体的某个xml映射文件 -->
       
       <package name="包路径.xml文件"/>
       <!-- 
   		以包为单位，引入指定包路径下所有的xml映射文件（创建Directory时，可使用/快速建多层目录）
   			要求：① Mapper接口所在的包，与映射文件所在的包名称一致
   				 ② Mapper接口，要与映射文件的名称一致
   	-->
       
   </mappers>
   ```

   





