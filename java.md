## Java基础部分

### 常用DOS命令

```shell
盘符:		# 进入指定盘符下
dir 	# directory，查看当前目录下的所有文件
md 文件夹名		# make directory，创建一个新文件夹
rd 文件夹名		# remove directory，删除指定文件夹（文件夹内需无东西）
cd 文件夹/文件夹名		# 进入 指定文件夹下 的 指定文件夹 中
cd ..		# 退回上一层目录
cd /		# 退回最顶层目录
del 文件名		# 删除指定文件
del *.后缀名	# 删除所有特定后缀名的文件
cls		# 清屏
```





### Java背景知识

#### 历史与版本

- java之父 --> 詹姆斯-高斯林
- JavaSE（桌面端）、JavaEE（企业版）、JavaME（小型端）
- Java8.0、Java11.0、Java17.0 都是长期版本（LTS），会长期更新

#### JDK介绍

- jdk（即开发工具等） = jre + jvm
- jvm是java文件运行时使用的虚拟机，会自动分配和释放内存，但仍会导致内存溢出（太多运行文件占用内存）或内存泄漏（太多无用的运行文件没有被及时清理）问题
- java属于半编译半解释型语言：在运行java文件时，会通过java编译器将其编译为字节码文件，字节码文件通过jvm编译时，有可能是逐行解释，也有可能是通过jit（即时）编译器进行编译

#### 文件格式特点

- 在运行.java文件时，先使用`javac 文件名.java`（javac.exe工具）将java文件中的所有`class`类编译为.class字节码文件，再使用`java 类文件名`（java.exe工具）运行字节码文件
- 类名严格区分大小写，而window对文件不区分大小写

#### Java安装与配置

1. 下载（oracle.com官方 --> 最下方developer --> Downloads --> JDK17.0）
2. 安装（留意jdk安装的路径）
   - jdk17.0在安装时，不会再次安装jre（其内已包含）
3. 配置环境变量（用于快速启用开发工具等，jdk17.0已自动配置）
   - 此电脑 --> 属性 --> 高级系统设置 --> 环境变量 --> 系统变量处新建环境变量（变量名为`JAVA_HOME`，变量值为jdk安装的路径） --> 编辑系统变量中的`Path`变量（新建`%JAVA_HOME%\bin`，并上移到最顶部确保会优先调用）
4. 运行`java -version`查看JDK版本





### 注释

- 注释包括单行（`//`）、多行（`/* */`）、文档（`/** @标签 内容 */`）注释
  
- 注释掉的内容不会存在于字节码文件中
  
- 文档注释的内容可以被javadoc.exe工具解析，生成网页形式的文档说明书

  - `javadoc -d 文件夹名 -说明内容 文件.java`

    ```java
    /** 
    	@author LY
    	@version 1.0
    */
    
    javadoc -d test -author -version a.java
    // -d：存放于指定的文件夹中
    // -author -version：标签，若标签存在，则按此格式表示在文档中
    ```





### 标识符命名规则

> 所有用户能自定义名称的地方，都可以称为标识符

1. 由大小写字母、数字、_ 或 $ 组成

2. 不能以数字开头，不能使用关键字、保留字、空格

   > 不能以数字开头原因举例：
   >
   > ```java
   > int 123L = 21;
   > long a = 123L;
   > // 此时的a的值？
   > ```

3. 严格区分大小写





### 变量

- 变量的格式：`数据类型 变量名 = 变量值`（变量名遵循标识符命名规则）

- java中的变量按数据类型分为：

  1. 基本数据类型：

     - 整型：`byte`（1字节，值范围：`-128~127`）、`short`（2字节）、`int`（4字节，常用）、`long`（8字节）

       > 定义long类型变量，变量值末尾要加上`L`或`l`修饰

     - 浮点型：`float`（4字节，精确到7位有效数字）、`double`（8字节，常用）

       > float表示范围比long还大，但精确度不高
       >
       > 定义float类型变量，变量值末尾要加上`F`或`f`修饰

     - 字符型：`char`（2字节）

       > 4种表现形式：
       >
       > ① 使用`''`包裹，内部有且仅有一个字符（unicode字符）
       >
       > ​	如：`'a'`、`'中'`、`'1'`、`%`、`☾`
       >
       > ② 使用`''`包裹，内部使用unicode值表示字符
       >
       > ​	如：`\u0036` 表示 6
       >
       > ③ 使用`''`包裹，内部代表转义字符
       >
       > ​	如：`'\n'`、`'\t'`
       >
       > ④ 直接使用 ASCII 对应的具体的数值
       >
       > ​	如：`97` 表示 a

     - 布尔型：`boolean`（只有两个值：`true`、`false`）

       > 在分配内存时，true被分配为整型1，false被分配为整型0，因此占用4个字节

  2. 引用数据类型：

     类`class`、数组`array`、接口`interface`

     枚举`enum`、注解`annotation`、记录`record`

- 数据存储的最小单位：bit（位）（每个 0或1 即是1位，8bit = 1byte）

- 数据存储的基本单位：byte（字节）（1024byte = 1kb）

> 变量有特定的作用域，一个作用域内不能申明两个同名的变量；定义变量后，可以通过变量名的方式进行赋值、调用、运算等；变量赋值时，需满足其数据类型的数值范围





### 运算规则

> 进行运算时，是从左往右依次运算

（一）基本数据类型中，可以运算的基本数据类型有7种（不包括boolean类型），运算规则包括：

1. 自动类型提升：当容量（指的是数据的取值范围）小的变量与容量大的变量做运算时，结果将转换为容量大的数据类型。如：

   ```java
   int a = 10;
   long b =20L;
   long res = a + b;
   // long类型的变量值不加L并不会报错，因为此时的变量值被当做int类型（但该数值不能超出int的取值范围）
   // 反之，float类型的变量值不加F会报错，因为不满足自动类型提升规则
   ```

   > byte、short、char 类型间的变量做运算，结果都为 int 类型
   >
   > 整型常量的值为 int类型，浮点型常量的值为 double类型
   >
   > byte、short、char  --->  int  --->  long  --->  float  --->  double

2. 强制类型转换：若将容量大的变量的数据类型转换为容量小的变量的数据类型时，需要使用`()`进行强制类型转换。如：

   ```java
   long a = 10L;
   int b = (int)a;
   ```
   
   > 本质上是对 bit （高位）的截断

（二）基本数据类型与引用数据类型String做运算：

- 只能与基本数据类型（共8种）间做连接运算（使用`+`表示）

  > String类型的变量，使用`""`进行赋值，其内部可以有0或多个字符





### 进制

（一）表示形式：

1. 二进制：0b
2. 八进制：0
3. 十六进制：0x

（二）计算机数据底层以 二进制补码 的形式存储，最高位代表符号位

- 正数：三码合一（原码、反码、补码 相同）

- 负数：

  ① 原码：找正数的原码

  ② 反码：除符号位外，其他 按位取反

  ③ 补码：反码 + 1

  ```shell
  0 0 0 0 1 0 1 1 # 11原码
  1 0 0 0 1 0 1 1	# -11原码
  1 1 1 1 0 1 0 0 # -11反码
  1 1 1 1 0 1 0 1 # -11补码
  ```

（三）进制间的转换：

1. 二进制转十进制：2的次方相加

   ```shell
   0 0 0 0 1 0 1 1
   # 2^0 + 2^1 + 2^3 = 11
   ```

2. 十进制转二进制：除2取余的逆

   ```shell
   11	# 1 0 1 1
   # 11/2 --> 5 ... 1
   # 5/2 --> 2 ... 1
   # 2/2 --> 1 ... 0
   # 1/2 --> 0 ... 1
   ```

3. 二进制转八进制：三位1算（2^3）

   二进制转十六进制：四位1算（2^4）

   ```shell
   11 101 001	# 0351
   # 001 --> 1
   # 101 --> 5
   # 011 --> 3
   ```

4. 八进制转二进制：每位还3

   十六进制转二进制：每位还4

   ```shell
   0x23	# 0010 0011
   # 2 --> 0010
   # 3 --> 0011
   ```





### 运算符

#### 知识点

1. `/`：舍弃小数部分，取整

2. `%`：当存在负数时，结果与 被模数 的符号相同

3. `++`（前）：先自增，后赋值

   ```java
   int a = 10;
   int b = ++a;
   // a --> 11 ; b --> 11
   ```

4. （后）`++`：先赋值，后自增

   ```java
   int a = 10;
   int b = a++;
   // a --> 11 ; b --> 10
   ```

5. `+=`：结果不会改变本身的数据类型

   ```java
   int a = 1;
   a *= 0.1;
   // a = 0，类似于：a=(int)(a*0.1)
   ```
   
6. 比较运算符：运算的结果为boolean类型

7. 逻辑运算符：针对boolean类型变量进行操作，且运算的结果也为boolean类型

   - `&`（与）、`&&`（短路与）：全真为真，否则为假

     > 区别：&&在遇到false时则不再进行后面的判断

   - `|`（或）、`||`（短路或）：一真为真，否则为假

     > 区别：||在遇到true时则不再进行后面的判断

   - `!`（非）：真假互换

   - `^`（亦或）：不同则真，否则为假

     ```java
     a=true	b=true  --> a^b = false
     a=false b=true	--> a^b = true
     ```

8. 位运算符：针对数值类型的变量或常量进行操作，且运算的结果也为数值类型

   - `>>`（左移）：二进制值向左移动1位（相当于原有的数值*2）

   - `<<`（右移）：二进制值向右移动1位（相当于原有的数值/2）

     > 负数移动时高位补1

   - `>>>`（无符号右移）：右移后空位补0（正负数都适用）

9. 条件运算符：`(条件表达式)?表达式1:表达式2`

   > 条件表达式的结果为boolean类型




#### 面试题

   ```java
int c = 2;
c = c++;
// c --> 2
// 先赋值，即 将c此时的结果先临时存放在一块空间中，然后自增，最后将此临时存放的值赋值给c

int d = 3;
d = ++d;
// d --> 4;
// 先自增，然后将此时的结果临时存放在一块空间中，最后再将此临时存放的值赋值给d
   ```

   ```java
int m = 10;
int n = 20;

// 不适用于非数值类型，且有可能超出int的数值范围
m = m + n;	// 30=10+20;
n = m - n;	// 10=30-20;
m = m - n;	// 20=30-10;

// 不适用于非数值类型
m = m ^ n;	// 1010 ^ 10100 --> 11110（30）
n = m ^ n;	// 11110 ^ 10100 --> 1010（10）
m = m ^ n;	// 11110 ^ 1010 --> 10100（20）
   ```

```java
Integer a = 10;
Double b = 10;
System.out.println(a==b);	// 报错
// ==用于比较引用数据类型时，比较的是两者的地址值（只能比较相同类型或为继承关系的）
```

```java
Object o = true ? new Integer(1) : new Double(2.0);
System.out.println(o);	// 1.0
// 三目运算符会自动将两者的类型提升为相同的类型，再进行比较
```







### 控制台输入

> 非 lang包 下需要导包

```java
① import java.util.Scanner;	// 导包

② Scanner scan = new Scanner(System.in);	// 创建Scanner类的实例

③ String a = scan.next();	// 获取输入的字符串
char b = scan.next().charAt(0);	// 获取输入的字符串中的第一个字符
int c = scan.nextInt();	// 获取输入的int型

④ scan.close();	// 关闭资源，防止内存泄漏

// 可获取 byte/short/int/long/float/double/boolean/String 等类型值
```





### Switch-Case

```java
swtich(表达式){
    case 值:
    	break;
    default:
    	break;
}
// swtich中的表达式支持的数据类型：byte、short、char、int、枚举（JDK5.0）、String（JDK7.0）
// case后跟的值，只能与表达式进行相等的判断，不能进行范围的判断
// 使用break跳出switch-case结构，否则会进行 case穿透（继续执行其后的case中的语句）
// default的位置是灵活的（不一定是放在最末尾），但也可能造成 case穿透
```





### 标签

```java
laber:for(int i=0;i<3;i++){
    for(int j=0;j<10;j++){
        if(i%4==0){
            ① break laber;	// 结束指定的循环结构 ==> 123
        	② continue laber;
            // 结束某次循环，开始指定循环结构的下次循环 ==> 123123123
        }
    }
}
// 标签的名字可以任意
```





### 字符集介绍

1）存储在文件中的字符：

1. ascii：用来存储字母、数字、常用标点符号等，共128个，每个字符占用1个字节（byte）
2. iso-8859-1：欧洲使用的字符集，每个字符占用1个字节，兼容ascii
3. gbk：存储中文简繁体、字母、数字、常用标点等，中文使用2个字节存储，字母、数字、常用标点等使用1个字节存储，兼容ascii
4. utf-8：用来存储世界范围内多种语言的字符集，使用1-4个字节存储不同的字符。其中，中文使用3个字节存储，字母、数字、常用标点等使用1个字节存储，兼容ascii



2）存储在内存中的字符：Unicode字符集，一个字符（char）占用2个字节







### 知识点

1. 死循环、break、continue后不能有执行语句（该语句永远不会被执行）

2. 嵌套循环中，外层循环控制 行数 ，内层循环控制 列数

3. main方法中的修饰符public被换成private后，就只是一个普通的方法，不再作为程序运行的入口了

4. main()解析：

   - 可以看成是一个普通的静态方法

     ```java
     class Main{
     	// 静态方法
         public static void main(String[] args){
             for(int i=0; i<args.length; i++){
                 System.out.println(args[i]);
             }
         }
     }
     
     // 程序入口
     public static void main(String[] args){
         Main.main( new String[]{"Tom","Jack","July"} );
     }
     ```

   - 看成是程序的入口，格式是固定的

     > public 表示程序可以被调用，static 表示无需通过对象（也无法通过对象）调用，void 是因为没有返回值（栈中最底层），main 表明是程序入口，形参String args[] 是可以传值的





### 例题

#### 算法优化：质数

```java
public static void main(String[] args){
    int num=0;	// 求10万以内质数个数
    long start=System.currentTimeMillis();	// 程序开始时间
    for(int i=2;i<=100000;i++){
        boolean isPrime=true;	// 假设一开始是质数
        
        // 优化②：不能被 除1和本身的其他数（开方内的数）整除，即为质数
        for(int j=2;j<=Math.sqrt(i);j++){
            if(i%j==0){	//能被除1和本身除尽，说明非质数
                isPrime=false;
                
                // 优化①：跳出循环（仅对非质数有效）
                break;
            }
        }
        if(isPrime){
            num++;
            System.out.println(i);
        }
    }
    long end=System.currentTimeMillis();	// 程序结束时间
    System.out.println("共有"+num+"个质数，花费了"+(end-start)+"ms");
}
```







## IDEA使用

### 常用配置

- View --> Appearance --> Toolbar：打开一些快捷功能

> File --> settings 进行设置

#### Appearance & Behavior

1. 设置主题与菜单字体：Appearance --> Theme（主题）--> Use custom font（菜单字体）
2. 取消自动检查更新：System Settings --> Updates --> Check IDE updates（IDE自动更新）--> Check for plugin（插件自动更新）



#### Editor

1. 设置鼠标滚轮放大字体：General --> Change font size

2. 设置创建文件后的默认字体：Font --> Size

3. 设置文件编码格式：File Encodings --> Global Encoding 、Project Encoding 、Default encoding for（全使用UTF-8） --> Transparent native（对打开的文件进行编码）

4. 设置自动导包：General --> Auto Import --> Add unambiguous 、Optimize imports

5. 设置编写代码时忽略大小写：General --> Code Completion --> Match case（关闭区分大小写）

6. 自定义文件模板：File and Code Templates --> + --> Name（设置模版名）、Extension（设置文件模板默认后缀）、Enable Live Templates（开启光标动态效果，参考HTML模板）

7. 自定义方法模版：Live Templates --> + --> Template Group（添加分组）--> Live Template（添加模版）：以添加 单元测试方法 为例：

   ```java
   // 1、Abbreviation：调用模版的代码
   // 2、Description：关于模板的描述
   // 3、Template text：模板的内容
   
   @Test
   public void test$var1$(){	// 光标第一次闪烁停留的位置，按回车进入下一个
       $end$	// 光标最后一次闪烁停留的位置
   }
   
   // 4、Define：模板的应用范围（选择Java）
   // 5、应用即可
   ```



#### Advanced Settings

1. 取消连续shift打开搜索功能：Search advanced（搜索）--> double --> Disable double modifier



#### Build,Execution,Deployment：

1. Build Tools --> Reload project after changes --> Any changes 导入Maven等时，自动进行jar包依赖导入
2. Build Tools --> Maven --> Maven home path（设置本地Maven路径）--> User settings file（设置使用的Maven配置文件settings.xml的路径）--> Local repository（设置Maven本地仓库路径）
3. Compiler --> Java Compiler --> Project bytecode version、Target bytecode version（设置代码编译时使用的JDK版本）





### 常用快捷键

#### 基础

```shell
多行选中：ctrl + 鼠标
连续选中：shift + 鼠标

连续行选中：shift + Home/End
连续列选择：alt + 鼠标

复制/粘贴：ctrl + c/v
复制当前行：ctrl + d

剪切：ctrl + x
删除当前行：ctrl + y

撤销：ctrl + z
反撤销：ctrl + shift + z

保存：ctrl + s

全选：ctrl + a

选中代码向前移4个空格：tap
选中代码向后退4个空格：tap + shift
```



#### 常用

```shell
方法形参提醒：ctrl + p

智能补全：alt + enter
使用代码（try-catch、if等）包裹：ctrl + alt + t
调用构造器等：alt + insert
重写父类/接口中的方法：ctrl + o/i

查找：ctrl + f
查找与替换：ctrl + r
查找当前类中的方法：ctrl + f12
批量修改指定的变量、方法名等：shift + F6
在类中查看继承关系：ctrl + h

下一行插入：shift + enter
上一行插入：ctrl + alt + enter

向上/下移动（可出方法或类）：alt + shift + PgUp/PgDn
排版代码：ctrl + alt + l

单行注释与取消注释：ctrl + /
```



#### 补充

```shell
显示代码模板：ctrl + j

生成返回值变量：ctrl + alt + v
    
向上/下移动代码（不可出方法或类）：ctrl + shift + PgUp/PgDn

抽取代码构成新方法：ctrl + alt + m

对指定代码进行大小写切换：ctrl + shift + u

批量导包：ctrl + alt + o

查找指定源码：ctrl + n（选择AllPlaces）

后退/前进页面（源码跳转中使用）：ctrl + alt + Home/End

在打开的类文件中切换：alt + Home/End

查看方法说明文档：ctrl + q

查看类的UML关系图：ctrl + alt + u

定位到某行：ctrl + g

回溯变量或方法的来源：ctrl + alt + b

折叠/展开方法：ctrl + shift + +/-

多行注释与取消注释：ctrl + shift + /
```





### DeBug

三种断点格式：

1. 圆：断点普通语句
2. 方：断点方法（配合Resume Program，在调用到该方法时会自动停下）
3. 眼：断点成员变量（配合Resume Program，在调用到该属性时会自动停下）

功能介绍：

1）

1. Show Execution Point：回到当前代码执行的位置
2. Step Over：进入下一行
3. Step Into：进入自己写的方法中
4. Force Step Into：进入源码
5. Step Out：退出方法
6. Run to Cursor：调试的下一步跳转位置

2）

1. Return：重新运行DeBug
2. Resume Program：遇到断点（或一些特殊类型断点）时停止
3. Stop：（若在方法中）运行完并结束DeBug
4. View Breakpoints：停止所有断点的使用

3）

1. 右键方法 --> Force Return：结束当前方法的运行







## 数组

### 知识点

1. 数组中的元素在内存中是依次紧密、有序排列的

2. 数组属于引用数据类型变量，其元素可以是基本数据类型，也可以是引用数据类型

3. 数组在内存中占用的是一块连续的空间，因此，当数组初始化完成、长度确定后，其长度就不可再更改

4. ```java
   int[] a = new int[10];
   byte[] b = new byte[10];
   int[][] c = new int[3][4];
   
   a = b;	// 编译报错
   // int[]、byte[]属于两种不同类型的引用数据类型变量
   // 虽然a和b都属于地址，但其地址类型也不是相同的
   
   a[0] = b[0];
   // a[0]、b[0]这两种元素值，满足自动类型提升条件
   
   c = a;	// 编译报错
   // int[][]、int[]属于两种不同类型的引用数据类型变量
   
   c[0] = a;
   // 属于相同类型的引用数据类型变量（int[]），且其地址的类型也相同
   ```





### 一维数组

#### 定义

1. 静态初始化：数组变量的赋值，与其元素的赋值同时进行

   ```java
   数据类型[] 变量名;
   变量名 = new 数据类型[]{ 元素值1,元素值2 };
   
   // double[] a = new double[]{ 1,2,3 };
   ```

   > 类型推断：数组变量的声明、赋值与元素赋值同时进行
   >
   > ```java
   > 数据类型[] 变量名 = { 元素值1,元素值2 };
   > // int a[] = {1,2,3};
   > ```
   
2. 动态初始化：数组变量的赋值，与其元素的赋值分开进行

   ```java
   数据类型[] 变量名 = new 数据类型[ 数组长度 ];
   变量名[ 下标1 ] = 元素值1;
   变量名[ 下标2 ] = 元素值2;
   
   // String[] b = new String[3];
   ```

3. 获取数组的长度

   ```java
   变量名.length
       
   // int[] a = new int[3];
   // a.length --> 3
   ```

4. 特点：

   各类数组元素的默认初始化值：

   - 整型：0
   - 浮点型：0.0
   - 字符型：0（也为：'\u0000'）
   - 布尔型：false（0即为false）
   - 引用数据类型：null






#### 内存解析

> JVM运行时内存结构：程序计数器、虚拟机栈、本地方法栈、堆、方法区

与数组有关的内存结构：

1. 虚拟机栈：用于存放 在方法中声明的局部变量
2. 堆：用于存放数组的实体（即数组中的所有元素）

```java
public static void main(String[] args){
// 执行main方法，变量args入栈

    int[] a = new int[4];
// 数组变量a入栈，new（创建实例）后在堆中开辟空间A，对外仅暴露其首地址（此时a变量的值即为：在堆中 空间A的首地址）
    
    a[0] = 10;
    a[2] = 20;
// 通过首地址找到在堆中空间A的位置，修改数组a对应下标的值
    
    String[] b = new String[2];
// 变量b入栈，创建实例后在堆中开辟空间B1，此时b变量的值为：在堆中开辟的空间B1的首地址
    
    b[1] = "hello";
// 通过首地址找到在堆中空间B1的位置，修改数组b的元素默认值
    
    b = new String[3];
// 再次创建实例，在堆中又开辟一块空间B2，此时，将空间B2的首地址赋值给了变量b（栈中数组变量b指向的地址：空间B1 --> 空间B2）

// 空间B1已经没有被任何变量指向，会被GC（垃圾回收器）回收
    
    int[] c = a;
// 数组变量c入栈，此时数组变量c的值为：数组变量a的值（即空间A的首地址）
// 数组变量c与数组变量a 指向堆中的同一片空间（空间A）
    
    c[2] = 30;
// 通过首地址在堆中找到空间A的位置，对其（数组）下标为2的元素的值进行修改
    
}
```





### 二维数组

#### 定义

1. 静态初始化

   ```java
   数据类型[][] 变量名 = new 数据类型[][]{ {数组1元素值},{数组2元素值} };
   
   // int[][] a = new int[][]{ {1,2},{3,4,5},{6,7} };
   ```

   > 类型推断：
   >
   > ```java
   > 数据类型[][] 变量名 = { {数组1元素值},{数组2元素值} }
   > // int[][] a = { {1,2,3},{2,4},{2,1} };
   > ```

2. 动态初始化（方式一）：每个内层数组的长度相同

   ```java
   数据类型[][] 变量名 = new 数据类型[ 外层数组长度 ][ 内存数组长度 ];
   变量名[ 外层数组下标 ][ 内层数组下标 ] = 元素值;
   
   // String[][] a = new String[3][4];
   // a[0][1] = "hello";
   ```

   动态初始化（方式二）：每个内层数组的长度可变

   ```java
   数据类型[][] 变量名 = new 数据类型[ 外层数组长度 ][];
   变量名[ 外层数组下标 ] = new 数据类型[ 内层数组长度 ];
   变量名[ 外层数组下标 ][ 内层数组下标 ] = 元素值;
   
   // double[][] a = new double[2][];
   // double[1] = new double[4];
   // double[1][3] = 10.0;
   ```

3. 获取数组的长度

   ```java
   变量名.length	// 获取外层数组的长度
   变量名[ 外层数组下标 ].length	// 获取指定的内层数组的长度
       
   // int[][] a = new int[2][3];
   // a.length --> 2
   // a[0].length --> 3
   ```

4. 特点

   动态初始化下 的默认初始值：

   ① 动态初始化（方式一）

   ```java
   int[][] a = new int[2][3];
   // 外层数组元素初始化值：地址值
   // 内层数组元素初始化值：与一维数组各种数据类型的默认初始化值 相同
   ```

   ② 动态初始化（方式二）

   ```java
   int[][] b = new int[3][];
   // 外层数组元素初始化值：null
   // 内层数组元素初始化值：不存在（强制编译报 空指针异常）
   ```





#### 内存解析

```java
public static void main(String[] args){
// 执行main方法，args变量入栈
    
    String[][] a = new String[3][2];
// 数组变量a入栈，其值为：在堆中开辟的一块长度为3的空间A的首地址
// 这3个长度中，每个长度存放的都是：在堆中开辟的一块长度为2的空间AA的首地址
// 空间AA中的2个元素值，存放的都为String类型，在没初始化前，其值都为null
    
    int[][] b = new int[4][];
// 数组变量b入栈，其值为：在堆中开辟的一块空间B（大小为4）的首地址
    
    b[1] = new int[5];
// 根据数组变量b的值，找到其指向的空间B中的第二个元素值，并对其进行赋值，使其指向：在堆中开辟的一块空间BB（大小为5）
    
    b[1][1] = 1;
// 根据数组变量b，找到其指向的空间B的第二个元素值，再根据空间B第二个元素值中存放的值（地址），找到其指向的空间BB的第二个元素值，将其赋值为1
    
    b[2][2] = 1;	// 报错（空指针异常）
// 根据数组变量b，找到其指向的空间B的第三个元素值，但由于该元素值并没有指向任何空间，因此报错
    
    b = new int[2][];
// 在堆空间中开辟了一块空间C（长度为2），并将其首地址赋值给数组变量b
// 此时数组变量b就从原先的 指向空间B 转化为 指向空间C
// 因此，空间B就没有任何变量指向，将被GC回收，当空间B被回收后，其元素值的指向也将失效，即空间BB也将被回收
    
}
```





### 例题

#### 杨辉三角

```java
public static void main(String[] args) {
// 杨辉三角中间的值需要用其 上和左上 两个值进行计算获得，因此用二维数组进行获取
    int[][] yanghui = new int[10][];
    // 遍历获取杨辉三角的总行数
    for (int i = 0; i < yanghui.length; i++) {
        // 动态初始化确定每一行的列数
        yanghui[i] = new int[i + 1];
        // 杨辉三角每一行的 第一个和最后一个 都为1
        yanghui[i][0] = 1;
        yanghui[i][i] = 1;
        for (int j = 1; j <yanghui[i].length-1 ; j++) {
            yanghui[i][j]=yanghui[i-1][j-1]+yanghui[i-1][j];
        }
    }
    // 输出结果
    for (int i = 0; i < yanghui.length; i++) {
        for (int j = 0; j < yanghui[i].length; j++) {
            System.out.print(yanghui[i][j] + "\t");
        }
        System.out.println();
    }
}
```





#### 反转

```java
public static void main(String[] args) {
    int[] a = new int[]{20, 30, 10, 50, 60, 80};
    // 方式1：按顺序，第一个和最后一个互换
    for (int i = 0; i < a.length / 2; i++) {
        int temp = a[i];
        a[i] = a[a.length - 1 - i];
        a[a.length - 1 - i] = temp;
    }

    // 方式2：向中靠拢，第一个和最后一个互换
    for (int i = 0, j = a.length-1; i < a.length / 2; i++, j--) {
        int temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }

    // 方式3：新建数组，倒序拷贝
    int[] b = new int[a.length];
    for (int i = 0; i < a.length; i++) {
        b[i] = a[a.length - i - 1];
    }
    // 将b的地址赋值给a
    a = b;

    // 输出结果
    for (int i = 0; i < a.length; i++) {
        System.out.print(a[i] + "\t");
    }
}
```





#### 二分查找

