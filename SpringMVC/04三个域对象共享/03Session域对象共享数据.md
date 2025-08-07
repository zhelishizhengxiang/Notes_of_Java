
**在SpringMVC中使用session域共享数据，实现方式有多种，其中比较常见的两种方式：**  
1. **使用原生Servlet API：控制器方法上添加 HttpSession参数即可.SpringMVC会自动将session对象传递给这个参数**
2. **使用SessionAttributes注解:使用@SessionAttributes注解标注Controller。标注了当key是@SessionAttributes中属性的值时，数据将被存储到会话session中。如果没有SessionAttributes注解，默认存储到request域中**



## 1.使用原生Servlet API
```java
package com.powernode.springmvc.controller;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpSession;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * ClassName: SessionScopeTestController
 * Description:
 * Datetime: 2024/3/18 17:18
 * Author: 老杜@动力节点
 * Version: 1.0
 */
@Controller
public class SessionScopeTestController {

    @RequestMapping("/testSessionScope1")
    public String testServletAPI(HttpSession session) {
        // 向会话域中存储数据
        session.setAttribute("testSessionScope1", "使用原生Servlet API实现session域共享数据");
        return "view";
    }

}
```


视图页面：
```html
<div th:text="${session.testSessionScope1}"></div>
```

超链接：
```html
<a th:href="@{/testSessionScope1}">在SpringMVC中使用原生Servlet API实现session域共享数据</a><br>
```


## 2.使用@SessionAttributes注解

使用SessionAttributes注解标注Controller：
```java
package com.powernode.springmvc.controller;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpSession;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.SessionAttributes;

/**
 * ClassName: SessionScopeTestController
 * Description:
 * Datetime: 2024/3/18 17:18
 * Author: 老杜@动力节点
 * Version: 1.0
 */
@Controller
@SessionAttributes(value = {"x", "y"})// 标注x和y都是存放到session域中，而不是request域。
public class SessionScopeTestController {

    @RequestMapping("/testSessionScope2")
    public String testSessionAttributes(ModelMap modelMap){
        // 向session域中存储数据
        modelMap.addAttribute("x", "我是埃克斯");
        modelMap.addAttribute("y", "我是歪");

        return "view";
    }
}
```

**注意：SessionAttributes注解使用在Controller类上。标注了当key是 x 或者 y 时，数据将被存储到会话session中。如果没有 SessionAttributes注解，默认存储到request域中。**
