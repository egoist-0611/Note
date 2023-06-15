### JDBC概念

JDBC是java连接数据库技术的统称（是一种典型的面向接口编程），由两部分组成：

1. Java提供的jabc规范（接口），位于java.sql和javax.sql包下
2. 各类数据库厂商提供的实现驱动（实现类）jar包







### 基本步骤

#### 步骤

1. 导入jar包（mysql-connector-java.jar）
2. 注册驱动（对依赖的jar包进行安装）

3. 建立Connection连接对象（相当于造桥）
4. 创建发送SQL语句的Statement对象（相当于造拉货车）
5. 用Statement对象发送SQL语句到数据库，并将结果集存入ResultSet中返回（相当于拉货）
6. 解析结果集（相当于查货）
7. 释放Connection连接资源、Statement对象资源、ResultSet对象资源（相当于：拆桥、砸车、毁货）







#### 实现

2）注册驱动

- 调用 DirverManager 类的静态方法：`registerDriver(Diver)`

  >Diver对象：
  >
  > ① 若是驱动版本为8+，则导入的是 com.mysql.cj.jdbc.Driver
  >
  > ② 若是驱动版本为5+，则导入的是 com.mysql.jdbc.Driver



3）建立Connection连接对象

> 像Navicat等连接数据库一样，在建立连接时，都要指明连接的IP、端口号等信息

- 调用 DriverManager 类的静态方法：

  【Connection】`getConnection(String url,String user,String password)`

  >url格式：`jdbc:数据库厂商名://IP地址:端口号/数据库名`
  >
  >（如：jdbc:mysql://127.0.0.1:3306/atguigudb）
  >
  >user、password：数据库的用户名、密码



4）创建发送SQL语句的Statement对象

- 调用Connection对象的方法：【Statement】`createStatement()`



5）用Statement对象发送SQL语句到数据库，并获取返回的ResultSet结果集对象

- 调用Statement对象的方法：【ResultSet】`executeQuery(String sql)`

  > sql：要执行的sql语句



6）解析结果集

1. 获取列的信息（包括：列的名称、列的数量等）

   - 调用ResultSet对象的方法：【ResultSetMetaData】`getMetaData()`
     - 获取列的数量：通过ResultSetMetaData对象调用方法：【int】`getColumnCount()`
     - 获取列的别名：通过ResultSetMetaData对象调用方法：【String】`getColumnLabel(int index)`（通过列所在的下标获取该列的别名。若没有别名，则获取字段名）

2. 获取列的值

   - 调用ResultSet对象的方法：【boolean】`next()` 来判断是否有下一行数据，有的话则指针下移

     > 常用于while中循环获取每行数据

   - 调用ResultSet对象的方法：【Xxx】`getXxx(String column)` 来获取该行中指定的字段的值

     >如：
     >
     >getInt("id") 获取字段名为id，类型为INT的值
     >
     >getString("username") 获取字段名为username、类型为VARCHAR的值

   - 调用ResultSet对象的方法：【Object】`getObject(int index)` 来获取指定下标的列的值

     >列的下标从左到右，从1开始数



7）释放Connection连接资源、Statement对象资源、ResultSet对象资源

- 调用相应的方法：`close()`







#### 举例

```java
// 1.注册驱动
DriverManager.registerDriver(new Driver());

// 2.获取connection连接对象
Connection c = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/atguigu","root","123456");

// 3.创建发生SQL语句的Statement对象
Statement s = c.createStatement();

// 4.利用Statement对象发生SQL语句，并接收返回的结果集
ResultSet rs = s.executeQuery("SELECT * FROM t_user");

// 5.解析结果集
while(rs.next()){
    int id = rs.getInt("id");
    String account = rs.getString("account");
    String password = rs.getString("password");
    String nickname = rs.getString("nickname");
    System.out.println(id+"\t"+account+"\t"+password+"\t"+nickname);
}

// 6.关闭资源
rs.close();
s.close();
c.close();
```









### 详细讲解

#### 步骤二

>注册驱动

（方案一）调用 DriverManager类的静态方法：`registerDriver(new com.mysql.cj.jdbc.Driver())` 

- 观察 DriverManager、Driver 底层源码，发现该方式注册驱动，会存在【注册两次驱动】的问题。因此，考虑进行优化：只注册一次驱动

