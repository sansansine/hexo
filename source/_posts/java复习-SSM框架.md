---
title: java复习--SSM框架
date: 2017-09-15 09:16:16
tags:
---
![logo](JavaReview.png)
<strong>由于现在的工作是前端，毕业设计之后半年都没有写过Java，但又不想就此荒废掉，便趁空隙复习并学习一下如今比较常用的SSM框架，整合三大框架并简单应用。</strong>
<!--more-->
# 1、SSM的基本概念
SSM是由Spring、SpringMVC、MyBatis三大框架构成
1. Spring
Spring由Rod Johnson创建，是为解决业务逻辑层和其他各层的松耦合问题，Spring是一个分层的JavaSE/EEfull-stack(一站式) 轻量级开源框架。简单来说，Spring是一个轻量级的控制反转（IoC）和面向切面（AOP）的容器框架。
2. SpringMVC
Spring MVC属于SpringFrameWork的后续产品，已经融合在Spring Web Flow里面。Spring 框架提供了构建 Web 应用程序的全功能 MVC 模块。使用 Spring 可插入的 MVC 架构，从而在使用Spring进行WEB开发时，可以选择使用Spring的SpringMVC框架或集成其他MVC开发框架，如Struts1，Struts2等。
3. MyBatics
MyBatis 是一款优秀的持久层框架，它支持定制化 SQL、存储过程以及高级映射。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 POJOs(Plain Old Java Objects,普通的 Java对象)映射成数据库中的记录。
# 2、环境搭建（MAC）
1.安装JDK
去官网下载Mac专用的JDK并安装，然后在命令行使用<strong>open .bash_profile</strong>打开.bash_profile文件，配置环境变量如下：
![environment](environment.png)
在命令行使用<strong>source .bash_profile</strong>使得修改立即生效，环境变量配置完成后，在终端输入java -version，若有如下
![version](javaVersion.png)
版本号信息出现即为安装成功。
2.下载eclipse for Java EE
官网下载，直接解压即可
3.修改eclipse里面默认的mac的jdk 
Eclipse -> preferences -> java -> installed JRES 修改为我们下载的JDK地址即可
4.安装tomcat
官网下载，并配置环境（和配置JDK环境变量类似）
![tomcat](tomcat.png)
  然后在终端进入Tomcat的bin目录下执行 sh startup.sh启动Tomcat，在浏览器输入http://localhost:8080 若能进入Tomcat页面即为安装成功。
  最后在eclipse中配置Tomcat：eclipse -> preferences -> Server -> Runtime Environments -> add配置Tomcat安装目录 -> 新建Server
# 3、SSM框架整合
1.新建Web项目
2.引入SSM需要的JAR包


