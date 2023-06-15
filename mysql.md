## MySQL安装与配置

1）下载：

登录官网mysql.com --> DOWNLOADS --> MySQL Community (GPL) Downloads（社区版） --> MySQL Community Server --> Go to Download Page --> Archives（历史版本） --> Download即可

> zip与msi的区别：zip安装后，还需要自己手动进行配置，而mis会在安装时提供配置选项，不用再自己手动配置



2）安装：

1. Custom（自定义安装）--> Next
2. MySQL Servers --> MySQL Server --> MySQL Server 8.0 --> MySQL Server 8.0-x64 --> 添加 --> 点击添加后的MySQL Server 8.0-x64 --> Advanced Options（更换安装路径）--> Next
3. Next --> Execute --> Next --> Next



3）配置：

1. Development Computer（开发者电脑，消耗资源较少）--> Port（端口号，不冲突就行）--> Next
2. Use Strong Password（新的数据加密方式）--> Next
3. 设置Root用户的密码 --> Next
4. Start the MySQL Server at System Startup（开机自启mysql服务）--> Next
5. Execute --> Finish --> Next --> Finish



4）配置环境变量：

右键此电脑 --> 高级系统设置 --> 环境变量 --> 系统变量 --> Path --> 新建：mysql安装目录\bin --> 确定

> 使用 `mysql --version` 可测试是否成功







## 基础部分

### 知识点

#### 系统相关

1）查看mysql版本

```mysql
① mysql -V
② mysql --version
# 在系统命令行中查看
```



2）登录mysql

```mysql
mysql -u 用户名 -P 端口号 -h IP地址 -p密码
# 端口号不指名时，默认使用3306，IP地址不指名时，默认使用localhost
# -p后若明文输入用户密码，则不能用空格隔开（或直接回车，使用暗文输入密码）
```



3）退出mysql

```mysql
① exit
② quit
```







#### 标准与规则

- 每条命令以`;`或者`\g`结束
- 字符串和日期时间类型的数据可以使用`''`（单引号）表示，列的别名可以使用`""`（双引号）表示
- Windows环境下，不区分大小写。Linux环境下，数据库名、表名、表的别名、变量名等区分大小写，关键字、函数名、列名、列的别名等忽略大小写





#### 注释

1. （单行注释）：`#注释文字`
2. （单行注释）：`-- 注释文字`
3. （多行注释）：`/*注释文字*/`





#### 导入或导出

```mysql
source 文件路径.sql
# 导入sql文件
```









### DDL

> 数据定义语言，如：create、alter、drop、rename、truncate
>
> DDL的操作一旦执行，就不可以再进行回滚

#### 数据库

##### 创建

`CREATE DATABASE 库名`：使用默认字符集来创建数据库，若存在，则报错

- `CREATE DATABASE 库名 CHARACTER SET '字符集'`：使用指定的字符集来创建数据库，若存在，则报错
- `CREATE DATABASE IF NOT EXISTS 库名`：使用默认字符集创建数据库，若存在，则不会创建

> 使用举例：
>
> ```mysql
> CREATE DATABASE IF NOT EXISTS test CHARACTER SET 'utf8';
> # 使用utf8字符集来创建数据库，若数据库存在，则不创建
> ```





##### 查看

`SHOW DATABASES`：查看当前创建的所有数据库

`SHOW TABLES`：查看当前数据库中所有的表

- `SHOW TABLES FROM 库名`：查看指定数据库下所有的表



`SELECT DATABASE() FROM DUAL`：查看当前使用的数据库

`SHOW CREATE DATABASE 库名`：查看创建的数据库的创建信息





##### 使用

`USE 数据库名`：切换数据库





##### 修改

`ALTER DATABASE 数据库名 CHARACTER SET '字符集'`：更改数据库的字符集





##### 删除

`DROP DATABASE 数据库名`：删除指定的数据库，若不存在则报错

- `DROP DATABASE IF EXISTS 库名`：若数据库存在，则删除









#### 表

##### 创建

1. `CREATE TABLE 表名(字段1 类型,字段2 类型, ... )`：根据表所在的数据库所使用的字符集来创建表，若存在，则报错

   - `CREATE TABLE IF NOT EXISTS 表名(字段1 类型, ...)`：若表不存在，则创建

   > 使用举例：
   >
   > ```mysql
   > CREATE TABLE IF NOT EXISTS test(
   > id INT,
   > name VARCHAR(10)
   > );
   > # 创建test表，内含id、name字段
   > ```

2. `CREATE TABLE 表名 AS SELECT 字段名 FROM 其他表名`：根据现有的表，创建指定的表，并导入查找到的数据

   - 查询语句中字段的别名，将作为创建的新表的字段名
- 该创建方式导入的表结构，不会继承原表 除非空约束、默认值约束外的其他约束
  
   > 使用举例：
   >
   > ```mysql
   > CREATE TABLE test AS
   > SELECT employee_id eid,last_name FROM employees WHERE department_id < 1000;
   > # 查询员工表中满足条件的数据，并将查询到的数据用来创建新表test
   > ```





##### 查看

`DESC 表名`：查看表结构

`SHOW CREATE TABLE 表名`：查看创建表时的创建信息





##### 修改

###### 添加字段

`ALTER TABLE 表名 ADD 字段名 数据类型`：在表的末尾处添加一个新字段

- `ALTER TABLE 表名 ADD 字段名 数据类型 FIRST`：在表的开头处添加一个新的字段
- `ALTER TABLE 表名 ADD 字段名 数据类型 AFTER 字段名`：在指定的字段后面添加一个新的字段



###### 修改字段

`ALTER TABLE 表名 MODIFY 字段名 数据类型`：修改指定的字段的数据类型、长度或默认值

`ALTER TABLE 表名 CHANGE 旧字段名 新字段名 数据类型`：重命名指定的字段，也可用来修改数据类型、长度、默认值



###### 删除字段

`ALTER TABLE 表名 DROP COLUMN 字段名`：删除指定的字段





##### 重命名

1. `RENAME TABLE 旧表名 TO 新表名`：重命名表名
2. `ALTER TABLE 旧表名 RENAME TO 新表名`：重命名表名





##### 删除与清空

`DROP TABLE IF EXISTS 表名`：删除指定的表

`TRUNCATE TABLE 表名`：清空指定的表中的数据（一旦执行，表中数据全部清除，且不可回滚）







#### MySQL8.0新特性

##### 原子化

```mysql
CREATE DATABASE test;			# 创建数据库
USE test;						# 使用数据库

CREATE TABLE book1(				# 创建表book1
book_id INT,
book_name VARCHAR(255)
);

SHOW TABLES;					# 查看所有的表（仅有表book1）
DROP TABLE book1,book2;			# 删除表book1、book2（book2表不存在）
SHOW TABLES;
# 表book2不存在，整体删除失败，表book1不会被删除
```









### DML

> 数据操作语言，如：insert、delete、update、select
>
> DML的操作默认在执行后，将不可以回滚。但若在执行DML之前，执行了 `SET autocommit = FALSE`，则执行DML的操作就可以实现回滚

#### 查询

##### 基本用法

###### 基本语法

`SELECT 字段1,字段2 FROM 表名`：从指定的表中查询字段1、字段2的结果集

- `SELECT * FROM 表名`：代表查询表中的所有字段所对应的值

>```mysql
>① SELECT 1+2,3+4;
>② SELECT 1+2,3+4 FROM DUAL;
># 可得到（字段）1+2、3+4的值
># 由于字段1+2、3+4并不存在于任何一张表中，因此可以使用伪表dual
>```





###### 列的别名

`SELECT 字段1 别名1,字段2 AS 别名2,字段3 "别名3" FROM 表名`：从指定的表中查询字段1、字段2、字段3，并分别为这些字段起对应的别名

- `AS`一般可以进行省略，`""`一般用于别名中有空格时

> 使用举例：
>
> ```mysql
> SELECT employee_id eid,last_name AS name,salary "工资" FROM employees;
> # 插叙员工id、姓名和工资，并在返回的结果集中显示特定的别名
> ```





###### 显示表结构

`DESCRIBE 表名`：显示表中字段的详细信息

- `DESC 表名`：简化版





##### 去除重复行

`SELECT DISTINCT 字段名 FROM 表名`：去除字段值重复的数据

- `SELECT DISTINCT 字段1,字段2 FROM 表名`：去除字段1和字段2中，两个字段值都相同的数据

> 使用举例：
>
> ```mysql
> SELECT DISTINCT department_id FROM employees;
> # 找出员工所在的所有的部门
> ```





##### WHERE过滤

`SELECT 字段名 FROM 表名 WHERE 条件`：查询表中 满足了指定的条件后 所构成的结果集

- WHERE不能根据字段的别名进行筛选
- WHERE不能的过滤条件不能使用聚合函数
- WHERE声明在FROM之后，ORDER BY之前

> 使用举例：
>
> ```mysql
> SELECT * FROM employees WHERE salary > 10000;
> # 查询员工表中工资大于10000的
> ```





##### 排序

`SELECT 字段1,字段2 FROM 表名 ORDER BY 字段名 排序方式`：根据指定的字段，按照指定的排序方式进行排序

- 排序方式：
  1. ASC：升序（ascend，默认的排序方式）
  2. DESC：降序（descend）
- 排序的字段可以不在结果集中显示
- 可以根据别名进行排序，且别名只能在ORDER BY中使用，不能在WHERE中使用
- 可以有多级排序：先根据第一个排序，若出现相同时，再根据之后的进行排序。且每级可以是不同的排序方式

> 使用举例：
>
> ```mysql
> SELECT last_name,salary*12 all_salary FROM employees ORDER BY all_salary DESC,last_name ASC;
> # 先根据12个月的工资，从高到低进行排序，若出现相同工资时，再根据姓名进行升序排序
> ```





##### 分页

`SELECT 字段名 FROM 表名 LIMIT 偏移量,条目数`：从指定的位置开始，显示指定条目数的结果集

- 偏移量 = （当前要显示的页数-1）*条目数 = 要显示的第x条数据-1
- 当偏移量为0时，可以简化为：`LIMIT 条目数`
- LIMIT声明在最后
- MySQL8.0新特性：`LIMIT 条目数 OFFSET 偏移量`

> 使用举例：
>
> ```mysql
> SELECT last_name,salary FROM employees WHERE salary>5000 ORDER BY salary LIMIT 10,5;
> # 筛选出工资大于5000的，并按工资由低到高排序，然后进行分页：每页显示5条数据，显示第3页
> 
> SELECT last_name,salary FROM employees WHERE salary>5000 ORDER BY salary LIMIT 5 OFFSET 10;
> # 两者等价
> ```





##### 多表查询

###### 基本使用

`SELECT 字段1,字段2 FROM 表1,表2 WHERE 连接条件`：根据连接条件，将表与表之间连接起来

- 若查询的字段在多张表中出现，则要明确说明该查询的字段是属于哪张表的
- 可以为表起别名。一旦为表起了别名，则在其他地方使用了该表名的话，必须是用表的别名代替
- 若有n张表要实现多表查询，则至少要有n-1个连接条件

> 使用举例：
>
> ```mysql
> SELECT e.employee_id,e.departmant_id,d.department_name FROM employees e,departments d WHERE e.department_id = d.department_id;
> # 根据部门id，连接员工表和部门表，并获取员工id、部门id以及部门名称
> ```



###### 等值与非等值连接

1. 等值连接

   ```mysql
   SELECT e.employee_id,d.department_name FROM employees e,departments d WHERE e.department_id = d.department_id;
   # 连接条件是：员工表的部门id = 部门表的部门id
   ```

