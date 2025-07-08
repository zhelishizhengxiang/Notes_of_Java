![](assets/09ObjectInputStream和ObjectOuyputStream/file-20250329171332674.png)
![](assets/09ObjectInputStream和ObjectOuyputStream/file-20250329171449424.png)
* 推荐使用Serializable，是一个标记接口，里面没有使用方法；而Externalizable接口有方法需要实现，因此我们一般使用Serializable

![](assets/09ObjectInputStream和ObjectOuyputStream/file-20250329171711057.png)
* **ObjectInputStream和ObjectOuyputStream是包装流**
![](assets/09ObjectInputStream和ObjectOuyputStream/file-20250329171802125.png)
![](assets/09ObjectInputStream和ObjectOuyputStream/file-20250329171851171.png)


* ObjectOutputStream使用实例
![](assets/09ObjectInputStream和ObjectOuyputStream/file-20250329172517471.png)
![](assets/09ObjectInputStream和ObjectOuyputStream/file-20250329172944204.png)
* **写进文件的对象的所属于的类必须实现Serializable接口或者Externalizable接口**
* 序列化写入的文件里面的内容直接看是乱码，而不是说文件名是按照它的格式去保存，而是文件内容按照他的格式去保存


* ObjectInputStream反序列化使用实例
 
![](assets/09ObjectInputStream和ObjectOuyputStream/file-20250329173059659.png)
![](assets/09ObjectInputStream和ObjectOuyputStream/file-20250329173124724.png)
![](assets/09ObjectInputStream和ObjectOuyputStream/file-20250329173517068.png)

二者的使用细节：

![](assets/09ObjectInputStream和ObjectOuyputStream/file-20250330132024793.png)
![](assets/09ObjectInputStream和ObjectOuyputStream/file-20250330131419334.png)
![](assets/09ObjectInputStream和ObjectOuyputStream/file-20250330131524272.png)