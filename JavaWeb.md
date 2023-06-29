# 前端

## HTML

> HTML决定页面的构造

### 知识点

1. HTML页面的构成：

   ```html
   <html>
       <head>
       	<!-- 页面的头部分 -->
       </head>
       
       <body>
       	<!-- 页面的内容部分 -->
       </body>	
   </html>
   ```

2. HTML是解释型语言，因此浏览器的容错率高

3. 不区分大小写







### head

#### 单标签

##### 字符集

`<meta>`：字符集设置标签（需要配合属性使用）

- `charset`：必选，设置页面的字符集
  - UTF-8：设置编码格式为UTF-8
  - GBK：设置编码格式为GBK



##### 路径

`<base/>`：设置路径基础值（需要配合属性使用）

- `href`：必选，设置当前页面上 所有的路径都以该路径作为基础





#### 双标签

##### 标题

`<title></title>`：设置页面的标题







### body

#### 单标签

##### 图片

`<img/>`：图片标签（需要配合属性使用）

- `src`：必选，设置图片的路径（相对或绝对路径）
- `width`、`height`：设置图片的宽度、高度
- `alt`：鼠标停留、图片显示失败时，显示的文本信息







##### 其他

###### 换行

`<br/>`：换行



###### 分隔

`<hr/>`：分割线





#### 双标签

##### 标题

`<h1></h1>`：以标题的形式显示文本（共6档，即：\<h1> ~ \<h6>）





##### 列表

列表标签：

1. `<ol></ol>`：有序列表，显示列表每列的顺序
   - `type`：显示顺序的类型
     - 1：以数字的形式显示（默认）
     - A、a：以字母的形式显示
     - I、i：以罗马数字的形式显示
   - `start`：表示从指定的顺序开始
2. `<ul></ul>`：无序列表，不显示列表每列的顺序
   - `type`：不显示顺序的类型
     - disc：圆饼状（默认）
     - circle：圆圈状
     - square：方形状



`<li></li>`：列表中的每一行（需要使用在列表标签中）





##### 超链接

`<a></a>`：超链接标签（需要配合属性使用）

- `href`：必选，为文本指定跳转的超链接地址（相对路径或绝对路径）
- `target`：指定链接的打开方式
  - _self：在当前窗口打开
  - _blank：在新窗口打开
  - _parent：在父窗口打开
  - _top：在顶级窗口打开





##### 表格

`<table></table>`：表格标签

- `border`：设置边框厚度
- `width`、`height`：设置表格宽度、高度
- `cellspacing`：设置单元格间的空隙
- `cellpadding`：对单元格内部进行填充



`<tr></tr>`：表示表格中的一行（需要使用在表格标签中）

- `align`：单元格中内容的对齐方式
  - left：靠左对齐（默认）
  - right：靠右对齐
  - center：居中显示



`<th></th>`：表示表格一行中的一列，样式为加粗居中（需要使用在表格行标签中）

`<td></td>`：表示表格一行中的一列（需要使用在表格行标签中）

- `rowspan`：单元格跨越的行数（行合并）
- `colspan`：单元格跨越的列数（列合并）





##### 表单

`<form></form>`：表单标签

- `action`：指定表单提交的地址（请求路径）

  > 若没有指定提交的地址，则默认提交至当前页面

- `method`：指定表单提交的方式
  - get：使用get的方式提交表单
  - post：使用post的方式提交表单



`<input/>`：单标签，表示一个文本框

- `type`：指定文本框的类型

  - text：文本框（默认）

    > 文本框输入的内容即为value属性值

  - password：密码框

  - radio：单选框

    > 只有name属性值一致的单选框，才具有互斥效果
    >
    > 由value属性值来确定单选框的选项
    >
    > 使用`checked="checked"`可以设置该项为默认选项

  - checkbox：复选框

  - submit：提交按钮，用于提交表单数据

    > 不需要name属性值
    >
    > value属性值决定按钮的文本

  - reset：重置按钮，用于还原表单为默认值

  - button：普通按钮

  - hidden：隐藏域，功能与文本框类似，但不可见

- `name`：将数据发送给服务器时必须指定的属性，对应提交的表单数据中的 key

- `value`：对应提交的表单数据中的 value

- `size`：设置文本框长度（由字符数决定文本框长度）

- `readonly`：设置文本框为只读状态（当属性名与属性值一致时，属性值可以省略）

- `placeholder`：设置提示文本

- `disabled`：设置文本框操作状态

  - disabled：设置文本框不可操作
  - true：设置文本框不可操作



`<textarea></textarea>`：多行文本框（起始与结束标签间的文本内容为value属性值）

- `name`：将数据发送给服务器时必须指定的属性，对应提交的表单数据中的 key
- `rows`：指定文本框的最大行数
- `cols`：指定文本框中每行的最多列数



`<select></select>`：下拉框标签

- `name`：将数据发送给服务器时必须指定的属性，对应提交的表单数据中的 key

`<option></option>`：下拉框中的每一个选项（需要使用在下拉框标签中）

- `value`：对应提交的表单数据中的 value
- `selected`：默认选中的选项
  - selected：属性名与属性值一致时，属性值可以省略





##### 窗口

`<iframe/>`：窗口标签（需要配合属性使用）

- `src`：必选，设置窗口显示的内容的路径（相对路径或绝对路径）
- `frameborder`：设置是否显示窗口边框
  - 0：不显示
  - 1：显示（默认）
- `scrolling`：设置是否显示滚动条
  - yes：显示
  - no：不显示
  - auto：自动（默认）





##### 其他

###### 段落

`<p></p>`：以段落的形式显示文本（段落间空隙更大）



###### 文本样式

`<u></u>`：用下划线标记文本

`<b></b>`：用粗体标记文本

`<i></i>`：用斜体标记文本



###### 角标

`<sup></sup>`：以上角标的方式显示文本

`<sub></sub>`：以下角标的方式显示文本



###### 标记

`<span></span>`：标记该文本



###### 层

`<div></div>`：表示一层







#### 字符样式

`&nbsp;`：表示空格

`&gt;`：表示大于号

`&lt;`：表示小于号

`&ge;`：表示大于等于

`&le;`：表示小于等于

`&reg;`：表示注册商标

`&copy;`：表示版权符号







### frameset

> 替换body，用于表示页面框架，已淘汰

```html
<frameset rows="占比,占比" frameborder="no">
<!-- 
① rows：用于分割当前页面，如：rows="20%,*"，表示将整体分割为两部分，一部分占比20%，其余占比80%
② frameborder：设置是否显示分割的边框
--> 
	<frame src="页面路径"/>
    <!-- 用于显示指定的页面 -->
</frameset>
```









## CSS

> CSS修饰页面的样式

### 使用方式

1. 标签中定义：在标签中使用`style`属性

2. \<head>标签中定义：

   ```html
   <style type="text/css">
   	/* CSS代码 */
   </style>
   ```

3. \<head>标签中导入外部文件：

   ```html
   <link rel="stylesheet" href="CSS文件路径">
   ```







### 样式表分类

1. 标签样式表：根据标签名来定位

   ```css
   标签名{
       /* css样式 */
   }
   ```

2. 类样式表：根据标签中 class属性 的值来定位

   ```css
   .class属性值{
       /* css样式 */
   }
   ```

3. ID样式表：根据标签中 id属性 的值来定位

   ```css
   #id属性值{
       /* css样式 */
   }
   ```

4. 组合样式表：根据多种样式表来定位

   ```css
   样式表1 样式表2{
       /* css样式 */
   }
   ```


> 样式表中，可以使用：`[属性名=属性值]`，来更精确的进行筛选







### 常用样式

#### 字体

`font-size`：设置字体大小（px）

`font-family`：设置字体样式

`color`：设置字体颜色



`text-align`：设置字体对齐方式

- center：居中



`font-weight`：设置字体粗细

- bolder：粗体
- lighter：细线



`font-style`：设置字体斜度

- italic：斜体





#### 超链接

`text-decoration`：设置超链接的下划线

- none：无下划线





#### 背景

`background-color`：设置背景颜色

- transparent：透明





#### 尺寸

`width`：设置宽度（px、%）

`height`：设置高度（px、%）