2. 非等值连接

   ```mysql
   SELECT e.employee_id,e.salary,j.grade_level FROM employees e,job_grades j WHERE e.salary BETWEEN j.lowest_sal AND j.highest_sal;
   # 工资等级表中的lowest_sal、highest_sal刻画的是一定范围内的工资
   # 因此，连接条件是：员工表中的工资是对应着 处于工资等级表中最低工资和最高工资范围内的
   ```



###### 自与非自连接

1. 自连接

   ```mysql
   SELECT e.employee_id,d.department_name FROM employees e,departments d WHERE e.department_id = d.department_id;
   # 连接条件是：员工表的部门id = 部门表的部门id
   ```

2. 非自连接

   ```mysql
   SELECT e.employee_id,e.last_name,m.employee_id,m.last_name FROM employees e,employees m WHERE e.manager_id = m.employee_id;
   # 员工表中的员工的manager_id刚好也是员工表中某些员工的employee_id
   # 因此，连接条件是：员工表中的管理者id 是员工表中对应员工的id
   ```



###### 内与外连接

> 由于MySQL不能很好的支持SQL92的语法，因此使用SQL99的语法演示

- 内连接：结果集中仅有在两张表中匹配的数据
- 外连接：结果集中除了有两张表都匹配的数据外，还包括左表或右表中的数据

共计七种连接方式：

1. |A`|B|`C|（B）：内连接

   `SELECT 字段名 FROM 表名1 INNER JOIN 表名2 ON 连接条件`（INNER可以省略）

   ```mysql
   SELECT e.employee_id,d.department_name,l.city FROM employees e JOIN departments d ON e.department_id = d.department_id INNER JOIN locations l ON d.location_id = l.location_id;
   # 查询员工表中部门id与部门表中部门id相互对应、部门表中location_id和位置表中location_id相互对应 的数据
   # 使用SQL99的方式进行内连接
   ```

2. `|A|B|`C|（AB）：左外连接

   `SELECT 字段名 FROM 表名1 LEFT OUTER JOIN 表名2 ON 连接条件`（OUTER可以省略）

   ```mysql
   SELECT last_name,department_name FROM employees e LEFT JOIN departments d ON e.department_id = d.department_id;
   # 根据员工表部门id与部门表部门i，将员工表与部门表进行连接，并查询所有员工的名字和所在的部门名称
   # 使用SQL99的方式进行左外连接
   ```

3. |A`|B|C|`（BC）：右外连接

   `SELECT 字段名 FROM 表名1 RIGHT OUTER JOIN 表名2 ON 连接条件`（OUTER可以省略）

   ```mysql
   SELECT last_name,department_name FROM employees e RIGHT JOIN departments d ON e.department_id = d.department_id;
   # 根据员工表部门id与部门表部门i，将员工表与部门表进行连接，并查询所有的部门名称和部门中的员工名称
   # 使用SQL99的方式进行左外连接
   ```

4. `|A|`B|C|（A）

   `SELECT 字段名 FROM 表名1 LEFT JOIN 表名2 ON 连接条件 WHERE 表名2.特有字段 IS NULL`

   ```mysql
   SELECT employee_id,department_name FROM employees e LEFT JOIN departments d ON e.department_id = d.department_id WHERE d.department_id IS NULL;
   # 查询员工表中没有部门的员工
   # 先左外连接，得出员工表中所有的数据，再去掉所有有部门id的，剩下的就是没有部门的员工
   ```

5. |A|B`|C|`（C）

   `SELECT 字段名 FROM 表名1 RIGHT JOIN 表名2 ON 连接条件 WHERE 表名1.特有字段 IS NULL`

   ```mysql
   SELECT employee_id,department_name FROM employees e RIGHT JOIN departments d ON e.department_id = d.department_id WHERE e.employee_id IS NULL;
   # 查询部门表中没有员工部门
   # 先右外连接，得出部门表中所有的数据，再去掉所有部门中有员工id的，剩下的就是没有员工的部门
   ```

6. `|A|`B`|C|`（AC）（情况4和情况5的结合）

   `SELECT 字段名 FROM 表名1 LEFT JOIN 表名2 ON 连接条件 WHERE 表名2.特有字段 IS NULL UNION ALL SELECT 字段名 FROM 表名1 RIGHT JOIN 表名2 ON 连接条件 WHERE 表名1.特有字段 IS NULL`

7. `|A|B|C|`（ABC）：满外连接（情况2和情况5 或 情况3和情况4 的结合）

   `SELECT 字段名 FROM 表名1 RIGHT JOIN 表名2 ON 连接条件 UNION ALL SELECT 字段名 FROM 表名1 LEFT JOIN 表名2 ON 连接条件 WHERE 表名2.特有字段 IS NULL`（情况3和情况4的结合）

   

###### 两个SQL99新特性

1）自然连接：自动对两张表中相同的字段进行等值连接

```mysql
SELECT last_name,department_name FROM employees e JOIN departments d ON e.department_id = d.department_id AND e.manager_id = d.manager_id;
# SQL92中的写法

SELECT last_name,department_name FROM employees e NATURAL JOIN departments d;
# SQL99新特性写法
```

2）USING连接：自动根据指定的字段进行两张表间的等值连接

```mysql
SELECT last_name,department_name FROM employees e JOIN departments d ON e.department_id = d.department_id;
# SQL92中的写法

SELECT last_name,department_name FROM employees e JOIN departments d USING(department_id);
# SQL99新特性写法
```





##### 分组

`SELECT 字段名 FROM 表名 GROUP BY 字段名`：根据指定的字段名进行分组

- SELECT中出现的 非聚合函数的字段 必须声明在GROUP BY中，GROUP BY中声明的字段可以不出现在SELECT中
- 声明在WHERE后面，ORDER BY前面
- 多级分组 与 只有一级分组 的效果相同
- MySQL8.0新特性：`SELECT 字段名 FROM 表名 GROUP BY 字段名 WITH ROLLUP`，会在分组后，将整个表看成一组进行计算

> 使用举例：
>
> ```mysql
> SELECT department_id,AVG(salary) FROM employees GROUP BY department_id;
> # 按部门进行分组后求出每个部门中员工的平均值
> 
> SELECT department_id,job_id,AVG(salary) FROM employees GROUP BY job_id,department_id;
> # 先按工种分组，再按部门分组，然后求出每个部门中员工的平均值
> 
> SELECT department_id,AVG(salary) FROM employees GROUP BY department_id WITH ROLLUP;
> # 按部门分组后，求出每个部门中员工的平均值，再求出全体员工的平均值
> ```





##### HAVING过滤

`SELECT 字段名 FROM 表名 GROUP BY 字段名 HAVING 条件`：对分组后的内容进行过滤

- 过滤条件中使用了聚合函数，则必须使用HAVING来代替WHERE
- HAVING声明在GROUP BY后面，ORDER BY前面
- 一般与GROUP BY搭配使用，否则没有意义
- 若过滤条件中没有使用聚合函数，则使用WHERE来代替HAVING，效率更高

> 使用举例：
>
> ```mysql
> SELECT department_id,MAX(salary) FROM employees GROUP BY department_id HAVING MAX(salary)>10000;
> # 查询部门最高工资大于10000的
> ```





##### 总结

1. `\G`、`\g`可以使将表中的数据按列的方式展示

2. 着重号（` `` `），若查询的字段或表名与关键字或保留字重名时，则需要使用着重号来表示非关键字

3. 若查询的是常数（或常数进行运算）而非字段时，返回的结果集中的每条数据都会添加该常数

4. `UNION`和`UNION ALL`都是联合语句的作用，UNION会额外对返回的结果集进行去重操作。该关键字要求：联合的语句多条语句的结果集中，都具有相同数量的字段，以及字段的类型要匹配。

5. SQL99语法下的MySQL查询模板：

   ```mysql
   SELECT 字段名（存在聚合函数、DISTINCT()）
   FROM 表名1 （LEFT/RIGHT）JOIN 表名2 ON 连接条件
   WHERE 不含聚合函数的过滤条件
   GROUP BY 字段名
   HAVING 含聚合函数的过滤条件
   ORDER BY 字段名 （ASC/DESC）
   LIMIT 偏移量,条目数
   
   # 除了GROUP BY和LIMIT外，其他地方都可以使用子查询
   
   # 执行流程：  
   FROM ---> ON --> LEFT/RIGHT JOIN --> WHERE --> GROUP BY --> HAVING --> SELECT --> DISTINCT --> ORDER BY --> LIMIT
   ```







##### 子查询

> 外查询（主查询）和内查询（子查询）
>
> - 子查询在主查询之前执行完成
> - 子查询的结果被主查询使用
> - 若子查询的结果作为一张表出现，且要使用该表返回的字段时，则要为该表和其字段起别名
> - 单行操作符对应单行子查询，多行操作符对应多行子查询
> - 使用技巧：
>   - 若子查询相对简单，则可以从外往里写。若情况较为复杂，则考虑从里往外写
>   - 当查询类型为相关子查询时，考虑从外往里写

子查询可分为：

1. 单行子查询与多行子查询（从返回结果的条目数来分类）

   - 单行子查询：返回一条数据
- 多行子查询：返回多条数据
  
2. 相关子查询与不相关子查询（从内查询是否被多次执行来分类）

   - 相关子查询：内查询被多次执行

     > 如：查询 工资大于 员工所在部门的平均工资 的员工信息
     >
     > - 员工所在的部门可能不同，所在的部门的平均工资也就不同

   - 不相关子查询：内查询仅执行一次

     > 如：查询 工资大于 公司的平均工资 的员工信息
     >
     > - 公司的平均工资一定



###### 单行子查询

- 单行子查询操作符：=、!=、>、>=、<、<=

> 使用举例：
>
> ```mysql
> SELECT employee_id,manager_id,department_id
> FROM employees
> WHERE manager_id = (
> 					SELECT manager_id
> 					FROM employees
> 					WHERE employee_id = 141
> 					)
> AND department_id = (
> 					SELECT department_id
> 					FROM employees
> 					WHERE employee_id = 141
> 					)
> AND employee_id != 141;
> # 查询 与141员工的manager_id 和 department_id 都相同的 其他员工的信息
> ```
>
> ```mysql
> SELECT department_id,MIN(salary)
> FROM employees
> GROUP BY department_id
> HAVING MIN(salary)>(
> 					SELECT MIN(salary)
> 					FROM employees
> 					WHERE department_id = 50
> 					);
> # 查询最低工资大于 50号部门最低工资的员工 的部门id和最低工资
> ```
>
> ```mysql
> SELECT employee_id,last_name,CASE WHEN department_id = (
> 											SELECT department_id
> 											FROM departments
> 											WHERE location_id = 1800
> 													) THEN 'Canada'
> 								ELSE 'USA' END "location"
> FROM employees;
> # 查询员工的信息和location，其中，若员工department_id与 location_id为1800的department_id相同，则location为Canada，否则为USA
> ```





###### 多行子查询

- 多行子查询操作符：
  1. `IN`：与子查询返回的结果集中的 每一条数据都相互匹配
  2. `ANY`、`SOME`：与单行操作符一起使用，匹配子查询返回的结果集中的 某一条数据
  3. `ALL`：与单行操作符一起使用，匹配子查询返回的结果集中的 所有数据

