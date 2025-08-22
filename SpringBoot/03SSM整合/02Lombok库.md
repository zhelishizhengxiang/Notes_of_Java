## 1.Lombok基本介绍
* **Lombok 是一个 Java 库，它可以通过注解的方式减少 Java 代码中的样板代码。Lombok 自动为你生成构造函数、getter、setter、equals、hashCode、toString 方法等**，从而避免了手动编写这些重复性的代码。这不仅减少了出错的机会，还让代码看起来更加简洁。
* **<font style="color:#DF2A3F;">Lombok只是一个编译阶段的库，能够帮我们自动补充代码，在Java程序运行阶段并不起作用。（因此Lombok库并不会影响Java程序的执行效率）</font>**

例如我们有这样一个java源文件`User.java`，代码如下：

```java
@Data
public class User{
    private String name;
}
```

以上代码在程序的编译阶段，Lombok库会将`User.java`文件编译生成这样的`User.class`字节码文件：

```java
public class com.powernode.lomboktest.model.User {
  public com.powernode.lomboktest.model.User();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public java.lang.String getName();
    Code:
       0: aload_0
       1: getfield      #7                  // Field name:Ljava/lang/String;
       4: areturn

  public void setName(java.lang.String);
    Code:
       0: aload_0
       1: aload_1
       2: putfield      #7                  // Field name:Ljava/lang/String;
       5: return

  public boolean equals(java.lang.Object);
    Code:
       0: aload_1
       1: aload_0
       2: if_acmpne     7
       5: iconst_1
       6: ireturn
       7: aload_1
       8: instanceof    #8                  // class com/powernode/lomboktest/model/User
      11: ifne          16
      14: iconst_0
      15: ireturn
      16: aload_1
      17: checkcast     #8                  // class com/powernode/lomboktest/model/User
      20: astore_2
      21: aload_2
      22: aload_0
      23: invokevirtual #13                 // Method canEqual:(Ljava/lang/Object;)Z
      26: ifne          31
      29: iconst_0
      30: ireturn
      31: aload_0
      32: invokevirtual #17                 // Method getName:()Ljava/lang/String;
      35: astore_3
      36: aload_2
      37: invokevirtual #17                 // Method getName:()Ljava/lang/String;
      40: astore        4
      42: aload_3
      43: ifnonnull     54
      46: aload         4
      48: ifnull        65
      51: goto          63
      54: aload_3
      55: aload         4
      57: invokevirtual #21                 // Method java/lang/Object.equals:(Ljava/lang/Object;)Z
      60: ifne          65
      63: iconst_0
      64: ireturn
      65: iconst_1
      66: ireturn

  protected boolean canEqual(java.lang.Object);
    Code:
       0: aload_1
       1: instanceof    #8                  // class com/powernode/lomboktest/model/User
       4: ireturn

  public int hashCode();
    Code:
       0: bipush        59
       2: istore_1
       3: iconst_1
       4: istore_2
       5: aload_0
       6: invokevirtual #17                 // Method getName:()Ljava/lang/String;
       9: astore_3
      10: iload_2
      11: bipush        59
      13: imul
      14: aload_3
      15: ifnonnull     23
      18: bipush        43
      20: goto          27
      23: aload_3
      24: invokevirtual #24                 // Method java/lang/Object.hashCode:()I
      27: iadd
      28: istore_2
      29: iload_2
      30: ireturn

  public java.lang.String toString();
    Code:
       0: aload_0
       1: invokevirtual #17                 // Method getName:()Ljava/lang/String;
       4: invokedynamic #28,  0             // InvokeDynamic #0:makeConcatWithConstants:(Ljava/lang/String;)Ljava/lang/String;
       9: areturn
}
```

**通过字节码可以看到Lombok库的`@Data`注解可以帮助我们生成`无参构造器`、`setter`、`getter`、`toString`、`hashCode`、`equals`。**