`line-height`：设置行高（px）





#### 边框

`border-collapse`：设置边框

- `collapse`：合并边框



`border-radius`：设置边框半径（px）





#### 定位

`position`：标签定位的方式

- absolute：绝对定位，配合 left（x轴）、top（y轴） 来确定标签的位置

- relative：相对定位，配合 float（漂浮）、margin、padding等 来确定标签的位置

  > float：将当前行作为参照物，使标签漂浮，而不会独占一行（如：div标签）
  >
  > - left：左漂浮
  > - right：右漂浮





#### 可视

`visibility`：标签是否可视

- hidden：隐藏
- visible：可视





#### 盒子模型

> 去除浏览器边角：padding为0、margin为0
>
> 不同浏览器兼容性不同，如：IE = width、chrome = width + border + padding

##### 边框

`border`：设置边框（4条边）的厚度、形状、颜色

> `border-top`：设置顶边
>
> `border-bottom`：设置底边
>
> `border-left`：设置左边
>
> `border-right`：设置右边

- 厚度：px

  > 等价于使用：`border-width`

- 形状：solid（实线）、dotted（点状线）、double（双实线）

  > 等价于使用：`border-style`

- 颜色

  > 等价于使用：`border-color`







##### 间距

`margin`：设置指定标签在其父标签中，距离4条边的间距（px、%）

> `margin-top`：距离顶边
>
> `margin-bottom`：距离底边
>
> `margin-left`：距离左边
>
> `margin-right`：距离右边

- 一个值时，表示设置距离4条边的间距
- 两个值时，表示分别距离上下边、左右边的间距
- 三个值时，表示分别距离上边、左右边、下边的间距
- 四个值时，表示分别距离上边、右边、下边、左边的间距（顺时针）





##### 填充

`padding`：从边开始，往内部进行填充（px、%）

> `padding-top`：填充内部顶部
>
> `padding-bottom`：填充内部底部
>
> `padding-left`：填充内部左边
>
> `padding-right`：填充内部右边

- 一个值时，表示填充4边
- 两个值时，表示填充上下边、左右边
- 三个值时，表示填充上边、左右边、下边
- 四个值时，表示填充上边、右边、下边、左边









## JavaScript

> JavaScript提供页面特效

### 使用方式

1. \<head>标签中定义

   ```html
   <script laughter="text/javascript">
   	// js代码
   </script>
   ```

2. \<head>标签中导入外部文件

   ```html
   <script type="text/javascript" src="js文件路径"></script>
   ```





### 语法

#### 变量

```javascript
var 变量名 = 值;
```

1. 基本数据类型
   - 数值型：不区分整数、小数
   - 字符串：不区分字符、字符串
   - 布尔型：true（非0、非空字符串或对象）/ false（0、空字符串、null、undefined）
2. 引用数据类型
   - 所有new出来的对象
   - 用[]声明的数组
   - 用{}声明的对象





#### 方法

```javascript
function 方法名(参数列表){
    方法体;
}
// 由于是弱类型语言，因此没有返回值类型、参数列表无类型
// 调用方法时，可以传递多个参数，无需按照参数列表来传值

function(){
    方法体;
}
// 匿名方法
```





#### 对象

##### 普通对象

方式一：以new的方式创建

```javascript
var 变量名 = new Object();
// 创建一个对象

变量名.属性名 = 属性值;
// 定义属性或修改对象的属性值

变量名.方法名 = function(){
    方法体;
};
// 定义方法或修改方法的方法体

变量名.属性名;		// 调用对象的属性
变量名.方法名();		// 调用对象的方法
```



方式二：以键值对的方式创建

```javascript
var 变量名 = {
    属性名 : 属性值,			// "属性名" : 属性值,
    方法名 : function(){	  // "方法名" : function(){}
        方法体;
    }
}
变量名.属性名;		
变量名.方法名();
```







##### JSON对象

```javascript
// 定义JSON格式对象
var ele = { 
    "key1":"value1" , 
    "key2":"value2" 
};

ele.key1;		// 获取JSON格式对象的属性
```

【String】`JSON.stringify(jsonObj)`：将JSON对象转换为普通字符串

【Object】`JSON.parse(String)`：将JSON格式的String字符串转换为JSON对象









#### 正则表达式

##### 使用规则

1. 定义正则表达式对象
   - 对象形式：`var reg = new RegExp("内容")`
   - 直接量形式：`var reg = /内容/`
2. 定义要被校验的字符串
3. 调用正则表达式对象中的方法，对字符串进行校验
   - `正则表达式对象.test(被校验字符串)`：匹配内容是否在指定字符串中出现
   - `被校验字符串.replace(正则表达式对象,"替换的字符串")`：将被校验的字符串中，满足正则表达式规则的，替换为指定的字符串

```javascript
var reg = /abc/;
var str = "helloabcworldabc";

var res1 = reg.test(str);		// true
var res2 = str.replace(reg,"_");	// hello_worldabc
```





##### 匹配模式

1. 全局匹配：g（在被校验的字符串中，匹配全部满足正则表达式内容的，而不会匹配到第一个后就停止）
2. 忽略大小写匹配：i
3. 多行匹配：m（遇到`\n`会认为是换行，在配合`$`等时可判断每行是否以xxx字符串结尾）

```javascript
var reg = /abc/img;			// var reg = new RegExp("abc","mig");
var str = "helloabcworldABC";

var res = str.replace(reg,"_");		// hello_world_
```

> 三者可组合使用，且不区分先后顺序





##### 元字符

> 对修饰字符进行解释的字符

`.`：表示除换行符之外的任意字符

`\b`：锁定每个单词的开始和结尾（_连接表示一个单词）

`^`：匹配是否以指定字符串开始

`$`：匹配是否以指定字符串结束

`\w`：匹配所有的字母、数字、下划线（等价于`[a-zA-Z0-9_]`）

- `\W`：与`\w`正好相反

`\s`：匹配任意空白符（包括空格，制表符，换页、换行符等，等价于`[\f\n\r\t\v]`）

- `\S`：与`\s`正好相反

`\d`：匹配数字（等价于`[0-9]`）

- `\D`：与`\d`正好相反





##### 字符集合

`[字符列表]`：若包含字符列表中任意一个字符的，则匹配

- ```javascript
  var reg = /[abc]/;
  var str = "helloaworld";
  var res = reg.test(str);	// true
  ```

`[^字符列表]`：若出现不包含字符列表中指定字符的，则匹配

- ```javascript
  var reg = /[^abc]/;
  var str = "abc";
  var res = reg.test(str);	// false
  ```

`[字符范围]`：如[a-z]，表示由所有小写字母组成的字符列表





##### 出现次数

1. `*`：表示出现零次或多次
2. `?`：表示出现零次或一次
3. `+`：表示出现一次或多次
4. `{n}`：表示出现n次
5. `{n,}`：表示至少出现n次
6. `{n,m}`：表示至少出现n次，至多出现m次

```javascript
var str = "helloworld";

var reg1 = /a*/;
var res1 = reg1.test(str);		// true

var reg2 = /a+/;
var res2 = reg2.test(str);		// false
```











### 常见事件

> 绑定事件的两种方式：
>
> 1. `<标签名 事件="方法名()">` 的方式在标签内绑定
> 2. `对象.事件 = 方法名;` 的方式通过对象来绑定
>
> 绑定事件时：
>
> - `事件 = "return false"`：表示事件不会触发
> - `事件 = "return true"`：表示事件会被触发

`onmouseover`：鼠标悬浮时触发事件

`onmouseout`：鼠标离开时触发事件

`onload`：页面加载完成时触发事件

`onclick`：单击时触发事件

`onblur`：失去焦点时触发事件

`onkeydown`：当从键盘输入时触发事件

`onsubmit`：表单提交时触发事件









### 常见对象

#### event

> 当前触发事件的对象

##### 属性

`srcElement`、`target`：获取响应事件的元素

> srcElement 拥有事件的传递性，可以获取到响应事件的父元素（通过继续调用来获取）
>
> 而 target 不存在事件的传递性，每次获取到的都是响应事件的元素



`keyCode`：获取从键盘输入的值的ASCII

>0~9（48~57）、backspace（8）、enter（13）