```java
public static void main(String[] args) {
    // 前提：数组必须有序
    int[] a = new int[]{2, 4, 7, 21, 33};
    int find = 6;
    int start = 0;  // 起始下标
    int end = a.length - 1; // 末尾下标
    while (start <= end) {
        int million = (end + start) / 2;    // 每次对比的中间值的下标
        // 要找的比中间值小，向前找
        if (a[million] > find) {
            end = million - 1;  
        // 要找的比中间值大，向后找
        } else if (a[million] < find) {
            start = million + 1;   
        } else {
            System.out.println("找到了，下标为" + million);
            break;
        }
    }
    // 当起始下标比结束下标还大，说明遍历完成还未找到，无该数值
    if (start > end) {
        System.out.println("未找到");
    }
}
```





#### 冒泡排序

> 思路剖析：
>
> ① 从前往后，先对1、2两个值进行对比，若1大于2，则互换，否则不动
>
> 接着对2、3进行对比，若2大于3，则互换，否则不动
>
> 依此类推，则到确定出第一个最大值，位于末尾（共排列 length-1 次）
>
> ② 依旧是从前往后，对相邻的两个值进行对比，直到确定出第二大值（共排列 length-2 次）
>
> ③ 因此，其时间复杂度为 O(n^2)

```java
public static void main(String[] args) {
    int[] a = {2, 4, 5, 6, 13, 9, 30, 10, 49, 89, 20, 1};

// ③ 因此可以知道，相邻两个值做对比，确定出每次的最大值，那么还至少应要排序 length-1次，才能对比排序完
    for (int j = 0; j < a.length - 1; j++) {
        
        boolean isSort = true;	// 表示无交换过数值，用于直接退出循环
        
// ① 第一次排列时，确定了最大值（最后一次不用对比排序）,因此对比了 length-1次
// ② 第二次排列，确定第二大值（最后一次和最大值间无需对比），对比了 length-2次
        for (int i = 0; i < a.length - 1 - j; i++) {
            if (a[i] > a[i + 1]) {
                
                int temp = a[i];
                a[i] = a[i + 1];
                a[i + 1] = temp;
                
                // 说明有交换过数值
                isSort = false;
            }
        }

// 当从头到尾，都没有进行过一次前后数值交换时，那么可以确定：数值已全部按顺序排列完成，因此可以直接跳出循环
        if (isSort) {
            break;
        }
    }

    // 输出结果
    for (int i = 0; i < a.length; i++) {
        System.out.print(a[i] + "\t");
    }
}
```





#### 快速排序

> 思路剖析：
>
> 先取出第一个值，然后设置两个指针，指针1 从前往后（从第二个开始），指针2 从后往前，分别与该值进行对比
>
> - 若指针1小于该值，则继续往后移动
> - 若指针2大于该值，则继续往前移动
>







## 面向对象

### 定义

1. 创建类，并设计类的内部成员（属性、方法）
2. 实例化类的对象
3. 通过对象，调用内部声明的属性或方法，完成对应功能

```java
// 创建类
class People{
    // 类的属性
    String name;
    char gender;
    int age;
    
    // 类的方法
    public void sleep(int times){
        System.out.println(name+"每天睡了"+times+"个小时");
    }
}

public static void main(String[] args){
    // 实例化类的对象
	People p = new People();
    
    // 通过类的对象，调用其内部声明的属性或方法
    p.name = "小凡";
    p.sleep(7);
}
```





### 特性

#### 方法重载

##### 知识点

两同一不同：

1. 两同：同一个类、相同方法名

2. 一不同：参数列表不同
   - ① 参数个数不同
   - ② 参数类型不同 

> 方法重载与权限修饰符、返回值类型、形参名，都没有关系

```java
public void test(int a,double b){ }
public void test(double a,int b){ }
// 可以构成重载，因为每个位置的参数类型不是都相同的
```



##### 面试题

```java
int[] a = new int[]{1,2,3};
System.out.println(a);	// 地址值

boolean[] b = new boolean[]{false,true,true};
System.out.println(b);	// 地址值

char[] c = new char[]{'a','b','c'};
System.out.println(c);	// abc
// println有许多重载方法，其中的 println(char[]) 是遍历char数组，而非打印地址值
```



#### 可变形参

1. 格式：方法形参的类型是确定的，但参数个数不确定

   ```java
   (参数类型 ... 参数名)
   // public void test(int ... a){ }
   ```

2. 可变形参的个数，可以是0个或多个，使用 `变量名.length` 来获取可变形参的长度

3. 可变形参可与同名的方法间构成重载（除了：可变形参的参数为相同类型的数组）

   ```java
   ① public void test(int ... a){ }
   ② public void test(){ }
   ③ public void test(int a){ }
   // 可以构成重载
   
   ④ public void test(int[] a){ }
   // 不可以构成重载
   // 在JDK5.0前，可变形参未出现时，就是使用数组代替可变形参
   ```

4. 可变形参必须声明在参数列表的最后（且仅可有一个可变形参）





#### 包与导包

package：包，用于指明文件中定义的类、接口等结构所在的包

- 一个源文件中只能有一个声明包的package语句，且作为源文件中的第一条语句
- 取包名时不能为：`java.xxxx`（系统会认为是java提供的API，进而，若在API中查找不到该类，则会报错）

import：导入，用来导入指定包下所需的类

- 声明在包的定义和类的声明之间

- 使用 `包.*` 表示导入该包下所有的类（如：java.util.* ）

- 导入的类或接口为：java.lang 或 当前包 下的，则可以省略import语句

- 若要使用：指定包下的 子包中的类，则还需要导入该包下的子包（的类）

- 同时使用 不同包下同名的类时，需要用全类名的方式指明调用的是哪个类，如：

  ```java
  java.util.Date date1 = new java.util.Date();
  java.sql.Date date2 = new java.sql.Date(xxxx);
  ```






#### 重写

1. 父类被重写的方法 与 子类要重写的方法 的 方法名与形参列表必须相同（形参列表若不同则构成重载了）

2. 子类重写的方法的权限修饰符 不小于 父类被重写的方法的修饰符

   > 子类不能重写父类中声明为private权限的方法

3. 父类被重写的方法的返回值类型为void或基本数据类型，子类重写的方法的返回值类型也必须与之相同

   父类被重写的方法的返回值类型为引用数据类型（如类），子类重写的方法的返回值类型要与其相同 或是 其子类

   > 父类原本返回的是Son类型，但子类重写后返回的是Object类型，Son类型无法接收Object类型（需要强转为Son类型）

4. 子类重写的方法所抛出的异常，要与父类被重写的方法抛出的异常相同 或 比其小

5. 静态方法不能被重写，且不存在多态性（声明的是哪个类，则是哪个类在调用）

   > 重写是根据调用时，运行的对象的类型 来决定调用的方法，而静态方法是编译时就确定被类所调用的，因此，当子类与父类声明同名同参的static方法时，也是两个方法

   ```java
   People p = new People();
   p = null;
   p.eat();	// 依然可以调用，静态方法的调用不依赖对象
   
   People p = new Student();
   p.eat();	// 方法被声明的类（People）所调用
   
   class People{
       public static void eat(){}
   }
   
   class Student extends People{
       public static void eat(){}	
   // 子类继承父类后，若有同名同参的方法，要不都为非静态（构成重写），要不都为静态（被类调用，是不同的两个方法）
   }
   ```

   









#### 注解

> 从JDK5.0开始引入，形式：@注解名

作用范围：包、类、构造器、方法、成员变量、参数、局部变量等



##### 特点

1. 可添加参数，以 name=value 的形式保存
2. 可仅注释作用，或在类编译、运行时加载，体现不同的功能



##### 常用的三个注解

1. @Override：该注解只能用于方法，用于确定子类是否重写了父类的方法
2. @Deprecated：用于代表修饰的元素（类或方法等）已过时
3. @SuppressWarnings：抑制编译器的警告信息



##### 元注解

> 对注解进行解释说明的注解

1. @Target：可通过枚举类ElementType的10个常量对象来描述：注解的使用场景

   如：TYPE（类）、METHOD（方法）、CONSTRUCTOR（构造器）、PACKACE（包）等

   ```java
   @Target({TYPE,METHOD,CONSTRUCTOR})
   // 用于表示该注解只能在类、方法或构造器中使用
   ```

2. @Retention：可通过枚举类RetentionPolicy的3个常量对象来描述：注解的生命周期

   如：SOURCE（源代码）、CLASS（字节码）、RUNTIME（运行时）

3. @Documented：表明该注解会被javadoc工具所记录（如：@Deprecated）

4. @Inherited：允许子类继承父类中的注解



##### 自定义注解

> 参考：SuppressWarnings

```java
@Target({TYPE})	// 元注解，可选
// 表示该自定义注解只能用于注解类

public @interface 注解名{
    // 是否有参数值，可选
    
    String value() defaule "test";
// 有一个参数值，为String类型，若不显示赋值，则默认值为"test"
    
}

// 使用举例：
@注解名(value="值")		// 显示赋值
class Test{ }
```





#### 包装类

使用场景：

1. 在某些场景下，需要使用基本数据类型对应的包装类对象，就需要将基本数据类型变量转换为包装类对象。比如：ArrayList的add(Object obj)；Object的equals(Object obj)
2. 而当使用包装类对象时，就不能进行+-*/等运算，就需要将包装类对象转换为基本数据类型变量



##### 自定义包装类

```java
public static void main(){
    MyInteger mi1 = new MyInteger(10);
    MyInteger mi2 = MyInteger.valueOf(10);
    int num = MyInteger.intValue();
}

// 自定义的包装类
class MyInteger{
    private final int value;
    // 调用包装类的构造器 获取包装类对象
    public MyWrapper(int value){
        this.value = value;
    }
    // 调用包装类的valueOf() 获取包装类对象（与源码有所不同）
    public static void valueOf(int value){
        this.value=value;
    }
    // 调用包装类的xxxValue() 获取基本数据类型变量
    public static int intValue(){
        return value;
    }
}
```



##### 基本数据类型与包装类

1）8大基本数据类型与其包装类的对应关系：

① byte --> Byte	② short --> Short	③ int --> Integer	④ long --> Long

⑤ float --> Float	⑥ double --> Double	⑦ boolean --> Boolean	⑧ char --> Character

> 数据类型（①-⑥）对应的包装类的父类都是Number



2）用法

基本数据类型与包装类间的转化：

1. 基本数据类型 --> 包装类：
   - （JDK9.0过时）使用包装类的构造器
   
     ````java
     Boolean b = new Boolean(false);
     ````
   
   - 调用包装类的静态方法valueOf()
   
     ````java
     Double d = Double.valueOf(12.3);
     ````
   
   - （JDK5.0）使用自动装箱直接给包装类对象赋值
   
     ```java
     Integer i = 10;
     ```
2. 包装类 --> 基本数据类型
   - 调用包装类的静态方法xxxValue()
   
     ```java
     Integer i = Integer.valueOf(10);
     int num = i.intValue();
     
     Double d = Double.valueOf(12.3);
     double num = d.doubleValue();
     ```
   
   - （JDK5.0）使用自动拆箱直接将包装类对象赋值给基本数据类型变量
   
     ```java
     Float f = Float.valueOf(12.3F);
     float num = f;
     ```
   
     



##### String与包装类/基本数据类型

用法：

1. 基本数据类型/包装类 --> String类型
   - 调用String的重载静态方法valueOf()
   
     ```java
     String s = String.valueOf(10);
     // "s"
     ```
   
   - 使用 +"" 进行连接运算
   
     ```java
     String s = 10 + "";
     ```
2. String类型 --> 基本数据类型/包装类
   
   - 调用对应包装类的静态方法parseXxx()
   
     ```java
     int i = Integer.parseInt("10");
     
     Double d = Double.parseDouble("12.3");
     ```



##### 总结

1. Float、Integer等使用构造器创建包装类对象时，可以传入的是字符串，但其字符串数值要满足转化类型
2. 深入Boolean底层源码可发现：使用构造器创建Boolean包装类对象时，在忽略true大小写的情况下，对比传入的值是否与之相等，否则都认为传入的形参值是false
3. 包装类8大基本数据类型中，除了浮点型（Float/Double）底层没有使用数据缓存对象外，其他基本数据类型都有对应的缓存数组（详见面试题）
   - Byte、Short、Integer、Long（-128~127）
   - Character（0~127）
   - Boolean（true、false）
4. 当基本数据类型变量与包装类对象做运算时，包装类对象会自动拆箱为基本数据类型变量（需要满足自动类型提升等关系）



##### 面试题

```java
Integer m = 1;
Integer n = 1;
System.out.println(m==n);	// true，底层使用了缓存数组

Integer m = 128;
Integer n = 128;
System.out.println(m==n);	// false，缓存数组仅为-128~127，超出则重新创建空间
```

```java
Integer m = 100;
double n = 100;
System.out.println(m==n);	// true，Integer先自动拆箱为基本数据类型，再进行比较
```









### 内存解析

#### 属性

对象在内存中的分配 所涉及到的内存结构：

1. 栈：方法中定义的变量，存储在栈中
2. 堆：实例化（new）出来的结构（包括对象中的属性）
3. 方法区：存放类的模板

```java
public static void main(String[] args){
// 执行main方法，形成栈帧入栈，args变量入栈（栈帧）
    
    People p1 = new People();
// 实例化People类的对象（变量p1入栈），在堆中开辟空间P，其类中的成员变量（属性）加载进空间P中（拥有初始值），并将此空间P的首地址赋值给变量p1
    
    p1.name = "Jack";
    p1.age = 18;
// 通过变量p1所存储的地址，找到其指向的堆空间P，修改其name属性和age属性的值
    
    People p2 = new People();
// 实例化People类的对象，p2变量入栈，在堆中开辟新空间PP，加载类中的成员变量，并将此空间PP的首地址赋值给变量p2
    
    p2.name = "Lucy";
    p2.age = 16;
// 通过变量p2所存储的地址，找到其指向的堆空间PP，修改其name属性和age属性的值
    p1.age = 20;
// 通过变量p1所存储的地址，找到其指向的堆空间P，修改其age属性的值
    
    System.out.println(p1.name+":"+p1.age+","+p2.name+":"+p2.age);
// 输出结果：Jack:20,Lucy:16
// 修改p1对象的属性，不会影响p2对象的属性（不是指向同一块堆空间）
    
    People p3 = p1;
// 创建变量p3（入栈），其值为变量p1的值（即：空间P的首地址）
    
    p3.age = 28;
// 通过变量p3的值（指向空间P的地址），修改空间P的属性age的值
    
    System.out.println(p1.name+":"+p1.age);
// 输出结果：Jack:28
// 变量p1将其值（空间P的地址）赋值给了变量p3，此时p3也指向了空间P，因此通过变量p3也能修改空间P的属性值
}
```

```java
// 创建类模板Mydate
class MyDate{
    int month;
    int day;
}

// 创建类模板Employee
class Employee{
    String name;
    // 引用类型变量
    MyDate birthday;
}

public static void main(String[] args){
// 调用main方法，栈帧入栈，变量args入栈帧
    
    Employee emp = new Employee();
// 实例化类模板Employee，在堆中开辟新空间E，属性（成员变量）存储在堆空间E中，将该空间的首地址赋值给变量emp
    
    emp.name = "Jack";
// 通过变量emp存储的地址，找到空间E，修改 实例变量name（实例化后的属性）的值
    
    emp.birthday = new MyDate();
// 等价于：① MyDate tmp = new MyDate();	② emp.birthday = tmp;
// 引用类型变量需要在堆中重新开辟空间M，然后将该空间M的地址赋值给birthday属性
// 即：栈帧中的变量emp，指向了堆中 空间E的birthday实例变量，该实例变量是引用数据类型，在被实例化后，又指向了堆中的空间M
    
    emp.birthday.month = 10;
    emp.birthday.day = 22;
// 通过变量emp存储的地址，找到空间E，修改其实例变量birthday 所指向的地址（空间M）中 属性month和属性day的值
}
```



#### 方法

```java
// People类的模板
class People{
    String name;
    int age;
    
    public void eat(){
        System.out.println("吃饭");
    }
    
    public void sleep(int hour){
        System.out.println("每天睡"+hour+"个小时");
    }
    
    public String interest(String hobby){
        return "喜欢"+hobby;
    }
}

public static void main(String[] args){
// 执行main方法，形成栈帧入栈，变量args入栈帧
    
    People p = new People();
// 创建People类型变量，变量p入栈，实例化People类，在堆中开辟空间P（属性存储在空间P中并拥有初始值），变量p存放空间P的首地址
    
    p.name = "Jack";    
    p.age = 24;
// 通过变量p的首地址，修改空间P中的name属性和age属性值
    
    p.eat();
// eat方法形成栈帧入栈，执行完方法内的输出语句（println方法入栈后出栈）后出栈
    
    p.sleep(7);
// sleep方法入栈，执行完其内的输出语句后出栈
    
    String hobby = p.interest("编程");
// hobby变量入main方法的栈帧中，而interest方法形成栈帧后入栈，执行完方法体后，返回String类型常量出栈，其结果返回到main方法中，被hobby变量接收
    
// main方法执行完，出栈，变量p对堆空间P的指向被销毁，空间P被GC回收
    
}
```



#### 对象数组

```java
// Student类的模板
class Student{
    int id;
    int score;
}

public static void main(String[] args){
// main方法形成栈帧入栈，args变量进入栈帧
    
    Student[] s = new Student[3];
// 引用数据类型变量s入栈帧，在堆中开辟了一块长度为3的连续的空间S（元素初始值为null），将该空间的首地址赋值给变量s
    
    for(int i=0; i < s.length; i++){
        s[i] = new Student();
    }
// 循环 实例化Student类的对象（其属性初始化并存储在堆中），在堆中开辟空间，将空间的首地址分别赋值给了：变量s指向的空间S中的各个元素    

    s[0].id = 1;
    s[0].score = 99;
// 通过变量s 找到其指向的空间S 中的第一个元素值 所指向的空间中的属性，并修改其值
    
}
```



#### 值传递

##### 基本数据类型

> 数据值传递

```java
public static void main(String[] args){
// main方法形成栈帧后入栈，args变量进入栈帧
    
    int a = 10;
    int b = 20;
    swap(a,b);
// swap方法形成栈帧后入栈，a、b变量（值为：10、20）作为实参传递进去swap方法中
// swap方法执行完后，出栈
}

public void swap(int a,int b){
// swap方法形成栈帧后入栈，形参变量a、b入栈帧（值为：10、20）
    
    int temp = a;
    a = b;
    b = temp;
// 形参值进行交换
}
```



##### 引用数据类型

> 地址值传递

```java
public static void main(String[] args){
// main方法形成栈帧后入栈，args变量入栈帧
    
    Test t = new Test();
// 变量t进入栈帧，实例化Test类的对象，在堆中开辟空间T（属性初始化），将其首地址赋值给变量t
    
    t.a = 10;
    t.b = 20;
// 修改空间T中属性的值
    
    swap(t);
// swap方法形成栈帧后入栈，变量t作为实参传递（值为：空间T的首地址值）
// 执行完swap方法中的内容后，出栈
}

public void swap(Test t){
// swap方法形成栈帧后入栈，形参变量t入栈帧（值为：空间T的首地址值）
    
    int temp = t.a;
    t.a = t.b;
    t.b = temp;
// 通过变量t指向的地址值，修改其指向的空间（空间T）的属性的值
}

// 类Test的模板
class Test{
    int a;
    int b;
}
```





### 类的成员

#### 属性

1. 变量在类中声明的位置不同，分为：

   - 成员变量（或称为：属性、field、字段、域）
   - 局部变量（如：方法内、方法的形参、构造器中）

2. 属性与局部变量的不同点：

   - 在内存中分配的位置不同

     属性：随着对象的创建，存储在堆空间中

     局部变量：存储在栈空间中

   - 属性可以使用权限修饰符修饰，局部变量不可以

   - 属性都有默认初始化值，局部变量没有

3. 在类中，为属性（实例变量）赋值的方式（按赋值的先后顺序排列）：

   ① 默认初始化 

   ② 显示初始化 、代码块中初始化（由两者声明的先后顺序决定谁先执行）

   ③ 构造器中初始化

   ④ 通过调用 `对象.方法` 、`对象.属性` 的方式赋值
   
   ```java
   // 父类
   class Base{
       // 构造器
       Base(){
           method(100);
       }
       
       // 代码块
       {
           System.out.println("Base");
       }
       
       public void method(int i){
           System.out.println("base:"+i);
       }
   }
   
   // 子类
   class Sub extends Base{
       // 构造器
       Sub(){
           super.method(70);
       }
       
       // 代码块
       {
           System.out.println("Sub");
       }
       
       // 重写父类方法
       public void method(int j){
           System.out.println("sub:"+j);
       }
   }
   
   public static void main(String[] args){
       Sub s = new Sub();	// 结果：base -> sub:100 -> sub -> base:70
   // 1、创建Sub对象时，调用构造器Sub()，但，在其内先调用super()并加载父类的构造器
   // 2、调用super()（即Base()构造器）前，先调用Base类中的代码块（代码块先于构造器执行）
   // 3、调用Base类中的代码块，输出《base》后，执行Base()构造器中的method(100)，但此刻，子类已重写了父类的方法，因此输出结果为《sub:100》
   // 4、父类构造器执行完后，执行Sub()构造器前，先执行Sub类中的代码块，输出《sub》
   // 5、执行子类构造器，调用父类method()，输出《base:70》
   }
   ```
   
   



#### 方法

1. 格式：

   ```java
   // 方法头
   权限修饰符 [其他修饰符] 返回值类型 方法名(形参列表) [throws 异常类型]{
       方法体
   }
   // []中的内容是可选的
   ```

2. 返回值类型分为：

   - 无返回值类型：void表示

   - 有返回值类型：用 想要返回的数据类型 表示

     > 使用 return 返回 对应的数据类型的变量或常量，可返回基本数据类型 或 引用数据类型

3. 方法不能独立存在，必须定义在类中，且不能在方法内定义方法

4. 类中的方法，创建实例后，已经预加载进方法区中，因此，方法内允许调用本类中的其他（或自身）方法及属性（而与方法定义的顺序无关）





#### 构造器

作用：

1. 在搭配new关键字后，可以创建 类的对象
2. 在创建类的对象的同时，可以为该对象的相关属性赋值

特点：

- 构造器的声明格式：

  ```java
  权限修饰符 类名(形参列表){ }
  // public Person(String name,int age){ }
  ```

- 在定义的类中，若没有显示的提供任何构造器，则会默认提供一个 空参的构造器，且 该构造器的权限与此类的权限相同

- 一个类中可以声明多个构造器，且多个构造器间彼此构成重载



#### 代码块

作用：用来初始化 类或对象的信息（即：初始化类或对象的成员变量）

代码块只能使用static修饰，按有无被static修饰，分为：

1. 静态代码块：用来初始化类的信息（静态结构）

   - 随着类的加载而执行，且由于类只会加载一次，因此静态代码块也只会执行一次

     > 使用场景：在初始化静态属性，且当该属性的初始化不能一次性完成时 使用

2. 非静态代码块：用来初始化对象的信息（非静态结构）

   - 随着对象的创建而执行，每当创建一个类的对象，则会执行一次非静态代码块
   - 非静态代码块中也能调用静态结构

> 若类中声明了多个代码块，则：静态代码块先于非静态代码块执行，若声明有多个静态代码块，则按照声明的先后顺序执行

代码块作用域：

- 代码块中声明的属性，仅作用于代码块中

  ```java
  static int x;
  
  static{
      int x = 5;
      x--;
      // 代码块中的x非静态属性，出了代码块后无效
  }
  
  static{
      x--;	// 调用的是 static int
  }
  
  public static void main(String[] args){
      System.out.println(x);	// x = -1
  }
  ```



#### 内部类

##### 知识点

分为：

1. 成员内部类：声明在内部类里面，分为：静态成员内部类、非静态成员内部类
   - 以类的角度看待成员内部类：
     - 成员内部类的内部可声明：属性、方法、构造器、代码块、内部类等结构
     - 内部类中可继承或声明父类、可实现接口
     - 可以使用final、abstract等修饰
   - 从作为外部类的成员的角度看待成员内部类：
     - 像方法一样，在内部类的内部可以调用外部类的结构
     - 像属性一样，可以使用权限修饰符进行修饰
     - 像属性或方法引用，可以被定义为static
2. 局部内部类：声明在方法、构造器、代码块内，分为：匿名局部内部类、非匿名局部内部类



##### 使用举例

```java
public static void main(String[] args){
    A.C ac = new A.C();		// 创建类A的静态成员内部类C的对象
    
    A.B ab = new A().new B();	// 创建类A的非静态成员内部类B的对象
    ab.getName("Jack");	// 调用成员内部类B中的方法
}

class A{
    String name = "Tom";	// 类A中的属性
    
    class B{	// 非静态成员内部类
    	String name = "July";
        
        public void getName(String name){
            System.out.println(name);	// 形参，Jack
            System.out.println(this.name);	// 成员内部类B中的属性，July
            System.out.println( A.this.name );	// 类A中的属性，Tom
        }
    }	
    
    static class C{ }	// 静态成员内部类
    
    public void test(){
        
        class AA{ }	// 成员内部类
    
    }
}
```



##### 接口的匿名实现类的匿名对象

> 匿名实现类也属于内部类

```java
public static void main(String[] args){
    // 常规做法2：创建实现类的对象，通过实现类的对象调用重写的方法
    B b = new B();
    b.test();
    
    new A(){		// 由于是匿名的实现类，因此没有名（借用接口名），所以借用接口名
        public void test(){		// 创建实现类（实现接口）需要重写接口中的抽象方法
			// 方法体	
        }  
    }.test();
    // 因为是匿名的实现类，对象只能造一次，而对象也是匿名的，因此直接调用重写后的方法
	// 若不使用匿名对象，则可以利用多态性将new后的对象赋值给接口A（test()已被重写）
// 若非重写方法，而是定义新的方法，则无法利用多态性赋值给其父类或接口（因为父类或接口中无此方法（不是被重写方法），无法调用）
}

// 接口A
interface A{
    public abstract void test();
}

// 常规做法1：由一个类实现接口A，再调用该类
// 实现类B
class B implements A{
    public void test(){		// 重写接口中的方法
    	// 方法体
    }
}
```









### 关键字

#### this

理解：指的是 当前对象 或 当前正在创建的对象

```java
class Person{
    private String name;
    private int age;	// 属性
    
    Person(String name){
        this.name = name;
        // this指的是：在其他方法中，正在创建中的对象
    }
    
    Person(String name,int age){	// 构造器
        this(name);
        // 调用其他构造器
        this.age=age;
    }
    
    public void setName(String name){	// 为私有属性赋值
        this.name = name;
        // this指的是：在其他方法中，调用该方法的对象
        
        this.eat();
        // 此处调用方法时，this关键字可省略
    }
    
    public void eat(){	// 方法
        System.out.println("吃饭");
    }
}

public static void main(String[] args){
    Person p = new Person();	// 创建对象
    p.setName("Tom",10);	// 调用对象的方法
}
```

this可以调用的结构：

1. 成员变量（属性）：`this.属性`

2. 方法：`this.方法()`

3. 构造器：`this(形参列表值)`

   > 调用其他构造器时，该语句必须声明在当前构造器的首行（因此，只能调用一个其他的构造器）
   >
   > 若调用自身构造器，会造成递归，陷入死循环（因此，n个构造器中，最多只有n-1个构造器能声明该语句）

   

#### super

##### 知识点

作用：

1. 子类继承父类后，重写了父类的方法，若想再调用父类中被重写的方法，则要使用super关键字
2. 子类继承父类后，子类和父类都定义了同名的属性，则需要用super来调用父类中同名的属性（调用哪个属性满足：就近原则）

> 前提：满足封装性要求

可以调用的结构：

1. 属性：`super.属性`

2. 方法：`super.方法名()`

3. 构造器：`super(形参列表值)`

   > ① 子类继承父类，不会继承构造器，只能通过 super(形参列表) 调用父类中指定的构造器
   >
   > ② super(形参列表) 必须声明在构造器的首行（因此，this(形参列表)调用本类重载方法 和 super(形参列表) 调用父类构造器 只能同时调用其一）
   >
   > ③ 子类构造器中，没有显示调用 this(形参列表) 或 super(形参列表) ，则默认调用 super()（父类的空参构造器），若此时，父类不存在 super() ，则报错
   >
   > - 任何子类的构造器中，一定调用了 this(形参列表) 或 super(形参列表)
   > - 子类中，若声明了n个构造器，则至多有n-1个构造器使用了 this(形参列表)，剩下的那个一定是使用了 super(形参列表)
   > - 调用父类构造器的目的是：将父类中声明的 属性或方法 加载到内存中，使子类对象能够调用



##### 面试题

