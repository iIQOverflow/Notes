# spring.io之Building a RESTful Web Service

## 创建工程
这里在Intellij IDEA中使用gradle导入工程。
1. `git clone https://github.com/spring-guides/gs-rest-service.git`
2. 导入时选择`build.gradle`，注意jdk版本为1.8。
## 创建资源表示文件
创建一个简单老式Java对象（Plain Old Java Objects），它的定义是：

它可以**包含业务逻辑或持久化逻辑**，但**不担当任何特殊角色且不继承或不实现任何其它Java框架的类或接口**。

`src/main/java/hello/Greeting.java`
```java
package hello;

public class Greeting {

    private final long id;
    private final String content;

    public Greeting(long id, String content) {
        this.id = id;
        this.content = content;
    }

    public long getId() {
        return id;
    }

    public String getContent() {
        return content;
    }
}
```
## 创建资源管理器
Spring创建Web服务的方法中，HTTP请求由控制器处理。这些组件可以由[RestController](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/bind/annotation/RestController.html)注解轻松识别，下面的`GreetingController`通过返回`Greeting`类的新实例来处理`/greeting`的`GET`请求：

`src/main/java/hello/GreetingController.java`
```java
package hello;

import java.util.concurrent.atomic.AtomicLong;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {

    private static final String template = "Hello, %s!";
    private final AtomicLong counter = new AtomicLong();

    @RequestMapping("/greeting")
    public Greeting greeting(@RequestParam(value="name", defaultValue="World") String name) {
        return new Greeting(counter.incrementAndGet(),
                            String.format(template, name));
    }
}
```
`@RequestMapping`注释确保对`/greeting`的`HTTP`请求映射到`greeting()`方法。

注意：上面的示例未指定`GET`与`PUT`，`POST`等，因为`@RequestMapping`默认映射所有`HTTP`操作。使用`@RequestMapping（method = GET）`缩小此映射范围。

方法体的实现基于计数器的下一个值创建并返回具有`id`和`content`属性的新`Greeting`对象，并使用`greeting`模板格式化给定名称。

传统MVC控制器和上面的RESTful Web服务控制器之间的关键区别在于创建`HTTP`响应主体的方式。这个`RESTful Web`服务控制器只是填充并返回一个`Greeting`对象，而不是依靠视图技术来执行将问候数据的服务器端呈现为`HTML`。对象数据将作为`JSON`直接写入`HTTP`响应。

此代码使用`Spring 4`的新`@RestController`注释，它将类标记为控制器，其中**每个方法都返回一个域对象而不是视图**。这是`@Controller`和`@ResponseBody`汇总在一起的简写。

Greeting对象必须转换为`JSON`。由于Spring的`HTTP`消息转换器支持，无需手动执行此转换。因为`Jackson 2`在类路径上，所以会自动选择Spring的`MappingJackson2HttpMessageConverter`将`Greeting`实例转换为`JSON`。

## 应用执行
`src/main/java/hello/Application.java`
```java
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```
`@SpringBootApplication`注解是十分方便，它相当于添加了以下注解：
* `@Configuration`将类标记为应用程序上下文的bean定义源。
* `@EnableAutoConfiguration`告诉Spring Boot根据类路径设置，其他bean和各种属性设置开始添加bean。
* 通常你会为Spring MVC应用程序添加`@EnableWebMvc`，但Spring Boot会在类路径上看到spring-webmvc时自动添加它。这会将应用程序标记为Web应用程序并激活关键行为，例如设置DispatcherServlet。
* `@ComponentScan`告诉Spring在hello包中寻找其他组件，配置和服务，允许它找到控制器。

`main()`方法使用Spring Boot的`SpringApplication.run()`方法来启动应用程序。是否注意到没有一行XML？也没有`web.xml`文件。此Web应用程序是100％纯Java，您无需处理配置任何管道或基础结构。
## 测试
访问[http://localhost:8080/greeting](http://localhost:8080/greeting)，就会看到：
```json
{"id":1,"content":"Hello, World!"}
```
向[http://localhost:8080/greeting?name=User](http://localhost:8080/greeting?name=User)中提供`name`查询字符串，
```json
{"id":2,"content":"Hello, User!"}
```
此更改表明`GreetingController`中的`@RequestParam`排列正在按预期工作。`name`参数的默认值为“World”，但始终可以通过查询字符串显式覆盖。

另请注意id属性如何从`1`更改为`2`.这证明在多个请求中针对相同的`GreetingController`实例工作，并且其计数器字段在每次调用时按预期递增。