`returnValue`：事件的执行状况

- true：正常执行事件
- false：事件取消





##### 方法

`preventDefault()`：取消默认行为（如：超链接的跳转、表单的提交 等）







#### 标签

> 标签对象

##### 常规

###### 属性

`tagName`：获取标签名

`style`：获取标签的样式对象

`innerText`：获取标签内部的文本

`innerHTML`：获取标签内部的HTML

`parentElement`：获取父标签





##### table标签

###### 属性

`cells`：获取\<tr>标签中所有的单元格构成的数组

`rows`：获取\<table>标签中所有的行构成的数组

`rowIndex`：获取该\<tr>标签在rows中的下标





###### 方法

`deleteRow(rowIndex)`：删除\<table>标签下指定的行



`insertRow(index)`：在\<table>标签下的指定位置处插入一行，返回一个\<tr>标签对象

`insertCell(index)`：在\<tr>标签内的指定位置处插入一个单元格，返回一个\<td>标签对象

> 默index不填写时，在末尾处插入









##### input标签

###### 属性

`value`：获取文本框中的value值



###### 方法

`select()`：选择文本框中的文本

`blur`：使失去焦点







##### form标签

###### 属性

`action`：获取form表单提交的地址



###### 方法

`submit()`：提交表单







#### 节点

> 文本或标签都算是一个节点对象

##### 属性

`firstChild`：获取第一个子节点对象



`nodeType`：获取节点的类型

- ElementNode：1
- TextNode：3







#### style

> 标签的样式对象

##### 属性

`backgroundColor`：获取背景颜色

`color`：获取字体颜色



`cursor`：获取光标的形状

- hand：手状



`display`：获取显示状态

- inline：显示
- none：隐藏



`visibility`：获取标签显示状态

- hidden：隐藏
- visible：显示







#### 数组

##### 属性

`length`：获取数组的长度







#### window

> 当前窗口对象

##### 属性

`document`：获取页面HTML文档对象

`console`：获取控制台对象

`location`：获取地址栏对象





##### 方法

`alert(stringText)`：弹窗，只有确定按钮

`confirm(stringText)`：弹窗，有确定或取消按钮（确定时返回true，取消时返回false）







#### document

> 当前窗口的HTML文档对象

##### 方法

`getElementById(idName)`：获取指定id的标签的对象

`write(内容)`：向页面上输出内容





#### console

##### 方法

`log(variable)`：在控制台中打印内容





#### location

##### 属性

`href`：设置地址栏的值

> 带根路径（直接设置请求地址）







#### 其他

##### 方法

`typeof(variable)`：查看数据类型

`parseInt(stringValue)`：将字符串转换为数值类型











## Vue

### 使用步骤

```html
<!-- 1.导入vue.js文件 -->
<script type="text/javascript" src="vue.js"></script>

<script type="text/javascript">
    window.onload = function(){
        
        // 2.创建一个vue对象
        var ele = new Vue({

            // 3.根据id（#）、class（.）等，绑定标签
            "el":"#id值",			// el:"#id值"

            // 对该标签内部的节点进行其他操作
        });
    }
</script>
```





### 定义属性方法

```html
<script>
    window.onload = function(){
        var ele = new Vue({
            "el":"div",
            
            // 定义属性
            "date":{
                变量名1:值,
                变量名2:值
            },
            
          	// 定义方法
            "methods":{
    // methods中绑定的方法，其function()中的第一个参数，必定是触发当前方法的事件（event）对象
                "方法名":function(){		
                    方法体;
                }
            }
            
    	});
    }
</script>
 
<div>
    
    <p>{{变量名1}}</p>
    <!-- 文本中读取变量 -->      

    <p v-on:事件名="方法名"></p>
    <!-- 调用方法 -->
    
</div>
```





### 属性值绑定

1. 单向绑定：属性的值由变量决定

2. 双向绑定：变量的值决定属性的值，属性的值也可以改变变量的值

   - `trim`：去除首尾空格

   ```html
   <script>
       window.onload = function(){
           var ele = new Vue({
               el:"div",
               data:{
                   变量名:值
               }
       	});
       }
   </script>
   
   <div>
       <p>{{变量名}}</p>
       
       <!-- 单向绑定，可简写为【:value】 -->
       <input type="text" v-bind:value="变量名" />
       
       <!-- 双向绑定，可简写为【v-model】 -->
       <input type="text" v-model:value="'字符串'+变量名" />
       <!-- 单存在字符串时，使用''引起来，并使用+进行连接 -->
       
       <!-- 双向绑定可以使用 -->
       <input type="text" v-model.trim="变量名" />
       
   </div>
   ```





### 条件判断

```html
<script>
    window.onload = function(){
        var ele = new Vue({
            el:"#user",
            date:{
                uname:"Tom"
            } 
    	});
    }
</script>

<div id="user">
    
    <!-- 判断是否为true，若不为true，则输出v-else。if-else间不能有其他节点！ -->
	<p v-if="uname=='Tom'">A</p>  
    <p v-else="uname=='Tom'">B</p>
    
    <!-- 若为true，则会显示，若不为true，则display为none -->
	<div v-show="uname=='Tom'">C</div>
    
</div>
```





### 集合与迭代

```html
<script>
    window.onload = function(){
        var ele = new Vue({
            el:"div",
            data:{
                
            	// 集合的定义
                集合名称:[
                    { 变量名1:"值1-1" , 变量名2:"值1-2" },
                    { 变量名1:"值2-1" , 变量名2:"值2-2" }
                ]
                
            }
    	});
    }
</script>

<div>
    <table>
        
        <!-- 取出集合中每一个元素存放到变量中，再通过迭代变量获取到元素中指定变量的值 -->
        <tr v-for="迭代变量名 in 集合名称">
        	<td>{{迭代变量名.变量名1}}</td>
        	<td>{{迭代变量名.变量名2}}</td>
        </tr>
    
    </table>
</div>
```





### 绑定事件

```html
<script>
    window.onload = function(){
        var ele = new Vue({
            el:"div",
            methods:{
                方法名:function(){
                    alert("test");
                }
            }
    	});
    }
</script>

<div>

    <!-- 绑定单击事件，可以简化为【@click】 -->
    <input type="button" v-on:click="方法名"/>    
	<!-- 方法名后不能带()，否则方法会立刻执行，而不是在触发事件后才执行 -->
</div>
```





### 监听属性

```html
<script>
    window.onload = function(){
        var ele = new Vue({
            data:{
                变量名:值
            }
            
            // 监听属性：当变量的值发生改变时，会调用对应的方法（新变量名指的是：变量改变后的值）
            watch:{
            	变量名:function(新变量名){
            		方法体;
        		}
        	}
    
    	});
    }
</script>
```





### 生命周期

```html
<script>
	window.onload = function(){
        var ele = new Vue({
            
            // 在vue对象创建前调用
            beforeCreate:function(){},
        
        	// 在vue对象创建后调用
            created:function(){},
            
            // 在页面渲染前调用
            beforeMount:function(){},
            
            // 在页面渲染后调用
            mounted:function(){},
            
            // 在数值赋值前调用
            beforeUpdate:function(){},
            
            // 在数值赋值后调用
            updated:function(){}
            
        });
    }
</script>
```













# Servlet

## Tomcat

### 知识点

1. CS架构：客户端 - 服务器
   - 优点：充分利用客户端资源，减少服务器、网络负荷
   - 缺点：需要安装，且维护成本较高
2. BS架构：浏览器 - 服务器
   - 优点：不需要安装，且维护成本较低
   - 缺点：所有的计算和存储都是在服务端，服务器、网络负荷较重



Tomcat目录介绍：

1. bin：存放Tomcat可执行文件的目录
2. conf：配置文件存放目录
3. lib：存放Java代码的目录
4. logs：存放日志文件的目录
5. webapps：项目部署目录
6. work：Tomcat运行时的工作目录
7. temp：存放临时文件的目录









### 安装与配置

1. 解压即可
2. 配置：由于Tomcat是由 Java+C 编写的，因此需要依赖于Java虚拟机（即：JRE），因此需要配置环境变量：JAVA_HOME







### 部署

#### 直接部署

1. bin/startup.bat 启动Tomcat服务

