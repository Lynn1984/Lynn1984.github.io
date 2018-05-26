---
title: 'Asp.net MVC模式（一）'
date: 2018-05-23 14:19:24
tags:
---

标签（空格分隔）： C#

---

ASP.NET 是新一代 ASP 。它与经典 ASP 是不兼容的，但 ASP.NET 可能包括经典 ASP。
ASP.NET 页面是经过编译的，这使得它们的运行速度比经典 ASP 快。
ASP.NET 具有更好的语言支持，有一大套的用户控件和基于 XML 的组件，并集成了用户身份验证。
ASP.NET 页面的扩展名是 .aspx ，通常是用 VB (Visual Basic) 或者 C# (C sharp) 编写。
在 ASP.NET 中的控件可以用不同的语言（包括 C++ 和 Java）编写。
当浏览器请求 ASP.NET 文件时，ASP.NET 引擎读取文件，编译和执行脚本文件，并将结果以普通的 HTML 页面返回给浏览器。

## 1. 创建MVC项目
### 1.1 在 New Project 对话框中:
- 打开Visual C#模板
- 选择模板 ASP.NET Web Application
- 选择MVC模式
- 创建MVCDemo项目
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/1.png)
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/5.png)

## 2. MVC 文件夹
### 2.1 应用程序信息
- Properties
- References

### 2.2 应用程序文件夹
- App_Data 文件夹 ：用于存储应用程序数据
- Content 文件夹 ：用于存放静态文件，比如样式表（CSS 文件）、图标和图像
- Controllers 文件夹 ：包含负责处理用户输入和响应的控制器类
- Models 文件夹
- Scripts 文件夹 ：存储应用程序的 JavaScript 文件
- Views 文件夹

### 2.3 配置文件
- Global.asax
- packages.config
- Web.config

## 3. 样式和布局
### 3.1添加布局
    文件 _Layout.cshtml 表示应用程序中每个页面的布局。它位于 Views 文件夹中的 Shared 文件夹。
    打开文件 _Layout.cshtml，添加自己的布局。

### 3.2 HTML帮助器
在上面的代码中，HTML 帮助器用于修改 HTML 输出：
@Url.Content() - URL 内容将在此处插入。
@Html.ActionLink() - HTML 链接将在此处插入。

### 3.3 Razor 语法
在上面的代码中，红色标记的代码是使用 Razor 标记的 C#。
@ViewBag.Title - 页面标题将在此处插入。
@RenderBody() - 页面内容将在此处呈现。

### 3.4 添加样式
应用程序的样式表是 Site.css，位于 Content 文件夹中。
打开文件 Site.css，把内容替换成自己的样式。

### 3.5 _ViewStart 文件
Shared 文件夹（位于 Views 文件夹内）中的 _ViewStart 文件包含如下内容：
```
@{Layout = "~/Views/Shared/_Layout.cshtml";}
```
这段代码被自动添加到由应用程序显示的所有视图。
如果您删除了这个文件，则必须向所有视图中添加这行代码。

## 4. 控制器
### 4.1 Controllers 文件夹
MVC 要求所有控制器文件的名称以 "Controller" 结尾。
例如：HomeController.cs（用于 Home 页面和 About 页面）和AccountController.cs （用于登录页面）。
Web 服务器通常会将进入的 URL 请求直接映射到服务器上的磁盘文件。例如：URL 请求 "http://www.w3cschool.cc/index.php" 将直接映射到服务器根目录上的文件 "index.php"。
MVC 框架的映射方式有所不同。MVC 将 URL 映射到方法。这些方法在类中被称为"控制器"。
控制器负责处理进入的请求，处理输入，保存数据，并把响应发送回客户端。

### 4.2 添加控制器
右键点击Controllers文件夹-》添加——》控制器：
名称中必须以Controller结尾，不可以删除修改。
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/4.png)
```
namespace MVCDemo.Controllers
{
    public class TestController : Controller
    {
        // GET: Test
        public ActionResult Index()
        {
            return View();
        }

        public string GetString()
       {
            return "Welcome to  my home!)";
        }
    }
}
```

### 4.3 运行
按 F5 键，在地址栏中以“Test/GetString”这样的形式输入
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/6.png)

