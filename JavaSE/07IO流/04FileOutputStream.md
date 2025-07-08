## 1）FileOutputStream

![](assets/04FileOutputStream/file-20250325132242756.png)
![](assets/04FileOutputStream/file-20250325132305535.png)
* FileOutputStream用于项文件中写入数据
* **无论是使用哪个构造器，如果该流在打开文件进行输出时，目标文件不存在，那么该流会创建该文件**


OutputStream子类继承图如下图所示

![](assets/04FileOutputStream/file-20250325132429444.png)


使用实例1：

![](assets/04FileOutputStream/file-20250325132715249.png)
![](assets/04FileOutputStream/file-20250325133102164.png)


使用实例2：

![](assets/04FileOutputStream/file-20250325133521886.png)
* 构造器中第二个属性是`boolean append`，代表是否是追加在文件的末尾



字节输入流输出流的综合应用如下图所示：

![](assets/04FileOutputStream/file-20250325133910489.png)
![](assets/04FileOutputStream/file-20250325134600136.png)

![](assets/04FileOutputStream/file-20250325134401132.png)![](assets/04FileOutputStream/file-20250325134410163.png)
![](assets/04FileOutputStream/file-20250325135324538.png)