```java
class Father{
    private String info = "a";
    public void setInfo(String info){
        this.info = info;
    }
    public String getInfo(){
        return info;
    }
}

class Son extends Father{
    private String info = "b";
    public void test(){
        System.out.println(this.getInfo());
        System.out.println(super.getInfo());
    }
}

public static void main(String[] args){
    Father f = new Father();
    Son s = new Son();
    System.out.println(f.getInfo());	// a
    System.out.println(s.getInfo());	// a
// （子类没有重写父类的方法）在调用同名属性时，根据就近原则，选择距离最近的变量值
    s.test();	// a a
}
```



#### static

##### 知识点

用来修饰的结构：属性、方法、代码块、内部类

> 变量分类：
>
> 1. 按数据类型：基本数据类型、引用数据类型
> 2. 按在类中声明的位置：
>    - 成员变量：按是否被static修饰分类：
>      - 被static修饰：静态变量、类变量
>      - 没有static修饰：非静态变量、实例变量
>    - 局部变量：方法内、方法形参、构造器内、构造器形参、代码块中等



##### 静态与非静态

1）静态变量与实例变量的对比

1. 个数：

   - 静态变量：在内存中仅有一份，被类的多个对象共享
   - 实例变量：每个类的实例，都有自己的一份

2. 内存中的位置：

   - 静态变量：（jdk6前）方法区 或 堆空间中（jdk7后）
   - 实例变量：存放在堆空间的实例对象中

3. 加载时机：

   - 静态变量：随着类的加载而加载（由于类仅加载一次，因此静态变量也只加载一份）
   - 实例变量：随着对象的创建而创建，每个对象拥有一份实例变量

4. 调用：

   - 静态变量：可以使用类直接调用，也能使用对象调用
   - 实例变量：只能使用对象调用

   > 类可以调用类变量，但不能调用实例变量（实例变量在创建实例后才会被加载）
   >
   > 对象可以调用类变量，也能调用实例变量

5. 消亡时期：

   - 静态变量：随着类的卸载而消亡
   - 实例变量：随着对象的回收而消亡

2）静态方法和非静态方法的对比：

1. 静态方法可以直接通过类来调用

2. 静态方法不能被重写，且不存在多态性

3. 静态方法内，可以调用静态的属性或方法，不能调用非静态的属性或方法

   非静态方法可以调用类中的 静态和非静态结构（属性、方法）

   > 静态方法中要调用非静态方法，只能通过造对象后，通过对象来调用（造对象后，非静态的也加载完成，便可调用）

4. static修饰的方法内，不能使用this和super关键字（静态方法随着类的加载而加载，不一定已经实例化了对象）

3）使用静态属性和静态方法的场景：

静态属性：

1. 多个类的实例可以共享一个成员变量，且此成员变量的值是相同的
2. 一般将一些常量修饰为静态的，如Math.PI

静态方法：

1. 若方法内操作的变量为静态的，则此方法一般也声明为静态方法（不是静态方法也不报错，因为实例化晚于类的创建）
2. 一般将工具类中的方法声明为静态方法，如Arrays类



##### 面试题

```java
class Order{
    public static int count = 1;
    public static void hello(){
        System.out.println("Hello!");
    }
}

public static void main(String[] args){
    Order order = null;
    order.hello();	// Hello!
    System.out.println(order.count);	// 1
    // 因为是被静态修饰，随类的加载而加载，因此不会出现空指针异常
    // 静态方法的调用与对象无关，都是通过类来调用的
}
```





#### final

用来修饰的结构：

1. 类：此类不能被继承。如：String类
2. 方法：此方法不能被重写。如Object类中的getClass()方法
3. 变量：变量变常量，其值不能被修改。
   - 若修饰成员变量，则创建后应为其附上值。变量赋值位置：显示赋值、代码块中赋值、构造器中赋值
   - 若修饰局部变量，则在调用前一定要先赋值
4. static 与 final 共同修饰成员变量时，此变量称为：全局变量。如Math.PI





#### abstract

> 像几何图形为父类，子类可以为圆、三角形等，其计算面积和周长的方式不一样，但都有相同的方法（求面积、求周长），而父类又无法实现这些方法。因此，父类提供的方法可以没有方法体（即抽象方法），让子类去重写父类的方法。同时，父类也没有被调用的必要（即抽象类）

用来修饰的结构：类、方法

1. 类：抽象类
   - 抽象类不能被实例化
   - 抽象类包含构造器（子类在实例化对象时，父类的构造器可以供子类调用）
   - 抽象类中可以不声明抽象方法（同理，父类的方法可以供子类调用）
2. 方法：抽象方法
   - 抽象方法只有声明，没有方法体
   - 有抽象方法的类，一定是抽象类（为了 防止通过对象来调用 没有方法体的方法）
   - 子类必须重写父类中所有的抽象方法，才能被实例化，否则也是个抽象类（为了防止子类通过对象来调用抽象方法）

不能与abstract共用的关键字：

- private方法：子类不能重写父类中私有的方法
- static方法：防止通过类来调用抽象方法
- final方法：被final修饰的方法不能被重写
- final类：被final修饰的类不能被继承





#### interface

##### 知识点

1）可以声明的结构：

1. 属性：必须使用 `public static final` 修饰（即：全局常量）

2. 方法：

   - jdk8前：仅能声明抽象方法（即：`public abstract` ）

     jdk8：新增静态方法、默认方法

     jdk9：新增私有方法

   > 不可声明的结构：构造器、代码块

2）特点：

1. 一个类可以实现多个接口，一个类只能继承一个父类（单继承，多实现）

   ```java
   class A extends SuperA implement B,C { }
   // A 与 SuperA：继承关系
   // A 与 B、C：实现关系
   ```

2. 类要实现接口时，必须将接口中的所有抽象方法实现（重写）后才能实例化，否则，该类必须声明为抽象类

3. 接口与接口间可以有继承关系，且可以多继承

   ```java
   interface A extends B,C { }
   ```

4. 接口可以有多态性：接口名 变量名 = new 实现类对象名( )

   > 也可以使用instanceof判断：某对象是否是某接口的实现

5. 接口的：匿名实现类 的 匿名对象：

   ```java
   interface A{
       public abstract void test();
   }
   
   public static void main(String[] args){
       // 因为类是匿名的，因此，借用接口名来创建（实现类的对象），但
       new A(){
           public void test(){ }	// 需要重写接口中的方法
       }.test();	// 创建完匿名类的匿名对象后，直接调用该方法
   }
   ```



##### 新特性

1. 静态方法（JDK8）：`public static 返回值类型 变量名(){ 方法体 }`
   - 接口中声明的静态方法，只能由接口调用，不能由其实现类的对象调用
2. 默认方法（JDK8）：`public default 返回值类型 变量名(){ 方法体 }`
   - 接口中的默认方法会被实现类继承，当实现类中没有重写该方法时，默认调用的就是接口中的默认方法（若重写了默认方法，则调用的是重写后的方法）
   - 若类实现了两个接口，且这两个接口中，有着同名同参数的默认方法，则：当该类没有重写默认方法，会报错（接口冲突）
   - 若类继承了父类，且实现了接口，当父类和接口中都有着同名同参数的默认方法，则：当该类没有重写默认方法时，调用的是父类中的方法（类优先原则）
   - 在实现类的方法中，调用接口的默认方法：`接口名.super.默认方法名()`
3. 私有方法（JDK9）：`private 返回值类型 方法名(){ 方法体 }`
   - 仅能在接口中调用（可用于封装重复的默认方法的方法体，减少代码冗余）

```java
public static void main(String[] args){
    A.test1();	// 只能接口调用静态方法
}

// 接口
interface A{
    public static void test1(){ }	// 静态方法
    public default void test2(){ }	// 默认方法
    private void test3(){ }		// 私有方法
}

// 实现类
class B implements A{
    public void test2(){	// 实现类重写了接口中的默认方法
    	A.super.test2();	// 实现类方法中，调用接口中的默认方法
    }	
}
```





#### enum

枚举类本质也是类，只是枚举类的对象是有限的、固定的，无法随意创建

> 因此，若枚举类的对象只有一个，则可以看做单例模式的一种实现方式

特点：

1. 使用enum关键字定义的枚举类，其父类为java.util.Enum（因此，使用enum定义的枚举类，不可再显示的继承其他父类）
2. Enum中的常用方法



##### class类实现

> JDK5.0前使用

```java
class Person{
	// 1.定义类中的实例变量
    private final int age;
// 用private修饰，仅提供get方法，让外界获取，并用final修饰，使该变量在创建后能被附上值，且一旦被赋值后，就不要再更改（不可使用static修饰，否则所有对象共用同一个实例变量）
    
    // 2.私有化构造器，并通过构造器对实例变量进行赋值
    private Person(int age){
        this.age = age;
    }
    
    // 3.提供属性的get方法
    public int getAge(){
        return age;
    }
    
    // 4.创建当前类的对象
    public static final Person p1 = new Person(18);
    public static final Person p2 = new Person(20);
// 直接使用public static修饰，不额外用get方法获取实例对象。使用final修饰确保该对象不会被修改值（如在外界调用时：Person.p1 = null）
}
```



##### enum关键字实现

> JDK5.0后使用

```java
enum People{
    // 1.必须先声明好要实例的对象
// 原先写法：public static final People p1 = new People(19);
// 由于public static final People是所有People对象都共有的，因此省略，且必须省略！同样，=new People也是共有的，也要省略（若没有实例变量，则()也可以省略）
    p1(19),p2(20);	//对象之间用,隔开（没有对象时，也要用;表示）
    
    // 2.定义实例变量
    private final int age;
    
    // 3.私有化构造器
    private People(int age){
        this.age = age;
    }
// 构造器也一定是private的，但可以省略，也可以不省略
    
    // 4.提供属性的get方法
    public int getAge(){
        return age;
    }
}
```



##### 实现接口

1. 枚举类实现接口，当不同的枚举类对象调用该方法时，执行的是同一个方法

   ```java
   interface A{
       public abstract void test();
   }
   
   enum People implements A{
       ;	// 表示没有对象
       // 空参构造器
       public void test(){}	// 重写方法
   }
   ```

2. 枚举类的每个对象实现接口中的抽象方法，此时每个对象调用的就不再是同一个实现方法了

   ```java
   interface A{
       public abstract void test();
   }
   
   enum People implements A{
       p1(){
           public void test(){}	// 重写方法
       },
       p2(){
           public void test(){}	// 重写方法
       };
   }
   ```






##### 方法

【String】`toString()`：默认调用，返回的是当前的对象名（可重写）

【String】`name()`：也是返回当前的对象名

【int】`ordinal()`：返回当前对象所声明的位置（即下标）

【枚举类型[ ]】（static）`values()`：返回 当前枚举类型的所有对象 的数组

【枚举类型】（static）`valueOf(String)`：根据填入的字符串，找到对象名与其相同的，返回该对象（若不存在该对象，报错）

```java
public static void main(String[] args){
    System.out.println(People.p1);	// p1
    
   	System.out.println(People.p1.name);	// p1
    
    System.out.println(People.p2.ordinal);	// 1
    
    People[] p = People.values();
    for(int i=0;i<p.length;i++){
        System.out.println(p[i].getAge);	// 19,20
    }
    
    People pp = People.valueOf("p1");
    System.out.println(pp.getAge());	// 19
}

// 枚举类
emun People{
    p1(19),p2(20);
    private final int age;
    private People(int age){
        this.age = age;
    }
    public int getAge(){
        return age;
    }
}
```







### 三大特性

#### 封装

> 用4种权限修饰符修饰类及其内部成员，从而在调用时，体现了其可见性的大小

权限修饰符：private、缺省、protected、public

- private：仅允许在 本类内部中使用
- 缺省：允许在 本包中使用（包括本类）
- protected：允许在 本包或其他包的子类中使用
- public：在该工程下任意地方可使用

类：仅允许使用public、缺省修饰

> public所修饰的类的名字必须与文件名相同，且一个文件中仅可有一个public类

类内部成员：可用4种权限进行修饰





#### 继承

1. 场景：

   - 自上而下：定义了类A，在定义另一个类B时，发现类B功能与类A相似，则考虑让类B 继承于类A
   - 自下而上：定义了类B/C/D，发现类B/C/D中有相似的属性或方法，则考虑将其相同的内部成员进行抽取，封装为类A，让类B/C/D继承于类A

2. 用处：

   - 减少代码冗余，提高代码复用性
   - 利于功能的拓展
   - 让类与类间构成了 is - a 的关系

3. 特点：

   - 子类能获取到父类中声明的所有方法或属性（但受到封装性的影响，可能不能直接调用）

   - 子类继承父类后，也可拓展出自己特有的内部成员（使用 extends 关键字）

   - 若没有显示声明父类，则默认继承于 java.lang.Object

   - 支持多层继承（直接父类、间接父类），子父类的概念是相对的

   - 一个父类可以有多个子类，但一个子类只能有一个父类（单继承性）

     > 若不是单继承的话，可能：创建子类对象后，直接或间接调用 super(形参列表)，从而在加载父类的属性或方法时，不知道加载的先后顺序；且最后也都直接或间接继承了Object类

4. 定义

   ```java
   class A{ }
   class B extends A{ }
   
   // 类A：父类、superClass、超类、基类
   // 类B：子类、subClass、派生类
   ```





#### 多态

##### 知识点

体现：父类的引用指向子类的对象（子类的对象赋给父类的引用）

> 如：Person p = new Man();

- 编译时，认为调用的是左边父类中的方法

  运行时，执行的是右边子类重写父类后的方法

使用前提：

1. 要有类的继承关系
2. 适用于方法（重写了父类的方法），不适用于属性（属性值由左边的声明决定）

好处：虚拟方法调用（动态绑定），减少代码冗余（不需调用多个重载方法）

弊端：虽然创建了子类对象，也加载了子类特有的属性和方法，但由于声明为父类的引用，因此无法直接调用子类所特有的属性或方法（使用向下转型处理）

- a instanceof A：判断对象a是否是类A的实例（也可以判断：对象a是否是接口A的实现）

  ```java
  Person p = new Man();
  if(p instanceof Man){	// true
      Man m = (Man)p;	// 向下转型
  }
  if(p instanceof Woman){	// false
      Woman w = (Woman)p;
  }
  if(p instanceof Person){
      // true,若类S是类A的父类，则对象a也instanceof类S
  	// 因此，若存在父类S，则一般要在最后才进行判断
  }
  ```



##### 用法

```java
// 父类：几何图形
class GeometricObject {
	protected String color;	// 颜色
    protected double weight;	// 权重
    
    public double findArea(){	// 求面积方法
        return 0;
    }
}

// 子类：圆
class Circle extends GeometricObject{
    private double radius;	// 半径
    
    // 构造器
    public Circle(double radius) {
        this.radius = radius;
    }
    
    public void isCircle(){	// 子类特有的方法
        System.out.println("this is a circle");
    }
    
    public double findArea(){	// 重写父类求面积方法
        return Math.PI*radius*radius;
    }
}

// 子类：矩形
class Rectangle extends GeometricObject{
    private double width;	// 宽
    private double height;	// 高
    
    // 构造器
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    public void isRectangle(){	// 子类特有的方法
        System.out.println("this is a rectangle");
    }
    
    public double findArea(){	// 重写父类求面积的方法
        return width*height;
    }
}

// 测试类
class Test{
    // 对比两个几何图形的面积是否相等
    public void equalsArea(GeometricObject g1,GeometricObject g2){
        if (g1.findArea()==g2.findArea()){
            System.out.println("相等");
        }else{
            System.out.println("不相等");
        }
    }
    
    // 求几何图形的面积
    public void displayGeometricArea(GeometricObject g) {
        if(g instanceof Circle){
            Circle c = (Circle)g;	// 向下转型
            c.isCircle();	// 调用子类特有方法
        }
        if(g instanceof Rectangle){
            Rectangle r = (Rectangle)g;	// 向下转型
            r.isRectangle();	// 调用子类特有方法
        }
        System.out.println(g.findArea());
    }
}

public static void main(String[] args){
    Test t = new Test();
    t.displayGeometricArea(new Rectangle(1.0, 2.0));
    // 多态，传的是GeometricObject的子类
    
    Circle c1 = new Circle("red", 1.0, 2.0);
    Circle c2 = new Circle("red", 1.0, 2.0);
    gt.equalsArea(c1, c2);
    // 多态，传的是GeometricObject的子类
}
```



##### 面试题

```java
class Base{
    int count = 10;
    public void display(){
        System.out.println(this.count);
    }
}
class Sub extends Base{
    int count = 20;
    public void display(){
        System.out.println(this.count);
    }
}

public static void main(String[] args){
    Sub s = new Sub();
    System.out.println(s.count);	//20，调用子类对象的属性
    s.display();	//20，调用子类对象的方法，方法中，this调用的属性遵循就近原则
    
    Base b = s;	//多态
    System.out.println(b==s);	//指向同一块堆空间
    System.out.println(b.count);	//10，属性不支持多态，属性值看左边的声明
    b.display();
    //20，方法调用满足多态，调用子类重写父类的方法，方法中，this遵循就近原则
}
```





### 补充

#### 匿名子类与对象

1）匿名对象：`new 类名();`

> 匿名对象只能被调用一次（在每次创建时都是用不同的新地址），常常作为方法的实参传递并赋值给方法内的某个变量（当匿名对象的地址被赋值给变量后，就可以被多次调用了）

2）匿名子类：`类名 变量名 = new 类名(){};`

-  此时的子类是匿名的，且子类没有重写父类中的方法（与匿名实现类类似）
-  利用多态性可将该匿名子类的对象赋值给变量（父类的引用指向子类的对象）





#### 递归

> 递归结束循环的重要特性：向着 已经存在的结束条件 逐渐靠拢

##### 斐波那契

```java
// 斐波那契数列：1 1 2 3 5 8 13 21 34 55 ... 
public int getNum(int num){
    if(num == 1){
        return 1;
    }else if(num == 2){
        return 1;
    }else{
        return getNum(num-1)+getNum(num-2);
    }
}
// 4 --> getNum(3)+getNum(2)
// 3 --> getNum(2)+getNum(1)
// 逐渐向 num=2、num=1 靠拢
```



##### 数的阶乘

```java
// n的阶乘
public int getNum(int num){
    if(num==1){
        return 1;
    }else{
        return getNum(num-1)*num;
    }
}
// 4 --> getNum(3)*4
// 3 --> getNum(2)*3
// 逐渐向 num=1 靠拢
```





##### 获取目录大小

```java
public long getSize(File file){
    long sum = 0;
    if(file.isFile){
        sum = file.length();
    }else{
        File[] files = file.listFiles();
        for(File f:files){
            sum += getSize(f);
        }
    }
    return sum;
}
```

```java
public long getSize(File file,long sum){
    if(file.isFile){
        sum += file.length();
    }else{
        File[] files = file.listFiles();
        for(File f:files){
            sum = getSize(file,sum);		
         // 每次递归，都会在找到文件后才结束递归。
   // 所以，在每层递归结束后，都要将sum重新赋值，确保sum值进入下一层递归时，其值依旧是被累加
        }
    }
    return sum;
}
```







#### 单例模式

##### 饿汉式

特点：立即加载

> 当类加载时，实例就创建好了

```java
class Bank{
    // 1、私有化构造器
    private Bank(){}

    // 2、在类的内部提供一个实例化对象（私有化，仅能通过方法调用）
    // private Bank b = new Bank();
    
    // 5、get方法为静态后，静态方法想要获取属性值，该属性也必须为静态
    private static Bank b = new Bank();

    // 3、提供get方法返回实例化对象
    // public Bank getInstance(){
    //     return b;
    // }
    
    // 4、但由于调用该方法需要创建对象，而只能通过方法获取对象（相互矛盾），因此，该方法要声明为静态方法，使其能通过类直接调用
    public static Bank getInstance(){
        return b;
    }
}

public static void main(String[] args) {
    Bank a = Bank.getInstance();
    Bank b = Bank.getInstance();
    System.out.println(a==b);	// true，同一个对象，地址值相同
}
```



##### 懒汉式

特点：延迟加载

> 在需要使用该实例时，再进行创建

```java
class GirlFriend {
    // 1、私有化构造器
    private GirlFriend() {}

    // 2、提供一个实例化对象，但先不创建（私有化，仅能通过方法调用）
    // 6、同理，为了保证static方法能读取到该属性，该属性应声明为static属性
    private static GirlFriend gf = null;

    // 3、通过get方法返回实例化对象
    // 5、同理，为了保证能调用到该get方法，该方法应声明为static方法
    public static GirlFriend getInstance() {
    // 4、当需要获取实例化对象了，再来创建实例化对象，且应保证该实例化对象仅造一次
        if (gf == null) {
            gf = new GirlFriend();
        }
        return gf;
    }
}

public static void main(String[] args) {
    GirlFriend gf1 = GirlFriend.getInstance();
    GirlFriend gf2 = GirlFriend.getInstance();
    System.out.println(gf1 == gf2);	// true，同一个对象，地址值相同
}
```



##### 总结

- 饿汉式在内存中加载较早，使用更方便、更快，但会长期占用内存资源

  特点：线程安全

- 懒汉式在需要使用时才创建，节省了内存空间，但线程不安全





#### 单元测试

##### 导入

> 需要导入的jar包：junit.jar、hamcrest-core.jar

1. jar导入：File --> Project Structure --> Libraries --> + --> Java -->  选择要导入的jar包 --> 选择引用的模块 --> 应用（Apply）

2. 其他模块应用该jar包：Modules --> 选择模块 --> Add Selected --> + --> Library --> 选择jar包 --> Dependencies --> 选择jar包 --> Scope选项改成Compile --> 应用



##### 用法

```java
public class JUtilTest{
    @Test
    public void test(){}
}
```

使用前提：

1. 所在的类必须是public、非抽象、只有唯一无参构造器

   > @Test方法是通过 无参构造器 造该类的对象 来调用的

2. @Test标记的方法必须为public、非抽象、非静态、无返回值类型（void）、无形参

   > 非抽象：抽象方法不能有方法体
   >
   > 非静态：非静态方法可以直接调用非静态方法或静态方法，静态方法要调用非静态方法，需先造对象（加载后才能调用）
   >
   > 无返回值类型：与main一样，置于最底层，无任何可返回
   >
   > 无参：通过对象调用，没有传递任何参数

解决Scanner输入失败方法：HELP --> Edit Custom VM Options... --> 输入以下代码后重启

```java
-Deditable.java.test.console=true
```





#### JavaBean

> 常见于 循环创建不同参数、相同类型的对象时

满足指定规则的Java类，则被称为JavaBean：

1. 类是公共的

2. 有一个无参的公共构造器

   >原因1：子类对象实例化时，子类构造器默认会调用父类的空参构造器
   >
   >原因2：反射中，经常用来动态的创建运行时类的对象，提供空参的构造器就不需要考虑参数问题

3. 有属性，且有对应的 get、set 方法







#### < init >方法

< init >方法在字节码文件中查看，代码中的构造器在编译后，就会以该方法的形式呈现，每个< init >方法对应着类中的每一个构造器

- < init >方法内部的代码包含了：实例变量的显示赋值信息、代码块中的赋值信息 以及 构造器中的代码（包括调用super(形参列表) ）的信息
- < init >方法用来初始化当前创建的对象的信息







#### 比较对象大小

##### 自然排序

> 实现Comparable接口

```java
// 1.让 要比较的对象的类 实现Comparable接口
class Phone implements Comparable{
    private String name;
    private double price;
	public Phone(String name,double price){
        this.name=name;
        this.price=price;
    }    

// 2.重写Comparable接口中的compareTo(Object o)方法
    public int compareTo(Object o){

// 3.在方法体内书写比较规则
        if(o == this){
            return 0;			// 返回0表示相等
        }
        
        if(o instanceof Phone){
            Phone p = (Phone)o;
            
            int value = Double.compare(this.price,p.price);
            // 使用包装类的比较方法，返回1表示前者比后者大
            
            if(value!=0){
                return -value;		// 若两者价格不相等，价格按从大到小排序
            }
            
            return this.name.compareTo(p.name);
            // 若两者价格相等，则按名字从小到大排序（String已重写compareTo方法）
        }
        
        throw new RunTimeException("类型不匹配");	// 比较的类型不同时，抛出异常
    }
}

public static void main(String[] args){
    Phone p1 = new Phone("Huawei",5555);
    Phone p2 = new Phone("Honor",6666);
    p1.compareTo(p2);		// 返回负数，表示p1比p2小
}
```





##### 定制排序

> 实现Comparator接口的方式

```java
// 1.创建一个类实现Comparator接口
class A implements Comparator{
    
// 2.重写Comparator接口中的compare(Object o1,Object o2)方法
    public int compate(Object o1,Object o2){
        
// 3.在方法体内书写比较规则
        if(o1 instanceof String && o2 instanceof String){
            String s1 = (String)o1;
            String s2 = (String)o2;
            
            return s1.compareTo(s2);	
            // 按名字从小到大排序（String重写了compareTo方法）
        }
        
        throw new RunTimeException("类型不匹配");	// 比较类型不同时，抛出异常
    }
}

public static void main(String[] args){
    String[] s = new String[]{"July","Tom","Mike","Jary","Amy"};
	Arrays.sort(s,new A());  	// Amy,Jary,July,Mike,Tom
}
```





#### 增强for循环

```java
for(容器的元素类型 临时变量 : 容器){
    对临时变量进行操作;
}
```

实现步骤：依次取出容器（数组或集合）中的元素，放入临时变量中，对临时变量进行操作

> 1. 对临时变量的操作，可能不会影响原有容器中的元素
> 2. 对于集合来讲，增强for循环 底层仍是使用迭代器

使用举例：

```java
ArrayList a = new ArrayList();
a.add(1);
a.add(2);

// for(集合中的元素类型Object 临时变量o : 集合a){ }
for(Object o:a){
    System.out.println(o);
}
```









## 高级特性

### 异常

> Java中把不同的异常用不同的类表示，出现异常时，则创建该异常类型的对象，并抛出



#### 异常的体系架构

java.lang.Throwable（异常体系的根父类）

​	|--- java.lang.Error（错误：Java虚拟机无法解决的问题，一般不编写代码进行处理）

> 如：StackOverflowError、OutOfMemoryError（堆溢出）

​	|--- java.lang.Exception（异常：一般编写针对性代码进行处理）

​			|--- 编译时异常（受检异常，在执行 javac.exe 时出现的异常）

> 如：ClassNotFoundException、FileNotFoundException（子类）、IOException（父类）

​			|--- 运行时异常（非受检异常，在执行 java.exe 时出现的异常）

> 如：ArrayIndexOutOfBoundsException）、NullPointerException、ClassCastException（类型转换异常）、NumberFormatException（数值转换异常）、InputMismatchException（输入类型异常）、ArithmeticException（算术异常）



#### 异常的处理方式

步骤一：抛

- （自动抛）程序在执行过程中，一旦出现异常，就会在该代码处，生成对应的异常类对象，并将此对象抛出，且一旦抛出，后续的代码就不会再执行
- （手动抛）程序在运行过程中，若用户进行了不合我们预期的操作，则可以手动生成异常类对象，且一旦该异常类对象生成并被抛出，后续的代码就不会再执行

步骤二：抓

- 狭义上：对抛出的异常对象进行try-catch，一旦有对异常进行try-catch，则后续的代码可以正常执行
- 广义上：对抛出的异常对象进行捕获：try-catch（直接处理）、throws（向上抛出）



##### try-catch

```java
try{
    // 可能出现异常的代码，若出现异常后，则抛出异常类对象
    // 出现异常后，try中后续的代码都将不再执行
}catch(异常类型 变量名){
    // 捕获到异常后执行的代码，没有捕获到异常，则不执行
}finally{
    // 无论try中是否存在return，无论catch中是否会出现异常，都会执行finally中的语句
    // finally中，常用于对资源流的关闭
}
// 后续代码，若对异常进行了捕获，则可以正常执行
// 若出现异常后，依旧没有对异常进行处理，则程序会终止运行
```

> 注意：
>
> 1. try中声明的变量，在出了try结构后，则不能再被调用
> 2. 可以声明多个catch结构，不同的异常类型，若不存在子父类关系，则声明的先后顺序无要求，否则，子类声明必须在父类声明的上面
> 3. 除了一种特殊情况外，finally中的语句都会被执行：使用 System.exit(0) 直接结束程序
> 4. catch 和 finally 语句是可选的，但与try配合时，两者必须有其中一个

catch中常用的异常处理方式：

1. 编写自己的输出语句
2. 调用其父类（Throwable）中的两个方法：
   - printStackTrace()：打印异常的详细信息
   - 【String】getMessage()：获取发生异常的原因



##### throws

```java
权限修饰符 返回值类型 方法名() throws 异常类型1,异常类型2{
    // 方法体中的代码 可能出现编译时异常
}
```