> 使用举例：
>
> ```mysql
> SELECT employee_id,last_name,job_id,salary
> FROM employees
> WHERE job_id != 'IT_PROG' 
> AND salary <ANY(
> 				SELECT salary 
> 				FROM employees
> 				WHERE job_id = 'IT_PROG'
> 				);
> # 返回其他job_id中，比job_id为IT_PROG的任一工资低（比其中一个低即可） 的员工的信息
> ```
>
> ```mysql
> # 查询平均工资最高的job信息
> 
> # 方式一：
> SELECT *
> FROM jobs
> WHERE job_id = (
> 				SELECT job_id
> 				FROM employees 
> 				GROUP BY job_id
> 				HAVING AVG(salary) = (
> 									SELECT MAX(avg_sal)
> 									FROM (
> 											SELECT AVG(salary) avg_sal
> 											FROM employees
> 											GROUP BY job_id
> 											) t_tmp
> 									)
> 				);
> 
> # 方式二：
> SELECT * 
> FROM jobs
> WHERE job_id = (
> 				SELECT job_id 
> 				FROM employees
> 				GROUP BY job_id
> 				HAVING AVG(salary) = (
> 									SELECT AVG(salary)
> 									FROM employees
> 									GROUP BY job_id
> 									HAVING AVG(salary) >=ALL(
> 															SELECT AVG(salary)
> 															FROM employees
> 															GROUP BY job_id
> 															)
> 									)
> 				);
> 
> # 方式三：
> SELECT *
> FROM jobs j JOIN (
> 				SELECT job_id,AVG(salary) avg_sal
> 				FROM employees
> 				GROUP BY job_id
> 				ORDER BY avg_sal DESC
> 				LIMIT 1
> 				) t_tmp
> ON j.job_id = t_tmp.job_id;
> 
> # 先按job_id分类，获取到每种job_id的平均工资，再求出平均工资中最高的，获取其job_id，最后根据job_id获取到所有信息
> ```





###### 相关子查询

- 将外表传入内部，让外表的某个条件与内表中的某个条件进行配对

> 使用举例：
>
> ```mysql
> SELECT last_name,salary,department_id
> FROM employees e1
> WHERE salary > (
> 				SELECT AVG(salary)
> 				FROM employees e2
> 				WHERE e2.department_id = e1.department_id
> 				);
> # 查询员工中，工资大于 所在部门平均工资 的员工的信息
> 
> # 员工所在的部门不同，所在部门的平均工资也就不同，因此先获取员工所在部门的平均工资，在求出该部门中员工工资大于该平均工资的
> ```





###### EXIST关键字

- 当外表传入内部，与内表的某些条件进行逐一配对时，若配对成功，则保留该数据

> 使用举例：
>
> ```mysql
> SELECT employee_id,last_name,job_id,department_id
> FROM employees e1
> WHERE EXISTS(
> 			SELECT *
> 			FROM employees e2
> 			WHERE e1.employee_id = e2.manager_id
> 			);
> # 查询公司管理者的信息
> 
> # 外表作为管理表，当外表中某个员工的id是内表（员工表）中某个管理者的id时，说明该管理者存在
> 
> SELECT DISTINCT e2.employee_id,e2.last_name,e2.job_id,e2.department_id
> FROM employees e1 JOIN employees e2 
> ON e1.manager_id = e2.employee_id;
> # 两者等价
> ```
>
> ```mysql
> SELECT department_id,department_name
> FROM departments d
> WHERE NOT EXISTS(
> 				SELECT * 
> 				FROM employees e
> 				WHERE d.department_id = e.department_id
> 				);
> # 查询部门表中，不存在于员工表中的部门 的部门信息
> # 若员工表中的department_id与部门表中的department_id相同，说明该部门id中存在员工，则不会选择
> ```









#### 添加

1. `INSERT INTO 表名 VALUES(字段值)`：按照声明的字段，按顺序依次添加对应的值

   - `INSERT INTO 表名(字段名) VALUES(字段值)`：按照指定的字段，按顺序依次添加对应的值

     > 使用举例：
     >
     > ```mysql
     > INSERT INTO employees(employee_id,last_name,salary)
     > VALUES (111,"Tom",11100),(222,"Jack",NULL);
     > # 往员工表中依次插入2条数据
     > ```

2. `INSERT INTO 表名(字段名) SELECT 字段名 FROM 表名`：将查询到的结果插入到指定表中

   - 查询的字段要与添加的表中的字段一一对应

   - 要确保添加到表中数据的所能存储的长度足够（如：VARCHAR、DOUBLE等类型）

     > 使用举例：
     >
     > ```mysql
     > INSERT INTO emp(id,name,salary)
     > SELECT employee_id,last_name,salary
     > FROM employees
     > WHERE department_id IN(60,70);
     > # 查询employees表中员工的id、last_name、salary，并将查询到的结果插入到emp表中对应的字段：id、name、salary
     > ```





#### 修改

`UPDATE 表名 SET 字段名 = 值`：修改指定的字段的值

- 可以使用WHERE定位到具体的字段，或不使用WHERE对所有字段进行修改

  > 使用举例：
  >
  > ```mysql
  > UPDATE employees
  > SET hire_date = NOW(),last_name = 'Tom'
  > WHERE id = 1;
  > # 修改员工表中，id为1的员工的hire_date和last_name
  > ```





#### 删除

`DELETE FROM 表名`：删除表中的所有数据（可以支持回滚）

- 可以使用WHERE来删除仅满足条件的数据

  > 使用举例：
  >
  > ```mysql
  > DELETE FROM employees
  > WHERE id = 1;
  > # 删除员工表中id为1的员工的信息
  > ```

  





#### MySQL8.0新特性

##### 计算列

```mysql
CREATE TABLE test(							# 创建表
a INT,
b INT,
c INT GENERATED ALWAYS AS (a+b) VIRTUAL		# 计算列，该列的结果为a和b字段的和
);

INSERT INTO test(a,b) VALUE(10,20);			# 插入数据				
# 计算列c的结果为：30
# 插入数据时，计算列会根据所指定的字段进行计算后添加值

UPDATE test SET a = 100;					# 修改数据
# 计算列c的结果为：100
# 修改数据时，计算列会根据被修改的字段重新计算结果
```









### DCL

> 数据控制语言，如：commit、rollback、savepoint、grant、revoke

#### COMMIT与ROLLBACK

COMMIT：提交数据。一旦执行COMMIT，则数据就会永久保存在数据库中（数据就不能再回滚）

ROLLBACK：回滚数据。一旦执行ROLLBACK，则可以实现数据的回滚（回滚到最近一次COMMIT之后）

> 使用举例：
>
> ```mysql
> COMMIT;						# 提交数据
> SET autocommit = FALSE;		# 设置数据可以被回滚
> DELETE FROM employees;		# 清空表中的所有数据（此操作可以被回滚）
> ROLLBACK;					# 回滚数据到最近一次COMMIT之后
> TRUNCATE TABLE employees;	# 清空表中的所有数据（此操作不可以被回滚）
> ```









### 运算符

#### 算术运算符

|  运算符  |        名称        |                             特点                             |
| :------: | :----------------: | :----------------------------------------------------------: |
|    +     |     加法运算符     | 加上字符串时，会进行隐式转换（即：`'1'`-->`1`）。若不能隐式转换，则看做`0`处理） |
|    -     |     减法运算符     |                            同加法                            |
|    *     |     乘法运算符     |         整型 * 整型 = 整型 ；整型 \* 浮点型 = 浮点型         |
| /（DIV） |     除法运算符     | 整型 / 整型 = 浮点型 ；整型 / 浮点型 = 浮点型（浮点型保留小数点后4位）；0作为除数时，结果为NULL |
| %（MOD） | 取模（取余）运算符 |            取模结果为正数还是负数，取决于被取余数            |

注意：空值（`NULL`），不等价于`0`或`''`。若空值参与运算时，结果一定为空





#### 比较运算符

`=`（等于）：数值与字符串进行比较，若字符串不存在隐式转换，则看做0；若两边都是字符串，则按照字符串的比较规则进行比较；若空值参与运算时，结果为空

> 使用举例：
>
> ```mysql
> SELECT 0='a',1='1','a'='b',NULL=NULL FROM DUAL;	# 1,1,0,NULL
> ```



`<=>`（安全等于）：两边一方为NULL，则为0；两边都为NULL，则为1 

> 使用举例：
>
> ```mysql
> SELECT 1<=>NULL,NULL<=>NULL FROM DUAL;	# 0,1
> ```



`IS NULL`、`ISNULL(字段)`：若为NULL，则为1

> 使用举例：
>
> ```mysql
> ① SELECT commission_pct FROM employees WHERE commission_pct IS NULL;
> ② SELECT commission_pct FROM employees WHERE ISNULL(commission_pct);
> ③ SELECT commission_pct FROM employees WHERE commission_pct <=> NULL;
> # 三者等价
> ```



`IS NOT NULL`：若不为NULL，则为1

> 使用举例：
>
> ````mysql
> ① SELECT commission_pct FROM employees WHERE commission_pct IS NOT NULL;
> ② SELECT commission_pct FROM employees WHERE NOT ISNULL(commission_pct);
> ③ SELECT commission_pct FROM employees WHERE NOT commission_pct <=> NULL;
> # 三者等价
> ````



`LEAST(字段1,字段2)`、`GREATEST(字段1,字段2)`：获取最小值/最大值

> 使用举例：
>
> ```mysql
> SELECT LEAST(3,2,4),GREATEST('c','e','d') FROM DUAL;	# 2,e
> ```



`BETWEEN 下界条件 AND 上界条件`：包含指定条件的结果集（包含条件）

> 使用举例：
>
> ```mysql
> SELECT last_name,salary FROM employees WHERE salary BETWEEN 6000 AND 8000;
> # 查询工资大于6000并且小于8000的员工
> ```



`IN(条件1,条件2)`、`NOT IN(条件1,条件2)`：获取 包含指定条件/不包含指定条件 的结果集

> 使用举例：
>
> ```mysql
> SELECT last_name,department_id FROM employees WHERE department_id IN(10,20,30);
> # 查询部门id为10、20、30的员工
> 
> SELECT last_name,department_id FROM employees WHERE department_id = 10 OR department_id = 20 OR department_id = 30;
> # 两者等价
> ```



`LIKE`：模糊查询

