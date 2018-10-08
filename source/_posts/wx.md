---
title: 海迪云会员第三方接入介绍
date: 2018-08-01 12:55:08
tags:
---

标签（空格分隔）： 海迪云会员系统（仅供内部使用）

---

# 开发模式

* 海迪云会员系统采用微信开放平台中的第三方平台开发模式，即海迪云以作为第三方角色，来为其他广大公众号提供运营服务和行业解决方案。
* 其他广大公众号提供者（即海迪云渠道下的门店商户），需要授权给海迪云开放平台，让海迪云开放平台可以对其公众号进行开发操作。
* 一般设计的权限有
    * 消息管理权限：
    * 自定义菜单权限
    * 网页服务权限
    * 群发与通知权限
    * 用户管理权限
    * 微信卡券权限
* 权限说明详细链接
    https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=open1419318459&lang=zh_CN

# 目的
让海迪云的会员系统与微信会员打通，做到共享一个后台，并且海迪云会员系统还可以单独运转。

# 公众号授权给海迪云开放平台

在浏览器中输入http://member.haidiyun.top/wx/Default.aspx 链接
* 把该页面的返回链接在微信中发送给公众号的管理员
* 请公众号的管理员在微信中打开链接，并且点击”授权“按钮，进行授权
* 如下图：

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/cms/1.png)


# 自定义菜单管理

* 在授权的过程中，如果有进行“自定义菜单权限”权限授权，海迪云会员系统就可以在后台完全操作该公众号的自定义菜单了。

海迪云会员系统操作界面：
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/cms/4.png)
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/cms/5.png)

* 由于有些授权处理的公众号已经有自己的菜单，亦可以让公众号管理员在公众号中手动添加菜单。
例如“芳园四季”公众号，就需要手动添加菜单，加入海迪云会员链接，就可以接入海迪云的会员系统会员中心功能（该功能会持续完善），操作步骤如下：
登录公众号，选择“自定义菜单功能”

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/cms/2.png)

* 新增自定义菜单
海迪云为“芳园四季”提供的会员中心的地址为：
http://member.haidiyun.top/Wx/MemberCenter.aspx?orgId=20
该地址在添加自定义菜单时填入。如下图（海迪云为不同的公众号提供的链接均是不同的，上面链接仅供“芳园四季”使用）：
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/cms/3.png)

# 文档完善中......