


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

**二、连接mysql**

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
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>


<properties>
	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	<java.version>1.8</java.version>
	<springBoot.groupId>org.springframework.boot</springBoot.groupId>
</properties>


<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>

```

src/main/resources/application.properties

```java
spring.jpa.hibernate.ddl-auto=create
spring.datasource.url=jdbc:mysql://localhost:3306/db_example
spring.datasource.username=springuser
spring.datasource.password=ThePassword
```

主函数入口

```java
package com.spring;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import com.spring.controller.User;
import com.spring.controller.UserRepository;
@SpringBootApplication
public class AppleApplication {
    public static void main(String[] args) {
        SpringApplication.run(AppleApplication.class, args);
    }
    @Bean
	public CommandLineRunner demo(UserRepository repository) {
		return (args) -> {
			repository.save(new User(1, "Bauer"));
		};
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

package com.spring.controller;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
@Entity 
public class User {
    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private Integer id;
    private String name;
    public User() {};
    public User(int i, String string) {};
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
}

package com.spring.controller;
import org.springframework.data.repository.CrudRepository;
public interface UserRepository extends CrudRepository<User, Long> {}

```

[参考:accessing-data-mysql](https://spring.io/guides/gs/accessing-data-mysql/)

[参考:accessing-data-jpa](http://spring.io/guides/gs/accessing-data-jpa/)

**javabean**

```java
// @Entity注释指名这是一个实体Bean
@Entity
/**
* @Table注释指定了Entity所要映射带数据库表
* 其中name用来指定映射表的表名。
* 如果缺省@Table注释，系统默认采用类名作为映射表的表名
*/
@Table(name = "tbl_user", schema = "SIMULATE" )

public class User implements Serializable {

    // @Id注释指定表的主键，
    // 1、TABLE：容器指定用底层的数据表确保唯一；
    // 2、SEQUENCE：使用数据库德SEQUENCE列莱保证唯一（Oracle数据库通过序列来生成唯一ID）；
    // 3、IDENTITY：使用数据库的IDENTITY列莱保证唯一；
    // 4、AUTO：由容器挑选一个合适的方式来保证唯一；
    // 5、NONE：容器不负责主键的生成，由程序来完成。
    // @GeneratedValue注释定义了标识字段生成方式
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Integer id;

    // @Temporal注释用来指定java.util.Date或java.util.Calender属性
    // 与数据库类型date、time或timestamp中的那一种类型进行映射。
    @Temporal(value=TemporalType.TIME)
    private Date birthday;

    // @Column注释定义了将成员属性映射到关系表中的哪一列和该列的结构信息
    // 1、name：映射的列名。如：映射tbl_user表的name列，可以在name属性的上面或getName方法上面加入；
    // 2、unique：是否唯一；
    // 3、nullable：是否允许为空；
    // 4、length：对于字符型列，length属性指定列的最大字符长度；
    // 5、insertable：是否允许插入；
    // 6、updatetable：是否允许更新；
    // 7、columnDefinition：定义建表时创建此列的DDL；
    // 8、secondaryTable：从表名。如果此列不建在主表上（默认是主表），该属性定义该列所在从表的名字。

    @Column(name = "name")
    private String name;

    @Column(name = "age")
    private String age;

}
```


