![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=qULOx&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
Linux系统中超级用户是root，通过超级用户root可以创建其它的普通用户，Linux是一个支持多用户的操作系统。在实际使用中，一般会分配给开发人员专属的账户，这个账户只拥有部分权限，如果权限太高，操作的范围过大，一些误操作可能导致系统崩溃，或者数据不安全，所以多用户机制就是一种系统安全策略。
在Linux系统中任何一个用户都对应：一个用户名 + 一个口令。用户使用系统时需要输入用户名和口令进行登录，登录成功后就可以进入自己的主目录（主目录就是自己的工作目录）。
用户账号管理主要包括以下三方面：

- 用户组的管理
- 用户的管理
- 为用户主目录之外的目录授权

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=WVdWU&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# 用户组的管理
每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理。
用户组的管理涉及用户组的添加、修改和删除。
用户组的添加、修改和删除实际上就是对/etc/group文件的更新。
**使用root账户查看当前系统的用户组有哪些**
```shell
cat /etc/group
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1667445334980-7b5bdbd2-ce64-4563-9bfe-db500e1992d5.png#averageHue=%23100e0c&clientId=u808dc35b-33c7-4&from=paste&height=245&id=u2a9ea942&originHeight=245&originWidth=352&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13629&status=done&style=shadow&taskId=ua45b20fa-3969-40da-9e6b-645f27e0e3f&title=&width=352)
每一个用户组四部分组成：组名：密码标识：GID：该用户组中的用户列表
**查看当前登录的账户属于哪一组**
```shell
groups
```
**查看某个用户属于哪一组**
```shell
[root@localhost ~]# groups root
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1667462762537-adc5856a-8887-4e08-821c-a58d739c202a.png#averageHue=%230e0c0a&clientId=u92cac9e8-c7e5-4&from=paste&height=54&id=ua1861d3d&originHeight=54&originWidth=474&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2691&status=done&style=shadow&taskId=ua0794390-9cff-4b99-b473-13d0809b6ff&title=&width=474)

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=RgpUO&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 用户组的添加
语法：groupadd [选项] 组名
常用选项包括：

- -g     可以通过这个选项来指定新用户组的标识号（GID）
```shell
groupadd dev1
```
```shell
groupadd -g 101 dev2
```
其中101是dev2这个组的组号。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=NhfPa&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 用户组的修改
```shell
groupmod -g 102 dev2
```
```shell
# 将dev2修改为dev3
groupmod -n dev3 dev2
```
## 用户组的删除
```shell
# 删除用户组dev3
groupdel dev3
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=W33Rh&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# 用户的管理
用户管理工作主要涉及到用户的添加、修改和删除。
## 添加用户
添加用户就是在系统中创建一个新账号，然后为新账号分配用户组、主目录等资源。
语法：useradd [选项] 用户名
选项：

- -d    指定新用户的主目录
- -g    指定新用户属于哪个组（主组）
- -G    可以给新用户添加附加组
```shell
[root@localhost ~]# useradd lisi
```
注意：当新建用户时，没有指定组，也没有指定工作目录时：

- 默认的组名：和自己用户名一样
- 默认的主目录：/home/用户名
```shell
[root@localhost ~]# useradd -d /usr/zhangsan zhangsan
```
```shell
[root@localhost usr]# useradd -d /usr/lisi -g dev -G test lisi
```
添加lisi用户，该用户的主目录/usr/lisi，所属主组dev（开发组），附加组test（测试组）

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=N0LSE&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 设置密码
```shell
passwd lisi
```
注意：增加用户就是在/etc/passwd文件中为新用户增加一条记录，同时更新其他系统文件如/etc/shadow, /etc/group等。
通过查看/etc/passwd文件可以看到系统中有哪些用户，例如执行：cat /etc/passwd
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1670373917902-1acf7269-c7cc-4345-a9fe-8ac9387491c6.png#averageHue=%230e0c0a&clientId=u99ee4637-4121-4&from=paste&height=129&id=u41030c7c&originHeight=129&originWidth=1010&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12985&status=done&style=shadow&taskId=u22c8f637-77ce-42e6-9d5b-819f6d3bcf0&title=&width=1010)
以上信息描述了什么？
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1670374127042-35e70eef-1d23-4953-bc9d-eb8d1f61f5de.png#averageHue=%239b9896&clientId=u99ee4637-4121-4&from=paste&height=311&id=uf0e20ff8&originHeight=311&originWidth=1008&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22309&status=done&style=shadow&taskId=ue5e3dd1c-bbff-45e9-8583-b592f176500&title=&width=1008)
密码会单独存储在/etc/shadow文件中，例如执行：cat /etc/shadow
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1670374502252-611d400f-1975-45cb-99db-302b3a8792fa.png#averageHue=%230c0a09&clientId=u99ee4637-4121-4&from=paste&height=152&id=u031316c4&originHeight=152&originWidth=930&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13877&status=done&style=shadow&taskId=u749f5f64-cf25-46c7-ade8-030cf854480&title=&width=930)
可以看到这个密码是通过某种算法进行加密的。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=K78Db&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 切换用户
```shell
[root@localhost ~]# su bjpowernode
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1667445554507-4d62ec46-a55a-4d87-b584-4e91f8869f95.png#averageHue=%232f2d2b&clientId=u808dc35b-33c7-4&from=paste&height=121&id=PLUfQ&originHeight=121&originWidth=489&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6621&status=done&style=shadow&taskId=u2815386b-d631-40c9-bdcf-ea4832e135b&title=&width=489)
```shell
[bjpowernode@localhost root]$ su root
密码： 
[root@localhost ~]# 
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21376908/1667445665542-e7319c1d-3161-458b-8a49-6d55f053e77b.png#averageHue=%230c0a09&clientId=u808dc35b-33c7-4&from=paste&height=100&id=B4ofm&originHeight=100&originWidth=515&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5691&status=done&style=shadow&taskId=u0bd904df-8dc1-4983-af9d-0643f5f3704&title=&width=515)
注意：从普通用户切换到高级用户需要密码。密码输入时不回显。
注意：切换到普通用户之后，该普通用户默认只对自己的“主目录”有权限，主目录之外的目录是没有权限的。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=jfxo8&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 修改用户
修改用户就是对用户名，用户主目录，用户组等进行修改。
语法：usermod [选项] 用户名

- -d    指定新用户的主目录
- -g    指定新用户属于哪个组（主组）
- -G    可以给新用户添加附加组
- -l     指定新的用户名（小写的艾路）
```shell
usermod -l zhangsi zhangsan
```
```shell
# -m 选项很重要，当有了这个选项之后，目录不存在时会新建该目录。
usermod -d /usr/zhangsan2 -m zhangsan
```
```shell
usermod -g dev1 zhangsan
```
```shell
usermod -L zhangsan
```
```shell
usermod -U zhangsan
```

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=g3boS&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
## 删除用户
```shell
userdel -r zhangsan
```
-r 选项的作用是连同该用户主目录一块删除。

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=AAUqb&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
# 为用户主目录之外的目录授权
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

![标头.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&clientId=uc5a67c34-8a0d-4&from=paste&height=78&id=sdliO&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23158&status=done&style=shadow&taskId=u98709943-fd0b-4e51-821c-a3fc0aef219&title=&width=1400)