> 注意：
>
> 1. 子类重写父类的方法时，子类抛出的异常类型不能比父类所抛出的异常类型大（仅考虑编译时异常，与运行时异常无关（不受编译器检查））
>
>    ```java
>    Father f = new Son();
>    try{
>        f.test();
>    }catch(FileNotFoundException e){}
>    // 编译看声明，此刻父类原本抛出的异常类型较小，可以捕获
>    // 运行时，运行的是子类的方法，抛出的异常较大，无法被捕获
>    
>    class Father{
>        public void test() throws FileNotFoundException{}
>    }
>    class Son extends Father{
>        public void test() throws IOException{}
>    }
>    ```



##### throw

```java
public void test(){
    throw new 异常类型("错误信息");
    // 在方法体中手动抛出异常
}
```

> 若异常类型为运行时异常，可以不做任何处理。
>
> 若异常类型为编译时异常，则需要将异常向上抛出（throws）或进行处理（try-catch）



##### 自定义异常类

步骤：

1. 继承于现有的异常体系（常继承于：RuntimeException、Exception）
2. 提供几个重载的构造器
3. 提供一个全局常量：static final long serialVersionUID = xxxxL;

> 自定义异常类的目的：手动抛出异常时，能够见名知意



##### 使用场景分析

try-catch：

1. 若出现资源流等，则考虑使用
2. 若父类中没有抛出异常，子类重写父类方法，且子类中出现了异常，此刻无法抛出异常，就要用try-catch进行处理

throws：

1. 若方法a中，调用了b、c、d方法，且b调用了c方法，c调用了d方法（依次调用关系），则考虑throws，让a方法进行try-catch

   > 因为try-catch后，后续代码会被正常执行，因此用throws结束掉后续代码的执行

throw：

1. 当出现不符合预期的操作时，若系统没有生成对应的异常，则需要手动生成并抛出异常对象





#### 面试题

```java
int res = test("a");
System.out.println(res);
// 情况一：先输出test，再输出1
// 情况二：先输出test，再输出0

// 情况一：
public int test(String str){
    try{
        Integer.parseInt(str);	// 1、出现异常
        return 1;	// 2、返回前先调用finally
    }catch(Expection e){
        return -1;
    }finally{
        System.out.println("test");	// 3、输出语句，然后执行操作2的返回
    }
}

// 情况二：
public int test(String str){
    try{
        Integer.parseInt(str);	// 1、出现异常
        return 1;	// 2、返回前先调用finally
    }catch(Expection e){
        return -1;
    }finally{
        System.out.println("test");	// 3、输出语句
        return 0;	// 此处有return了，操作2作废，直接在此处返回
    }
}
```

```java
int res = test(10);
System.out.println(res);	// 先输出test，再输出10

// 栈帧中，细分为多个部分，其中有：操作栈 以及 局部变量表
public int test(int num){
// 方法入栈帧后，局部变量入的是栈帧中的局部变量表
    
    try{
        return num;	// 1、返回前先调用finally
// return前，将局部变量表中的数值输入到操作栈中
// return时，返回的是操作栈中的数值
    
    }catch(Expection e){
        return num--;
    }finally{
        System.out.println("test");	// 2、输出语句
        ++num;	// 3、执行++操作，然后执行操作1中的返回
// 此刻的++，是对局部变量表中的数值进行++，而操作栈中的数值没有发生任何改变
// 重新回去调用return时，返回的是操作栈中 当时未返回的数值
    
    }
}
```







### 多线程

#### 知识点

##### 概念

1）程序、进程与线程：

1. 程序：为完成特定任务，用某种语言编写的一组指令的集合（静态的代码）
2. 进程：运行中的程序（操作系统调度和分配资源的最小单位）
3. 线程：进程在运行时，可能具有多条执行路径，每条执行路径对应一个线程（CPU调度和执行的最小单位）

2）线程调度两种方式：

1. 分时调度：所有线程轮流使用CPU，且平均分配每个线程占用CPU的时间
2. 抢占式调度：让优先级较高的线程有较大的概率优先使用CPU

3）并行与并发：

1. 并行：指多个事件在同一时刻发生（同一时刻，多条指令在多个CPU上同时进行）
2. 并发：指多个事件在同一时间段内发生（同一时间段，多条指令在单个CPU上交替执行）





##### 线程构造器

```java
public Thread()
// 调用时，创建一个新的线程对象
// 继承Thread类的方式创建线程时常用
    
public Thread(String name)
// 调用时，创建一个新的线程对象，并指定线程名
   
public Thread(Runnable target)
// 调用时，创建一个新的线程对象，且为其属性target赋值
// 实现Runnable接口的方式创建线程时常用
    
public Thread(Runnable target,String name)
// 调用时，创建一个新的线程对象，并指定线程名，且为其属性target赋值
```





#### 方法

##### 常用

`start()`：启动线程，调用Thread中的run()

`run()`：将线程中要执行的操作声明在run()中

【Thread】（static）`currentThread()`：获取当前执行代码的线程对象

【String】`getName()`：获取指定线程对象的线程名

`setName(String)`：为指定线程设置线程名

（static）`sleep(long)`：执行时，将当前线程睡眠指定毫秒数

（static）`yield()`：执行时，释放当前线程的CPU占用权

`join()`：在线程A中，线程B调用join()，则线程A将进入阻塞状态，则到线程B执行结束

【boolean】`isAlive()`：判断当前线程是否存活





##### 线程通信

> 只能在同步代码块中使用，且是声明于Object类中的方法

`wait()`：当线程执行到此方法时，将会进入阻塞状态。同时，释放对同步监视器的调用（被唤醒时，继续执行后续代码）

`notify()`：当线程执行到此方法时，就会唤醒：因为wait()而进入阻塞状态的、优先级最高的 那一个线程（若wait()的所有线程的优先级都相同，则随机唤醒一个）

`notifyAll()`：当线程执行到此方法时，唤醒所有被wait()的线程





##### 优先级

【int】`getPriority()`：获取线程的优先级别（默认为5）

`setPriority(int)`：设置线程的优先级别（优先级：1~10）

>Thread类内部的三个优先级常量：
>
>1. MAX_PRIORITY：最高优先级（10）
>2. MIN_PRIORITY：最低优先级（1）
>3. NORM_PRIORITY：普通优先级（5）





##### 过时

stop()：强制结束线程的执行，直接进入死亡状态

【void】suspend()、resume()：暂停、启动线程（可能会造成死锁）







#### 创建多线程

##### ① 继承Thread

```java
// 1.创建一个继承于Thread类的子类
class A extends Thread{
	
// 2.在该子类中重写父类的run()
    public void run(){
        
// 3.书写要在另一个线程中执行的代码
        方法体;
    }
}

// 4.创建该子类的对象
A a = new A();

// 5.调用该子类对象的start()
a.start();
```

> 注意：
>
> 1. 直接调用run()不能启动多线程
>
> 2. 子类对象若调用过start()，则不能再重复调用start()，需要重新创建子类对象进行调用
>
> 3. 匿名子类的匿名对象方式：
>
>    ```java
>    new Thread(){
>        public void run(){
>            方法体;
>        }
>    }.start();
>    ```



##### ② 实现Runnable

```java
// 1.创建类实现Runnable接口
class A implements Runnable{
    
// 2.实现接口中的run()
    public void run(){
        
// 3.书写要在另一个线程中执行的代码
        方法体;
    }    
}

// 4.创建实现类的对象
A a = new A();

// 5.创建Thread类的实例，将实现类的对象作为参数传递到Thread类的构造器中
Thread t = new Thread(a);

// 6.通过Thread类的实例，调用start()启动
t.start();
```

> 注意：
>
> 1. Thread类的对象若调用过start()，则不能再调用start()，需要重新创建Thread类的对象
>
> 2. 匿名实现类的匿名对象：
>
>    ```java
>    new Thread(){new Runnable(){
>        public void run(){
>            方法体;
>        }
>    }}.start();
>    ```



##### ③ 实现Callable

```java
// 1.创建类实现Callable接口
class A implements Callable{
    
// 2.实现接口中的call()
    public Object call() throws Exception{
        
// 3.书写要在另一个线程中执行的代码
        方法体;
    }    
}

// 4.创建实现类的对象
A a = new A();

// 5.创建FutureTask类的对象，将实现类对象作为其构造器参数进行创建
FutureTask ft = new FutureTask(a);

// 6.创建Thread类的实例，将FutureTask类的对象作为其构造器参数进行创建
Thread t = new Thread(ft);

// 7.调用Thread类的对象的start()启动线程
t.start();

// 获取返回值参数：通过FutureTask类的对象，调用其get()获取返回值
Object ret = ft.get();
```

> 注意：
>
> 1. 与实现Runnable接口的方式相比，好处：
>    - call()可以有返回值，且Callable使用了泛型，可以指定call()返回值的类型
>    - call()可以抛出异常
> 2. 在主线程中，要获取call()的返回值，若此时的返回值还未返回，则主线程会进入阻塞状态



##### ④ 线程池

```java
// 1.调用Executors类的静态方法，生成ExecutorService类的对象，创建线程池
ExecutorService es = Executors.newFixedThreadPool(线程数);

// 2.执行ExecutorService类的对象的execute()，执行要进行多线程的类的run()
es.execute(new A());
    
class A extends Thread{
    public void run(){}
}
```

> 注意：
>
> 1. 若是实现Callable接口的方式创建多线程，则调用的是`service.submit(Callable c)`





##### 总结

共同点：

1. 启动线程时，调用的都是Thread类中的start()
2. 创建的线程对象，都是Thread类或其子类的实例

>建议使用实现Runnable接口的方式：
>
>1. 避免了单继承的局限性
>2. 更适合处理共享数据的问题
>   - 在实现Runnable接口的实现类中，若有属性值，其属性值在启动多线程时，保持共享数据的方式（多个线程共用唯一一个实现类的对象）
>   - 而继承Thread类的子类（若有属性值），在每次启动线程时，都要重新创建一个子类的对象（虽然属性也可以声明为static来实现共享数据）



注意要点：

1. 观察Thread类的底层源码，发现：`public class Thread implements Runnable{}`，因此可推出：

   - 继承Thread类：其run()是重写父类Thread中的run()，而Thread类再重写接口Runnable中的run()
   - 实现Runnable接口：直接重写Runnable接口中的run()

2. 实现Runnable接口时，将实现类的对象作为形参传递，因此，观察Thread类的构造器：

   - `public Thread(Runnable target){}`构造器中，调用的是`this()`，跳转到`private Thread()`，可发现：`this.target=target;`，继续跳转发现target是Thread类中的属性，类型为Runnable

   - start的作用是：调用Thread类中的run()

     - 继承Thread类的子类直接重写了Thread类中的run()

     - 实现Runnable接口的实现类，在创建Thread类的实例时，将实现类的对象作为形参传递给Thread类的构造器，并完成赋值。

       当调用run()时，观察Thread类的run()源码发现：`if(target!=null){ target.run() }`，因此，调用的也是实现类重写的run()



##### 面试题

```java
class A extends Thread{
    public void run(){
        System.out.println("run A ... ");
    }
}

// 场景一：
class B extends Thread{
    private A a;
    public B(A a){
        this.a = a;
    }
    public void run(){
        System.out.println("run B ...");
    }
}

A a = new A();
a.start();				// run A
new B(a).start();		// run B
// start()方法调用的是当前Thread类中的run()方法
// A重写了run()，B也重写了run()


// 场景二：
class B extends Thread{
    public B(A a){
        super(a);
    }
}

A a = new A();
a.start();				// run A，类A中重写了run()
new B(a).start();		// run A
// 调用类B构造器，将a传递给其父类（Thread类）的构造器（看源码，可知：public Thread(Runnable target){}）
// 调用start()时，会调用run()，而类B中没有重写run()，因此调用的是其父类（Thread类）的run()（看源码，可知：if(target!=null){ target.run() }）
// 因此，调用的就是a的run()
```





#### 线程的生命周期

JDK5.0之前：新建、就绪、阻塞、运行、死亡

1. 新建 --> 就绪：通过start()，调用run()后，进入就绪状态
2. 就绪 --> 运行：获得CPU的占用权后，进入运行状态
3. 运行 --> 就绪：主动 或 通过yield()释放CPU的占用权后，重新进入就绪状态
4. 运行 --> 阻塞：当sleep()、join()、wait()、suspend()或失去同步锁时，进入阻塞状态
5. 阻塞 --> 就绪：当sleep()结束、join()所对应的线程结束、wait()时间结束（或notify()、notifyAll() 唤醒）、resume()开始、获得同步锁 时，从阻塞状态进入就绪状态（无法直接恢复为运行状态）
6. 运行 --> 死亡：当run()执行结束、出现异常、stop()强制结束时，进入死亡状态





#### 解决线程安全问题

##### ① 同步代码块

```java
synchronized(同步监视器){
    // 需要被同步的代码（要准确，不能多也不能少）
}
```

1. 在被synchronized包裹后，若有一个线程在执行该代码，则其他线程必须先等待

2. 同步监视器（锁），可以由任何一个类的对象充当，但所有的线程必须 共用同一个 同步监视器，哪个线程拿到了锁，哪个线程就能执行 需要被同步的代码

   > 在实现Runnable接口的方式创建线程中：常用《this》作为锁
   >
   > - this是指调用当前方法的对象，而在实现Runnable接口的方式中，调用run()的，始终是：作为参数传递给Thread类的构造器的对象
   >
   > 在继承Thread类的方式创建的线程中，常用《当前类.class》作为锁
   >
   > - 同理，T实现Runnable接口的方式中，也可以使用该对象作为锁



##### ② 同步方法

```java
权限修饰符 synchronized 返回值类型 方法名(){
    // 需要被同步的代码
}
```

1. 本质上就是将同步代码块中需要被同步的代码封装到一个方法中

2. 对于非静态的同步方法，默认的同步监视器为《this》

   对于静态的同步方法，默认的同步监视器为《当前类.class》



##### ③ 锁

> JDK5.0新特性

1. 创建Lock的实例，且要保证多个线程共用一个Lock的实例对象

   `ReentrantLock lock = new ReentrantLock();`

   > Lock是一个接口，ReentrantLock是该接口的一个实现类

2. 执行lock()方法，对同步数据进行锁定

   `lock.lock();`

3. 执行unlock()方法，对锁定的同步数据进行解锁

   `lock.unlock();`

举例：（卖票）

```java
new Window().start();
new Window().start();

class Window extends Thread{
    private static int ticket = 100;
    private static Lock lock = new ReentrantLock();	// 必须确保Lock对象唯一
    public void run(){
        while(true){
            try{
                lock.lock();	// 锁定要同步的代码
                if(ticket>0){
                    System.out.println("当前票数为："+ticket);
                    ticket--;
                }else{
                    break;
                }
            }finally{
                lock.unlock();	// 释放锁定
            }
        }
    }
}
```





##### 实例

解决懒汉式线程安全问题：

```java
class A{
    private A(){}
    private static A a = null;
    public static synchronized A getA(){
        if(a==null){
            a=new A();
        }
        return a;
    }
}
```

拓展型优化：

```java
class A{
    private A(){}
    private static volatile A a = null;
// 5.此方式可以提高效率，但可能会造成一个问题：指令重排。添加此关键字解决
    public static A getA(){
        if(a==null){
// 1.线程一进来后，拿到锁 正在进行判断
// 2.线程二进来，等待拿到同步锁
// 3.线程一完成判断，创建类A实例，并返回，线程二做判断，不满足条件，拿到a返回
// 4.此时线程三要进来，判断发现已经有a，不再等待获取同步锁
            synchronized(A.class){
            	if(a==null){
                    a=new A();
                }
        	}
        }
        return a;
    }
}
```

> 指令重排：在创建a对象时，是分为很多步骤的，有可能a对象已经造好，但还未进行初始化，而此刻其他线程判断后发现 a!=null，就会直接返回a对象，但a对象还未初始化。





#### 死锁

不同的线程，占用着对方所需要的同步锁不放，都在等待对方放弃自己的同步监视器，就形成了线程的死锁

```java
Object o1 = new Object();	// 同步锁1
Object o2 = new Object();	// 同步锁2

// 线程1
new Thread(){
    public void run(){
        // 先握住锁1
        synchronized(o1){
            System.out.println("o1-Thread1");
            // 在此处睡眠一会效果更明显
            synchronized(o2){
                System.out.println("o2-Thread1");
            }
        }
    }
}.start();

// 线程2
new Thread(){
    public void run(){
        // 先握住锁2
        synchronized(o2){
            System.out.println("o2-Thread2");
            // 在此处睡眠一会效果更明显
            synchronized(o1){
                System.out.println("o1-Thread2");
            }
        }
    }
}.start();
```





#### 线程通信

> 当一件事情需要多个线程来共同完成，并且是有规律的执行时，那么，这些线程间就需要一些通信机制

通信所涉及到的3个方法：wait()、notify()、notifyAll()（详见：常用方法 -- Object类 -- 线程）

1）注意点：

1. 三个方法的使用，都必须是在同步代码块 或 同步方法中

   > Lock则需要配合Condition实现线程间的通信

2. 三个方法的调用者，都是同步监视器

3. 三个方法都是声明在Object类中的

   > 因为同步监视器可以是任意对象，而三个方法又都是同步监视器调用的

4. wait()的线程被唤醒后，依旧执行后续代码



2）wait()和sleep()间的区别：

1. 相同点：一旦执行，线程都会进入阻塞状态
2. 不同点：
   - 声明位置：
     - wait()声明在Object类中
     - sleep()声明在Thread类中
   - 使用要求：
     - wait()只能使用在同步代码块或同步方法中
     - sleep()可以使用在任意场景
   - 使用时：
     - wait()执行后，会释放同步监视器
     - sleep()执行时，也不会释放同步监视器
   - 结束阻塞方式：
     - wait()在 到达指定时间自动结束 或 被notify()、notifyAll() 唤醒后，结束阻塞
     - sleep() 只能在 到达指定时间后 才能自动结束阻塞





#### 例题

> 生产者与消费者模型

```java
// 易错点：同步监视器的使用

public static void main(String[] args) {
    Clerk clerk = new Clerk();		// 创建店员对象 
    // 确保共用一个店员对象
    Customer c1 = new Customer(clerk);	
    Producer p1 = new Producer(clerk);
    // 创建多线程实现同时生产和消费
    Thread t1 = new Thread(p1, "生产者1");
    Thread t2 = new Thread(c1, "消费者1");
    t1.start();
    t2.start();
}

// 店员
class Clerk {
    private int product;		// 当前产品数量
    public int getProduct() {	// 获取当前产品数量
        return product;
    }
    
    // 生产产品
    public void addProduct() { product++; }
	// 消费产品
    public void minusProduct() { product--; }
}

// 消费者
class Customer extends Thread {
    private Clerk clerk;	// 店员对象
    public Customer(Clerk clerk) {	// 通过构造器获取店员对象
        this.clerk = clerk;
    }
	
    // 多线程执行内容
    public void run() {
        while (true) {
            // 同步监视器必须是同一个!!!
            synchronized (Clerk.class) {
                // 当产品数量大于0个时，就可以进行消费，否则等待生产者生产后，唤醒
                if (clerk.getProduct() > 0) {
                    System.out.println("消费第"+clerk.getProduct()+"个产品");
                    clerk.minusProduct();
                    Clerk.class.notify();	// 是同步监视器来调用notify()
                } else {
                    try {
                        Clerk.class.wait();	// 是同步监视器来调用wait()
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
}

// 生产者
class Producer extends Thread {
    private Clerk clerk;
    public Producer(Clerk clerk) {
        this.clerk = clerk;
    }
    public void run() {
        while (true) {
            synchronized (Clerk.class) {
                if (clerk.getProduct() < 20) {
                    clerk.addProduct();
                    Clerk.class.notifyAll();
                    System.out.println("生产第"+clerk.getProduct()+"个产品");
                } else {
                    try {
                        Clerk.class.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
}
```









### 常用类

#### Object类

> java.lang.Object，任何一个Java类的 直接或间接 父类
>
> - 没有属性
> - 提供一个空参构造器

##### equals

== 与 equals 的区别：

1. `==`：适用于基本数据类型、引用数据类型
   - 基本数据类型：判断两者的数值是否相等
   - 引用数据类型：比较两个变量的地址值是否相同（两个引用是否指向同一片空间）
2. `equals()`：只适用于引用数据类型
   - 自定义类中，若没有重写equals()，则比较的是两个对象的引用地址是否相同
   - 像String、File、Date和包装类等重写了equals()，则比较的是两个对象中的内容是否相同

重写举例：

```java
class User{
    String name;
    int age;
    Student student;	// 自定义类

    public boolean equals(Object obj){
        // 先判断两个对象的地址是否相同，若相同则一定内容相等
        if(this == obj){
            return true;
        }

        // 判断要比较的对象是否是该类的实例
        if (obj instanceof User){
            User u = (User)obj;
            return this.name.equals(u.name) && this.age == u.age && this.student.equals(u.student.equals);
// 对比内容是否相同：String已重写equals()，数值类型直接==判断，自定义的类也还需重写equals()
        }

        return false;
    }
}
```

> 特别：
>
> ```java
> String s1 = "test";
> String s2 = "test";
> s1 == s2; // true
> ```
>
> ```java
> String s1 = new String("test");
> String s2 = new String("test");
> s1.equals(s2);	// true
> s1 == s2;	// false
> ```





##### toString

> 在打印对象的引用变量时，本质上是调用了 对象.toString()

1. 自定义的类，在没有重写toString()的情况下，返回的是当前对象的地址值
2. 像String、File、Date或包装类等，都重写了Object类的toString()，返回的是对象的内容

重写举例：

```java
class User{
    String name;
    int age;
    public String toString(){
        return "User{ name = "+ name +" , age = "+ age +" }";
    }
}
```





##### 线程相关

> 只能在同步代码块中使用

`wait()`：当线程执行到此方法时，将会进入阻塞状态。同时，释放对同步监视器的调用（被唤醒时，继续执行后续代码）

`notify()`：当线程执行到此方法时，就会唤醒：因为wait()而进入阻塞状态的、优先级最高的 那一个线程（若wait()的所有线程的优先级都相同，则随机唤醒一个）

`notifyAll()`：当线程执行到此方法时，唤醒所有被wait()的线程





##### 补充

【Object】`clone()`：在堆中，对该对象进行复制，并返回复制后的空间的首地址

```java
Animal a = new Animul();
Animal b = a.clone();
```



`finalize()`：当GC要回收对象时，会先调用此方法（此方法已过时：可能会导致内部出现循环调用，使得对象无法被回收）







#### Arrays类

> 位于 `java.util.Arrays` 包下

##### 对比

【boolean】（static）`equals(数组A,数组B)`：判断 数组A和数组B中的元素是否相等

> ```java
> int[] a = {1,2,3,4,5};
> int[] b = {1,2,3,5,4};
> boolean isEqual = Array.equals(a,b);	// false
> // 由于数组中的元素在内存中是 依次紧密有序排列的，因此元素值相同、但位置不同也为false
> ```





##### 获取

【String】（static）`toString(数组)`：返回一个 数组中的所有元素 的字符串

> ```java
> int[][] a = new int[][]{ {1,2},{2,3} };
> String res = Arrays.toString(a);	// 返回2个地址值
> String res = Arrays.toString(a[0]);	// 返回[1, 2]
> // 说明 toString 获取的是 指定的数组中，所指向的空间中存放的内容
> ```



【List】（static）`asList(数组)`：将数组中的所有元素 封装到集合中（数组 --> 集合）

```java
Integer[] arr1 = new Integet[]{1,2,3};
List l1 = Arrays.asList(arr1);
System.out.println(l1.size());		//  3

int[] arr2 = new int[]{1,2,3};
List l2 = Arrays.asList(arr2);
System.out.println(l2.size());		// 1

// 观察源码发现：asList()中传递的实质上是Object类型的可变形参（也就是数组）
// Integet[]中的元素会自动装箱为包装类对象，而int[]中的元素为基本数据类型，因此只有int[]这一个对象
// 所以，asList(arr1)存放的是arr1中的元素，而asList(arr2)存放的是arr2数组的地址
```





##### 填充

（static）`fill(数组,填充值)`：将指定类型的值 填充（替换原有的值）到 对应类型的数组中





##### 排序

（static）`sort(数组)`：（使用快速排序算法）对指定的数组进行 从小到大的排序

（static）`sort(数组,Comparator)`：（按照Comparator接口的实现类 重写的Compare方法 中的规则）对指定的数组进行排序



##### 查找

【int】（static）`binarySearch(数组,查找值)`：使用二分法 查找指定数组下 某个值的索引，并返回，未找到时，返回一个负数（使用前提：数组是有序的）







#### String类

##### 知识点

###### 声明

> jdk8.0

public final class String implements java.io.Serializable,Comparable,CharSequence

- final：String类不可被继承
- Serializable：可序列化接口，凡是实现了该接口，则其类的对象就可以通过网络或本地流进行数据传输
- Comparable：凡是实现了该接口，其类的对象都可以比较大小
- CharSequence：字符标志



###### 内部属性

（jdk8.0）private final char value[]

（jdk9.0）private final byte[] value

- 是存储字符串数据的容器，final修饰后表示：value数组一旦初始化，其值（地址）就不能发生改变
- jdk9.0中，将char[] 换成 byte[]，以便节省内存空间



###### 字符串常量池

- 字符串常量都存储在字符串常量池中

- 字符串常量池中不能存放两个相同的字符串常量

  ```java
  String s1 = "hello";
  String s2 = "hello";
  System.out.println( s1 == s2 );		// true
  ```

- 字符串常量池在不同的jdk版本中，所在的位置不同：

  - jdk7.0之前：存放在方法区中
  - jdk7.0及之后：存放在堆中（便于GC的频繁回收）



###### 不可变性

- 当对现有字符串变量进行重新赋值时，需要在字符串常量池中，重新指定新的位置，不会在原有的位置上进行修改

  ```java
  String s1 = "hello";
  String s2 = "hi";
  System.out.println(s1);		// hello
  ```

- 当对现有字符串变量进行拼接、replace()替换时，需要在堆中开辟新的空间进行操作，不会在原有的位置上进行修改

  ```java
  String s1 = "hello";
  String s2 = "hello";
  s2 += "world";
  System.out.println(s1);		// hello
  System.out.println(s2);		// helloworld
  
  s3 = s2.replace('l','w');
  System.out.println(s2);		// hello
  System.out.println(s3);		// hewwo
  // replace()底层也是用new的方式创建的新字符串
  ```



###### 构造器

1. public String()：创建一个数组长度为0、内容为""的String对象
2. public String(String org)：创建一个String对象，且其内容为org
3. public String(char[] value)：将char[]中的内容合并，赋值给新创建的String对象
4. public String(byte[] value)：使用平台默认的字符集，解码后，将byte[]中的内容合并，赋值给新创建的String对象





###### 转换

> Object --> String：toString()

1. String与基本数据类型间的转换：（前者转换为后者，就从后者里找方法）

   - String --> 基本数据类型：调用包装类中的parseXxx(String)方法

     ```java
     String s = "123";
     int i = Integer.parseInt(s);
     ```

   - 基本数据类型 --> String：使用【+】进行拼接 或 调用String的valueOf(基本数据类型)

     ```java
     int i = 10;
     String s1 = i + "";
     String s2 = String.valueOf(i);
     ```

2. String与char[]间的转换：

   - String --> char[]：调用String的toCharArray()

     ```java
     String s = "hello";
     char[] c = s.toCharArray();
     ```

   - char[] --> String：调用String的构造器

     ```java
     char[] c = new char[]{'a','b','c'};
     String s = new String(c);
     ```

3. String与byte[]间的转换：

   - String --> byte[]：调用String的getBytes()（平台默认字符集）、getBytes("字符集")

     ```java
     String s = "abc";
     byte[] b1 = s.getBytes();
     byte[] b2 = s.getBytes("gbk");
     ```

   - byte[] --> String：调用String的构造器

     ```java
     byte[] b = {97,98,99};
     String s1 = new String(b);		// 使用平台默认的字符集进行解码
     String s2 = new String(b,"utf-8");		// 使用UTF-8字符集进行解码
     ```

   > 1. UTF-8字符集中，汉字占3个字节，字母1个字节（因为兼容ASCII）
   >
   >    GBK字符集中，汉字占2个字节，字母1个字节（因为兼容ASCII）
   >
   > 2. 编码与解码：
   >
   >    - 编码：String --> 字节或字节数组（看得懂 --> 看不懂，如：保存）
   >    - 解码：字节或字节数组 --> String（看不懂 --> 看得懂，如：读取）
   >
   >    编码与解码使用的字符集不一致时，会导致乱码

   





##### 内存解析

###### 创建实例

String实例化的两种方式：

1. String s1 = "hello";