2. 创建文件夹 webapps/项目名/WEB_INF

   > 该项目名也被称为：context root

3. 将页面存放到 webapps/项目名 下即可访问：`协议://IP地址:端口号/项目名/页面`





#### IDEA部署

> IDEA部署的项目，并不是存放在webapps下的

1. 添加为web项目

   - File --> Project Structure --> Facets --> + --> Web --> 选择要添加的项目 --> 检查Delopyment和Web Resource的Path是否正确 --> OK

     > Delopyment：web.xml文件要存放于：项目名/web/WEB-INF目录下（不存在xml则 + 创建）
     >
     > Web Resource文件要存放在：项目名/web目录下

2. 打包web项目（添加部署包）

   - File --> Project Structure --> Artifacts --> + --> Web Application:Exploded --> From Modules --> 选取要打包的项目 --> OK

     > 若后续添加Jar包时，不是添加在WEB-INF下的，则需要将该Jar包重新添加进Artifacts中

3. 将web项目（部署包）部署到Tomcat上

   - Add Configuration... --> + --> TomcatServer --> Local

   - Deployment --> + --> Artifact --> 选择要部署的项目

     > Application context：决定context root

   - Server --> Configure --> 选择Tomcat程序的目录（主目录，包含七大目录）

     > - Default：默认打开的浏览器
     >
     > - URL：默认打开浏览器时，打开的页面
     >
     >   末尾有 / 时，表示访问默认欢迎页
     >
     >   可在Tomcat或IDEA中的web.xml文件中\<welcome-file-list>标签下进行配置
     >
     > - On Update action：当有更新操作时，Redeploy --> 重新部署
     >
     > - On frame deactivation：当窗口失去焦点时









## 知识点

### 流程解析

1. 表单提交数据，根据action发送请求：action="请求路径"
2. 在web.xml中，寻找\<servlet-mapping>中，url-pattern为`/请求路径`所对应的servlet-name
3. 根据servlet-name，获取与\<servlet>中相对应的，根据servlet-class定位到请求接收的类
4. 接收的类根据提交的方式method，执行doPost或doGet方法





#### 映射

1. 在web.xml文件中的\<web-app>标签中添加映射关系：

   ```xml
   <servlet-mapping>
       <servlet-name>映射关系名</servlet-name>
       <url-pattern>/请求路径</url-pattern>
   </servlet-mapping>
   
   <servlet>
   	<servlet-name>映射关系名</servlet-name>
       <servlet-class>全类名</servlet-class>
   </servlet>
   ```

2. 在Java请求处理类（Servlet）中使用注解的方式添加映射关系：

   ```java
   @WebServlet("/请求路径")
   public class Servlet处理类 extends HttpServlet{}
   ```

注意：

- `/请求路径` 与 `请求路径` 从路径方面来讲，效果是一样的。/代表根路径，即web根目录下；web.xml文件本身也是出于web根目录下（web.xml文件的相对路径被认为也是相对于web根目录的）。
- 但是，从Servlet语法规范和匹配规则方面来讲，`/`不能省略
- @WebServlet 与之有所不同，如（`@WebServlet("*.do")`：解析所有以.do结尾的请求）也是可以被Servlet解析的（但也要满足一定的Servlet语法规则）







#### 请求处理

1. 要继承于HttpServlet类（位于Tomcat - lib - servlet-api.jar 包下）
2. 根据请求数据的发送方式（method）重写doPost或doGet等方法
3. HttpServletRequest：请求所提交的数据都会封装进该类中
   - 调用方法获取 表单中name属性所在的标签的value值：【String】`getParameter(String name)`







### 编码设置

1. Tomcat8之前：

   - GET：需要先将获取到的数据（字符串）根据 ISO-8859-1（Tomcat底层编码格式）转换为字节数组，再将字节数组转换为 UTF-8 编码格式的字符串

     ```java
     String valueStr = HttpServletRequest对象.getParameter("name属性")
     byte[] b = valueStr.getBytes("ISO-8859-1");
     valueStr = new String(b,"UTF-8");
     ```

   - POST：在所有的获取参数的动作发生之前，调用方法设置编码格式

     - `HttpServletRequest对象.setCharacterEncoding("UTF-8")`

2. Tomcat8及之后：GET方法已不需要设置编码，POST方法设置方式依旧不变









### 三大方法

|--- javax.servlet.Servlet接口

​		|--- javax.servlet.GenericServlet抽象类

​				|--- javax.servlet.http.HttpServlet抽象子类

Servlet接口中的三个重要方法：

1. void init(config)：初始化方法

2. void service(request,response)：服务方法，在有请求过来时，会自动调用该方法

   > 在HttpServlet抽象子类中实现该方法

3. void destroy()：销毁方法





#### 405错误

原因：没有调用到doXxx()方法

- 当有请求过来时，会自动调用service()方法
- 在方法内部，会判断所请求的method的类型，从而调用对应的doXxx()方法（如：doGet()、doPost()）
- 这些doXxx()方法内部，基本上都是报出405或400错误
- 因此，若是没有调用到子类重写了父类后的doXxx()方法，而是调用了父类中的doXxx()方法，则会报405错误





#### 生命周期

在默认情况下：

- 在第一次接收到请求时，会调用Servlet实现类进行实例化（调用构造方法），再调用init()方法进行初始化，再调用service()方法进行服务
- 在之后的每一次请求时，调用service()方法进行服务
- 当Tomcat容器关闭时，servlet实例会被销毁，并调用destroy()方法

> 1. Servlet实例Tomcat只会创建一个，所有的请求都是根据这个实例去调用的
> 2. 若要选择在第一次请求前就完成实例和初始化，可以在web.xml文件中，对应的\<servlet>中添加load-on-startup指定在启动时就进行加载
>    - load-on-startup设置的数字越小，就越靠前就加载，最小值为0
>    - 启动时加载可以提高响应速度，但会影响服务启动时的速度
> 3. Servlet在Tomcat容器中的特点：单例、线程不安全





#### init方法

- init有两个重载方法，分别是：init()、init(ServletConfig)

- 在 init(ServletConfig) 中，会对ServletConfig对象进行赋值，并调用 init()

- 在 init() 中，没有任何代码实现，是供我们重写后调用的方法，可以获取到 在初始化时获取的参数 的值

  - 通过调用`getServletConfig()`获取到ServletConfig对象
  - 通过调用`ServletConfig对象.getInitParameter(String keyName)`获取保存的初始化参数值

- 初始化参数值的设置：

  1. 在web.xml文件中设置：

     ```xml
     <servlet>
     <!-- 在<servlet>标签中设置 -->
         
         <init-param>
         	<param-name>keyName</param-name>
             <param-value>value值</param-value>
         </init-param>
         <!-- 初始化参数值可以有多个 -->
     </servlet>
     ```

  2. 在Servlet处理类使用注解的方式设置：

     ```java
     @WebServlet(urlPatterns={"/test1","/test2"},	// 请求接收路径
                initParams={							// 设置初始化参数值
                    @WebInitParam(name="keyName",value="value值")
                })
     public class 类名 extends HttpServlet{}
     ```

- init()方法还可以获取ServletContext上下文对象：`getServletContext()`

  - 也可以调用`getInitParameter("String keyName")`获取到保存在web.xml配置文件中的初始化值

  - 在web.xml文件中的配置初始化值的方法：

    ```xml
    <!-- 在<web-app>中定义 -->
    <context-param>
    	<param-name>keyName</param-name>
        <param-value>value值</param-value>
    </context-param>
    <!-- thymeleaf页面渲染中也用到了该方式 -->
    ```









### Session

> 由于HTTP协议是无状态的，服务端无法区分两次请求是否是同一个客户端所发送的，因此需要一个唯一变量来标记客户端（即：使用会话跟踪技术）

#### 概念

1）会话跟踪技术：

1. 客户端第一次发送请求给服务端，服务端获取其Session。若获取不到，则创建一个新的Session，并响应给客户端
2. 客户端在接下来的请求中，将sessionID发送给服务器，服务器获取到后，进行比对，就可以知道两个请求是否同一个客户端发送的





2）保存作用域：

> 在保存作用域中存放key-value，即使在不同的Servlet处理类中，都可以根据保存作用域的类型，在一定的范围内，通过key获取到对应的value值

