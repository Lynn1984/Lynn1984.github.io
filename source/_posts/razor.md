---
title: ASP.NET Razor - 标记
date: 2018-06-05 20:16:25
tags:
---

标签（空格分隔）： C#

---

## Razor简介

### 什么是 Razor？

* Razor是一种标记语法，可以让您将基于服务器的代码（Visual Basic 和 C#）嵌入到网页中。
* 基于服务器的代码可以在网页传送给浏览器时，创建动态Web内容。
* 当一个网页被请求时，服务器在返回页面给浏览器之前先执行页面中的基于服务器的代码。
* 通过服务器的运行，代码能执行复杂的任务，比如进入数据库。
* Razor是基于 ASP.NET 的，是为创建Web应用程序而设计的。
* 它具有传统ASP.NET的功能，但更容易使用并且更容易学习。

### Razor 语法

```
<ul>
@for (int i = 0; i < 10; i++) {
<li>@i</li>
}
</ul>
```

### Razor 帮助器

* ASP.NET 帮助器是通过几行简单的Razor代码即可访问的组件。
* 您可以使用Razor语法构建自己的帮助器，或者使用内建的ASP.NET帮助器。
* 下面是一些有用的Razor帮助器的简短说明：
    * Web Grid（Web 网格）
    * Web Graphics（Web 图形）
    * Google Analytics（Google 分析）
    * Facebook Integration（Facebook 集成）
    * Twitter Integration（Twitter 集成）
    * Sending Email（发送电子邮件）
    * Validation（验证）

### Razor 编程语言

Razor 支持 C# (C sharp) 和 VB (Visual Basic)。


## Razor 语法

### 主要的 Razor C# 语法规则

* Razor 代码块包含在 @{ ... } 中
* 内联表达式（变量和函数）以 @ 开头
* 代码语句用分号结束
* 变量使用 var 关键字声明
* 字符串用引号括起来
* C# 代码区分大小写
* C# 文件的扩展名是 .cshtml

```
<!-- Single statement block -->
@{ var myMessage =	"Hello World"; }

<!-- Inline expression or variable -->
<p>The value of myMessage is: @myMessage</p> 

<!--	Multi-statement block -->
@{
var greeting = "Welcome to our site!";
var weekDay = DateTime.Now.DayOfWeek;
var greetingMessage = greeting + " Here in Huston it is: " + weekDay;
}
<p>The greeting is: @greetingMessage</p>
```

### 它是如何工作的？

* Razor是一种将服务器代码嵌入在网页中的简单的编程语法。
* Razor语法是基于ASP.NET框架，专门用于创建Web应用程序的部分Microsoft.NET框架。
* Razor语法支持所有 ASP.NET的功能，但是使用的是一种简化语法，对初学者而言更容易学习，对专家而言更有效率的。
* Razor网页可以被描述成带以下两种类型内容的HTML网页：HTML内容和Razor代码。
* 当服务器读取页面时，它首先运行Razor代码，然后再发送 HTML页面到浏览器。
* 在服务器上执行的代码能够执行一些在浏览器上不能完成的任务，比如，访问服务器数据库。
* 服务器代码能创建动态的 HTML内容，然后发送到浏览器。
* 从浏览器上看，服务器代码生成的HTML与静态的HTML内容没有什么不同。
* 带 Razor 语法的 ASP.NET 网页有特殊的文件扩展名 cshtml（Razor C#）或者 vbhtml（Razor VB）。


## 变量

* 变量是用来存储数据的。
* 一个变量的名称必须以字母字符开头，并且不能包含空格或者保留字符。
* 一个变量可以是一个指定的类型，表示它所存储的数据类型。
* string 变量存储字符串值（"Welcome to RUNOOB.COM"），integer 变量存储数字值（103），date 变量存储日期值，等等。
* 变量使用 var 关键字声明，或通过使用类型（如果您想声明类型）声明，但是 ASP.NET 通常能自动确定数据类型。

### 变量类型

| 类型      |描述  | 实例 |
| :-------- | --------:| :--: |
| int | 整数（全数字） |  103, 1258   |
| float     |   浮点数 |  3.14, 3.4896 |
| decimal   |   十进制数字（高精度） | 1037.196543  |
| bool      |   布尔值 | true, false  |
| string    |   字符串 | "Welcome to BeiJing"  |

### 转换数据类型

| 类型      |描述  | 实例 |
| :-------- | --------:| :--: |
| AsInt() IsInt() | 转换字符串为整数。 |  if (myString.IsInt()) {myInt=myString.AsInt();}   |
| AsFloat() IsFloat()|转换字符串为浮点数。|if (myString.IsFloat()) {myFloat=myString.AsFloat();}|
| AsDecimal() IsDecimal()|转换字符串为十进制数。|if (myString.IsDecimal()) {myDec=myString.AsDecimal();}|
| AsDateTime() IsDateTime()|转换字符串为 ASP.NET DateTime 类型。|myString="10/10/2012"; myDate=myString.AsDateTime();  |
| AsBool() IsBool()|转换字符串为布尔值。|myString="True";myBool=myString.AsBool();|
| ToString()|转换任何数据类型为字符串。|myInt=1234;myString=myInt.ToString();|

## 循环和数组

```
<html>
<body>
@for(var i = 10; i < 21; i++)
{<p>Line @i</p>}
</body>
</html>
```

```
<html>
<body>
<ul>
@foreach (var x in Request.ServerVariables)
{<li>@x</li>}
</ul>
</body>
</html>
```

```
<html>
<body>
@{
var i = 0;
while (i < 5)
{
i += 1;
<p>Line @i</p>
}
}
</body>
</html>
```

```
@{
string[] members = {"Jani", "Hege", "Kai", "Jim"};
int i = Array.IndexOf(members, "Kai")+1;
int len = members.Length;
string x = members[2-1];
}
<html>
<body>
<h3>Members</h3>
@foreach (var person in members)
{
<p>@person</p>
}
<p>The number of names in Members are @len</p>
<p>The person at position 2 is @x</p>
<p>Kai is now in position @i</p>
</body>
</html>
```


## 逻辑

```
@{var price=50;}
<html>
<body>
@if (price>30)
{
<p>The price is too high.</p>
}
</body>
</html>
```

```
@{var price=20;}
<html>
<body>
@if (price>30)
{
<p>The price is too high.</p>
}
else
{
<p>The price is OK.</p>
} 
</body>
</html>
```

```
@{var price=25;}
<html>
<body>
@if (price>=30)
{
<p>The price is high.</p>
}
else if (price>20 && price<30) 
{
<p>The price is OK.</p>
}
else
{
<p>The price is low.</p>
} 
</body>
</html>
```

```
@{
var weekday=DateTime.Now.DayOfWeek;
var day=weekday.ToString();
var message="";
}
<html>
<body>
@switch(day)
{
case "Monday":
message="This is the first weekday.";
break;
case "Thursday":
message="Only one day before weekend.";
break;
case "Friday":
message="Tomorrow is weekend!";
break;
default:
message="Today is " + day;
break;
}
<p>@message</p>
</body>
</html>
```