* tomcat分为两大组件：连接器和容器部分。连接器connector用于用于对外交流对外沟通，哦让其部分负责实现servlet规范运行servlet组件
* 其中连接器部分就用到了线程池。图中的Executor就是线程池

![](assets/08tomcat线程池（了解）/file-20250925130239306.png)

![](assets/08tomcat线程池（了解）/file-20250925130435319.png)
![](assets/08tomcat线程池（了解）/file-20250925130905257.png)
![](assets/08tomcat线程池（了解）/file-20250925130925496.png)



###### tomcat线程池配置

![](assets/08tomcat线程池（了解）/file-20250925131236474.png)
![](assets/08tomcat线程池（了解）/file-20250925131326422.png)
* 上述配置对应于tomcat中server.xml中的connector和executor的标签  
	![](assets/08tomcat线程池（了解）/file-20250925131223675.png)
* tomcat对任务队列也做了一定的升级，命名为taskQueue，具体流程如下  
	![](assets/08tomcat线程池（了解）/file-20250925131615856.png)