1. page：页面级别，几乎已无用
2. request：一次请求响应范围内有效（如：能用于内部转发，不能用于客户端重定向）
3. session：在一次会话中有效（如：重启客户端失效）
4. application：在一次应用程序范围内有效（如：一人存放，全都可用，重启服务失效）







#### 常用方法

1）获取保存作用域对象

【HttpSession】`HttpServletRequest对象.getSession()`：获取当前会话（若不存在，则创建新的会话）

- `getSession(true)`：与不带参数效果相同
- `getSession(false)`：若会话不存在，则返回null，不会创建新的



【ServletContext】`HttpServletRequest对象.getServletContext()`：获取当前Servlet上下文

【ServletContext】`HttpSession对象.getServletContext()`：获取当前Servlet上下文对象

> 上下文：取得从服务开启开始，到后续服务停止而结束的对象（application）
>
> - 还可以在 init()初始化方法、监听器ServletContextListener（ServletContextEvent对象）中，获取到ServletContext对象





2）往保存作用域中存值或取值：

`HttpSession对象.setAttribute(String key,Object value)`：在Session保存作用域中存放内容

【Object】`HttpSession对象.getAttribute(String key)`：通过key获取Session保存作用域中对应的value值

`HttpSession对象.removeAttribute(String key)`：移除Session保存作用域中指定key和value值





3）其他方法：

【String】`HttpSession对象.getId()`：获取Session的sessionID

【boolean】`HttpSession对象.isNew()`：判断当前Session是否是新创建的

`HttpSession对象.invalidate()`：强制性使Session立即失效



【int】`HttpSession对象.getMaxInactiveInterval()`：获取Session的非激活间隔时长（默认为1800s）

- `setMaxInactiveInterval(int time)`：设置Session的非激活间隔时长

> 非激活间隔时长：长期无活跃时使Session过期的时间









### Cookie

> 客户端在第一次发送请求时，若服务端有响应Cookie，则客户端会保存该Cookie
>
> 后续请求该客户端时，若该Cookie未过期，则在每次发送请求时，都会携带上该Cookie
>
> 可用于：设置是否记住用户密码、设置用户密码保存的天数、设置是否自动登录

```java
// 1.调用Cookie类中的Cookie(String name,String value)构造器，创建一个Cookie对象：
Cookie c = new Cookie("name","value");

// 2.调用HttpServletResponse对象的addCookie(Cookie)方法，将该Cookie响应给客户端
response.addCookie(c);
```



`Cookie对象.setMaxAge(int s)`：设置Cookie多少秒后失效



`Cookie对象.setPath(String URI)`：设置只有指定的URI才会发送Cookie

`Cookie对象.setDomain(String pattern)`：设置只有指定的参数列表才会发送Cookie









### 内部转发与重定向

服务器内部转发：

- 客户端向指定URL发送请求，被Servlet转发给其他Servlet进行处理，并在处理后将结果响应给客户端（请求的URL没有发生变化）
- 调用方法：`HttpServletRequest对象.getRequestDispatcher("请求路径").forward(HttpServletRequest对象,HttpServletResponse对象)`



客户端重定向：

- 客户端向指定URL发送请求，Servlet响应302重定向状态码，并让客户端重新发送请求到指定URL处，让对应的Servlet处理请求（请求的URL发生了变化）
- 调用方法：`HttpServletResponse对象.sendRedirect("请求路径")`









### 过滤器

> 在每次请求指定路径时，过滤器会对满足该路径的进行拦截，执行到 doFilter() 中的放行后，就会让请求发送过去，当响应回客户端时，又会进行拦截，执行 doFilter() 之后的代码后，才会让响应发送过去

1. 让类实现 javax.Servlet.Filter接口，并实现三个方法：init、doFilter、destroy

2. 配置拦截关系，拦截指定请求路径

   - 在web.xml文件中的\<web-app>标签中配置

     ```xml
     <filter>
     	<filter-name>映射关系名</filter-name>
         <filter-class>过滤器类</filter-class>
     </filter>
     <filter-mapping>
     	<filter-name>映射关系名</filter-name>
         <url-pattern>/请求路径</url-pattern>
     </filter-mapping>
     ```

   - 在类声明处使用注解的方式配置

     ```java
     @WebFilter("/请求路径")			// 也可以使用通配符("*.do")表示拦截所有以.do结尾的请求
     public class 类名 implements Filter{}
     ```

3. 过滤数据后，在 doFilter() 中对请求放行：

   调用方法：`FilterChain对象.doFilter(ServletRequest对象,ServletResponse对象)`

当配置了多个过滤器时：

- 注解的方式：按照全类名的先后顺序执行
- xml的方式：按照配置声明的先后顺序执行









### 本地线程

使用方法：

```java
ThreadLocal<泛型> tl = new ThreadLocal<>();			// 创建本地线程对象

tl.set(泛型);			// 在当前线程上存储tl对象的数据

tl.get();			// 在当前线程中获取tl对象的数据

tl.remove();		// 移除掉当前线程中tl对象存储的值
// 在事务中存储连接时，要及时移除，否则会导致连接已被释放，当依旧能获取到tl对象的值的情况
```

> 源码剖析：
>
> - set(Object)：
>
>   ```java
>   Thread t = Thread.currentThread();		// 获取当前线程
>   ThreadLocalMap map = getMap(t);			// 每个线程都会维护的一个Map容器
>   if(map != null){
>       map.set(this,value);		
>   // this对应的就是ThreadLocal对象，因为一个线程中可以有多个ThreadLocal对象（即：一个线程可以有多个需要被共享的值）
>   }else{
>       createMap(t,value);					// 该容器创建方式是等到需要时在创建的
>   }
>   ```
>
> - get()：
>
>   ```java
>   Thread t = Thread.currentThread();		// 获取当前线程
>   ThreadLocalMap map = getMap(t);			// 每个线程都会维护的一个Map容器
>   if(map != null){
>       ThreadLocalMap.Entry e = map.getEntry(this);
>       // 获取当前线程中当前ThreadLocal保存的value值
>   }
>   ```









### 监听器

① ServletContextListener：监听ServletContext对象的创建和销毁

② HttpSessionListener：监听HttpSession对象的创建和销毁

③ ServletRequestListener：监听ServletRequest对象的创建和销毁



④ ServletContextAttributeListener：监听ServletContext的保存作用域的改动（add、romove、delete）

⑤ HttpSessionAttributeListener：监听HttpSession的保存作用域的改动

⑥ ServletRequestAttributeListener：监听ServletRequest的保存作用域的改动



⑦ HttpSessionBindingListener：监听某个对象在Session中的创建与移除

⑧ HttpSessionActivationListener：监听某个对象在Session中的序列化和反序列化



使用举例：

1. 实现对应接口及其抽象方法

   > 如：ServletContextListener接口中的抽象方法：contextInitialized()、contextDestroyed()

2. 应用监听器到实现类上

   - 在web.xml文件中\<web-app>标签中声明

     ```xml
     <listener>
     	<listener-class>实现类全类名</listener-class>
     </listener>
     ```

   - 在实现类上使用注解声明

     ```java
     @WebListener
     public class 实现类 implements 监听器{}
     ```







### 验证码

1. 导入jar包：`kaptcha.jar`

2. 配置web.xml配置文件：

   ```xml
   <servlet>
   	<servlet-name>映射关系名</servlet-name>
       <servlet-class>com.google.code.kaptcha.servlet.KaptchaServlet</servlet-class>
       <!-- 可选参数 -->
       <init-param>
       	<param-name>参数名</param-name>
           <param-value>参数值</param-value>
       </init-param>
   </servlet>
   
   <servlet-mapping>
   	<servlet-name>映射关系名</servlet-name>
   	<url-pattern>/图片请求路径</url-pattern>
   </servlet-mapping>
   ```

   > 参数可在com.google.code.kaptcha.Constants类中查看

3. 每次获取图片（生成验证码）时，都会往Session中，保存验证码的值，key为：`KAPTCHA_SESSION_KEY`











## 页面渲染

