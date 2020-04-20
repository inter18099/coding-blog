---
title: Spring Boot 指南之一之Hello World
category: Java
---

### <div style="color:#FFFFFF;background-color:#0000FF;border-bottom:2px solid #0000FF;padding:4px;width:100%">简介</div>

Spring Boot 使我们更方便得创建独立、工业级的以 Spring 为基础的应用。我们对 Spring 平台和第三方库持保留态度，所以我们可以以最小的代价运行程序。大多数 Spring Boot应用几乎不需要 Spring 配置。

你可以使用 Spring Boot 创建 Java 程序，并使用 java -jar 或者 war 文件来运行。我们也提供一种命令行工具，它可以运行 spring scripts。

我们的目标：

提供真正快速、广泛可用的 Spring 开发的体验。

坚持开箱即用原则，对需要特殊配置的方式持保留态度。

提供一系列对大型应用常见的特性（嵌入服务器、安全、健康检查等）。

杜绝生成冗余代码和 XML 配置。


### <div style="color:#FFFFFF;background-color:#0000FF;border-bottom:2px solid #0000FF;padding:4px;width:100%">系统要求</div>

Spring Boot 2.0.3要求 Java 8 或 9，和 Spring Framework 5.0.7 或更高。Maven 3.2+ 和 Gradle 4 提供显性构建支持。

### <div style="color:#FFFFFF;background-color:#0000FF;border-bottom:2px solid #0000FF;padding:4px;width:100%">Servet 容器</div>

Name | Servlet Version 
------------ | ------------- 
Tomcat 8.5 | 3.1
Jetty 9.4 | 3.1
Undertow 1.4 | 3.1


### <div style="color:#FFFFFF;background-color:#0000FF;border-bottom:2px solid #0000FF;padding:4px;width:100%">安装指南</div>

你可以以传统 Java 库的方式使用 Spring Boot，也就是把 spring-boot-*.jar 加入到classpath中。Spring Boot不要求任何工具整合，你可以用任何 IDE。

虽然你可以复制 Spring Boot jars，但还是推荐使用 Maven 或 Gradle。

#### 安装 Maven

Spring Boot 兼容 Apache Maven 3.2 或更高。如果你还没安装 Maven，可以去 maven.apache.org 下载安装。

Spring Boot 依赖使用 org.springframework.boot 的 groupId。一般情况下，你的 Maven POM 文件会继承 spring-boot-starter-parent 项目，并对一个或更多的 Starters 声明以来。Spring Boot 同样提供一个可选的 Maven 插件来创建可运行的 jars。

以下展示了一个典型的 pom.xml 文件：
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.example</groupId>
	<artifactId>myproject</artifactId>
	<version>0.0.1-SNAPSHOT</version>

	<!-- Inherit defaults from Spring Boot -->
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.0.3.RELEASE</version>
	</parent>

	<!-- Add typical dependencies for a web application -->
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
	</dependencies>

	<!-- Package as an executable jar -->
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
</project>
```

spring-boot-starter-parent 是一个很棒的使用 Spring Boot 的方式，但它并不适用于所有情况。有些时候，你需要继承另一个父 POM 文件，或你压根不喜欢我们的默认设置。这时，请看[Section 13.2.2, “Using Spring Boot without the Parent POM”](https://docs.spring.io/spring-boot/docs/2.0.3.RELEASE/reference/htmlsingle/#using-boot-maven-without-a-parent)使用 import 范围的另一个解决方案。

#### Gradle installation

略

#### 安装 Spring Boot CLI

略



### <div style="color:#FFFFFF;background-color:#0000FF;border-bottom:2px solid #0000FF;padding:4px;width:100%">开发你的第一个 Spring Boot 应用</div>

这一章描述如何开发一个简单的 Hello World web 应用，我们使用 Maven来构建这个项目。

先在命令行运行：

$ java -version

$ mvn -v

来确认是否安装完毕。

#### 创建 POM 文件

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.example</groupId>
	<artifactId>myproject</artifactId>
	<version>0.0.1-SNAPSHOT</version>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.0.3.RELEASE</version>
	</parent>

	<!-- Additional lines to be added here... -->

</project>
```

你可以运行 mvn package 来测试以上代码。


#### 增加 Classpath 依赖

运行
```
$ mvn dependency:tree
```
可以看到，spring-boot-starter-parent 自身没有提供任何依赖。所以把以下代码添加的 pom.xml 文件的 parent 节点下。
```
<dependencies>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId>
	</dependency>
</dependencies>
```
再运行
```
$ mvn dependency:tree
```
可以看到一系列依赖包括 tomcat server，Spring Boot 自身。

#### 写代码

Maven 默认编译 src/main/java 下的源代码，所以你需要创建这个文件结构，然后新建一个文件名为 src/main/java/Example.java，它包括以下代码：

```
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.web.bind.annotation.*;

@RestController
@EnableAutoConfiguration
public class Example {

	@RequestMapping("/")
	String home() {
		return "Hello World!";
	}

	public static void main(String[] args) throws Exception {
		SpringApplication.run(Example.class, args);
	}

}
```

##### @RestController 和 @RequestMapping 注解

对于 @RestController 来说，它首先是一个 web @Controller，所以 Spring 用它处理进入的 web 请求。它同时告诉 Spring 直接输出结果字符串给调用者。

@RequestMapping 注解提供一个路由信息，它告诉 Spring，任何由 / 路径来的 HTTP 请求都由 home 方法处理。

> @RestController 和 @RequestMapping 注解是 Spring MVC 注解（它们不是 Spring Boot独有的）。

##### @EnableAutoConfiguaration 注解

第二个类注解是 @EnableAutoConfiguration。这个注解告诉 Spring 基于你的依赖，来猜测你希望如何设置 Spring。因为 spring-boot-starter-web 添加了 Tomcat 和 Spring MVC，Spring会认为你在开发 web 应用，然后就依此设置 Spring。

##### main 方法

最后一个部分是 main 方法，这是符合 Java 标准的应用入口。main 方法委托调用 Spring Boot 的 SpringApplication类的run方法。SpringApplication 启动应用，开启 Spring，开启自动配置好的 Tomcat。我们需要将 Example.class 传递给 run 方法，让 Spring知道谁是 Spring 组件。


### <div style="color:#FFFFFF;background-color:#0000FF;border-bottom:2px solid #0000FF;padding:4px;width:100%">运行 Spring Boot 应用</div>

在项目根目录输入以下代码来开启应用。
```
mvn spring-boot:run
```
打开你的游览器，进入 localhost:8080，如果看到 Hello World，说明成功。按 Ctrl-c 关闭应用。

注：(此文档依据 [Spring Boot 2.0.3 官方文档](https://docs.spring.io/spring-boot/docs/2.0.3.RELEASE/reference/htmlsingle/))
































