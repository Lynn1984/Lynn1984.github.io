---
title: Asp.net MVC模式（二）
date: 2018-05-25 17:38:56
tags:
---

标签（空格分隔）： C#

---

## 1. 数据访问层
使用SQL Server和EF（Entity Framework）创建相关的数据库及数据库访问层。

### 1.1 简述实体框架（EF）

EF是一种ORM工具，ORM表示对象关联映射。
在RDMS中，对象称为表格和列对象，而在.net中（面向对象）称为类，对象以及属性。
任何数据驱动的应用实现的方式有两种：

* 通过代码与数据库关联（称为数据访问层或数据逻辑层）
* 通过编写代码将数据库数据映射到面向对象数据，或反向操作。
ORM是一种能够自动完成这两种方式的工具。EF是微软的ORM工具。

### 1.2 EF的三种方式来实现项目

* 数据库优先方法——创建数据库，包含表，列以及表之间的关系等，EF会根据数据库生成相应的Model类（业务实体）及数据访问层代码。
* 模型优先方法——模型优先指模型类及模型之间的关系是由Model设计人员在VS中手动生成和设计的，EF将模型生成数据访问层和数据库。
* 代码优先方法——代码优先指手动创建POCO类。这些类之间的关系使用代码定义。当应用程序首次执行时，EF将在数据库服务器中自动生成数据访问层以及相应的数据库。

### 1.3 POCO类

POCO即Plain Old CLR对象，POCO类就是已经创建的简单.Net类。在上两节的实例中，Employee类就是一个简单的POCO类。

## 2. 数据传递使用数据库

### 2.1 创建数据库
连接SQL SERVER ,创建数据库 “MVCDemoDB”。
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/8.png)

### 2.2 创建连接字符串（ConnectionString）

打开Web.Config 文件，在< Configuration >标签内添加以下代码：

```
<add connectionString="Data Source=.;Initial Catalog=MVCDemoDB;User ID=sa;Password=123456" name="MVCDemoDAL" providerName="System.Data.SqlClient" />
```

### 2.3 添加EF引用

右击项目->管理Nuget 包。选择Entity Framework 并点击安装。

### 2.4 创建数据访问层

* 在根目录下，新建文件夹”DataAccessLayer“，并在DataAccessLayer文件夹中新建类” MVCDemoDAL “
* 在类文件顶部添加 Using System.Data.Entity代码。
* 继承DbContext类

```
namespace MVCDemo.DataAccessLayer
{
    public class MVCDemoDAL: DbContext
    {
    }
}
```

### 2.5 创建Employee类的主键

打开Employee类，输入using语句

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.ComponentModel.DataAnnotations;

namespace MVCDemo.Models
{
    public class Employee
    {
        [Key]
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public int Salary { get; set; }
    }
}
```

### 2.6 定义映射关系

修改MVCDemoDAL类

```
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Web;

namespace MVCDemo.DataAccessLayer
{
    public class MVCDemoDAL: DbContext
    {
        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Employee>().ToTable("TblEmployee");
            base.OnModelCreating(modelBuilder);
        }
    }
}
```

注意：上述代码中提到“TblEmployee”是表名称，是运行时自动生成的。

### 2.7 在数据库中添加新属性Employee

在 MVCDemoDAL 类中添加新属性 Employee。

```
public DbSet<Employee> Employees { get; set; }
```

DbSet表示数据库中能够被查询的所有Employee

### 2.8 改变业务层代码，并从数据库中获取数据

修改EmployeeLayer类，添加GetAllEmployees方法：

```
public List<Employee> GetAllEmployees()
{
    MVCDemoDAL dal = new MVCDemoDAL();
    return dal.Employees.ToList();
}
```

### 2.9 插入测试数据

插入数据：

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/9.png)

运行效果：

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/10.png)

## 3. 数据集

* DbSet数据集是数据库方面的概念 ，指数据库中可以查询的实体的集合。
* 当执行Linq 查询时，Dbset对象能够将查询内部转换，并触发数据库。

## 4. 连接数据访问层和数据库

数据访问层和数据库之间的映射通过名称实现的，一般ConnectionString（连接字符串）的名称和数据访问层的类名称是相同的，都是MVCDemoDAL，因此会自动实现映射。
如果需要修改连接字符串的名称，则需要使用下面的方式：

```
public MVCDemoDAL():base("NewName"){}
```

## 5. 创建数据入口（Data Entry Screen）

* 在TestController中新建 “AddNew”action 方法：

```
public ActionResult addNew()
{
    return View("CreateEmployee");
}
```

* 在View文件夹中的MyView6.cshtml文件中加入addNew action的入口：

```
<a href ="/Test/AddNew"> Add New </a>
```

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/11.png)

* View文件夹中的CreateEmployee.cshtml文件：

```
@{
    Layout = null;
}
<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>CreateEmployee</title>
</head>
<body>
    <div>
        <form action="/Test/SaveEmployee" method="post">
            First Name: <input type="text" id="TxtFName" name="FirstName" value="" /><br />
            Last Name: <input type="text" id="TxtLName" name="LastName" value="" /><br />
            Salary: <input type="text" id="TxtSalary" name="Salary" value="" /><br />
            <input type="submit" name="BtnSave" value="Save Employee" />
            <input type="button" name="BtnReset" value="Reset" />
        </form>
    </div>