- `%`：表示0个或多个字符不确定的字符
- `_`：表示1个不确定的字符
- `\`：表示转义（或者：使用`ESCAPE '字符'`来表示该字符代表转义字符）

> 使用举例：
>
> ```mysql
> SELECT last_name FROM employees WHERE last_name LIKE '_\_%a%';
> # 查找员工中，姓名第二个为_的
> 
> SELECT last_name FROM employees WHERE last_name LIKE '_*_%a%' ESCAPE '*';
> # 两者等价
> ```



`REGEXP`、`RLIKE`：匹配正则表达式

> 使用举例：
>
> ```mysql
> SELECT 'Amy' REGEXP '^A' FROM DUAL;
> # 是否以A开头
> ```





#### 逻辑运算符

|   运算符   | 名称 |                      特点                      |
| :--------: | :--: | :--------------------------------------------: |
| AND（&&）  |  与  |    条件1和条件2都要满足（AND优先级高于OR）     |
| OR（\|\|） |  或  |              条件1或条件2满足其一              |
|  NOT（!）  |  非  |                    结果取反                    |
|    XOR     | 异或 | 条件1满足时条件2不满足；条件2满足时条件1不满足 |





#### 按位运算符

| 运算符 |   名称   |        特点        |
| :----: | :------: | :----------------: |
|   &    |  按位与  |  同1则0，否则为0   |
|   \|   |  按位或  |  一1则1，否则为0   |
|   ^    | 按位异或 |  相同为0，否则为1  |
|   ~    | 按位取反 |     0变1，1变0     |
|   <<   | 按位左移 | 满足一定条件下，*2 |
|  \>>   | 按位右移 | 满足一定条件下，/2 |









### 函数

#### 单行函数

>接收一个参数，返回一个结果

##### 基本数值类

###### 数学

`ABS(x)`：返回x的绝对值

`SIGN(x)`：返回x的符号（正数返回1，负数返回-1，返回0）

`PI()`：返回圆周率的值

`MOD(x,y)`：返回x/y之后的余数

`SQRT(x)`：返回x的平方根（若x为负数，则返回NULL）

`POW(x,y)`、`POWER(x,y)`：返回x的y次方





###### 最值

`LEAST(x1,x2,x3...)`：取其中的最小值

`GREATEST(x1,x2,x3...)`：取其中的最大值





###### 随机数

`RAND()`：返回0~1间的随机值

`RAND(x)`：返回0~1间的随机值，x为种子值，用相同的种子值取出的随机数相同





###### 取整

`CEIL(x)`、`CEILING(x)`：返回x的最大整数（取天花板数）

`FLOOR(x)`：返回x的最小整数（取地板数）



`ROUND(x)`：对x进行四舍五入

`ROUND(x,y)`：对x进行四舍五入，保留小数点后y位（若y为负数，则对整数部分进行四舍五入）

`TRUNCATE(x,y)`：对x进行截断，保留小数点后y位（若y为负数，则对整数部分进行四舍五入）

`FORMAT(x,n)`：对x进行格式化，n表示进行四舍五入保留至小数点后n位（若n为负数，则四舍五入到整数部分）





###### 进制

`BIN(x)`：返回x的二进制数

`OCT(x)`：返回x的八进制数

`HEX(x)`：返回x的十六进制数

`CONV(x,f1,f2)`：返回x作为f1进制数时，转换为f2进制数后的值







##### 字符串类

> MySQL的字符串，索引下标从1开始

###### 比较

`STRCMP(s1,s2)`：根据字符串s1、s2的ASCII值，比较两者的大小

`NULLIF(s1,s2)`：比较两个字符串，若相等，则返回NULL，若不相等，则返回s1





###### 查找

`LOCATE(x,s)`、`POSITION(x IN s)`、`INSTR(s,x)`：返回x在字符串中首次出现的位置下标（若未找到，返回0）





###### 求长

`CHAR_LENGTH(s)`、`CHARACTER_LENGTH(s)`：返回字符串的字符个数

`LENGTH(s)`：返回字符串的字符集个数





###### 连接

`CONCAT(s1,s2,s3...)`：连接字符串

`CONCAT_WS(x,s1,s2,s3...)`：连接字符串，中间用x隔开





###### 替换

`INSERT(s,index,length,x)`：将字符串s的指定下标开始，指定长度间的字符串，替换为x

`REPLACE(s,x,y)`：将字符串s中，所有的字符串x替换为字符串y





###### 转换

`REVERSE(s)`：将字符串进行反转



`UPPER(s)`、`UCASE(s)`：将字符串中所有的字母转换为大写字母

`LOWER(s)`、`LCASE(s)`：将字符串中所有的字母转换为小写字母





###### 取值

`SUBSTR(s,index,length)`：返回字符串s中，从指定下标开始，指定长度之间的字符串

`ASCII(s)`：返回字符串中第一个字符的ASCII



`LEFT(s,n)`：返回字符串中最左边的n个字符

`RIGHT(s,n)`：返回字符串中最右边的n个字符



`ELT(i,s1,s2,s3...)`：返回指定位置的字符串（如：i=1，返回s1）

`FIELD(s,s1,s2,s3...)`：返回字符串s在字符串列表中首次出现的位置

`FIND_IN_SET(s1,s2)`：返回字符串s1在字符串s2中首次出现的位置，其中，字符串s2是一个以`,`进行分隔的字符串





###### 填充

`REPEAT(s,n)`：返回字符串重复n次后的结果

`SPACE(n)`：返回n个空格



`LPAD(s,length,x)`：用x对字符串的最左边进行填充，直到字符串的长度为length

`RPAD(s,length,x)`：用x对字符串的最右边进行填充，直到字符串的长度为length





###### 去重

`TRIM(s)`：去除字符串首位空格

`LTRIM(s)`：去除字符串首部空格

`RTRIM(s)`：去除字符串尾部空格

`TRIM(x FROM s)`：去除字符串首位的x

`TRIM(LEADING x FROM s)`：去除字符串首部的x

`TRIM(TRAILING x FROM s)`：去除字符串尾部的x







##### 日期时间类

###### 获取时间

`CURDATE()`、`CURRENT_DATE()`：获取当前日期（年月日）

`CURTIME()`、`CURRENT_TIME()`：获取当前时间（时分秒）

`NOW()`、`SYSDATE()`、`CUEEENT_TIMESTAMP()`、`LOCALTIME()`、`LOCALTIMESTAMP()`：获取当前日期和时间（年月日时分秒）





###### 转换

`UNIX_TIMESTAMP()`：以UNIX时间戳的形式返回当前时间

`UNIX_TIMESTAMP(date)`：以UNIX时间戳的形式返回指定的时间

`FROM_UNIXTIME(time)`：将UNIX时间戳还原为普通格式的时间



`TIME_TO_SEC(date)`：将指定时间中的时分秒转换为秒数

`SEC_TO_TIME(time)`：将指定的秒数转换为时分秒



`DATE_FORMAT(date,type)`：按照指定type格式化日期

`STR_TO_DATE(str,type)`：按照指定type解析字符串为日期

- type：

  `%Y`（4位数年）、`%y`（2位数年）

  `%m`（2位数月）、`%M`（月份名）、`%b`（缩写月名）、`%c`（1位数月）

  `%d`（2位数天）、`%D`（天名）、`%e`（1位数天）

  `%H`（2位数24小时制）、`%h`（2位数12小时制）、`%k`（1位数24小时制）、`%l`（1位数12小时制）

  `%i`（2位数分钟）

  `%S`、`%s`（2位数秒）

  `%W`（一周星期名）、`%a`（一周星期名缩写）、`%w`（一周星期名数字表示，0表示Sunday）

  `%j`（3位数年中的天）





###### 运算

`DATE_ADD(date,INTERVAL x type)`、`ADDDATE(date,INTERVAL x type)`：为指定的时间的某类型加上指定的值（若加的值为负数，则相当于减去）

`DATE_SUB(date,INTERVAL x type)`：为指定的时间的某类型减少指定的值

- type：

  `YEAR`、`MONTH`、`DAY`、`HOUR`、`MINUTE`、`SECOND`：年、月、日、时、分、秒

  `YEAR_MONTH`、`DAY_HOUR`、`DAY_MINUTE`、`DAY_SECOND`、`HOUR_MINUTE`、`HOUR_SECOND`、`MINUTE_SECOND`：年-月、日-时、日-分、日-秒、时-分、时-秒、分-秒



`ADDTIME(date1,date2)`：date1的时分秒加上date2的时分秒。当date2为数字时，代表秒（可为负数）

`SUBTIME(date1,date2)`：date1的时分秒减去date2的时分秒

`DATEDIFF(date1,date2)`：返回两个日期间隔的天数（date1-date2）

`TIMEDIFF(date1,date2)`：返回两个日期间隔的时分秒

`FROM_DAYS(n)`：返回从0000年1月1日起，n天以后的日期

`TO_DAYS(date)`：返回日期距离0000年1月1日的天数

`LAST_DAY(date)`：返回所在月份的最后一天的日期

`MAKEDATE(year,n)`：根据给定的年份与所在年份的天数，返回一个日期

`MAKETIME(hour,minute,second)`：根据给定的时分秒，组合成一个时间

`PERIOD_ADD(time,n)`：返回time加上n后的时间





###### 其他获取

`YEAR(date)`、`EXTRACT(YEAR FROM date)`：获取指定时间的年

`MONTH(date)`、`EXTRACT(MONTH FROM date)`：获取指定时间的月

`DAY(date)`、`EXTRACT(DAY FROM date)`：获取指定时间的日

`HOUR(date)`、`EXTRACT(HOUR FROM date)`：获取指定时间的时

`MINUTE(date)`、`EXTRACT(MINUTE FROM date)`：获取指定时间的分

`SECOND(date)`、`EXTRACT(SECOND FROM date)`：获取指定时间的秒

`MONTHNAME(date)`：指定时间中的月份名称

`DAYNAME(date)`：指定时间中的星期名称

`WEEKDAY(date)`、`DAYOFWEEK(date)`：指定时间中的周几（周一从0开始）、（周日从1开始）

`QUARTER(date)`、`EXTRACT(QUARTER FROM date)`：指定时间中的第几个季度（对应1~4）

`WEEK(date)`、`WEEKOFYEAR(date)`、`EXTRACT(WEEK FROM date)`：是一年中的第几周

`EXTRACT(MONTH FROM date)`：是一年中的第几个月

`DAYOFYEAR(date)`：是一年中的第几天

`DAYOFMONTH(date)`：所在的月份是该月的第几天







##### 流程控制

> 查询时自带循环

`IF(res,vlaue1,value2)`：若res的结果为true，则返回value1，否则返回value2

`IFNULL(value1,value2)`：若value1不为NULL，则返回value1，否则返回value2

> 使用举例：
>
> ```mysql
> SELECT last_name,commission_pct,IF(commission_pct IS NULL,0,commission_pct) FROM employees;
> # 若员工的奖金为NULL，则输出0
> 
> SELECT last_name,commission_pct,IFNULL(commission_pct,0) FROM employees;
> # 两者等价
> ```



`CASE WHEN 条件1 THEN 结果1 WHER 条件2 THEN 结果2 ... ELSE 结果 END`：相当于if-else

> 使用举例：
>
> ```mysql
> SELECT last_name,salary,CASE WHEN salary>10000 THEN "Good" 
> 							WHEN salary>9000 THEN "Ok" 
> 							WHEN salary>8000 THEN "Well" 
> 							ELSE "No" END 
> FROM employees;
> # ELSE可以省略
> ```



`CASE 条件 WHEN 值1 THEN 结果1 WHEN 值2 THEN 结果2 ... ELSE 结果 END`：相当于switch-case

> 使用举例：
>
> ```mysql
> SELECT last_name,salary,CASE department_id WHEN 10 THEN salary*1.1
> 										WHEN 20 THEN salary*1.2
> 										WHEN 30 THEN salary*1.3
> 										ELSE salary*1.4 END "add_salary"
> FROM employees;
> # ELSE可以省略
> ```







##### 其他函数

###### 加密

`MD5(s)`：将内容进行md5加密（不可逆）

`SHA(s)`：将内容进行sha加密（不可逆）





###### 系统相关

`VERSION()`：返回当前MySQL的版本号

`CONNECTION_ID()`：返回当前MySQL服务器的连接数

`DATABASE()`、`SCHEMA()`：返回当前所在的数据库

`USER()`、`CURRENT_USER()`、`SYSTEM_USER()`、`SESSION_USER()`：返回当前连接MySQL的用户名

`CHARSET(s)`：返回内容所使用的字符集

`COLLATION(s)`：返回内容所使用的比较规则

`CONVERT(s USING char_code)`：将s使用的字符编码集修改为char_code

`BENCHMARK(n,expr)`：将一条执行语句重复执行n次（用于计算效率）



`INET_ATON(ip)`：将ip地址进行转化（计算）后返回一个数字

`INET_NTOA(num)`：将由ip地址转化的数字重新还原为ip地址格式









#### 聚合函数

> 又称多行函数、分组函数。接收多个参数，返回一个结果
>
> 聚合函数不能嵌套使用

1）`AVG(字段)`：求平均值，只能适用于数值类型的字段（忽略值为NULL的）

2）`SUM(字段)`：求总和，只能适用于数值类型的字段（忽略值为NULL的）

```mysql
SELECT AVG(salary),SUM(salary) FROM employees;
# 查看员工们的平均工资、总工资