2. String s2 = new String("hello");

   > new的方式在内存中创建了两个内存对象：
   >
   > 1. 在堆中new的空间，其地址赋值给变量s2
   >
   > 2. 若在字符串常量池中没有【hello】这个字符串，则在字符串常量池中，会开辟一块新空间，此时堆空间中value属性的值即为该空间的地址
   >
   >    （若已存在【hello】这个字符串，则根据：字符串常量池中不存在两个相同的字面量。此刻value属性的值即为【hello】字符串所在的空间的地址）

```java
class Person{
    private String name;
}

public static void main(String[] args){
// main方法入栈，args变量入栈帧
    
    Person p1 = new Person();
    Person p2 = new Person();
    // 在堆中开辟新空间P和PP，将该空间的首地址赋值给变量p1和p2
    
    p1.name = "Tom";
    p2.name = "Tom";
// 由于name属性是字符串，且字面量相同，因此，堆中空间P和空间PP的属性name所存放的是：指向 字符串常量池中 开辟的 同一块地址N，又由于字符串底层使用数组进行存储，因此该地址N指向的是：字符串常量池中，另一块 存放字符的数组 的地址
    
    p1.name = "Jerry";
// 通过p1存放的地址，找到空间P，由于现在要修改其name属性的字面量，而根据String的不可变性，此时会在字符串常量池中重新开辟一块新空间，其name属性值存放的是该空间的地址NN。又由于字符串底层使用数组进行存储，进而该地址NN指向的是：字符串常量池中，该数组的地址
}
```





###### 连接操作

1. 常量 + 常量：结果依旧是存储在字符串常量池中（常量也可以是final修饰的变量）

   ```java
   String s1 = "helloworld";
   String s2 = "hello" + "world";
   System.out.println( s1 == s2 );		// true
   // 常量 + 常量
   
   final String s3 = "hello";
   String s4 = s3 + "world";
   System.out.println( s1 == s4 );		// true
   // final变量（常量） + 常量
   ```

2. 常量 + 变量 或 变量 + 变量：都会以new的方式，在堆中开辟新的空间

   > 当涉及到变量使用 + 时，观察底层源码发现：先是创建了StringBuilder类的实例，再调用append()添加字符串，最后调用toString()，在方法内新new了一个String

   ```java
   String s1 = "helloworld";
   String s2 = "hello";
   String s3 = s2 + "world";
   System.out.println( s1 == s3 );		// false
   // 变量 + 常量
   
   String s4 = "world";
   String s5 = s2 + s4;
   System.out.println( s1 == s5 );		// false
   // 变量 + 变量
   ```

3. 调用字符串的intern()：返回的是在字符串常量池中的字面量

   ```java
   String s1 = "hello";
   String s2 = s1.intern();
   System.out.println( s1 == s2 );		// true
   ```

4. 调用字符串的concat(String s)：返回的都是一个新new的对象的值

   ```java
   String s1 = "hello";
   String s2 = "world";
   String s3 = "helloworld";
   String s4 = s1.concat(s2);
   String s5 = "hello".concat("world");
   System.out.println( s3 == s4 );		// false
   System.out.println( s3 == s5 );		// false
   ```








##### StringBuffer

> StringBuffer与StringBuilder的区别：使用的方法有无使用synchronized修饰



###### 对比String

1. 相同点：底层都是使用char[]（JDK8.0及之前）或byte[]（JDK9.0及之后）
2. 不同点：String是不可变的字符序列，而StringBuffer 和 StringBuilder是可变的字符序列

StringBuffer：JDK1.0声明，线程安全（方法使用 synchronized 修饰）

StringBuilder：JDK5.0声明，线程不安全



可变性与不可变性分析：

1. String的不可变性：底层使用的value[]，使用了final修饰

   > String s = new String();	--->	char[] value = new char[0];
   >
   > String s = new String("abc");	--->	char[] value = new char[]{ 'a' , 'b' , 'c' };

2. StringBuffer 和 StringBuilder的不可变性：继承的父类AbstractStringBuilder中，属性value[] 没有使用final修饰，且存在一个用于记录 已存储的字符的个数的属性：int count;

   > StringBuilder sb = new StringBuilder();	--->	char[] value = new char[16];
   >
   > StringBuilder sb = new StringBuilder("abc");	--->	char[] value = new char[16 + "abc".length() ];
   >
   > - 创建时，StringBuilder数组的默认长度为：当前字符串长度 + 16



特点：

1. 扩容时，会创建新的value[]，默认将数组长度扩容为原来的【2倍+2】，并将原来value[]中的数据复制到新的数组中（可变性的体现）
2. 若开方中大体确定了操作的字符个数，则可以使用构造器public StringBuilder(int capacity)明确指定创建的value[]长度





###### 面试题

```java
String str = null;
StringBuffer sb1 = new StringBuffer();
sb1.append(str);

System.out.println(sb1.length());	// 4
// 观察源码发现，会先判断str是否为null，若为null，则拼接上'n'、'u'、'l'、'l'

StringBuffer sb2 = new StringBuffer(str);
System.out.println(sb2);		// 空指针异常
// 调用构造器时，在获取str.length()时出现空指针异常
```







##### 方法

###### String

1）常用：

【boolean】`isEmpty()`：判断字符串是否为空（底层判断的是数组value的长度是否为0）

【int】`length()`：返回字符串的长度

【String】`concat(String)`：拼接字符串后，返回新new的字符串

【String】`trim()`：去掉字符串前后的空白后，返回一个新new的字符串

【String】`intern()`：返回常量池中的值（共享字符串常量池）



【boolean】`equals(Object)`：比较两个字符串是否相等，区分大小写（一个一个字符的进行对比）

【boolean】`equalsIgnoreCase(Object)`：比较两个字符串是否相等，不区分大小写



【int】`compareTo(String)`：比较字符串大小，区分大小写，（以前者减去后者进行比较）返回正数表示前者大，负数表示前者小，0表示相等（按字符集编码值比较大小）

【int】`compareToIgnoreCase(String)`：比较字符串大小，不区分大小写



【String】`toLowerCase()`：将字符串中大写字母转换为小写字母后，返回一个新new的字符串

【String】`toUpperCase()`：将字符串中小写字母转换为大写字母





2）查找：

【boolean】`contains(String)`：查找字符串中是否包含指定字符串



【int】`indexOf(String)`：在字符串中，从前往后找指定的字符串，若找到，返回 指定的字符串 在 字符串中 第一次出现的位置的下标，若未找到，返回-1

【int】`indexOf(String,int)`：在字符串中，从指定下标处开始，从前往后查找指定字符串

【int】`lastIndexOf(String)`：在字符串中，从后往前找指定字符串，若找到，返回 指定的字符串 在 字符串中 最后一次 出现的位置的下标，若未找到，返回-1

【int】`lastIndexOf(String,int)`：在字符串中，从指定下标处的后一个开始，从后往前找指定字符串





3）获取：

【char】`charAt(int)`：返回指定下标处的字符（若下标超出，则抛异常）



【String】`substring(int)`：返回从指定下标处开始，截取到末尾的一个新new的字符串

【String】`substring(int beginIndex,int endIndex)`：返回从指定下标处开始，到指定下标前一个结束，即：[a,b)，中间的所有字符串（若下标超出，则抛异常）





4）转换：

【char】`toCharArray()`：将字符串转换为字符数组，并返回



【String】（static）`valueOf(char[])`、

【String】（static）`copyValueOf(char[])`：将字符数组转换为字符串，返回一个新new的字符串

【String】（static）`valueOf(char[],int beginIndex,int count)`、

【String】（static）`copyValueOf(char[],int beginIndex,int count)`：将字符数组中，指定下标开始，取指定个数，将这些字符转换为字符串并返回（新new的字符串）





5）判断：

【boolean】`startsWith(String)`：判断字符串 是否以指定字符串开头

【boolean】`startsWith(String,int)`：判断字符串中，从指定下标开始，是否以指定字符串开头

【boolean】`endsWith(String)`：判断字符串 是否以指定字符串结尾





6）替换：

【String】`replace(char oldChar,char newChar)`：将字符串中，所有的某个字符替换为指定的字符，返回一个新new的字符串

【String】`replace(String oldString,String newString)`：将指定字符串中，所有的某个字符串替换为指定的字符串

【String】`replaceAll(String regex,String replacement)`：用正则表达式的方式替换字符串中所有满足条件的

【String】`replaceFirst(String regex,String replacement)`：用正则表达式的方式替换字符串中第一个满足条件的







###### StringBuffer

> StringBuffer 与 StringBuilder 中的方法是一样的，仅是否有synchronized修饰的区别

1）增：

【StringBuffer】`append(xxx)`：在原先的StringBuffer对象上，对其内的字符串进行追加（可追加多种数据类型），返回该对象的地址（可变性）



2）删：

【StringBuffer】`delete(int start,int end)`：删除指定下标开始，到指定下标的前一个，即：[ start , end )，中间的所有字符

【StringBuffer】`deleteCharAt(int index)`：删除指定下标处的字符



3）改：

【StringBuffer】`replace(int start,int end,String str)`：将指定下标开始，到指定下标的前一个，即：[ start , end )，中间所有的字符，替换为指定的字符串

`setCharAt(int,char)`：将指定下标处的字符替换为指定字符



4）查：

【char】`charAt(int)`：获取指定下标处的字符



5）插：

【StringBuffer】`insert(int,xxx)`：以字符串的方式，在指定下标处的前面插入数据（可多种数据类型）



6）求长度：

【int】`length()`：获取存储的字符数组的长度



7）反转：

【StringBuffer】`reverse()`：对存储的内容进行反转



8）补充：

【int】`indexOf(String)`：在当前字符序列中，查询并返回 指定字符串 在该字符序列中 第一次出现的下标，若没有找到，返回-1

【int】`indexOf(String,int)`：从指定位置开始到末尾，查询并返回  指定字符串 在该字符序列中 第一次出现的下标

【int】`lastIndexOf(String)`：查询并返回 指定字符串 在该字符序列中 最后一次出现的下标

【int】`lastIndexOf(String,int)`：从指定位置的后一个开始到开头，查询并返回 指定字符串 在该字符序列中 最后一次出现的下标



【String】`substring(int)`：截取从指定下标处开始到末尾间 所有的字符

【String】`substring(int start,int end)`：截取从指定下标处开始，到指定下标处的前一个，之间所有的字符



【String】`toString()`：将字符序列中的数据以字符串的方式返回



【String】`setLength(int)`：设置当前字符序列的长度为指定的长度（即：直接修改count值（能读取的字符的数量），而非修改 value[] 的长度）









#### Date类

MySQL与Java中的日期时间类处理中，可直接转换的有：

1. date、year --> java.sql.Date
2. time --> java.sql.Time
3. datetime、timestamp --> java.sql.Timestamp（时区不符）
4. datetime --> java.time.LocalDateTime





##### JDK8.0之前

###### Date

> java.sql.Date 是 java.util.Date 的子类

两个构造器和两个方法：

1. 构造器：
   - 【java.util.Date独有】Date()：创建一个基于当前系统时间的Date的实例
   - Date(long)：创建一个基于指定时间戳的Date的实例
2. 方法：
   - toString()：返回字符串格式的日期时间
   - getTime()：返回该Date实例日期的时间戳



将一个 java.util.Date 实例转换为 java.sql.Date 实例：（利用时间戳）

```java
import java.util.Date;

Date date = new Date();
java.sql.Date sDate = new java.sql.Date(date.getTime());
```





###### SimpleDateFormat

用指定样式的日期格式，对Date类型的指定日期时间进行格式化或解析

- 格式化：日期 --> 字符串
  - 调用方法：【string】`format(Date)`
- 解析：字符串 --> 日期
  - 调用方法：【Date】`parse(String)`

> 要解析的日期格式字符串，必须与原本的日期格式相同

使用方式：

```java
// 1.在创建SimpleDateFormat类的实例时，可指定日期的格式
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
// yyyy年MM月dd日HH时mm分ss秒

Date date = new Date();

// 2.使用SimpleDateFormat类的实例的format(Date)方法，将Date类型日期-->日期格式字符串
String sdate = sdf.format(date);
System.out.println(sdate);		// 2023-04-01 12:41:30

// 3.使用SimpleDateFormat类的实例的parse(String)方法，将日期格式字符串-->Date类型日期
Date d = sdf.parse("2023-04-01 12:41:30");
System.out.println(d);
```



将指定格式的日期时间存入数据库：

```java
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
// 指定当前日期的格式

java.util.Date d = sdf.parse("2023-4-1");
// 将当前日期转换为java.util.Date类型

java.sql.Date date = new java.sql.Date(d.getTime());
// 获取java.util.Date类型的时间戳，作为java.sql.Date类型的参数，创建java.sql.Date类的对象
```







###### Calendar

> 日历类

获取Calendar类的实例：

- Calendar是一个抽象类，因此需要通过创建子类的实例。可以直接通过`Calendar.getInstance()`方式获取（返回的就是子类的实例对象）



常用属性：

1. （static）DAY_OF_MONTH：这个月的第几天
2. （static）DAY_OF_YEAR：这一年的第几天
3. （static）DAY_OF_WEEK：这一周的第几天



常用方法：

1. 【Calendar】`getInstance()`：获取Calendar类的子类的实例
2. 【int】`get(int)`：通过属性获取当前指定的日期
3. `set(int field,int)`：通过属性获取当前指定的日期，并修改为指定的日期
4. `add(int field,int)`：通过属性获取当前指定的日期，并在当前日期上加上指定时间
5. 【Date】`getTime()`：获取Date类型的当前时间（Calendar-->Date）
6. `setTime(Date)`：用指定的Date类型日期设置当前时间（Date-->Calendar）



使用举例：

```java
Calendar c = Calendar.getInstance();	// 获取实例对象
int i = c.get(Calendar.DAY_OF_MONTH);		// 获取当前时间是这个月的第几天
c.set(Calendar.DAY_OF_YEAR,11);		// 设置当前时间是今年的第11天
c.add(Calendar.DAY_OF_WEEK,2);		// 设置当前时间是这个星期的第（x+2）天
Date d = c.getTime();		// Calendar --> Date
c.setTime(new Date());		// Date --> Calendar
```





##### JDK8.0及之后

> 新版的日期时间类解决的4个问题：
>
> 1. 可变性：像日期时间应该是不可变的（如：Calendar的set()等，是在原来的日期上进行修改）
> 2. 偏移性：Date中过时的构造器中，创建时间时，年份从1900年开始，月份从0月开始
> 3. 格式化：SimpleDateFormat格式化日期时间时，只能操作Date类型，Calendar类型不能操作
> 4. 线程不安全：不能处理闰秒等



###### Instant

> 类似于旧版：Date

两种创建实例的方式：

1. 【Instant】（static）`now()`：创建一个基于当前系统时间（时区为中时区）的Instant类的实例
2. 【Instant】（static）`ofEpochMilli(long)`：创建一个基于指定时间戳的Instant类的实例



两个方法：

1. 【long】`toEpochMilli()`：将当前时间转换为时间戳
2. （补充）【Instant】`atOffset(ZoneOffset.ofHours(int))`：用于设置时区





###### LocalDateTime

> 类似于旧版：Calendar

- LocalDate：只获取日期
- LocalTime：只获取时间
- LocalDateTime：获取日期和时间



（以LocalDateTime为代表）两种创建实例的方式：

1. 【LocalDateTime】（static）`now()`：获取一个基于当前系统时间的LocalDateTime类的实例
2. 【LocalDateTime】（static）`of(int year,int month,int day,int hour,int minute,int second)`：获取一个基于指定时间的LocalDateTime类的实例



（以LocalDateTime为代表）常用方法：

1. 【int】`getXxx()`：通过属性获取当前指定的日期
2. 【LocalDateTime】`withXxx(int)`：通过属性获取当前指定的日期，并修改为指定的日期
3. 【LocalDateTime】`plusXxx(int)`：通过属性获取当前指定的日期，并在当前日期上加上指定时间
4. 【LocalDateTime】`minusXxx(int)`：通过属性获取当前指定的日期，并在当前日期上减去指定时间



使用举例：

```java
LocalDateTime ldt = LocalDateTime.now();	// 获取实例对象
int i = ldt.getDayOfMonth();		// 获取当前时间是这个月的第几天
LocalDateTime ldt1 = ldt.withDayOfYear(10);		// 修改当前时间是这一年的第10天
LocalDateTime ldt2 = ldt.plusYears(5);		// 修改今年是第（x+5）年
LocalDateTime ldt3 = ldt.minusHours(5);		// 修改当前是（x-5）点
```







###### DateTimeFormatter

> 类似于旧版：SimpleDateFormat

用指定样式的日期格式，对LocalDate、LocalTime、LocalDateTime类型的指定日期时间进行格式化或解析

- 格式化：日期 --> 字符串
  - 调用方法：【string】`format(LocalDateTime)`
- 解析：字符串 --> 日期
  - 调用方法：
    1. 【TemporalAccessor】`parse(String)`
    2. 【LocalDateTime】（static）`LocalDateTime.from(TemporalAccessor)`

> 要解析的日期格式字符串，必须与原本的日期格式相同



使用方式：

```java
// 1.在创建DateTimeFormatter类的实例时，指定日期的格式
DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy年MM月dd日 HH:mm:ss");

LocalDateTime ldt = LocalDateTime.now();

// 2.使用DateTimeFormatter类的实例的format(LocalDateTime)方法，将LocalDateTime类型日期-->日期格式字符串
String sdtf = dtf.format(ldt);
System.out.println(sdtf);		// 2023年04月01日 16:52:55

// 3.使用DateTimeFormatter类的实例的parse(String)方法，将日期格式字符串-->LocalDateTime类型日期
TemporalAccessor tmp = dtf.parse("2023年04月01日 12:41:30");
LocalDateTime nldt = LocalDateTime.from(tmp);
System.out.println(nldt);
```













#### 其他类

##### System类

【long】（static）`currentTimeMillis()`：获取当前时间（与1970年1月1日0时0分0秒之间对应的）毫秒数（时间戳）

（static）`gc()`：调用GC进行强制性释放内存（但不一定立即回收）

（static）`exit(int)`：结束程序运行，并在控制台中输出结束码（0表示正常结束，-1表示异常结束）





##### Runtime类

对应一个 Java进程的内存使用 的运行时环境对象，是单例的





##### Math类

【double】（static）`random()`：返回一个 [ 0.0 ,1.0 ) 范围内的 double类型的 随机数

> 获取指定范围内的随机整数：`(int)(Math.random()*(b-a+1)+a)`
>
> ```java
> [60,100] --> (int)(Math.random()*41+60)
> // *0 都为0，为了保证0能获取到60，因此必须 +60
> // 有了 +60，为了能获取到100，因此至少还要 *40（60+40）
> // 又由于是开区间，因此是 *41
> ```



【double】（static）`ceil(double)`：取数值的天花板（取最大的整数）

```java
Math.ceil(3.3);		// 4.0
Math.ceil(-4.4);	// -4.0
```

【double】（static）`floor(double)`：取数值的地板（取最小的整数）

```java
Math.floor(8.8);	// 8.0
Math.floor(-3.6);	// -4.0
```

【long】（static）`round(double)`：四舍五入

```java
Math.round(5.5);	// 6
Math.round(5.4);	// 5
Math.round(-12.3);	// -12
Math.round(-12.6);	// -13
Math.round(-12.5);	// -12，中间值，取最大
// 技巧：floor(x+0.5)
```



【基本数据类型】（static）`sqrt(double)`：对数值进行开方

【double】（static）`PI`：Math类的属性，用于获取π的值





##### Random类

【int】`nextInt(int)`：获取一个 [ 0 , x ) 之间的一个随机整数

```java
Random random = new Random();
int i = (randomo.nextInt(100))+1;
// 获取1~100间的一个随机整数
```





##### UUID类

> 可用于生成：基本上全球唯一的32位字符串

【UUID】（static）`randomUUID()`：生成一个UUID对象（UUID v4）

【String】`toString()`：将UUID对象转换为字符串





##### BigDecimal类

BigDecimal：可以表示任意精度的浮点数

> 内部是将其转换为字符串后进行运算

```java
// 1.将需要进行运算的值转换为BigDecimal对象
double d1,d2 = 0.0;
BigDecimal b1 = new BigDecimal("" + d1);
BigDecimal b2 = new BigDecimal("" + d2);

// 2.调用BigDecimal对象内部的xxx(BigDecimal)方法，让该BigDecimal与另一个BigDecimal进行xxx运算
BigDecimal h = b1.add(b2);			// 求和
BigDecimal j = b1.multiply(b2);		// 求积

// 3.调用BigDecimal对象内部的xxxValue()方法，将该BigDecimal的值转换为对应的类型的值
int res1 = h.intValue();
double res2 = j.doubleValue();
```





##### BigInteger类

BigInteger：可以表示任意长度的整数









### 集合

#### 知识点

##### 对比数组

1. 数组一旦初始化，其长度就不可变了
2. 数组存储的元素是有序的、可重复的，对于存储无序、不可重复的数据的场景无能为力
3. 数组中，对元素进行删除、插入等操作时，性能较差
4. （优点）数组存储的数据类型是确定的，若非此类型的数据，不能添加进数组中（必要时也可声明为 Object[] 存储多种类型数据）
5. （优点）数组存储的元素类型，可以是基本数据类型，也可以是引用数据类型





##### 体系框架

java.util.Collection（接口）：存储一个一个的数据

​	|----- List（子接口）：存储有序的、可重复的数据（"动态"数组）

​			|----- ArrayList：主要实现类，线程不安全，底层使用Object[]数组存储

​			|----- LinkedList：底层使用双向链表方式存储

​			|----- Vector：古老实现类，线程安全，底层使用Object[]数组存储

​	|----- Set（子接口）：存储无序的、不可重复的数据

​			|----- HashSet：主要实现类，底层使用HashMap（数组+单向链表+红黑树）存储

​					|----- LinkedHashSet（子类）：底层使用 数组+单向链表+红黑树+双向链表 存储

​			|----- TreeSet：底层使用红黑树存储



java.util.Map（接口）：存储一对一对的数据

​	|----- HashMap：主要实现类，线程不安全，底层使用 数组+单向链表+红黑树 的结构存储

​			|----- LinkedHashMap（子类）：底层使用 数组+单向链表+红黑树+双向链表 的结构存储

​	|----- TreeMap：底层使用 红黑树 存储

​	|----- Hashtable：古老实现类，线程安全，底层使用 数组+单向链表 的结构存储

​			|----- Properties（子类）







##### 底层源码

###### ArrayList

- 实现了List接口，存储有序的、可重复的数据，线程不安全

- 底层使用Object[]数组存储

- 在进行查找或添加（尾部添加）时，效率高（时间复杂度为O(1)）

  在进行删除或插入时，效率低（时间复杂度为O(n)）

- 在大体知道要存放的数据量时，可以使用`new ArrayList(int capacity)`的方式创建集合，此时底层创建的数组的长度即为capacity的值，可以有效避免频繁扩容、复制数组数据

1）JDK7.0版本：

```java
ArrayList<String> l = new ArrayList<>();
// 执行此代码时，底层会初始化数组，长度为10
// Object[] elementData = new Object[10]

l.add("AA");		// elementData[0] = "AA";
l.add("BB");		// elementData[1] = "BB";

// 当要添加第11个元素时，底层elementData数组已满，则会进行扩容
// 默认会扩容为原来长度的1.5倍，并将原数组中的元素复制到新数组中
```



2）JDK8.0版本：

```java
ArrayList<String> l = new ArrayList<>();
// 执行此代码时，底层会初始化数组，长度为0
// Object[] elementData = new Object[]{}

list.add("AA");
// 首次添加元素时，会初始化数组，长度为10：elementData = new Object[10];
// 再添加元素elementData[0] = "AA";
list.add("BB");		// elementData[1] = "BB";

// 当要添加第11个元素时，底层elementData数组已满，则会进行扩容
// 默认会扩容为原来长度的1.5倍，并将原数组中的元素复制到新数组中
```







###### Vector

- 实现了List接口，存储有序的、可重复的数据，线程安全
- 底层使用Object[]数组存储
- 开发中已基本不用

```java
Vector v = new Vector();
// 执行此代码时，底层会初始化数组，长度为10
// Object[] elementData = new Object[10];

v.add("AA");		// elementData[0] = "AA";
v.add("BB");		// elementData[1] = "BB";

// 当要添加第11个元素时，底层elementData数组已满，则会进行扩容
// 默认会扩容为原来长度的2倍，并将原数组中的元素复制到新数组中
```







###### LinkedList

- 实现了List接口，存储有序的、可重复的数据，线程不安全

- 底层使用双向链表存储

- 在进行查找或添加（尾部添加（无记录最后一个元素））时，效率低（时间复杂度为O(n)）

  在进行删除或插入时，效率高（时间复杂度为O(1)）

> LinkedList类中有两个属性：Node first、Node last，存放链表的第一个元素和最后一个元素

```java
LinkedList<String> l = new LinkedList<>();
l.add("AA");		
// 先将AA封装到一个Node对象中，在将first、last属性指向该Node对象
l.add("BB");
// 先将BB封装到一个Node对象中，让此Node对象（BB）指向上一个Node对象（AA）
// 再让last属性指向该Node对象，然后让上一个Node对象（AA）执行此Node对象（BB）
```







###### HashMap

- HashMap中的常量：
  1. DEFAULT_INITIAL_CAPACITY = 1 << 4（默认的初始容量）
  2. MAXIMUM_CAPACITY = 1 << 30（最大容量）
  3. DEFAULT_LOAD_FACTOR = 0.75f（默认加载因子）
  4. TREEIFY_THRESHOLD = 8（默认树化的阈值，当链表长度达到该值时，考虑树化）
  5. UNTREEIFY_THRESHOLD = 6（默认反树化阈值，当树中节点的个数为该值时，考虑转化为链表）
  6. MIN_TREEIFY_CAPACITY = 64（最小树化的容量，当同时满足树化容量和树化阈值时，才进行树化操作）
  7. Node<K,V> table（数组）
  8. size（记录有效映射关系的键值对数，也是Entry对象的个数）
  9. threshold（当size达到该阈值时，考虑扩容）
  10. loadFactor（加载因子，影响扩容的频率）

1）jdk7.0

````java
HashMap<String,Integer> map = new HashMap<>();
// 创建对象时，底层会初始化数组，默认长度为16：Entry[] table = new Entry[16];

map.put("AA",78);		// 将AA和78封装到一个Entry对象中，并可能添加到table数组中
````

添加流程：

1. 先调用 key1 所在的类的 hashCode() ，计算 key1 的 哈希值1 ，此 哈希值1 经过 hash() 之后，得到 哈希值2

2. 哈希值2 经过 indexFor() 之后，就能确定 (key1,value1) 在数组table中的存放位置 索引i

   ① 若此 索引i 上没有元素，则 (key1,value1) 添加成功【情况1】

   ② 若此 索引i 上有元素 (key0,value0) ，则比较 key0 和 key1 两者间的哈希值2（哈希冲突）

   - 若 key0 和 key1 两者的 哈希值2 不相同，则 (key1,value1) 添加成功【情况2】
   - 若 key0 和 key1 两者的 哈希值2 相同，则 调用 key1.equals(key0)
     - 若 equals() 返回false，则 (key1,value1) 添加成功【情况3】
     - 若 equals() 返回true，则认为 key0 和key1 是相同的，则 value1 会 替换掉 value0

情况1添加成功后：将 (key1,value1) 存放到数组中 索引i 的位置上

情况2/3添加成功后：将 (key1,value1) 与 (key0,value0) 构成单向链表，(key1,value1) 指向 (key0,value0)（头插）



扩容：当元素的个数达到临界值（数组长度*加载因子（默认0.75），默认为12）时，就会考虑扩容

- 默认扩容为原来的2倍
- 要添加的值存放在数组中，且数组在该下标处的值为null时，就先不扩容，直接将值添加进数组



源码解读：

① 创建实例时：

```java
public HashMap(){		// 构造器
 this(DEFAULT_INITIAL_CAPACITY,DEFAULT_LOAD_FACTOR);//默认数组长度16，加载因子0.75
}

// 调用空参构造器时，调用此构造器
public HashMap(int initialCapacity,float loadFactor){
    int capacity = 1;
    // 通过循环，得到capacity的最终值（Entry数组的长度），此时capacity一定是2的整数倍
    // 取整数倍是为了后面：决定存放在数组时的索引下标范围
    while(capacity < initialCapacity)
        capacity << = 1;
    this.loadFactor = loadFactor;		// 确定属性loadFactor的值（加载因子的值）
    threshold=(int)Math.min(capacity*loadFactor,MAXIMUM_CAPACITY+1);//确定临界值
    table = new Entry[capacity];		// 初始化数组，长度为capacity
}
```

② put(key,value) 时：