> 在HTML页面中加载Java内存中的数据，此过程称之为渲染（render）
>
> thymeleaf是用来做视图渲染的技术
>
> 只有在经过服务端渲染后，thymeleaf语法才能在页面上生效

### 渲染流程

1. 导入jar包：attoparser.RELEASE.jar、javassist-GA.jar、log4j.jar、ognl.jar、slf4j-api.jar、slf4j-log4j12.jar、thymeleaf-RELEASE.jar、unbescape.RELEASE.jar

2. 新建一个ViewBaseServlet类，继承于HttpServlet类

   > ViewBaseServlet类代码模板：
   >
   > ```java
   > public class ViewBaseServlet extends HttpServlet {
   >     private TemplateEngine templateEngine;
   >     @Override
   >     public void init() throws ServletException {
   >         // 1.获取ServletContext对象
   >         ServletContext servletContext = this.getServletContext();
   >         // 2.创建Thymeleaf解析器对象
   >         ServletContextTemplateResolver templateResolver = new ServletContextTemplateResolver(servletContext);
   >         // 3.给解析器对象设置参数
   >         // ①HTML是默认模式，明确设置是为了代码更容易理解
   >         templateResolver.setTemplateMode(TemplateMode.HTML);
   >         // ②设置web.xml配置文件对应的前缀
   >         String viewPrefix = servletContext.getInitParameter("view-prefix");
   >         templateResolver.setPrefix(viewPrefix);
   >         // ③设置web.xml配置文件对应的后缀
   >         String viewSuffix = servletContext.getInitParameter("view-suffix");
   >         templateResolver.setSuffix(viewSuffix);
   >         // ④设置缓存过期时间（毫秒）
   >         templateResolver.setCacheTTLMs(60000L);
   >         // ⑤设置是否缓存
   >         templateResolver.setCacheable(true);
   >         // ⑥设置服务器端编码方式
   >         templateResolver.setCharacterEncoding("utf-8");
   >         // 4.创建模板引擎对象
   >         templateEngine = new TemplateEngine();
   >         // 5.给模板引擎对象设置模板解析器
   >         templateEngine.setTemplateResolver(templateResolver);
   >     }
   > 
   >     protected void processTemplate(String templateName, HttpServletRequest req, HttpServletResponse resp) throws IOException {
   >         // 1.设置响应体内容类型和字符集
   >         resp.setContentType("text/html;charset=UTF-8");
   >         // 2.创建WebContext对象
   >         WebContext webContext = new WebContext(req, resp, getServletContext());
   >         // 3.处理模板数据
   >         templateEngine.process(templateName, webContext, resp.getWriter());
   >     }
   > }
   > ```

3. 在web.xml文件中添加配置

   - 配置要渲染的页面的前缀（路径）：view-prefix

     ```xml
     <context-param>
     	<param-name>view-prefix</parma-name>
         <param-value>/前缀路径</param-value>
     </context-param>
     ```

   - 配置要渲染的页面的后缀（类型）：view-suffix

     ```xml
     <context-param>
     	<param-name>view-suffix</param-name>
         <param-value>.后缀类型</param-value>
     </context-param>
     ```

4. 让Servlet处理类继承于ViewBaseServlet类

5. 重写doGet()或doPost()方法处理请求

6. 将响应的结果添加到保存作用域中

7. 调用父类ViewBaseServlet中的方法，进行渲染：`processTemplate(String templateName,HttpServletRequest,HttpServletResponse)`

   - templateName：逻辑视图名，根据web.xml文件中配置的前缀和后缀，对指定的真实视图进行渲染

     > 真实视图 = view-prefix + 逻辑视图名 + view-suffix（如：/baidu/ + index + .html）

8. 在渲染的页面上获取保存作用域中的数据







### 基础语法

#### 语法规则

##### 标签头

`<html xmlns:th="http://www.thymeleaf.org">`



##### 调用

1. `对象.属性`：本质上是调用了对象内部的getXxx()，可用来获取该属性的值（没有该方法是获取不到的）
2. 涉及到调用属性或方法的，都需要使用`${}`修饰（变量赋值不需要）



##### 变量

1. `变量名:值`：变量赋值，可用在循环标签中



##### 路径

1. `@{}`用来表示路径（只有在经过服务端渲染后，thymeleaf语法才会生效）

   - 相对路径：以配置的view-prefix前缀为基础
   - 绝对路径：/开头，代表以web根目录作为基础

   > 若是Servlet请求，则view-prefix是不会生效的，而是按照Servlet的映射规则（以根目录作为基础）进行解析

2. 路径带参数（get方式）：
   - `@{'/请求路径?key=' + value值}`：拼接字符串的方式携带参数（ '字符串' 表示不需要被解析）
   - `@{|/请求路径?key=value值|}`：||内，遇到themeleaf语法，会自动识别，否则按字符串处理
   - `@{/请求路径(key1 ='value值', key2 ='value值')}`：使用`(key='value')`携带参数







#### 常见标签

`th:if`：分支结构，如果成立，则执行该标签的后面部分

`th:unless`：分支结构，如果不成立，则执行标签的后部分

`th:each`：循环结构，循环执行该标签

> ```html
> <tr th:unless="${#lists.isEmpty(session.fruits)}" th:each="fruit:${session.fruits}">
> <!--
> ① 判断 Session保存作用域中的fruits集合对象 是否为空，若不为空，则执行后面标签。
> ② 取出 Session保存作用域中的fruits集合对象中的一个对象，并赋值给fruit，且总量-1。在执行完标签内容后，再次（循环）执行该标签。
> -->
> ```



`th:text`：设置标签内部文本

`th:value`：设置标签value属性值

> ```html
> <td th:text="${fruit.name}"></td>
> <!-- 设置标签内部text文本为：fruit对象的name属性的值 -->
> 
> <input th:value="${fruit.name}">
> <!-- 设置input标签的value的属性的值 -->
> ```



`th:href`：设置标签属性href的值（修改发送请求路径）

`th:action`：设置标签属性action的值（修改提交数据路径）

> ```html
> <a th:href="@{ '/update?id=' + ${fruit.id} }"></a>
> <!-- 点击超链接时，将请求发送至对应的Servlet处理类，并携带参数id -->
> 
> <form th:action="@{/update(id=${fruit.id})}"></form>
> <!-- 提交表单时，将数据提交至对应的Servlet处理类，并携带参数id -->
> ```



`th:object`：表示该标签内容下所有调用的属性或方法，都是该对象的，使用 `*{属性名}` 取出对象的属性值

> ```html
> <tr th:object="${fruit}">	
> <!-- tr标签下，所有调用属性或方法，都是fruit对象的（Request保存域取出的对象） -->
> 	
>     <td th:text="*{name}"></td>
> 	<!-- 获取fruit对象的name属性值（相当于调用fruit的getName()方法） -->
>     
> </tr>
> ```



`th:事件名`：动态绑定标签事件

`th:属性名`：动态绑定标签属性

>```html
><td th:onclick="|delete( ${fruit.id} )|">删除</td>
><!-- 单击该标签时，触发单击事件，调用delete()方法，动态传入不同的fruit.id作为参数 -->
>
><input th:disabled="${fruit.count==1}">
><!-- 当fruit.count的值为1时，则结果为true，锁定文本框，文本框将不可操作  -->
>```









#### 常见对象

`#lists`：集合对象

- `isEmpty(内容)`：方法，判断指定内容是否为空



`#dates`：日期类对象

- `format(内容,'格式')`：方法，将日期（java.util.Date格式）转换为指定样式的字符串
- 因为通过`对象.属性`本质上是调用的`getXxx()`方法，因此可以直接在get方法内部进行日期的格式化转换，最后再返回即可



`xxx`：通过xxx（key）可直接获取在Request保存作用域中的value值或对象

`session`：Session保存作用域对象

- `xxx`：属性，通过xxx（key）获取在Session保存作用域中的value值或对象

`application`：Application保存作用域对象

- `xxx`：属性，通过xxx（key）获取在Application保存作用域中的value值或对象









## Ajax

> 异步请求，局部刷新

### 基本使用

使用步骤：

