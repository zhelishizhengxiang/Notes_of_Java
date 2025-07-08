![](assets/03Connection类/file-20250627100230378.png)
![](assets/03Connection类/file-20250627100354940.png)
* 因为存储过程不常用，所以后面只会重点讲解前两个类


![](assets/03Connection类/file-20250627100535117.png)
* **JDBC中的事务管理是通过`Connection`对象来进行管理的**
* `setAutoCommit(false)`就代表是手动提交事务，那么使用这句代码的时候也代表手动开启事务；`connection.setAutoCommit(true);`代表自动提交事务

使用实例

![](assets/03Connection类/file-20250627103018547.png)