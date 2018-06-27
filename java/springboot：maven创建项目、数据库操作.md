


### maven创建spring boot

[参考：maven环境搭建](http://www.cnblogs.com/wenziii/p/6075929.html)

[参考：使用Eclipse插件创建Java WebApp项目](http://www.cnblogs.com/wenziii/p/6075929.html)

**一、创建项目**

1、创建maven project

2、选择“maven-archetype-webapp”（不要勾选第一项 “Create a simple project”）

3、填写项目相关信息，Finish。

4、修改JRE版本。

```java
<build>
  <plugins>
  <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-compiler-plugin</artifactId>
  <version>2.1</version>
  <configuration>
  <source>1.7</source>
  <target>1.7</target>
  </configuration>
  </plugin>
  </plugins>
</build>
```

设置完后，选中项目右击，弹出的菜单中选择 Maven->Update Project .

5、修改错误信息

更新之后，发现项目还存在错误。

在项目根目录右击弹出菜单，选择 Build Path ->Configure Buile Path，进入项目的 Java build path 配置页面：

![](img/1.png)

这时只要将 Libraries 标签中的 JRE System Library (双击)设置为本机默认的(workspace default JRE)就可以解决该错误

6、添加服务器运行环境（Java Application）

在 Eclipse 中对项目设置一下运行主函数就可以了。

切换到 Libraries 标签，点击“Add Library”按钮。

**二、修改pom.xml**

```java

<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>1.4.3.RELEASE</version>
</parent>


<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>


```

主函数入口

```java
package com.spring;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
@SpringBootApplication
public class AppleApplication {
    public static void main(String[] args) {
        SpringApplication.run(AppleApplication.class, args);
    }
    
}

package com.spring.controller;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
@Controller
public class Controller2 {
	@RequestMapping("/hi")
	public String hi() {
		return "index.html";
	}
}

package com.spring.controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
@RestController
public class Controller1 {
	@RequestMapping("/login")
	public String login() {
		return "login";
	}
}

```
[参考：CSDN](https://blog.csdn.net/webzhuce/article/details/54176004)

