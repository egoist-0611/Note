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







### 特殊SQL语句

1. 模糊查询：

   ```xml
   <mapper namespace="接口全类名">
       <select id="方法名" resultType="数据载体全类名">
       <!--
   		方式一：SELECT * FROM user WHERE name Like "%"#{参数名}"%"
   
   		方式二：SELECT * FROM user WHERE name Like '%${参数名}%'
   	-->    
       </select>
   </mapper>
   ```

2. 批量删除：

   ```xml
   <mapper namespace="接口全类名">
       <delete id="方法名">
           DELETE FROM 表名 WHERE 字段名 in (${参数名})
           <!-- 传入的参数应为：【1,2】这种类型的字符串 -->
       </delete>
   </mapper>
   ```

3. 动态设置表名：

   ```xml
   <mapper namespace="接口全类名">
       <select id="方法名" resultType="数据载体全类名">
           SELECT * FROM ${参数名} WHERE 字段名 = 值;
       </select>
   </mapper>
   ```

4. 获取自增长主键：

   ```xml
   <mapper namespace="接口全类名">
   	<insert id="方法名" useGeneratedKeys="true" keyProperty="属性名">
       <!--
   		useGeneratedKeys="true"：开启获取自增长主键的功能
   		keyProperty="字段名"：设置存放自增长主键的字段
    	-->
           INSERT INTO 表名(字段名) VALUES(#{属性名})
       </insert>
   </mapper>
   ```

   - 获取自增长主键的办法：

     1. 使用实体类的方式传递参数，并留出一个属性存放自增长主键
     2. 开启了获取自增长主键，并设置了存放自增长主键的属性

     在执行完 INSERT 语句后，就会将新增的数据所产生的自增长主键，存放到指定的属性中











## 核心配置文件

MyBatis的核心配置文件中标签的定义，是需要遵循一定的顺序的

1. properties：引入properties文件

   ```xml
   <properties resource="外部文件名.properties"/>
   <!-- 使用 ${key名} 获取对应的value值 -->
   ```

2. settings：添加一些全局配置

   ```xml
   <settings>
   	<setting name="mapUnderscoreToCamelCase" value="true"/>
       <!-- 添加配置：将下划线更改为驼峰（可用于参数映射） -->
       
      	<setting name="lazyLoadingEnabled" value="true"/>
       <!-- 添加配置：开启延迟加载功能 -->
   </settings>
   ```
   
3. typeAliases：设置select查询结果后，存储的数据载体的类型的别名

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

4. plugins：添加插件

   ```xml
<plugins>
       <!-- 添加分页插件 -->
       <plugin interceptor="com.github.pagehelper.PageInterceptor"/>
   </plugins>
   ```
   
5. environments：配置多个连接数据库的环境

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

6. mappers：引入映射文件

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

使用@Param注解，为传递的参数声明指定的名称，通过 指定的名称 来获取对应的参数值（推荐使用）

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

若接口方法中的参数是 一个实体类类型，则可以直接通过 属性名 来获取对应的属性值（推荐使用）

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









## 参数映射

### 处理属性名和字段名

当字段名与属性名不一致时，有三种解决方案：

1. 查询字段时，为字段重新设置 与属性名同名 的别名

   ```xml
   <mapper namespace="接口全类名">
       <select id="方法名" resultType="数据载体全类名">
           SELECT 字段名 别名 FROM 表名;
       </select>
   </mapper>
   ```

2. 在核心配置文件添加以下配置，使所有的 下划线（xx_yy）更改为 驼峰（xxYy）

   ```xml
   <settings>
       <setting name="mapUnderscoreToCamelCase" value="true"/>
   </settings>
   ```

3. 使用resultMap的方式设置属性名与字段名间的映射关系

   ```xml
   <mapper namespace="接口全类名">
       <resultMap id="resultMap标识id" type="数据载体全类名">
           <id property="属性名" column="主键名"/>
           <result property="属性名" column="字段名"/>
       </resultMap>
       <select id="方法名" resultMap="resultMap标识id">
           SELECT * FROM 表名
       </select>
   </mapper>
   ```









