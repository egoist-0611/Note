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

3. 配置为IDEA中的模板：Settings --> Editor --> File and Code Templates --> 设置模板名、模版默认后缀名、Enable Live Templates 动态光标功能（`#[[$Title$]]#` 光标从此开始、`#[[$END$]]#` 光标从此结尾）









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

#### 基本介绍

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





#### 查询单条数据

##### ① 通过实体类对象接收

```java
public interface UserMapper {
    User getUserByIdOnPojo(@Param("id")Integer id);
}
```

```xml
<mapper namespace="com.atguigu.mapper.UserMapper">
    <select id="getUserByIdOnPojo" resultType="com.atguigu.pojo.User">
        SELECT * FROM user WHERE id = #{id}
    </select>
</mapper>
```

- 返回值类型为具体的pojo对象





##### ② 通过List集合接收

```java
public interface UserMapper {
    List<User> getUserByIdOnList(@Param("id") Integer id);
}
```

```xml
<mapper namespace="com.atguigu.mapper.UserMapper">
    <select id="getUserByIdOnList" resultType="com.atguigu.pojo.User">
        SELECT * FROM user WHERE id = #{id}
    </select>
</mapper>
```

- 返回的类型为pojo对象所构成的List集合（但仅有单条数据）





##### ③ 通过Map集合接收

```java
public interface UserMapper {
    Map<String, Object> getUserByIdOnMap(@Param("id") Integer id);
}
```

```xml
<mapper namespace="com.atguigu.mapper.UserMapper">
    <select id="getUserByIdOnMap" resultType="Map">
        SELECT * FROM user WHERE id = #{id}
    </select>
</mapper>
```

- 返回的类型为Map集合，以查询到的结果中的 字段名为key，字段值为value
- resultType 中声明的返回值类型为 Map，是使用了默认提供的类名别名







#### 查询多条数据

##### ① 通过 List\<pojo> 接收

```java
public interface UserMapper {
    List<User> getAllUserOnListPojo();
}
```

```xml
<mapper namespace="com.atguigu.mapper.UserMapper">
    <select id="getAllUserOnListPojo" resultType="com.atguigu.pojo.User">
        SELECT * FROM user
    </select>
</mapper>
```

- 返回的类型为pojo对象所构成的List集合





##### ② 通过 List\<Map> 接收

```java
public interface UserMapper {
    List<Map<String,Object>> getAllUserOnListMap();
}
```

```xml
<mapper namespace="com.atguigu.mapper.UserMapper">
    <select id="getAllUserOnListMap" resultType="Map">
        SELECT * FROM user
    </select>
</mapper>
```

- 返回的类型为Map集合所构成的List集合。其中Map集合中的key为查询的结果中的字段名，value为查询的结果中的字段值
- 查询到多条记录，若仅用单个Map集合来接收，就会出现 TooManyResultsException 的异常





##### ③ 通过 Map\<Map> 接收

```java
public interface UserMapper {
    @MapKey("id")
    Map<String,Map<String,Object>> getAllUserOnMapMap();
}
```

```xml
<mapper namespace="com.atguigu.mapper.UserMapper">
    <select id="getAllUserOnMapMap" resultType="Map">
        SELECT * FROM user
    </select>
</mapper>
```

- 我们是不能直接通过 Map\<Map> 的方式存储结果的，因为我们查询到的字段名和字段值，作为内存Map的key和value，而每一行数据，只能作为外层Map的key或value。因此，我们需要为外层Map指定一个key
- 通过 `@MapKey("字段名")` 的方式，我们可以指定某个字段的值作为（外层）Map的key
- 通常使用主键作为外层Map的key，因为这样不会重复（在Map中，重复的key的value会被后者替换）







#### 查询复合函数

```java
public interface UserMapper {
    Integer getUserCount();
}
```

```xml
<mapper namespace="com.atguigu.mapper.UserMapper">
    <select id="getUserCount" resultType="Integer">
        SELECT COUNT(*) FROM user
    </select>
</mapper>
```

- 查询的是 COUNT()、MAX() 等函数时，返回值类型由方法返回值来决定
- resultType 中声明的返回值类型为 Integer，是由函数执行后返回的值决定的，且是默认提供的类名的别名









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

   







## 获取参数

### 两种方式

MyBatis中，获取调用接口方法时传递的参数的方式有两种：`${}` 和 `#{}`

