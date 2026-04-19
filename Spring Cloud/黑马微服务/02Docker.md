Docker是什么，有什么用？  
![[file-20260312211946836.png]]
![[file-20260312211910007.png]]


# 一、Docker安装

我们统一在CentOS的虚拟机中安装Docker，统一学习环境。具体安装直接参考⁠[‌‌​⁠​​﻿​​‌​​‌​​⁠​​​‌‌‬​​‌​‍​​​⁠‬​‌​‬﻿⁠​‬​‍‍​​​‬‍​‍day02-Docker - 飞书云文档](https://my.feishu.cn/wiki/MWQIw4Zvhil0I5ktPHwcoqZdnec)​‍​‬﻿​⁠​‌‌⁠‍﻿​​​‌﻿‌‌﻿​⁠‌﻿​​‬‍​​‌​​⁠​​‍⁠﻿
# 二、Docker核心概念

1. **镜像**：当我们利用Docker安装应用时，Docker会自动搜索并下载应用**镜像（image）。镜像不仅包含应用本身，还包含应用运行所需要的环境、配置、系统函数库**。这样就可以在任何服务器版本上运行该应用。
2. **容器：** **Docker会在运行镜像时创建的隔离环境。该隔离环境有自己独立的内存空间，文件系统、网络空间(有自己IP地址，但外界不可访问)等。**。是镜像的运行态，与其他进程相互隔离。
3. **镜像仓库**：**存储和管理镜像的平台**，Docker官方维护了一个公共仓库：Docker Hub，但是访问较慢。国内也有一些镜像仓库，如阿里云，腾讯云，华为云等等。

# 三、Docker常见命令

**所有的docker命令都是以docker为前缀**  
![[file-20260313112407311.png]]
* docker pull：将远程镜像拉取本地，即下载镜像到本地
* docker images：查看所有的本地镜像
* docker rmi(remove image)：删除本地镜像
* docker build：基于dockerfile自己构建镜像。（后面会细讲）
* docker save：将镜像保存到本地，导出为一个压缩文件（用的较少）
* docker load：将压缩文件加载为本地镜像。（用的较少）
* docker push：将本地镜像推到镜像仓库
* docker run：**创建并运行容器（每次执行会创建一个新的容器）**。如果本地没有该镜像，会先自动进行拉取，拉取了再去自动创建容器。
* **docker stop：停止容器（内的进程，）但容器还是在的。**
* **docker start：启动容器（即启动容器内的进程）**
* **docker ps(proccess status)：查看容器的运行状态**
* **docker rm：删除容器**
* docker logs：查看容器运行的日志
* **docker exec：在容器内部执行一些命令**。使用exit命令退出容器內部
###### docker run
以下图命令来进行讲解（\代表换行）
![[file-20260313105520670.png]]
* **docker run ：创建并运行一个容器，-d 是让容器在后台运行。** 若不加-d，代表从前台执行，系统会一直等在那里，等待输入。
* **--name mysql ：给容器起个名字，必须唯一**
* **-p (宿主机)3306:(docker容器)3306 ：设置端口映射，将宿主机的端口与宿主机的端口做一个映射**。这样外界访问：`ip:7890`，实际会转发到容器内的 `3306`端口，连上 MySQL。
* **-e KEY=VALUE ：是设置环境变量。** 比如mysql的镜像就需要图中两个环境变量一个是root账户的密码，一个是时区。这些是由镜像制作者决定，使用时查镜像文档即可。
* **mysql ：指定运行的镜像的名称。** 
	1. **镜像名称一般分两部分组成：\[repository]:\[tag]**。其中repository就是镜像名tag是镜像的版本
	2. **在没有指定tag时，默认是latest，代表最新版本的镜像**


###### 使用案例

剩下的命令使用一个案例来进行讲解    
![[file-20260313112609522.png]]
* 在DockerHub中搜索Nginx镜像，查看镜像的名称：docker pull nginx
* 查看本地所有镜像：docker images
* 创建并运行Nginx容器：docker run -d --name nginx -p 80:80 nginx
*  查看容器运行状态： docker ps
* 停止容器：docker stop nginx
* 再次启动容器：docker start nginx
* 查看容器日志：docker logs nginx
* 进入nginx容器：docker exec -it(可交互终端) nginx bash（用bash命令行进行交互）。可以修改配置文件等等。或者直接docker exec -it mysql mysql -uroot -p代表直接进入容i去并执行吗命令
* 删除容器：docker rm nginx
* 查看容器详细信息：docker inspect


注：
1. 如果拉取报超时就代表镜像没有配置成功，此时只需要去网上直接招最新的docker镜像源重新配置即可
2. 如果有命令不知道怎么用直接使用--help参数，比如docker save --help

# 四、数据卷(volume)

问题：所有的镜像虽然包含一些系统函数库和依赖环境，**但只是包含其运行所必备的系统函数库和依赖环境（最小化环境）**。比如nginx容器的内部就不会包含ll命令、vim命令，因为nginx运行不需要这些系统函数库。所以在容器内修改文件（如将静态资源拷贝到nginx的html目录下）十分困难，这就是问题，所以引出来了数据卷。

### 1.定义和原理
**数据卷：就是一个虚拟目录，他是容器内目录于宿主机目录之间之前映射的桥梁**

图解    
![[file-20260315061628489.png]]
原理及要点： 
1. 数据卷的创建，使用docker命令去创建的，docker会在宿主机去创建对应真实的目录（linux下路径固定为/var/lib/docker/volumes/`创建的目录`）。并且会在该目录下创建一个_data目录。**数据卷和真实目录一一对应**。
2. **只需要让容器目录与数据卷做挂载**（二者产生关联），这样就相当于容器的目录与宿主机下真是目录产生了关联。**即docker会将宿主机目录与容器内目录的双向绑定。** 通俗的讲，只要在_data目录下做一些改动，这个改动在容器内目录也会生效，反之亦然。这样通过使用宿主机的一些命令(系统函数库)来做一些复杂的修改操作

### 2.相关命令
![[file-20260315061733977.png]]
* 与之前同理，不用死记硬背，直接通过docker volume --help即可查看如何使用  
	![[file-20260315061835343.png]]
* **数据卷的挂载：`docker run -v 数据卷名称:容器内目录`** 。**数据卷的挂载动作一定要在docker run的时候去执行，如果容器已经创建则没有办法再做挂载**。
* **当创建容器时，如果挂载了数据卷且数据卷不存在，则会自动创建数据卷**。
### 3.使用实例

需求：
1. 创建Nginx容器，修改nginx容器内的html目录下的index.html文件，查看变化。
2. 将静态资源部署到nginx的html目录

步骤：  
![[file-20260315063220669.png]]
1. 创建容器并挂载：docker run -d --name nginx -p 80:80 -v html:/usr/share/nginx/html nginx
2. 修改_data目录下index.html并访问   
	![[file-20260315063859490.png]]
3. 访问静态资源   
	![[file-20260315064018823.png]]


# 五、本地目录挂载

之前的数据卷挂载，宿主机都是在指定目录的下创建。现在**想要自定义宿主机挂载的目录**。具体如下：   
![[file-20260315065346204.png]]
![[file-20260315065903085.png]]


# 六、DockerFile

### 1.镜像的结构
将来自己开发的java应用如果用doocker去部署的话，也要制作成镜像才行，所以需**要自己制作java应用的镜像**

![[file-20260315070426010.png]]
* **docker会把上述每一步产生的文件分别打成压缩包作为镜像的一部分，最终合在一起才是完整的镜像**

![[file-20260315071542009.png]]
* 如果每次制作镜像都需要自己对每一层进行压缩打包的话，工作量也很大。**而dokcer并不会让我们亲自动手做镜像，我们只需要用固定的语法描述清楚镜像结构，docker会自动帮我们完成整个镜像的构建。这种记录镜像结构的文件就称为Dockerfile**

### 2.Docker的语法
![[file-20260315072430689.png]]
* 更详细语法说明，请参考官网文档： https://docs.docker.com/engine/reference/builder
* **并且对于Dockerfile文本文件的命名，必须叫该名字Dokcerfile，不能是其他名字**

例子：基于ubuntu构建java应用的镜像   
```Dockerfile
# 指定基础镜像
FROM ubuntu:16.04
# 配置环境变量，JDK的安装目录、容器内时区
ENV JAVA_DIR=/usr/local
ENV TZ=Asia/Shanghai
# 拷贝jdk和java项目的包
COPY ./jdk8.tar.gz $JAVA_DIR/
COPY ./docker-demo.jar /app.jar
# 设定时区
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# 安装JDK
RUN cd $JAVA_DIR \
 && tar -xf ./jdk8.tar.gz \
 && mv ./jdk1.8.0_144 ./java8
# 配置环境变量
ENV JAVA_HOME=$JAVA_DIR/java8
ENV PATH=$PATH:$JAVA_HOME/bin
# 指定项目监听的端口
EXPOSE 8080
# 入口，java项目的启动命令
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

思考：以后我们会有很多很多java项目需要打包为镜像，他们都需要Linux系统环境、JDK环境这两层，只有上面的3层不同（因为jar包不同）。如果每次制作java镜像都重复制作前两层镜像，是不是很麻烦


所以，**就有人提供了基础的系统加JDK环境，我们在此基础上制作java镜像**，就可以省去JDK的配置了：
```Dockerfile
# 基础镜像
FROM openjdk:11.0-jre-buster
# 设定时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# 拷贝jar包
COPY docker-demo.jar /app.jar
# 入口
ENTRYPOINT ["java", "-jar", "/app.jar"]
```


### 3.构建镜像

**dockerfile构建镜像命令：docker build -t myImage:1.0  .**。其中-t可以理解为tag的缩写，代表要给镜像起一个名字。后面就是镜像名称和版本号。  
![[file-20260315074006681.png]]

在课前资料中，我们准备好了一个demo项目及对应的Dockerfile：   
![[file-20260323220144227.png]]

  

首先，我们将课前资料提供的`docker-demo.jar`包以及`Dockerfile`拷贝到虚拟机的`/root/demo`目录：   
![[file-20260323220153565.png]]

  

然后，执行命令，构建镜像：
```Bash
# 进入镜像目录
cd /root/demo
# 开始构建
docker build -t docker-demo:1.0 .
```

命令说明：
- `docker build` : 就是构建一个docker镜像
- `-t docker-demo:1.0` ：`-t`参数是指定镜像的名称（`repository`和`tag`）
- `.` : 最后的点是指构建时Dockerfile所在路径，由于我们进入了demo目录，所以指定的是`.`代表当前目录，也可以直接指定Dockerfile目录：
```Bash
    # 直接指定Dockerfile目录
    docker build -t docker-demo:1.0 /root/demo
```
    

结果：     
![[file-20260323220228976.png]]

查看镜像列表：
```Bash
# 查看镜像列表：
docker images
# 结果
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
docker-demo   1.0       d6ab0b9e64b9   27 minutes ago   327MB
nginx         latest    605c77e624dd   16 months ago    141MB
mysql         latest    3218b38490ce   17 months ago    516MB
```

然后尝试运行该镜像：
```Bash
# 1.创建并运行容器
docker run -d --name dd -p 8080:8080 docker-demo:1.0
# 2.查看容器
dps
# 结果
CONTAINER ID   IMAGE             PORTS                                                  STATUS         NAMES
78a000447b49   docker-demo:1.0   0.0.0.0:8080->8080/tcp, :::8090->8090/tcp              Up 2 seconds   dd
f63cfead8502   mysql             0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060/tcp   Up 2 hours     mysql

# 3.访问
curl localhost:8080/hello/count
# 结果：
<h5>欢迎访问黑马商城, 这是您第1次访问<h5>
```
# 七、容器网络互连
![[file-20260315145931191.png]]
* **如图所示的原因，虽然各个容器虽然时独立空间，但是他们通过网桥建立了连接，所以可以相互进行访问**
* **但docker当中的容器的ip地址是顺序分配的**，有可能在一个容器的重启过程中，其他容器启动了并且占用了该ip地址，**会有启动后的ip地址与原IP地址不同的问题**

![[file-20260315160836974.png]]
* 创建自定义网络，它会形成一个新的网桥，其ip地址的网段就与docker的默认网的网段不一致了。
* **加入自定义网络的容器可以通过容器名字进行访问，可以不通过ip地址来进行访问，更加方便。** 容器无论重启多少次，ip地址可能变化，但是容器名不会发生变化，这样就解决上述的问题。
* docker network * 命令代表自定义网络命令  
* **`docker network connect 网络名 容器名` 是将已经存在的容器加入某网络。如果想在创建的时候就把他加入某个网络，可以使用--network参数来进行操作**  
	![[file-20260315232218437.png]]


使用实例：
```Bash
# 1.首先通过命令创建一个网络
docker network create hmall

# 2.然后查看网络
docker network ls
# 结果：
NETWORK ID     NAME      DRIVER    SCOPE
639bc44d0a87   bridge    bridge    local
403f16ec62a2   hmall     bridge    local
0dc0f72a0fbb   host      host      local
cd8d3e8df47b   none      null      local
# 其中，除了hmall以外，其它都是默认的网络

# 3.让dd和mysql都加入该网络，注意，在加入网络时可以通过--alias给容器起别名
# 这样该网络内的其它容器可以用别名互相访问！
# 3.1.mysql容器，指定别名为db，另外每一个容器都有一个别名是容器名
docker network connect hmall mysql --alias db
# 3.2.db容器，也就是我们的java项目
docker network connect hmall dd

# 4.进入dd容器，尝试利用别名访问db
# 4.1.进入容器
docker exec -it dd bash
# 4.2.用db别名访问
ping db
# 结果
PING db (172.18.0.2) 56(84) bytes of data.
64 bytes from mysql.hmall (172.18.0.2): icmp_seq=1 ttl=64 time=0.070 ms
64 bytes from mysql.hmall (172.18.0.2): icmp_seq=2 ttl=64 time=0.056 ms
# 4.3.用容器名访问
ping mysql
# 结果：
PING mysql (172.18.0.2) 56(84) bytes of data.
64 bytes from mysql.hmall (172.18.0.2): icmp_seq=1 ttl=64 time=0.044 ms
64 bytes from mysql.hmall (172.18.0.2): icmp_seq=2 ttl=64 time=0.054 ms
```

# 八、使用docker部署项目到服务器

具体操作参考[‬​‍⁠⁠﻿​​⁠​​​‍​‬﻿‌​​​‍​​​​​‬‍​‬﻿​​‬‌﻿​​​﻿‌⁠​﻿​​‍​⁠day02-Docker - 飞书云文档](https://my.feishu.cn/wiki/MWQIw4Zvhil0I5ktPHwcoqZdnec)


# 九、Docker Compose

刚刚我们部署一个简单的java项目，其中包含3个容器：
- MySQL
- Nginx
- Java项目
    
而稍微复杂的项目，其中还会有各种各样的其它中间件，需要部署的东西远不止3个。如果还像之前那样手动的逐一部署，就太麻烦了。

  而**Docker Compose就可以帮助我们实现多个相互关联的Docker容器的快速部署**。**它允许用户通过一个单独的 docker-compose.yml 模板文件（YAML 格式）来定义一组相关联的应用容器。**

一般一个docker-compose文件对应一个项目，里面的每一个容器被称之为一个服务  
![[file-20260318235920152.png]]
### 1.基本语法


**docker-compose文件的语法其实与yaml文件格式相同，并且对应的语法就是就是将docker run命令的哪些参数写在文件当中。** 所以不必特意去学，到时候现学现查即可。  
![[file-20260319000352302.png]]

具体语法见[Compose Develop Specification | Docker Docs](https://docs.docker.com/reference/compose-file/develop/)

多容器的docker-compose文件如下所示：   
![[file-20260319000635021.png]]![[file-20260319000704119.png]]
* java项目将来需要docker build出本地镜像来才能run，而在这里直接将docker build内容通过build写出来对应的参数，无需先制定镜像。
* 之前的自定义网络是我们手动创建的，但是现在有了docker-compose问文件，只需要通过networks声明出对应得网络，将来会自动创建

### 2.使用docker-compose部署项目

**执行命令**：
```Bash
docker compose [OPTIONS] [COMMAND]
```
![[file-20260319001553393.png]]
* 同样需要加-d参数让其在后台运行，而不是前台