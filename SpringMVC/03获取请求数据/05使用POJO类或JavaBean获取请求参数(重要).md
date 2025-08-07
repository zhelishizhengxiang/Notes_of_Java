
* 以上方式大家可以看到，当提交的数据非常多时，方法的形参个数会非常多，这不是很好的设计。
* **在SpringMVC中也可以使用POJO类/JavaBean来接收请求参数**。不过有一个非常重要的要求：**`POJO类的属性名`必须和`请求参数的参数名`保持一致(前提是遵循JavaBean命名规范)。
* **请求参数是否可以赋值到JavaBean对应的属性上，不是取决于属性名，而是setter方法名**。
* **底层实现原理使用反射机制，创建User对象，并通过User对象的setter给属性赋值**。具体如下   
	![](assets/05使用POJO类或JavaBean获取请求参数(重要)/file-20250804233509023.png)
```java
package com.powernode.springmvc.pojo;

import java.util.Arrays;

/**
 * ClassName: User
 * Description:
 * Datetime: 2024/3/15 10:51
 * Author: 老杜@动力节点
 * Version: 1.0
 */
public class User {
    private Long id;
    private String username;
    private String password;
    private String sex;
    private String[] hobby;
    private String intro;

    public User() {
    }

    public User(Long id, String username, String password, String sex, String[] hobby, String intro) {
        this.id = id;
        this.username = username;
        this.password = password;
        this.sex = sex;
        this.hobby = hobby;
        this.intro = intro;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public String[] getHobby() {
        return hobby;
    }

    public void setHobby(String[] hobby) {
        this.hobby = hobby;
    }

    public String getIntro() {
        return intro;
    }

    public void setIntro(String intro) {
        this.intro = intro;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", sex='" + sex + '\'' +
                ", hobby=" + Arrays.toString(hobby) +
                ", intro='" + intro + '\'' +
                '}';
    }
}

```

在控制器方法的形参位置上使用javabean来接收请求参数：
```java
@PostMapping("/register")
public String register(User user){
    System.out.println(user);
    return "success";
}
```


执行结果：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710471338770-502eefb8-15b7-4632-9d07-37b1fa60a539.png#averageHue=%23fbfafa&clientId=u9c5ec896-edd3-4&from=paste&height=353&id=u67c745a5&originHeight=353&originWidth=554&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10870&status=done&style=shadow&taskId=uc3623ddc-d1c6-4630-903b-7c9ea45c67c&title=&width=554)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710471328104-0ef741e8-70d3-4294-a559-165ffee8a821.png#averageHue=%23f8f7f7&clientId=u9c5ec896-edd3-4&from=paste&height=177&id=uaf53d3c9&originHeight=177&originWidth=474&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9721&status=done&style=shadow&taskId=udaf52420-4bdc-4236-a0ed-cb12b2edfe5&title=&width=474)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710471357753-31d5fe77-a5bf-470c-a5d0-ce777859188b.png#averageHue=%23f7eae7&clientId=u9c5ec896-edd3-4&from=paste&height=107&id=u9b0fe2d3&originHeight=107&originWidth=1097&originalType=binary&ratio=1&rotation=0&showTitle=false&size=27074&status=done&style=shadow&taskId=u741d104a-6492-4b30-b596-e60b081fae3&title=&width=1097)




