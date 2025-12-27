
库存秒杀线程解决方案，Linux是一个支持多用户的操作系统。在实际使用中，一般会分配给开发人员专属的账户，这个账户只拥有部分权限，如果权限太高，操作的范围过大，一些误操作可能导致系统崩溃，或者数据不安全，**所以多用户机制就是一种系统安全策略。**
在Linux系统中任何一个用户都对应：一个用户名 + 一个口令。用户使用系统时需要输入用户名和口令进行登录，登录成功后就可以进入自己的主目录（主目录就是自己的工作目录）。
**用户账号管理主要包括以下三方面**：

- **用户组的管理**
- **用户的管理**
- **为用户主目录之外的目录授权**


# 一、用户组的管理
**每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理。**    
用户组的管理涉及用户组的添加、修改和删除。    
**用户组的添加、修改和删除实际上就是对/etc/group文件的更新。**  
**使用root账户查看当前系统的用户组有哪些**
```shell
cat /etc/group
```
![](assets/第5章%20Linux用户管理/file-20251127183237715.png)
![](assets/第5章%20Linux用户管理/file-20251127183343068.png)
* 每一个用户组四部分组成：组名：密码标识：GID：该用户组中的用户列表

**查看当前登录的账户属于哪一组**
```shell
groups
```
**查看某个用户属于哪一组**
```shell
[root@localhost ~]# groups root
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1667462762537-adc5856a-8887-4e08-821c-a58d739c202a.png#averageHue=%230e0c0a&clientId=u92cac9e8-c7e5-4&from=paste&height=54&id=ua1861d3d&originHeight=54&originWidth=474&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2691&status=done&style=shadow&taskId=ua0794390-9cff-4b99-b473-13d0809b6ff&title=&width=474)


## 1.用户组的添加
**语法：groupadd \[选项] 组名**
常用选项包括：

- -g     可以通过这个选项来指定新用户组的标识号（GID）
```shell
groupadd dev1
```
```shell
groupadd -g 101 dev2
```
其中101是dev2这个组的组号。


## 2.用户组的修改
```shell
groupmod -g 102 dev2
```
```shell
# 将dev2修改为dev3
groupmod -n dev3 dev2
```
## 3.用户组的删除
```shell
# 删除用户组dev3
groupdel dev3
```


# 二、用户的管理
用户管理工作主要涉及到用户的添加、修改和删除。
### 1.新增用户——useradd
![](assets/第3章%20系统命令/file-20251123230215268.png)
![](assets/第3章%20系统命令/file-20251123230303952.png)
![](assets/第3章%20系统命令/file-20251123230936873.png)
### 2.切换用户命令——su
**su 用户名**
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1713790037663-4f74afad-9ccf-4b75-bc5a-f52eba6e64de.png#averageHue=%230c0a09&clientId=u5a9fa857-9742-4&from=paste&height=86&id=u14f45aa5&originHeight=86&originWidth=509&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5423&status=done&style=none&taskId=u724a6cf6-49ca-4ea3-b4c5-0b4de904ba1&title=&width=509)

**sudo 命令：表示使用超级管理员身份执行该命令，如果你当前不是管理员，希望以管理员身份执行某个命令时，使用sudo，需要输入超级管理员的密码**：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1713862409692-abeda774-bb80-41bc-bdf2-925c2a4c2a3f.png#averageHue=%230d0b0a&clientId=u60d3495d-8551-4&from=paste&height=190&id=u1e15b695&originHeight=190&originWidth=655&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14216&status=done&style=none&taskId=uaa8ecb98-c405-4726-a46a-359d2e4a52d&title=&width=655)

赋予用户可以sudo的权限：   
![](assets/第3章%20系统命令/file-20251123231804201.png)
![](assets/第3章%20系统命令/file-20251123232111220.png)
### 3.删除用户——userdel
![](assets/第3章%20系统命令/file-20251123231335679.png)


# 三、为用户主目录之外的目录授权
建议看该内容之前先看文件权限的命令[第6章 文件权限](第6章%20文件权限.md)


第一步：创建目录
```shell
mkdir /java
```
第二步：给目录授权
```shell
# -R表示递归设置权限，该目录下所有的子目录以及子文件
chmod -R 775 /java
```
第三步：创建组
```shell
groupadd dev
```
第四步：把目录赋予组
```shell
chgrp -R dev /java
```
第五步：创建用户
```shell
useradd xiaoming
```
第六步：设置密码
```shell
passwd xiaoming
```
第七步：给用户添加附加组
```shell
usermod -G dev xiaoming
```