1. 创建一个XMLHttpRequest对象

   ```javascript
   var xmlHttpRequest;
   
   function createXMLHttpRequest(){
       // 符号DOM2标准的浏览器的创建方式
       if(window.XMLHttpRequest){
           xmlHttpRequest = new XMLHttpRequest();
       // IE浏览器的创建方式
       }else if(window.ActiveXObject){
           try{
               // IE浏览器的创建方式有两种，一种失败的话，则用另一种创建
               xmlHttpRequest = new ActiveXObject("Microsoft.XMLHTTP");
           }catch(e){
               xmlHttpRequest = new ActiveXObject("Msxm12.XMLHTTP");
           }
       }
   }
   ```

2. 调用XMLHttpRequest对象的`open(url,"GET",true)`方法，将请求发送至指定url

   > 也可以使用POST方法，但有所不同。true代表开启异步方式发送请求

3. 为XMLHttpRequest对象绑定事件`onreadystatechange`：当每次发送状态发生改变时，调用指定的函数（回调函数）：

   - 根据XMLHttpRequest对象的【发送状态码`readyState`（0-4）】以及【响应状态码`status`（200）】决定是否对响应数据进行解析
   - 调用XMLHttpRequest对象的属性`responseText`，获取服务端响应的文本（HTML代码），就可根据响应内容进行处理

4. 调用XMLHttpRequest对象的`send()`方法，发送请求







### Axios

> 是Ajax的一个框架。使用前，需要引用`axios.js`文件

#### 基本格式

##### 无返回值

```javascript
axios({			// 发送请求，需要一个对象，并且有几个必要的参数：请求方式、请求路径
    method:"POST",		// 请求方式
	url:"请求路径",
    			// date（JSON数据）、params（普通请求参数）
}).then(function(value){
    // 成功响应后执行的回调函数。
    // 其中，value.data 可以获取到服务器端响应的内容
}).catch(function(reason){
    // 出现异常时执行的回调函数。
    // 其中，reason.response.data 可以获取到服务器端响应的内容
    // reason.message、reason.stack 可以查看错误信息
})
```



##### 有返回值

```javascript
var a = new Vue({
    el:"xxx",
    methods:{
        methodName:function(){
            return axios({
            	// 对象属性
            }).then(function(value){
                return value;		// 返回值
            }).then().catch();  
            // 再添加一个then()的原因：
            // 调用回调函数是另外一个线程执行，主线程会在执行最后一个then()时开始执行
        }    
    }
});

// 在方法中调用methodName方法，获取回调函数的返回值（异步）
// 方式一：
function method1(){
    var param = a.methodName().then(function(returnValue){
        // returnValue则为回调函数的返回值
    })    
}

// 方式二：
async function method2(){
    var param = await a.methodName();
 // 该方式也能获取方法调用后的返回值。不同的是，该方式是让a.methodName()（异步）方法执行完后，再获取结果
    // 虽然可以让该方法内部的代码按顺序执行，但该method2方法本身依旧是异步方法
}
```







#### 普通参数

##### 前端请求

```html
<script type="text/javascript" src="vue.js"></script>
<script type="text/javascript" src="axios.js"></script>
<script type="text/javascript">
	window.onload = function(){
        var ele = new Vue({
            el:"input",
            methods:{
                sendParameter:function(){
                    
                    axios({
                        method:"POST",		// 请求方式
                        url:"axios.do"		// 请求地址
                        params:{			// 普通参数列表
                        	name:"Amy",		
                        	age:19
                    	}
                    }).then(function(value){
                        alert(value.data);
                    }).catch(function(reason){});
                    
                }
            }
        });
    }
</script>

<input type="button" v-on:click="sendParameter">
```



##### 后端响应

```java
@WebServlet("/axios.do")
public class 类名 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp){
        String name = req.getParameter("name");
        int age = Integer.parseInt(req.getParameter("age"));
        System.out.println(name + "_" + age);
        resp.getWriter().write("Ok!She is good!");			// 响应给客户端
    }
}
```





#### JSON格式

##### 前端请求

```html
<script>
	window.onload = function(){
        var ele = new Vue({
            el:"input",
            methods:{
                sendParameter:function(){
                    
                    axios({
                        "method":"POST",		// 请求方式
                        "url":"axios.do",		// 请求地址
                        "data":{				// JSON格式数据
                            "name":"Amy",
                            "age":19
                        }
                    }).then(function(value){
                        alert(value.data);
                        // Axios会自动帮我们将JSON格式的字符串转换为JSON对象
                    }).catch(function(reason){});
                    
                }
            }
        });
    }
</script>

<input type="button" v-on:click="sendParameter">
```



##### 后端响应

> 若要处理JSON格式字符串，需要导包：`gson.jar`

```java
@WebServlet("/axios.do")
public class 类名 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp){
        BufferedReader reader = req.getReader();		// 获取数据输入流
        StringBuffer stringBuffer = new StringBuffer();		
        // 获取一个StringBuffer对象，用于存储读取到的JSON格式字符串
        String str = null;
        while( (str=reader.readLine())!=null ){
            stringBuffer.append(str);
        }
        str = stringBuffer.toString();			// 获取读取到的JSON格式字符串
        Gson gson = new Gson();			// 创建一个Gson对象
        User user = gson.fromJson(str,User.class);		
        // 解析JSON格式字符串，并存储到对应的Java类中
    }
}
```

Gson类：用于解析JSON格式字符串，或将Java对象转化为JSON格式字符串

1. 创建Gson类的实例：

   - 调用构造方法：`Gson()`

2. 调用对应的方法

   - 【T】`fromJson(String json,Class<T>)`：将JSON格式字符串解析并存储到对应的Java对象的属性中

     > 【JsonObject】`fromJson(String json,JsonObject.class)`：将JSON格式字符串解析为JSON格式对象

   - 【String】`toJson(T obj)`：将Java对象转换为JSON格式字符串









## XML

>XML：可拓展的标记语言

### 文件格式

1. XML声明
2. DTD文档类型定义（可选）
3. XML正文

```xml
<?xml version="1.0" encoding="UTF-8" ?>	
<!-- XML声明 -->

<!DOCTYPE 外层标签名 [
	<!ELEMENT 外层标签名 (内层标签名*)>	（表示外层标签内只能有指定的内层标签，可以有一个或多个）
	<!ELEMENT 内层标签名 (子标签名*)>	（同理）
	<!ELEMENT 子标签名 (#PCDATA)>		（表示子标签内无标签了，只是文本节点）
	
	<!ATTLIST 标签名 属性名 ID #REQUIRED>
	<!ATTLIST 标签名 属性名 CDATA #IMPLIED>
	<!ATTLIST 标签名 属性名 IDREF #IMPLIED>
		（#REQUIRED表示该属性必须有值，#IMPLIED表示该属性的值可有可无）
		（IDREF表示其属性值是对应ID的属性值，CDATA表示该属性值是字符串）
]>
<!-- DTD文档类型定义，规定XML正文的构成，书写格式要求严格 -->

<标签名 属性名="属性值"/>
<!-- XML正文，标签名、属性名等都是可拓展的 -->

<标签名 属性名="属性值">
	<子元素节点名 属性名="属性值"/>
</标签名>
<!-- 节点分为：元素节点、文本节点（每一个标签对应一个元素节点；空格、换行、文本等都属于文本节点）-->
```







### 读取xml文件

```java
// 获取xml文件的输入流对象
InputStream is = getClass().getClassLoader().getResourceAsStream("com/fruit/relation.xml");

// 获取Document对象
DocumentBuilderFactory documentBuilderFactory = DocumentBuilderFactory.newInstance();
DocumentBuilder documentBuilder = documentBuilderFactory.newDocumentBuilder();
Document document = documentBuilder.parse(is);

// 根据标签名获取到指定的所有的节点对象
NodeList beanList = document.getElementsByTagName("标签名");

// 获取节点集合的大小，循环获取所有的节点
for (int i = 0; i < beanList.getLength(); i++) {
    
    // 取出指定下标的节点
   Node bean = beanList.item(i);
    
   // 获取节点的类型，若节点的类型为元素节点类型
   if (bean.getNodeType() == Node.ELEMENT_NODE) {
        // 则强转为元素节点类型
       Element element = (Element) bean;
        
        // 获取节点中指定属性名的属性值
       String name = element.getAttribute("属性名");
        
        // 获取元素节点下所有的子节点
       NodeList elementChildNodes = element.getChildNodes();
       for (int j = 0; j < elementChildNodes.getLength(); j++) {
          Node tmp = elementChildNodes.item(i);
           
          // 获取节点的类型 和 节点名，若节点的类型为元素节点类型 并且 为指定的节点名
          if(tmp.getNodeType()==Node.ELEMENT_NODE&&"节点名".equals(tmp.getNodeName())){
                Element ref = (Element) tmp;
                String refName = ref.getAttribute("属性名");
            }
        }
    }
}
```