SELECT AVG(commission_pct), SUM(commission_pct)/COUNT(commission_pct), SUM(commission_pct)/107 FROM employees;
# 由COUNT()不包含NULL，可得出SUM()、AVG()也不包含NULL
```



3）`MAX(字段)`：求最大值，适用于数值类型、字符串类型、日期时间类型的字段

4）`MIN(字段)`：求最小值，适用于数值类型、字符串类型、日期时间类型的字段

```mysql
SELECT MAX(salary),MIN(last_name) FROM employees;
# 查看员工中的最大工资、名字最小（ASCII最小）的员工
```



5）`COUNT(字段)`：计算指定的字段在查询结构中出现的个数（忽略值为NULL的）

```mysql
SELECT COUNT(*),COUNT(1),COUNT(salary) FROM employees;
# 求员工表数据的条数

SELECT COUNT(commission_pct) FROM employees;
# 求员工表中commission_pct值不为NULL的
```



总结：

1. `AVG = SUM / COUNT`  恒成立（都不会考虑NULL值）









### 数据类型

#### 字符集相关

>MySQL5.7版本默认使用的字符集是latin（拉丁，欧洲字符集），因此不支持中文
>
>MySQL8.0版本默认使用的字符集改为了utf8mb4，支持中文

1. 在创建表时，可以指定表中字段所用的字符集：

   ```mysql
   CREATE TABLE 表名(
   字段名 数据类型 CHARACTER SET '字符集'
   );
   ```

2. 在创建表时，可以指定表所使用的字符集：

   ```mysql
   CREATE TABLE 表名(
   字段名 数据类型
   ) CHARACTER SET '字符集';
   ```

3. 在创建数据库时，可以指定数据库所使用的字符集：

   ```mysql
   CREATE DATABASE 数据库名 CHARACTER SET '字符集';
   ```

4. 在配置文件my.ini中，可以设置数据库默认使用的字符集

   ① 在 [mysql] 下添加语句：`default-character-set=utf8`（设置默认字符集）

   ② 在 [mysqld] 下添加语句：

   ```shell
   character-set-server=utf8
   collation-server=utf8_general_ci
   ```

   ③ 重启mysql服务



> 字段的字符集默认为表的字符集，表的字符集默认为数据库的字符集，数据库的字符集默认为配置文件的字符集。查看配置文件中设置的字符集的办法：
>
> 1. `show variables like 'character_%'`：查看默认使用的字符集
> 2. `show variables like 'collation_%'`：查看比较规则使用的字符集







#### 类型取值

1. UNSIGNED：无符号类型
2. ZEROFILL：当位数不足时，填充0

> 使用举例：
>
> ```mysql
> CREATE TABLE test(
> f1 TINYINT UNSIGNED,		# 无符号型，范围为0~255
> f2 INT(5) ZEROFILL			# 当存储的数值的宽度不足5位时，使用0填充（且为UNSIGNED类型）
> );
> ```







#### 数值类

##### 整型

1. TINYINT：1字节
2. SMALLINT：2字节
3. MEDIUMINT：3字节
4. INT、INTEGER：4字节
5. BIGINT：8字节





##### 浮点型

1. FLOAT：单精度浮点数，4字节
2. DOUBLE：双精度浮点数，8字节
3. REAL：默认是DOUBLE，通过修改sql_mode字段可更改为FLOAT
4. DECIMAL(M,D)：底层使用字符串进行存储的，用来高精度表示小数，M+2字节

注意：

- 浮点数类型的无符号数取值范围，仅为 有符号数取值范围的一半（有符号数>=0的部分）

- MySQL中 DOUBLE 和 FLOAT 可以使用非标准语法，即：`(M,D)`

  > M=整数位数+小数位数，D=小数位数
  >
  > 若D位超出范围时，进行四舍五入；若M位超出范围时，则会报错

- DECIMAL默认情况下为(10,0)

> DICIMAL高精度体现：
>
> ```mysql
> CREATE TABLE test(			# 创建表
> f1 DOUBLE,
> f2 DECIMAL(5,2)
> );
> 
> INSERT INTO test(f1,f2) 	# 插入表数据
> VALUES(0.10,0.10),(0.35,0.35),(0.45,0.45);
> 
> SELECT SUM(f1)=0.9,SUM(f2)=0.9 FROM test;
> # 返回结果：0,1。FLOAT/DOUBLE类型存在精度损失，而DECIMAL类型不会
> ```





##### 位类型

1. BIT(M)：存储的是二进制值，约为 (M+7)/8 个字节

注意：

- 若没有指定M，则为1位。M代表二进制位数（范围：1<=M<=64）









#### 日期时间类

|   类型    |   名称   | 字节 |        格式         |         最小值          |         最大值          |
| :-------: | :------: | :--: | :-----------------: | :---------------------: | :---------------------: |
|   YEAR    |    年    |  1   |        YYYY         |          1901           |          2155           |
|   TIME    |   时间   |  3   |      HH:MM:SS       |       -838:59:59        |        838:59:59        |
|   DATE    |   日期   |  3   |     YYYY-MM-DD      |       1000-01-01        |       9999-12-03        |
| DATETIME  | 日期时间 |  8   | YYYY-MM-DD HH:MM:SS |   1000-01-01 00:00:00   |   9999-12-31 23:59:59   |
| TIMESTAMP | 日期时间 |  4   | YYYY-MM-DD HH:MM:SS | 1970-01-01 00:00:00 UTC | 2038-01-19 03:14:07 UTC |

1）YEAR

1. 用4位字符串或数字表示YEAR类型，其格式为YYYY（默认）

   用2位字符串或数字表示YEAR类型，其格式为YY，最小为00，最大为99

   - 当为01~69时，表示2001~2069年
   - 当为70~99时，表示1970~1999年
   - 当为整数0或00时，表示0000年
   - 当为字符串'0'时，表示2000年

2）DATE

1. 仅表示日期，无时间部分。YYYY表示年，MM表示月，DD表示日
2. 当使用2位的YY时，效果与YEAR相同

3）TIME

1. 仅表示时间，无日期部分。HH表示小时，MM表示分钟，SS表示秒数

2. `D HH:MM:SS`中，D表示天数，最小值为0，最大值为34

   `HH:MM`表示小时和分钟，`D HH:MM`表示天数、小时和分钟，`D HH`表示天数和小时，`SS`表示秒数

4）DATETIME

1. YYYY表示年，MM表示月，DD表示日，HH表示时，MM表示分，SS表示秒
2. 当使用2位的YY时，效果与YEAR相同

5）TIMESTAMP

1. UTC表示世界统一时间（世界标准时间）
2. 底层使用毫秒值进行存储（距离1970-1-1 0:0:0 0ms 间的毫秒值）
3. 会根据时区的不同，显示不同的结果









#### 文本字符串类

|    类型    |     长度范围     |   占用空间    |
| :--------: | :--------------: | :-----------: |
|  CHAR(M)   |    0<=M<=255     |     M字节     |
| VARCHAR(M) |   0<=M<=65535    |    M+1字节    |
|  TINYTEXT  |    0<=L<=255     |    L+2字节    |
|    TEXT    |   0<=L<=65535    |    L+2字节    |
| MEDIUMTEXT |  0<=L<=16777215  |    L+3字节    |
|  LONGTEXT  | 0<=L<=4294967295 |    L+4字节    |
|    ENUM    |   1<=L<=65535    |    1/2字节    |
|    SET     |     0<=L<=64     | 1/2/3/4/8字节 |

1）CHAR与VARCHAR：

1. CHAR(M)：固定长度。若不指定M时，默认为1。
   - 效率较高
   - M表示的是存储的字符的个数
   - 若实际存储的长度小于指定的长度，则会在右侧填充空格；当检索CHAR类型数据时，会自动去除尾部空格（包括自己添加的空格）
2. VARCHAR(M)：可变长度。必须指定长度M，否则报错
   - 效率较低
   - 在MySQL4.0及以下版本，M表示的是存储的字节数；在MySQL4.0版本以上，M表示的是存储的字符个数
   - 多出来的一个字节用于记录存储的数据的实际长度
   - 实际能存储的字符个数为：65535/3

2）TEXT：

- 用来保存文本类型的字符串
- 按照实际数据的长度进行存储，不需要预先定义长度（与VARCHAR类似）

3）ENUM与SET：

1. ENUM：也叫枚举类，存储空间由定义的成员个数所决定。取的值需要在定义字段时指定，且只能取单个值

   - 取值时，忽略大小写
   - 可以有索引来取枚举元素的值

2. SET：表示一个字符串对象，可以包含0或多个成员，但成员个数上限为64，存储空间由定义的成员个数所决定。取的值需要在定义字段时指定，可以取0或多个值

   - 可以插入重复的成员，但相同的成员会被删除

   > 使用举例：
   >
   > ```mysql
   > CREATE TABLE test(				# 创建表
   > f1 ENUM('A','B','C','D'),
   > f2 SET('AA','BB','CC','DD')
   > );
   > 
   > INSERT INTO test(f1) VALUES('A'),(2),('d');
   > # 从定义的成员中选取，可以由索引来添加值，忽略大小写
   > 
   > INSERT INTO test(f2) VALUES('AA,BB'),('CC,DD,CC');
   > # 从定义的成员中选取，可以插入重复的枚举元素的值，但相同的值会被删除
   > ```







#### 其他类型

##### 二进制字符串类

|     类型     |        长度范围         | 占用空间 |
| :----------: | :---------------------: | :------: |
|  BINARY(M)   |        0<=M<=255        |  M字节   |
| VARBINARY(M) |       0<=M<=65535       | M+1字节  |
|   TINYBLOB   |        0<=L<=255        | L+1字节  |
|     BLOB     |   0<=L<=65535（64KB）   | L+2字节  |
|  MEDIUMBLOB  | 0<=L<=16777215（16MB）  | L+3字节  |
|   LONGBLOB   | 0<=L<=4294967295（4GB） | L+4字节  |

1）BINARY与VARBINARY：

1. BINARY(M)：固定长度的二进制字符串
   - M表示存储的字节数，若未指定，则存储1个字节。若存储的字节数不足M时，则在右边填充'\0'
2. VARBINARY(M)：可变长度的二进制字符串
   - M必须被指定。额外的1个字节用来记录实际存储的数据的字节数

2）BLOB：

- 用来存储可变数量的二进制数据





##### JSON类

- 是一种轻量级的数据交换格式，可以将JavaScript对象中表示的一组数据转换为字符串，通过网络等传输该字符串，并在需要时再还原为指定的数据格式

> 使用举例：
>
> ```mysql
> CREATE TABLE test(			# 创建表
> f json
> );
> 
> INSERT INTO test 			# 往表中插入json格式的数据
> VALUES('{"name":"Tom","age":18,"address":{"prov":"sh","city":"sh"}}');
> 
> SELECT f->'$.name',f->'$.age',f->'$.address.prov',f->'$.address.city' FROM test;
> # 获取表中的json格式的数据
> ```









### 约束

为了确保数据的完整性，则需要对表中的数据进行额外的条件限制。

>数据的完整性：
>
>1. 实体完整性（如：一个表中，不能存在两条完全一样的数据）
>2. 域完整性（如：性别只有男/女）
>3. 引用完整性（如：员工所在的部门，需要在部门表中找到该部门）
>4. 用户自定义完整性（如：用户名唯一、密码有复杂度要求）

1）约束的分类：

1. 按约束的字段个数，可分为：
   - 单列约束：对单个字段进行约束
   - 多列约束：对多个字段进行约束
2. 按约束的作用范围，可分为：
   - 列级约束：将约束声明在对应字段的后面
   - 表级约束：在所有的字段声明完后，声明约束
3. 按约束的功能进行分类

2）查看约束的方式：

`SELECT * FROM information_schema.table_constraints WHERE table_name = '表名';`









#### 非空约束

1）创建约束：

`CREATE TABLE 表名(字段名 数据类型 NOT NULL)`：在创建表时限定该字段不能为空



2）修改约束：

`ALTER TABLE 表名 MODIFY 字段名 数据类型 NOT NULL`：修改指定表中的字段不能为空

- 修改时，若要修改的字段中，已存在值为NULL的，则会修改失败



3）删除约束：

`ALTER TABLE 表名 MODIFY 字段名 数据类型 NULL`：取消指定表中的字段的限定非空



使用举例：

```mysql
CREATE TABLE test(
id INT,
name VARCHAR(10) NOT NULL		# 限定该字段不能为NULL
);

