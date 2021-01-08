# SpringMVC

SSM：MyBatis + Spring + SpringMVC 

## 1、MVC

### 1.1 MVC 介绍

- MVC 是模型（Model）、视图（View）、控制器（Controller）的简写，是一种软件设计规范。
- 是将**业务逻辑、数据、显示分离**的方法来组织代码。
- MVC 主要作用是**降低了视图与业务逻辑间的双向耦合**。
- MVC 不是一种设计模式，**MVC是一种架构模式**。当然不同的 MVC 存在差异。

**Model（模型）：**数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或 JavaBean 组件（包含数据和行为），不过现在一般都分离开来：**Value Object（数据 Dao） 和 服务层（行为 Service）**。也就是模型提供了模型数据查询和模型数据的状态更新等功能，包括数据和业务。

**View（视图）：**负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。

**Controller（控制器）：**接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型数据返回给视图，由视图负责展示。也就是说控制器做了个**调度员**的工作。

**最典型的MVC就是 JSP + servlet + javabean 的模式。**

### 1.2 Model1 时代

- 在 web 早期的开发中，通常采用的都是 Model1。
- Model1 中，主要分为两层：视图层和模型层。

<img src="SpringMVC/pictures/image-20200622231646874.png" alt="image-20200622231646874" style="zoom: 50%;" />

Model1 优点：架构简单，比较适合小型项目开发；

Model1 缺点：JSP职责不单一，职责过重，不便于维护。

### 1.3 Model2 时代

Model2 把一个项目分成三部分，包括**视图、控制、模型。**

1. 用户发请求
2. Servlet 接收请求数据，并调用对应的业务逻辑方法
3. 业务处理完毕，返回更新后的数据给 servlet
4. servlet 转向到 JSP，由 JSP 来渲染页面
5. 响应给前端更新后的页面

<img src="SpringMVC/pictures/image-20200622231745792.png" alt="image-20200622231745792" style="zoom:50%;" />

**职责分析：**

- **Controller 控制器：**取得表单数据；调用业务逻辑；转向指定的页面

- **Model 模型：**业务逻辑；保存数据的状态

- **View 视图：**显示页面

Model2 这样不仅提高的代码的复用率与项目的扩展性，且大大降低了项目的维护成本。Model1 模式的实现比较简单，适用于快速开发小规模项目，Model1 中 JSP 页面身兼 View 和 Controller 两种角色，将控制逻辑和表现逻辑混杂在一起，从而导致代码的重用性非常低，增加了应用的扩展性和维护的难度。Model2 消除了 Model1 的缺点。

### 1.4 Servlet

1. 新建一个Maven工程当做父工程，pom 文件中导入依赖和解决静态资源过滤问题

   ```xml
   <!--依赖-->
   <dependencies>
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.12</version>
       </dependency>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-webmvc</artifactId>
           <version>5.1.9.RELEASE</version>
       </dependency>
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>servlet-api</artifactId>
           <version>2.5</version>
       </dependency>
       <dependency>
           <groupId>javax.servlet.jsp</groupId>
           <artifactId>jsp-api</artifactId>
           <version>2.2</version>
       </dependency>
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>jstl</artifactId>
           <version>1.2</version>
       </dependency>
   </dependencies>
   <!--Maven可能存在资源过滤的问题-->
   <build>
       <resources>
           <resource>
               <directory>src/main/java</directory>
               <includes>
                   <include>**/*.properties</include>
                   <include>**/*.xml</include>
               </includes>
               <filtering>false</filtering>
           </resource>
           <resource>
               <directory>src/main/resources</directory>
               <includes>
                   <include>**/*.properties</include>
                   <include>**/*.xml</include>
               </includes>
               <filtering>false</filtering>
           </resource>
       </resources>
   </build>
   ```

2. 新建一个模块，添加 Web app的支持

   步骤：在模块名上右键 —> Add Framework Support —> 选中 Web Application 生成一个 web.xml 文件 （这样操作的原因是：maven 直接创建的 web 项目中 web.xml 不是最新版，而我们需要最新版）

3. 导入servlet 和 jsp 的 jar 依赖

4. 编写一个 Servlet 类，用来处理用户的请求

   ```java
   // 实现servlet接口
   public class HelloServlet extends HttpServlet {
   
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           // 取得参数
           String method = req.getParameter("method");
           if (method.equals("add")){
               req.getSession().setAttribute("msg","执行了add方法");
           }
           if (method.equals("delete")){
               req.getSession().setAttribute("msg","执行了delete方法");
           }
           // 业务逻辑
           // 视图跳转，转发或重定向，这里用了转发
           req.getRequestDispatcher("/WEB-INF/jsp/test.jsp").forward(req,resp);
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req,resp);
       }
   }
   ```

5. 编写 test.jsp，放在 WEB-INF 目录下新建的 jsp 文件夹中

   注意：页面为了安全，使用户不可见，放在 WEB-INF 下面，公共页面放在 web 下面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
       ${msg}
   </body>
   </html>
   ```

6. 在 **web.xml 中注册 Servlet**

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
       <servlet>
           <servlet-name>hello</servlet-name>
           <servlet-class>com.song.servlet.HelloServlet</servlet-class>
       </servlet>
   
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
   
       <!--配置超时时间，15分钟-->
       <!--<session-config>-->
           <!--<session-timeout>15</session-timeout>-->
       <!--</session-config>-->
   
       <!--欢迎页-->
       <!--<welcome-file-list>-->
           <!--<welcome-file>index.jsp</welcome-file>-->
       <!--</welcome-file-list>-->
   </web-app>
   ```

7. 配置 Tomcat，将项目部署在服务器中，并启动测试

   - ==localhost:8080/hello?method=add	       最后页面上会显示：执行了add方法==

   - localhost:8080/user?method=delete       最后页面上会显示：执行了delete方法


**MVC 框架要做的事情：**

1. 将 url 映射到 java 类或 java 类的方法
2. 封装用户提交的数据
3. 处理请求，调用相关的业务处理，封装响应数据
4. 将响应的数据进行渲染 . jsp / html 等表示层数据

**说明：**

常见的服务器端MVC框架有：Struts、Spring MVC、ASP.NET MVC、Zend Framework、JSF；

常见前端MVC框架：vue、angularjs、react、backbone；

由MVC演化出了另外一些模式如：MVP、MVVM（M|V|VM: viewModel 双向绑定） 等等....



## 2、Spring MVC

### 2.1 简介

Spring MVC 是Spring Framework的一部分，是基于 **Java 实现 MVC 的轻量级 Web 框架**。

> 官方文档：https://docs.spring.io/spring/docs/5.2.7.RELEASE/spring-framework-reference/web.html#spring-web

Spring MVC 特点：

1. 轻量级，简单易学
2. 高效 , 基于请求响应的 MVC 框架
3. 与 Spring 兼容性好，无缝结合
4. 约定优于配置
5. 功能强大：**RESTful**、数据验证、格式化、本地化、主题等
6. 简洁灵活

### 2.2 中心控制器

Spring 的 web 框架围绕 **DispatcherServlet** [ 调度Servlet ] 设计。DispatcherServlet 的作用是**将请求分发到不同的处理器**。

从Spring 2.5开始，使用 Java 5 或者以上版本的用户可以采用基于注解形式进行开发，十分简洁。

Spring MVC 框架像许多其他 MVC 框架一样，**以请求为驱动，围绕一个中心 Servlet 分派请求及提供其他功能**，**DispatcherServlet 是一个实际的 Servlet（它继承自 HttpServlet 基类）**。

#### SpringMVC 的原理

当发起请求时，被前置的控制器拦截到请求；**根据请求参数生成代理请求**，找到请求对应的实际控制器；控制器处理请求，创建数据模型，访问数据库；将模型响应给中心控制器；控制器使用模型与视图渲染视图结果；将结果返回给==中心控制器==；再将结果返回给请求者。

![image-20200623223723506](SpringMVC/pictures/image-20200623223723506.png)

### 2.3 Spring 具体执行流程

<img src="SpringMVC/pictures/springmvc.png" alt="image-20200624092256226" style="zoom: 80%;" />

1. 【**DispatcherServlet**】 表示**前置控制器，是整个 SpringMVC 的控制中心**。用户发出请求，DispatcherServlet 接收请求并拦截请求。

   假设请求的 url 为 : http://localhost:8080/SpringMVC/hello，这个 url 可被拆成三部分：
   
   - http://localhost:8080  服务器域名；SpringMVC 部署在服务器上的 web 站点；hello 表示控制器。

   - 所以，url 表示为：请求位于服务器 localhost:8080 上的 SpringMVC 站点的 hello 控制器。