## 5.视图
### 5.1 Views 文件夹
Views 文件夹存储的是与应用程序显示（用户界面）相关的文件（HTML 文件）。根据所采用的语言内容，这些文件可能扩展名可能是 html、asp、aspx、cshtml 和 vbhtml。
Views 文件夹中包含每个控制器对应的一个文件夹。
在 Views 文件夹中，Visual Web Developer 已经创建了一个 Account 文件夹、一个 Home 文件夹、一个 Shared 文件夹。
Account 文件夹包含用于用户账号注册和登录的页面。
Home 文件夹用于存储诸如 home 页和 about 页之类的应用程序页面。
Shared 文件夹用于存储控制器间分享的视图（母版页和布局页）。

### 5.2 控制器中返回View
* 在TestController中添加新的Action
```
public ActionResult GetView()
{
    return View("MyView");
}
```
* 在代码中右键点击GetView，添加试图，取消选择“使用布局”的复选框，点击添加：
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/CS/7.png)
* 在Views->Test文件夹下面会生成MyView.cshtml文件,如下所示：
```
@{
    Layout = null;
}

<!DOCTYPE html>

<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>MyView</title>
</head>
<body>
    <div> 
    </div>
</body>
</html>
```
* 将View 放在Shared文件夹中所有的Controller都可用。

## 6. 模型
### 6.1 在GetView 方法中创建Employee 对象
```
Models.Employee employee = new Models.Employee();
employee.FirstName = "Lynn";
employee.LastName = "Jiang";
employee.Salary = 30000;
```
### 6.2 展示View
* ViewData展示方式

Controller代码块：
```
        public ActionResult GetView()
       {
            Models.Employee employee = new Models.Employee();
            employee.FirstName = "Lynn";
            employee.LastName = "Jiang";
            employee.Salary = 30000;
            ViewData["Employee"] = employee;
            return View("MyView");
        }
```
View代码块：
```
    <div> 
        @{
            MVCDemo.Models.Employee employee = (MVCDemo.Models.Employee)
            ViewData["Employee"];
        }

        <b>Employee Details </b>
        <br />
        Employee Name: @employee.FirstName@employee.LastName
        <br />
        Employee Salary: @employee.Salary.ToString("C")
    </div>
```

* ViewBag展示方式

Controller代码块：
```
        public ActionResult GetView2()
        {
            Models.Employee employee = new Models.Employee();
            employee.FirstName = "Lynn";
            employee.LastName = "Jiang";
            employee.Salary = 30000;
            ViewBag.Employee = employee;
            return View("MyView2");
        }
```
View代码块：
```
    <div> 
        @{
            MVCDemo.Models.Employee employee = (MVCDemo.Models.Employee)
            ViewData["Employee"];
        }

        <b>Employee Details </b>
        <br />
        Employee Name: @employee.FirstName@employee.LastName
        <br />
        Employee Salary: @employee.Salary.ToString("C")
    </div>
```

* 强类型View方式

Controller代码块：
```
        public ActionResult GetView3()
        {
            Models.Employee employee = new Models.Employee();
            employee.FirstName = "Lynn";
            employee.LastName = "Jiang";
            employee.Salary = 30000;
            return View("MyView3", employee);
        }
```
View代码块：
```
@model MVCDemo.Models.Employee //放在View的顶部
...
    <div> 
        <b>Employee Details </b>
        </br >
        Employee Name : @Model.FirstName @Model.LastName
        </br >
        @if (Model.Salary > 15000)
        {
            <span style="background-color:yellow">
                Employee Salary: @Model.Salary.ToString("C")
            </span>
        }
        else
        {
            <span style="background-color:green">
                Employee Name : @Model.FirstName @Model.LastName
            </span>
        }     
    </div>
```

### 6.3 使用ViewModle的强类型View

* 在项目下创建ViewModels文件夹
* 在ViewModels文件夹下创建EmployeeViewModel类