```java
public V put(K key,V value){
    // HashMap中，允许添加key为null的值，将此(key,value)存放到table数组索引0的位置
    if(key == null)
        return putForNullKey(value);
    
    // 将key传入hash()，内部使用了key的哈希值1。执行完此方法后，返回哈希值2
    int hash = hash(key);
    
    // 确定当前key，value在数组中的存放位置i
    int i = indexFor(hash,table.length);
    
// 若table数组中，该索引下标处是null，则略过该循环，直接进行添加
// 若该索引下标处不是null，则判断 哈希值2 是否相等、equals是否相等，相等时，进行修改，否则遍历完循环，进行添加操作
    for (Entry<K,V> e = table[i] ; e != null ; e = e.next){
        Object k;
        if(e.hash == hash && ((k = e.key) == key || key.equals(k))){
            V oldValue = e.value;
            e.value = value;
            e.recordAccess(this);
            return oldValue;
        }
    }
    
    // 将key、value封装为一个Entry对象，在将此对象保存在索引i的位置处
    addEntry(hash,key,value,i);		
    return null;		// 添加操作的话，返回值是null
}

// 计算哈希值2
final int hash(Object k){
    h ^= k.hashCode();		// 使用并获取了key的哈希值1
    h^=(h>>>20)^(h>>>12);
    return h^(h>>>7)^(h>>>4);		// 返回哈希值2
}

// 判断是否扩容，并创建Entry对象
void addEntry(int hash,K key,V value,int bucketIndex){
    // 判断是否扩容（当前存放数量大于临界值，并且当前要添加的位置的数组下标处不是null）
    if((size>=threshold)&&(null!=table[bucketIndex])){
        resize(2*table.length);		// 默认扩容为原来容量的2倍
        hash=(null!=key)?hash(key):0;	// 对 哈希值2 进行进一步计算后保存
        bucketIndex = indexFor(hash,table.length);	// 决定存放在数组中的下标
    }
    createEntry(hash,key,value,bucketIndex);	// 存放为链表
}

// 用于计算存放在数组的哪个下标处
static int indexFor(int h,int length){
    return h & (length-1);  //只要是2的次幂，&1后的结果一定存放在数组下标处（效率更高）
}

// 创建一个个的Entry对象，存放在数组中或链表连接起来
void createEntry(int hash,K key,V value,int bucketIndex){
    Entry<K,V> e = table[bucketIndex];
    table[bucketIndex] = new Entry<>(hash,key,value,e);
    size++;
}

// Entry类的定义
static class Entry<K,V> implements Map.Entry<K,V>{
    final K key;
    V value;
    Entry<K,V> next;
    int hash;			
    // 保存哈希值2，在修改对象属性时，属性值变了，但哈希值依旧不变（详见Set子接口中面试题）
}
```



2）jdk8.0：

1. 在创建了HashMap实例后，底层没有初始化table数组。当首次添加 (key,value) 时，进行判断，若发现table并未完成初始化，再对数组进行初始化

2. HashMap中的 Entry内部类 被 替换为 Node内部类，也就是说，创建的是Node[] table数组

3. 在数组相同索引处 构成链表时，新的 (key,value) 指向旧的 (key,value)（头插法）被替换为：旧的 (key,value) 指向 新的 (key,value)（尾插法）

4. 从jdk7.0的 数组+单向链表 结构 转化为 数组+单向链表+红黑树 的结构

   - 单向链表 --> 红黑树：当数组索引位置上的元素个数达到8，且数组长度达到64时，会将该索引位置上的多个元素从单向链表结构转换为红黑树结构进行存储

     > 使用红黑树：修改、查找、删除等操作时，时间复杂度为O(log n)

   - 红黑树 --> 单向链表：当使用红黑树的索引位置上的元素个数小于6时，会将红黑树结构退化为单向链表结构

     >使用单向链表：红黑树存储所占用的空间更大







###### LinkedHashMap

- 与HashMap相比，底层使用了双向链表
- 底层大部分源码与HashMap相同，但重写了newNode()，重新定义了Entry

```java
Node<K,V> newNode(int hash,K key,V value,Node<K,V> e){
  LinkedHashMap.Entry<K,V> p = new LinkedHashMap.Entry<K,V>(hash,key,value,e);
  linkNodeLast(p);
  return p;
}

static class Entry<K,V> extends HashMap.Node<K,V> {
    Entry<K,V> before, after;			// 记录前后的Entry
    Entry(int hash, K key, V value, Node<K,V> next) {
        super(hash, key, value, next);
    }
}
```









#### Collection

> 像contains()、add()、remove()等，底层都使用了equals()进行元素是否相等的判断，因此，实现类中，添加的元素对象所在的类，都要重写equals()（除了TreeSet）



##### Collection接口中的抽象方法

###### 添加

【boolean】`add(Object)`：添加元素到当前集合中

【boolean】`addAll(Collection)`：将 指定集合中的元素 一个一个的 添加进当前集合中

>当add()中的Object对象为集合时，则是 将整个集合作为一个元素进行添加





###### 判断

【boolean】`isEmpty()`：判断当前集合是否为空集合（底层：判断size是否为0）

【boolean】`equals(Object)`：判断当前集合与Object是否相等



【boolean】`contains(Object)`：判断 指定的对象 是否存在于当前集合中（底层：调用对象的equals方法进行是否相等的判断）

【boolean】`containsAll(Collection)`：判断指定集合中的所有元素 是否都存在于当前集合中





###### 删除

`clear()`：清空当前集合中的所有元素（底层：遍历将集合中的元素置为null）



【boolean】`remove(Object)`：删除当前集合中 第一个与obj对象相同 的元素（底层：调用对象的equals方法进行是否相等的判断，再进行删除）

【boolean】`removeAll(Collection)`：删除当前集合中所有的 与指定集合中 元素相同的元素

【boolean】`retainAll(Collection)`：删除当前集合中所有的 与指定集合中 元素不同的元素

> removeAll() 与 retainAll() 配合的话，约等于 clear()





###### 获取

【int】`size()`：获取当前集合中元素的个数

【int】`hashCode()`：获取当前集合对象的哈希值



【Object】`toArray()`：返回一个 包含当前集合中所有元素 的数组 （集合 --> 数组）

>将数组转换为集合：【List】（static）`Arrays.asList(数组)`



【Iterator】`iterator()`：返回当前集合所对应的迭代器的实例（用于遍历集合）

>1. Iterator是一个接口，返回的是该接口的实现类的对象
>2. 迭代器的常用方法：
>   - `hasNext()`：判断是否还有下一个元素
>   - `next()`：指针下移，并获取下移后 集合中的元素 并返回
>
>使用举例：
>
>```java
>ArrayList a = new ArrayList();		// 创建集合的实例
>Iterator i = a.iterator();		// 获取集合的迭代器对象
>while(i.hasNext()){			// 当还有下一个元素时，循环
>    System.out.println(i.next());	// 指针下移，并获取下移后的元素
>}
>```
>
>注意：要使用同一个迭代器对象，否则会陷入死循环





###### 两个默认方法

【Stream】`实现类对象.stream()`：获取Stream类的实例

`实现类对象.forEach(Consumer c)`：用于遍历









##### List子接口中的方法

> 除了从父类继承的方法外，额外有一些 根据索引进行操作 的方法

常用方法举例：

1）增

1. 【boolean】`add(Object)`（继承父类）
2. 【boolean】`addAll(Collection)`（继承父类）



2）删

1. 【boolean】`remove(Object)`（继承父类）
2. 【Object】`remove(int)`：删除当前集合中 指定索引下标的元素，返回被删除的元素



3）改

1. 【Object】`set(int,Object)`：修改当前集合中 指定索引下标的元素，返回被修改的元素



4）查

1. 【Object】`get(int)`：获取当前集合中 指定索引下标的元素后并返回



5）插

1. `add(int,Object)`：在指定的索引下标前面 插入元素
2. 【boolean】`addAll(int,Collection)`：在指定的索引下标前面，插入指定的集合中的 所有元素



长度：

1. 【int】`size()`（继承父类）



遍历：

1. 使用迭代器（使用到 iterator() 方法）
2. 增强for循环
3. 循环遍历（使用到 get() 方法）



补充：

1. 【List】`subList(int fromIndex,int toIndex)`：获取 从指定下标处开始 到指定下标处的前一个 之间的所有元素，返回一个集合对象
2. 【int】`indexOf(Object)`：返回obj对象在集合中首次出现的位置，若不存在，则返回-1
3. 【int】`lastIndexOf(Object)`：返回obj对象在集合中最后一次出现的位置，若不存在，则返回-1







##### Set子接口

###### HashSet与LinkedHashSet

> LinkedHashSet底层使用的双向链表，用来标识添加的元素的顺序，遍历时更明确

无序性：添加元素的位置是无序的

- 在添加元素时，根据添加的元素的哈希值，来决定存储的位置



不可重复性：添加到集合中的元素不能相同

1. 添加元素时，调用对象的hashCode()，根据属性等信息进行计算，得到哈希值，若哈希值不同，则直接添加元素
2. 若哈希值相同，则调用对象的equals()，若结果表示相同，则认为元素是相同的（若 equals() 后的结果不同，则以链表的形式存放在同一个位置处）

> 因此，添加的元素要满足：重写 equals() 和 hashCode()



面试题：

```java
HashSet set = new HashSet();
Person p1 = new Person("AA");		// 已重写hashCode()和equals()
Person p2 = new Person("BB");

set.add(p1);
set.add(p2);
System.out.println(set);		// p1和p2都被添加

p1.name = "CC";
set.remove(p1);
System.out.println(set);		// p1不会被删除
// remove和add一样，都会调用hashCode()和equals()进行比较
// 删除时，先根据属性值等信息进行计算后得到（由CC计算得到的）哈希值，再与集合中已有的（与AA计算后得到的）哈希值做比较，发现没有与之相同的，则认为无该元素

set.add(new Person("CC"));
System.out.println(set);		// CC被添加成功
// 由于属性被改成CC的哈希值，是由AA计算后得到的，此时CC计算后得到的哈希值，在当前集合中没有与之相同的，直接添加成功，并没有调用equals()

set.add(new Person("AA"));
System.out.println(set);		// AA被添加成功
// 属性被改成CC的哈希值，是由原来的AA计算后得到的，此时新添加的AA计算哈希值后，发现哈希值一样，则调用equals()，发现两者不同（String重写了Comparable接口），添加成功
```







###### TreeSet

> 按指定的排序方式，对添加的元素进行了排序

向TreeSet中添加元素的特点：

1. 添加的元素必须的是同类型数据
2. 根据所添加的元素的比较器（自然排序 或 定制排序）进行数值是否相等的判断，但依旧满足不可重复性（比较后结果相同的数据不会被添加）

> 因此，TreeSet中的元素对象，其类并不需要重写 equals() 和 hashCode() 方法，只需要满足自然排序 或 定制排序 即可
>
> 1. 自然排序：元素所在的类，实现Comparable接口，重写compareTo()，在其内书写 元素对象是否相等的比较规则
> 2. 定制排序：自定义类，实现Comparator接口，重写compare()，在其内书写 元素对象是否相等的比较规则，并将此自定义类的对象，作为TreeSet构造器的参数 来创建TreeSet对象，进行操作







#### Map

##### Map接口中的方法

###### 添加修改

【Object】`put(Object key,Object value)`：将指定的 key-value 添加到当前集合中，返回值为null

- 当已存在相同的 key 时，则为：在当前集合中，修改指定的 key 的值为value，返回值为 被修改前的value的值

  > 底层：在判断key是否相同时，调用key所在的类的 hashCode() 和 equals() 方法进行判断



`putAll(Map)`：将指定集合中的所有 key-value 添加到当前集合中





###### 删除

【Object】`remove(Object)`：在当前集合中，根据指定的key，移除key所对应的元素，返回key所对应的value（当不存在该key时，返回值为null）

`clear()`：清空当前集合中的所有元素





###### 查询

【Object】`get(Object)`：获取并返回指定key所对应的value

【int】`size()`：返回当前集合中的元素个数

【boolean】`isEmpty()`：判断当前集合是否为空

【boolean】`equals(Object)`：判断当前集合与obj对象是否相同



【boolean】`containsKey(Object)`：判断当前集合中是否包含指定的key

【boolean】`containsValue(Object)`：判断当前集合中是否包含指定的value





###### 遍历

【Set】`keySet()`：取出当前集合中的所有key，并构成一个Set集合后返回

【Collection】`values()`：取出当前集合中所有的value，并构成一个Collection集合后返回



【Set】`entrySet()`：取出当前集合中的每个 key-value 所构成的Entry，并构成一个Set集合后返回

- 使用Entry内的 getKey() 和 getValue() 可获取key 和 value值

```java
Map m = new HashMap();		
// 创建一个Map接口的实现类对象

Set s = m.entrySet();		
// 获取集合中所有的Entry

for(Object o:s){	// 增强for循环获取每一个Entry
    
    Map.Entry e = (Map.Entry)o;		// 将Object强转为Entry
    // Entry为Map的一个内部接口，HashMap中实现了该接口的实现类为Node
    
    System.out.println(e.getKey()+"->"+e.getValue());	
    // 调用Entry中的方法获取key和value
}
```







##### 知识点

###### HashMap

1. 相较于Hashtable的优点：可以添加 null 的key和value

2. HashMap中元素的特点：

   - 所有的key彼此间：不可重复、无序（所有的key构成了一个Set集合）

     > key所在的类还需重写 hashCode() 和 equals() 

   - 所有的value彼此间：可重复、无序（所有的value构成了一个Collection集合）

     > value所在的类还需要重写 equals()

   - 所有的 key-value 构成了一个Entry，所有的Entry彼此间：不可重复、无序（所有的Entry又构成了一个Set集合）
   
     > 观察Map源码发现：Entry是Map接口中的内部接口
     >
     > 观察HashMap源码发现：HashMap内部实现了该内部接口（内部实现类名为Node）
     >
     > - key-value作为Entry实现类中的属性出现
     > - 其内重写的方法：getKey() 和 getValue() 用来获取key-value的值





###### LinkedHashMap

是HashMap的子类，使用的双向链表，可以记录添加的元素的顺序，在遍历元素时，就可以按照添加的顺序显示





###### TreeMap

可以按照添加的元素的 key 所在的类中 重写的比较器 的比较规则，来对元素进行排序显示

- 因此，key 所在的类 重写的比较器（自然排序 或 定制排序）中的比较方法，决定了 两个元素是否相同

且TreeMap中添加的key，必须都是相同类型





###### Properties

是Hashtable的子类，其key和value都是String类型，常用来处理数据文件

> 若数据与代码存放到同一个文件中，则：
>
> 1. 耦合度过高
> 2. 当要修改数据时，就要重新编译代码、打包发布
>
> 因此，考虑将数据存放到配置文件中，让程序读取配置文件中的信息即可

使用举例：

```java
File file = new File("文件名.properties");		// 创建对应的文件的File类对象
FileInputStream fis = new FileInputStream(file);	// 创建输入型字节流对象

Properties p = new Properties();
p.load(fis);		// 加载流所对应的文件中的数据
String username = p.getProperty("username"); // 读取文件中key为username的value值
String password = p.getProperty("password"); // 读取文件中key为password的value值
fis.close();		// 关闭流资源
```







#### Collections类

##### 排序

（static）`reverse(List)`：反转集合中元素的存放顺序

（static）`shuffle(List)`：对集合中的元素进行随机排序

（static）`swap(List,int,int)`：将集合中 两个指定下标位置处的元素 进行交换



（static）`sort(List)`：对集合中的元素 按照其自然排序规则 进行排序

> 要求：
>
> 1. 集合中的元素必须是相同类型
> 2. 元素所在的类，实现Comparable接口，重写CompareTo()

（static）`sort(List,Comparator)`：对集合中的元素 按照其定制排序规则 进行排序

> 要求：集合中的元素必须是相同类型







##### 查找

【Object】（static）`max(Collection)`：根据自然排序中的规则，返回集合中 按自然排序后 处于集合最右边的元素

【Object】（static）`min(Collection)`：根据自然排序中的规则，返回集合中 按自然排序后 处于集合最左边的元素

> 要求：
>
> 1. 集合中的元素必须是相同类型
> 2. 元素所在的类必须实现自然排序



【Object】（static）`max(Collection,Comparator)`：根据定制排序中的规则，返回集合中 按定制排序后 处于集合最右边的元素

【Object】（static）`min(Collection,Comparator)`：根据定制排序中的规则，返回集合中 按定制排序后 处于集合最左边的元素

> 要求：集合中的元素必须是相同类型



【int】（static）`binarySearch(List,Object)`：使用二分查找的方式，按照自然排序方式进行比较，返回obj对象在集合中的下标

> 要求：
>
> 1. obj对象所在的类必须与 集合中的元素是同一类 或是 集合中的元素的父类
> 2. 集合中的元素必须是相同类型
> 3. 元素所在的类必须实现自然排序
> 4. 元素必须有序（根据自然排序）

【int】（static）`binarySearch(List,Object,Comparator)`：使用二分查找的方式，按照定制排序方式进行比较，返回obj对象在集合中的下标

>要求：
>
>1. obj对象所在的类必须与 集合中的元素是同一类 或是 集合中的元素的父类
>2. 集合中的元素必须是相同类型
>3. 元素必须有序（根据定制排序）



【int】（static）`frequency(Collection,Object)`：返回指定对象在集合中出现的次数







##### 复制

（static）`copy(List dest,List src)`：复制源集合中的元素 到 目标集合中

> copy()底层源码，会判断目标集合中是否有足够的存放空间（size是否比源集合的size大）
>
> 因此，目标集合必须先被填充到足够长。可以考虑循环填充null，或者：
>
> ```java
> List dest = Arrays.asList(new Object[src.size]);
> ```





##### 替换

【boolean】（static）`replaceAll(List,Object oldValue,Object newValue)`：使用新值 替换 集合中 所有的旧值





##### 添加

【boolean】（static）`addAll(Collection,Object...o)`：将指定的元素添加进集合中





##### 其他

【Xxx】（static）`synchronizedXxx(Xxx)`：调用该方法，传入Map或Collection实现类的对象，返回一个对应的 线程安全的操作对象

【Xxx】（static）`unmodifiableXxx(Xxx)`：调用该方法，传入Map或Collection实现类的对象，返回一个对应的 不可被修改（仅可查看）的操作对象









### 泛型

#### 知识点

> jdk5.0新特性

1）定义：

- 在定义类或接口时，允许通过 `<数据类型>` 的方式进行标识，使该类或接口中的 属性 为确定的类型、方法的返回值或参数 为确定的类型
- 该类型（参数）在调用时确定，如：继承或实现接口时、创建实例时、调用方法时
- 与形参类似的作用，未调用时 值不确定，调用时才能确定



2）注意：

1. 指明泛型类型时，不能为基本数据类型（泛型类型默认不指明时，为Object类型）
2. 在声明了泛型，但没有使用时，则认为操作的是Object类型
3. 若类或接口中使用了泛型，并确定其泛型类型后，相关的方法的参数或返回值类型使用了泛型的话，则都为泛型类型



3）使用举例：在HashMap中使用泛型

```java
HashMap<String,Integer> hm = new HashMap<>();	 // 由类型推断可知后部分泛型类型
// 源码：class HashMap<K,V>
// 指明HashMap类使用的泛型类型，K只能为String，V只能为Integer

hm.put("Tom",19);
hm.put("Amy",18);
// 源码：put(K key, V value)
// put()方法使用了泛型，此时受泛型限制，K只能为String，V只能为Integer

Set<Map.Entry<String,Integer>> e = hm.entrySet();
// 源码：Set<Map.Entry<K,V>> entrySet()
// 调用entrySet()后，返回的是Set集合，类型为Map的内部接口Entry
// Entry内部接口的定义：Entry<K, V>

for(Map.Entry me:e){
    System.out.println(me.getKey() +"->"+ me.getValue());
}
// 已经确定了Set集合中的数据类型为Map.Entry，可直接使用Map.Entry类型接收
// Map.Entry内部方法getKey()、getValue()使用了泛型
```







#### 自定义泛型

##### 泛型类或接口

格式：`class 类名<T>{}`

1. 泛型参数可以有多个，如<T,E>

2. jdk7.0开始，泛型使用时可以简化，如：`List<String> l = new ArrayList<>();`

3. 不能再泛型类中`new T[]`

   > 数组创建时，类型要确定，可以取巧：`E[] e = (E[])new Object[]`

4. 泛型类或接口中，不能在静态方法（非泛型方法）中使用泛型

   > 泛型类型在 创建对象 或 被继承 时体现，静态方法直接是通过类调用

5. 异常类不能是泛型类（没有作用）



泛型类被继承的几种情况：

```java
class A<T>{}			// 泛型类

class B extends A{}		// B不是泛型类，且A没有使用泛型特性
class C extends A<T>{}	// C不是泛型类，但A使用了泛型特性
class D extends A<String>{}		// D不是泛型类，但A使用了泛型特性，且已确定泛型类型

class E<T> extends A<T>{}		// E是泛型类，且与A使用了相同的泛型类型
class F<E> extends A<T>{}		// F是泛型类，但与A使用了不同的泛型类型
class G<E,T> extends A<T>{}   // G是泛型类，与A有使用了相同的泛型，但也额外多了些类型
```







##### 泛型方法

格式：`权限修饰符 <T> 返回值类型 方法名(形参类型 形参值){}`

1. 定义了泛型类型为T后，一般在 返回值类型 或 形参类型 处会使用到泛型类型

2. <E>一定要添加，否则，若直接在返回值类型或形参类型处使用泛型时，会被当成类处理

3. 泛型方法可以被声明为静态

   > 在调用方法传递参数时，确定泛型类型，与类无关

4. 泛型方法所在的类可以不是泛型类









#### 通配符

##### 泛型的继承关系

1. 类SuperA是类A的父类，则 Class< SuperA > 与 Class< A > 的关系：没有任何子父类的关系

   ```java
   ArrayList<Object> l1 = null;
   ArrayList<String> l2 = new ArrayList<>();
   l1 = l2;		// 报错，假设有关系的话：
   // l1.add(123);
   // String s = l2.get(0);   就会出错
   ```

2. 类SuperA是类A的父类，SuperA< E > 与 A< E >的关系：有继承关系







##### 通配符

> 需求：
>
> ```java
> ArrayList<Object> l1 = ;
> ArrayList<String> l2 = ;
> method(l1);
> method(l2);		// 报错，原因见上
> 
> public void method(ArrayList<Object> l){}
> ```
>
> 调用方法时，可否传递不同泛型类型的参数？

使用通配符：< ? >

1. 可以看成：存储 ( -无穷 ~ +无穷 ) 之间的所有类型

2. 可以读取数据，读取后的返回值类型为Object类型

   ```java
   ArrayList<?> l1 = null;
   ArrayList<Integer> l2 = new ArrayList<>();
   l1 = l2;
   l2.add(123);
   Object o1 = l1.get(0);		// 假设返回值类型不是为Object，则应该为Integer
   
   ArrayList<String> l3 = new ArrayList<>();
   l1 = l3;
   l3.add("test");
   Object o2 = l1.get(0);		// 假设返回值类型不是为Object，则应该为String
   
   // 所以返回值类型应该为Object，用于接收所有类型的数据
   ```

3. 不可以写入数据（只能写入null）

   ```java
   ArrayList<?> l1 = null;
   ArrayList<Integer> l2 = new ArrayList<>();
   l1 = l2;
   l1.add("test");		// 报错，假设可以的话：
   // Integet i = l2.get(0);	就会出现问题
   ```







##### 有限制条件的通配符

1. Class< ? extends A >：可以将 Class< A > 或 Class< B > 赋值给 Class< ? extends A >，其中，类B是类A的子类

   - 可以看成：存储 ( -无穷 ~ A ) 之间的所有类型

   - 可以读取数据，返回值类型为类型A，或者是类A的父类（如Object类型）

     ```java
     class A{}
     class B extends A{}
     
     ArrayList<? extends A> l = null;
     ArrayList<B> l1 = new ArrayList<>();
     l = l1;
     l1.add(new B());
     A a = l.get(0);			// 多态，父类的引用指向子类的对象
     // 因此，只要是A类的子类，其对象都可以赋值给其父类
     ```

   - 不可以写入数据（只能写入null）

     ```java
     class A{}
     class B extends A{}
     
     ArrayList<? extends A> l1 = null;
     ArrayList<B> l2 = new ArrayList<>();
     l1 = l2;
     l1.add(new A());		// 报错，假设可以的话：
     // B b = l2.get(0);	就会出现问题（子类类型存储父类对象？）
     ```

2. Class< ? super A >：可以将 Class< A > 或 Class< B > 赋值给 Class< ? super A >，其中，类B是类A的父类

   - 可以看成：存储 ( A ~ +无穷 ) 之间的所有类型

   - 可以读取数据，返回值类型为Object

     ```java
     class C{}
     class B extends C{}
     class A extends B{}
     
     ArrayList<? super A> l = null;
     ArrayList<B> l1 = new ArrayList<>();
     l = l1;
     l1.add(new B());
     Object o1 = l.get(0);		// 假设返回值类型不是Object，则应该是B
     
     ArrayList<C> l2 = new ArrayList<>();
     l = l2;
     l2.add(new C());
     Object o2 = l.get(0);		// 假设返回值类型不是Object，则应该是C
     
     // 所以返回值类型应该为Object，用于接收所有类型的数据
     ```

   - 可以写入数据，仅能存储 A类或A类子类 类型的数据

     ```java
     class B{}
     class A extends B{}
     class C extends A{}
     
     List<? super A> l = null;
     List<B> l1 = new ArrayList<>();
     l = l1;
     l.add(new A());		// 多态，B是A的父类，因此可以存储A类
     l.add(new C());		// 多态，B是A的父类，A是C的父类，也可以存储C类
     
     // 因此，只要是类A或类A的子类，都可以被 已有的类B 或 类B的父类等 所存储
     ```









### 文件操作

#### File类

##### 知识点

1. 位于java.io包下
2. 每一个File类的对象，对应着操作系统中的一个文件或文件目录
3. File类的对象，通常作为io流操作文件的端点出现（将File类的对象作为参数传递到IO流相关类的构造器中）
4. 相对路径与绝对路径：
   - 相对路径：相对于某个文件目录而言的位置
     - 若使用单元测试方法，则相对而言是当前的module
     - 若使用main()方法，则相对而言是当前的project





##### 构造器

```java
public File(String pathname)
// 以pathname作为路径创建File类的对象（可以是相对路径或绝对路径）

public File(String parent,String child)
// 以parent为父路径，child为子路径 创建的File类的对象
// 父路径 一定是一个文件目录的路径
// 子路径 可以是一个文件的路径，也可以是一个文件目录的路径
    
public File(File parent,String child)
// 根据 父File类的对象 和 子路径 创建的File类的对象
// 父File类的对象 一定是一个文件目录
// 子路径 可以是一个文件的路径，也可以是一个文件目录的路径
```





##### 方法

> 类中声明了新建、修改、删除等方法，但都没有涉及到对文件内容进行读写的操作

1）获取File类对象

【File】`getAbsoluteFile()`：获取 记录文件或目录存放的绝对路径 的File类对象

【File[]】`listFiles()`：获取 当前目录中所有的子文件或目录 的每一个File类对象





2）获取文件和目录基本信息

【String】`getName()`：获取文件或目录的名称

【String】`getPath()`：获取File类中指定的 文件或目录的路径

【String】`getAbsolutePath()`：获取文件或目录存放的绝对路径

【String】`getParent()`：获取File类中指定的 文件或目录的 上层文件的路径，若无，则返回null

【long】`length()`：获取文件的长度（文件内容的总字节数），不能获取目录的长度（返回0）

【long】`lastModified()`：获取最后一次修改文件或目录的时间（毫秒）

【String[]】`list()`：获取当前目录中的所有子文件或目录





3）判断

【boolean】`exists()`：判断File类中指定的 文件或目录是否存在

【boolean】`isDirectory()`：判断File类中指定的 是否是一个目录

【boolean】`isFile()`：判断File类中指定的 是否是一个文件

【boolean】`canRead()`：判断是否可读

【boolean】`canWrite()`：判断是否可写

【boolean】`isHidden()`：判断是否隐藏





4）创建、修改与删除

【boolean】`createNewFile()`：创建文件，若文件存在，则不创建；若文件路径错误，抛出异常

【boolean】`mkdir()`：创建文件目录，若文件目录存在，则不创建；若文件目录的上层目录不存在，也不创建

【boolean】`mkdirs()`：创建文件目录，若文件目录的上层目录不存在，则一并创建



【boolean】`renameTo(File)`：将文件重命名为 指定的File类对象 所指定的文件路径下的 指定文件名

> 被重命名的文件必须存在，重命名的文件必须不存在，重命名的文件所在的目录必须存在



【boolean】`delete()`：删除文件或目录

> 永久删除，且删除文件目录时，文件目录内不能含有其他文件或文件目录







##### 例题