## 2.Lombok 的主要注解
1. **@Data**：
	+ 等价于 `@ToString`, `@EqualsAndHashCode`, `@Getter`，`@Setter`, `@RequiredArgsConstructor`,`@NoArgsConstructor`
	+ **用于生成：必要参数的构造方法、getter、setter、toString、equals 和 hashcode 方法**。
2. **@Getter** / **@Setter**：
	+ 分别用于生成所有的 getter 和 setter 方法。
	+ 可以作用于整个类，也可以作用于特定的字段。
3. **@NoArgsConstructor**：生成一个无参构造方法。
4. **@AllArgsConstructor**：生成一个包含所有实例变量的构造器。
5. **@RequiredArgsConstructor**：生成包含所有被 `final` 修饰符修饰的实例变量的构造方法。如果没有final的实例变量，则自动生成无参数构造方法**
6. **@ToString** / **@EqualsAndHashCode**：+ 用于生成 toString 和 equals/hashCode 方法。这两个注解都有**exclude**属性，通过这个属性可以定制toString、hashCode、equals方法。

**<font style="color:#DF2A3F;background-color:#ffffff;"></font>**
## 3.如何使用 Lombok？
创建一个普通的Maven模块来快速测试一下Lombok库的使用：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729146805658-b4d88132-5dbf-4419-a233-9723b9bf390d.png)


### 添加依赖
在 Maven 的 `pom.xml` 文件中添加 Lombok 依赖：

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.34</version>
    <scope>provided</scope>
</dependency>
```

### IDEA中安装Lombok插件
高版本的IntelliJ IDEA工具默认都是绑定Lombok插件的，不需要再额外安装：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729214346742-9d325648-834a-4fc0-b04f-09593d36c35e.png)



**<font style="color:#DF2A3F;">Lombok插件不是必须要安装的</font>**，为了提高开发效率以及开发者的体验，安装Lombok插件是有必要的。

也就是说安装了Lombok插件之后，编写代码的时候，才会有方法的提示功能。



当IDEA中没有安装lombok插件时：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729148836670-4ffa5660-3bc5-494b-afe8-dc8c24133f44.png)

但是程序可以正常执行：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729148873396-d5abf752-043b-457c-8cdd-f585e0101072.png)

如果在IDEA中安装了lombok插件：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729148922988-0cb43fc7-4233-4a7d-86f8-e328f28451fd.png)

程序会有很好的提示功能。

### 使用 Lombok 注解
在 Java 类中使用 Lombok 提供的注解。

```java
import lombok.Data;

@Data
public class User {
    private String name;
}
```

编写测试程序：

```java
package com.powernode.lomboktest;

import com.powernode.lomboktest.model.User;

public class Test {
    public static void main(String[] args) {
        User user = new User();
        user.setName("jackson");
        System.out.println(user.getName());
        System.out.println(user.toString());
        System.out.println(user.hashCode());
        User user2 = new User();
        user2.setName("jackson");
        System.out.println(user.equals(user2));
    }
}

```

测试结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729148006400-a32df299-5efe-49b7-82e4-34a7a23b4b88.png)




以下的注解可以自行测试：

+ @Getter
+ @Setter
+ @ToString【exclude属性】
+ @EqualsAndHashCode【exclude属性】
+ @NoArgsConstructor
+ @AllArgsConstructor
+ @RequiredArgsConstructor

**<font style="color:#DF2A3F;">注：Lombok只能帮助我们生成无参数构造方法和全参数构造方法，其他定制参数的构造方法无法生成。</font>**


## 4.Lombok的其他常用注解
其他常用插件：@Value，@Builder，@Singular，@Slf4j
### @Value
**该注解会给所有属性添加`final`，给所有属性提供`getter`方法，自动生成`toString`、`hashCode`、`equals`**

**通过这个注解可以创建不可变对象。**

```java
package com.powernode.lomboktest.model;

import lombok.Value;

