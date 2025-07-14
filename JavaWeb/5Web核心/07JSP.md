1现在的主流开发方式是servlet+html+ajax。servlet+jsp已经过时，下面的内容了解即可
### 一、JSP概述

![](assets/07JSP/file-20250712162116953.png)
* 比servlet更加方便。如果需要一个比较丰富的代码，那么使用servlet的代码如下。需要满屏的写入和拼接字符串

	![](assets/07JSP/file-20250712161950290.png)

* 而使用jsp后，就可以直接将java代码和html代码携带一块

	![](assets/07JSP/file-20250712162045015.png)

### 二、JSP快速入门
![](assets/07JSP/file-20250712162851042.png)
* 和servlet同理，导入的坐标的scope为provided，因为运行时tomcat已经自带了该jar包


![](assets/07JSP/file-20250712164312477.png)![](assets/07JSP/file-20250712164335215.png)


### 三、JSP原理
![](assets/07JSP/file-20250712170539282.png)
* 位于服务器端的JSP文件，在被访问时，tomcat会自动将其转换为一个servlet，之后再编译为一个字节码文件。所以**JSP在本质上就是一个Servlet，其转换是tomcat自动完成**。因为他是网页，所以可以写html，本质上又是一个servlet，所以可以写java代码

![](assets/07JSP/file-20250712170406758.png)
![](assets/07JSP/file-20250712170706137.png)
![](assets/07JSP/file-20250712170849451.png)
* jsp转换的servlet的java文件中也有service对应的service方法，每次一访问该jsp，就会访问该方法


### 四、JSP脚本
![](assets/07JSP/file-20250712171843031.png)
* 第一种脚本作为service方法中的方法体。
* 第二种脚本作为resp的输出流的参数.
* 第三种作为jsp转为servlet的整个类的成员


第一种脚本  
![](assets/07JSP/file-20250712171102950.png)![](assets/07JSP/file-20250712171121480.png)

第二种脚本  
![](assets/07JSP/file-20250712171439404.png)
![](assets/07JSP/file-20250712171510484.png)
![](assets/07JSP/file-20250712171450943.png)

第三种脚本  
![](assets/07JSP/file-20250712171815060.png)![](assets/07JSP/file-20250712171827884.png)


###### 应用实例

![](assets/07JSP/file-20250712175114641.png)
![](assets/07JSP/file-20250712175135628.png)
![](assets/07JSP/file-20250712175035571.png)


结果  
![](assets/07JSP/file-20250712175151235.png)

截断的字节码文件长这样（下图代码是在service()里），所以它可以完成for循环遍历的作用

![](assets/07JSP/file-20250712175256018.png)
* 很容易可以感觉到这么写代码可读性和可维护性特别差，所以下面来讲缺点。

上面jsp脚本的缺点

![](assets/07JSP/file-20250713175255457.png)
* Servlet来处理数据，通过**请求转发的方式**交给JSP来进行数据的展示

###  五、EL表达式
![](assets/07JSP/file-20250713181023281.png)
* EL用于适用于获取数据的，他的主要目的是简化JSP中的Java代码，即写到响应体中的java代码

servlet代码

![](assets/07JSP/file-20250713180328656.png)

jsp代码

![](assets/07JSP/file-20250713180830906.png)

演示结果

![](assets/07JSP/file-20250713180758741.png)


### 六、JSTL标签
![](assets/07JSP/file-20250713181605235.png)

JSTL的使用：

![](assets/07JSP/file-20250713181645746.png)

主要介绍两个jstl标签：if和for-each。这两个标签就是用于替换if else和for循环的。使用示例如下图所示

###### if标签

![](assets/07JSP/file-20250713184046076.png)
![](assets/07JSP/file-20250713184104182.png)
![](assets/07JSP/file-20250713184128315.png)
* 本人设置的prefix是c，所以标签就为c:if。
* test属性中写逻辑表达式，if标签中是要输出的内容。
* c:if表中中没有else标签，实现else的逻辑就是写两个c:if，让其中的逻辑判断相反即可

###### forEach标签

![](assets/07JSP/file-20250713185755611.png)
* varStatus是用于展示数据的序号值连续的，因为数据库可能会删除某些数据，由于主键自增导致序号不连续，未来展现出连续的数据就用status。其有两个属性：index从0开始，count从1开始

	![](assets/07JSP/file-20250713190011187.png)
* fori循环和for-each循环时一样的。可以做分页控制条

使用示例：

![](assets/07JSP/file-20250713184046076.png)
![](assets/07JSP/file-20250713185517428.png)
![](assets/07JSP/file-20250713190230627.png)
* 注意brand中的属性底层会调用pojo类的get方法，所以变量名一定要遵守驼峰命名，不然没法匹配上对应的方法。