### 多对一关系

> 数据库中存在数据是多对一的关系时，数据载体类中也需要使用 实体类属性 表示这种关系
>
> ```java
> // 多
> public class Empartment{
>     private int eid;
>     private String empName;
>     private int age;
>     private Department dept;
> }
> ```
>
> ```java
> // 一
> public class Department{
>     private int did;
>     private String deptName;
> } 
> ```



方式一（多表关联查询）：通过 `实体类.属性` 的方式，直接为属性赋值（级联方式）

```xml
public interface MoreThenOneMapper {
    List<Employee> getAllEmp();
}

<mapper namespace="com.atguigu.mapper.MoreThenOneMapper">
    <resultMap id="methodOne" type="com.atguigu.pojo.Employee">
        <id property="eid" column="eid"/>
        <result property="empName" column="emp_name"/>
        <result property="age" column="age"/>
        
        <!-- 直接为实体类中的属性赋值 -->
        <result property="dept.did" column="did"/>
        <result property="dept.deptName" column="dept_name"/>
        
    </resultMap>
    <select id="getAllEmp" resultMap="methodOne">
        SELECT * FROM employee e LEFT JOIN department d ON e.did = d.did
    </select>
</mapper>
```



方式二（多表关联查询）：通过association，设置需要被赋值的实体类属性中的属性与其对应的字段

```xml
public interface MoreThenOneMapper {
    List<Employee> getAllEmp();
}

<mapper namespace="com.atguigu.mapper.MoreThenOneMapper">
    <resultMap id="methodTwo" type="com.atguigu.pojo.Employee">
        <id property="eid" column="eid"/>
        <result property="empName" column="emp_name"/>
        <result property="age" column="age"/>
        
        <!-- 通过association， 设置需要被赋值的实体类属性的中的属性名及其对应的字段 -->
        <association property="dept" javaType="com.atguigu.pojo.Department">
            <id property="did" column="did"/>
            <result property="deptName" column="dept_name"/>
        </association>
        
    </resultMap>
    <select id="getAllEmp" resultMap="methodOne">
        SELECT * FROM employee e LEFT JOIN department d ON e.did = d.did
    </select>
</mapper>
```



方式三（多步查询）：先查询出所有的信息，从中获取到外键字段，再通过外键字段去查询，从而获取到所有信息

```xml
public interface MoreThenOneMapper {
    // 第一步：查询所有的信息
	List<Employee> getAllEmp();

    // 第二步：根据查询到的外键字段，去查询该字段所对应的另一张表中的信息
    Department getDepartmentById(@Param("did")Integer did);
}

<mapper namespace="com.atguigu.mapper.MoreThenOneMapper">
	<resultMap id="methodThreeOfEmp" type="com.atguigu.pojo.Employee">
        <id property="eid" column="eid"/>
        <result property="empName" column="emp_name"/>
        <result property="age" column="age"/>
        <!-- 获取其中的外键字段，根据外键字段去查询另一张表中的信息 -->
        <association property="dept" 
                     select="com.atguigu.mapper.MoreThenOneMapper.getDepartmentById" 
                     column="did"/>
    </resultMap>
    <select id="getAllEmp" resultMap="methodThreeOfEmp">
        SELECT * FROM employee
    </select>
    
    <resultMap id="methodThreeOfDept" type="com.atguigu.pojo.Department">
        <id property="did" column="did"/>
        <result property="deptName" column="dept_name"/>
    </resultMap>
    <select id="getDepartmentById" resultMap="methodThreeOfDept">
        SELECT * FROM department WHERE did = #{did}
    </select>
</mapper>
```

association标签中：

- select属性：根据该唯一标识（接口全类名.方法名），去执行对应的接口方法
- column属性：关联字段、查询字段







### 一对多关系

> 数据库中存在数据是一对多的关系时，数据载体类中也需要使用 实体类属性集合 表示这种关系
>
> ```java
> // 多
> public class Employee{
>     private int eid;
>     private String empName;
>     private int age;
> }
> ```
>
> ```java
> // 一
> public class Dept{
>     private int did;
>     private String deptName;
> 	private List<Employee> employees;
> } 
> ```