@Value
public class Customer {
    Long id;
    String name;
    String password;
}
```

测试程序：

```java
package com.powernode.lomboktest;

import com.powernode.lomboktest.model.Customer;

public class CustomerTest {
    public static void main(String[] args) {
        Customer c1 = new Customer(1L, "jackson", "123");
        System.out.println(c1);
        System.out.println(c1.getId());
        System.out.println(c1.getName());
        System.out.println(c1.getPassword());
        System.out.println(c1.hashCode());
        Customer c2 = new Customer(1L, "jackson", "123");
        System.out.println(c1.equals(c2));
    }
}

```

运行结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729219457643-9a89e6b4-bc3c-4a3a-b456-462574324d7d.png)

可以查看一下字节码，你会发现，@Value注解的作用只会生成：全参数构造方法、getter方法、hashCode、equals、toString方法。（没有setter方法。）



### @Builder
#### ①GoF23种设计模式之一：建造者模式
建造者模式（Builder Pattern）属于创建型设计模式。GoF23种设计模式之一。

* **用于解决对象创建时参数过多的问题。它通过将对象的构造过程与其表示分离，使得构造过程可以逐步完成，而不是一次性提供所有参数**。建造模式的主要目的是让对象的创建过程更加清晰、灵活和可控。

简而言之，建造模式用于：

1. **简化构造过程**：通过逐步构造对象，**避免构造函数参数过多**。
2. **提高可读性和可维护性**：让构造过程更加清晰和有序。
3. **增强灵活性**：允许按需配置对象的不同部分。

这样可以更方便地创建复杂对象，并且使得代码更加易于理解和维护。


#### ② 建造模式的代码
建造模式代码如下：

```java
package com.powernode.lomboktest.model;

// 建造模式
public class Person {
    // 一般建造模式的bean属性使用final进行修饰，final可以加可以不加
    private final String name;
    private final int age;
    private final String email;

    //  提供一个私有的全参数的构造方法
    private Person(String name, int age, String email) {
        this.name = name;
        this.age = age;
        this.email = email;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public String getEmail() {
        return email;
    }
	// 通过这个公开的静态方法获取建造器对象。
    public static PersonBuilder builder() {
        return new PersonBuilder();
    }

    // 静态内部类:建造器建造起对象
    public static class PersonBuilder {
        private String name;
        private int age;
        private String email;
		//这里的name指的是内部类的name属性
        public PersonBuilder name(String name) {
            this.name = name;
            return this;
        }

        public PersonBuilder age(int age) {
            this.age = age;
            return this;
        }

        public PersonBuilder email(String email) {
            this.email = email;
            return this;
        }

        // 建造对象的核心方法，穿入的参数是内部类的属性值
        public Person build() {
            return new Person(name, age, email);
        }
    }

    @Override
    public String toString() {
        return "Person{" + "name='" + name + '\'' + ", age=" + age + ", email='" + email + '\'' + '}';
    }

    public static void main(String[] args) {
        Person person = Person.builder()
                .name("jackson")
                .age(20)
                .email("jackson@123.com")
                .build();
        System.out.println(person);
    }
}

```

执行结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729221598738-0d96abcb-bd11-47f5-a2f9-633b824a8b60.png)



#### ③使用@Builder注解自动生成建造模式的代码
该注解可以直接帮助我们生成以上的代码。使用`@Builder`注解改造以上代码。

```java
package com.powernode.lomboktest.model;

import lombok.Builder;
import lombok.Data;

@Data
@Builder
// 建造模式
public class Person {
    // 属性
    private String name;
    private int age;
    private String email;

