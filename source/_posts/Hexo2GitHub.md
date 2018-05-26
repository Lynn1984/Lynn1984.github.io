---
title: Windows下Hexo+GitHub环境搭建
date: 2018-05-18 11:55:57
tags:
---

标签（空格分隔）： hexo

---

## 1. 安装Node.js  

- 下载地址：https://nodejs.org/en/
- 可以通过node -v的命令来测试NodeJS是否安装成功 

## 2. 安装Git
	
- 下载地址：https://git-scm.com/
- 去 Git 官网下载相应版本，进行安装即可

## 3. 注册Github账号

- 去 Github 官网进行注册即可
- 注册完之后记得添加 SSH Key
- 这个 SSH Key是一个认证，让github识别绑定这台机器，允许这台机器提交
- 在Git Bash执行代码：ssh-keygen -t rsa -C "email address"
- 成功后会生成两个文件id_rsa 以及id_rsa.pub
- 添加id_rsa.pub中的内容到Github的SSH Key中。

## 4. 部署到Github上

### 4.1 编辑本地Hexo目录下文件_comfig.yml

- deploy:
- type: git
- repository: https://github.com/Lynn1984/Lynn1984.github.io.git
- branch: master
- 说明：repository复制Github中的即可。仓库名=用户名+github.io 必须的，不然会访问失败。

### 4.2 部署命令

```
$ hexo generate
$ hexo deploy
```

### 4.3 保存到Github

```
$ npm install hexo-deployer-git --save
```

## 5. hexo源码提交到Github
master 分支是存放 Hexo 博客生成的页面，所以源码另建一个分支用来存放源码
实际备份中，.deploy_git、public文件夹和我们的master分支内容重复
.gitignore文件添加

```
.deploy_git
public  
_config.yml
```