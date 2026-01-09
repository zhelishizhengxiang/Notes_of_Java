# 一、概述
MP不仅提供通用的Mapper，还提供通用的service接口。

* 提供的service接口就是**IService**。该接口进一步封装 CRUD 采用 **get** 查询单行 **remove** 删除 **list** 查询集合 **page** 分页 **save插入**，前缀命名方式区分 Mapper 层避免混淆。**泛型 T 为操作的实体类对象**    
	![[file-20260109205529791.png]]
* 建议如果存在自定义通用 Service 方法的可能，请创建自己的 IBaseService 继承
* **该接口还有一个实现类`ServiceImpl<M extends BaseMapper<T>, T>`，封装了常见的业务层逻辑。其泛型一共有两个。第一个为我们所写的Mapper，第二个为我们所要操作的实体类对象**

# 二、使用方法
 
 实际场景中较为复杂，ServiceImpl无法满足需求，所以不建议直接使用ServiceImpl。
 
 **建议独自创建Service接口来继承Iservice，独自创建实现类来继承ServiceImpl再继承自己写的Service**。这与既可以使用Iservice提供的功能，也可以使用自定义的功能。

```java
// 接口  UserService继承IService模板提供的基础功能
public interface UserService extends IService<User> {

}
// 实现类
/*
   ServiceImpl实现了IService，提供了IService中基础功能的实现
   若ServiceImpl无法满足业务需求，则可以使用自定的UserService定义方法，
   并在实现类中实现
*/
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {

}
```

# 三、使用举例

1. 测试记录总数
![[file-20260109211214277.png]]
![[file-20260109211158611.png]]


2. 测试批量添加功能   
![[file-20260109211443296.png]]
![[file-20260109211524400.png]]