ALTER TABLE test MODIFY id INT NOT NULL;	# 指定该字段不能为空
ALTER TABLE test MODIFY id INT NULL;		# 取消该字段的非空约束
```



注意：

- 只能声明在字段的后面（列级约束），且只能是单列约束
- 一个表中可以有多个列限定非空
- `''`字符串、0，都不等于NULL







#### 唯一约束

1）创建约束：

`CREATE TABLE 表名(字段名 数据类型 UNIQUE)`：使用列级约束，在创建表时限定该字段唯一

`CREATE TABLE 表名(字段名 数据类型,CONSTRAINT 约束名 UNIQUE(字段名))`：使用表级约束，在创建表时指定该字段唯一

`CREATE TABLE 表名(字段1 数据类型,字段2 数据类型,UNIQUE(字段1,字段2))`：只能使用表级约束，创建多列的唯一约束（在多条数据中，两个字段中只要有一个的值不同，则认为不同）



2）修改约束：

`ALTER TABLE 表名 ADD CONSTRAINT 约束名 UNIQUE(字段名)`：修改表中的字段唯一（类似于表级约束）

`ALTER TABLE 表名 MODIFY 字段名 数据类型 UNIQUE`：修改表中的字段为唯一（类似于列级约束）



3）删除约束：

`ALTER TABLE 表名 DROP INDEX 约束名`：删除唯一约束（即：删除唯一索引）



使用举例：

```mysql
CREATE TABLE test1(			# 使用列级约束
id INT UNIQUE,
name VARCHAR(10),
email VARCHAR(20) UNIQUE
);

CREATE TABLE test2(			# 使用表级约束
id INT,
name VARCHAR(10),
email VARCHAR(20),
UNIQUE(id)
);

CREATE TABLE test3(			# 使用多列约束
id INT,
name VARCHAR(10),
email VARCHAR(20),
CONSTRAINT un_id_email UNIQUE(id,email)
);

ALTER TABLE test2 ADD CONSTRAINT un_eamil UNIQUE(email);		# 添加约束（表级）
ALTER TABLE test2 MODIFY name VARCHAR(20) UNIQUE;				# 添加约束（列级）

ALTER TABLE test2 DROP INDEX name;			# 删除约束（由约束名定位到唯一索引，进行删除）
```



注意：

- 可以是列级约束或表级约束，也可以是单列约束或多列约束
- 一个表中可以有多个唯一约束
- 唯一约束的列的值可以为空，且可以重复为NULL
- 只能在表级约束中指定唯一约束名。若在创建唯一约束时未指定约束名，则默认和列名相同（多列约束的话则和第一个列名相同）
- 在添加唯一约束时，会在该列上自动创建唯一索引，而唯一索引名与唯一约束名相同







#### 主键约束

1）创建约束：

`CREATE TABLE 表名(字段名 数据类型 PRIMARY KEY)`：用列级的方式创建主键约束

`CREATE TABLE 表名(字段名 数据类型,PRIMARY KEY(字段名))`：用表级的方式创建主键约束

`CREATE TABLE 表名(字段1 数据类型,字段2 数据类型,PRIMARY KEY(字段1,字段2))`：创建复合型主键约束



2）修改约束：

`ALTER TABLE 表名 ADD PRIMARY KEY(字段名)`：添加主键约束（类似于表级约束）

`ALTER TABLE 表名 MODIFY 字段名 数据类型 PRIMARY KEY`：添加主键约束（类似于列级约束）



3）删除约束：

`ALTER TABLE 表名 DROP PRIMARY KEY`：删除主键约束（依旧非空）



使用举例：

```mysql
CREATE TABLE test1(			# 使用列级的方式创建主键约束
id INT PRIMARY KEY,
name VARCHAR(10)
);

CREATE TABLE test2(			# 使用表级的方式创建主键约束
id INT,
name VARCHAR(10),
PRIMARY KEY(id)
);

CREATE TABLE test3(			# 创建复合型（多列）主键约束
id INT,
name VARCHAR(10),
PRIMARY KEY(id,name)
);

ALTER TABLE test1 DROP PRIMARY KEY;			# 删除主键约束（主键名永远是PRIMARY KEY）
ALTER TABLE test1 ADD PRIMARY KEY(id);		# 添加主键约束（表级）
ALTER TABLE test1 MODIFY id INT PRIMARY KEY;	# 添加主键约束（列级）
```



注意：

1. 可以是列级约束或表级约束，也可以是单列约束或多列约束
2. 一个表中只能有一个主键约束
3. 若是多列约束，则还必须要求复合的所有的列不能为空
4. 主键名总是PRIMARY
5. 在创建主键约束时，会建立对应的主键索引，若删除主键约束，则对应的索引也会被删除（破坏数据结构的完整性）







#### 自增长约束

1）创建约束：

`CREATE TABLE 表名(字段名 数据类型 主键/唯一约束 AUTO_INCREMENT)`：为 被主键或唯一约束的字段 添加自增长约束



2）修改约束：

`ALTER TABLE 表名 MODIFY 字段名 数据类型 AUTO_INCREMENT`：添加自增长约束

- 必须具有主键或唯一约束



3）删除约束：

`ALTER TABLE 表名 MOFIFY 字段名 数据类型`：删除自增长约束



使用举例：

```mysql
CREATE TABLE test1(						# 列级的方式添加自增长约束
id INT PRIMARY KEY AUTO_INCREMENT
);

ALTER TABLE test1 MODIFY id INT;		# 删除自增长约束
ALTER TABLE test1 MODIFY id INT AUTO_INCREMENT;			# 添加表级约束
```



注意：

1. 只能声明在字段的后面（列级约束），且自增长约束的列必须有主键约束或唯一约束
2. 一个表中只能有一个自增长约束
3. 自增长约束的列的数据类型必须是整数类型
4. 若自增长列指定了0或NULL，则会在当前的最大值上进行自增。若手动指定了具体值，则直接赋值为具体值（可为负数）。否则都会在当前的最大值上进行自增
5. MySQL8.0中，将自增的计数器持久化到了重做日志中（不再单单存储在内存层面），从而在重启服务等情况下，下一次的自增依旧会按保存到重做日志中的值进行自增







#### 外键约束

1）创建约束：

`CREATE TABLE 表名(字段名 数据类型,FOREIGN KEY(字段名) REFERENCES 主表名(关联字段名))`：与指定表的字段进行关联，创建外键约束

- 关联的表的字段必须是主键或唯一约束



2）修改约束：
`ALTER TABLE 表名 ADD CONSTRAINT 约束名 FOREIGN KEY(字段名) REFERENCES 主表名(关联字段名)`：为指定的字段添加外键约束

- 关联的表的字段必须是主键或唯一约束



3）删除约束：

`ALTER TABLE 表名 DROP FOREIGN KEY 约束名`：删除外键约束

`ALTER TABLE 表名 DROP INDEX 约束名`：删除外键约束所对应的索引

- `SHOW INDEX FROM 表名`：查看表中的索引



使用举例：

```mysql
CREATE TABLE test1(				# 主表
id INT PRIMARY KEY
);

CREATE TABLE test2(				# 从表
tid INT,
CONSTRAINT fk_id FOREIGN KEY(tid) REFERENCES test1(id) ON UPDATE CASCADE ON DELETE RESTRICT				# 添加外键约束
);

ALTER TABLE test2 DROP FOREIGN KEY fk_id;		# 删除外键约束
ALTER TABLE test2 DROP INDEX fk_id;				# 删除外键约束对应的索引

ALTER TABLE test2 ADD CONSTRAINT fk_n_id FOREIGN KEY(tid) REFERENCES test1(id);
# 添加外键约束
```



注意：

1. 外键关联的表叫主表（父表），声明外键的表称为副表（子表、从表）

2. 只能为表级约束，且要关联的主表的字段必须被主键或唯一约束

3. 若在创建外键约束时没有为外键约束命名，则会自动产生一个外键名（非列名）

4. 要创建表指定外键约束时，必须先创建主表，在创建从表；删除外键约束的数据时，必须先删除从表的数据（或删除外键约束），再删除主表的数据

5. 一张表中可以有多个外键约束

6. 主表与从表关联的列，名字可以不同，但数据类型必须相同，使用的存储引擎必须相同

7. 创建外键约束时，会自动创建对应的索引。删除外键约束时，需要将索引手动删除

8. 约束等级：

   - Cascade：父表进行update/delete时，子表同步进行update/delete
   - Set null：父表进行update/delete时，子表数据设置为NULL（子表外键字段不能为非空约束）
   - No action、Restrict（默认）：若子表中有匹配的数据，则父表不能进行update/delete操作
   - Set default：父表有变更时，子表数据设置为默认值

   推荐：`ON UPDATE CASCADE ON DELETE RESTRICT`







#### 检查约束

1）创建约束：

`CREATE TABLE 表名(字段名 数据类型 CHECK(条件))`：使用列级的方式，在添加数据时，检查该字段添加的数据值是否满足条件



使用举例：

```mysql
CREATE TABLE test1(				# 列级方式添加检查约束
id INT CHECK(id>10)
);
```



注意：MySQL5.7中不支持检查约束







#### 默认值约束

1）创建约束：

`CREATE TABLE 表名(字段名 数据类型 DEFAULT 默认值)`：给指定的字段添加默认值约束，当没有为该字段赋值时，则该值在添加时为默认值



2）修改约束：
`ALTER TABLE 表名 MODIFY 字段名 数据类型 DEFAULT 默认值`：给指定的字段添加默认值约束



3）删除约束：

`ALTER TABLE 表名 MODIFY 字段名 数据类型`：删除已有的默认值约束



使用举例：

```mysql
CREATE TABLE test(
id INT DEFAULT 1,			# 创建默认值约束
name VARCHAR(10)
);

ALTER TABLE test MODIFY name VARCHAR(10) DEFAULT 'Unknowed';		# 添加默认值约束

ALTER TABLE test MODIFY name VARCHAR(10);			# 删除默认值约束
```



注意：只能使用列级约束









### 视图

> 举例：想对表中的某些字段进行隐藏，不让其他人看，考虑将可以公开的数据放在一张新表里，但属于两张表，更改内容互不影响，因此使用视图

1. 视图中SELECT语句所涉及到的表，称为基表

2. 对视图进行DML操作，会影响对应的基表中的数据，反之亦然

   > 1. 操作视图的DML语句与操作表的DML语句一样
   > 2. 进行DML操作时有可能失败，比如：对基本不存在的字段进行修改；约束限制等

3. 视图本质上是存储起来的SELECT语句，因此对视图的删除并不会影响到基本中的数据

4. 作用：简化查询、控制数据访问权限





#### 创建

`CREATE VIEW 视图名 AS SELECT 字段名 FROM 表名`：基于查询到的数据创建对应的视图

- `CREATE VIEW 视图名(列名) AS SELECT 字段名 FROM 表名`：为视图中的字段起别名

  > 1. 也可以在SELECT后修改字段的别名，同样能应用到视图
  > 2. 必须与查询后的字段一一对应

- 视图中的字段可以是基表中没有的字段

- 可以在视图的基础上再创建视图

> 使用举例：
>
> ```mysql
> CREATE VIEW vu_emp(dept_id,avg_sal)
> AS
> SELECT department_id,AVG(salary) 
> FROM employees
> GROUP BY department_id;
> ```







#### 查看

`SHOW TABLES`：查看数据库中的表、视图

`DESC 视图名`：查看视图结构



`SHOW TABLE STATUS LIKE '视图名'`：查看视图信息（存储引擎、创建时间等）

`SHOW CREATE VIEW 视图名`：查看视图的创建信息







#### 修改

1. `CREATE OR REPLACE VIEW 视图名 AS SELECT 字段名 FROM 表名`：修改已存在的视图
2. `ALTER VIEW 视图名 AS SELECT 字段名 FROM 表名`：修改已有的视图





#### 删除

`DROP VIEW 视图名`：删除视图

> 删除视图后，可能导致基于该视图创建的视图无法正常使用









### 存储过程与存储函数

#### 创建与调用

##### 存储过程

1）创建格式：

```mysql
DELIMITER $
# SQL语句默认以;代表一条语句的结束。用DELIMITER可以修改为 以指定的符号结束SQL语句
# 通常使用符号：$、//