我们来测试一下：当JavaBean的属性名和请求参数的参数名不一致时，会出现什么问题？（注意：**getter和setter的方法名不修改，只修改属性名**）
```java
package com.powernode.springmvc.pojo;

import java.util.Arrays;

/**
 * ClassName: User
 * Description:
 * Datetime: 2024/3/15 10:51
 * Author: 老杜@动力节点
 * Version: 1.0
 */
public class User {
    private Long id;
    private String uname;
    private String upwd;
    private String usex;
    private String[] uhobby;
    private String uintro;

    public User() {
    }

    public User(Long id, String username, String password, String sex, String[] hobby, String intro) {
        this.id = id;
        this.uname = username;
        this.upwd = password;
        this.usex = sex;
        this.uhobby = hobby;
        this.uintro = intro;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getUsername() {
        return uname;
    }

    public void setUsername(String username) {
        this.uname = username;
    }

    public String getPassword() {
        return upwd;
    }

    public void setPassword(String password) {
        this.upwd = password;
    }

    public String getSex() {
        return usex;
    }

    public void setSex(String sex) {
        this.usex = sex;
    }

    public String[] getHobby() {
        return uhobby;
    }

    public void setHobby(String[] hobby) {
        this.uhobby = hobby;
    }

    public String getIntro() {
        return uintro;
    }

    public void setIntro(String intro) {
        this.uintro = intro;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + uname + '\'' +
                ", password='" + upwd + '\'' +
                ", sex='" + usex + '\'' +
                ", hobby=" + Arrays.toString(uhobby) +
                ", intro='" + uintro + '\'' +
                '}';
    }
}

```

测试结果：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710471749061-322c5c24-45e5-40b4-95f0-3732508150b7.png#averageHue=%23f8f8f8&clientId=u9c5ec896-edd3-4&from=paste&height=363&id=ud9e62fc3&originHeight=363&originWidth=530&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11045&status=done&style=shadow&taskId=u9744a328-d8bb-4f6f-bfdc-89c41559642&title=&width=530)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710471758221-ea101ba1-8586-472c-9adc-e44729d1bac4.png#averageHue=%23f7f6f5&clientId=u9c5ec896-edd3-4&from=paste&height=154&id=u4074f891&originHeight=154&originWidth=478&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9571&status=done&style=shadow&taskId=uc18498a4-e874-467d-bb78-f69754d1f59&title=&width=478)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710471772183-af36c134-1f73-4cb4-afc6-4a6a827aacbd.png#averageHue=%23f7e5e2&clientId=u9c5ec896-edd3-4&from=paste&height=74&id=u0d56b9d1&originHeight=74&originWidth=1139&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18899&status=done&style=shadow&taskId=u25c77c23-8970-479e-ad63-ef845d09e00&title=&width=1139)
通过测试，我们得知：`请求参数名`可以和`JavaBean的属性名`不一致。

我们继续将其中一个属性的setter和getter方法名修改一下：
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710471908862-89d1b430-cff1-43e2-9678-49017f49d663.png#averageHue=%23fdf9f9&clientId=u9c5ec896-edd3-4&from=paste&height=288&id=u86492b81&originHeight=288&originWidth=455&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22276&status=done&style=shadow&taskId=u326d4035-e801-4f1b-82a0-011bb90fee2&title=&width=455)



再次测试：  
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710471941379-7da74eee-7b34-4dae-8589-98c0cf0a4d04.png#averageHue=%23f9f8f8&clientId=u9c5ec896-edd3-4&from=paste&height=364&id=u284e64cc&originHeight=364&originWidth=575&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11192&status=done&style=shadow&taskId=uf1d524de-4be1-4784-809e-ba148b477ae&title=&width=575)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710471949916-1049b6e4-df85-44f0-ba78-d2fe556f4fb9.png#averageHue=%23f6f5f4&clientId=u9c5ec896-edd3-4&from=paste&height=152&id=u7ee086b3&originHeight=152&originWidth=448&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9510&status=done&style=shadow&taskId=u0af2e626-0dd0-4f28-a0e4-4044fd0ddc1&title=&width=448)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/21376908/1710471961917-33f50796-7f73-4d40-a0ef-2befe83d5ebf.png#averageHue=%23f9f4f2&clientId=u9c5ec896-edd3-4&from=paste&height=85&id=u358b10f2&originHeight=85&originWidth=1179&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18263&status=done&style=shadow&taskId=u2c283f3a-2641-4ce3-9e71-3e36969abc4&title=&width=1179)