方式一（多表关联查询）：通过association，设置需要被赋值的实体类属性集合中，每个实体类的属性与其对应的字段

```xml
public interface MoreThenOneMapper {
    List<Department> getAllDept();
}

<mapper namespace="com.atguigu.mapper.OneThenMoreMapper">
    <resultMap id="methodOne" type="com.atguigu.pojo.Department">
        <id property="did" column="did"/>
        <result property="deptName" column="dept_name"/>
        
        <!-- collection是用来表示集合，ofType是集合中每个实体类的类型 -->
        <collection property="employees" ofType="com.atguigu.pojo.Employee">
            <id property="eid" column="eid"/>
            <result property="empName" column="emp_name"/>
            <result property="age" column="age"/>
        </collection>
        <!-- 为实体类集合类型的属性 中的每个实体类 的属性赋值 -->
        
    </resultMap>
    <select id="getAllDept" resultMap="methodOne">
        SELECT * FROM department d LEFT JOIN employee e ON d.did = e.did
    </select>
</mapper>
```



方式二（多步查询）：先查询出所有的信息，从中获取到与主表关联的字段，再通过该字段去查询主表，从而获取到相关的所有信息

```xml
public interface OneThenMoreMapper {
    // 第一步：查询所有的信息
	List<Department> getAllDept();

    // 第二步：根据与主表的绑定的字段，获取主表中的信息
    List<Employee> getEmployeeByDid(@Param("did")Integer did);
}

<mapper namespace="com.atguigu.mapper.OneThenMoreMapper">
    <resultMap id="methodTwoOfSecond" type="com.atguigu.pojo.Employee">
        <id property="eid" column="eid"/>
        <result property="empName" column="emp_name"/>
        <result property="age" column="age"/>
    </resultMap>
    <select id="getEmployeeByDid" resultMap="methodTwoOfSecond">
        SELECT * FROM employee WHERE did = #{did}
    </select>
    
    <resultMap id="methodTwoOfFirst" type="com.atguigu.pojo.Department">
        <id property="did" column="did"/>
        <result property="deptName" column="dept_name"/>
        <!-- 根据唯一标识调用指定的接口方法，查询并关联条件为did的信息 -->
        <association property="employees" 
                     select="com.atguigu.mapper.OneThenMoreMapper.getEmployeeByDid" 
                     column="did"/>
    </resultMap>
    <select id="getAllDept" resultMap="methodTwoOfFirst">
        SELECT * FROM department
    </select>
</mapper>
```









### 延迟加载

使用分步查询时，每一步可以算是独立的。因此，当开启了延迟加载后，系统会自动根据所需去查询对应的数据，而不是每次都去执行所有步骤

- 在核心配置文件中添加全局配置，开启延迟加载功能

  ```xml
  <settings>
      <setting name="lazyLoadingEnabled" value="true"/>
  </settings>
  ```

若想要某个SQL语句的执行不需要进行延迟加载，则可以在 association标签中添加属性：`fetchType` ，设置属性值为 `eager`，使其立即加载，但仅在开启了延迟加载功能后生效（`lazy`为延迟加载）











## 动态SQL

### if 标签

```xml
<mapper namespace="com.atguigu.mapper.TapMapper">
    <select id="getEmpByNameAge" resultType="com.atguigu.pojo.Employee">
        SELECT * FROM employee WHERE 1=1
        <if test="empName!=null and empName!=''">
            AND emp_name = #{empName}
        </if>
        <if test="age!=null and age!=''">
            AND age = #{age}
        </if>
    </select>
</mapper>
```

- if标签中，test属性必须指定：若满足条件，则会在SQL语句中拼接上标签中的内容

  - test中可以直接获取方法的参数值
  - 不能使用&&或||，可以使用 `and` 或 `or` 代替