CREATE PROCEDURE 存储过程名(参数)
BEGIN
	存储过程体;
END $

DELIMITER ;
```



2）调用格式：

```mysql
CALL 存储过程名(参数)
```



按参数类型分类：

1. 没有参数（无参数无返回值）

   > 使用举例：
   >
   > ```mysql
   > DELIMITER //
   > CREATE PROCEDURE avg_sal_emp()
   > BEGIN
   > 	SELECT AVG(salary) FROM employees;
   > END //
   > DELIMITER ;
   > # 创建存储过程，用于返回员工的平均工资
   > 
   > CALL avg_sal_emp();
   > # 调用存储过程
   > ```

2. 仅带IN类型（有参数无返回值）：

   > 使用举例：
   >
   > ```mysql
   > DELIMITER //
   > CREATE PROCEDURE show_sal_emp(IN emp_name VARCHAR(20))	# 要确保传入的值类型匹配
   > BEGIN
   > 	SELECT salary
   > 	FROM employees
   > 	WHERE last_name = emp_name;			# 查询指定的员工
   > END //
   > DELIMITER ;
   > # 创建存储过程，用于查看指定的员工的工资
   > 
   > CALL show_sal_emp('Tom');
   > # 调用存储过程，将参数传递给存储过程
   > ```

3. 仅带OUT类型（无参数有返回值）

   > 使用举例：
   >
   > ```mysql
   > DELIMITER $
   > CREATE PROCEDURE show_min_sal(OUT min_sal DOUBLE)	# 要确保传入的值类型匹配
   > BEGIN 
   > 	SELECT MIN(salary) INTO min_sal		# 将查询到的结果存储在变量中
   > 	FROM employees;
   > END $
   > DELIMITER ;
   > # 创建存储过程，用于查看员工表中最低工资的员工
   > 
   > CALL show_min_sal(@min_salary);
   > # 调用存储过程，将存储在变量中的结果读取出来
   > ```

4. 既带IN又带OUT（有参数有返回值）

   > 使用举例：
   >
   > ```mysql
   > DELIMITER $
   > CREATE PROCEDURE show_emp_sal(IN emp_name VARCHAR(20),OUT emp_sal DOUBLE)
   > BEGIN
   > 	SELECT salary INTO emp_sal
   > 	FROM employees
   > 	WHERE last_name = emp_name;
   > END $
   > DELIMITER ;
   > # 创建存储过程，用于查看指定员工的工资
   > 
   > CALL show_emp_sal('Tom',@emp_sal);
   > # 调用存储过程，将参数传递给存储过程，并将存储在变量中的结构读取出来
   > ```

5. 带INOUT（有参数有返回值）

   > 使用举例：
   >
   > ```mysql
   > DELIMITER //
   > CREATE PROCEDURE show_mgr_name(INOUT emp_name VARCHAR(25))
   > BEGIN
   > 	SELECT last_name INTO emp_name
   > 	FROM employees
   > 	WHERE employee_id = (
   >     					SELECT manager_id
   >         				FROM employees
   >         				WHERE last_name = emp_name
   >     					);
   > END //
   > DELIMITER ;
   > # 创建存储过程，用于获取指定员工的管理者的姓名
   > 
   > SET @emp_name = 'Tom';		# 定义一个变量，并赋值
   > CALL show_mgr_name(@emp_name);
   > # 调用存储过程，传递参数的同时，接收在存储过程内存储了的数值
   > ```



注意：IN、OUT、INOUT是可以在一个存储过程中拥有多个的







##### 存储函数

1）创建格式：

```mysql
DELIMITER // 

SET GLOBAL log_bin_trust_function_creators = 1;		# 跳过创建存储函数时的校验

CREATE FUNCTION 存储函数名(参数名 数据类型)
RETURNS 返回值类型
特性		# 若没有跳过校验，则要设置特性才能创建存储函数。
# 设置如：DETERMINISTIC、CONTAINS SQL、READS SQL DATA等
BEGIN 
	RETURN (函数体);
END //

DELIMITER ;
```



2）调用格式：

```mysql
SELECT 存储函数名(参数);
```



使用举例：

```mysql
SET GLOBAL log_bin_trust_function_creators = 1;			# 跳过校验

DELIMITER //

CREATE FUNCTION email_by_id(emp_id INT)		# 传入参数
RETURNS VARCHAR(25)				# 设置返回值的类型
BEGIN
	RETURN (SELECT email FROM employees WHERE employee_id = emp_id);	# 返回数据
END //

DELIMITER ;
# 创建存储函数，用于求指定员工的邮箱

SELECT email_by_id(101);
# 调用存储函数
```



注意：FUNCTION必须有一个返回值







#### 查看

1. `SHOW CREATE PROCEDURE/FUNCTION 存储过程名/函数名`：查看存储过程或函数的创建信息
2. `SHOW PROCEDURE/FUNCTION STATUS`：查看所有的存储过程或函数的状态信息（可使用模糊查询）
3. `SELECT * FROM information_schema.Routines WHERE ROUTINE_NAME = '存储过程名/函数名'`：查看存储过程或函数的信息





#### 修改

> 存储过程与函数的修改有限，仅能修改特性部分

`ALTER PROCEDURE/FUNCTION 存储过程名/函数名 特性`





#### 删除

`DROP PROCEDURE/FUNCTION 存储过程名/函数名`：删除指定的存储过程或函数









### 变量

#### 系统变量

1. 全局变量：针对当前MySQL实例生效。一旦重启mysql服务，则失效
2. 会话变量：针对当前会话失效。一旦关闭或重新开启一个新的会话，则失效
3. 既是全局变量又是会话变量



##### 查看

1. 查看全局变量：

   ```mysql
   SHOW GLOBAL VARIABLES;
   # 查看所有的全局变量
   
   SHOW GLOBAL VARIABLES LIKE '模糊匹配';
   # 查看满足条件的全局变量
   
   SELECT @@global.变量名;
   # 查看指定的全局系统变量的值
   # 系统变量用 @@ 来表示
   ```

2. 查看会话变量：

   ```mysql
   ① SHOW VARIABLES;
   ② SHOW SESSION VARIABLES;
   # 查看所有的会话变量
   
   SHOW SESSION VARIABLES LIKE '模糊匹配';
   # 查看满足条件的会话变量
   
   SELECT @@session.变量名;
   # 查看指定的会话系统变量的值
   
   SELECT @@变量名;
   # 会先标记为会话系统变量，若会话系统变量不存在，则标记为全局系统变量
   ```







##### 修改

1. 修改MySQL配置文件（my.ini），但需要重启mysql服务

2. 在MySQL运行时使用SET进行修改（重启mysql服务失效）

   - 修改全局变量：

     ```mysql
     ① SET @@global.变量名 = 变量值;
     ② SET GLOBAL 变量名 = 变量值;
     # 修改全局系统变量的值
     
     ```

  SET PERSIST GLOBAL 变量名 = 变量值;
     # MySQL8.0新特性：全局变量的持久化（写入配置文件中，重启mysql服务后依旧有效）
  ```
   
   - 修改会话变量：
   
     ```mysql
     ① SET @@session.变量名 = 变量值;
     ② SET SESSION 变量名 = 变量值;
     # 修改会话系统变量的值
  ```









#### 用户变量

1. 会话用户变量：只对当前的连接会话有效
2. 局部变量：只在 BEGIN ... END 语句块中有效（即：只能在存储过程或存储函数中使用）



##### 会话用户变量

> 使用 @ 来表示为会话用户变量

1. 定义与赋值

   ```mysql
   # ① 在SET中用 = 或 := 赋值
   SET @变量名 = 值;
   SET @变量名 := 值;
   
   # ② 在SELECT语句中用 := 或 INTO 赋值
   SELECT @变量名 := 字段名 FROM 表名;
   SELECT 字段名 INTO @变量名 FROM 表名;
   ```

2. 调用

   ```mysql
   SELECT @变量名;
   ```

   



##### 局部变量

> 必须使用在 BEGIN ... END 中，且使用 DECLARE 声明的局部变量 必须是位于 BEGIN ... END 中的首行

1. 定义

   ```mysql
   DECLARE 变量名 数据类型 DEFAULT 默认值
   # 默认值的设置可有可无
   # 必须声明在 BEGIN END 中的首行
   ```

2. 赋值

   ```mysql
   # ① 在SET中用 = 或 := 赋值
   SET 变量名 = 值;
   SET 变量名 := 值;
   
   # ② 在SELECT语句中用 INTO 赋值
   SELECT 字段名 INTO 变量名 FROM 表名;
   ```

3. 调用

   ```mysql
   SELECT 变量名;
   ```









### 错误处理

`DECLARE 处理方式 HANDLER FOR 错误类型 处理语句`：在SQL执行时，若出现指定类型的错误，则为其执行对应的处理

- 处理方式：

  1. `CONTINUE`：遇到错误不处理，继续执行之后语句
  2. `EXIT`：遇到错误时马上退出
  3. `UNDO`：遇到错误时撤回之前的操作（MySQL暂不支持）

- 错误类型：

  1. `SQLSTATE '字符串错误码'`：匹配长度为5的 字符串类型的sqlstate_value错误代码

  2. `错误代码`：匹配长度为4的 数值类型的MySQL_error_code错误代码

  3. `自定义错误名`：匹配 由用户自定义的某种指定的错误代码

     > 自定义方式：
     >
     > ```mysql
     > # ① 用MySQL_error_code来自定义的错误类型
     > DECLARE 自定义错误名 CONDITION FOR 错误代码;
     > 
     > # ② 用sqlstate_value来自定义的错误类型
     > DECLARE 自定义错误名 CONDITION FOR SQLSTATE '错误代码';
     > ```

  4. `SQLWARNING`：匹配 所有以01开头的sqlstate_value错误代码

  5. `NOT FOUND`：匹配 所有以02开头的sqlstate_value错误代码

  6. `SQLEXCEPTION`：匹配 所有没有被SQLWARNING和NOT FOUND匹配的sqlstate_value错误代码

- 处理语句：

  - 可以是像`SET @变量 = 值`等的简单语句，也可以是由`BEGIN ... END`编写的复合语句









### 流程控制

> 用于存储结构中（存储过程或存储函数）

#### 分支结构

##### IF

```mysql
IF 条件表达式1
	THEN 操作
ELSEIF 条件表达式2
	THEN 操作
ELSE 
	操作
END IF;
```

> 只能在 BEGIN ... END 中使用



使用举例：

