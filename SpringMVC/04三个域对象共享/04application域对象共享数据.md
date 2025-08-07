
**在SpringMVC实现application域数据共享，最常见的方案就是直接使用Servlet API了：**

使用的频率不高
```java
package com.powernode.springmvc.controller;

import jakarta.servlet.ServletContext;
import jakarta.servlet.http.HttpServletRequest;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * ClassName: ApplicationScopeTestController
 * Description:
 * Datetime: 2024/3/18 17:37
 * Author: 老杜@动力节点
 * Version: 1.0
 */
@Controller
public class ApplicationScopeTestController {

    @RequestMapping("/testApplicationScope")
    public String testApplicationScope(HttpServletRequest request){
	    // 将数据存储到application域当中  
		// 怎么获取ServletContext对象？通过request，通过session都可以获取。
        // 获取ServletContext对象
        ServletContext application = request.getServletContext();

        // 向应用域中存储数据
        application.setAttribute("applicationScope", "我是应用域当中的一条数据");

        return "view";
    }
}

```



视图页面：
```html
<div th:text="${application.applicationScope}"></div>
```

超链接：
```html
<a th:href="@{/testApplicationScope}">在SpringMVC中使用ServletAPI实现application域共享数据</a><br>
```

