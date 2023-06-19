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
   
   // 3.获取接口对象（底层使用动态代理方式创建对象）
   接口类型 mapper = sqlSession.getMapper(接口名.class);
   
   // 4.获取接口方法调用后的返回结果
   返回值类型 res = mapper.方法名();
   
   // 5.提交事务（JDBC下需要手动提交事务）
   sqlSession.commit();
   ```

   