> 递归获取指定文件目录下的所有文件

```java
public void getAllFile(File f){
    if(f.isFile()){				// 递归结束条件：file类的对象是一个文件而非目录
        System.out.println(f.getName());
    }else{
        File[] files = f.listFiles();	// 获取当前目录下的所有文件或目录
        for(File file:files){
            printFileName(file);
            // 取出当前目录下的所有文件或目录，递归调用
            // 当是文件时，退出递归，当还是文件夹时，继续取出目录下的所有文件或目录，递归
        }
    }
}
```







#### IO流

##### 知识点

IO流的分类：

1. 流向的不同：输入流、输出流
2. 处理单位的不同：字节流、字符流
3. 流的角色不同：节点流、处理流

|   分类   |     字节输入流      |      字节输出流      |    字符输入流     |     字符输出流     |
| :------: | :-----------------: | :------------------: | :---------------: | :----------------: |
| 抽象基类 |     InputStream     |     OutputStream     |      Reader       |       Writer       |
|  文件流  |   FileInputStream   |   FileOutputStream   |    FileReader     |     FileWriter     |
|  缓冲流  | BufferedInputStream | BufferedOutputStream |  BufferedReader   |   BufferedWriter   |
|  转换流  |                     |                      | InputStreamReader | OutputStreamWriter |
|  对象流  |  ObjectInputStream  |  ObjectOutputStream  |                   |                    |

处理步骤：

1. 创建读取文件或写出文件的File类的对象
2. 创建输入流或输出流对象
3. 处理读入或写出的过程
4. 关闭流资源







##### 文件流

> 节点流的一种

###### 处理字符

> 读取文件

```java
// 1.创建File类的对象
File file = new File("文件名");

// 2.创建输入型字符流对象，用于读取文件中的数据
FileReader fr = new FileReader(file);

// 3.调用read(char[])读取数据
char buffer = new char[10];		// 存放 用read()读取的数据 的数组
int len;						// 用于记录 每次read()读取的数据的个数
while( (len = fr.read(buffer)) != -1 ){
    for(int i=0;i<len;i++){		// 不能读取数组的长度，因为 有读取少于10个字符的情况
        System.out.println(buffer[i]);
    }
}

// 4.关闭资源流
fr.close();				// 由于要关闭资源流，所以出现异常时，要try-catch-finally处理
```



> 写入文件

```java
// 1.创建File类的对象
File file = new File("文件名");

// 2.创建输出型字符流对象，用于输出数据到文件中
FileWriter fw = new FileWriter(file);

// 3.调用write(String)输出数据
fw.write("内容");

// 4.关闭资源流
fw.close();				// 由于要关闭资源流，所以出现异常时，要try-catch-finally处理
```



案例：实现将一个文件中的内容 复制到 另一个文件中

```java
File f1 = new File("old.txt");		// 读取内容的文件
File f2 = new File("new.txt");		// 写入内容的文件
FileReader fr = new FileReader(f1);	// 输入型字符流
FileWriter fw = new FileWriter(f2);	// 输出型字符流
char[] buffer = new char[5];		// 读取内容后存放数据的数组
int len;		// 每次读取内容的个数
while((len=fr.read(buffer))!=-1){	// 不为-1表示内容还没读取完
    fw.write(buffer,0,len);		// 写入字符数组 从0到len下标处的字符
}
fr.close();		// 关闭流资源
fw.close();
```



总结：

1. 对于输入流来讲，其File类的对象所对应的文件必须存在，否则会报错。

   对于输出流来讲，其File类的对象所对应的文件可以不存在：

   - 若文件不存在，则会自动创建该文件，并写入数据
   - 若文件存在，则：
     - 使用 `FileWriter(File)` 或 `FileWriter(File,false)` 创建的输出流对象，在输出数据前，会创建同名的文件 并对现有的文件进行覆盖
     - 使用 `FileWriter(File,true)` 输出数据时，会在现有的文件的末尾 进行内容的追加

2. 读入数据时，可以使用 `read()` ，每次只读取一个字符（会频繁与磁盘进行交互，效率低）

3. 写入数据时，可以使用 `write(char[],int fromIndex,int lenth)` ，每次写入：指定字符数组中，从指定起始位置开始，指定个数的字符（读与写共同操作文件时常用）







###### 处理字节

案例：将一张图片进行复制

```java
File f1 = new File("old.jpg");			// 要复制的图片路径和文件
File f2 = new File("new.jpg");			// 复制后的新路径和文件名
FileInputStream fis = new FileInputStream(f1);		// 创建输入型字节流对象
FileOutputStream fos = new FileOutputStream(f2);	// 创建输出型字节流对象
byte[] buffer = new byte[1024];			// 创建字节数组，用于每次保存读取到的数据
int len;		// 每次读取到的数据的个数
while((len=fis.read(buffer))!=-1){		// 当个数不为-1时，表示文件还未读取完
    fos.write(buffer,0,len);			// 将字节数据进行写出
}
fos.close();		// 关闭资源
fis.close();
```

注意：

1. 字节数据存储的数组为byte[]类型
2. 字节流可以用来复制文本文件（字符），但在读取时，可能会出现乱码（与字符编码有关）







##### 缓冲流

> 处理流的一种，是对已有的节点流进行封装的操作
>
> 将数据缓存在缓冲区（本质上是一个数组）中，再写入到磁盘或读取到内存中，减少了与磁盘的交互次数，提高效率

###### 处理字节

案例：复制图片

```java
File f1 = new File("old.jpg");		// 要复制的文件
File f2 = new File("new.jpg");		// 复制后的文件
FileInputStream fis = new FileInputStream(f1);		// 输入型文件流
FileOutputStream fos = new FileOutputStream(f2);	// 输出型文件流
BufferedInputStream bis = new BufferedInputStream(fis);		// 输入型缓冲流
BufferedOutputStream bos = new BufferedOutputStream(fos);	// 输出型缓冲流
byte[] buffer = new byte[1024];		// 存放从文件中读取的数据
int len;		// 记录每次读入到buffer中的数据的个数
while((len=bis.read(buffer))!=-1){	// 使用缓冲流读取
    bos.write(buffer,0,len);		// 使用缓冲流写入
    bos.flush();		// 刷新缓冲区
}
bis.close();
bos.close();
```

注意：

1. 刷新缓冲区，是为了确保数据写入到磁盘中：若资源没关闭而程序结束运行，可能导致有些数据在缓冲区而没有及时写入到文件中
2. 关闭流资源时，要先关闭外层流，再关闭内层流。由于外层流的关闭会自动对内层流也进行关闭，因此内层流的关闭操作可以省略





###### 处理字符

案例：复制文本文件

```java
File f1 = new File("old.txt");		// 要复制的文件
File f2 = new File("new.txt");		// 复制后的文件
BufferedReader br = new BufferedReader(new FileReader(f1)); // 输入型缓冲流
BufferedWriter bw = new BufferedWriter(new FileWriter(f2)); // 输出型缓冲流
String str;			// 存放读取的数据
while((str=br.readLine())!=null){	// 当读取后返回值不为bull，表示未到文件末尾
    bw.write(str);			// 写出数据
    bw.newLine();			// 换行
}
br.close();			// 关闭资源
bw.close();
```

注意：

1. `readLine()` 每次读取一行数据（但不包括行尾的换行符），返回字符串；当返回值为 null 时，表示已经读取到文件末尾
2. `newLine()` 表示输出换行符







##### 转换流

> 处理流的一种，用于实现字节与字符之间的转换

案例：将gbk编码集的文件转换为utf-8编码集的文件

```java
File f1 = new File("gbk.txt");		// 转换前的gbk编码集的文件
File f2 = new File("utf.txt");		// 转换后的utf-8编码集的文件
FileInputStream fis = new FileInputStream(f1);		// 输入型字节流
FileOutputStream fos = new FileOutputStream(f2);	// 输出型字节流

// 读取gbk编码格式的文件，写出为utf8编码格式的文件
InputStreamReader isr = new InputStreamReader(fis,"gbk");
// 将一个输入型的字节流转换为输入型的字符流（解码，字节 --> 字符）
OutputStreamWriter osw = new OutputStreamWriter(fos,"utf8");
// 将一个输出型的字节流转换为输出型的字符流（编码，字符 --> 字节）

char[] buffer = new char[1024];		// 将字节转换为字符读入到内存中，用char[]
int len;		// 每次读取数据的长度
while((len=isr.read(buffer))!=-1){
    osw.write(buffer,0,len);		// 写出时，将字符转换为字节进行保存
}
isr.close();	// 关闭资源
osw.close();
```







##### 对象流

> 处理流的一种，可用于处理序列化和反序列化

1. 序列化：允许将内存中的数据转换为字节流，从而可以将此字节流写入到磁盘中 或 通过网络的方式将该字节流传输到网络中的另一个节点

   > ObjectOutputStream：将内存中的对象输出出去

2. 反序列化：当其他节点获取到了该字节流，可以恢复成原来内存中的数据

   > ObjectInputStream：将传输过来的还原为内存中的对象



###### 序列化

```java
File file = new File("aim.dat");		// 序列化后保存数据的文件
FileOutputStream fos = new FileOutputStream(file);		// 输出型字符流
ObjectOutputStream oos = new ObjectOutputStream(fos);	// 输出型字节流
oos.writeUTF("字符串内容");			// 传输字符串类型的对象
oos.flush();			// 刷新缓冲区
oos.writeObject("Object类型的数据");	// 传输Object类型的对象
oos.flush();
oos.close();			// 关闭资源
```



###### 反序列化

```java
File file = new File("aim.dat");		// 存储序列化数据的文件
FileInputStream fis = new FileInputStream(file);	// 输入型字符流
ObjectInputStream ois = new ObjectInputStream(fis);	// 输入型字节流
String s1 = ois.readUTF();				// 读取文件中的String类型的对象
String s2 = (String)ois.readObject();	// 读取文件中的Object类型的对象
ois.close();			// 关闭资源
```

注意：

若自定义类需要实现序列化，则需要：

1. 实现接口：Serializable（标识接口）

2. 声明一个全局常量：static final long serialVersionUID = xxxxxL;（用来唯一标识当前类）

   >若不声明时，系统会自动生成一个 针对于现在的类的serialVersionUID。若后期修改该类时，会导致原先的serialVersionUID发生改变，从而在进行反序列化时，根据改后的serialVersionUID找不到对应的类，还原失败，抛出异常

3. 自定义的类中的每一个属性，也必须是可序列化的（即：当属性为引用数据类型，且也为自定义类的类型时，该类也需要满足这3点要求）

   > 若类中的属性（值）不想被序列化时，可以声明为 `transient` 或 `static`
   >
   > - static不会被序列化的原因：属性值为类所管，与对象无关







### 网络编程

#### 知识点

1）要想实现网络编程，需要解决的三个问题：

1. 如何准确的定位到网络上的一台或多台主机？
   - 使用IP地址
2. 如何定位主机上的特定应用？
   - 使用端口号
3. 在定位主机、绑定特定应用后，如何进行数据传输？
   - 遵循网络通信协议



2）IP地址分类：

1. 按地址类型：
   - IPv4：占用4个字节
   - IPv6：占用16个字节
2. 按地址可用范围：
   - 公网地址（万维网）
   - 私有地址（局域网）

> 特殊IP：127.0.0.1（本地回路地址）



3）域名：用于便捷的记录IP地址



4）端口号：唯一标识主机中的某个应用程序（不同进程分配不同的端口号，范围为0~65535）



5）通信协议：为了实现可靠而又高效的数据传输。

> 两种常见的网络参考模型：OSI参考模型（7层）、TCP/IP参考模型（4层）







#### InetAddress类

> 每一个InetAddress类的实例，对应一个IP地址

获取类的实例：

1. 【InetAddress】（static）`getByName(String)`：根据IP或域名，获取对应的InetAddress类的对象
2. 【InetAddress】（static）`getLocalHost()`：获取本机的IP地址



常用方法：

1. 【String】`getHostName()`：获取对象所对应的IP地址 的域名（若没有域名时，返回IP地址）
2. 【String】`getHostAddress()`：获取对象所对应的IP地址







#### TCP网络编程

> 可靠的连接，发送数据前进行三次握手，断开连接时进行四次挥手，能进行大数据量的传输，效率较低

客户端：

1. 创建Socket类的对象，指明服务端的IP地址和端口号

   > 构造器：`Socket(String IP,int port)`

2. 创建输出流，发送数据

   > 【OutputStream】`Socket对象.getOutputStream()`：获取输出流
   >
   > `OutputStream对象.write(byte[])`：发送数据

3. 关闭Socket、流资源

服务端：

1. 创建ServerSocket类的对象，指明开放的端口号

   > 构造器：`ServerSocket(int port)`

2. 接收来自客户端的Socket

   > 【Socket】`ServerSocket对象.accept()`：获取来自客户端的Socket

3. 创建输入流，接收数据

   >【InputStream】`Socket对象.getInputStream()`：获取输入流
   >
   >`InputStream对象.read(byte[])`：接收数据

4. 关闭资源



案例：客户端发送数据，服务端收到数据后进行响应

> 客户端

```java
Socket socket = new Socket("127.0.0.1",2333);
// 创建Socket类的对象，指明服务端的IP和端口号

OutputStream os = socket.getOutputStream();		// 获取输出流对象
os.write("你好，我是客户端".getBytes());		// 将byte类型数据发送给服务端
socket.shutdownOutput();		// 告知服务端：已发送数据完毕

InputStream is = socket.getInputStream();		// 获取输入流对象
byte[] buffer = new byte[1024];		// 创建byte[]，用于存储每次接收到的数据
int len;		// 每次接收到的数据的长度
ByteArrayOutputStream baos = new ByteArrayOutputStream();
// 处理流，将存储在byte[]中的数据暂时写入到此对象中，最后再一次性输出该对象中 内置的byte[]中的数据

while((len=is.read(buffer))!=-1){	// 当输入流读取到值后没返回-1，表明文件还未到末尾
    baos.write(buffer,0,len);		// 将byte[]中的数据先存储在baos对象内置的数组中
}
System.out.println(baos);			// 再一次性读取所有的byte类型的数据就不会出现乱码

baos.close();				// 关闭资源
is.close();
os.close();
socket.close();
```



> 服务端

```java
ServerSocket serverSocket = new ServerSocket(2333);
// 创建ServerSocket类的对象，指明开放的端口号

Socket socket = serverSocket.accept();		// 接收来自客户端的Socket（阻塞式方法）

InputStream is = socket.getInputStream();	// 获取输入流对象

byte[] buffer = new byte[1024];		// 创建byte[]，用于存储每次接收到的数据
int len;		// 每次接收到的数据的长度
ByteArrayOutputStream baos = new ByteArrayOutputStream();
// 处理流，内置了数组，可以将存储在byte[]中的数据暂时写入到此对象中，最后再一次性输出该对象中 内置的byte[]中的数据，用来防止不连续输出byte时可能出现的乱码问题

while((len=is.read(buffer))!=-1){	// 当输入流读取到值后没返回-1，表明文件还未到末尾
    baos.write(buffer,0,len);		// 将byte[]中的数据先存储在baos对象内置的数组中
}
System.out.println(baos);			// 再一次性读取所有的byte类型的数据就不会出现乱码

String ip = socket.getInetAddress().getHostAddress();	// 获取客户端的IP地址
System.out.println("收到了来自" + ip + "的数据");

OutputStream os = socket.getOutputStream();		// 获取输出流对象
os.write("你好，我是服务端".getBytes());		// 响应数据给客户端

os.close();					// 关闭资源
baos.close();
is.close();
socket.close();
serverSocket.close();
```

注意：

1. 发送完数据后，要及时使用`Socket对象.shutdownOutput()` 告知对方自己已发送完数据，否则对方会一直停留在 read() 阶段，等待自己发送数据
2. ByteArrayOutputStream流的作用：是为了防止在输出byte类型的数据时出现的乱码问题。因此，若是为复制图片等（不涉及到打印）操作时，完全可以不使用该流
3. 关闭资源流时，可以从下往上看，防止出现遗漏。ServerSocket服务端的资源，在开发中一般不会关闭







#### UDP网络编程

> 不可靠的连接，发送前，不需要确认对方是否存在，使用数据报进行传输数据（限制在64kb以内），效率较高

客户端：

1. 创建DatagramSocket类的对象

   > 构造器：`DatagramSocket()`

2. 发送数据

   > `DatagramSocket对象.send(DatagramPacket)`
   >
   > 构造器：`DatagramPacket(byte[],int from,int length,InetAddress ip,int port)`
   >
   > - byte[]：要发送的数据
   > - int from：从指定下标处开始取数据
   > - int length：取指定长度的数据
   > - InetAddress ip：服务端IP地址
   > - int port：服务端端口号

3. 关闭Socket资源

服务端：

1. 创建DatagramSocket类的对象，指明接收数据的端口号

   > 构造器：`DatagramSocket(int port)`

2. 接收数据

   > `DatagramSocket对象.receive(DatagramPacket)`
   >
   > 构造器：`DatagramPacket(byte[],int from,int length)`
   >
   > - byte[]：要存储数据的数组
   > - int from：从指定下标处开始取数据
   > - int length：取指定长度的数据

3. 获取数据

   > `DatagramPacket对象.getData()`：获取存储数据的数组
   >
   > `DatagramPacket对象.getLength()`：获取存储了数据的量

4. 关闭Socket资源



案例：

> 客户端

```java
DatagramSocket ds = new DatagramSocket();			// 创建Socket对象

InetAddress ia = InetAddress.getByName("127.0.0.1");
// 获取服务端IP所对应的InetAddress类的对象
byte[] data = "这是数据内容".getBytes("utf-8");		// 将数据转换为字节

DatagramPacket dp = new DatagramPacket(data,0,data.length,ia,2333);	
// 封装数据包

ds.send(dp);			// 发送数据包
ds.close();				// 关闭资源
```



> 服务端

```java
DatagramSocket ds = new DatagramSocket(2333);		// 创建Socket对象

byte[] buffer = new byte[1024*64];				// 创建存储数据的数组
DatagramPacket dp = new DatagramPacket(buffer,0,buffer.length);	// 接收数据包格式
ds.receive(dp);			// 接收数据包

String data = new String(dp.getData(),0,dp.getLength());	// 获取数据包中的数据
System.out.println(data);
ds.close();				// 关闭资源
```







#### URL网络编程

> 统一资源定位符，俗称种子。每一个具体的url对应着互联网上的某一资源的地址
>
> 格式：http://127.0.0.1:80/test/abc.jpg?id=1
>
> - 应用层协议://IP地址:端口号/资源地址?参数列表

1）创建URL类对象

> 构造器：`URL(String url)`

2）常用方法：

【String】`getProtocol()`：获取url的协议名

【String】`getHost()`：获取url的IP

【String】`getPort()`：获取url的端口号

【String】`getPath()`：获取url的文件路径

【String】`getFile()`：获取url的文件名

【String】`getQuery()`：获取url的参数列表



案例：下载URL中的资源

```java
URL url = new URL("http://127.0.0.1:8080/test/abc.jpg");		// 获取URL类对象
URLConnection uc = url.openConnection();			// 建立与URL对象的连接
HttpURLConnection huc = (HttpURLConnection)uc;		// 强转为对应协议类型
InputStream is = huc.getInputStream();				// 获取连接的输入流对象

FileOutputStream fos = new FileOutputStream(new File("d:\\test\\cba.jpg"));
// 获取输出流对象
byte[] buffer = new byte[1024];				// 创建存储数据的数组
int len;				// 每次存入数组的数据长度
while((len=is.read(buffer))!=-1){		
    fos.write(buffer,0,len);			// 将每次读取到的byte类型数据写出到指定文件中
}
fos.close();			// 关闭资源
is.close();
huc.disconnection();
```









### 反射

#### 知识点

##### 反射的理解

反射的作用：

1. 使用反射，可以动态的获取运行时对象所属的类的类型
2. 使用反射，可以调用运行时类中的任意结构

反射与封装性的理解：

1. 封装性：体现的是 是否建议调用。若修饰为private，则说明不建议我们去调用该结构。
2. 反射：体现的是 是否能够调用。因为类的结构都加载到了内存中，因此就能调用。





##### Class类的理解

对一个编写好的 .java源文件进行编译（javac.exe），会生成一个或多个 .class的字节码文件。当使用java.exe去解释运行 .class文件时，此时的 .class字节码文件就会被 类的加载器 加载到内存（方法区）中，而加载到内存中的 .class文件所对应的结构，就是Class类的一个实例

- 此 被加载进内存中的类，也被称为 运行时类

- 每一个运行时类所对应的 Class类的实例，都会在内存中的方法区中 缓存 起来，因此，整个程序的执行过程中，一个类（运行时类）只有一个对应的Class类的实例

- 该运行时类所对应的Class类的实例，可以看做是 反射的源头

- Class类的实例的类型，可以是：

  - class：内部类、外部类（Object.class）

  - interface：接口（Comparable.class）

  - []：一维数组（String[].class）、二维数组（int\[][].class）

    > 其中，只要元素类型和维度一样，则认为是同一个Class类的实例

  - enum：枚举类（ElementType.class）

  - annotation：注解（Override.class）

  - 基本数据类型（int.class）

  - void（void.class）





##### 类的加载器

> 类加载器的作用：负责类的加载，并对应于一个Class类的实例
>
> 以jdk8.0为例



###### 类的加载过程

1. 类的装载（loading）：将类的 .class文件载入内存，并创建对应的Class类的实例对象，此过程就由 类的加载器完成

2. 链接（linking）：

   ① 验证（Verify）：确保加载的类的信息符合JVM规范（如：以十六进制的cafebabe开头 --> 表示没有安全问题）

   ② 准备（Prepare）：正式为类变量（static）分配内存，并设置类变量的默认初始值 的阶段（分配内存都会在方法区中进行分配）

   ③ 解析（Resolve）：将虚拟机常量池中 符号引用（常量名） 替换为 直接引用（地址）的过程

3. 初始化（initialization）：执行类构造器\<clinit>的过程

   > 类构造器<clinit>是由：编译期间自动收集 类中所有的类变量的赋值动作 所合并而成的



###### 类的加载器分类

1. BootstrapClassLoader：引导类加载器/启动类加载器

   - 由C/C++编写的，不能通过java代码获取实例
   - 负责加载java的核心库

2. 继承于ClassLoader类的加载器

   ① ExtensionClassLoader：扩展类加载器

   - 负责从java.ext.dirs系统属性所指定的目录中加载类库，或从JDK安装目录的jre/lib/ext子目录下加载类库

   ② SystemClassLoader / ApplicationClassLoader：系统类加载器/应用程序类加载器

   - 用户自定义的类默认使用的类加载器

   ③ 用户自定义的类加载器

   - 用于实现应用的隔离（同一个类在一个应用程序中可以加载多份）或数据的加密





###### 获取类的加载器

```java
ClassLoader cl = ClassLoader.getSystemClassLoader();
// 获取系统类加载器：AppClassLoader

ClassLoader clP1 = cl.getParent();
// 获取扩展类加载器：ExtClassLoader

ClassLoader clP2 = clP1.getParent();
// 获取引导类加载器：null（失败，无法获取引导类加载器）
```

> 类的加载器间属于同级关系，实现了 双亲委托机制





###### 其他方法

`类加载器对象.getResource(String path)`：获取指定文件路径在编译后的绝对路径





###### 使用案例

> 使用类的加载器读取文件，获取文件对应的流对象

```java
ClassLoader cl = ClassLoader.getSystemClassLoader();	// 获取系统类加载器
InputStream is = cl.getResourceAsStream("文件名");		// 获取指定文件的输入流对象
Properties p = new Properties();		// 创建Map实现类对象
p.load(is);			// 将流所对应的文件中的数据加载进来
String s = p.getPorperty("key名");		// 获取文件中key所对应的value的值
is.close();			// 关闭资源
```

注意：加载的文件的相对路径：在src目录下







#### Class类实例的创建

方式一：调用运行时类的静态属性：class

```java
Class clazz = Person.class;
```

方式二：调用类（运行时类）的对象的方法：getClass()

```java
Person p = new Person();
Class clazz = p.getClass();
```

方式三：调用Class类中的静态方法：forName(String className)

```java
Class clazz = Class.forName("java.lang.String");		// 要使用全类名
```

方式四：使用类的加载器（此方式获取Class类的实例，只经历类的加载中 类的装载的过程）

```java
ClassLoader cl = ClassLoader.getSystemClassLoader();
// 调用ClassLoader类的方法获取类的加载器

Class clazz = cl.loadClass("java.lang.String");
// 调用类的加载器中的方法获取Class类的实例
```







#### 反射的应用

```java
class Person{
    private static int id;
    private String name;
    
    public Person(){}
    private Person(String name){
        this.name = name;
    }
    
    private String getInfo(String name,int age){
        return name+" is "+age+" years old now";
    }
}
```

##### 调用构造器

> 调用公共的无参构造器

```java
Class clazz = Person.class;			// 创建Class类的实例	
Constructor c = clazz.getConstructor();	// 通过Class类的实例，获取所在的类的无参构造器
Person p = (Person)c.newInstance();			// 通过构造器创建该类的对象
```



> 调用私有的带参构造器

```java
Class clazz = Person.class;			// 创建Class类的实例
Constructor c = clazz.getDeclaredConstructor(String.class,int.class);
// 通过Class类的实例，获取所在的类的 带指定参数的构造器
c.setAccessible(true);			// 设置可访问私有构造器
Person p = (Person)c.newInstance("Tom",12);		// 传递构造器所需参数，创建对象
```





##### 调用属性

> 调用私有的属性

```java
Class clazz = Person.class;			// 创建Class类的实例	
Person p = (Person)clazz.newInstance();		// 调用公共的无参构造器，创建对象（过时）

Field nameField = clazz.getDeclaredField("name");
// 获取属性名为name的Field类型的操作对象
nameField.setAccessible(true);		// 设置可访问私有属性
nameField.set(p,"Jack");			// 设置指定的对象 指定的属性的值
String name = (String)nameField.get(p);		// 获取指定的对象 指定的属性的值
```



> 调用静态的私有属性

```java
Class clazz = Person.class;			// 创建Class类的实例
Field idField = clazz.getDeclaredField("id");
// 获取属性名为id的Field类型的操作对象
idField.setAccessible(true);		// 设置可访问私有属性
idField.set(null,1001);	
// 由于静态属性与对象无关，由类调用，因此不需要传入对象。而类在创建Class类实例时已确定，因此可以不使用Person.class，而是直接使用null
int id = (int)idField.get(null);		// 直接获取该类的静态属性
```





##### 调用方法

> 调用私有的带返回值的方法

```java
Class clazz = Person.class;			// 创建Class类的实例
Person p = (Person)clazz.newInstance();		// 调用公共的无参构造器，创建对象（过时）

Method getInfoMethod =clazz.getDeclaredMethod("getInfo",String.class,int.class);
// 获取方法名为getInfo，带有指定的形参列表的方法
getInfoMethod.setAccessible(true);			// 设置可访问私有的方法
String s = (String)getInfoMethod.invoke(p,"Amy",18);
// 调用指定的对象的指定方法，返回方法调用后的返回值（若方法没有返回值，则返回null）
```





##### 获取运行时类的父类的泛型

```java
Class c = Person.class;	// 获取Class类的实例
Type t = c.getGenericSuperclass();		// 获取运行时类的 带泛型的父类

ParameterizedType pt = (ParameterizedType)t;	// 若父类带了泛型，则可以强制类型转换
Type[] ts = pt.getActualTypeArguments();	// 获取泛型参数（可能多个）

Class ct = (Class)ts[0];		// 获取指定泛型类型，并强转为Class类型（为了调用方法）
String name = ct.getName();		// 获取泛型名称
```





##### 获取注解的值

```java
// 自定义注解，参考：SuppressWarnings
@Target({TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface MyClassAnnotation {
    String name() default "null";
}

// 自定义注解
@Target({FIELD})
@Retention(RetentionPolicy.RUNTIME)
public @interface MyFieldAnnotation {
    String name() default "null";
    String type() default "null";
}

// 自定义类
@MyClassAnnotation(name = "Customer")
public class People {
    
    @MyFieldAnnotation(name = "customer_name", type = "varchar(11)")
    private String name;
    
    @MyFieldAnnotation(name = "customer_age", type = "int")
    private int age;
}
```



> 获取类上的

```java
Class clazz = People.class;			// 获取Class类对象
MyClassAnnotation mca = (MyClassAnnotation)clazz.getDeclaredAnnotation(MyClassAnnotation.class);
// 获取该运行时类所在的类所使用的注解 的操作对象
String s = mca.name();		// Customer
// 获取该注解上name属性的值，返回值类型由注解中定义的类型决定
```



> 获取类的属性的