## 补充

### HTTP协议

> 超文本传输协议，无状态，分为请求和响应

请求：

1. 请求行
   - 请求的方式（如：GET、POST）
   - 请求的URL
   - 请求的协议类型（一般都为HTTP1.1）
2. 请求头：包含客户端的信息、客户端发送给服务端的信息
3. 请求体：
   - GET：无请求体，但有queryString
   - POST：form data
   - JSON：request payload



响应：

1. 响应行
   - 协议
   - 响应状态码（如：200、404）
   - 响应状态（如：OK）
2. 响应头：包含服务器的信息、服务器发送给客户端的信息
3. 响应体：响应的实际内容（如：HTML页面\<html>...）







### 其他方法

HttpServletRequest对象：

- 【String】`getRequestURL()`：获取到客户端请求的URL完整路径

  【String】`getRequestURI()`：获取到客户端请求的URL中请求部分的路径

  【String】`getQueryString()`：获取到客户端请求的URL中请求的参数列表

  > 可用于：对请求的路径进行拦截，判断用户是否登录，对非法的请求路径进行重定向等





HttpServletResponse对象：

- `setCharacterEncoding("字符集")`：设置响应的字符集

- `setContentType(String)`：设置客户端解析内容的方式

  1）必选：

  - `text/html`：设置响应的文本内容以HTML的格式解析（默认）
  - `application/json`：设置响应的文本内容以JSON字符串的格式解析

  2）可选：

  - `charset=utf-8`：设置编码集（优先级比setCharacterEncoding要高）

- 【PrintWriter】`getWriter()`：获取输出对象

  `PrintWriter对象.write(String)`：将字符串文本响应给客户端

  `PrintWriter对象.print(Object)`：调用对象的toString方法，将内容转换为字符串后响应给客户端
  
  `PrintWriter对象.println(Object)`：响应时，末尾会附带换行符
  
  `PrintWriter对象.flush()`：刷新缓冲区，立即输出文本内容
  
  > 可用于写入JavaScript，进行弹窗、跳转页面等









# MVC架构

## 思路与步骤

1）一个业务会存在大量的Servlet，一个Servlet对应一个处理类显然不合适，因此，考虑将一个业务的Servlet统一封装进一个处理类中，之前的Servlet改为方法，通过请求参数（如：operate=add）设置对应的switch-case来调用对应的方法

2）若业务处理逻辑非常复杂，则switch-case语句会特别臃肿，因此，考虑将方法名和请求参数名一致，获取参数名后，通过反射调用对应的方法来实现

3）每一个业务都会用到反射，且每个业务中的方法，都会获取参数值，都会进行页面跳转或渲染，因此考虑将这些封装进一个类中，由该类来处理以下工作：

1. 根据请求url能定位到处理请求的处理类

   - 接收所有 .do结尾的请求：`@WebServlet("*.do")`

   - 调用方法获取请求的地址：`HttpServletRequest对象.getServletPath()`

   - 从请求url中提取关键字，由该关键字定位处理请求的处理类，这种对应关系存储在xml文件中

     > 因此，要读取xml文件中的所有对应关系，并将对应关系封装为Map集合对象（key-value）

   - 获取请求参数（如：operate=add），由该请求参数定位到处理类中对应的方法

2. 由该类来调用该处理类中的方法

   - 获取方法调用的参数
     - 通过`Method对象.getParameters()`获取方法的所有参数信息
     - 通过`Parameter对象.getName()`获取参数的名称（需设置）
     - 通过`Parameter对象.getType()`获取参数的类型，判断是否需要转换
   - 执行方法，获取返回值
   - 根据返回值做视图处理（如：返回值为 redirect，则进行重定向；返回值为 add，则进行页面渲染）









## 业务构造

MVC：Model（模型）、View（视图）、Controller（控制器）

视图：用于做数据展示、与用户交互的界面

控制器：接收客户端请求，但具体的业务功能还需借助模型组件完成

模型：如 pojo/vo（值对象、数据载体）、BO（业务模型组件）、DAO（数据访问层组件）等

> DAO中的方法是 单精度/细粒度 方法（一个方法值考虑一个操作，如insert、select...）
>
> BO属于业务方法，是粗粒度方法（如：注册业务：检查用户名、客户表新增数据、新增日志数据... 等）









## IOC 与 DI

IOC（控制反转）：

- 在Servlet中，若创建Service对象，则：
  1. 若该Service对象是出现在Servlet的某方法中，则该Service对象的生命周期为该方法内
  2. 若该Service对象是作为Servlet中的成员变量，则该Service对象的生命周期为该实例中
- 改为：在xml文件中，通过对应关系来创建Service对象，并存放到Map集合中

我们将Service对象的生命周期的控制权，由 程序员决定 --> Map集合对象决定，这种称之为控制反转

> 该集合的创建，可以考虑在服务器初始化时就自动创建（ServletContextListener），并且该xml文件的读取应是从web.xml配置文件中读取的（通过读取context-param初始化参数获取）



DI（依赖注入）：

- Servlet层 与 Service层、Service层与DAO层 存在耦合（如：Servlet层内需要创建Service层的实例）

- 将存在耦合的地方删除，在xml文件中配置Servlet层、Service层所需要的实例的对应关系

  > ```xml
  > <beans>
  >     <bean id="fruit" class="com.fruit.bo.fruitBo.FruitServlet">
  >         <!-- 该类中有属性fruitService，需要id="fruitService"所在的类的实例 -->
  >         <ref name="fruitService" ral="fruitService"/>
  >     </bean>
  >     
  >     <bean id="fruitService" class="com.fruit.bo.fruitBo.FruitService">
  >         <!-- 该类中有属性fruitDao，需要id="fruitDao"所在的类的实例 -->
  >         <ref name="fruitDao" ral="fruitDao"/>
  >     </bean>
  >     
  >     <bean id="fruitDao" class="com.fruit.dao.fruitDao.FruitDaoImp"/>
  > </beans>
  > ```







## 事务处理

在原先的事务处理中，是针对一个DAO操作进行事务管理的，显然不对（如：进行转账时，一方减钱成功，一方加钱失败）

因此，事务的处理应当是针对一个业务。考虑在过滤器中进行事务操作：

1. 在接收到请求时，开启事务，然后放行
2. 在接收到响应时，提交事务，关闭事务（注意先后顺序）
3. 在出现异常时，捕获异常，进行回滚









## 总结

> 开发流程

1. 创建数据库

   > 根据需要实现的功能，得出可能需要的表，再对表与表之间的关系进行关联

2. 配置web.xml文件

   - 页面渲染所需的view-prefix、view-suffix
   - 监听器所解析的application.xml配置文件路径

3. 创建myssm包，内含多种通用功能

   - baseDAO：进行与数据库交互的最基础功能
   - filters：内含至少3个过滤器（characterSet修改字符集、开启事务、对未登录用户进行限制）
   - listens：监听器，解析application.xml配置文件，生成IOC容器，并保存到application作用域中
     - application.xml：创建组件、为组件内部所需的属性赋值
   - DispatcherServlet：获取IOC容器；获取请求的Controller组件；调用组件方法，进行视图处理
   - 其他：如utils、libs、exceptions、tests包等等
   
4. 模块功能：

   - html页面、css样式、js特效
   - POJO类（数据载体）
   - DAO接口及其实现类（单精度方法）
   - Service接口及其实现类
     - 业务逻辑封装在Service这一层，不要分散在Controller层或DAO层
     - 当一个业务涉及到另一个业务的功能时，要调用另一个业务的Service层方法，不要去深入DAO层
   - Controller层：调用Service层服务，获取结果，返回要跳转的页面