```
namespace MVCDemo.ViewModels
{
    public class EmployeeViewModel
    {
        public string EmployeeName { get; set; }
        public string Salary { get; set; }
        public string SalaryColor { get; set; }
        public string UserName { get; set; }
    }
}
```
* Controller GetView4代码
```
        public ActionResult GetView4()
        {
            Employee employee = new Employee();
            employee.FirstName = "Lynn";
            employee.LastName = "Jiang";
            employee.Salary = 30000;

            EmployeeViewModel vmEmp = new EmployeeViewModel();
            vmEmp.EmployeeName = employee.FirstName + " " + employee.LastName;
            vmEmp.Salary = employee.Salary.ToString("C");
            if (employee.Salary > 15000)
            {
                vmEmp.SalaryColor = "yellow";
            }
            else
            {
                vmEmp.SalaryColor = "green";
            }
            return View("MyView4", vmEmp);
        }
```
* View中使用ViewModel
```
@model MVCDemo.ViewModels.EmployeeViewModel
...
    <div> 
        <b>Employee Details </b>
        </br >
        Employee Name : @Model.EmployeeName
        </br >
        <span style="background-color:@Model.SalaryColor">
         Employee Salary: @Model.Salary
        </span>
    </div>
```

### 6.4 带有集合的View
* 修改EmployeeViewModel 类，删除UserName属性

```
namespace MVCDemo.ViewModels
{
    public class EmployeeViewModel
    {
        public string EmployeeName { get; set; }
        public string Salary { get; set; }
        public string SalaryColor { get; set; }
    }
}
```

* 在ViewModels 文件下，创建新类并命名为EmployeeListViewModel

```
namespace MVCDemo.ViewModels
{
    public class EmployeeListViewModel
    {
        public List<EmployeeViewModel> Employees { get; set; }
        public string UserName { get; set; }
    }
}
```

* 在ViewModels 文件下，创建新类并命名为EmployeeLayer

```
namespace MVCDemo.ViewModels
{
    public class EmployeeLayer
    {
        public List<Employee> GetEmployees()
        {
            List<Employee> employees = new List<Employee>();
            Employee emp = new Employee();
            emp.FirstName = "Miss";
            emp.LastName = " Wang";
            emp.Salary = 14000;
            employees.Add(emp);

            emp = new Employee();
            emp.FirstName = "Mr";
            emp.LastName = "White";
            emp.Salary = 16000;
            employees.Add(emp);

            emp = new Employee();
            emp.FirstName = "Mrs";
            emp.LastName = " Liu";
            emp.Salary = 20000;
            employees.Add(emp);

            return employees;
        }
    }
}
```

* 在Controller中添加GetView6

```
        public ActionResult GetView6()
        {
            EmployeeListViewModel employeeListViewModel = new EmployeeListViewModel();

            EmployeeLayer empBal = new EmployeeLayer();
            List<Employee> employees = empBal.GetEmployees();

            List<EmployeeViewModel> empViewModels = new List<EmployeeViewModel>();

            foreach (Employee emp in employees)
            {
                EmployeeViewModel empViewModel = new EmployeeViewModel();
                empViewModel.EmployeeName = emp.FirstName + " " + emp.LastName;
                empViewModel.Salary = emp.Salary.ToString("C");
                if (emp.Salary > 15000)
                {
                    empViewModel.SalaryColor = "yellow";
                }
                else
                {
                    empViewModel.SalaryColor = "green";
                }
                empViewModels.Add(empViewModel);
            }
            employeeListViewModel.Employees = empViewModels;
            employeeListViewModel.UserName = "Admin";
            return View("MyView6", employeeListViewModel);
        }
```

* 展示在View中

```
@using MVCDemo.ViewModels
@model EmployeeListViewModel

@{
    Layout = null;
}

<!DOCTYPE html>

<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>MyView</title>
</head>
<body>
    Hello @Model.UserName
    <hr />
    <div>
        <table>
            <tr>
                <th>Employee Name</th>
                <th>Salary</th>
            </tr>
            @foreach (EmployeeViewModel item in Model.Employees)
            {
                <tr>
                    <td>@item.EmployeeName</td>
                    <td style="background-color:@item.SalaryColor">@item.Salary</td>
                </tr>
            }
        </table>
    </div>
</body>
</html>
```

大家在进行ASP.NET MVC的开发时，还可以借助一些开发工具。ComponentOne Studio ASP.NET MVC 是一款轻量级控件，与Visual Studio无缝集成，完全与MVC6和ASP.NET 5.0兼容，将大幅提高工作效率。