```mysql
DELIMITER //

CREATE PROCEDURE update_salary(IN emp_id INT)
BEGIN
	
	DECLARE emp_sal DOUBLE DEFAULT 0;
	DECLARE emp_bonus DOUBLE;
	
	SELECT salary INTO emp_sal
	FROM employees 
	WHERE employee_id = emp_id;
	
	SELECT commission_pct INTO emp_bonus
	FROM employees 
	WHERE employee_id = emp_id;
	
	IF emp_sal < 9000
		THEN UPDATE employees
			 SET salary = 9000
			 WHERE employee_id = emp_id;
	ELSEIF emp_sal < 10000 AND emp_bonus IS NULL
		THEN UPDATE employees
			 SET commission_pct = 0.1
			 WHERE employee_id = emp_id;
	ELSE
		UPDATE employees
		SET salary = salary+100
		WHERE employee_id = emp_id;
	END IF;
	
END //

DELIMITER ;

# 定义存储过程，传入员工id，若该员工工资小于9000，则工资设置为9000；若工资大于等于9000且小于10000，但奖金为NULL，则更新奖金为0.1；其他情况的话则工资涨100
```







##### CASE

1）类似于switch

````mysql
CASE 字段名
WHEN 值1 THEN 操作
WHEN 值2 THEN 操作
...
ELSE 操作
END CASE;
````

2）类似于if

```mysql
CASE
WHEN 条件表达式1 THEN 操作
WHEN 条件表达式2 THEN 操作
...
ELSE 操作
END CASE;
```

> 若是在 BEGIN ... END 中使用，则末尾的CASE一定要加上









#### 循环结构

##### LEAVE与ITERATE

1. LEAVE：除了可以用于循环中，还可以用于 BEGIN ... END 中（相当于break）
2. ITERATE：只能用于循环中（相当于continue）

> 两者都需要借助 标签 来使用



使用举例：

```mysql
DELIMITER //
CREATE PROCEDURE leave_iterate()
BEGIN
	DECLARE num INT DEFAULT 0;
	
	while_laber:WHILE TRUE DO
		SET num := num + 1;
		IF num<10							# 当num<10时，iterate继续循环
			THEN ITERATE while_laber;
		ELSEIF num>15						# 当num>15时，leave结束循环
			THEN LEAVE while_laber;
		ELSE
			SELECT num;
		END IF;
	END WHILE;
	
END //
DELIMITER ;
```







##### LOOP

> 用于死循环中（需要借助于LEAVE来跳出循环）

```mysql
标签名:LOOP
	循环体;
	IF 循环条件 
		THEN LEAVE 标签名;
	END IF;
END LOOP 标签名
```



使用举例：

```mysql
DELIMITER //
CREATE PROCEDURE update_salary_loop(OUT num INT)
BEGIN
	DECLARE avg_sal DOUBLE;			# 记录公司的平均工资
	DECLARE loop_count INT DEFAULT 0;		# 记录当前循环的次数
	
	loop_laber:LOOP
		SELECT AVG(salary) INTO avg_sal FROM employees;
		IF(avg_sal >12000)			# 当平均工资大于12000，退出循环
			THEN LEAVE loop_laber;
		END IF;
		UPDATE employees SET salary = salary*1.1;		# 涨薪
		SET loop_count := loop_count + 1;				# 循环次数+1
	END LOOP;	
	
	SET num := loop_count;		# 将循环次数返回	
END //
DELIMITER ;
# 当公司的平均工资小于12000时，员工工资每次涨薪1.1倍
```







##### WHILE

```mysql
WHILE 循环条件 DO
	循环体;
END WHILE
```



使用举例：

```mysql
DELIMITER //
CREATE PROCEDURE update_salary_while(OUT num INT)
BEGIN
	DECLARE avg_sal DOUBLE;
	DECLARE while_count INT DEFAULT 0;
	SELECT AVG(salary) INTO avg_sal FROM employees;
	
	WHILE avg_sal>6000 DO
		UPDATE employees SET salary = salary * 0.9;
		SET while_count := while_count + 1;
		SELECT AVG(salary) INTO avg_sal FROM employees;
	END WHILE;
	
	SET num := while_count;
END //
DELIMITER ;
# 当公司的平均工资大于6000时，员工工资每次降薪为0.9倍
```







##### REPEAT

> 会先执行一遍循环体，再进行判断

```mysql
REPEAT
	循环体;
UNTIL 循环条件
END REPEAT
```



使用举例：

```mysql
DELIMITER //
CREATE PROCEDURE update_salary_repeat(OUT num INT)
BEGIN
	DECLARE avg_sal DOUBLE;
	DECLARE repeat_count INT DEFAULT 0;
	
	REPEAT 
		UPDATE employees SET salary = salary * 1.1;
		SET repeat_count := repeat_count + 1;
		SELECT AVG(salary) INTO avg_sal FROM employees;
	UNTIL avg_sal>13000
	END REPEAT;
	
	SET num := repeat_count;
END //
DELIMITER ;
# 当公司的平均工资小于13000时，员工工资每次涨薪1.1倍
```









### 游标

> 使用在 BEGIN ... END 中，且必须声明在处理程序之前、变量之后

当想要在返回的结果集当中，精确的只对某一些数据进行操作时，就需要用到游标

1. 声明游标：`DECLARE 游标名 CURSOR FOR SELECT 字段名 FROM 表名`

   > 用游标关联要获取指定数据的结果集

2. 打开游标：`OPEN 游标名`

3. 使用游标：`FETCH 游标名 INTO 变量名`

   > 获取结果集中对应字段的值存入到变量中（结果集中的字段与变量：数量相同，类型匹配）

4. 关闭游标：`CLOSE 游标名`



使用举例：

```mysql
DELIMITER //
CREATE PROCEDURE cursor_salary(IN max_sal DOUBLE,OUT num INT)
BEGIN
	DECLARE cursor_sal DOUBLE;			# 每次游标获取的工资
	DECLARE now_sal DOUBLE DEFAULT 0;	# 当前游标已获取的总工资
	DECLARE emp_count INT DEFAULT 0;	# 当前游标已获取的次数（已计算的员工人数）
	DECLARE cursor_emp CURSOR FOR SELECT salary 
								  FROM employees
								  ORDER BY salary DESC;		# 与游标关联的结果集
	OPEN cursor_emp;
	
	loop_laber:LOOP			# 循环：让游标依次获取工资，直到游标获取的总工资大于max_sal
		FETCH cursor_emp INTO cursor_sal;
		SET now_sal := now_sal + cursor_sal;
		SET emp_count := emp_count + 1;
		IF now_sal > max_sal
			THEN LEAVE loop_laber;
		END IF;
	END LOOP;
	
	CLOSE cursor_emp;
	SET num := emp_count;
END //
DELIMITER ;
# 按员工工资从高到低排序。连续取前n个员工的工资，直到这前n个员工的总工资大于max_sal。返回n的值
```









### 触发器

>若在子表中定义了外键约束，且外键中使用了 ON UPDATE ... ON DELETE ... ，此时若是修改父表中被约束的字段，则也会引起子表中对应字段的修改，但不会激活触发器



#### 创建

```mysql
CREATE TRIGGER 触发器名
【BEFORE、AFTER】 【INSERT、UPDATE、DELETE】 ON 表名
FOR EACH ROW
执行语句;
```

- 在指定的表进行【增、删、改】的操作之【前、后】，先执行指定的内容

- 执行的可以是单条SQL语句，也可以是由 BEGIN ... END 构成的复合语句块

- NEW：获取触发器所绑定的表中的数据。若该数据是表中原先不存在的数据，则使用NEW表示

  >比如：在插入一条数据，并激活触发器对该数据进行备份时，该插入的数据对于原表来说本来是不存在的，因此在备份数据时要通过NEW的方式来获取到原表中本不存在的数据

- OLD：获取触发器所绑定的表中的数据。若该数据是表中原先已存在的数据，则使用OLD表示

  > 比如：在删除一条数据，并激活触发器对该数据进行备份时，该删除的数据对于原表来说已经时存在了的，因此在备份数据时要通过OLD的方式来获取到原表中已存在了的数据



使用举例1：

```mysql
# 当插入数据时，在某张表上进行数据的记录
DELIMITER //

CREATE TRIGGER add_trigger
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
	INSERT INTO employees_log(employee_id,last_name,email,hire_date,job_id)
	VALUES(NEW.employee_id,NEW.last_name,NEW.email,NEW.hire_date,NEW.job_id);
	# 该备份的数据对于原表来讲也本是不存在的数据
END //

DELIMITER ;
```



使用举例2：

```mysql
# 在删除数据时，在某张表上进行数据的备份
CREATE TRIGGER delete_trigger
BEFORE DELETE ON employees
FOR EACH ROW
	INSERT INTO employees_log(employee_id,last_name,email,hire_date,job_id)
	VALUES(OLD.employee_id,OLD.last_name,OLD.email,OLD.hire_date,OLD.job_id);
	# 该备份的数据对应原表来讲，是已经存在的数据
```



使用举例3：

```mysql
DELIMITER //

CREATE TRIGGER salary_check_trigger
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
	DECLARE mar_sal DOUBLE;			# 记录新添加的员工的管理者的工资
	SELECT salary INTO mar_sal 
	FROM employees 
	WHERE employee_id = NEW.manager_id;
	IF NEW.salary > mar_sal			# 若新添加的员工的工资大于其管理者的工资，则报错
		THEN SIGNAL SQLSTATE 'HY000' SET MESSAGE_TEXT = '员工薪资高于领导';
	END IF;	
END //

DELIMITER ;
# 在添加新数据前，激活触发器：判断添加的数据中，员工的薪资是否大于其管理者的薪资，若是，则报错，否则添加成功
```







#### 查看

`SHOW TRIGGERS`：查看当前数据库中所有触发器的信息

`SHOW CREATE TRIGGER 触发器名`：查看当前数据库中指定的触发器的信息

`SELECT * FROM information_schema.TRIGGERS;`：查看所有数据库中所有触发器的详细信息







#### 删除

`DROP TRIGGER 触发器名`：删除指定的触发器









### 其他新特性

#### 窗口函数

> 窗口函数类似于：在查询中对数据进行分组。但分组操作是将结果合并为一条数据，而窗口函数不会

格式：

1. `SELECT 窗口函数 OVER(PARTITION BY 分组字段 ORDER BY 排序字段) FROM 表名`
2. `SELECT 窗口函数 OVER 窗口名 FROM 表名 WINDOW 窗口名 AS (PARTITION BY 分组字段 ORDER BY 排序字段)`（WINDOW窗口就相当于是起别名。要声明在WHERE后面）

|   分类   | 函数 | 说明 |
| :------: | :--: | :--: |
| 序号函数 | ROW_NUMBER() | 顺序排序 |
| 序号函数 |RANK()|并列排序，会跳过重复的符号（如：1、1、3）|
| 序号函数 |DENSE_RANK()|并列排序，不会跳过重复的符号（如：1、1、2）|
| 分布函数 | PERCENT_RANK() | 等级值百分比 |
| 分布函数 | CUME_DIST() | 累积分布值 |
| 前后函数 | LAG(expr,n) | 返回当前行的前n行的expr的值 |
| 前后函数 | LEAD(expr,n) | 返回当前行的后n行的expr的值 |
| 首位函数 | FIRST_VALUE(expr) | 返回第一个expr的值 |
| 首位函数 | LAST_VALUE(expr) | 返回最后一个的expr的值 |
| 其他函数 | NTH_VALUE(expr,n) | 返回第n个expr的值 |
| 其他函数 | NTILE(n) | 将分区中的有序数据分为n个桶，并记录桶编号 |



使用举例：

```mysql
SELECT ROW_NUMBER() OVER(PARTITION BY job_id ORDER BY salary DESC) rn,job_id,salary
FROM employees;
# 按照job_id进行了分组，并按薪资进行了降序排序，最后按顺序显示了排序后的序号
```







#### 公用表表达式

> 可以复用子查询语句

格式：`WITH 公用表名 AS (子查询语句) 【SELECT、DELETE、UPDATE】 语句`