    public static void main(String[] args) {
        Person person = Person.builder()
                .name("jackson")
                .age(20)
                .email("jackson@123.com")
                .build();
        System.out.println(person);
    }
}

```

执行结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729222063021-95517247-a76a-424a-aab9-511aa799bfe9.png)



### @Singular
* **@Singular注解是辅助@Builder注解的。@Singular("addPhone")里面传的是添加的方法。**
* **当被建造的对象的属性是一个集合，这个集合属性使用@Singular注解进行标注的话，可以连续调用集合属性对应的方法完成多个元素的添加。如果没有这个注解，则无法连续调用方法完成多个元素的添加**。代码如下：

```java
package com.powernode.lomboktest.model;

import lombok.Builder;
import lombok.Data;
import lombok.Singular;

import java.util.List;

@Data
@Builder
// 建造模式
public class Person {
    // 属性
    private final String name;
    private final int age;
    private final String email;
    // Singular翻译为：单数。表示一条一条添加
    @Singular("addPhone")
    private final List<String> phones;

    public static void main(String[] args) {
        Person person = Person.builder()
                .name("jackson")
                .age(20)
                .email("jackson@123.com")
                .addPhone("15222020214")
                .addPhone("14875421424")
                .addPhone("16855241424")
                .build();
        System.out.println(person);
    }
}

```

执行结果如下：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729222583196-6c5543d0-a6f2-44b3-92ea-8e12db9d7b72.png)



### @Slf4j
**Lombok 支持多种日志框架的注解，可以根据你使用的日志框架选择合适的注解。也就是说我们只需要加对应日志框架的注解，这样可以帮我们自当生成一个Logger对象，名字为log，该对象可以直接用**。以下是 Lombok 提供的部分日志注解及其对应的日志框架：

1. **`@Log4j`：**
    - 自动生成一个 `org.apache.log4j.Logger` 对象。
    - 适用于 Apache Log4j 1.x 版本。
2. **`@Slf4j`**：
    - 自动生成一个 `org.slf4j.Logger` 对象。
    - 适用于 SLF4J（Simple Logging Facade for Java），这是一种日志门面，可以与多种实际的日志框架（如 Logback、Log4j 等）集成。
3. **`@Log4j2`**：
    - 自动生成一个 `org.apache.logging.log4j.Logger` 对象。
    - 适用于 Apache Log4j 2.x 版本。



#### 使用示例
假设我们有一个类 `ExampleClass`，**并且我们想要使用 SLF4J 作为日志框架，我们可以这样使用 `@Slf4j` 注解：**

```java
package com.powernode.lomboktest.service;

import lombok.extern.slf4j.Slf4j;

@Slf4j
public class UserService {
    public void login(){
        log.info("登录验证...");
    }
    // 测试
    public static void main(String[] args) {
        UserService userService = new UserService();
        userService.login();
    }
}
```

在这个例子中，`log` 是一个静态成员变量，表示一个 `org.slf4j.Logger` 对象。Lombok 自动生成了这个日志对象，并且你可以直接使用它来进行日志记录。




#### 选择合适的注解
选择哪个注解取决于你使用的日志框架。例如：

+ 如果你使用的是 SLF4J，可以选择 `@Slf4j`。
+ 如果你使用的是 Log4j 1.x，可以选择 `@Log4j`。
+ 如果你使用的是 Log4j 2.x，可以选择 `@Log4j2`。

#### 注意事项
**确保在使用这些注解之前，已经在项目中引入了相应的日志框架依赖**。例如，如果你使用 SLF4J，你需要在项目中添加 SLF4J 的依赖，以及一个具体的日志实现（如 Logback）。对于其他日志框架，也需要相应地添加依赖。

#### 示例依赖
如果你使用 Maven 项目，并且选择了 SLF4J + Logback 的组合，可以添加以下依赖：

```xml
<!--Slf4j日志规范-->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>2.0.16</version>
</dependency>
<!--Slf4j日志实现：logback-->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.5.11</version>
</dependency>
```

通过这些日志注解，你可以方便地在类中使用日志记录功能，而无需手动创建日志对象。


执行结果：

![](https://cdn.nlark.com/yuque/0/2024/png/21376908/1729234205353-0573b981-7c8f-41ba-94eb-e5a857d8815d.png)