- 若要实现只注册一次驱动，就要从Driver底层调用静态代码块入手。静态代码块随着类的加载而执行，而可以触发类加载的方式有：

  ①new关键字；②调用静态属性或方法；③接口中的default默认方法；④反射；⑤子类触发父类；⑥作为程序入口main

  综上，使用【反射】实现注册驱动最好，可以实现通过读取外部文件的方式来动态注册不同数据库驱动

（方案二）通过反射注册驱动：`Class.forName("com.mysql.cj.jdbc.Driver");`







#### 步骤三

> 建立Connection连接对象

调用 DriverManager 类的静态方法创建连接对象：

1. 【Connection】`getConnection(String url,String user,String password)`

   url：jdbc:数据库厂商名://IP地址:端口号/数据库名

   user：登录账号

   password：登录密码

2. 【Connection】`getConnection(String url)`

   url：jdbc:数据库厂商名://IP地址:端口号/数据库名?user=账号&password=密码

3. 【Connection】`getConnection(String url,Properties info)`

   url：jdbc:数据库厂商名://IP地址:端口号/数据库名

   info：该对象应当包含有两个key：user 和 password，value分别对应：数据库的账号和密码

   > Properties：Map的实现类之一，但key和value都是字符串类型，一般用于配置文件中。

> 若 IP地址:端口号 是【本机IP:3306】时，可以替换为`/`
>
> 可选属性：serverTimezone=Asia/Shanghai&useUnicode=true&characterEncoding=utf8&useSSL=true
>
> - 驱动8.0.25以后，支持自动识别时区，因此 serverTimezone=Asia/Shanghai 可以不用添加了
> - 驱动8版本后，默认使用的就是utf8格式，useUnicode=true&characterEncoding=utf8&useSSL=true 可以不用添加了





#### 步骤四与步骤五

##### 方案一

> 该方案需要手动的拼接值，且存在SQL注入问题

1. 创建发送SQL语句的Statement对象

   - 调用Connection对象的方法：【Statement】`createStatement()`

2. 用Statement对象发送SQL语句到数据库，并将结果集存入ResultSet中返回

   - 调用Statement对象的方法：

     ①【ResultSet】`executeQuery(String sql)`

     > 用于DQL类型的SQL语句

     ②【int】`executeUpdate(String sql)`

     > 用于非DQL类型的SQL语句
     >
     > 1. DML类型：返回影响的行数，如：删除3行数据-->return 3
     > 2. 非DML类型：return 0

   > SQL分类：DDL（容器创建、删除、修改）、DML（数据插入、修改、删除）、DQL（查询）、DCL（权限控制）、TPL（事务控制语言）





##### 方案二

1. 创建PreparedStatement对象，对不包含动态值的SQL语句进行预编译（动态值使用占位符 `?` 替换）

   - 调用Connection对象的方法：【PreparedStatement】`prepareStatement(String sql)`

2. 通过PreparedStatement对象调用其内部方法来填充占位符：`setObject(index,value)`

   - index：该占位符的位置，从左往右，从1开始数
   - value：动态值

3. 用PreparedStatement对象 将预编译并填充好占位符的SQL语句发送到数据库，并获取结果

   - 调用PreparedStatement对象的方法：

     ①【ResultSet】`executeQuery()`

     ②【int】`executeUpdate()`

     > 已经进行预编译，确定了要传入的SQL语句，因此不需要再传入SQL语句







#### 步骤六

>解析结果集

想要进行数据的解析，想要关心两件事情：

1. 移动游标获取每一行的数据

   - resultSet内部包含了一个游标，用于指定当前的行数据（默认游标指定的是第一行数据之前）

   - 调用resultSet的内部方法：【boolean】`next()`

     - 当返回true时，表示还有下一行数据，并向下移动一行
     - 当返回false时，表示没有更多行数据，不移动

     因此可以使用 while() 来循环获取每一行数据

2. 获取指定行数据的每一列（获取游标指定的行的列的数据）

   - 调用resultSet的内部方法：
     1. 【Xxx】`getXxx(String columnLabel)`：根据 列名 或 列的别名 来获取指定列的值
     2. 【Xxx】`getXxx(int columnIndex)`：根据 列所处的下标 来获取（从左到右，从1开始）