2. 【**HandlerMapping**】 处理器映射器，DispatcherServlet 调用它，它会**根据请求 url 查找 Handler**。
3. HandlerExecution 具体的 Handler，主要作用是**根据 url 查找控制器**，比如上面的 url 被查找的控制器为 hello。
4. HandlerExecution 将**解析后的信息传递给 DispatcherServlet**，如解析控制器映射等。
5. 【**HandlerAdapter**】 处理器适配器，其**按照特定的规则去执行 Handler**。
6. 具体的 Controller 来执行 Handler。
7. Controller 将具体的执行信息返回给 HandlerAdapter，如 ModelAndView。
8. HandlerAdapter 将**视图逻辑名或模型**传递给 DispatcherServlet。
9. DispatcherServlet 调用视图解析器（ViewResolver）来解析 HandlerAdapter 传递的逻辑视图名。
10. 【**ViewResolver**】 将**解析过的逻辑视图名**传给 DispatcherServlet。
11. DispatcherServlet 根据视图解析器解析的视图结果，**调用具体的视图**。
12. 最终视图呈现给用户。

**对应编写程序时的实现步骤为：**

1. 新建 web 项目，导入相关 jar 包
2. 编写 web.xml , 注册 DispatcherServlet
3. 编写 SpringMVC 配置文件
4. 创建对应的控制类 , Controller
5. 完善前端视图和 controller 之间的对应
6. 测试运行调试



## 3、Spring MVC 入门程序

### 3.1 使用配置

1. 新建一个模块，添加 web 的支持

2. 确定项目中有 spring MVC 的依赖，确定发布的项目中也有所有的依赖

   在 Project Structure 中查看 WEB-INF 目录下是否有 lib 文件夹，如果没有则新建一个，并把所有依赖加入。