</body>
</html>
```

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/12.png)

## 6. Form 标签

Form标签是HTML中产生请求的一种方式，Form标签内部的提交按钮只要一被点击，请求会被发送到相关的action 属性。

方法属性决定了请求类型。有四种请求类型：get，post，put以及delete.

* Get： 当需要获取数据时使用。
* Post： 当需要新建一些事物时使用。
* Put: 当需要更新数据时使用。
* Delete：需要删除数据时使用。

当请求类型是Get，Put或Delete时，值会通过查询语句发送，当请求是Post类型，值会通过Post数据传送。
所有输入控件的值将随着请求一起发送。同一时间可能会接收到多个值，为了区分发送到所有值为每个值附加一个Key，这个Key在这里就是名称属性。
提交按钮在给服务器发送请求而专门使用的，而简单的按钮是执行一些自定义的客户端行为而使用的。按钮不会自己做任何事情。

## 7. 在服务器端（或Controller）获取Post数据

### 7.1 创建SaveEmployee action方法

在Test控制器中创建 名为 ”SaveEmployee“ action 方法：

```
public string SaveEmployee(Employee e)
{
    return e.FirstName + "|" + e.LastName + "|" + e.Salary;
}
```

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/13.png)
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/14.png)

### 7.2 增加重置按钮和取消按钮

* View界面代码：

```
@{
    Layout = null;
}
<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>CreateEmployee</title>

    <script type="text/javascript">
        function ResetForm() {
            document.getElementById('TxtFName').value = "";
            document.getElementById('TxtLName').value = "";
            document.getElementById('TxtSalary').value = "";
        }
    </script>
</head>
<body>
    <div>
        <form action="/Test/SaveEmployee" method="post">
            First Name: <input type="text" id="TxtFName" name="FirstName" value="" /><br />
            Last Name: <input type="text" id="TxtLName" name="LastName" value="" /><br />
            Salary: <input type="text" id="TxtSalary" name="Salary" value="" /><br />
            <input type="submit" name="BtnSubmit" value="Save Employee" />
            <input type="button" name="BtnReset" value="Reset" onclick="ResetForm();" />
            <input type="submit" name="BtnSubmit" value="Cancel" />
        </form>
    </div>
