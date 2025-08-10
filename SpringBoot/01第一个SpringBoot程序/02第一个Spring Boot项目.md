# 一、第一个Spring Boot项目
需求：在浏览器上输入请求路径 http://localhost:8080/hello，在浏览器上显示 HelloWorld!

使用Spring Boot3 开发web应用，实现步骤如下：

## 第一步：创建一个空的工程，并设置JDK版本21（Spring Boot 3要求JDK最低版本是17）  
![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726108555353-e1941b34-dc63-4767-954c-4f6e5e3a9aa8.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726108673944-7c20a530-a244-4258-a98a-d8ef9c6d04da.png)




## 第二步：设置maven
![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726109769284-7a5cae72-e931-4fd3-bf1c-c99a37cb4023.png)





## 第三步：创建一个Maven模块 sb3-01-first-web
![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726110490826-f7c563eb-4aff-488e-83dc-8daf82a700e9.png)

## 第四步：打开Spring Boot 3官方文档，按照文档一步一步进行
![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726111379585-fd90758d-f6bd-4a71-9ba9-b1549dd18c84.png)





![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726111411632-0a5cfc79-f222-4395-ae19-35b8d3868f3d.png)




## 第五步：要使用Spring Boot 3，需要继承这个开源项目。从官方指导文档中复制以下内容：
![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726111736067-221c85d6-e3ce-4d41-9b20-2893705b11de.png)

```xml
<!--继承Spring Boot 3.3.3开源项目-->
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>3.3.3</version>
</parent>
```
* **使用SpringBoot框架，首先要继承SpringBoot这个父工程。(后面会讲为什么不是引入SpringBoot依赖而是继承SpringBott工程)**
* **我们开发的每一个项目其实可以看做是 Spring Boot 项目下的子项目。**




## 第六步：添加Spring Boot的web starter
![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726112199621-1d90cda6-1bb5-4c66-ac9f-b0f2bedb87b3.png)
* **做web开发就需要引入web启动器。引入web启动器，这样的话自动会将web开发相关的所有依赖全部引入，例如：json、tomcat、springmvc等，包括这些依赖的版本也不需要我们管理，自动管理**

* 在parent下立即添加如下配置，让Spring Boot项目具备开发web应用的依赖：

```xml
<dependencies>
    <!--引入Spring Boot web启动器依赖-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```



关联的依赖也被引入进来，如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726112541453-f0298719-9217-4792-9af5-bb39d6582560.png)

可以看到spring mvc被引入了，tomcat服务器也被引入了。

## 第七步：编写Spring Boot主入口程序
```java
package com.powernode.springboot3;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
//所有的Springboot应用的主入口程序必须使用@SpringBootApplication注解进行标注
@SpringBootApplication
public class MyApplication {

    public static void main(String[] args) {
	    //主入口，运行main方法，启动服务器。其中参数是主入口类的class对象，还有一个是main方法的参数args
        SpringApplication.run(MyApplication.class, args);
    }
}
```



## 第八步：编写controller
```java
package com.powernode.springboot3.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
//Controller要想被spring容器管理，该类的位置必须要和SpringBoot主入口程序再同级目录下或者子目录下
@RestController
public class MyController {

    @RequestMapping("/hello")
    public String index(){
        return "Hello World!";
    }
    
}

```
* 

## 第九步：运行main方法就是启动web容器
![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726197869192-497ee49e-fffb-4201-9c20-6caa97823963.png)





## 第十步：打开浏览器访问
![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726113139402-b64d1c16-9055-495c-aa3d-1ef13baf9816.png)

# 二、便捷的部署方式
## 打jar包运行
**Spring Boot提供了打包插件，可以将Spring Boot项目打包为可执行 jar 包。Web服务器（Tomcat）也会连同一块打入jar包中。只要电脑上安装了Java的运行环境（JDK），就可以启动Spring Boot项目。**

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726199029529-6ffc59e1-3d92-4d4e-ab44-31f24cba9585.png)



根据官方文档指导，使用打包功能需要引入以下的插件：

```xml
<build>
	<plugins>
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
		</plugin>
	</plugins>
</build>
```



执行打包命令，生成可执行jar包：  

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726199412578-3892d163-9341-401d-83b5-a4412a1f7bdf.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726199440187-2e04d480-92c7-44e2-afb4-17c495fdc415.png)

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726199497215-d3080065-f6dc-4c1d-b37c-4af2393faf58.png)



单独的将这个 jar 包可以拷贝到任何位置运行，通过`java -jar sb3-01-first-web-1.0-SNAPSHOT.jar`命令来启动 Spring Boot 项目：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726199690515-f22a7212-d34a-4644-a6cd-141a58ed490c.png)



打开浏览器访问：  
![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1726199728479-c1365c32-65d1-428f-b4fa-3d10f20df2e0.png)







另外，Spring Boot框架为我们提供了非常灵活的配置，在可执行jar包的同级目录下新建配置文件：application.properties，并配置以下信息：

```properties
server.port=8888
```

重新启动服务器，然后使用新的端口号访问：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1727503630959-3bcc152d-e57d-4723-8630-bc576582e215.png)

## SpringBoot的jar包和普通jar包的区别
**Spring Boot 打包成的 JAR 文件与传统的 Java 应用程序中的 JAR 文件相比确实有一些显著的区别，主要体现在`依赖管理`和`可执行性`上**。

**依赖管理**：
+ **Spring Boot 的 JAR 包通常包含了应用程序运行所需的所有依赖项**，也就是说它是一个“fat jar”（胖 JAR 包），**这种打包方式使得应用可以独立运行**，而不需要外部的类路径或应用服务器上的其他依赖。
+ 普通的 JAR 文件一般只包含一个类库的功能，并且需要依赖于特定的类路径来找到其他的类库或者框架，这些依赖项通常在部署环境中已经存在，比如在一个应用服务器中。

**可执行性**：
+ **Spring Boot 的 JAR 文件可以通过直接执行这个 JAR 文件来启动应用程序，也就是说它是一个可执行的 JAR 文件**。通过 `java -jar your-application.jar` 命令就可以直接运行应用程序。
+ 而普通的 JAR 文件通常是不可直接执行的，需要通过指定主类（main class）的方式或者其他方式来启动一个应用程序，例如使用 `-cp` 或 `-classpath` 加上类路径以及主类名来执行。

Spring Boot 的这些特性使得部署和运行变得更加简单和方便，特别是在微服务架构中，每个服务都可以被打包成独立的 JAR 文件并部署到任何支持 Java 的地方。



SpringBoot的可执行jar包目录结构：  

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729577060207-7c7bbf86-12ee-4ea4-9f0a-fb2d44d5774c.png)

普通jar包的目录结构：  

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729576629470-76daa653-7d27-4e33-a1c1-05191c529e6a.png)