#### 举例

##### 用户登录

```java
	// 获取用户名密码
Scanner scanner = new Scanner(System.in);
System.out.println("请输入用户名");
String account = scanner.next();
System.out.println("请输入密码");
String password = scanner.next();

// 1.注册驱动
Class.forName("com.mysql.cj.jdbc.Driver");
// 2.获取连接
Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/atguigu?user=root&password=123456");
// 3.创建发送SQL语句的PreparedStatement对象，并预编译SQL语句
String sql = "SELECT * FROM t_user WHERE account = ? AND password = ? ;";
PreparedStatement preparedStatement = connection.prepareStatement(sql);
// 4.填充占位符
preparedStatement.setObject(1,account);
preparedStatement.setObject(2,password);
// 5.发送SQL语句，获取结果集
ResultSet resultSet = preparedStatement.executeQuery();

	// 判断是否登录成功
if(resultSet.next()){
    System.out.println("登录成功");
}else{
    System.out.println("登录失败");
}

// 6.关闭资源
resultSet.close();
preparedStatement.close();
connection.close();
```





##### 获取用户信息

> 并封装到 List\<Map> 中

```java
// 1.注册驱动
Class.forName("com.mysql.cj.jdbc.Driver");
// 2.创建连接
Connection connection = DriverManager.getConnection("jdbc:mysql:///atguigu", "root", "123456");
// 3.创建PreparedStatement对象，预编译sql语句
String sql = "SELECT * FROM t_user";
PreparedStatement preparedStatement = connection.prepareStatement(sql);
// 4.发送sql语句，获取结果集
ResultSet resultSet = preparedStatement.executeQuery();
	// 获取结果集中列的信息
ResultSetMetaData metaData = resultSet.getMetaData();
	// 获取结果集中列的个数
int columnCount = metaData.getColumnCount();
	// 创建list，存储多个map
ArrayList<Map> arrayList = new ArrayList<>();
// 5.解析结果集
while (resultSet.next()){
    	// 创建map，存储每个列以及对应的值
    Map<String,Object> map = new LinkedHashMap<>();
    	// 循环获取每一行中的每一列
    for (int i = 1; i <= columnCount; i++) {
        	// 获取列名
        String name = metaData.getColumnName(i);
        	// 获取列值
        Object value = resultSet.getObject(i);
        map.put(name,value);
    }
    arrayList.add(map);
}
System.out.println(arrayList);
// 6.关闭资源
resultSet.close();
preparedStatement.close();
connection.close();
```









### 拓展

#### 主键回显

> 在主表中插入一条数据后，可能会需要用自增长的主键对子表进行数据的插入，因此就需要用到主键回显（即：获取自增长的主键的值）

1. 在创建PrepareStatement对象、对SQL语句进行预编译时，让其：执行SQL语句后，将自增长的主键给带回

   - 调用Connection对象的方法：【PreparedStatement】`prepareStatement(String sql, Statement.RETURN_GENERATED_KEYS)`

     > Statement.RETURN_GENERATED_KEYS：也可等价于1

2. 在执行SQL语句后，获取存储 自增长的主键 的结果集对象（结果集中仅有一行一列的数据）

   - 调用Statement对象的方法：【ResultSet】`getGeneratedKeys()`



使用举例：

```java
// 注册驱动
Class.forName("com.mysql.cj.jdbc.Driver");
// 获取连接对象
Connection connection = DriverManager.getConnection("jdbc:mysql:///atguigu?user=root&password=123456");
// 创建Statement对象、预编译SQL语句、告知要返回自增长主键
String sql = "INSERT INTO t_user(account,password,nickname) VALUES(?,?,?)";
PreparedStatement preparedStatement = connection.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS);
// 填充占位符
preparedStatement.setObject(1, "test1");
preparedStatement.setObject(2, "test1");
preparedStatement.setObject(3, "test2");
// 执行SQL语句，返回执行结果
int count = preparedStatement.executeUpdate();
if (count > 0) {
    System.out.println("数据插入成功！");
// 获取存储自增长主键的结果集
    ResultSet resultSet = preparedStatement.getGeneratedKeys();
// 解析结果集
    resultSet.next();
    int id = resultSet.getInt(1);
    System.out.println(id);
// 关闭资源
    resultSet.close();
}
preparedStatement.close();
connection.close();
```







