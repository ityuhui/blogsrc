title: 使用spring boot构建web app

date: 2014-05-31 15:30:28

categories:
- Java

tags:
- spring
- web development

---

在2014年5月，流行的Java web框架可能只有struts2和springMVC了。

spring是一个非常大的项目组合，几乎涵盖了java web开发领域的各个方面。目前官方推荐的使用spring boot来开发web app。另外，官方的例子都使用了gradle工具来进行build和依赖管理，由于我找不到一个好用的gradle plugin for eclipse, 所以，我仍然使用了maven(m2eclipse)。

<!--more-->

### 安装和配置好m2eclipse

### 创建一个空的java项目，当然也可以使用maven创建项目。

### 将项目转为maven项目，注意pom.xml一定要放到项目的根目录下。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.yuhui.webapp</groupId>
	<artifactId>YangCheJi</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>
	<name>YangCheJi</name>
 
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.0.2.RELEASE</version>
	</parent>
 
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.17</version>
		</dependency>
		<dependency>
			<groupId>ch.qos.logback</groupId>
			<artifactId>logback-classic</artifactId>
			<version>1.0.9</version>
		</dependency>
	</dependencies>
	<properties>
		<start-class>hello.Application</start-class>
	</properties>
 
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
 
	<repositories>
		<repository>
			<id>spring-milestone</id>
			<url>http://repo.spring.io/libs-milestone</url>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
		</repository>
	</repositories>
 
	<pluginRepositories>
		<pluginRepository>
			<id>spring-milestone</id>
			<url>http://repo.spring.io/libs-milestone</url>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
		</pluginRepository>
	</pluginRepositories>
</project>
```

### 创建目录

src/main/java/hello

src/main/resources/templates

### 创建文件

src/main/java/hello/GreetingController.java

```java
package hello;
 
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
 
@Controller
public class GreetingController {
	
    @RequestMapping("/greeting")
    public String greeting(@RequestParam(value="name", required=false, defaultValue="World") String name, Model model) {
        model.addAttribute("name", name);
        return "greeting";
    }
}
```

src/main/java/hello/Application.java

```java
package hello;
 
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.SpringApplication;
import org.springframework.context.annotation.ComponentScan;
 
@ComponentScan
@EnableAutoConfiguration
public class Application {
 
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
 
}
```

src/main/resources/templates/greeting.html

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Getting Started: Serving Web Content</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
    <p th:text="'hi, ' + ${name} + '!'" />
</body>
</html>
```

### run as maven "spring-boot:run"，访问http://127.0.0.1:8080/greeting

### 生成了一个jar文件，也可以用命令行启动, java -jar XX.jar