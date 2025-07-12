### 1.简介和基本使用

![](assets/03Web服务器-Tomcat/file-20250707191255673.png)
![](assets/03Web服务器-Tomcat/file-20250707191512641.png)
![](assets/03Web服务器-Tomcat/file-20250707191655360.png)


#### 基本使用
![](assets/03Web服务器-Tomcat/file-20250707193404679.png)
* bin目录下的.bat文件是windows下的可执行而文件，.sh是linux下的可执行文件
* webapps目录下放的就是tomcat服务器下的一些项目，把web项目扔到该目录下就完成了项目的部署
* 启动tomcat只需要启动startup.dat即可

#### 配置Tomcat

![](assets/03Web服务器-Tomcat/file-20250708000402034.png)
![](assets/03Web服务器-Tomcat/file-20250708000632861.png)

#### idea中的web项目
![](assets/03Web服务器-Tomcat/file-20250708001210291.png)
* web项目的pom.xml中打包方式是war而不是jar

![](assets/03Web服务器-Tomcat/file-20250708001949899.png)

#### idea中使用Tomcat

有两种方式，其中第二种更为方便。这两种方式就可以无需手动复制 WAR 文件到 `webapps` 目录后在启动Tomcat

![](assets/03Web服务器-Tomcat/file-20250708003241176.png)

![](assets/03Web服务器-Tomcat/file-20250708005328423.png)
* 该种方式配置完以后，相当于当前项目以及有了一个内置的Tomcat
* 该插件有一个小问题：当前插件只支持到tomcat7的版本，并不支持更高版本。但是在项目的开发测试阶段版本7已经够用。