#### 批量插入

> 优化原理：以追加values的方式，减少与数据库交互的次数

1. 在建立连接时，传递的url中追加参数：`rewriteBatchedStatements=true`，让数据库支持批量插入
2. 在构造插入语句 INSERT INTO 时，要使用 VALUES 表示，且语句末尾不能加 ;
3. 每条语句填充好占位符后，不是直接执行，而是让其进行追加
   - 调用PreparedStatement对象的方法：`addBatch()`
4. 追加完所有的values后，实现一次性批量插入
   - 调用PreparedStatement对象的方法：`executeBatch()`



使用举例：

```java
// 注册驱动
Class.forName("com.mysql.cj.jdbc.Driver");
// 创建连接对象、告知数据库要支持批量插入
Connection connection = DriverManager.getConnection("jdbc:mysql:///atguigu?rewriteBatchedStatements=true", "root", "123456");
// 创建Statement对象、预编译sql语句
String sql = "INSERT INTO t_user(account,password,nickname) VALUES(?,?,?)";
PreparedStatement preparedStatement = connection.prepareStatement(sql);
// 进行批量追加
for (int i = 0; i < 1000; i++) {
// 填充占位符
    preparedStatement.setObject(1, "customer"+i);
    preparedStatement.setObject(2,(int)(Math.random()*1000000)+"");
    preparedStatement.setObject(3,"UnKnow");
// 不执行，而是追加
    preparedStatement.addBatch();
}
// 执行批量追加后的sql语句
preparedStatement.executeBatch();
// 关闭资源
preparedStatement.close();
connection.close();
```







#### 事务的使用

> 属于一个事务的基本要求：属于同一个连接对象

1. 关闭自动提交事务

   - 调用Connection对象的方法：`setAutoCommit(false)`

   > 每条SQL语句都相当于是一个事务：要么执行成功，要么执行失败回滚。
   >
   > 关闭事务后，就可以实现：要么多条SQL语句执行都成功，要么有一条SQL语句执行失败就回滚

2. 在 try catch 中体现对事务的操作：

   - 没有出现异常时，提交事务
     
     - 调用Connection对象的方法：`commit()`
   - 出现异常时，进行数据回滚

     - 调用Connection对象的方法：`rollback()`

       > - rollback方法内部在执行结束时，会关闭连接资源。
       > - 但是，finally中的代码会在rollback方法调用结束前，被执行。
       > - 且若在finally中已关闭了连接，则rollback内部的关闭连接操作会失效。



使用举例：银行转账

```java
// 基础操作类
public class BankDao {
    // 给对应账户加钱
    public void add(Connection connection, String addAccount, int money) throws Exception {
        String sql = "UPDATE t_bank SET money = money + ? WHERE account = ?";
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        preparedStatement.setObject(1, money);
        preparedStatement.setObject(2, addAccount);
        preparedStatement.executeUpdate();
        preparedStatement.close();
    }

    // 给对应账户减钱
    public void min(Connection connection, String minAccount, int money) throws Exception {
        String sql = "UPDATE t_bank SET money = money - ? WHERE account = ?";
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        preparedStatement.setObject(1, money);
        preparedStatement.setObject(2, minAccount);
        preparedStatement.executeUpdate();
        preparedStatement.close();
    }
}

// 账户操作类
public class Bank {
    // 给定转账的账户和转账金额，调用基础操作类进行转账
    public void tranMoney(String addAccount,String minAccount,int money) throws Exception {
        BankDao bankDao = new BankDao();
		// 同一个事务的基本要求：属于同一个连接对象
        Class.forName("com.mysql.cj.jdbc.Driver");
        Connection connection = DriverManager.getConnection("jdbc:mysql:///atguigu?user=root&password=123456");
		// 关闭自动提交事务
        connection.setAutoCommit(false);
		// 在try catch中体现事务
        try {
            bankDao.add(connection, addAccount, money);
            bankDao.min(connection, minAccount, money);
 			// 提交事务
            connection.commit();
            System.out.println("转账成功！");
        } catch (Exception e) {
            // 事务回滚
            connection.rollback();
            System.out.println("转账失败！");
        }finally{
            connection.close();
        }
    }
}
```

