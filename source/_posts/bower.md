---
title: 'Bower的使用'
date: 2018-05-24 11:21:24
tags:
---

标签（空格分隔）： C#

---

大多数客户端的JavaScript库都可以通过Bower使用。Bower提供了成千上万的Javascript客户端库。

### 1. 添加Bower文件
- 使用“项模板” Bower Configuration File，可以把Bower听啊及到Asp.net Web项目中。
如下bower.json所示：
```
{
  "name": "asp.net",
  "private": true,
  "dependencies": {
    "bootstrap": "3.3.6",
    "jquery": "2.2.0",
    "jquery-validation": "1.14.0",
    "jquery-validation-unobtrusive": "3.2.6"
  }
}
```
- 向项目中Bower也会添加.bowerrc文件，用来配置Bower。默认情况下，使用directory设置时，脚本文件（以及脚本库附带的CSS和HTML文件）会被复制到wwwroot/lib目录。如下.bowerrc所示：
```
{
  "directory": "wwwroot/lib"
}

```

### 2. 通过“管理Bower程序包”管理Bower
右键点击Bower文件，选择“管理Bower程序包”，则可以开始管理Bower包了。如下图所示：
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/bower/1.png)