```java
Class clazz = People.class;			// 获取Class类的实例
Field nameField = clazz.getDeclaredField("name");
// 获取Class类实例中 属性名为name的Field类型的操作对象
MyFieldAnnotation mfa= nameField.getDeclaredAnnotation(MyFieldAnnotation.class);
// 获取该运行时类的属性所使用的注解 的操作对象
String s1 = mfa.name();		// customer_name
String s2 = mfa.type();		// varchar(11)
// 获取该注解上name属性和type属性的值，返回值类型由注解中定义的类型决定
```









##### 其他常用方法

【Object】`newInstance()`：获取当前运行时类内部的空参构造器

>注意：
>
>1. 该方式获取的是空参的构造器
>2. 需要满足条件：
>   - 该运行时类必须提供空参的构造器
>   - 要求提供的空参构造器的权限是足够的
>
>已过时：要求过于苛刻



【Field[]】`getFields()`：获取 当前运行时类以及父类中 所有声明为public权限的属性

【Field[]】`getDeclaredFields()`：获取当前运行时类以及父类中所有的属性（包括私有）



【Method[]】`getMethods()`：获取 当前运行时类以及父类中 所有声明为public权限的方法

【Method[]】`getDeclaredMethods()`：获取当前运行时类以及父类中所有的方法

- 【Parameter[]】`Method对象.getParameters()`：获取方法的参数值

  > 需要再Settings --> Build,Execution,Deployment --> Compiler --> Java Compiler --> Additional command line parameters 中，添加 `-parameters`，让编译器在编译class文件时将参数名带进去



【ClassLoader】`getClassLoader()`：获取当前运行时类的类加载器

【String】`getName()`：获取运行时类的名称

【Class[]】`getInterfaces()`：获取运行时类实现的接口

【Package】`getPackage()`：获取运行时类所属的包



【Class】`getSuperclass()`：获取运行时类的父类

【Type】`getGenericSuperclass()`：获取运行时类的父类（带泛型）









## 新特性

### Lambda表达式

#### 知识点

只有为函数式接口提供实现类的对象时，才可以使用Lambda表达式

> 函数式接口：接口中仅声明了一个抽象方法（）
>
> - jdk8.0中声明的函数式接口，都位于java.util.function包下
>
> - 4种基本的函数式接口：
>
>   |  类型  |     接口      |   其内抽象方法    |        说明        |
>   | :----: | :-----------: | :---------------: | :----------------: |
>   | 消费型 |  Consumer<T>  | void accept(T t)  | 有传入值，无返回值 |
>   | 供给型 |  Supplier<T>  |      T get()      | 无传入值，有返回值 |
>   | 函数型 | Function<T,R> |   R apply(T t)    | 有传入值，有返回值 |
>   | 判断型 | Predicate<T>  | boolean test(T t) |  传入值，进行判断  |



1）Lambda表达式的格式：`lambda形参列表 -> lambda体`

- lambda形参列表：要重写的接口中的方法 的形参列表
- lambda体：实现类要实现接口中的抽象方法 的方法体

2）Lambda表达式的本质：

1. 作为接口的实现类对象
2. 匿名函数（匿名方式重写抽象方法）







#### 用法

最基本格式：lambda形参列表 -> lambda体

```java
// 匿名实现类：
Comparator<Integer> c1 = new Comparator<>(){
    @Override
    public int compare(Integer i1,Integer i2){
        System.out.println(i1+","+i2);
        return i1.compareTo(i2);
    }
};

// Labmda表达式：
Comparator<Integer> c2 = (Integer i1,Integer i2) -> {
  	System.out.println(i1+","+i2);
    return i1.compareTo(i2);
};
```



优化一（对lambda形参列表进行优化）：形参列表中的数据类型可以省略（类型推断）

```java
// 匿名实现类：
Comparator<Integer> c1 = new Comparator<>(){
    @Override
    public int compare(Integer i1,Integer i2){
        System.out.println(i1+","+i2);
        return i1.compareTo(i2);
    }
};

// Lambda表达式：
Comparator<Integer> c2 = (i1,i2) -> {
    System.out.println(i1+","+i2);
    return i1.compareTo(i2);
};
```



优化二（对lambda形参列表进行优化）：若形参列表仅有一个时，其()可以省略

```java
// 匿名实现类：
Consumer<String> c1 = new Consumer<>(){
    @Override
    public void accept(String s){
        System.out.println(s);
    }
};

// Lambda表达式：
Consumer<String> c2 = s -> {
    System.out.println(s);
};
```



优化三（对lambda体进行优化）：当方法体中仅有一条语句时，{}可以省略（若是return语句，则{}和return必须一并省略）

```java
// 匿名实现类：
Consumer<String> c1 = new Consumer<>(){
    @Override 
    public void accept(String s){
        System.out.println(s);
    }
};

// Lambda表达式：不含return的一条语句
Consumer<String> c2 = s -> System.out.println(s);
```

```java
// 匿名实现类：
Comparator<Integer> c1 = new Comparator<>(){
    @Override
    public int compare(Integer i1,Integer i2){
        return i1.compareTo(i2);
    }
};

// Lambda表达式：含return的一条语句
Comparator<Integer> c2 = (i1,i2) -> i1.compareTo(i2);
```



总结：

1. lambda形参列表
   - 参数类型可以省略
   - 若形参列表仅有一个，则()可以省略
2. lambda体
   - 若方法体中仅有一条执行语句，则{}可以省略（若是return语句，则 return 和 {} 要一并省略）









### 方法引用

#### 方法引用

使用要求：

1. 满足特定条件
2. 可看做是Lambda表达式的进一步简化，但方法内部仅能有一条执行语句



注意：形参列表或返回值类型相同，不一定要一模一样，自动拆箱/装箱、多态等情况的话也满足条件





##### 情况一

格式：`对象::实例方法`

条件：函数式接口中的抽象方法的形参列表和返回值类型  与 内部实现方法中用某个对象调用的（非静态）方法的形参列表和返回值类型 相同



> 举例一：

```java
// 匿名实现类：
Consumer<String> c1 = new Consumer<>(){
    @Override
    public void accept(String s){
    // accept抽象方法：返回值类型->void，形参列表->String
        System.out.println(s);		
        //（System.out对象调用）println实例方法：返回值类型->void，形参列表->String
    }
};

// 方法引用：
Consumer<String> c2 = System.out::println;		// System.out是PrintStream类的对象
```



> 举例二：

```java
class Person{
    private String name;
    public String getName(){
        return name;
    }
}

Person p = new Person();

// 匿名实现类：
Supplier<String> s1 = new Supplier<>(){
    @Override
    public String get(){			// get()返回值类型：String，形参列表：无
        return p.getName();			// getName()返回值类型：String，形参列表：无
    }
};

// 方法引用
Supplier<String> s2 = p::getName;
```







##### 情况二

格式：`类::静态方法`

条件：函数式接口中的抽象方法的形参列表和返回值类型  与 内部实现方法中用某个类调用的（静态）方法的形参列表和返回值类型 相同



> 举例一：

```java
// 匿名实现类：
Comparator<Integer> c1 = new Comparator<>(){
    @Override
    public int compare(Integer i1,Integer i2){	
    // compare抽象方法：返回值类型->int，参数列表->Integer、Integer
        return Integer.compare(i1,i2);
        // （Integer类调用）compare静态方法：返回值类型->int，参数列表->int、int
    }
};

// 方法引用：
Comparator<Integer> c2 = Integer::compare;
```



> 举例二：

```java
// 匿名实现类
Function<Double,Long> f1 = new Function<>(){
    @Override
    public Long apply(Double d){
    // apply抽象方法：返回值类型->Long，参数列表->Double
        return Math.round(d);
        //（Math类调用）round静态方法：返回值类型->long，参数列表->double
    }
};

// 方法引用：
Function<Double,Long> f2 = Math::round;
```







##### 情况三

格式：`类::实例方法`

条件：函数式接口中的抽象方法的返回值类型  与 内部实现方法中 用抽象方法的第一个参数调用的（非静态）方法 的返回值类型相同，并且抽象方法形参列表中的其他参数 与 实现方法中被调用的方法的形参列表中的参数类型都相同



> 举例一：

```java
// 匿名实现类
Comparator<String> c1 = new Comparator<>(){
    @Override
    public int compare(String s1,String s2){
    // compare抽象方法：返回值类型->int，形参列表->String、String
        return s1.compareTo(s2);
    	// 抽象方法第一个参数调用compareTo方法后的返回值类型，与抽象方法的返回值类型相同
    // 抽象方法形参列表个数 = compareTo方法形参列表个数 +1，且形参类型都为String（除第一个）
    }
};

// 方法引用
Comparator<String> c2 = String::compareTo;
```



> 举例二：

```java
// 匿名实现类
BiPredicate<String,String> b1 = new BiPredicate<>(){
    @Override
    public boolean test(String s1,String s2){
    // test抽象方法：返回值类型->boolean，形参列表->String、String
        return s1.equals(s2);
    	// 抽象方法第一个参数调用equals方法后的返回值类型，与抽象方法的返回值类型相同
   	   // 抽象方法形参列表个数 = equals方法形参列表个数 +1，且形参类型都为String（除第一个）
    }
};

// 方法引用
BiPredicate<String,String> b2 = String::equals;
```







#### 构造器与数组引用

构造器引用：`类名::new`

数组引用：`数组名[]::new`

> 调用了类中某个确定的构造器，而调用哪个构造器取决于 函数式接口中的抽象方法的形参列表

```java
class Person{
    private String name;
    public Person(){}
    public Person(String name){
        this.name=name;
    }
    public String getName(){
        return name;
    }
}

// 匿名实现类
Supplier<Person> s1 = new Supplier<>(){
    @Override
    public Person get(){
        return new Person();
    }
};
Function<String,Person> f1 = new Function<>(){
    @Override
    public Person apply(String s){
        return new Person(s);
    }
};

// 构造器引用：
Supplier<Person> s2 = Person::new;		
// Person是创建的构造器类型，所以调用的是无参构造器Person()

Function<String,Person> f2 = Person::new;
// Person是创建的构造器类型，所以调用的是有参构造器Person(String)
```

```java
// 匿名实现类：
Function<Integer,Person[]> f1 = new Function<>(){
    @Override
    public Person[] apply(Integer lenth){
        return new Person[lenth];
    }
};

// 数组引用：
Function<Integer,Person[]> f2 = Person[]::new;
// Person[]是创建的数组类型，所以Integer代表的是创建的数组的长度
```









### Stream类

#### 知识点

Stream的功能：是对数据进行一系列操作（排序、查找、过滤等）

> Stream对集合的操作，就类似于SQL对数据表的操作



使用流程：

1. 获取Stream的实例
2. 进行一系列操作
3. 执行终止操作



说明：

1. Stream并不会存储数据，而是对数据的操作
2. Stream不会改变源对象，而是返回一个新的Stream
3. Stream的一系列操作是延迟执行的，在有终止操作时才会执行。操作完成后，返回新的Stream
4. Stream一旦执行了终止操作，就不能再调用该Stream去执行其他操作了







#### 获取Stream实例

##### 通过集合获取

调用Collection接口中的默认方法：

1. 【Stream】`list对象.stream()`（顺序流）
2. 【Stream】`list对象.parallelStream()`（并行流）

```java
ArrayList<String> al = new ArrayList<>();
Stream<String> s = al.stream();
```





##### 通过数组获取

调用Arrays类中的静态方法：

- 【Stream】（static）`Arrays.stream(数组)`

```java
Integer[] i1 = new Integer[]{};
Stream<Integer> s1 = Arrays.stream(i1);

int[] i2 = new int[]{};
IntStream s2 = Arrays.stream(i2);
```





##### 通过内部静态方法

调用Stream内部的静态方法：

- 【Stream】（static）`Stream.of(Object...values)`

```java
Stream<String> s = Stream.of("AA","BB","CC");
```







#### 一系列操作

> 一系列操作是可以连续进行的，每个操作完成后返回的都是新的Stream操作对象

##### 过滤

1）`filter(Predicate p)`：过滤出来所有满足条件的元素

```java
List<Employee> list = Employees.getEmployee();	  // 获取集合，内含一个个Employee对象
list.stream().filter(emp -> emp.getSalary()>1000).forEach(System.out::println);
// 获取Stream实例，调用filter筛选出所有工资大于1000的，并遍历结果
```

> Predicate：（函数式接口）boolean test(T t);



2）`limit(int count)`：截取前n条数据

```java
List<Employee> list = Employees.getEmployee();	  // 获取集合，内含一个个Employee对象
list.stream().limit(2).forEach(System.out::println);
// 获取Stream实例，调用limit筛选出前2条数据，并遍历结果
```



3）`skip(int count)`：获取 跳过前n条数据后 的数据

```java
List<Employee> list = Employees.getEmployee();	  // 获取集合，内含一个个Employee对象
list.stream().skip(2).forEach(System.out::println);
// 获取Stream实例，调用limit筛选出除了前2条之外的所有数据，并遍历结果
```



4）`distinct()`：去除重复数据

> 根据元素所在的类的 hashCode() 和 equals() 来判断数据是否重复

```java
List<Employee> list = Employees.getEmployee();	  // 获取集合，内含一个个Employee对象
list.stream().distinct().forEach(System.out::println);
// 获取Stream实例，调用distinct去除掉所有重复的数据，并遍历结果
```





##### 映射

`map(Function f)`：提取所有元素的信息，或对所有元素进行操作

```java
List<Employee> list = Employees.getEmployee();	  // 获取集合，内含一个个Employee对象
list.stream().map(emp -> emp.getName()).filter(name -> name.length()>3).forEach(System.out::println);
// 获取Stream实例，取出所有的姓名，从中筛选出名字长度大于3的，并遍历结果
```

```java
List<String> list = Arrays.asList("aa","bb","cc");	 //创建集合
list.stream().map(str -> str.toUpperCase()).forEach(System.out::println);
// 获取Stream实例，将所有字符串进行大写的转换，并遍历结果
```

> Function：（函数式接口）R apply(T t);





##### 排序

1）`sorted()`：对元素进行自然排序

> 元素所在的类必须实现Comparable接口

```java
int[] i = new int[]{1,3,2};			// 创建数组
Arrays.stream(i).sorted().forEach(System.out::println);
// 获取Stream实例，将所有元素进行自然排序，并遍历结果
```



2）`sorted(Comparator c)`：对元素进行定制排序

```java
List<Employee> list = Employees.getEmployee();	  // 获取集合，内含一个个Employee对象
list.stream().sorted((e1,e2) -> -(e1.getAge()-e2.getAge())).forEach(System.out::println);
// 获取Stream实例，按年龄从大到小进行定制排序，并遍历结果
```

> Comparator：（函数式接口）int compare(T o1, T o2);







#### 终止操作

> 可以没有一系列操作而直接终止操作

##### 判断

1）【boolean】`allMatch(Predicate p)`：判断所有的元素是否都满足条件

```java
List<Employee> list = Employees.getEmployee();	  // 获取集合，内含一个个Employee对象
System.out.println(list.stream().allMatch(emp -> emp.getSalary()>1000));
// 获取Stream实例，调用allMatch判断所有的元素是否都满足条件，并输出结果
```

> Predicate：（函数式接口）boolean test(T t);



2）【boolean】`anyMatch(Predicate p)`：判断所有的元素中，是否有至少一个元素满足条件

```java
List<Employee> list = Employees.getEmployee();	  // 获取集合，内含一个个Employee对象
System.out.println(list.stream().anyMatch(emp -> emp.getSalary()>1000));
// 获取Stream实例，调用allMatch判断所有的元素中，是否至少有一个元素满足条件，并输出结果
```

> Predicate：（函数式接口）boolean test(T t);





##### 获取

1）【Optional】`findFirst()`：返回第一条数据

```java
List<String> list = Arrays.asList(new String[]{"aa","cc","dd"});	// 创建集合
System.out.println(list.stream().findFirst());
// 获取Stream实例，调用findFirst获取第一条数据，并输出结果
```



2）【long】`count()`：获取数据的条数

```java
int[] i = new int[]{1,2,3,4,5};				// 创建数组
System.out.println(Arrays.stream(i).count());
// 获取Stream实例，调用count获取元素的个数，并输出结果
```



3）【Optional】`max(Comparator c)`：返回元素中的最大值

> 根据Comparator的比较规则进行比较排序后，选出最右边的值作为最大值

```java
List<Employee> list = Employees.getEmployee();	  // 获取集合，内含一个个Employee对象
System.out.println(list.stream().max((e1,e2) -> Double.compare(e1.getSalary(),e2.getSalary())));
// 获取Stream实例，调用max选出工资最高的，并输出结果
```

>Comparator：（函数式接口）int compare(T o1, T o2);



4）【Optional】`min(Comparator c)`：返回元素中的最小值

> 根据Comparator的比较规则进行比较排序后，选出最左边的值作为最小值

```java
List<Employee> list = Employees.getEmployee();	  // 获取集合，内含一个个Employee对象
System.out.println(list.stream().min((e1,e2) -> Double.compare(e1.getSalary(),e2.getSalary())));
// 获取Stream实例，调用min选出工资最低的，并输出结果
```

>Comparator：（函数式接口）int compare(T o1, T o2);



5）`forEach(Consumer c)`：进行内部的迭代

```java
int[] i = new int[]{1,2,3,4,5};			// 创建数组
Arrays.stream(i).forEach(System.out::println);
// 获取Stream实例，调用forEach遍历所有元素
```

> Consumer：（函数式接口）void accept(T t);





##### 归约

1）【Optional】`reduce(BinaryOperator b)`：将所有元素反复结合起来

```java
List<Employee> list = Employees.getEmployee();	  // 获取集合，内含一个个Employee对象
System.out.println(list.stream().map(emp -> emp.getSalary()).reduce((s1,s2) -> Double.sum(s1,s2)));
// 获取Stream实例，调用map获取所有的工资，调用reduce求工资的总和，并输出结果
```

> BinaryOperator：（函数式接口）R apply(T t, U u);



2）【T】`reduce(T identity,BinaryOperator b)`：指定初始值，再将所有的元素反复结合起来

```java
List<Integer> list = Arrays.asList(1,2,3,4,5);		// 创建集合
System.out.println(list.stream().reduce(10,(i1,i2) -> i1+i2));
// 获取Stream实例，调用reduce求 原先集合中所有元素和+10 的总和，并输出结果
```

> BinaryOperator：（函数式接口）R apply(T t, U u);





##### 保存

【Object】`collect(Collector c)`：接收一个Collector接口的实现，对一系列操作后的结果进行保存

```java
List<Employee> list = Employees.getEmployee();	  // 获取集合，内含一个个Employee对象
List<Employee> list1 = list.stream().filter(emp -> emp.getSalary()>1000).collect(Collectors.toList());
// 获取Stream实例，调用fileter筛选出所有工资大于1000的，并将结果保存到一个新的集合中
```







### Optional类

作用：容器之一，用于避免空指针异常

【Optional\<T>】（static）`ofNullable(T obj)`：创建一个Optional的实例，将形参作为内部属性value的值

【T】`Optional对象.orElse(T obj)`：若Optional对象的内部属性value值为null，则替换为obj对象

【T】`Optional对象.get()`：取出Optional对象的内部属性value的值，若为null，抛出异常

使用举例：

```java
String num1 = "abc";
String num2 = "cba";
Optional<String> o = Optional.ofNullable(num1);
String num = o.orElse(num2);
// 若在调用ofNullable()时存放的num1为 null，则在调用orElse()时，就会被替换为num2的值
// 这样就可以确保调用的num一定是有值的，从而可以避免空指针异常
System.out.println(num);
```









### 其他

#### try-catch

> jdk7.0

将要关闭资源的对象声明在 try 后面的 () 中，无论是否出现异常，都会自动关闭资源

```java
try( FilterWriter fw = new FilterWriter(new File("abc.txt"));
	BufferedWriter bw = new BufferedWriter(fw);
){
    bw.write("hello,world");
}catch(IOException e){
    e.printStackTrace();
}
```



> jdk9.0

相较于jdk7.0，可以将对象声明在外部，try() 内指明要关闭的对象即可

```java
FilterWriter fw = new FilterWriter(new File("abc.txt"));
BufferedWriter bw = new BufferedWriter(fw);
try(fw;bw){
    bw.write("hello,world");
}catch(IOException e){
    e.printStackTrace();
}
```



注意：

1. 自动关闭资源的类，必须直接或间接的实现AutoCloseable接口中的close()方法
2. 声明在try()中的对象，是被final修饰的，其值不能再被修改







#### var

> 类型推断

常用于：

1. 局部变量的实例化，如：`var list = new ArrayList<String>();`
2. 增强for循环中的索引，如：`for(var v : 容器){}`
3. 返回值类型确定的结构，如：`var entry = Map实现类对象.entrySet();`







#### instanceof

> 由于instanceof判断是否满足多态关系后，一般都会进行强转并进行调用，因此可以考虑省去转换这一步

```java
public boolean equals(Object o){
	if(o instanceof Computer c){
    	return this.getSalary() - c.getSalary();
	}
    throw new RuntimeException("error");
}
// 若o满足多态关系，则直接强转为Computer类型，并赋值给变量c
```







#### switch

`->`：用来代替 break，防止忘记添加break而导致case穿透

```java
String s = "AA";
switch(s){
    case "AA"-> System.out.println("AA");
    case "BB"-> System.out.println("BB");
    case "CC","DD"->System.out.println("CD");
}
// 不需要使用break的地方，则用,隔开
```



> jdk12.0

可以使用变量接收switch表达式的结果

```java
int day = 6;
String res = switch (day) {
    case 1 -> "周一";
    case 2 -> "周二";
    case 3 -> "周三";
    case 4 -> "周四";
    case 5 -> "周五";
    default -> "假日";
};
System.out.println(res);
```



> jdk13.0

`yield`：使用yield关键字可以返回指定的数据，结束switch结构

```java
String season = "春";
String res = switch (season){
    case "春":{
        System.out.println("阳光明媚");
        yield "3~5月";
    }
    case "夏":{
        System.out.println("夏日炎炎");
        yield "6~8月";
    }
    case "秋":{
        System.out.println("秋高气爽");
        yield "9~11月";
    }
    case "冬":{
        System.out.println("白雪皑皑");
        yield "12~2月";
    }
    default:{
        throw new RuntimeException();
    }
};
System.out.println(res);
```







#### record

> 用于省略构造器、toString、get、set等一系列重复的操作

使用举例：

```java
public record Person(int id,String name){}
// 定义后，该类就会默认生成：
// 带id和name的参数的构造器、get/set方法、toString、hashCode、equals等结构
```

注意：

1. 可以在record中声明：构造器（需调用 `this(参数列表)` ）、静态字段、静态/实例方法
2. 不可以在record中声明：实例字段
3. record中的所有结构都被final修饰（包括类本身，因此不能被继承）
4. 不能显示的声明父类（已继承Record类）







#### sealed

作用：使指定的类可以被继承，而其他类不行

使用举例：

```java
public sealed class Person permits Student,Teacher,Worker{}
// 要求：指定的子类必须是三种类型之一：final、sealed、non-sealed

final class Student extends Person{}
// 使用final修饰的类不能被继承

sealed class Teacher extends Person permits ChineseTeacher,EnglishTeacher{}
// 被sealed修饰的类必须指定允许被继承的子类

non-sealed class Worker extends Person{}
// 被non-sealed修饰的类无任何要求，其类也可以被继承
```







#### 其他

`""" 文本信息 """`：用三个"来表示该字符串为文本信息，输出时按原样输出

- 可在需要的地方，用`\`来取消换行，用`\s`表示空格

> 比如：文本信息中进行换行时，输出的结果也会换行











## 数据结构

### 知识点

#### 关系

1）逻辑关系：

1. 集合结构：包含关系
2. 线性结构：一对一
3. 树形结构：一对多
4. 图形结构：多对多

2）物理结构：

1. 顺序结构
2. 链式结构
3. 索引结构
4. 散列结构

3）开发角度理解：

1. 线性表（一对一）：一维数组、单向/双向链表、栈、队列
2. 树（一对多）：各种树，如二叉树、B+树等
3. 图（多对多）
4. 哈希表：如HashMap、HashSet等





#### 概念

##### 树形结构中的概念

点（结点）：树中的每一个元素

根节点：最上层的节点称为根节点（每个节点都可以是其子树的根节点）

父节点：当前节点的上一层节点

子节点：当前节点的下一层节点

兄弟节点：具有相同父节点的节点

节点的度数：节点拥有的子树的个数

树叶（终端节点）：度数为0的节点

非终端节点（分支节点）：树叶以外的节点（度数不为0的节点）

树的深度（高度）：树中节点的最大层次树

节点的层数：从根节点到树中某个节点 之间所经的分支节点数，称为节点的层数

同代：树中拥有相同层数的节点





##### 二叉树的遍历

前序遍历：中左右（根左右）

- 先遍历根节点，在遍历左子树（若左子树也有子树的话，依旧先遍历左子树，依次类推，再遍历左子树的右子树，依次遍历完），再遍历右子树（若右子树也有左子树的话，依旧先遍历左子树，再遍历右子树）



中序遍历：左中右（左根右）

- 先遍历左子树，直到父节点，再遍历父节点下的右子树，遍历完右子树后，再遍历左子树 - 父节点 - 右子树，依次类推



后序遍历：左右中（左右根）

- 先遍历左子树，再遍历右子树，在访问父节点，再依次遍历 左子树 - 右子树 - 父节点









### 结构模型

#### 链表

##### 单向链表

```java
class Node{
    Object data;		// 数值
    Node next;			// 存放下一个节点
    public Node(Object data){
        this.data = data;
    }
}

Node node1 = new Node("A");
Node nextNode = new Node("B");
node1.next = nextNode;				// 修改node1的下一个节点为nextNode
```



##### 双向链表

```java
class Node{
    Node prev;			// 存放上一个节点
    Object data;		// 数值
    Node next;			// 存放下一个节点
	public Node(Object data){
        this.data = data;
    }
}

Node node1 = new Node("A");
Node node2 = new Node("B");
Node node3 = new Node("C");
node1.next = node2;			// 修改node1的下一个节点为node2
node2.prev = node1;			// 修改node2的上一个节点为node1
node2.next = node3;			// 修改node2的下一个节点为node3
node3.prev = node2;			// 修改node3的上一个节点为node2
```







#### 二叉树

```java
class TreeNode{
    TreeNode parent;		// 存放父节点
    TreeNode left;			// 存放左边子节点
 	Object data;			// 数值
    TreeNode right;			// 存放右边子节点
    public TreeNode(Object data){
        this.data=data;
    }
}

TreeNode node = new TreeNode("A");
TreeNode leftNode = new TreeNode("B");
TreeNode rightNode = new TreeNode("C");
node.left = leftNode;
node.right = rightNode;
```







#### 栈

> 抽象数据类型（ADI）：通过控制模拟出的效果，可使用数组或链表模拟
>
> 特点：先进后出

```java
class Stack{
    Object[] values;		// 存放数值
    int size;				// 记录当前数据的存放个数
    public Stack(int length){
        values = new Object[length];		// 传入数组长度，对数组进行初始化
    }
    
    // 提供两个方法，让添加或删除数据时，体现为入栈或出栈的效果  
    public void push(Object value){		// 入栈
        if(size >= values.length){
            throw new RuntimeException("栈空间已满");
        }
        values[size++] = value;
    }
    
    public Object pop(){				// 出栈：控制出数组中的最后一个数值
        if(size <= 0){
            throw new RuntimeException("栈空间无数据");
        }
        Object value = values[size-1];		// 取出数组中最后一个数值，存放在变量中
        values[--size]=null;				// 取出后，将该数值置为null，数组长度-1
        return value;						// 返回：存放了最后一个数值的变量
    }					 
}
```







#### 队列

>抽象数据类型（ADI）：通过控制模拟出的效果，可使用数组或链表模拟
>
>特点：先进先出

```java
class Queue{
    Object[] values;		// 存放数值
    int size;				// 记录当前数据的存放个数
	public Queue(int length){
        values = new Object[length];		// 传入数组长度，对数组进行初始化
    }
    
    // 提供两个方法，让添加或删除数据时，体现为入队列和出队列的效果
    public void push(Object value){		// 入队列			
        if(size >= values.length){
            throw new RuntimeException("队列已满");
        }
        values[size++] = value;
    }
 	
    public Object get(){				// 出队列：控制出数组中的第一个数值
        if(size <= 0){
            throw new RuntimeException("队列中无数据");
        }
        Object value = values[0];			// 取出数组中第一个数值，放在一个变量中
        for(int i=0;i<values.length-1;i++){	// 循环将数值往前移（置换掉第一个数值）
            values[i] = values[i+1];			
        }
        values[--size] = null;				// 将最后一个数值置为null，数组长度-1
        return value;						// 返回存放了第一个数值的变量
    }
}
```