> 若要开启事务，则大概率会使用本地线程变量ThreadLocal来获取连接。原因：
>
> - 为了减少代码冗余，省去每个Servlet都进行事务管理，我们一般会将事务提取到Filter中
> - 在请求发送过来，Filter会拦截请求，并开启事务，然后放行
> - 而若不是将连接存放在ThreadLocal中，那么我们在开启事务获取连接后，就需要将connection传递到后续可能使用的DAO中
> - 而 doFilte() 并不能传递connection对象，且从 doFilter() 到我们调用DAO方法过程中，还存在并调用了许多不是我们声明的方法
> - 因此，只能考虑使用ThreadLocal来获取连接，因为这些方法的调用都是处于一个线程的，可以通过ThreadLocal对象获取到对应的唯一的值（即connection对象）







### Druid连接池

> 底层会一次性创建多个连接，并将连接存放在池子中，用于多次复用

- javax.sql.DataSource接口，是连接池的实现规范。

  规范了连接池获取连接的方法，以及连接池回收连接的方法

- Druid连接池的创建需要依赖阿里提供的第三方jar包：druid.jar



#### 基本步骤

1. 创建一个Druid连接池对象（DataSource接口的实现类对象）
2. 设置连接池参数
   - 必须参数：url、username、password、driverClassName（注册的驱动）
   - 非必须参数：initialSize（初始化连接数）、maxActive（最大连接数）
3. 获取连接：
   - 调用DataSource连接池对象的方法：【Connection】`getConnection()`
4. 回收连接：
   - 调用Connection连接对象的方法：`close()`





#### 硬编码方式

> 在程序中设置连接池参数（不推荐）

```java
// 创建Druid连接池对象
DruidDataSource dataSource = new DruidDataSource();
// 设置连接池参数
dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
dataSource.setUrl("jdbc:mysql:///atguigu");
dataSource.setUsername("root");
dataSource.setPassword("123456");
// 获取连接
Connection connection = dataSource.getConnection();
// 关闭连接
connection.close();

// 可用：dataSource.setUrl("jdbc:mysql:///atguigu?user=root&password=123456");
```





#### 软编码方式

> 在配置文件中设置连接池参数

```shell
# 在.properties文件中编写key-value
driverClassName=com.mysql.cj.jdbc.Driver
url=jdbc:mysql:///atguigu
username=root
password=123456
```

```java
// 创建Properties对象来加载.properties文件
Properties properties = new Properties();
// 创建.properties文件的输入流对象
FileInputStream fis = new FileInputStream("src/jdbc/druid/test1/druid.properties");
// 加载.properties文件
properties.load(fis);
// 创建Druid连接池对象
DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);
// 获取连接
Connection connection = dataSource.getConnection();
// 关闭连接
connection.close();
```

> 建议使用类加载器的方式获取文件输入流对象：
>
> `Class对象.getClassLoader().getResourceAsStream("properties文件名")`
>
> 原因是：
>
> - 在Maven中，配置文件是保存在 `src/main/resources` 目录下的
> - 当war被打包并部署到服务器上时，resources目录下的文件会被保存在 `WEB-INF/classes` 目录下
> - `WEB-INF/classes` 目录存放的还有编译好的 .class 字节码文件，该目录被我们称为类路径，是一个基准，无论在本地还是在服务器，都是一样的
> - 通过类加载器读取文件，就是以类路径为基准来读取的









### 工具类封装

#### 连接

> 线程本地变量：若在一个线程中，可以确保调用的线程本地变量中的值是同一个
>
> 调用线程本地变量（获取同一个连接）的好处：进行事务操作时，在不同的方法中，不再需要传递连接作为参数，可以直接通过该工具类获取到同一个连接对象