- 若不添加 1=1，则 AND emp_name 中的AND必须去掉。但是，当emp_name的条件不满足，age条件满足时，依旧会出现问题

  且当两者都不成立时，还会出现多出个WHERE的问题





### where 标签

```xml
<mapper namespace="com.atguigu.mapper.TapMapper">
    <select id="getEmpByNameAge" resultType="com.atguigu.pojo.Employee">
        SELECT * FROM employee
        <where>
            <if test="empName!=null and empName!=''">
                emp_name = #{empName}
            </if>
            <if test="age!=null and age!=''">
                AND age = #{age}
            </if>
        </where>
    </select>
</mapper>
```

- 当 where 标签中存在内容时，会自动拼接上where关键字，并作为 where 的条件出现；当 where 标签中不存在内容时，该标签不会进行任何操作；当 where 关键字后有 and 或 or 时，会自动将其省略





### trim 标签

```xml
<mapper namespace="com.atguigu.mapper.TapMapper">
    <select id="getEmpByNameAge" resultType="com.atguigu.pojo.Employee">
        SELECT * FROM employee
        <trim prefix="WHERE" suffixOverrides="and|or">
            <if test="empName!=null and empName!=''">
                emp_name = #{empName} AND
            </if>
            <if test="age!=null and age!=''">
                age = #{age} AND
            </if>
        </trim>
    </select>
</mapper>
```

- 当 trim 标签中存在内容时，会在拼接到SQL语句前，添加上 prefix 中的内容，并在最终 trim 标签结束后，若末尾为 suffixOverrides 中的内容，则删除该部分内容
- 当 trim 标签不存在内容时，则不会进行任何操作





### choose - when - otherwise 标签

```xml
<mapper namespace="com.atguigu.mapper.TapMapper">
    <select id="getEmpByNameAge" resultType="com.atguigu.pojo.Employee">
        SELECT * FROM employee
        <where>
            <choose>
                <when test="empName!=null and empName!=''">
                    emp_name = #{empName}
                </when>
                <when test="age!=null and age!=''">
                    age = #{age}
                </when>
                <otherwise>
                    eid = 1
                </otherwise>
            </choose>
        </where>
    </select>
</mapper>
```

- choose - when - otherwise 相当于 if - elseif -else，当 when 中有一个满足时，则后续不再执行；当 when 中所有都不满足时，则执行 otherwise





### foreach 标签

```xml
<mapper namespace="com.atguigu.mapper.TapMapper">
	<!-- 批量删除 -->
    <delete id="deleteMoreByArray">
        DELETE FROM employee WHERE eid IN
        <foreach collection="eids" item="eid" separator="," open="(" close=")">
            #{eid}
        </foreach>
    </delete>

	<!-- 批量插入 -->
    <insert id="insertMoreByList">
        INSERT INTO employee VALUES
        <foreach collection="emps" item="emp" separator=",">
            (NULL,#{emp.empName},#{emp.age},#{emp.sex},#{emp.phone},NULL)
            <!-- 通过遍历获取集合中的每个实体类，通过 . 来访问实体类中的属性 -->
        </foreach>
    </insert>
</mapper> 
```

- foreach 循环指定内容
  - collection：要循环的数组或集合
  - item：存储循环值的变量
  - separator：循环内容后，以xxx作为分隔符（分隔符左右有用空格隔开）
  - open：循环前输出的内容
  - close：循环结束后输出的内容





### sql 标签

```xml
<mapper namespace="com.atguigu.mapper.TapMapper">
	<sql id="empColumnName">eid,emp_name,age,sex,phone</sql>-->
    <select id="getAllEmployee" resultType="Employee">
		SELECT <include refid="empColumnName"/> FROM employee-->
    </select>
</mapper>
```

- 使用 sql 标签，将重复使用的内容提取出来。在需要用到时，使用 include 标签进行导入









## 缓存机制

