---
title: maven学习
date: 2017-11-13 15:22:53
tags:
---
<strong>Maven项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的软件项目管理工具。</strong>
<!--more-->
# 1、pom.xml详解
```xml
<groupId>com.medical.maven</groupId><!-- 项目的包名-->
<artifactId>模块名</artifactId>
<version>0.0.1-SNAPSHOT</version><!-- 版本号 -->
```
依赖：
```xml
<dependencies>  
    <dependency>  
        <groupId>junit</groupId>  
        <artifactId>junit</artifactId>  
        <version>4.11</version>  
    </dependency> 
</dependencies>
```

# 2、maven常用的构建命令
> mvn -v : 查看版本；
> mvn compile : 编译
> mvn test : 测试
> mvn package : 打包
> mvn clean : 删除target(编译项目主代码)
> mvn install : 安装jar包到本地仓库

# 3、maven的生命周期
一个完整的项目构建过程包括清理、编译、 测试、打包、集成测试、验证、部署
<strong>maven生命周期：</strong>
1. clean :清理项目；
1.1 pre-clean : 执行清理前的工作；
1.2 clean : 情理上一次构件生成的所有文件
1.3 post-clean : 执行清理后的文件
2. default :构建项目（核心）；
比如compile、test、package、install等等
3. site :生成项目站点；
3.1 pre-site : 在生成项目站点前要完成的工作；
3.2 site : 生成项目的站点文档
3.3 post-site : 在生成项目站点后要完成的工作
3.4 site-deploy : 发布生成的站点到服务器上

# 4、运用Maven的plugin:jetty来部署
1. 在节点<build><plugins>…</plugins></build>中配置Jetty插件依赖如下：
```xml
<plugin>
	<groupId>org.mortbay.jetty</groupId>
	<artifactId>maven-jetty-plugin</artifactId>
	<version>6.1.26</version>
	<configuration>
		<webAppSourceDirectory>${basedir}/src/main/webapp</webAppSourceDirectory>
	</configuration>
</plugin>
```
2. 右击项目 –> Run As –> Maven Build… –> Goals  输入：jetty:run 即可：
3. 打开浏览器输入：http://localhost:8080/myweb/ 即可验证web项目是否启动正常。

# 5、使用Tomcat部署
```xml
<plugin>
    <groupId>org.apache.tomcat.maven</groupId>  
    <artifactId>tomcat7-maven-plugin</artifactId>  
    <version>2.2</version>
    <executions>
    <execution>
    <!-- 在打包成功后使用jetty:run来运行jrtty服务 -->
    <phase>package</phase>
    <goals>
        <goal>run</goal>
    </goals>
    </execution>
    </executions>
</plugin>
```

# 6、在打包成功后使用jetty:run来运行jrtty服务(tomcat同)
```xml
<plugin>
    <groupId>org.mortbay.jetty</groupId>
    <artifactId>maven-jetty-plugin</artifactId>
    <version>6.1.26</version>
    <executions>
    <execution>
    <!-- 在打包成功后使用jetty:run来运行jrtty服务 -->
    <phase>package</phase>
    <goals>
        <goal>run</goal>
    </goals>
    </execution>
    </executions>
</plugin>
```
右击项目 –> Run As –> Maven Build… –> Goals  输入：clean package 即可

# 7、使用maven构建web项目
1. new -> project -> maven project ->webapp...
2. new -> source folder ->java、test/java、test/resource
3. 右击项目 -> proporities ->project Facted ->

