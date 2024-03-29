# 65-Java-如何使用Maven构建工程.md

#### 创建Maven工程前知识准备：
+ 什么是maven？

maven:中央仓库 编译，打包测试，部署 一体化 [maven官网](https://maven.apache.org)

![](65-Images/maven的中央仓库.png)

![](65-Images/18.png)

无须下载安装，Eclipse已经集成咯maven环境

![](65-Images/19.png)

![](65-Images/20.png)

Maven的中央仓库：https://repo.maven.apache.org/maven2/


![](65-Images/21.png)

+ 查看Eclipse中Maven默认本地仓库

window ---> Preferences ---> Maven ---> User Settings

![](65-Images/22.png)

从中可以看到maven默认本地仓库存放在C盘，那么我们需要更改其默认路径，那么如何更改呢？参考博客[Java-如何设置Maven本地仓库不使用C盘默认仓库](https://www.jianshu.com/p/056ae13ed684)

#### 创建Maven工程

1. 使用Eclipse新建一个Maven工程

![](65-Images/1.png)

![](65-Images/2.png)

![](65-Images/3.png)

```
备注：
Group Id: 公司名
Aftifact Id: 工程名
Packaging:war(web工程) 
使用springboot jar包war包都可以

点击 Finish 之后稍作等待。。。生成工程目录结构
```
**如图所示为Maven工程的目录结构**

![](65-Images/4.png)

**Java Resources/src/main/java 放入Java代码** 如图所示：

![](65-Images/5.png)

**Java Resources/src/main/resources 资源文件** 如图所示：

![](65-Images/6.png)

**Java Resources/src/test/java** 测试类存放处

**Java Resources/src/test/resources** 测试时的配置文件

```
备注：
测试的这堆东西也就是项目进行时测试时所用，项目部署时不作发布
```
**src/main/webapp** 放前端页面

2. 生成web.xml文件

![](65-Images/7.png)

![](65-Images/8.png)

3. 新建一个index.jsp页面

![](65-Images/9.png)

**Jsp 报错如下：**

![](65-Images/10.png)

```
The superclass "javax.servlet.http.HttpServlet" was not found on the Java Build Path
```
**解决方法一：**

![](65-Images/11.png)

![](65-Images/12.png)

![](65-Images/13.png)

**解决方法二：** 导入对应的包，在pom.xml中配置

![](65-Images/14.png)

![](65-Images/15.png)

![](65-Images/16.png)

![](65-Images/17.png)

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.tencent</groupId>
  <artifactId>demo_springMVC_maven</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>war</packaging>
  
  <!-- 怎么导包呢？ -->
  <dependencies>
	  <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
	<dependency>
	    <groupId>javax.servlet</groupId>
	    <artifactId>javax.servlet-api</artifactId>
	    <version>3.1.0</version>
	    <scope>provided</scope>
	</dependency>
  </dependencies>
</project>
```
只要一保存，将会自动下载jar包,如图所示仓库已下载。

![](65-Images/23.png)

4. 打开目录 Java Resources ---> Libraries 目录下发现我的目录下居然没有 Maven Dependencies目录

**解决办法**

Build Path ---> Configure Build Path ---> Java Build Path ---> Maven Dependencies ---> Edit ---> Maven Project settings ---> 去掉勾选Resolve dependencies from Workspace projects ---> yes ---> Apply ---> Apply and Close ---> Finish ---> Apply ---> Apply and Close

![](65-Images/24.png)

5. 运行Maven工程

**第一种：**

右键项目 ---> Run As ---> 1 Run on Server 如图所示启动成功

![](65-Images/25.png)

**第二种：部署Maven项目**

+ 先停之前的Server

+ 在pom.xml里面声明内嵌一个Tomcat服务器

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.tencent</groupId>
  <artifactId>demo_springMVC_maven</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>war</packaging>
  
  <!-- 怎么导包呢？ -->
  <dependencies>
	  <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
	<dependency>
	    <groupId>javax.servlet</groupId>
	    <artifactId>javax.servlet-api</artifactId>
	    <version>3.1.0</version>
	    <scope>provided</scope>
	</dependency>
  </dependencies>
  	<!-- 配置build环境 -->
   <build>
       <plugins>
          <plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.2</version>
				<configuration>
					<source>1.7</source>
					<target>1.7</target>
					<encoding>utf-8</encoding>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.apache.tomcat.maven</groupId>
				<artifactId>tomcat7-maven-plugin</artifactId>
				<version>2.2</version>
				<configuration>
					<port>8080</port>
					<path>/demo_springMVC_maven</path>
				</configuration>
			</plugin>      
       </plugins>
  </build> 
</project>
```
一、 更新下Maven

项目右键 ---> Maven ---> Update Project 

![](65-Images/26.png)

其目的让其检查依赖

二、启动Maven项目

![](65-Images/27.png)

![](65-Images/29.png)

报错信息如下 ：

```
Unknown lifecycle phase "mvn". You must specify a valid lifecycle phase or a goal in the format <plugin-prefix>:<goal> or <plugin-group-id>:<plugin-artifact-id>[:<plugin-version>]:<goal>. Available lifecycle phases are: validate, initialize, generate-sources, process-sources, generate-resources, process-resources, compile, process-classes, generate-test-sources, process-test-sources, generate-test-resources, process-test-resources, test-compile, process-test-classes, test, prepare-package, package, pre-integration-test, integration-test, post-integration-test, verify, install, deploy, pre-clean, clean, post-clean, pre-site, site, post-site, site-deploy. -> [Help 1]
```
![](65-Images/28.png)

**原因：Eclipse已经集成Maven环境，无需要输入mvn命令，正确的在Goals中输入应该是 tomcat7:run**

如图所示，启动成功：

![](65-Images/30.png)

复制其URL在浏览器中输入回车，如图所示Maven工程已运行。

![](65-Images/31.png)


**以上就是我关于 Java-如何使用Maven构建工程  知识点的整理与总结的全部内容 [另附源码](https://github.com/javaobjects/demo_springMVC_maven)**

==================================================================
#### 分割线
==================================================================

**博主为咯学编程：父母不同意学编程，现已断绝关系;恋人不同意学编程，现已分手;亲戚不同意学编程，现已断绝来往;老板不同意学编程,现已失业三十年。。。。。。如果此博文有帮到你欢迎打赏，金额不限。。。**

![](https://upload-images.jianshu.io/upload_images/5227364-e76764b127f255ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)