```java
// 声明私有属性DataSource连接池对象
private static DataSource dataSource = null;
// 声明私有属性ThreadLocal线程本地变量
private static ThreadLocal<Connection> threadLocal = new ThreadLocal<>();

// 软连接方式初始化DataSource连接池对象
static {
    Properties properties = new Properties();
    InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("druid.properties");
    try {
        properties.load(is);
        dataSource = DruidDataSourceFactory.createDataSource(properties);
    } catch (Exception e) {
        e.printStackTrace();
    }
}

// 获取连接对象
public static Connection getConnection() throws SQLException {
	// 从线程本地变量中，查看是否已存在连接
    // 若存在连接，则直接返回；若不存在，则添加进线程本地变量后，返回
    Connection connection = threadLocal.get();
    if (connection == null) {
        connection = dataSource.getConnection();
        threadLocal.set(connection);
    }
    return connection;
}

// 关闭连接对象
public static void closeConnection() throws SQLException {
    // 从线程本地变量中，查看是否已存在连接
    // 若存在，则关闭连接，并将存储在线程本地变量中的连接移除掉（可考虑是否将事务设置为自动提交）
    // 若没有及时移除本地线程变量，则下一次获取连接时，依旧能获取到ThreadLocal中的值，但连接已被回收
    Connection connection = threadLocal.get();
    if (connection != null) {
        connection.close();
        threadLocal.remove();
    }
}
```







#### BaseDAO

##### DQL

```java
public int urdOption(String sql,Object... args) throws SQLException {
    // 用上述获取连接的工具类来获取到连接
    Connection connection = connectionUtils.getConnection();
    // 创建Statement对象，并预编译SQL语句
    PreparedStatement preparedStatement = connection.prepareStatement(sql);
   	// 填充占位符（可变形参可按数组方式操作）
    for (int i = 1; i <= args.length ; i++) {
        preparedStatement.setObject(i,args[i-1]);
    }
    // 执行SQL语句，返回操作的条目数
    int rows = preparedStatement.executeUpdate();
    // 关闭资源
    preparedStatement.close();
    // 若是启动了事务，则不能关闭连接对象
    if(connection.getAutoCommit()){
        connectionUtils.closeConnection();
    }
    return rows;
}
```







##### DML

使用 List\<T> 来存储结果集，而不考虑使用 List\<Map> 来存储结果集

- 因为有多条数据，因此考虑使用 List 来存储
- Map中虽然key-value可以很好的表示，但value的值在添加时，不能自定义筛选（如：让 sex 只存放 “男” 等情况），而对象可以通过 get/set 方法进行自定义筛选

> MySQL中的每张表，可以对应为Java中的每一个类；表中的每一条数据，可以对应为Java中类的一个对象；数据中的每一列，可以对应为Java中类的对象中的每一个属性
>

```java
// 传入Class<T>的两个作用：①确定泛型（类）的类型 ②为后面的反射操作提供一个对应的Class类对象
public <T> List<T> cOption(Class<T> clazz,String sql,Object... args) throws Exception{
    // 创建集合，用于存放多个T类型对象
    ArrayList<T> list = new ArrayList<>();
    // 用上述获取连接的工具类来获取到连接
    Connection connection = connectionUtils.getConnection();
    // 创建Statement对象，预编译SQL语句
    PreparedStatement preparedStatement = connection.prepareStatement(sql);
    // 填充占位符
    for (int i = 1; i <= args.length; i++) {
        preparedStatement.setObject(i, args[i - 1]);
    }
    // 执行SQL语句，返回结果集
    ResultSet resultSet = preparedStatement.executeQuery();
    // 获取结果集中关于 列 的信息
    ResultSetMetaData metaData = resultSet.getMetaData();
    // 获取结果集中 列的条目数
    int columnCount = metaData.getColumnCount();
    // 循环取出每一行结果，将每行结果封装为一个对象，存储在集合中
    while (resultSet.next()) {
        // 利用Class<T>对象获取构造器
        Constructor<T> constructor = clazz.getDeclaredConstructor();
        // 设置为允许访问私有构造器
        constructor.setAccessible(true);
        // 调用空参构造器，创建一个对应的T类型的对象
        T obj = constructor.newInstance();
        // 循环取出每一列，将每一列的结果赋值给对象的属性
        for (int i = 1; i <= columnCount; i++) {
            // 获取列名
            String columnName = metaData.getColumnName(i);
            // 根据列名获取对象中对应的属性
            Field field = clazz.getDeclaredField(columnName);
            // 设置允许访问私有属性
            field.setAccessible(true);
            // 将对象属性的值设置为对应的列的值
            field.set(obj, resultSet.getObject(columnName));
        }
        list.add(obj);
    }
    return list;
}
```