- ${}：本质上是在获取到值后，进行字符串的拼接（因此需要手动添加 `''`）如：

  ````xml
  SELECT * FROM user WHERE name = ${username}
  <!--
  	转换后相当于：SELECT * FROM user WHERE name = Tom
  		需要改为 'Tom' 使SQL语句合法，即改为：'${username}'
  -->
  ````

- #{}：本质上是在获取到值后，对占位符内容进行填充，如：

  ```xml
  SELECT * FROM user WHERE name = #{username}
  <!-- 
  	转换后相当于：SELECT * FROM user WHERE name = ?
  		? 会被最终填充为 'Tom'
  -->
  ```





### 获取当个参数

若接口中的方法的参数为单个，则可以直接使用 任意名称 获取到参数的值

> 以 #{} 举例：
>
> ```xml
> public interface UserMapper {
>    	User getUserById(Integer id);
> }
> 
> <mapper namespace="com.atguigu.mapper.UserMapper">
>     <select id="getUserById" resultType="com.atguigu.pojo.User">
>         SELECT * FROM user WHERE id = #{id}
>         <!-- #{任意名称} 都可获取到传递的参数值 -->
>     </select>
> </mapper>
> ```







### 获取多个参数

#### 使用内置Map集合

若接口中的方法的参数为多个，此时MyBatis会自动将这些参数存放到一个Map集合中，并以两种方式存储：

1. 以 `arg0`、`arg1` ... 为key，以参数值为value进行存储
2. 以 `param1`、`param2` ... 为key，以参数值为value进行存储

通过固定的key来获取到value值

> 以 #{} 举例：
>
> ```xml
> public interface UserMapper {
>     User getUserByNameAge(String name,Integer age);
> }
> 
> <mapper namespace="com.atguigu.mapper.UserMapper">
>     <select id="getUserByNameAge" resultType="com.atguigu.pojo.User">
> 		SELECT * FROM user WHERE name = #{arg0} AND age = #{arg1}
>         <!-- 通过了 #{arg0}、#{arg1} 获取到了传递的 参数1、参数2 -->
>     </select>
> </mapper>
> ```





#### 自定义Map集合

若接口方法中传递的是一个Map集合，则可以 直接通过key 来获取对应的value

> 以 #{} 举例：
>
> ```java
> public interface UserMapper {
>     User getUserByMap(Map<String,Object> map);
> }
> 
> <mapper namespace="com.atguigu.mapper.UserMapper">
>     <select id="getUserByMap" resultType="com.atguigu.pojo.User">
>         SELECT * FROM user WHERE name = #{name} AND age = #{age}
> 		// 传递的是Map集合，内部有两个key：name、age，通过key获取到对应的value值
>     </select>
> </mapper>
>         
> public void test(){
> 	InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
>     SqlSession sqlSession = new SqlSessionFactoryBuilder().build(is).openSession();
>     UserMapper mapper = sqlSession.getMapper(UserMapper.class);
> 	HashMap<String, Object> map = new HashMap<>();
> 	map.put("name","Amy");		// 存的第一个key和value
> 	map.put("age",19);			// 存的第二个key和value
> 	User user = mapper.getUserByMap(map);		// 将Map作为参数传递
> 	System.out.println(user);        
> }
> ```





#### 使用@Param注解

使用@Param注解，为传递的参数声明指定的名称，通过 指定的名称 来获取对应的参数值

- 也可以使用 `param1`、`param2` ... ，以固定的key来获取对应的value

> 使用 #{} 举例：
>
> ```java
> public interface UserMapper {
>     // 通过注解指定获取参数时使用的名称
> 	User getUserByAnnotation(@Param("name")String name,@Param("age")Integer age);    
> }
> 
> <mapper namespace="com.atguigu.mapper.UserMapper">
> 	<select id="getUserByAnnotation" resultType="com.atguigu.pojo.User">
>         SELECT * FROM user WHERE name = #{name} AND age = #{age}
>     </select>
> </mapper>
> ```





#### 使用实体类类型

若接口方法中的参数是实体类类型，则可以直接通过 属性名 来获取对应的属性值

> 使用 #{} 举例：
>
> ```xml
> public interface UserMapper {
> 	void insertUser(User user);
> }
> 
> <mapper namespace="com.atguigu.mapper.UserMapper">
> 	<insert id="insertUser">
>         INSERT INTO user(name,age,sex,phone) VALUES(#{name},#{age},#{sex},#{phone})
>     	<!-- 通过属性名获取到封装在实体类属性中的值 -->
>     </insert>
> </mapper>
> ```