</body>
</html>
```

* TestController 代码

```
public ActionResult SaveEmployee(Employee e, string BtnSubmit)
{
    switch (BtnSubmit)
    {
        case "Save Employee":
            return Content(e.FirstName + "|" + e.LastName + "|" + e.Salary);
        case "Cancel":
            return RedirectToAction("GetView6");
    }
    return new EmptyResult();
}
```

* 运行http://yourAddress/Test/GetView6，点击AddNew，Save Employee和Cancel按钮，测试相应的功能

### 7.3 实现多重提交按钮的其他方法

* 隐藏 Form 元素

```
<form action="/Employee/CancelSave" id="CancelForm" method="get" style="display:none"> 
</form>
<input type="button" name="BtnSubmit" value="Cancel" onclick="document.getElementById('CancelForm').submit()" />
```

* 使用JavaScript 动态的修改URL

```
<form action="" method="post" id="EmployeeForm" >
<input type="submit" name="BtnSubmit" value="Save Employee" onclick="document.getElementById('EmployeeForm').action = '/Employee/SaveEmployee'" />
<input type="submit" name="BtnSubmit" value="Cancel" onclick="document.getElementById('EmployeeForm').action = '/Employee/CancelSave'" />
</form>
```

* Ajax
使用常规输入按钮来代替提交按钮，并且点击时使用jQuery或任何其他库来产生纯Ajax请求。

## 8. 保存数据库记录，更新表格

* 在EmployeeLayer  中创建 SaveEmployee，如下：

```
public Employee SaveEmployee(Employee e)
{
    MVCDemoDAL dal = new MVCDemoDAL();
    dal.Employees.Add(e);
    dal.SaveChanges();
    return e;
}
```

* TestController修改

```
public ActionResult SaveEmployee(Employee e, string BtnSubmit)
{
    switch (BtnSubmit)
    {
        case "Save Employee":
            EmployeeLayer empBal = new EmployeeLayer();
            empBal.SaveEmployee(e);
            return RedirectToAction("Index");
        case "Cancel":
            return RedirectToAction("Index");
    }
    return new EmptyResult();
}
```

* 运行
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/15.png)
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/15.png)
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/17.png)

## 9. 添加服务器端验证

### 9.1 Model Binder

当Action方法包含元类型参数，Model Binder会与参数名称对比。

* 当匹配成功时，响应接收的数据会被分配给参数
* 匹配不成功时，参数会设置为缺省值，例如，如果是字符串类型则被设置为null，如果是整型则设置为0
* 由于数据类型未匹配异常的抛出，不会进行值分配

当参数为类，Model Binder将通过检索类所有的属性，将接收的数据与类属性名称比较。
当匹配成功时：

* 如果接收的值是空，则会将空值分配给属性，如果无法执行空值分配，会设置缺省值，ModelState.IsValid将设置为fasle
* 如果空值分配成功，会考虑值是否合法，ModelState.IsValid将设置为fasle
* 如果匹配不成功，参数会被设置为缺省值。在本实验中ModelState.IsValid不会受影响

## 9.2  使用 DataAnnotations 装饰属性

* 修改Employee类

```
public class Employee
{
    [Key]
    [Required(ErrorMessage = "Please enter First Name:")]
    public string FirstName { get; set; }

    [StringLength(5, ErrorMessage = "Last Name length should not be greater than 20")]
    public string LastName { get; set; }

    public int Salary { get; set; }
}
```

* 修改TestController中的SaveEmployee方法

```
public ActionResult SaveEmployee(Employee e, string BtnSubmit)
{
    switch (BtnSubmit)
    {
        case "Save Employee":
            if (ModelState.IsValid)
            { 
                EmployeeLayer empBal = new EmployeeLayer();
                empBal.SaveEmployee(e);
            } 
            else
            {
                return View("CreateEmployee");
            }
            return RedirectToAction("GetView6");
        case "Cancel":
            return RedirectToAction("GetView6");
    }
    return new EmptyResult();
}
```

* 修改CreateEmployee.cshtml

```
<form action="/Test/SaveEmployee" method="post">
    <table>
        <tr>
            <td>
                First Name:
            </td>
            <td>
                <input type="text" id="TxtFName" name="FirstName" value="" />
            </td>
        </tr>
        <tr>
            <td colspan="2" align="right">
                @Html.ValidationMessage("FirstName")
            </td>
        </tr>
        <tr>
            <td>
                Last Name:
            </td>
            <td>
                <input type="text" id="TxtLName" name="LastName" value="" />
            </td>
        </tr>
        <tr>
            <td colspan="2" align="right">
                @Html.ValidationMessage("LastName")
            </td>
        </tr>
        <tr>
            <td>
                Salary:
            </td>
            <td>
                <input type="text" id="TxtSalary" name="Salary" value="" />
            </td>
        </tr>
        <tr>
            <td colspan="2" align="right">
                @Html.ValidationMessage("Salary")
            </td>
        </tr>
        <tr>
            <td colspan="2">
                <input type="submit" name="BtnSubmit" value="Save Employee" />
                <input type="submit" name="BtnSubmit" value="Cancel" />
                <input type="button" name="BtnReset" value="Reset" onclick="ResetForm();" />
            </td>
        </tr>
    </table>
</form>
```

* 运行

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/19.png)
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/20.png)

* 遇到的问题
“System.InvalidOperationException”类型的异常在 EntityFramework.dll 中发生，但未在用户代码中进行处理
其他信息: 支持“MVCDemoDAL”上下文的模型已在数据库创建后发生更改。请考虑使用 Code First 迁移更新数据库(http://go.microsoft.com/fwlink/?LinkId=238269)。

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/18.png)

解决方法，在Global.asax文件中在 Application_Start后添加以下语句：

```
Database.SetInitializer(new DropCreateDatabaseIfModelChanges<MVCDemoDAL>());
```