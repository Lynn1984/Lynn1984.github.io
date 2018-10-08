---
title: 新增“用餐历史”新菜单
date: 2018-06-25 09:32:13
tags:
---

标签（空格分隔）： 会员管理系统（仅供内部使用）

---

## 新增“用餐历史”三级菜单

- 进入主界面，点击“系统管理”，点击“菜单”，如下图：

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/member/1.png)

- 在“菜单”页面树形菜单中选择“会员管理”，再选择“会员”，在该级目录下添加一条“用餐历史”的记录，如下图：

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/member/2.png)

- 查找Sys_Menu表中“用餐历史”对应的MenuId

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/member/14.png)

- 在MenuGuid.cs中把MeunId加入

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/member/15.png)

## 创建“用餐历史”数据库表

- 给每个字段都添加说明

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/member/5.png)

- 创建表成功后，添加表的说明

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/member/6.png)

- 字段说明和表的说明必须要添加，否则自动代码生成器会识别失败，不能自动创建需要的文件

## 利用代码生成器，生成基础文件

- MMS\SourceCode\RDFGenerateCode为代码生成器项目，直接运行项目:

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/member/7.png)

- 填好需要连接的数据库信息，选择需要生成代码的数据表。下图红框中的数据是自动加载的，则表明数据表 有按原则创建：

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/member/8.png)

- 在对应的目录下创建ConsumeHistory目录，拷贝生成的类文件到ConsumeHistory中，如下图：

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/member/9.png)

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/member/10.png)

- 把生成的html文件拷贝到UI项目中去，具体看图：

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/member/11.png)

## 增加权限设置

- 给超级管理员增加完整权限
- 已经审核的数据不能直接修改，需要反审核后进行修改

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/member/3.png)

## 设置菜单表选项

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/member/4.png)