1. 一级缓存（默认）：在同一个SqlSession中，查询的数据会被缓存；当下次查询相同的数据是，会直接从缓存中获取，而不会从数据库中查询

   - 一级缓存失效的原因：

     ① 不同的SqlSession

     ② 同一个SqlSession，但查询的条件不同

     ③ 同一个SqlSession，但两次查询期间执行了任意一次增删改操作（无论是否提交事务）

     ④ 同一个SqlSession，但手动清空了缓存（调用 `SqlSession对象.clearCache()` 方法）

2. 二级缓存：属于SqlSessionFactory级别的，通过同一个SqlSessionFactory创建的SqlSession查询的结果会被缓存

   - 二级缓存开启条件：

     ① 核心配置文件中，设置全局配置 cacheEnabled = "true"（默认开启）

     ② 在映射文件中添加标签 \<cache />

     ③ 在SqlSession关闭（`SqlSession对象.close()`）或提交（`SqlSession对象.commit()`）后才会缓存

     ④ 查询结果所存储的数据载体类必须实现序列化接口 Serializable

   - 二级缓存失效原因：两次查询之间进行了任意一次增删改操作（会使一级缓存和二级缓存同时失效）



缓存的查询顺序：先查询二级缓存，再查询一级缓存，若还未查询到需要的数据，则会查询数据库









## 逆向工程

### 基础配置

1. 导入Maven插件：

   ```xml
   <build>
       <plugins>
           <!-- 导入插件后，在 右边Maven工具栏 -> Plugins -> mybatis-generator 中双击使用 -->
           <plugin>
               <groupId>org.mybatis.generator</groupId>
               <artifactId>mybatis-generator-maven-plugin</artifactId>
               <version>1.3.0</version>
               <!-- 插件所需依赖 -->
               <dependencies>
                   <dependency>
                       <groupId>org.mybatis.generator</groupId>
                       <artifactId>mybatis-generator-core</artifactId>
                       <version>1.3.2</version>
                   </dependency>
                   <dependency>
                       <groupId>com.mchange</groupId>
                       <artifactId>c3p0</artifactId>
                       <version>0.9.2</version>
                   </dependency>
                   <dependency>
                       <groupId>mysql</groupId>
                       <artifactId>mysql-connector-java</artifactId>
                       <version>8.0.33</version>
                   </dependency>
               </dependencies>
           </plugin>
       </plugins>
   </build>
   ```

2. 创建配置文件：`generatorConfig.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE generatorConfiguration
           PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
           "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
   <generatorConfiguration>
       
       <!-- 
   		targetRuntime：设置执行的逆向工程的版本
   			① MyBatis3Simple：生成最基本的CRUD（简洁版）
   			② MyBatis3：生成可拥有条件的CRUD（奢华版）
    	-->
       <context id="DB2Tables" targetRuntime="MyBatis3Simple">
           
           <!-- 数据库连接信息 -->
           <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                           connectionURL="jdbc:mysql://localhost:3306/mybatis"
                           userId="root"
                           password="123456"/>
           
           <!-- 
   			javaModelGenerator：生成pojo类
                   targetPackage：设置生成的包
                   targetProject：设置在哪个路径下生成包
                   enableSubPackages：设置生成的包所用的.是否是代表一层目录，true表示是
                   trimStrings：设置读取字段名时去除首尾空格
   		-->
           <javaModelGenerator targetPackage="com.atguigu.pojo" 
                               targetProject="./src/main/java">
               <property name="enableSubPackages" value="true"/>
               <property name="trimStrings" value="true"/>
           </javaModelGenerator>
           
   		<!-- 生成Mapper映射文件 -->
           <sqlMapGenerator targetPackage="com.atguigu.mapper" 
                            targetProject="./src/main/resources">
               <property name="enableSubPackages" value="true"/>
           </sqlMapGenerator>
           
           <!-- 生成Mapper接口 -->
           <javaClientGenerator type="XMLMAPPER" targetPackage="com.atguigu.mapper" 
                                targetProject="./src/main/java">
               <property name="enableSubPackages" value="true"/>
           </javaClientGenerator>
           
           <!-- 
   			设置被逆向分析的表
   				tableName：逆向分析的表名
   				domainObjectName：生成的pojo类名
    		-->
           <table tableName="employee" domainObjectName="Employee"/>
           <table tableName="department" domainObjectName="Department"/>
       </context>
   </generatorConfiguration>
   ```







