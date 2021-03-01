---
title:Maven配置
categories:
  - Maven
tags:
  - MAVEN
  - java
date:2020-07-12 00:00:00
---
# Maven配置

## 下载Maven

[Maven下载地址](http://maven.apache.org/download.cgi)

![Maven下载地址截图](https://raw.githubusercontent.com/1122pcd1122/My-picture/master/img/image-20210125112711286.png)

## 设置Maven_Home环境变量

变量名:Maven_Home

变量值:C:\java\apache-maven-3.6.3



![环境变量配置](https://raw.githubusercontent.com/1122pcd1122/My-picture/master/img/image-20210125165911397.png)



修改Path值

![path值修改](https://raw.githubusercontent.com/1122pcd1122/My-picture/master/img/image-20210125165911397.png)

## 检查Maven安装

检查命令:mvn -v

![image-20210125170408166](https://raw.githubusercontent.com/1122pcd1122/My-picture/master/img/image-20210125170408166.png)

## 生成.m2文件夹（抄自网页）

网友链接:https://blog.csdn.net/pan_junbiao/article/details/104264644

在cmd中运行 

```java
mvn help:system
```

​	该命令会打印出所有的Java系统属性和环境变量，这些信息对我们日常的变成工作很有帮助。该命令的目的是让Maven执行一个正在的任务。我们可以从命令行输出看到Maven会下载\maven-help-plugin，包括pom文件和jar文件。这些文件都被下载到了Maven本地仓库中。

现在.m2文件夹就生成了,在用户目录下可以找到.m2文件夹，如C:\Users\peichendong\.m2

Maven用户可以选择配置安装目录下的\conf\settings.xml文件或者.m2文件夹下的settings.xml文件。前者是全局范围的，整台机器上的所有用户都会直接受到该配置的影响，而后者是用户范围的，只有当前用户才会受到该配置的影响（推荐）。



## 配置Maven本地仓库

打开maven的conf文件夹下跌setting.xml,添加如下配置(全局配置)

```java
<localRepository>C:/java/maven-repository/repository</localRepository>
```

当前用户配置和全局配置一样

## 配置镜像

在settings.xml文件下的<mirrors>节点中，添加如下配置：

```html
<!-- 配置中央仓库的镜像（改用：阿里云中央仓库镜像）-->
<mirror>        
  <id>alimaven</id>
  <name>aliyun-maven</name>
  <mirrorOf>central</mirrorOf>
  <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```