3. 配置 web.xml，注册 DispatcherServlet

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
       <!--配置 DispatcherServlet：spring MVC的核心，请求分发器/前端控制器-->
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--DispatcherServlet 要绑定 spring-MVC 的配置文件-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <!--启动级别：1 和服务器一块启动-->
           <load-on-startup>1</load-on-startup>
       </servlet>
   
       <!--
           在 spring MCV 中：/  和   /*区别
           / ：只匹配所有的请求，不会去匹配 jsp 页面，即：.jsp不会进入spring的 DispatcherServlet类【一般都写 /】
           /*：匹配所有的请求，包括 jsp 页面，会出现返回 jsp视图 时再次进入spring的DispatcherServlet 类，导致找不到对应的controller所以报 404 错。
       -->
       <servlet-mapping>
           <servlet-name>springmvc</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   
   </web-app>
   ```

4. 编写 SpringMVC 的配置文件 springmvc-servlet.xml  （在 src—main—resources 文件夹下）

   命名：【servlet-name】-servlet.xml

5. 添加【处理映射器】

6. 添加【处理器适配器】

7. 添加【视图解析器】

   ```
   视图解析器：DispatcherServlet 给它 ModelAndView
       1、获取了 ModelAndView 中的数据
       2、解析 ModelAndView 的视图名字
       3、拼接视图名字，找到对应的视图 /WEB-INF/jsp/hello.jsp
       4、将数据渲染到视图上
   ```

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--处理器映射器-->
       <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
   
       <!--处理器适配器-->
       <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
   
       <!--视图解析器：模板引擎 Thymeleaf Freemarker-->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!-- 后缀 -->
           <property name="suffix" value=".jsp"/>
       </bean>
   </beans>
   ```

8. 编写要操作业务的 Controller 类 ，要么实现 Controller 接口，要么增加注解；需要返回一个 ModelAndView，封装数据，视图跳转

   ```java
   public class HelloController implements Controller {
       public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
           // ModelAndView 模型和视图
           ModelAndView mv = new ModelAndView();
   		// 调用业务层
           // 封装对象，放在 ModelAndView 中
           String result = "HelloSpringMVC";
           mv.addObject("msg", result);
           // 封装要跳转的视图，放在ModelAndView中，视图跳转
           mv.setViewName("test");
           return mv;
       }
   }
   ```

9. 将自己的类交给 SpringIOC 容器，注册 bean

   ```xml
   <!--BeanNameUrlHandlerMapping：bean-->
   <bean id="/hello" class="com.song.controller.HelloController"/>
   ```

10. 编写要跳转的 test.jsp  页面（放在 WEB-INF 下新建的 jsp 文件夹中），显示 ModelandView 存放的数据，以及我们的正常页面，即完善前端视图和 controller 之间的对应。

    注意：在视图解析器中我们把所有的视图都存放在/WEB-INF/目录下，这样可以保证视图安全，因为这个目录下的文件，客户端不能直接访问。

    ```jsp
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>Title</title>
    </head>
    <body>
    	${msg}
    </body>
    </html>
    ```

11. 配置 Tomcat，部署项目，启动测试，在浏览器地址栏输入请求路径 localhost:8080/hello

可能遇到的问题：访问出现 404。排查步骤：

1. 查看控制台输出，看一下是不是缺少了什么 jar 包。
2. 如果 jar 包存在，显示无法输出，就在 IDEA 的项目发布中，添加 lib 依赖！
3. 重启 Tomcat 即可解决！

注意：实际开发中并不会这样写程序。

### 3.2 使用注解

0. 新建模块、添加依赖等，和上面一样

1. 配置 web.xml，和使用配置的 web.xml 相同

   - web.xml 文件要最新版；
   - 注册 DispatcherServlet；
   - 关联 SpringMVC 的配置文件；
   - 启动级别为 1；
   - 映射路径为 / 【不要用 /*，会 404】

2. 编写 Spring MVC 配置文件，注意要导入头文件约束

   为了支持基于注解的 IOC，设置了**自动扫描包**的功能

   - 让 IOC 的注解生效（开启自动扫描包）；
   - **静态资源过滤**，让 Spring MVC 不处理静态资源 ：HTML、 . JS、 . CSS 、图片和视频等；
   - **MVC 的注解驱动**；
   - 配置视图解析器

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd
          http://www.springframework.org/schema/mvc
          https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
       <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
       <context:component-scan base-package="com.song.controller"/>
       <!-- 让Spring MVC不处理静态资源 .css .js  .html .mp3  .mp4...-->
       <mvc:default-servlet-handler />
       <!--
       支持mvc注解驱动
           在spring中一般采用@RequestMapping注解来完成映射关系
           要想使@RequestMapping注解生效
           必须向上下文中注册DefaultAnnotationHandlerMapping
           和一个AnnotationMethodHandlerAdapter实例
           这两个实例分别在类级别和方法级别处理。
           而annotation-driven配置帮助我们自动完成上述两个实例的注入。
        -->
       <mvc:annotation-driven />
   
       <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
             id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/WEB-INF/jsp/" />
           <!-- 后缀 -->
           <property name="suffix" value=".jsp" />
       </bean>
   
   </beans>
   ```

3. 创建控制类 Controller，添加注解

   - @Controller 是为了让 Spring IOC 容器初始化时自动扫描到；
   - @RequestMapping 是为了**映射请求路径**，类与方法上都可以有映射；
   - 方法中声明 Model 类型的参数是为了把 Action 中的**数据带到视图中**;
   - 方法**返回的结果是要跳转的页面**，因此写上视图的名称 hello，加上配置文件中的前后缀变成 WEB-INF/jsp/**hello**.jsp

   ```java
   @Controller
   @RequestMapping("/hello")
   public class HelloController {
   
       // 访问地址：localhost:8080/hello/h1 
       // 如果没有上面的 hello，直接是 localhost:8080/h1
       @RequestMapping("/h1")
       public String hello(Model model){
           // 封装数据，向模型中添加属性msg与值，可以在JSP页面中取出并渲染
           model.addAttribute("msg", "hello,SpringMVCAnnotation");
           return "hello"; //会被视图解析器处理
       }
   }
   ```

4. 创建视图层，和之前一样

   - WEB-INF/ jsp 目录下创建 hello.jsp，视图可以直接取出并展示从 Controller 带回的信息；
   - 可以通过 EL 表达式取出 Model 中存放的值或者对象。

5. 配置 Tomcat 运行，在浏览器地址栏输入请求路径 localhost:8080/hello/h1   

### 小结：

使用springMVC必须配置的三大件：**处理器映射器、处理器适配器、视图解析器**。

通常，我们只需要**手动配置视图解析器**，而**处理器映射器**和**处理器适配器**只需要开启**注解驱动**即可，省去大段的 xml 配置。



## 4、控制器 Controller

**控制器：**

- 控制器负责提供访问应用程序的行为，通常通过接口定义或注解定义两种方法实现
- 控制器负责解析用户的请求并将其转换为一个模型
- 在 Spring MVC 中一个控制器类可以包含多个方法
- 在 Spring MVC 中，对于 Controller 的配置方式有很多种

**注意：**

- 实现接口 Controller 定义控制器是较老的办法
- 缺点是：一个控制器中只有一个方法，如果要多个方法则需要定义多个 Controller，定义的方式比较麻烦

### 4.1 实现 Controller 接口

Controller 是一个接口（函数式接口），在 org.springframework.web.servlet.mvc 包下，实现该接口的类获得控制器功能，接口中只有一个方法，源码如下：

```java
@FunctionalInterface
public interface Controller {
	//处理请求且返回一个模型与视图对象
   @Nullable
   ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception;
}
```

**测试：**

1. SpringMVC 的配置文件中只有视图解析器

   **不用配置处理器和适配器就可以使用，因为 Spring 自动帮我们配置了**

2. 编写一个 controller 类

   ```java
   // 只要实现了 Controller 接口的类，说明这就是一个控制器了
   public class ControllerDemoTest1 implements Controller {
       public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
           ModelAndView mv = new ModelAndView();
           
           mv.addObject("msg","hello, ControllerDemoTest1");
           mv.setViewName("test");
           
           return mv;
       }
   }
   ```

3. 在 SpringMVC 配置文件中注册请求的 bean，name 对应请求路径，class 对应处理请求的类

   ```xml
   <bean name="/t1" class="com.song.controller.ControllerDemoTest1"/>
   ```

4. WEB-INF/jsp 目录下编写前端 test.jsp，对应视图解析器

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   	${msg}
   </body>
   </html>
   ```

5. 配置 Tomcat 运行测试，在浏览器地址栏输入 http://localhost:8080/t1
6. 测试结果：页面上会显示 hello, ControllerDemoTest1

### 4.2 使用注解 @Controller【平时使用的最多的方式】

@Controller 注解类型用于声明 Spring 类的实例是一个控制器（IOC 中还有 3 个相同功能的注解）：

- @Component  组件

- @Service           service

- @Repository     dao

**测试：**

1. SpringMVC 的配置文件中除了配置视图解析器，还要配置自动扫描包

   因为 Spring 可以使用扫描机制来找到应用程序中所有基于注解的控制器类，为了保证 Spring 能找到我们的控制器，需要在配置文件中声明组件扫描。

   ```xml
   <context:component-scan base-package="com.song.controller"/>
   ```

2. 编写一个 controller 类，使用注解实现；

   ```java
   // 被这个注解的类中的所有方法，如果返回值是String，并且有具体的页面可以跳转，那么就会被视图解析器解析
   @Controller // 代表这个类会被 Spring接管，
   public class ControllerTest2 {
   
       @RequestMapping("/t2")   //映射访问路径
       public String test(Model model){
   		//Spring MVC会自动实例化一个Model对象用于向视图中传值
           model.addAttribute("msg", "Hello, ControllerTest2");
   		//返回视图位置
           return "test"; //  WEB-INF/jsp/test.jsp
       }
   }
   ```

3. 配置 Tomcat 运行测试，在浏览器地址栏输入 http://localhost:8080/t2

4. 测试结果：页面上会显示 hello, ControllerDemoTest2

**注意**：这两种实现控制器的方式，两个请求都可以指向一个视图，但是页面的结果是不一样的，从这里可以看出视图是被复用的，即控制器与视图之间是弱耦合关系。



## 5、RequestMapping

@RequestMapping 注解用于**映射 url 到控制器类或一个特定的处理程序方法**。

可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。

- 只注解在方法上：访问路径：http://localhost:8080 / 项目名 / t1

```java
@Controller
public class ControllerTest3 {

    @RequestMapping("t1")
    public String test1(Model model){
        model.addAttribute("msg", "test3");
        return "test";
    }
}
```

- 同时注解类和方法上：访问路径：http://localhost:8080 / 项目名 /c3/t1

```java
@Controller
//@RequestMapping("/c3")
public class ControllerTest3 {

    //@RequestMapping("/t1")
    public String test1(Model model){
        model.addAttribute("msg", "test3");
        return "test";
    }
}
```

注意：一般直接在方法上写 `//@RequestMapping("/c3/t1")`，和同时在类和方法上注解作用一样。



## 6、结果跳转方式

### 6.1 ModelAndView

设置 ModelAndView 对象 , 根据 **view 的名称和视图解析器跳到指定的页面** 

页面 : 视图解析器前缀 + viewName + 视图解析器后缀

```java
public class ControllerDemoTest1 implements Controller {
   
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        ModelAndView mv = new ModelAndView();
        
        mv.addObject("msg","hello, ControllerDemoTest1");
        mv.setViewName("test");
        //返回一个模型视图对象
        return mv;
    }
}
```

### 6.2 ServletAPI

通过设置 ServletAPI , 不需要视图解析器

1. 通过 HttpServletResponse 进行输出

2. 通过 HttpServletResponse 实现重定向

3. 通过 HttpServletRequest 实现转发

```java
@Controller
public class ModelTest {

    @RequestMapping("/m/t1")
    public String test1(HttpServletResponse response, HttpServletRequest request){
        HttpSession session = request.getSession();
        System.out.println(session.getId());
        return "test";
    }
    
    @RequestMapping("/m/t2")
   	public void test2(HttpServletRequest req, HttpServletResponse rsp) throws IOException {
       	rsp.getWriter().println("Hello,Spring By servlet API");
    }

   	@RequestMapping("/m/t3")
   	public void test3(HttpServletRequest req, HttpServletResponse rsp) throws IOException {
       	rsp.sendRedirect("/index.jsp");
    }

   	@RequestMapping("/m/t4")
   	public void test4(HttpServletRequest req, HttpServletResponse rsp) throws Exception {
      	 //转发
       	req.setAttribute("msg","/m/t4");
       	req.getRequestDispatcher("/WEB-INF/jsp/test.jsp").forward(req,rsp);
    }
}
```

### 6.3 SpringMVC

通过 SpringMVC 来实现**转发和重定向**有两种方式：需要视图解析器和没有视图解析器

- 无需视图解析器（测试前，需要将视图解析器注释掉）

- 有视图解析器

  **重定向不需要视图解析器，本质就是重新请求一个新地方**，所以注意路径问题。可以重定向到另外一个请求实现。

```java
public class ModelTest1 {
	// 没有视图解析器
    @RequestMapping("m1/t2")
    public String test2(Model model){
        model.addAttribute("msg","model test2");
        // 转发
//        return "/WEB-INF/jsp/test.jsp";
//        return "forward:/WEB-INF/jsp/test.jsp";
        // 重定向
        return "redirect:/index.jsp";
	}
    
    // 有视图解析器，默认转发，重定向的话要加上 redirect:/
    @RequestMapping("m1/t3")
    public String test3(Model model){
        model.addAttribute("msg","model test3");
        // 转发
//        return "test";
        // 重定向
        return "redirect:/index.jsp";
    }
```



## 7、数据处理

### 7.1 处理提交数据

1. **提交的域名称**和处理**方法的参数名**一致

   - Controller 类：

     提交的域名称为 name，方法参数名为 name，**可以直接识别**

   ```java
   @Controller
   @RequestMapping("/user")
   public class UserController {
   
       // localhost:8080/user/t1?name=xxx
       @GetMapping("/t1")
       public String test1(String name, Model model){
           //1.接收前端参数
           System.out.println("接收到前端的参数为" + name);
           //2.将返回的结果传递给前端 Model
           model.addAttribute("msg", name);
   
           return "test";
       }
   }
   ```

   - 结果：

   ```
   提交数据： localhost:8080/user/t1?name=xxx
   控制台输出：xxx
   前端 test.jsp 页面显示：xxx
   ```

2. 提交的域名称和处理方法的参数名**不一致** 

   - Controller 类：

     提交的域名称为 username，方法参数名为 name，要用 **@RequestParam(" ")** 注解**将来自前端的参数命名为和前端提交的域名称一致**。【建议来自前端的参数都加上这个注解 @RequestParam】

   ```java
   // localhost:8080/user/t1?username=xxx
   // @RequestParam 建议来自前端的参数都加上这个
   @GetMapping("/t1")
   public String test1(@RequestParam("username") String name, Model model){
       //1.接收前端参数
       System.out.println("接收到前端的参数为" + name);
       //2.将返回的结果传递给前端 Model
       model.addAttribute("msg", name);
   
       return "test";
   }
   ```

   - 结果：

   ```
   提交数据： localhost:8080/user/t1?username=xxx
   控制台输出：xxx
   前端 test.jsp 页面显示：xxx
   ```

3. 提交的是一个**对象**

   要求**提交的表单域和对象的属性名一致**  , **方法中的参数使用对象即可**。

   注意：如果使用对象，**前端传递的参数名和对象中的属性名必须一致**，否则就是 null。

   - 实体类 User

   ```java
   @Data
   @NoArgsConstructor
   @AllArgsConstructor
   public class User {
       private int id;
       private String name;
       private int age;
   }
   ```

   - Controller 类：

   ```java
   // 前端接收的是一个对象 id name age
   /*
       1.接收前端用户传递的参数，判断参数的名字，假设名字直接在方法上，可以直接使用
       2.假设传递的是一个对象，匹配对象的字段名，如果名字一致则 OK，否则，匹配不到
    */
   @GetMapping("/t2")
   public String test2(User user){
       System.out.println(user);
       return "test";
   }
   ```

   - 结果

   ```
   提交数据： localhost:8080/user/t1?name=zhangsan&id=1&age=3
   控制台输出：User(id=1, name=zhangsan, age=3)
   ```

### 7.2 数据显示到前端

1. 通过 ModelAndView（**实现 Controller 接口**）

   ```java
   public class ControllerDemoTest1 implements Controller {
       
       public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
           ModelAndView mv = new ModelAndView();
           mv.addObject("msg","hello, ControllerDemoTest1");
           mv.setViewName("test");
   
           return mv;
       }
   }
   ```

2. 通过 ModelMap

   ```java
   @Controller
   @RequestMapping("/user")
   public class UserController {
       
       @GetMapping("/t3")
       public String test3(ModelMap map){
           map.addAttribute("msg","ModelMap");
   
           return "test";
       }
   }
   ```

3.  通过 Model

   ```java
   @Controller
   @RequestMapping("/user")
   public class UserController {
       
       @GetMapping("/t1")
       public String test1(@RequestParam("username") String name, Model model){
           model.addAttribute("msg", name);
   
           return "test";
       }
   }
   ```

**三种方式的对比：**

- ModelAndView 可以**在储存数据的同时，可以进行设置返回的逻辑视图**，进行**控制展示层的跳转**。

- ModelMap：继承了 **LinkedHashMap**，除了实现了自身的一些方法，还拥有 LinkedHashMap 的全部功能。

- Model：精简版，**只有寥寥几个方法只适合用于储存数据**，简化了新手对于Model 对象的操作和理解，（大部分情况，我们都直接使用 Model）。

### 7.3 乱码问题

将后台的中文数据显示页面时会出现乱码问题，乱码问题都是通过过滤器解决。

注意：**get 方式不会乱码，post 方式会乱码**。

- SpringMVC 给我们提供了一个过滤器 , 可以在 web.xml 中配置 。

  ```xml
  <!--配置 springMVC 的乱码过滤，注意：<url-pattern> 要使用 /*-->
  <filter>
      <filter-name>encoding</filter-name>
      <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
      <init-param>
          <param-name>encoding</param-name>
          <param-value>utf-8</param-value>
      </init-param>
  </filter>
  <filter-mapping>
      <filter-name>encoding</filter-name>
      <url-pattern>/*</url-pattern>
  </filter-mapping>
  ```


- 之前的做法

  编写一个过滤器的类 EncodingFilter，实现 Filter 接口，并将其配置在 web.xml 文件中。

  乱码问题得以解决。

  ```java
  public class EncodingFilter implements Filter {
      public void init(FilterConfig filterConfig) throws ServletException {
      }
  
      public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
          request.setCharacterEncoding("utf-8");
          response.setCharacterEncoding("utf-8");
          chain.doFilter(request,response);
      }
      
      public void destroy() { 
      }
  }
  ```

  ```xml
  <!--注意：使用 /* 可以解决乱码问题，/ 不可以-->
  <filter>
      <filter-name>encoding</filter-name>
      <filter-class>com.song.filter.EncodingFilter</filter-class>
  </filter>
  <filter-mapping>
      <filter-name>encoding</filter-name>
      <url-pattern>/*</url-pattern>
  </filter-mapping>
  ```

  

## 8、RestFul 风格

### 8.1 概念

Restful 就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。

### 8.2 功能

资源：互联网所有的事物都可以被抽象为资源

资源操作：使用 POST、DELETE、PUT、GET 不同方法对资源进行操作，分别对应添加、 删除、修改、查询。

**传统方式操作资源**  ：通过**不同的参数来实现不同的效果**，方法单一，post 和 get

​	http ://127.0.0.1/item/queryItem.action?id=1 查询，GET

​	http ://127.0.0.1/item/saveItem.action 新增，POST

​	http ://127.0.0.1/item/updateItem.action 更新，POST

​	http ://127.0.0.1/item/deleteItem.action?id=1 删除，GET或POST

**使用 RESTful 操作资源** ：可以通过相同的请求方式来实现不同的效果，如下：请求地址一样，但是功能可以不同

​	http ://127.0.0.1/item/1 查询，GET

​	http ://127.0.0.1/item 新增，POST

​	http ://127.0.0.1/item 更新，PUT

​	http ://127.0.0.1/item/1 删除，DELETE

访问路径的变化如下：

```
原来的：http://localhost:8080/add?a=1&b=2
Restful：http://localhost:8080/add/a/b
```

### 8.3 结合 SpringMVC 的使用

#### @PathVariable 及 @RequestMapping

1. 新建一个 Controller 类

2. 在 Spring MVC 中可以使用  **@PathVariable 注解**，让**方法参数的值对应绑定到一个 URI 模板变量上**。

   如下所示，@RequestMapping("/add") 需要写为 **@RequestMapping("/add/{a}/{b}")** 

   ```java
   @Controller
   public class RestfulController {
     
       // Restful：http://localhost:8080/add/a/b
       @RequestMapping("/add/{a}/{b}")
       public String test1(@PathVariable int a, @PathVariable int b, Model model){
           int res = a + b;
           model.addAttribute("msg","结果为：" + res);
           return "test";
       }
   
        // 原来的：http://localhost:8080/add?a=1&b=2
   //    @RequestMapping("/add")
   //    public String test1(int a, int b, Model model){
   //        int res = a + b;
   //        model.addAttribute("msg","结果为：" + res);
   //        return "test";
   //    }
   }
   ```

3. 测试及结果

   - 地址栏输入： localhost:8080/add/1/2
   - test.jsp 页面显示： 结果为：3

   **使用路径变量的好处：**

   - 使路径变得更加**简洁**；
   - **获得参数更加方便**，框架会自动进行类型转换；
   - **通过路径变量的类型可以约束访问参数**，如果类型不一样，则访问不到对应的请求方法，如果这里访问的路径是 /add/1/a，则路径与方法不匹配，而不会是参数转换失败。

4. 修改对应的参数类型，将参数 b 的类型 int 修改为 String

   ```java
   @Controller
   public class RestfulController {
   
       @RequestMapping("/add/{a}/{b}")
       public String test1(@PathVariable int a, @PathVariable String b, Model model){
           String res = a + b;
           model.addAttribute("msg","结果为：" + res);
           return "test";
       }
   }
   ```

5. 测试及结果

   - 地址栏输入： localhost:8080/add/1/a
   - test.jsp 页面显示： 结果为：1a

#### @RequestMapping 的 method 属性

Spring MVC 的 @RequestMapping 注解能够处理 HTTP 请求的方法，比如 GET、PUT、POST、DELETE、 PATCH。

可以**使用 method 属性指定请求类型：用于约束请求的类型，可以收窄请求范围**，指定请求的类型如 GET、POST、HEAD、OPTIONS、PUT、PATCH、DELETE、TRACE 等。 

1. @RequestMapping 中添加一个 method 属性

	```java
// 映射访问路径,必须是 POST 请求
@RequestMapping(value = "/add/{a}/{b}", method = RequestMethod.POST)
//@PostMapping("/add/{a}/{b}") // 组合注解，和上面的注解作用相同
public String test1(@PathVariable int a, @PathVariable int b, Model model){
    	int res = a + b;
    	model.addAttribute("msg","结果1为：" + res);
    	return "test";
}
	```

2. 使用浏览器地址栏进行访问默认是 Get 请求，所以会报错 405，如果将 POST 修改为 GET 则正常。

	```java
// 映射访问路径,必须是 get 请求
@RequestMapping(value = "/add/{a}/{b}", method = RequestMethod.GET)
//@GetMapping("/add/{a}/{b}")  // 组合注解，和上面的注解作用相同
   public String test1(@PathVariable int a, @PathVariable int b, Model model){
   	int res = a + b;
   	model.addAttribute("msg","结果1为：" + res);
	return "test";
	}
	```

**注意：所有的地址栏请求默认都是 HTTP GET 类型的。**

**组合注解**（方法级别的注解变体）：

它所扮演的是 @RequestMapping(method =RequestMethod.XXX) 的一个快捷方式，有以下几种：

```
@GetMapping			等价于 @RequestMapping(method =RequestMethod.GET)
@PostMapping
@PutMapping
@DeleteMapping
@PatchMapping
```



## 9、JSON

### 9.1 JSON 简介

- JSON（JavaScript Object Notation，JS 对象标记）是一种轻量级的数据交换格式，目前使用特别广泛。
- 采用**完全独立于编程语言的文本格式来存储和表示数据**。
- 简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。
- 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

### 9.2 JSON 与 JavaScript

在 JavaScript 语言中，一切都是对象。因此，**任何 JavaScript 支持的类型都可以通过 JSON 来表示**，例如字符串、数字、对象、数组等。

**JSON 是 JavaScript 对象的字符串表示法**，它使用文本表示一个 JS 对象的信息，本质是一个字符串。

```
var obj = {a: 'Hello', b: 'World'}; //这是一个对象，注意键名也是可以使用引号包裹的
var json = '{"a": "Hello", "b": "World"}'; //这是一个 JSON 字符串，本质是一个字符串
```

要求和语法格式：

- **对象表示为键值对，数据由逗号分隔**
- **花括号保存对象**
- **方括号保存数组**

JSON 键值对是用来保存 JavaScript 对象的一种方式，和 JavaScript 对象的写法也大同小异，键/值对组合中的**键名写在前面并用双引号 "" 包裹**，使用**冒号 : 分隔**，然后紧接着值：

```
{"name": "张三"}
{"age": "3"}
{"sex": "男"}
```

JSON 和  JavaScript 对象互转：

- JSON 字符串转换为 JavaScript 对象，使用 ` JSON.parse() ` 方法：

- JavaScript 对象转换为 JSON 字符串，使用 ` JSON.stringify()`  方法：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        // 编写一个 JavaScript 对象
        var user = {
            name:"张三",
            age:3,
            sex:"男"
        };
        //将js对象转换成json字符串
        var str = JSON.stringify(user);
        console.log(str);
        //将json字符串转换为js对象
        var user2 = JSON.parse(str);
        console.log(user2)
    </script>
</head>
<body>
</body>
</html>
```

浏览器的控制台输出为：

![image-20200625144016693](SpringMVC/pictures/image-20200625144016693.png)



### 9.3  Jackson——Controller 返回 JSON 数据

Jackson 应该是目前比较好的 **json 解析工具**，当然工具不止这一个，比如还有阿里巴巴的 Fastjson 等。

#### 测试一：输出对象

1. 这里使用 Jackson，使用它需要导入它的jar包

   ```xml
   <dependency>
       <groupId>com.fasterxml.jackson.core</groupId>
       <artifactId>jackson-databind</artifactId>
       <version>2.10.0</version>
   </dependency>
   ```

2. web.xml 配置 servlet 和 乱码过滤

3. springmvc-servlet.xml 配置视图解析器和开启自动扫描包

4. 实体类 User 

   ```java
   @Data 	//这里使用了 Lombok，需要先导入 Lombok 的依赖
   @AllArgsConstructor
   @NoArgsConstructor
   public class User {
       private String name;
       private int age;
       private String sex;
   }
   ```

5. Controller 类，需要使用一个注解 **@RestController** 和一个 **ObjectMapper** 对象

   - 被 @RestController 注解的类和方法，**不会走视图解析器**，会**直接在页面输出我们返回的结果**；

   - **ObjectMapper 会创建一个 Jackson 的对象映射器，用来解析数据**。

   ```java
   @Controller    // 会走视图解析器
   //@RestController //里面所有的方法都只会返回 json 字符串
   public class UserControlller {
   
       //produces:指定响应体返回类型和编码
       //@RequestMapping(value = "/j1",produces = "application/json;charset=utf-8")
       @RequestMapping("/j1")
       @ResponseBody //它就不会走视图解析器，会直接返回一个字符串，配合@Controller使用
       public String json1() throws JsonProcessingException {
   
           // jackson  ObjectMapper
           //创建一个jackson的对象映射器，用来解析数据
           ObjectMapper mapper = new ObjectMapper();
           //创建一个对象
           User user = new User("张三", 3, "男");
           //将我们的对象解析成为json格式
           String str = mapper.writeValueAsString(user);
           //由于 @ResponseBody 注解，这里会将str转成json格式返回；十分方便
           return str;
       }
   ```

   **注意**：在**类上**直接使用 @RestController，这样里面所有的方法都只会返回 json 字符串了，不用再每一个都添加 **@ResponseBody**，在前后端分离开发中，一般都使用 @RestController ，十分便捷。

6. 配置Tomcat ， 启动测试，浏览器地址栏输入  http ://localhost:8080/j1，输出出现乱码问题

7. 乱码解决

   - 通过 @RequestMaping 的 **produces** 属性来实现

   ```java
   //produces:指定响应体返回类型和编码
   @RequestMapping(value = "/j1",produces = "application/json;charset=utf-8")
   ```

   - **乱码的统一解决方法：**

   前面解决乱码的方法比较麻烦，如果项目中有许多请求则每一个都要添加 produces属性，可以通过 **Spring 配置统一指定**，这样就不用每次都去处理。

   可以在 **springmvc 的配置文件上添加**一段消息 StringHttpMessageConverter 转换配置：

   ```xml
   <!--json乱码问题配置-->
   <mvc:annotation-driven>
       <mvc:message-converters register-defaults="true">
           <bean class="org.springframework.http.converter.StringHttpMessageConverter">
               <constructor-arg value="UTF-8"/>
           </bean>
           <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
               <property name="objectMapper">
                   <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                       <property name="failOnEmptyBeans" value="false"/>
                   </bean>
               </property>
           </bean>
       </mvc:message-converters>
   </mvc:annotation-driven>
   ```


#### 测试二：输出集合

1. 在 UserControlller 类中增加一个新的方法

   ```java
   @RequestMapping("/j2")
   public String json2() throws JsonProcessingException {
   
       //创建一个jackson的对象映射器，用来解析数据
       ObjectMapper mapper = new ObjectMapper();
       //创建一个对象
       User user1 = new User("张三1号", 3, "男");
       User user2 = new User("张三2号", 3, "男");
       User user3 = new User("张三3号", 3, "男");
       User user4 = new User("张三4号", 3, "男");
       List<User> list = new ArrayList<User>();
       list.add(user1);
       list.add(user2);
       list.add(user3);
       list.add(user4);
   
       //将我们的对象解析成为json格式
       String str = mapper.writeValueAsString(list);
       return str;
   }
   ```

2. 运行，成功输出结果

   ![image-20200625151145064](SpringMVC/pictures/image-20200625151145064.png)

#### 测试三：输出时间对象

1. 在 UserController 类中增加一个新的方法

   ```java
   @RequestMapping("/j3")
   public String json3() throws JsonProcessingException {
       ObjectMapper mapper = new ObjectMapper()；
       //创建时间一个对象，java.util.Date
       Date date = new Date();
       //将我们的对象解析成为json格式
       String str = mapper.writeValueAsString(date);
       return str;
   }
   ```

2. 运行：结果为一串数字

   ObjectMapper 将**时间解析后的默认格式为：Timestamp 时间戳**，即**默认日期格式会变成一个数字**，是1970年1月1日到当前日期的**毫秒数**。

3. 解决方法

   - 使用 java 中的日期转换方式

   ```java
   @RequestMapping("/j3") 
   public String json3() throws JsonProcessingException {
        ObjectMapper mapper = new ObjectMapper();
   
        // 自定义日期格式
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
   
        //创建时间一个对象，java.util.Date
        Date date = new Date();
        return mapper.writeValueAsString(date);
       
     // ObjectMapper 时间解析后的默认格式为：Timestamp 时间戳
        return mapper.writeValueAsString(sdf.format(date));
 }
   ```

   - 使用 ObjectMapper 对象来格式化输出：**取消timestamps形式 ， 自定义时间格式**
   
   ```java
   @RequestMapping("/j3")
   public String json3() throws JsonProcessingException {
       ObjectMapper mapper = new ObjectMapper();
   
       // 使用ObjectMapper来格式化输出，不使用时间戳的方式
       mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
       // 自定义日期格式对象
       SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
       // 指定日期格式
       mapper.setDateFormat(sdf);
   
       // 创建时间一个对象，java.util.Date
       Date date = new Date();
       return mapper.writeValueAsString(date);
   
   }
   ```

#### 抽取为工具类

可以将之前重复写代码封装到一个工具类中，以后使用时会更加简洁

```java
public class JsonUtils {

    public static String getJson(Object object){
        return getJson(object, "yyyy-MM-dd HH:mm:ss");
    }

    public static String getJson(Object object, String dateFormat) {
        ObjectMapper mapper = new ObjectMapper();
        //不使用时间戳的方式
        mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
        //自定义日期格式对象
        SimpleDateFormat sdf = new SimpleDateFormat(dateFormat);
        mapper.setDateFormat(sdf);
        try {
            return mapper.writeValueAsString(object);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

使用：

```java
@RequestMapping("/j4")
public String json4() throws JsonProcessingException {
    Date date = new Date();
	// 使用工具类
    return JsonUtils.getJson(date);
}
```

### 9.4 FastJson

Fastjson 是阿里开发的一款专门用于 Java 开发的包，可以方便的实现 **json 对象与 JavaBean 对象的转换**，实现 JavaBean 对象与 json 字符串的转换，实现 **json 对象与 json 字符串**的转换。实现 json 的转换方法很多，最后的实现结果都是一样的。

使用前需要先导入 Fastjson 的 pom依赖：

```xml
<dependency>
   <groupId>com.alibaba</groupId>
   <artifactId>fastjson</artifactId>
   <version>1.2.60</version>
</dependency>
```

Fastjson 三个主要的类：

- JSONObject  代表 json 对象 

- JSONArray  代表 json 对象数组

- JSON 代表  JSONObject 和 JSONArray 的转化

Fastjson 的使用：

```java
@RequestMapping("/j5")
public String json5() throws JsonProcessingException {

    //创建一个对象
    User user1 = new User("张三1号", 3, "男");
    User user2 = new User("张三2号", 3, "男");
    User user3 = new User("张三3号", 3, "男");
    User user4 = new User("张三4号", 3, "男");
    List<User> list = new ArrayList<User>();
    list.add(user1);
    list.add(user2);
    list.add(user3);
    list.add(user4);

    System.out.println("*******Java对象 转 JSON字符串*******");
    String str1 = JSON.toJSONString(list);
    System.out.println("JSON.toJSONString(list)==>"+str1);
    String str2 = JSON.toJSONString(user1);
    System.out.println("JSON.toJSONString(user1)==>"+str2);

    System.out.println("\n****** JSON字符串 转 Java对象*******");
    User jp_user1=JSON.parseObject(str2,User.class);
    System.out.println("JSON.parseObject(str2,User.class)==>"+jp_user1);

    System.out.println("\n****** Java对象 转 JSON对象 ******");
    JSONObject jsonObject1 = (JSONObject) JSON.toJSON(user2);
    System.out.println("(JSONObject) JSON.toJSON(user2)==>"+jsonObject1.getString("name"));

    System.out.println("\n****** JSON对象 转 Java对象 ******");
    User to_java_user = JSON.toJavaObject(jsonObject1, User.class);
    System.out.println("JSON.toJavaObject(jsonObject1, User.class)==>"+to_java_user);

    return "hello";
}
```

结果，控制台输出

```
*******Java对象 转 JSON字符串*******
JSON.toJSONString(list)==>[{"age":3,"name":"张三1号","sex":"男"},{"age":3,"name":"张三2号","sex":"男"},{"age":3,"name":"张三3号","sex":"男"},{"age":3,"name":"张三4号","sex":"男"}]
JSON.toJSONString(user1)==>{"age":3,"name":"张三1号","sex":"男"}

****** JSON字符串 转 Java对象*******
JSON.parseObject(str2,User.class)==>User(name=张三1号, age=3, sex=男)

****** Java对象 转 JSON对象 ******
(JSONObject) JSON.toJSON(user2)==>张三2号

****** JSON对象 转 Java对象 ******
JSON.toJavaObject(jsonObject1, User.class)==>User(name=张三2号, age=3, sex=男)
```



## 10、Ajax

### 10.1 简介

- **AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。**
- AJAX 是一种在**无需重新加载整个网页的情况下，能够更新部分网页的技术**。
- AJAX 不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的 Web 应用程序的技术。

- 传统的网页（即不用 AJAX 技术的网页），想要更新内容或者提交一个表单，都需要重新加载整个网页。使用 AJAX 技术的网页，通过**在后台服务器进行少量的数据交换，就可以实现异步局部更新**。
- 使用 AJAX ，用户可以创建接近本地桌面应用的直接、高可用、更丰富、更动态的 Web 用户界面。

在 2005 年，Google 通过其 Google Suggest 使 AJAX 变得流行起来。Google Suggest 能够自动帮你完成搜索单词。Google Suggest 使用 AJAX 创造出动态性极强的 web 界面：当您在谷歌的搜索框输入关键字时，JavaScript 会把这些字符发送到服务器，然后服务器会返回一个搜索建议的列表。和国内百度的搜索框一样。

### 10.2 伪造 Ajax

使用前端的 iframe 标签来伪造一个 Ajax 的样子。

编写一个 html 使用 iframe 测试

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>iframe测试体验页面无刷新</title>

    <script>
        function go() {
            var url = document.getElementById("url").value;
            document.getElementById("iframe1").src=url;
        }
    </script>

</head>
<body>

<div>
    <p>请输入地址：</p>
    <p>
        <input type="text" id="url" value="https://www.baidu.com/">
        <input type="button" value="提交" onclick="go()">
    </p>
</div>

<div>
    <h3>加载页面位置：</h3>
    <iframe id="iframe1" style="width: 100%;height: 500px"></iframe>
</div>

</body>
</html>
```

**利用AJAX可以做：**

- 注册时，输入用户名自动检测用户是否已经存在
- 登陆时，提示用户名密码错误
- 删除数据行时，将行 ID 发送到后台，后台在数据库中删除，数据库删除成功后，在页面 DOM 中将数据行也删除等

### 10.3 jQuery.ajax

纯 JS 原生实现Ajax 是通过 XMLHttpRequest 对象，所以 Ajax 的核心是 XMLHttpRequest 对象（XHR）。XHR 为向服务器发送请求和解析服务器响应提供了接口，能够以异步方式从服务器获取新数据。

jQuery 提供多个与 AJAX 有关的方法。通过 jQuery AJAX 方法，能够使用 HTTP Get 和 HTTP Post 从远程服务器上请求文本、HTML、XML 或 JSON，同时能够把这些外部数据直接载入网页的被选元素中。

jQuery Ajax 本质就是 XMLHttpRequest，对它进行了封装，方便调用。

```
jQuery.ajax(...)
      部分参数：
            url：请求地址
            type：请求方式，GET、POST（1.9.0之后用method）
        headers：请求头
            data：要发送的数据
    contentType：即将发送信息至服务器的内容编码类型(默认: "application/x-www-form-urlencoded; charset=UTF-8")
          async：是否异步
        timeout：设置请求超时时间（毫秒）
      beforeSend：发送请求前执行的函数(全局)
        complete：完成之后执行的回调函数(全局)
        success：成功之后执行的回调函数(全局)
          error：失败之后执行的回调函数(全局)
        accepts：通过请求头发送给服务器，告诉服务器当前客户端可接受的数据类型
        dataType：将服务器端返回的数据转换成指定类型
          "xml": 将服务器端返回的内容转换成xml格式
          "text": 将服务器端返回的内容转换成普通文本格式
          "html": 将服务器端返回的内容转换成普通文本格式，在插入DOM中时，如果包含JavaScript标签，则会尝试去执行。
        "script": 尝试将返回值当作JavaScript去执行，然后再将服务器端返回的内容转换成普通文本格式
          "json": 将服务器端返回的内容转换成相应的JavaScript对象
        "jsonp": JSONP 格式使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数
```

#### 使用原始的 HttpServletResponse 处理

1. 配置 web.xml 和 springmvc 的配置文件，复制上面案例的即可 【记得静态资源过滤和注解驱动配置上】

2. 编写一个 AjaxController

   ```java
   @RestController
   public class AjaxController {
   
       @RequestMapping("/a1")
       public void a1(String name, HttpServletResponse response) throws IOException {
           System.out.println("a1:param= " + name);
           if("zhangsan".equals(name)){
               response.getWriter().print("true");
           } else {
               response.getWriter().print("false");
           }
   
       }
   }
   ```

3. 导入 jquery ， 可以使用在线的 CDN ， 也可以下载导入

   ```jsp
   <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
   <script src="${pageContext.request.contextPath}/statics/js/jquery-3.5.1.min.js"></script>
   ```

4. 编写 index.jsp 测试

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: Administrator
     Date: 2020/6/26
     Time: 8:46
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
     <head>
       <title>$Title$</title>
   
       <script src="${pageContext.request.contextPath}/statics/js/jquery-3.5.1.js"></script>
       <script>
         function a() {
             $.post({
                 url:"${pageContext.request.contextPath}/a1",
                 data:{"name":$("#username").val()},
                 success:function (data,status) {
                     console.log("data=" + data);
                     console.log("status=" + status);
                 }
             })
         }
       </script>
   
     </head>
     <body>
   
     <%--失去焦点的时候，发起一个请求(携带信息)到后台--%>
     <input type="text" id="username" onblur="a()">
     
     </body>
   </html>
   ```

5. 启动 tomcat 测试

   打开浏览器的控制台，当我们的鼠标离开输入框的时候，可以看到发出了一个ajax的请求，是后台返回给我们的结果，测试成功。

#### 使用 Spring MVC 实现

1. 实体类 User

   ```java
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class User {
   
       private String name;
       private int age;
       private String sex;
   }
   ```

2. 获取一个集合对象，展示到前端页面

   ```java
   @RestController
   public class AjaxController {
   
       @RequestMapping("/a2")
       public List<User> a2(){
           List<User> userList = new ArrayList<User>();
           //添加数据
           userList.add(new User("张三1",1,"男"));
           userList.add(new User("张三2",2,"男"));
           userList.add(new User("张三3",3,"男"));
           return userList;
       }
   }
   ```

3. 前端页面 test2.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   
       <script src="${pageContext.request.contextPath}/statics/js/jquery-3.5.1.js"></script>
   
       <script>
           $(function () {
               $("#btn").click(function () {
                   //$.post(url, param[可以省略],success)
                   $.post("${pageContext.request.contextPath}/a2",function (data) {
                       //console.log(data);
                       var html="";
                       for (let i = 0; i < data.length; i++){
                           html += "<tr>" +
                               "<td>" + data[i].name +"</td>"+
                               "<td>" + data[i].age +"</td>"+
                               "<td>" + data[i].sex +"</td>"+
                               "</tr>"
                       }
                       $("#content").html(html);
                   });
               })
           });
       </script>
   
   </head>
   <body>
       
   <input type="button" value="加载数据" id="btn">
   <table>
       <tr>
           <td>姓名</td>
           <td>年龄</td>
           <td>性别</td>
       </tr>
       <tbody id="content">
           <%--数据，后台--%>
       </tbody>
   </table>
   
   </body>
   </html>
   ```

4. 测试，成功实现了数据回显！

### 10.4 案例：注册提示

1. Controller

   ```java
   @RestController
   public class AjaxController {
       
       @RequestMapping("/a3")
       public String a3(String name, String pwd){
           String msg = "";
           if (name != null){
               // admin 这些数据应该在数据库中查询
               if ("admin".equals(name)){
                   msg = "ok";
               } else {
                   msg = "用户名有误";
               }
           }
           if(pwd != null){
               if ("123456".equals(pwd)){
                   msg = "ok";
               } else {
                   msg = "密码有误";
               }
           }
           return msg;
       }
   }
   ```

2. 前端页面 login.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
       
       <script src="${pageContext.request.contextPath}/statics/js/jquery-3.5.1.js"></script>
   
       <script>
           function a1() {
               $.post({
                   url:"${pageContext.request.contextPath}/a3",
                   data:{"name":$("#name").val()},
                   success:function (data) {
                       // console.log(data);
                       if (data.toString() === 'ok'){
                           $("#userInfo").css("color","green");
                       } else{
                           $("#userInfo").css("color","red");
                       }
                       $("#userInfo").html(data);
                   }
               })
           }
           function a2() {
               $.post({
                   url:"${pageContext.request.contextPath}/a3",
                   data:{"pwd":$("#pwd").val()},
                   success:function (data) {
                       // console.log(data);
                       if (data.toString() === 'ok'){
                           $("#pwdInfo").css("color","green");
                       }else{
                           $("#pwdInfo").css("color","red");
                       }
                       $("#pwdInfo").html(data);
                   }
               })
           }
       </script>
       
   </head>
   <body>
   
   <p>
       用户名：<input type="text" id="name" onblur="a1()">
       <span id="userInfo"></span>
   </p>
   <p>
       密码：<input type="text" id="pwd" onblur="a2()">
       <span id="pwdInfo"></span>
   </p>
   
   </body>
   </html>
   ```

3. 测试，动态请求响应，会局部刷新

   注意：要处理 JSON 乱码问题

   

## 11、拦截器

### 11.1 简介

SpringMVC 的处理器拦截器类似于 Servlet 开发中的过滤器 Filter，用于对处理器进行预处理和后处理。开发者可以自己定义一些拦截器来实现特定的功能。

**过滤器与拦截器的区别：**拦截器是AOP思想的具体应用。

**过滤器**

- servlet 规范中的一部分，任何 java web 工程都可以使用
- 在 url-pattern 中配置了 /* 之后，可以对所有要访问的资源进行拦截

**拦截器** 

- 拦截器是 SpringMVC 框架自己的，只有使用了 SpringMVC 框架的工程才能使用
- 拦截器只会拦截访问的控制器方法， 如果访问的是 jsp/html/css/image/js 是不会进行拦截的

### 11.2 自定义拦截器

想要自定义拦截器，必须实现 `HandlerInterceptor` 接口。

1. 新建一个 Moudule，添加web支持

2. 配置 web.xml 和 springmvc-servlet.xml 文件

3. 编写一个拦截器

   ```java
   public class MyInterceptor implements HandlerInterceptor {
   	// 在请求处理的方法之前执行
       // return true 执行下一个拦截器，放行
       // return false 就不执行下一个拦截器
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           System.out.println("处理前");
           return true;
       }
   
       // 在请求处理方法执行之后执行，如日志
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
           System.out.println("处理后");
       }
   	 // 在 dispatcherServlet 处理后执行，做清理工作
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
           System.out.println("清理");
       }
   }
   ```

4. 在 springmvc 的配置文件中配置拦截器

   ```xml
   <!--拦截器-->
   <mvc:interceptors>
       <mvc:interceptor>
           <!--包括这个请求下面的所有请求（路径及其子路径）-->
           <!--/admin/* 拦截的是/admin/add等等这种 , /admin/add/user不会被拦截-->
           <!--/admin/** 拦截的是/admin/下的所有-->
           <mvc:mapping path="/**"/>
           <bean class="com.song.config.MyInterceptor"/>
       </mvc:interceptor>
   </mvc:interceptors>
   ```

5. 编写一个 Controller，接收请求

   ```java
   @RestController
   public class TestController {
   
       @RequestMapping("/t1")
       public String test(){
           System.out.println("TestController的test方法执行了");
           return "OK";
       }
   }
   ```

6. 启动tomcat 测试

   地址栏输入 localhost:8080/t1 ，页面会显示 OK，服务器控制台输出为

   ![image-20200627112001707](SpringMVC/pictures/image-20200627112001707.png)

   

### 11.3 案例：验证用户是否登录

**需求分析：**

【index.jsp】 页面有一个【登录页面】和【首页】的超链接。

要求：未登录时点击【首页】会跳转到【登录页面】进行登录，如果登录过会直接跳到【首页】。

登录成功会将用户名显示在【首页】，并且首页会有一个【注销】按钮，注销之后会返回到【登录页面】。

**思路：**

1、有一个登录页面，需要写一个 controller 访问页面。

2、登录页面有一个提交表单的动作，需要在controller中处理，判断用户名密码是否正确。如果正确，向session 中写入用户信息，返回登录成功。

3、拦截用户请求，判断用户是否登陆。如果用户已经登陆。放行， 如果用户未登陆，跳转到登陆页面。

**测试：**

1. 【index.jsp】 页面

   ```jsp
   <h1><a href="${pageContext.request.contextPath}/user/goLogin">登录页面</a></h1>
   <h1><a href="${pageContext.request.contextPath}/user/main">首页</a></h1>
   ```

2. 登陆页面  【login.jsp】

   ```jsp
   <%--在 WEB-INF 下的页面或者资源，只能通过controller 或者servlet 进行访问--%>
   <h1>登录页面</h1>
   
   <form action="${pageContext.request.contextPath}/user/login" method="post">
       <%--注意：表单中的数据要想被提交，必须指定其name属性--%>
       用户名：<input type="text" name="username">
       密码：<input type="text" name="password">
       <input type="submit" value="提交">
   </form>
   ```

3. LoginController

   ```java
   @Controller
   @RequestMapping("/user")
   public class LoginController {
   
       @RequestMapping("/main")
       public String main(){
           return "main";
       }
   
       @RequestMapping("/goLogin")
       public String login(){
           return "login";
       }
   
       @RequestMapping("/login")
       public String login(HttpSession session, String username, String password, Model model){
           System.out.println("login=====>" + username);
           // 把用户的信息存在 session中
           session.setAttribute("userLoginInfo",username);
           model.addAttribute("username",username);
           return "main";
       }
   
       @RequestMapping("/goOut")
       public String login(HttpSession session){
           // 把用户的信息存在 session中
           session.removeAttribute("userLoginInfo");
           return "login";
       }
   }
   ```

4. 首页【main.jsp】，也就是登录成功的页面

   ```jsp
   <h1>首页</h1>
   
   <span>${username}</span>
   
   <p>
           <a href="${pageContext.request.contextPath}/user/goOut">注销</a>
   </p>
   ```

5. 在 index.jsp 页面上测试跳转，启动 Tomcat 测试，未登录也可以进入主页！

6. 编写用户登录拦截器

   ```java
   public class LoginInterceptor implements HandlerInterceptor {
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           HttpSession session = request.getSession();
           // 放行：判断什么情况下登录
           // 登录页面放行
           if (request.getRequestURI().contains("goLogin")){
               return true;
           }
           // 说明正在提交登录
           if (request.getRequestURI().contains("login")){
               return true;
           }
   
           // 第一次登录也是没有 session的
           if (session.getAttribute("userLoginInfo") != null){
               return true;
           }
   
           // 判断什么情况下没有登录
           request.getRequestDispatcher("/WEB-INF/jsp/login.jsp").forward(request,response);
           return false;
       }
   }
   ```

7. 在Springmvc的配置文件中注册拦截器

   ```xml
   <!--拦截器-->
   <mvc:interceptors> 
       <mvc:interceptor>
           <mvc:mapping path="/user/**"/>
           <bean class="com.song.config.LoginInterceptor"/>
       </mvc:interceptor>
   </mvc:interceptors>
   ```

8. 重启 Tomcat 测试，登录拦截功能实现。



## 12、文件上传和下载

### 12.1 准备工作

文件上传是项目开发中最常见的功能之一，springMVC 可以很好的支持文件上传，但是SpringMVC上下文中默认没有装配 MultipartResolver，因此默认情况下其不能处理文件上传工作。如果想使用 Spring 的文件上传功能，则需要在上下文中配置 MultipartResolver。

前端表单要求：为了能上传文件，必须将表单的 method 设置为 POST，并将 enctype 设置为 multipart/form-data。只有在这样的情况下，浏览器才会把用户选择的文件以二进制数据发送给服务器。

**对表单中的 enctype 属性做个详细的说明：**

- application/x-www=form-urlencoded：默认方式，只处理表单域中的 value 属性值，采用这种编码方式的表单会将表单域中的值处理成 URL 编码方式。
- multipart/form-data：这种编码方式会以二进制流的方式来处理表单数据，这种编码方式会把文件域指定文件的内容也封装到请求参数中，不会对字符编码。
- text/plain：除了把空格转换为 "+" 号外，其他字符都不做编码处理，这种方式适用直接通过表单发送邮件。

```jsp
<form action="" enctype="multipart/form-data" method="post">
   <input type="file" name="file"/>
   <input type="submit">
</form>
```

一旦设置了 enctype 为 multipart/form-data，浏览器即会采用二进制流的方式来处理表单数据，而对于文件上传的处理则涉及在服务器端解析原始的HTTP响应。在2003年，Apache Software Foundation 发布了开源的 Commons FileUpload 组件，其很快成为 Servlet/JSP 程序员上传文件的最佳选择。

- Servlet3.0 规范已经提供方法来处理文件上传，但这种上传需要在 Servlet 中完成。
- 而 Spring MVC 则提供了更简单的封装。
- Spring MVC 为文件上传提供了直接的支持，这种支持是用即插即用的 MultipartResolver 实现的。
- Spring MVC 使用 Apache Commons FileUpload 技术实现了一个 MultipartResolver 实现类：

  CommonsMultipartResolver。因此，SpringMVC的文件上传还需要依赖 Apache Commons FileUpload 的组件。

### 12.2 文件上传

1. 导入文件上传的jar包，commons-fileupload ， Maven会自动帮我们导入他的依赖包 commons-io包；

   ```xml
   <!--文件上传-->
   <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.3</version>
   </dependency>
   <!--servlet-api导入高版本的-->
   <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
   </dependency>
   ```

2. 配置bean：multipartResolver

   【**注意！！！这个bena的id必须为：multipartResolver ， 否则上传文件会报400的错误！**】

   ```xml
   <!--文件上传配置-->
   <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
      <!-- 请求的编码格式，必须和jSP的pageEncoding属性一致，以便正确读取表单的内容，默认为ISO-8859-1 -->
      <property name="defaultEncoding" value="utf-8"/>
      <!-- 上传文件大小上限，单位为字节（10485760=10M） -->
      <property name="maxUploadSize" value="10485760"/>
      <property name="maxInMemorySize" value="40960"/>
   </bean>
   ```

   CommonsMultipartFile 的 常用方法：

   - **String getOriginalFilename()：获取上传文件的原名**
   - **InputStream getInputStream()：获取文件流**
   - **void transferTo(File dest)：将上传文件保存到一个目录文件中**

3. 编写前端页面

   ```jsp
   <form action="${pageContext.request.contextPath}/upload" enctype="multipart/form-data" method="post">
    	<input type="file" name="file"/>
    	<input type="submit" value="upload">
   </form>
   ```

4. Controller

   ```java
   @Controller
   public class FileController {
      //@RequestParam("file") 将name=file控件得到的文件封装成CommonsMultipartFile 对象
      //批量上传CommonsMultipartFile则为数组即可
      @RequestMapping("/upload")
      public String fileUpload(@RequestParam("file") CommonsMultipartFile file ,HttpServletRequest request) throws IOException {
   
          //获取文件名 : file.getOriginalFilename();
          String uploadFileName = file.getOriginalFilename();
   
          //如果文件名为空，直接回到首页！
          if ("".equals(uploadFileName)){
              return "redirect:/index.jsp";
         }
          System.out.println("上传文件名 : "+uploadFileName);
   
          //上传路径保存设置
          String path = request.getServletContext().getRealPath("/upload");
          //如果路径不存在，创建一个
          File realPath = new File(path);
          if (!realPath.exists()){
              realPath.mkdir();
         }
          System.out.println("上传文件保存地址："+realPath);
   
          InputStream is = file.getInputStream(); //文件输入流
          OutputStream os = new FileOutputStream(new File(realPath,uploadFileName));//文件输出流
   
          //读取写出
          int len=0;
          byte[] buffer = new byte[1024];
          while ((len=is.read(buffer))!=-1){
              os.write(buffer,0,len);
              os.flush();
         }
          os.close();
          is.close();
          return "redirect:/index.jsp";
     }
   }
   ```

5. 测试，上传文件成功

**采用 file.Transto 来保存上传的文件**

1. 编写Controller

   ```java
   /*
   * 采用file.Transto 来保存上传的文件
   */
   @RequestMapping("/upload2")
   public String  fileUpload2(@RequestParam("file") CommonsMultipartFile file,HttpServletRequest request) throws IOException {
   
      //上传路径保存设置
      String path = request.getServletContext().getRealPath("/upload");
      File realPath = new File(path);
      if (!realPath.exists()){
          realPath.mkdir();
     }
      //上传文件地址
      System.out.println("上传文件保存地址："+realPath);
   
      //通过CommonsMultipartFile的方法直接写文件（注意这个时候）
      file.transferTo(new File(realPath +"/"+ file.getOriginalFilename()));
   
      return "redirect:/index.jsp";
   }
   ```

2. 前端表单提交地址修改

3. 访问提交测试，OK！

### 12.3 文件下载

**文件下载步骤：**

1. 设置 response 响应头
2. 读取文件 -- InputStream
3. 写出文件 -- OutputStream
4. 执行操作
5. 关闭流 （先开后关）

**代码实现：**

1. Controller

   ```java
   @RequestMapping(value="/download")
   public String downloads(HttpServletResponse response , HttpServletRequest request) throws Exception{
       //要下载的图片地址
       String  path = request.getServletContext().getRealPath("/statics");
       String  fileName = "1.png";
   
       //1、设置response 响应头
       response.reset(); //设置页面不缓存,清空buffer
       response.setCharacterEncoding("UTF-8"); //字符编码
       response.setContentType("multipart/form-data"); //二进制传输数据
       //设置响应头
       response.setHeader("Content-Disposition",
               "attachment;fileName="+ URLEncoder.encode(fileName, "UTF-8"));
   
       File file = new File(path,fileName);
       //2、 读取文件--输入流
       InputStream input=new FileInputStream(file);
       //3、 写出文件--输出流
       OutputStream out = response.getOutputStream();
   
       byte[] buff =new byte[1024];
       int index=0;
       //4、执行 写出操作
       while((index= input.read(buff))!= -1){
           out.write(buff, 0, index);
           out.flush();
       }
       out.close();
       input.close();
       return null;
   }
   ```

2. 前端

   ```jsp
   --<a href="${pageContext.request.contextPath}/download">点击下载</a>
   ```

3. 测试，文件下载成功

   





























项目的架构是设计好的，还是演进的？答：演进的

所有的项目 All in one --> 微服务，比如阿里巴巴

高难度面试题：

网站是如何进行访问的：

1. 输入一个域名，回车

2. 检查本机的 C:\Windows\System32\drivers\etc\hosts 配置文件下有没有这个域名的映射

   - 有：直接返回对应的 ip 地址，这个地址中有我们需要访问的应用程序，可以直接访问

      127.0.0.1 localhost

   - 没有：去 DNS 服务器（全世界的域名都在这里管理），找到的话就返回，找不到就返回找不到



前后端分离时代：

后端部署后端，提供接口，提供数据



json：数据交换格式



前端独立部署，负责渲染后端的数据