### 奢华版使用

奢华版可以实现：对任意字段，根据任意条件，来进行操作

使用举例：

1. 查询所有的员工：

   ```java
   // 获取SqlSession对象
   InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
   SqlSession sqlSession = new SqlSessionFactoryBuilder().build(is).openSession();
   
   // 获取映射接口对象
   EmployeeMapper mapper = sqlSession.getMapper(EmployeeMapper.class);
   
   // 查询方法有两个：一个是根据条件进行查询，一个是根据主键进行查询
   // 很显然，查询所有的员工，则需要根据条件进行查询，而条件就是null
   List<Employee> employees = mapper.selectByExample(null);
   System.out.println(employees);
   ```

   - SQL语句相当于：`SELECT * FROM employee`

2. 查询姓名为June，年龄为19；或姓名为July，年龄为19的员工

   ```java
   // 获取SqlSession对象
   InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
   SqlSession sqlSession = new SqlSessionFactoryBuilder().build(is).openSession();
   
   // 获取映射接口对象
   EmployeeMapper mapper = sqlSession.getMapper(EmployeeMapper.class);
   
   // 很显然，根据条件进行查询，而selectByExample()需要一个对应的Example参数
   // 创建一个Example对象
   EmployeeExample employeeExample = new EmployeeExample();
   
   // 调用Example对象中的方法来设置条件（where条件）
   // 使用createCriteria()来创建条件，andXxx()来设置条件的值（见名知意）
   employeeExample.createCriteria().andEmpNameEqualTo("June").andAgeEqualTo(19);
   
   // or() 来表示where条件中的or，or之后的条件单独作为一部分（即：使用了()包裹）
   employeeExample.or().andEmpNameEqualTo("July").andAgeEqualTo(19);
   
   List<Employee> employees = mapper.selectByExample(employeeExample);
   System.out.println(employees);
   ```

   - SQL语句相当于：`SELECT * FROM employee WHERE (emp_name='June' AND age=19) OR (emp_name='July' AND age=19)`
   
3. 增删改操作，如：updateByPrimaryKey、updateByPrimaryKeySelective 两者的区别：

   - updateByPrimaryKey：当值为 null 时，修改依旧会进行，原值会被置为 null
   - updateByPrimaryKeySelective：当值为 null 时，不会修改该值，仍为原值









## 分页插件

### 配置

1. 添加依赖：pagehelper

   ```xml
   <dependency>
       <groupId>com.github.pagehelper</groupId>
       <artifactId>pagehelper</artifactId>
       <version>5.3.1</version>
   </dependency>
   ```

2. 在MyBatis核心配置文件中，配置插件：

   ```xml
   <plugin interceptor="com.github.pagehelper.PageInterceptor"/>
   ```





### 使用

> 当前查询的数据的索引 = ( 当前显示的页码 - 1 )  * 每页显示条数

1. 在查询前开启分页

   ```java
   PageHelper.startPage(int pageNum,int pageSize);
   /*
   	pageNum：当前页码
   	pageSize：每页显示条数
   */
   ```

2. 在查询后获取分页信息

   ```java
   PageInfo<T> page = new PageInfo<>(List list,int navigatePages);
   /* 
   	T：数据载体类的类型
   	list：查询后的结果集（List集合，要被分页的数据）
   	navigatePages：导航分页的数量
   */
   ```



PageInfo返回结果分析：

- pageNum：当前页数
- pageSize：每页条目数
- size：当前页显示的条目数
- total：总记录数
- pages：总页数
- prePage / nextPage：上一页 / 下一页 的页码
- isFirstPage / isLastPage：是否是 第一页 / 最后一页
- hasPreviousPage / hasNextPage：是否存在 上一页 / 下一页
- navigatePages：导航分页的页码数
- navigateFirstPage / navigateLastPage：当前导航分页的 第一页 / 最后一页
- navigatepageNums：当前导航分页的所有页码