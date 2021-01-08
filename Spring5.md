## 1、Spring 简介

Spring : 春天 ---> 给软件行业带来了春天

Spring 理念 : 使现有技术更加容易使用，本身就是一个大杂烩，整合了现有的框架技术

Spring 是为了解决企业级应用开发的复杂性而创建的，简化开发。

### 1.1 发展

2002 年，Rod Jahnson 首次推出了 Spring 框架雏形 interface21 框架。

2004 年 3 月 24 日，Spring 框架以 interface21 框架为基础，经过重新设计，发布了 1.0 正式版。

很难想象 Rod Johnson 的学历 , 他是悉尼大学的博士，然而他的专业不是计算机，而是音乐学。

官网 : http://spring.io/

官方下载地址 : https://repo.spring.io/libs-release-local/org/springframework/spring/

GitHub : https://github.com/spring-projects

在Maven工程中使用 Spring 需要先导入依赖 jar 包

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.0.RELEASE</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.0.RELEASE</version>
</dependency>
```

### 1.2 优点

- Spring 是一个开源免费的框架（容器） 
- Spring 是一个轻量级的框架，非入侵式的框架
- **控制反转 IoC  , 面向切面编程 AOP**
- 支持事务的处理 , 对框架整合的支持

总结：**Spring 就是一个轻量级的控制反转（IoC）和面向切面编程（AOP）的框架。**

### 1.3 组成（七大模块）

Spring 框架是一个分层架构，由 7 个定义良好的模块组成。Spring 模块构建在核心容器之上，核心容器定义了创建、配置和管理 bean 的方式。

![img](https://images2017.cnblogs.com/blog/1219227/201709/1219227-20170930225010356-45057485.gif)

组成 Spring 框架的每个模块（或组件）都可以单独存在，或者与其他一个或多个模块联合实现。每个模块的功能如下：

- **核心容器**：核心容器提供 Spring 框架的基本功能。**核心容器的主要组件是 BeanFactory，它是工厂模式的实现**。BeanFactory 使用**控制反转（IOC）** 模式将应用程序的配置和依赖性规范与实际的应用程序代码分开。
- **Spring 上下文**：Spring 上下文是一**个配置文件**，向 Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。
- **Spring AOP**：通过配置管理特性，Spring AOP 模块直接**将面向切面的编程功能 , 集成到了 Spring 框架中**。所以，可以很容易地使 Spring 框架管理任何支持 AOP 的对象。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖组件，就可以将声明性事务管理集成到应用程序中。
- **Spring DAO**：JDBC DAO 抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异常代码数量（例如打开和关闭连接）。Spring DAO 的面向 JDBC 的异常遵从通用的 DAO 异常层次结构。
- **Spring ORM**：Spring 框架插入了若干个 ORM 框架，从而提供了 ORM 的对象关系工具，其中包括 JDO、Hibernate 和 iBatis SQL Map。所有这些都遵从 Spring 的通用事务和 DAO 异常层次结构。
- **Spring Web 模块**：**Web 上下文模块建立在应用程序上下文模块之上**，为基于 Web 的应用程序提供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。
- **Spring MVC 框架**：MVC 框架是一个全功能的**构建 Web 应用程序的 MVC 实现**。通过策略接口，MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText 和 POI。

### 1.4 拓展

> Spring 官网介绍：现代 Java 的开发，说白了就是基于 Spring 的开发

构建一切、协调一切、连接一切

#### Spring Boot 和 Spring Cloud

**Spring Boot**

- 是 Spring 的一个快速配置开发的脚手架

- 基于 SpringBoot 可以快速的开发单个微服务

- 约定大于配置，很多集成方案已经帮你选择好了，能不配置就不配置 

**Spring Cloud**

- Spring Cloud 很大的一部分是基于 Spring Boot 来实现，Spring Boot 可以离开 Spring Cloud 独立使用开发项目，但是 Spring Cloud 离不开 Spring Boot，属于依赖的关系

- 关注全局的服务治理框架

- 基于 Spring Boot 实现的，Spring Boot 在 Spring Clound中起到了承上启下的作用，如果要学习 Spring Cloud 必须要学习 Spring Boot。

   现在大多数公司都在使用 SpringBoot 进行快速开发，学习 SpringBoot 前提，需要完全掌握 Spring 及 SpringMVC，承上启下的作用。

**弊端**：发展了太久之后，违背了原来的理念，配置十分繁琐，人称“配置地狱”。



## 2、IoC 理论推导

### 2.1 IoC 思想的理解

#### 传统方法：

0. 新建一个 Maven 工程，用原来的方式写一段代码

1. UserDao 接口

```java
public interface UserDao {
    void getUser();
}
```

2. UserDaoImpl 实现类（有多个 UserDaoMysqlImpl、UserDaoOracleImpl......）

```java
public class UserDaoImpl implements UserDao {
    public void getUser() {
        System.out.println("默认获取用户的数据");
    }
}
```

```java
public class UserDaoMysqlImpl implements UserDao {
    public void getUser() {
        System.out.println("mysql 获取用户的数据");
    }
}
```

```java
public class UserDaoOracleImpl implements UserDao {
    public void getUser() {
        System.out.println("Oracle 获取用户数据");
    }
}
```

3. UserService 业务接口

```java
public interface UserService {
    void getUser();
}
```

4. **UserServiceImpl 业务实现类**

```java
public class UserServiceImpl implements UserService {

    private UserDao userDao = new UserDaoImpl();
//    private UserDao userDao = new UserDaoMysqlImpl();
//    private UserDao userDao = new UserDaoOracleImpl();

    public void getUser() {
        userDao.getUser();
    }
}
```

5. 测试类

```java
public class MyTest {
    public static void main(String[] args) {
        // 用户实际调用的是业务层，dao 层他们不需要接触
        UserService userService = new UserServiceImpl();
        userService.getUser();
    }
}
```

6. 调试
   - 如果使用实现类 `UserDaoImpl`，在业务层调用 `private UserDao userDao = new UserDaoImpl();`
   - 如果要使用 `UserDaoMysqlImpl`，需要在业务层修改调用，变为 `private UserDao userDao = new UserDaoMysqlImpl();`
   - 同理，如果要使用`UserDaoOracleImpl`，则将业务层的调用修改为`private UserDao userDao = new UserDaoOracleImpl();`

7. 总结

   在这个业务操作中中，用户的需求可能会影响我们原来的代码，我们**需要根据用户的需求去修改源代码（业务层代码）**，如果程序代码量十分大，修改一次的成本代价十分昂贵。

#### 改进：

1. 在 UserServiceImpl 业务实现类中，**使用一个 set 方法传入一个实现类对应的接口，在客户端去传入具体的实现类**，这已经发生了革命性变化

```java
public class UserServiceImpl implements UserService {

    private UserDao userDao;

    // 利用 set 进行动态实现值的注入
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
    
    public void getUser() {
        userDao.getUser();
    }
}
```

2. 测试类：

```java
public class MyTest {
    public static void main(String[] args) {
        // 用户实际调用的是业务层，dao 层他们不需要接触
        UserService userService = new UserServiceImpl();

//        ((UserServiceImpl) userService).setUserDao(new UserDaoImpl());
        ((UserServiceImpl) userService).setUserDao(new UserDaoMysqlImpl());
//        ((UserServiceImpl) userService).setUserDao(new UserDaoOracleImpl());
        userService.getUser();
    }
}
```

3. 调试

   - 如果使用实现类 `UserDaoImpl`，在客户端调用 set 方法传入接口的实现类对象 `new UserDaoImpl()`
   - 如果要使用 `UserDaoMysqlImpl`，在客户端调用 set 方法传入 `new UserDaoMysqlImpl()`
   - 同理，如果要使用`UserDaoOracleImpl`，在客户端调用 set 方法传入`new UserDaoOracleImpl()`

4. 总结

   **在需要用到某个实现类的地方，我们不直接去实现它，而是留出一个接口，利用 set() 方法，让客户去修改**。即客户需求改变时，不用去修改源代码（不操作业务层），只需要在客户端传入不同的接口实现类对象即可。  

#### 对比分析：

- 之前，程序是主动创建对象，控制权在程序员手上

- 使用了 set 注入后，程序不再具有主动性，而是变成了被动的接收对象，程序只负责提供一个接口

  这种思想，从本质上解决了问题，**程序员不用再去管理对象的创建，系统耦合性大大降低，可以更加专注于业务的实现上，这是 IOC 原型**。

![ioc2](Spring5/pictures/ioc2.png)

### 2.2 IoC 本质

**控制反转 IoC（Inversion of Control），是一种设计思想，DI（依赖注入）是实现 IoC 的一种方法**，也有人认为 DI 只是 IoC 的另一种说法。没有 IoC 的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，**控制反转后将对象的创建转移给第三方**，个人认为所谓控制反转就是：**获得依赖对象的方式反转了**。

**IoC 是 Spring 框架的核心内容**，使用多种方式完美的实现了 IoC，**可以使用 XML 配置，也可以使用注解，新版本的 Spring 也可以零配置实现 IoC**。

**Spring 容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从  Ioc 容器中取出需要的对象。**

![img](https://docs.spring.io/spring/docs/5.2.7.RELEASE/spring-framework-reference/images/container-magic.png)

- 采用 **XML 方式配置 Bean 的时候，Bean 的定义信息是和实现分离的**； 

- 而采用注解的方式可以把两者合为一体，**Bean 的定义信息直接以注解的形式定义在实现类中，从而达到了零配置**的目的。

**控制反转是一种通过描述（XML 或注解）并通过第三方去生产或获取特定对象的方式。在 Spring 中实现控制反转的是 IoC 容器，其实现方法是依赖注入（Dependency Injection, DI）。**

![ioc1](Spring5/pictures/ioc1.png)

## 3、Spring 入门程序

> 官网参考资料：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">  
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->

</beans>
```

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
```

### 3.1 Hello Stirng 案例

先导入依赖 jar 包，再编写代码

1. Hello 实体类

   ```java
   public class Hello {
       private String str;
       public String getStr() {
           return str;
       }
       public void setStr(String str) {
           this.str = str;
       }
       @Override
       public String toString() {
           return "Hello{" +
                   "str='" + str + '\'' +
                   '}';
       }
   }
   ```
2. Spring 配置文件（头部信息参考官方文档），这里命名为 beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--使用 Spring 创建对象，在 Spring 中这些都称为 Bean
           类型  变量名 = new  类型();
           Hello hello = new Hello();
   
           id = 变量名
           class = new 的对象
           property 相当于给对象中的属性设置一个值
       -->
       <bean id="hello" class="com.song.pojo.Hello">
           <property name="str" value="Spring"/>
       </bean>
   
   </beans>
   ```

3. 测试类

   ```java
   public class MyTest {
       public static void main(String[] args) {
           // 获取 Spring 的上下文对象
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
           // 我们的对象现在都在 Spring 中管理了，如果要使用，直接取出来就可以
           Hello hello = (Hello) context.getBean("hello");
           System.out.println(hello.toString());
       }
   }
   ```

**总结：**

- Hello 对象是谁创建的 ?  【hello 对象是由 Spring 创建的】
- Hello 对象的属性是怎么设置的 ? 【hello 对象的属性是由 Spring 容器设置的】

这个过程就叫控制反转（IoC） :

- 控制 : 谁来控制对象的创建 , 传统应用程序的对象是由程序本身控制创建的 , 使用 Spring 后 , 对象是**由 Spring 来创建的**
- 反转 : 程序本身不创建对象 , 而变成**被动的接收对象**
-  IOC 是一种编程思想，由**主动的编程变成被动的接收**。

**依赖注入** : 就是利用 **set() 方法来进行注入**的。

### 3.2 案例修改

对 2.1 中的案例进行修改，使用 Spring 容器来管理对象。

新增一个 Spring 配置文件 beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="mysqlImpl" class="com.song.dao.UserDaoMysqlImpl"/>
    <bean id="oracleImpl" class="com.song.dao.UserDaoOracleImpl"/>

    <bean id="userServiceImpl" class="com.song.service.UserServiceImpl">
        <!--<property name="userDao" ref="mysqlImpl"/>-->
        <property name="userDao" ref="oracleImpl"/>
    </bean>
    <!--
        ref：引用 Spring 容器中创建好的配置文件
        value：具体的值，基本数据类型
    -->
</beans>
```

测试类：

```java
public class MyTest1 {
    public static void main(String[] args) {
        // 获取 ApplicationContext：拿到 Spring 容器
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        // 需要什么对象，直接 get
        UserServiceImpl userServiceImpl = (UserServiceImpl)context.getBean("userServiceImpl");

        userServiceImpl.getUser();
    }
}
```

要实现不同的操作，我们彻底不用在程序中去改动，只需要在 xml 配置文件中进行修改。

所谓 IoC：对象由 **Spring 来创建、管理、装配**。



## 4、IoC 创建对象的方式

### 4.1 通过无参构造方法创建对象【默认方式】

实体类

```java
public class User {
   private String name;
    
   public User() {
       System.out.println("user无参构造方法");
  }
    
   public void setName(String name) {
       this.name = name;
  }
   public void show(){
       System.out.println("name="+ name );
  }
}
```

Spring 的配置文件 beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

   <bean id="user" class="com.song.pojo.User">
       <property name="name" value="张三"/>
   </bean>
    
</beans>
```
测试类
```java
@Test
public void test(){
   ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
   //在执行getBean的时候，user已经创建好了，通过无参构造
   User user = (User) context.getBean("user");
   //调用对象的方法
   user.show();
}
```

结果可以发现，在调用 show 方法之前，User 对象已经通过无参构造初始化了。

### 4.2 通过有参构造方法创建对象

> 官方文档：Constructor-based Dependency Injection

java 实体类（只有一个有参构造）：

```java
public class User {
    private String name;

    public User(String name){
        this.name = name;
    }

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public void show(){
        System.out.println("name = " + name);
    }
}
```

1. 下标赋值

   ```xml
   <!--第一种，下标赋值-->
   <bean id="user" class="com.song.pojo.User">
       <!-- index指构造方法 , 下标从0开始 -->
       <constructor-arg index="0" value="张三"/>
   </bean>
   ```

2. 类型赋值【不建议使用，因为参数类型有可能一样】

   ```xml
   <!--第二种：通过类型创建【不建议使用】-->
   <bean id="user" class="com.song.pojo.User">
       <constructor-arg type="java.lang.String" value="李四"/>
   </bean>
   ```

3. 参数名赋值

   ```xml
   <!--第三种，直接通过参数名设置-->
   <bean id="user" class="com.song.pojo.User">
       <!-- name指参数名 -->
       <constructor-arg name="name" value="王五"/>
   </bean>
   ```

#### 扩展：

如果再创建一个实体类 UserT：

```java
public class UserT {
    private String name;

    public UserT(){
        System.out.println("UserT 被创建了");
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```

对应的 bean.xml 文件为：

```xml
<bean id="user" class="com.song.pojo.User">
    <constructor-arg name="name" value="王五"/>
</bean>

<bean id="userT" class="com.song.pojo.UserT">
</bean>
```

测试类：

```java
public class MyTest {
    public static void main(String[] args) {
		// Spring 容器
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        User user = (User)context.getBean("user");
        User user2 = (User)context.getBean("user");
        System.out.println(user == user2);
    }
}
```

输出结果：

```java
UserT 被创建了
true
```

**总结**：在**配置文件加载时，容器中管理的对象就已经初始化了。并且容器内一个类的实例对象只有一个（单例）**。



## 5、Spring 配置说明

### 5.1 别名

alias 设置别名 , 为 bean 设置别名 , 可以设置多个别名

```xml
<!--别名，如果添加了别名，我们也可以使用别名获取到这个对象-->
<alias name="user" alias="userNew"/>
```

### 5.2 bean 的配置

```xml
<!--
    id：bean 的唯一标识符，相当于对象名
    class：bean 对象所对应的全限定名：包名 + 类名
    name：也是别名，而且可以同时取多个别名，分隔符也很多
-->
<bean id="userT" class="com.song.pojo.UserT" name="user2 u2,u3;u4">
    <property name="name" value="张三"/>
</bean>
```

### 5.3 import

import 一般用于团队开发使用，可以将多个配置文件导入合并为一个。

假设一个项目中有多个人开发，每个人负责不同的类开发，不同的类需要注册在不同的 bean 中（beans.xml、beans2.xml、beans3.xml……），可以利用 import 将所有人的 beans.xml 合并为一个总的 ，使用的时候直接使用总的配置即可。

在总配置 applicationContext.xml 中：

```xml
<import resource="beans.xml"/>
<import resource="beans2.xml"/>
<import resource="beans3.xml"/>
```



## 6、依赖注入（DI）

依赖注入（Dependency Injection，DI）

- 依赖 : 指 bean 对象的**创建依赖于容器**，bean 对象的依赖资源。
- 注入 : 指 bean 对象中的**所有属性由容器来注入**

### 6.1 构造器注入

之前的案例已经使用

### 6.2 set 方式注入【重点】

#### 6.2.0 环境搭建

1. 复杂类型

   ```java
   public class Address {
       private String address;
   
       getXxx()/setXxx()/toString()
   }
   ```

2. 真实测试对象

   ```java
   public class Student {
   
       private String name;
       private Address address;
       private String[] books;
       private List<String> hobbies;
       private Map<String,String> card;
       private Set<String> games;
       private String wife;
       private Properties info;
       
       getXxx()/setXxx()/toString()
   }
   ```

3. beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="student" class="com.song.pojo.Student">
           <!--第一种：普通值注入，value -->
           <property name="name" value="张三"/>
       </bean>
   
   </beans>
   ```

4. 测试类

   ```java
   public class MyTest {
   	public static void main(String[] args) {
       	ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
       	Student student = (Student)context.getBean("student");
       	System.out.println(student.getName());
   	}
   }
   ```


#### 6.2.1 普通值（常量）注入

```xml
<bean id="student" class="com.song.pojo.Student">
    <!--普通值注入，value -->
    <property name="name" value="张三"/>
</bean>
```

#### 6.2.2 Bean 注入，引用

```xml
<bean id="address" class="com.song.pojo.Address">
    <property name="address" value="北京"/>
</bean>

<bean id="student" class="com.song.pojo.Student">
    <!--Bean 注入，ref-->
    <property name="address" ref="address"/>
</bean>
```

#### 6.2.3 数组注入

```xml
<!--数组注入-->
<property name="books">
    <array>
        <value>红楼梦</value>
        <value>西游记</value>
        <value>水浒传</value>
        <value>三国演义</value>
    </array>
</property>
```

#### 6.2.4 list 注入

```xml
<!-- list 注入-->
<property name="hobbies">
    <list>
        <value>打球</value>
        <value>唱歌</value>
        <value>跳舞</value>
    </list>
</property>
```

#### 6.2.5 map 注入

```xml
<!-- map 注入-->
<property name="card">
    <map>
        <entry key="身份证" value="123456"/>
        <entry key="银行卡" value="456789"/>
    </map>
</property>
```

#### 6.2.6 set 注入

```xml
<!-- set 注入-->
<property name="games">
    <set>
        <value>LOL</value>
        <value>COC</value>
        <value>BOB</value>
    </set>
</property>
```

#### 6.2.7 null 注入

```xml
<!-- null 注入-->
<!--<property name="wife" value=""/>-->
<property name="wife">
    <null/>
</property>
```

#### 6.2.8 properties 注入

```xml
<!-- properties 注入-->
<property name="info">
    <props>
        <prop key="driver">2018020202</prop>
        <prop key="url">男</prop>
        <prop key="username">张三</prop>
        <prop key="password">123456</prop>
    </props>
</property>
```

测试：

```java
public static void main(String[] args) {
    ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
    Student student = (Student)context.getBean("student");
    System.out.println(student.toString());
}
```

测试结果：

```java
Student{
     name='张三',
     address=Address{address='北京'},
     books=[红楼梦, 西游记, 水浒传, 三国演义],
     hobbies=[打球, 唱歌, 跳舞],
     card={身份证=123456,银行卡=456789},
     games=[LOL, COC, BOB],
     wife='null',
     info={password=123456,url=男,driver=2018020202,userme=张三}
}
```

### 6.3 拓展方式注入（p 命名空间和 c 命名空间）

实体类：

```java
public class User {

    private String name;
    private int age;

    有参构造/无参构造/get/set/toString……
}
```

beans.xml 配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- p 命名空间注入，可以直接注入属性的值：property -->
    <bean id="user" class="com.song.pojo.User" p:name="张三" p:age="18"/>

    <!-- c 命名空间注入，通过构造器注入：construct-arg -->
    <bean id="user2" class="com.song.pojo.User" c:age="18" c:name="李四"/>

</beans>
```

测试：

```java
@Test
    public void test2(){
        ApplicationContext context = new ClassPathXmlApplicationContext("userbeans.xml");
//        User user = (User) context.getBean("user");
//        User user = context.getBean("user", User.class); // 第二个参数写上，就不用强转了
        User user = context.getBean("user2", User.class);
        System.out.println(user);

    }
```

结果：

```java
//User{name='张三', age=18}
User{name='李四', age=18}
```

**注意**：不能直接使用，必须先在头文件中加入约束文件。

- p 命名空间约束文件：`xmlns:p="http://www.springframework.org/schema/p"`
- **p 命名空间类似于 set 方法注入，bean 实体类中必须有无参构造**

- c 命名空间约束文件：`xmlns:c="http://www.springframework.org/schema/c"`

- **c 命名空间注入类似于构造器注入，bean 实体类中必须有有参构造**

### 6.4 bean 的作用域（Bean scopes）

在 Spring 中，那些组成应用程序的主体及由 Spring IoC 容器所管理的对象，被称之为 bean。简单地讲，bean 就是由 IoC 容器初始化、装配及管理的对象。

> 官方文档：

| Scope                                                        | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [singleton](https://docs.spring.io/spring/docs/5.2.7.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-singleton) | (Default) Scopes a single bean definition to a single object instance for each Spring IoC container. |
| [prototype](https://docs.spring.io/spring/docs/5.2.7.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-prototype) | Scopes a single bean definition to any number of object instances. |
| [request](https://docs.spring.io/spring/docs/5.2.7.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-request) | Scopes a single bean definition to the lifecycle of a single HTTP request. That is, each HTTP request has its own instance of a bean created off the back of a single bean definition. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [session](https://docs.spring.io/spring/docs/5.2.7.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-session) | Scopes a single bean definition to the lifecycle of an HTTP `Session`. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [application](https://docs.spring.io/spring/docs/5.2.7.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-application) | Scopes a single bean definition to the lifecycle of a `ServletContext`. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [websocket](https://docs.spring.io/spring/docs/5.2.7.RELEASE/spring-framework-reference/web.html#websocket-stomp-websocket-scope) | Scopes a single bean definition to the lifecycle of a `WebSocket`. Only valid in the context of a web-aware Spring `ApplicationContext`. |

1. 单例模式（Spring 默认机制）

   当一个 bean 的作用域为 Singleton，那么 Spring IoC 容器中只会存在一个共享的 bean 实例，并且所有对 bean 的请求，只要 id 与该 bean 定义相匹配，则只会返回 bean 的同一实例。

   Singleton 是单例类型，就是在**创建起容器时就同时自动创建了一个 bean 的对象**，不管你是否使用，他都存在了，**每次获取到的对象都是同一个对象**。

   注意：Singleton 作用域是 Spring 中的**缺省作用域**。显式表示为：

   ```xml
   <bean id="user" class="com.song.pojo.User" scope="singleton"/>
   ```

   ```java
   @Test
   public void test2(){
       ApplicationContext context = new ClassPathXmlApplicationContext("userbeans.xml");
   
       User user = context.getBean("user", User.class);
       User user2 = context.getBean("user", User.class);
       System.out.println(user == user2); // true
   }
   ```

2. 原型模式

   **每次从容器中 get 时，都会产生一个新对象**。

   当一个 bean 的作用域为 Prototype，表示一个 bean 定义对应多个对象实例。Prototype 作用域的 bean 会导致在每次对该 bean 请求（将其注入到另一个 bean 中，或者以程序的方式调用容器的 getBean() 方法）时都会创建一个新的 bean 实例。

   Prototype 是原型类型，它在我们创建容器的时候并没有实例化，而是当我们**获取bean的时候才会去创建一个对象，而且我们每次获取到的对象都不是同一个对象**。

   根据经验，对有状态的 bean 应该使用 prototype 作用域，而对无状态的 bean 则应该使用 singleton 作用域。

   ```xml
   <bean id="user" class="com.song.pojo.User" scope="prototype"/>
   ```

   ```java
   @Test
   public void test2(){
       ApplicationContext context = new ClassPathXmlApplicationContext("userbeans.xml");
   
       User user = context.getBean("user", User.class);
       User user2 = context.getBean("user", User.class);
       System.out.println(user == user2); // false
   }
   ```

3. 其余的 request、session、application 这些只能在 web 开发中使用，只能用在基于 web 的 Spring ApplicationContext 环境。

![request_session](Spring5/pictures/request_session.png)

## 7、bean 的自动装配

- 自动装配是使用 **Spring 满足 bean 依赖的一种方式**

- Spring 会**在应用上下文中为某个 bean 寻找其依赖的 bean**，即在上下文自动寻找**并自动给 bean 装配属性**

由于在手动配置 xml 过程中，常常发生字母缺漏和大小写等错误，而无法对其进行检查，使得开发效率降低。采**用自动装配将避免这些错误，并且使配置简单化**。

在 Spring 中有三种装配方式：

1. 在 xml 中显式的配置

2. 在 Java 中显式的配置

3. 隐式的自动装配 bean【重要】

Spring 的自动装配需要从两个角度来实现，或者说是两个操作：

1. 组件扫描（component scanning）：Spring 会自动发现应用上下文中所创建的 bean；
2. 自动装配（autowiring）：Spring 自动满足 bean 之间的依赖，也就是我们说的 IoC/DI；

组件扫描和自动装配组合发挥巨大威力，使得显示的配置降低到最少。

**推荐不使用自动装配 xml 配置，而使用注解。**

### 7.1 测试

环境搭建：一个人有两个宠物

```java
public class Dog {
    public void shout(){
        System.out.println("汪~");
    }
}
```

```java
public class Cat {
    public void shout(){
        System.out.println("喵~");
    }
}
```

```java
public class People {
    private Cat cat;
    private Dog dog;
    private String name;
    
    set/get/toString...
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="cat" class="com.song.pojo.Cat"/>    
    <bean id="dog" class="com.song.pojo.Dog"/>
    
    <bean id="people" class="com.song.pojo.People">
        <property name="name" value="张三"/>
        <property name="cat" ref="cat"/>
        <property name="dog" ref="dog"/>
    </bean>
</beans>
```

```java
public class MyTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        People people = context.getBean("people", People.class);
        people.getCat().shout(); // 喵~
        people.getDog().shout(); // 汪~
    }
}
结果正常输出
```

### 7.2 byName 自动装配

**autowire byName (按名称自动装配)**

修改 bean 配置，增加一个属性  autowire="byName"，使用 ref 的 property 可以省略：

```xml
<bean id="cat" class="com.song.pojo.Cat"/>
<bean id="dog" class="com.song.pojo.Dog"/>

<!--
    byName: 会自动在容器上下文中查找，和自己对象 set 方法后面的值对应的 beanid
-->
<bean id="people" class="com.song.pojo.People" autowire="byName">
    <property name="name" value="张三"/>
    <!--<property name="cat" ref="cat"/>-->
    <!--<property name="dog" ref="dog"/>-->
</bean>
```

**结论：**

当一个 bean 节点带有 autowire byName 的属性时：

1. 将查找其类中所有的 set 方法名，例如 setCat，**获得将 set 去掉并且首字母小写的字符串**，即 cat。
2. **去 spring 容器中寻找是否有此字符串名称 id 的对象**。
3. 如果有，就取出注入；如果没有，就报空指针异常。

即，**byName 必须保证 bean 的 id 唯一**，并且这个 bean 要和自动注入的属性的 setXxx 方法的名字 xxx（将 set 去掉并且首字母小写） 一致。

### 7.3 byType 自动装配

**autowire byType (按类型自动装配)**

修改bean配置，增加一个属性  autowire="byType"：

```xml
<bean id="cat" class="com.song.pojo.Cat"/>
<bean class="com.song.pojo.Dog"/>

<!--
    byType：会自动在容器上下文中查找，和自己对象 属性类型相同的 bean
-->
<bean id="people" class="com.song.pojo.People" autowire="byType">
    <property name="name" value="张三"/>
    <!--<property name="cat" ref="cat"/>-->
    <!--<property name="dog" ref="dog"/>-->
</bean>
```

**结论：**

- byType 必须**保证所有 bean 的 class 唯一**，并且这个 bean 需要和自动注入的属性的类型一致。

- byType 必须保证类型全局唯一。即：同一类型的对象，在 spring 容器中唯一，如果不唯一，会报不唯一的异常 `NoUniqueBeanDefinitionException`。

==注意：对于 byName 自动装配，自动注入的属性的 bean 的 id 是可以省略的。==

### 7.4 使用注解实现自动装配

> The introduction of annotation-based configuration raised the question of whether this approach is “better” than XML. 

jdk1.5 开始支持注解，spring2.5 开始全面支持注解。

使用注解的准备工作：

1. 导入约束：context 约束

2. 配置注解的支持：`<context:annotation-config/>`

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           https://www.springframework.org/schema/context/spring-context.xsd">
   
       <context:annotation-config/>
   </beans>
   ```

#### @Autowried

直接在属性上使用即可，也可以在 set 方式上使用。

使用 Autowired 可以不用编写 set 方法，前提是这个自动装配的属性在 IOC（Spring）容器中存在，且符合类型 byType。

**测试**：

1. 在属性上使用 @Autowired 注解：

   ```java
   public class People {
       @Autowired
       @Qualifier(value = "cat1")
       private Cat cat;
       @Autowired
       private Dog dog;
       private String name;
       
       getter方法……
   }
   ```

2. 配置文件内容：

   ```xml
   <!--开启注解支持-->
   <context:annotation-config/>
   
   <bean id="cat" class="com.song.pojo.Cat"/>
   <bean id="dog" class="com.song.pojo.Dog"/>
   <bean id="people" class="com.song.pojo.People"/>
   ```

3. 测试结果：成功输出

**总结**：

@Autowired **首先按类型（byType）**自动装配；如果同一类型有多个 bean 存在，**再按照属性名（byName）**进行装配，即 bean 的 id 与属性名一致；如果属性名装配不成功，可以**使用 `@Qualifier（value = “xxx”）`装配与 id 一致的属性名**。

**扩展**：

> Autowired 源码：
>
> ```java
> public @interface Autowired {
>     	boolean required() default true;
> }
> ```

关于对象为空的问题

- Autowired 的 required 属性

  required 属性默认为 true，对象必须存对象，不能为 null；**如果为 false，则这个对象可以为 null**。
  
  ```java
  // 如果显式定义了 Autowired 的 required 属性为false，说明这个对象可以为 null，否则不允许为空
  @Autowired(required = false)
  ```

- @Nullable

  字段标记了这个注解，说明这个字段可以为 null 

#### @Qualifier

如果 @Autowired 自动装配的环境比较复杂，自动装配无法通过一个注解【@Autowired】完成的时候，可以使用 `@Qualifier(value = "xxx") ` 去配置 @Autowired 的使用，**指定一个唯一的 bean 对象注入（byName）**。

注意：``@Qualifier(value = "xxx") `` **不能单独使用**

**测试**：

1. 配置文件修改内容，保证类型存在，但名字不为类的默认名字！

   ```xml
   <!--开启注解支持-->
   <context:annotation-config/>
   <bean id="cat1" class="com.song.pojo.Cat"/>
   <bean id="cat111" class="com.song.pojo.Cat"/>
   <bean id="dog2" class="com.song.pojo.Dog"/>
   <bean id="dog222" class="com.song.pojo.Dog"/>
   <bean id="people" class="com.song.pojo.People"/>
   ```

2. 没有加 @Qualifier 测试，直接报错

3. 在属性上添加 @Qualifier 注解

   ```java
   public class People {
       @Autowired(required = false)
       @Qualifier(value = "cat111")
       private Cat cat;
       @Autowired
       @Qualifier(value = "dog222")
       private Dog dog;
       private String name;
   }
   ```

4. 测试结果：成功输出

#### @Resource 

@Resource 如果**有指定的 name 属性**，先按该属性进行 **byName** 方式查找装配；其次再进行默认的 byName 方式进行装配；如果以上都不成功，则按 **byType** 的方式自动装配；都不成功，则报异常。

```java
public class People {
    @Resource(name = "cat2")
    private Cat cat;
    @Resource
    private Dog dog;
    private String name;
}
```

#### 小结：

@Autowired 和 @Resource 异同：

- 作用相同都是用注解方式注入对象，都是用来自动装配 bean 的，都可以放在属性字段，或写在 setter 方法上。
- @Autowired （属于spring规范）默认通过 byType 的方式实现，而且必须要求这个对象存在（除非制定了 required 属性为 false），类型重复再通过 id 与属性名匹配（byName）找，都不合适则结合 @Qualifier 注解使用名字装配。
- @Resource（属于 J2EE 复返） 默认通过 byName 的方式实现，名称可以通过 name 属性进行指定。如果没有指定 name 属性，当注解写在字段上时，默认取字段名进行按照名称查找，如果注解写在 setter 方法上默认取属性名进行装配。如果找不到与名称匹配的 bean 时，则通过 byType 实现自动装配。如果两个都找不到的情况下就报错。注意，**如果 name 属性一旦指定，就只会按照名称进行装配**。
- 执行顺序不同：
  - @Autowired 先 byType 再 byName；
  - @Resource 先 byName 再 byType。



## 8、使用注解开发

- 在 spring4 之后，想要使用注解形式，必须得要引入 aop 的包。

![image-20200620113132113](Spring5/pictures/image-20200620113132113.png)

- 使用注解需要导入 context 约束，增加注解的支持。

- 必须**指定要扫描的包**，使这个包下的注解生效。扫描是为了类上的注解。

Spring 的配置文件 applicationContext.xml ： 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <!--指定要扫描的包，这个包下的注解就会生效-->
    <context:component-scan base-package="com.song"/>
    <context:annotation-config/>

</beans>
```

### 8.1 Bean 的实现（@Component）

在指定包下编写类，增加注解

```java
// 等价于配置文件中 <bean id="user" class="com.song.pojo.User"/>
@Component  // 组件
public class User {
    public String name = "张三";
}
```

测试：

```java
public void test(){
   ApplicationContext Context = new ClassPathXmlApplicationContext("applicationContext.xml");
   User user = (User) Context.getBean("user");
   System.out.println(user.name); // 张三
}
```

**注意**：@Component("xxx") 可以指定 id

- 如果是 @Component，即没有指定 id，那么这个 bean 的 **id 就是当前类的首字母小写的字符串**
- 如果是 @Component("xxx")，即指定了 id，那么 **getBean 时只能使用这个 id**，即`Context.getBean("xxx")`

### 8.2 属性注入（@Value）

可以在属性名上添加注解，也可以在setter方法上添加。

```java
@Component
public class User {
    // 相当于 <property name="name" value="张三"/>
    //@Value("张三")
    public String name;

    @Value("张三")
    public void setName(String name) {
        this.name = name;
    }
}
```

### 8.3 衍生的注解（@Repository、@Service、@Controller）

@Component 的三个衍生注解，在 web 开发中，会按照 mvc 三层架构分层。

- dao 层【@Repository】
- service 层【@Service】
- web 层【@Controller】

这四个注解功能都是一样的，都是代表将某个类注册到 Spring 中，装配 bean。

### 8.4 自动装配

前面已经学过。

### 8.5 作用域（@Scope）

- singleton：默认的，Spring会采用单例模式创建这个对象。**关闭工厂 ，所有的对象都会销毁**。
- prototype：多例模式。关闭工厂 ，所有的对象不会销毁。**内部的垃圾回收机制会回收**。

```java
@Component
@Scope("prototype")
public class User {
    //@Value("张三")
    public String name;

    @Value("张三")
    public void setName(String name) {
        this.name = name;
    }
}
```

### 8.6 小结

**xml 和注解比较**：

- xml 更加万能，适用于任何场合，结构清晰，维护简单方便
- 注解，不是自己提供的类使用不了，维护相对复杂，开发简单方便

**xml 与注解整合开发**：

- **xml 用来管理 bean**
- **注解只负责完成属性的注入**
- 使用时需要注意：必须让注解生效，需要开启注解的支持 `<context:annotation-config/>  `



## 9、使用 java 的方式配置 Spring

完全不使用 Spring 的 xml 配置，全权交给 java来做。

javaConfig 是 Spring 的一个子项目，它**通过 Java 类的方式提供 Bean 的定义信息**，在 Spring4 之后它成为了一个核心功能。这种纯 java 的配置方式，在 SpringBoot 中随处可见。

### 测试：

1. 实体类

   ```java
   // 注解意思：这个类被 Spring 接管了，注册到了容器中
   @Component // 其实不加这个注解也可以测试成功
   public class User {
       private String name;
   
       @Value("zhangsan") //属性注入值
       public void setName(String name) {
           this.name = name;
       }
       
       getter/toString……
   }
   ```

2. 新建一个 config 配置包，编写一个 javaConfig  配置类

   ```java
   //这个也会被 Spring 容器托管，注册到容器中，因为它本来就是一个 @Component
   // @Configuration 代表这是一个配置类，和之前的 beans.xml 一样
   @Configuration
   @ComponentScan("com.song.pojo")
   @Import(MyConfig2.class) //导入合并其他配置类，类似于配置文件中的 import 标签
   public class MyConfig {
   
       // 通过方法注册一个 bean，相当于之前写的一个 bean 标签
       // 方法的名字，相当于 bean 标签中的 id 属性
       // 方法的返回值，相当于 bean 标签中的 class 属性
       @Bean
       public User getUser(){
           return new User(); // 返回要注入 bean 的对象
       }
   }
   ```

   ```java
   @Configuration  //代表这是一个配置类
   public class MyConfig2 {
   }
   ```

3. 测试类

   ```java
   public class MyTest {
       public static void main(String[] args) {
           // 如果完全使用了配置类方式去做，就只能通过 AnnotationConfig 上下文来获取容器，通过配置类的class对象加载
           ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
           User getUser = (User) context.getBean("getUser"); // 参数为方法名       
           System.out.println(getUser.getName()); // zhangsan
           
           User user = (User) context.getBean("user"); // 参数为 类名的首字母小写
           System.out.println(user.getName()); // zhangsan
           
           System.out.println(getUser == user);//fasle
       }
   }
   ```

### 思考：

发现以下两种方式的注解操作都可以正确输出

1. 只在 **MyConfig 类中加 @Bean 注解**，`context.getBean("getUser")` 参数需要是 **@Bean 注解时的方法名**；

2. **不加 @Bean 注解，在 User 类中加 @Component 注解**，在 **MyConfig 类中加 @ComponentScan("xxx.xxx.xxx") 注解**，`context.getBean("user")` 参数需要是**类名的首字母小写字符串**。
3. 如上面的测试案例，将 @Bean 和 @Component 注解都加上，通过 getBean 得到的对象是**两个不同的对象**，也就是说，在 Spring 容器中存在着该类的两个对象。



## 10、代理模式

AOP 的底层机制就是动态代理。

代理模式的分类：

- 静态代理
- 动态代理

### 10.1 静态代理

#### 角色分析：

- 抽象角色 : 一般使用接口或者抽象类来实现

- 真实角色 : 被代理的角色

- 代理角色 : 代理真实角色，代理真实角色后 , 一般会做一些附属的操作

- 客户  :  访问代理对象的人，使用代理角色来进行一些操作 

  ![proxy1](Spring5/pictures/proxy1.png)

#### 代码步骤：（租房子案例）

1. 接口，抽象角色

   ```java
   // 出租房子
   public interface Rent {
       public void rent();
   }
   ```

2. 真实角色

   ```java
   // 房东
   public class Host implements Rent{
       public void rent() {
           System.out.println("房东要出租房子");
       }
   }
   ```

3. 代理角色

   ```java
   // 中介：帮房东出租房子
   public class Proxy implements Rent{
   
       private Host host; //多用组合，少用继承
   
       public Proxy(){
       }
   
       public Proxy(Host host) {
           this.host = host;
       }
   
       public void rent() {
           seeHouse();
           host.rent();
           hetong();
           fare();
       }
       // 看房
       public void seeHouse(){
           System.out.println("中介带你看房");
       }
       // 签合同
       public void hetong(){
           System.out.println("签租赁合同");
       }
       //收中介费
       public void fare(){
           System.out.println("收中介费");
       }
   }
   ```

4. 客户端访问代理角色

   ```java
   // 租客，租房的人
   public class Client {
       public static void main(String[] args) {
           // 房东要出租房子
           Host host = new Host();
   //        host.rent();//没有代理时直接访问房东       
           // 代理，中介帮房东租房子，但是代理角色一般会有一些附属操作
           Proxy proxy = new Proxy(host);
           // 不用面对房东，直接找中介租房即可
           proxy.rent();
       }
   }
   ```

在这个过程中，租客直接接触的就是中介，就如同现实生活中的样子，你看不到房东，但是你依旧租到了房东的房子通过代理，这就是所谓的代理模式。

#### 静态代理模式优缺点：

- 好处：
  - 可以使得**真实角色的操作更加纯粹，不用去关注一些公共的事情** 
  - **公共的业务由代理来完成**，实现了业务的分工 
  - 公共**业务发生扩展时，方便集中管理**

- 缺点：**一个真实角色就会产生一个代理角色**，代码量会翻倍，开发效率会变低

既想要静态代理的好处，又不想要静态代理的缺点，所以 , 就有了动态代理 !

#### 加深理解：

1. 创建一个抽象角色，比如平时做的用户业务，抽象起来就是增删改查

   ```java
   public interface UserService {
       public void add();
       public void delete();
       public void update();
       public void query();
   }
   ```

2. 需要一个真实对象来完成这些增删改查操作

   ```java
   // 真实对象
   public class UserServiceImpl implements UserService {
       public void add() {
           System.out.println("增加了一个用户");
       }
   
       public void delete() {
           System.out.println("删除了一个用户");
       }
   
       public void update() {
           System.out.println("修改了一个用户");
       }
   
       public void query() {
           System.out.println("查询了一个用户");
       }
   
   }
   ```

3. 需求：需要增加一个日志功能，该怎么实现

   - 思路1：在实现类上增加代码 【麻烦！】
   - 思路2：使用代理来做，能够不改变原来的业务情况下，实现此功能就是最好的

4. 设置一个代理类来处理日志，即代理角色

   ```java
   public class UserServiceProxy implements UserService {
   
       private UserServiceImpl userService;
   
       public void setUserService(UserServiceImpl userService) {
           this.userService = userService;
       }
   
       // 日志方法
       public void log(String  msg){
           System.out.println("[debug]使用了" + msg + "方法");
       }
       
       public void add() {
           log("add");
           userService.add();
       }
   
       public void delete() {
           log("delete");
           userService.delete();
       }
   
       public void update() {
           log("update");
           userService.update();
       }
   
       public void query() {
           log("query");
           userService.query();
       }  
   }
   ```

5. 测试访问类：

   ```java
   public class Client {
       public static void main(String[] args) {
           UserServiceImpl userService = new UserServiceImpl();
   //        userService.add();
   
           UserServiceProxy proxy = new UserServiceProxy();
           proxy.setUserService(userService);
           proxy.add();
       }
   }
   ```

6. 测试输出

   ```java
   [debug]使用了add方法
   增加了一个用户
   ```

**代理的思想：在不改变原来的代码的情况下，实现了对原有功能的增强，这是 AOP 中最核心的思想。**

### 10.2 动态代理

- 动态代理的角色和静态代理的一样 

- 动态代理的**代理类是动态生成的**，静态代理的代理类是我们提前写好的

- 动态代理分为两类 : 一类是基于接口动态代理 , 一类是基于类的动态代理

  - **基于接口**： **JDK 动态代理**【下面使用】

  - **基于类**： cglib
  - java 字节码实现： javassist  


#### JDK 动态代理

需要了解两个类：Proxy 和 InvocationHandler

- InvocationHandler：调用处理程序，并返回一个结果

> `InvocationHandler` 是代理实例的*调用处理程序* 实现的接口。 
>
> 每个代理实例都具有一个关联的调用处理程序。对代理实例调用方法时，将**对方法调用进行编码**并将其**指派到它的调用处理程序的 `invoke`  方法**。

````java
Object invoke(Object proxy, Method method, Object[] args)；
//参数
//proxy - 调用该方法的代理实例
//method -所述方法对应于调用代理实例上的接口方法的实例。方法对象的声明类将是该方法声明的接口，它可以是代理类继承该方法的代理接口的超级接口。
//args -包含的方法调用传递代理实例的参数值的对象的阵列，或null如果接口方法没有参数。原始类型的参数包含在适当的原始包装器类的实例中，例如java.lang.Integer或java.lang.Boolean 。
//返回：从代理实例的方法调用返回的值。
````

- Proxy：代理，生成动态代理实例

> `Proxy` 提供用于创建动态代理类和实例的静态方法，它还是由这些方法创建的所有动态代理类的超类。
>
>  每个代理实例都有一个关联的*调用处理程序* 对象，它可以实现接口 [`InvocationHandler`](../../../java/lang/reflect/InvocationHandler.html)。
>
> `newProxyInstance` 方法返回一个**指定接口的代理类实例**

```java
Object Proxy.newProxyInstance(ClassLoader loader, 
                              Class<?>[] interfaces,InvocationHandler h)
//参数:
//loader - 定义代理类的类加载器
//interfaces - 代理类要实现的接口列表
//h - 指派方法调用的调用处理程序 
//返回：一个带有代理类的指定调用处理程序的代理实例，它由指定的类加载器定义，并实现指定的接口 
```

##### 代码实现（之前的租房案例）：

抽象角色和真实角色和之前一样

1. 接口，抽象角色

   ```java
   // 出租房子
   public interface Rent {
       public void rent();
   }
   ```

2. 真实角色

   ```java
   // 房东
   public class Host implements Rent{
       public void rent() {
           System.out.println("房东要出租房子");
       }
   }
   ```

3. 代理角色 ProxyInvocationHandler.class

   ```java
   // 会用这个类自动生成代理类
   public class ProxyInvocationHandler implements InvocationHandler{
   
       // 被代理的接口
       private Rent rent;
   
       public void setRent(Rent rent) {
           this.rent = rent;
       }
   
       // 生成得到代理类，重点是第二个参数，获取要代理的抽象角色，之前都是一个角色，现在可以代理一类角色
       public Object getProxy(){
           return Proxy.newProxyInstance(this.getClass().getClassLoader(),
                   rent.getClass().getInterfaces(),this);
       }
   
       // 处理代理实例并返回结果
       // proxy : 代理类 method : 代理类的调用处理程序的方法对象
       public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
           seeHouse();
           // 动态代理本质，就是使用反射机制实现
           Object result = method.invoke(rent, args);
           fare();
           return result;
       }
       
   	//看房
       public void seeHouse(){
           System.out.println("中介带你看房子");
       }
       //收中介费
       public void fare(){
           System.out.println("收中介费");
       }
   }
   ```

4. 客户端

   ```java
   public class Client {
       public static void main(String[] args) {
           // 真实角色
           Host host = new Host();
           // 代理角色的调用处理程序
           ProxyInvocationHandler pih = new ProxyInvocationHandler();
           // 通过调用程序处理角色来处理我们要调用的接口对象（将真实角色放进去）
           pih.setRent(host);
           Rent proxy = (Rent) pih.getProxy(); // 动态生成对应的代理类
           proxy.rent();
       }
   }
   ```

核心：一个动态代理 , 一般代理某一类业务 , **一个动态代理可以代理多个类，代理的是接口**！

##### 动态代理模板

以之前用户业务为例

```java
// 会用这个类自动生成代理类
public class ProxyInvocationHandler implements InvocationHandler{

    // 被代理的接口
    private Object target;

    public void setTarget(Object target) {
        this.target = target;
    }
    
    // 生成得到代理类
    public Object getProxy(){
        return Proxy.newProxyInstance(this.getClass().getClassLoader(),
                target.getClass().getInterfaces(),this);
    }

    // 处理代理实例，并返回结果
    // proxy : 代理类
    // method : 代理类的调用处理程序的方法对象
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        log(method.getName());
        Object result = method.invoke(target, args);
        return result;
    }
	// 增加日志功能
    public void log(String msg){
        System.out.println("[debug]执行了"+ msg +"方法");
    }
}
```

测试：

```java
public class Client {
    public static void main(String[] args) {
        // 真实角色
        UserServiceImpl userService = new UserServiceImpl();
        // 代理角色，不存在
        ProxyInvocationHandler pih = new ProxyInvocationHandler();
        // 设置要代理的对象
        pih.setTarget(userService);
        // 动态生成代理类
        UserService proxy = (UserService) pih.getProxy();
		
        proxy.delete();
    }
}
```

结果：

```java
[debug]执行了delete方法
删除了一个用户
```

##### 动态代理的好处：

不仅拥有静态代理的所有好处，还具有以下好处：

- 一个动态代理类代理的是一个接口，一般就是对应某一类业务
- 一个动态代理可以代理多个类，只要是实现了同一个接口即可

![aop1](Spring5/pictures/aop1.jpg)



## 11、AOP

### 11.1 AOP 简介

AOP（Aspect Oriented Programming）意为：面向**切面编程**。

通过**预编译方式和运行期动态代理**实现程序功能的统一维护的一种技术。AOP 是 OOP 的延续，是软件开发中的一个热点，也是 Spring 框架中的一个重要内容，是**函数式编程的一种衍生范型**。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得**业务逻辑各部分之间的耦合度降低**，**提高程序的可重用性，同时提高了开发的效率。**

Spring 的 AOP 就是将公共的业务（日志 , 安全等）和领域业务结合起来，当执行领域业务时，将会把公共业务加进来，实现**公共业务的重复利用**，**领域业务更纯粹**，程序员专注领域业务。

其本质还是动态代理，即 Aop 在**不改变原有代码的情况下 , 增加新的功能**。

![image-20200621101134481](Spring5/pictures/image-20200621101134481.png)

### 11.2 Aop在Spring中的作用

提供声明式事务；允许用户自定义切面

- 横切关注点：跨越应用程序多个模块的**方法或功能**。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志 , 安全 , 缓存 , 事务等等 ....
- 切面（ASPECT）：**横切关注点 被模块化 的特殊对象**。即，它是一个**类**。【如 Log 类】
- 通知（Advice）：**切面必须要完成的工作**。即，它是**类中的一个方法**。【如 Log 类中的方法】
- 目标（Target）：**被通知对象**。
- ==代理（Proxy）：向目标对象应用通知之后创建的对象。==
- 切入点（PointCut）：**切面通知 执行的 “地点”**的定义。
- 连接点（JointPoint）：与切入点匹配的**执行点**。

![image-20200621101245400](Spring5/pictures/image-20200621101245400.png)

Spring AOP 中，通过 Advice 定义横切逻辑，Spring 中支持 5 种类型的 Advice：

| 通知类型     | 连接点               | 实现接口                                        |
| ------------ | -------------------- | ----------------------------------------------- |
| 前置通知     | 方法前               | org.springframework.aop.MethodBeforeAdvice      |
| 后置通知     | 方法后               | org.springframework.aop.AfterReturningAdvice    |
| 环绕通知     | 方法前后             | org.aopalliance.intercept.MethodInterceptor     |
| 异常抛出通知 | 方法抛出异常         | org.springframework.aop.ThrowsAdvice            |
| 引介通知     | 类中增加新的方法属性 | org.springframework.aop.IntroductionInterceptor |

### 11.3 使用 Spring 实现 AOP

准备工作：

- 使用AOP织入，需要导入一个依赖包

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```

- Spring 的配置文件中要导入 aop 的约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <bean/>......
<beans>
```

- 目标业务接口：

```java
public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void query();
}
```

- 目标业务类：

```java
public class UserServiceImpl implements UserService{
    public void add() {
        System.out.println("增加了一个用户");
    }
    public void delete() {
        System.out.println("删除了一个用户");
    }
    public void update() {
        System.out.println("修改了一个用户");
    }
    public void query() {
        System.out.println("查询了一个用户");
    }
}
```

- 测试类

```java
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        // 注意：动态代理代理的是接口
        UserService userService = (UserService) context.getBean("userService");
        userService.add();
    }
}
```

#### 方式一：使用 Spring 的 API 接口实现

1. 编写**增强类，要实现通知接口**

   ```java
   // 前置增强类
   public class Log implements MethodBeforeAdvice {
       // method：要执行的目标对象的方法
       // objects：被调用的方法的参数
       // target：被调用的目标对象
       public void before(Method method, Object[] objects, Object target) throws Throwable {
           System.out.println(target.getClass().getName() + "的" + method.getName() + "被执行了");
       }
   }
   // 后置增强类
   public class AfterLog implements AfterReturningAdvice {
   	//returnValue 返回值
       public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
           System.out.println("执行了" + method.getName() + " 方法，返回结果为："+ returnValue);
       }
   }
   ```

 2. 在 Spring 的配置文件中注册，实现 aop 切入

    ```xml
    <!--注册bean-->
    <bean id="userService" class="com.song.service.UserServiceImpl"/>
    <bean id="log" class="com.song.log.Log"/>
    <bean id="afterLog" class="com.song.log.AfterLog"/>
    
    <!--方式一：使用原生的 Spring API 接口-->
    <!--配置aop，需要导入 aop 的约束-->
    <aop:config>
        <!--切入点 expression：表达式，execution(要执行的位置！* * * * *) -->
        <aop:pointcut id="pointcut" expression="execution(* com.song.service.UserServiceImpl.*(..))"/>
        <!--执行环绕增强！-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
    </aop:config>
    ```

3. 测试结果

   ```java
   com.song.service.UserServiceImpl的add被执行了
   增加了一个用户
   执行了add 方法，返回结果为：null
   ```

#### 方式二：自定义类来实现 AOP【主要是切面定义】

1. **自定义一个切入类**

   ```java
   public class DiyPointCut {
       public void before(){
           System.out.println("=======方法执行前=========");
       }
   
       public void after(){
           System.out.println("=======方法执行后=========");
       }
   }
   ```

2. 在 Spring 的配置文件中配置

   ```xml
   <bean id="userService" class="com.song.service.UserServiceImpl"/>
   
   <!--方式二：自定义类-->
   <bean id="diy" class="com.song.diy.DiyPointCut"/>
   <aop:config>
       <!--自定义切面，ref 要引用的类-->
       <aop:aspect ref="diy">
           <!--切入点-->
           <aop:pointcut id="pointcut" expression="execution(* com.song.service.UserServiceImpl.*(..))"/>
           <!--通知-->
           <aop:before method="before" pointcut-ref="pointcut"/>
           <aop:after method="after" pointcut-ref="pointcut"/>
       </aop:aspect>
   </aop:config>
   ```

3. 测试结果

   ```java
   =======方法执行前=========
   增加了一个用户
   =======方法执行后=========
   ```

#### 方式三：使用注解实现

1. ==编写一个注解实现的增强类==

   ```java
   // 使用注解方式实现 AOP
   @Aspect //标注这个类是一个切面
   public class AnnotionPointCut {
   
       @Before("execution(* com.song.service.UserService.*(..))")
       public void before(){
           System.out.println("=======方法执行前=========");
       }
   
       @After("execution(* com.song.service.UserService.*(..))")
       public void after(){
           System.out.println("=======方法执行后=========");
       }
   
       // 在环绕增强中，我们可以给定一个参数，代表我们要获取处理切入的点
       @Around("execution(* com.song.service.UserService.*(..))")
       public void around(ProceedingJoinPoint jp) throws Throwable {
           System.out.println("环绕前");
           // 执行方法
           Object proceed = jp.proceed();
           System.out.println("环绕后");
   
   //        Signature signature = jp.getSignature();// 获得签名，类的信息
   //        System.out.println("signature" + signature);
   
       }
   }
   ```
   
2. Spring 配置

   ```xml
   <!--方式三：使用注解实现-->
   <bean id="annotationPointCut" class="com.song.diy.AnnotionPointCut"/>
   <!--开启注解支持   
   	JDK(默认 proxy-target-class="false")  cglib(proxy-target-class="true")
   -->
   <aop:aspectj-autoproxy proxy-target-class="true"/>
   ```

3. 测试结果

   ```java
   环绕前
   =======方法执行前=========
   增加了一个用户
   环绕后
   =======方法执行后=========
   ```

注意，执行顺序为：**环绕前 ---->  方法前  ---->  方法 ---->  环绕后 ---->  方法后**

`aop:aspectj-autoproxy`说明：

- 通过 aop 命名空间的 `<aop:aspectj-autoproxy />` **声明自动为 spring 容器中那些配置 `@aspectJ` 切面的 bean 创建代理，织入切面**。当然，spring 在内部依旧采用 `AnnotationAwareAspectJAutoProxyCreator` 进行自动代理的创建工作，但具体实现的细节已经被 `<aop:aspectj-autoproxy />` 隐藏起来了。

- `<aop:aspectj-autoproxy />` 有一个 `proxy-target-class` 属性：
  - 默认为 false，表示使用 JDK 动态代理织入增强；
  - 当配为 `poxy-target-class="true"` 时，表示使用 CGLib 动态代理技术织入增强；
  - 不过即使 `proxy-target-class` 设置为 false，如果目标类**没有声明接口**，则 spring 将自动使用 CGLib 动态代理。



## 12、整合 MyBatis

### 12.1 步骤：

1. 导入相关 jar 包
   - junit
   - mybatis
   - mysql 数据库
   - spring 相关的
   - aop 织入
   - **mybatis-spring**

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>

<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.2</version>
</dependency>

<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>

<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.1.9.RELEASE</version>
</dependency>
<!--spring 操作数据库的话，还需要一个spring-jdbc-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.1.9.RELEASE</version>
</dependency>

<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.2</version>
</dependency>

<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.16.10</version>
</dependency>
```

注意：配置Maven静态资源过滤问题

```xml
<!--在 build 中配置resources，来防止资源导出失败的问题-->
<build>
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

2. 编写配置文件
3. 代码实现

### 12.2 MyBatis 回顾

1. MyBatis 核心配置文件 mybatis-config.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <!--核心配置文件-->
   <configuration>
   
       <typeAliases>
           <!--<typeAlias type="com.song.pojo.User" alias="User"/>-->
           <package name="com.song.pojo"/>
       </typeAliases>
   
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC"/>
               <dataSource type="POOLED">
                   <property name="driver" value="com.mysql.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                   <property name="username" value="root"/>
                   <property name="password" value="root"/>
               </dataSource>
           </environment>
       </environments>
   
       <mappers>
           <!--<mapper resource="com/song/mapper/UserMapper.xml"/>-->
           <mapper class="com.song.mapper.UserMapper"/>
       </mappers>
   </configuration>
   ```

2. 编写实体类 

   ```java
   @Data
   public class User {
       private int id;
       private String name;
       private String pwd;
   }
   ```

3. 编写 UserMapper 接口

   ```java
   public interface UserMapper {
       public List<User> selectUser();
   }
   ```

4. UserMapper 的配置文件 UserMapper.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.song.mapper.UserMapper">
   
       <select id="selectUser" resultType="user">
           select * from mybatis.user
       </select>
   </mapper>
   ```

5. 测试

   ```java
   @Test
   public void test1() throws IOException {
       String resource = "mybatis-config.xml";
       InputStream is = Resources.getResourceAsStream(resource);
       SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(is);
       SqlSession sqlSession = factory.openSession(true);
   
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       List<User> users = mapper.selectUser();
   
       for (User u : users){
           System.out.println(u);
       }
       sqlSession.close();
   }
   ```

### 12.3 MyBatis-Spring

> 官方文档：http://mybatis.org/spring/zh/index.html

**MyBatis-Spring 简介：**

MyBatis-Spring 会帮助你将 MyBatis 代码无缝地整合到 Spring 中。

**MyBatis-Spring 版本选择：**

| MyBatis-Spring | MyBatis | Spring 框架 | Spring Batch | Java    |
| :------------- | :------ | :---------- | :----------- | :------ |
| 2.0            | 3.5+    | 5.0+        | 4.0+         | Java 8+ |
| 1.3            | 3.4+    | 3.2.2+      | 2.1+         | Java 6+ |

#### 整合方式一：

**步骤：**

1. 引入 Spring 配置文件 spring-dao.xml【这个 xml 文件可以作为一个固定的模板使用】

2. 编写数据源配置【取代 mybatis-config.xml 中的 \<environments> 】

3. 编写 sqlSessionFactory 配置【取代 mybatisUtils.class 中 SqlSessionFactory 建立过程】  

4. 编写 sqlSessionTemplate 配置【也就是 MyBatis 中的 sqlSession 】

   注意：**sqlSessionTemplate 只能使用构造器注入 sqlSessionFactory，因为它没有 setter 方法**

   >  `SqlSessionTemplate` 是 MyBatis-Spring 的核心。作为 **`SqlSession` 的一个实现**，这意味着可以使用它无缝代替你代码中已经在使用的 `SqlSession`。`SqlSessionTemplate` 是**线程安全**的，可以被多个 DAO 或映射器所共享使用。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/aop
           https://www.springframework.org/schema/aop/spring-aop.xsd">
   
       <!--DataSource：使用 spring 的数据源替换 MyBatis 的配置 c3p0  druid-->
       <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
           <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
           <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
           <property name="username" value="root"/>
           <property name="password" value="root"/>
       </bean>
   
       <!--sqlSessionFactory-->
       <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
           <property name="dataSource" ref="dataSource"/>
           <!--绑定 MyBatis 配置文件-->
           <property name="configLocation" value="classpath:mybatis-config.xml"/>
           <property name="mapperLocations" value="classpath:com/song/mapper/*.xml"/>
       </bean>
   
       <!-- org.mybatis.spring.SqlSessionTemplate：就是我们使用的sqlSession  -->
       <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
           <!--只能使用构造器注入 sqlSessionFactory，因为它没有 set 方法-->
           <constructor-arg index="0" ref="sqlSessionFactory"/>
       </bean>
   
   </beans>
   ```

5. 需要给接口加实现类 UserMapperImpl

   因为要把 UserMapper 注入到 Spring 容器中管理，但是 UserMapper 是一个接口，所以需要一个实现类，把这个实现类注入到 Spring 容器中。

   ```java
   public class UserMapperImpl implements UserMapper {
   
       // 原来所有的操作，都使用sqlSession来执行，现在都使用 SqlSessionTemplate
       private SqlSessionTemplate sqlSession;
   	// 使用 set 注入 SqlSessionTemplate 对象
       public void setSqlSession(SqlSessionTemplate sqlSession) {
           this.sqlSession = sqlSession;
       }
   
       public List<User> selectUser() {
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           return mapper.selectUser();
       }
   }
   ```

6. 将**自己写的实现类 UserMapperImpl，注入到 Spring 中**，applicationContext.xml 会通过 `<import>` 标签整合其他 Spring 的 xml 文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           https://www.springframework.org/schema/context/spring-context.xsd
           http://www.springframework.org/schema/aop
           https://www.springframework.org/schema/aop/spring-aop.xsd">
   
      <import resource="spring-dao.xml"/>
   
       <bean id="userMapper" class="com.song.mapper.UserMapperImpl">
           <property name="sqlSession" ref="sqlSession"/>
       </bean>
   
   </beans>
   ```

7. 测试使用即可

   ```java
   @Test
   public void test2(){
       ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
       UserMapper userMapper = context.getBean("userMapper", UserMapper.class);
       List<User> users = userMapper.selectUser();
       for (User u : users){
           System.out.println(u);
       }
   }
   ```

**注意：**

MyBatis 的**核心配置文件实际上都可以被 Spring 整合**，但是**通常会将取别名 `<typeAliases>` 和设置 `<settings>` 操作保留在核心配置文件中配置**。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--核心配置文件-->
<configuration>
    <typeAliases>
        <!--<typeAlias type="com.song.pojo.User" alias="User"/>-->
        <package name="com.song.pojo"/>
    </typeAliases>

    <!--设置-->
    <!--<settings>-->
        <!--<setting name="" value=""/>-->
    <!--</settings>-->
</configuration>
```

#### 整合方式二

mybatis-spring 1.2.3 版本以上的才有这个。

> `SqlSessionDaoSupport` 是一个抽象的支持类，用来为你提供 `SqlSession`。调用 `getSqlSession()` 方法你会得到一个 `SqlSessionTemplate`，之后可以用于执行 SQL 方法。

也就是说，不用我们手动通过 `sqlSessionFactory` 来创建 `SqlSessionTemplate`，**`SqlSessionDaoSupport` 已经帮我们创建好了**，我们只需要**通过其中的  `getSqlSession()` 方法就可以拿到这个  `SqlSessionTemplate` 对象**。比起方式一，不需要管理 SqlSessionTemplate，而且**对事务的支持更加友好** 。

`SqlSessionDaoSupport` 源码如下：

```java
public abstract class SqlSessionDaoSupport extends DaoSupport {
    private SqlSessionTemplate sqlSessionTemplate;

    public SqlSessionDaoSupport() {
    }

    public void setSqlSessionFactory(SqlSessionFactory sqlSessionFactory) {
        if (this.sqlSessionTemplate == null || sqlSessionFactory != this.sqlSessionTemplate.getSqlSessionFactory()) {
            this.sqlSessionTemplate = this.createSqlSessionTemplate(sqlSessionFactory);
        }
    }
    
    public SqlSession getSqlSession() {
        return this.sqlSessionTemplate;
    }   
    ....
}
```

**步骤：**

1. 修改整合方式一中的 UserDaoImpl，只需要**继承 `SqlSessionDaoSupport` ，使用  `getSqlSession()` 方法得到 sqlSession**。

   ```java
   public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper{
   
       public List<User> selectUser() {
   //        SqlSession sqlSession = getSqlSession();
   //        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   //        return mapper.selectUser();
           return getSqlSession().getMapper(UserMapper.class).selectUser();
       }
   }
   ```

2. 修改 bean 配置文件 applicationContext.xml，注意，在这个 bean 中注入的是 sqlSessionFactory

   ```xml
   <bean id="userMapper2" class="com.song.mapper.UserMapperImpl2">
       <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
   </bean>
   ```

3. spring-dao.xml 中获取 sqlSession 的 bean 就可以不需要了

4. 测试

   ```java
   @Test
   public void test2(){
       ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
       UserMapper userMapper = context.getBean("userMapper2", UserMapper.class);
       List<User> users = userMapper.selectUser();
       for (User u : users){
           System.out.println(u);
       }
   }
   ```

**总结：**

整合到 Spring 以后**可以完全不要 MyBatis 的配置文件**，除了这些方式可以实现整合之外，我们**还可以使用注解来实现**。



## 13、声明式事务

### 13.1 事务

- 把一组业务当成一个业务来做，要么同时成功，要么同时失败
- 事务在项目的开发中十分重要，涉及到数据的一致性问题
- 确保完整性和一致性

**事务四个属性 ACID**

1. 原子性（atomicity）
   - 事务是原子性操作，由一系列动作组成，事务的原子性确保动作要么全部完成，要么完全不起作用

2. 一致性（consistency）
   - 一旦所有事务动作完成，事务就要被提交。数据和资源处于一种**满足业务规则的一致性状态中**

3. 隔离性（isolation）
   - 可能多个事务会同时处理相同的数据，因此每个事务都应该与其他事务隔离开来，防止数据损坏

7. 持久性（durability）

   - 事务一旦提交，无论系统发生什么问题，结果都不会再被影响，被持久化的写到存储器中

### 13.2 Spring 中的事务管理

Spring 在不同的事务管理 API 之上定义了一个抽象层，使得开发人员不必了解底层的事务管理 API 就可以使用 Spring 的事务管理机制。Spring 支持编程式事务管理和声明式的事务管理。

**声明式事务：利用 AOP**

- 一般情况下比编程式事务好用。
- 将事务管理代码从业务方法中分离出来，以声明的方式来实现事务管理。
- 将事务管理作为横切关注点，通过 AOP 方法模块化。Spring 中通过 Spring AOP 框架支持声明式事务管理。

**编程式事务：需要在代码中进行事务的管理**

- 将事务管理代码嵌到业务方法中来控制事务的提交和回滚
- 缺点：必须在每个事务操作业务逻辑中包含额外的事务管理代码

**为什么需要事务：**

- 如果不配置事务，可能存在数据提交不一致的情况

- 如果不在 spring 中去配置事务，就需要在代码中手动配置事务

#### 声明式事务：利用 AOP

##### 测试：

1. 基于之前的案例，给 userDao 接口新增两个方法，删除和增加用户

   ```java
   public interface UserMapper {
       public List<User> selectUser();
       //添加一个用户
       public int addUser(User user);
       //删除一个用户
       public int deleteUser(int id);
   }
   ```

2. mapper.xml 文件，故意把 delete 写错为 deletes

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   
   <mapper namespace="com.song.mapper.UserMapper">
       <select id="selectUser" resultType="user">
           select * from mybatis.user
       </select>
   
       <insert id="addUser" parameterType="user">
           insert into user (id, name, pwd) values (#{id},#{name},#{pwd});
       </insert>
   
       <delete id="deleteUser" parameterType="_int">
           <!--deletes from user where id=#{id}-->
           delete from user where id=#{id}
       </delete>
   </mapper>
   ```

3. 编写接口的实现类，在实现类中，进行调用操作

   ```java
   public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper {
       public List<User> selectUser() {
           User user = new User(5, "小王", "123456");
           UserMapper mapper = getSqlSession().getMapper(UserMapper.class);
   
           mapper.addUser(user);// 调用
           mapper.deleteUser(5);// 调用
   
           return mapper.selectUser();
       }
   
       public int addUser(User user) {
           return getSqlSession().getMapper(UserMapper.class).addUser(user);
       }
   
       public int deleteUser(int id) {
           return getSqlSession().getMapper(UserMapper.class).deleteUser(id);
       }
   }
   ```

4. 测试

   ```java
   @Test
   public void test(){
       ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
       UserMapper userMapper = context.getBean("userMapper", UserMapper.class);
       List<User> users = userMapper.selectUser();
       for (User u : users){
           System.out.println(u);
       }
   }
   ```

5. 测试结果

   报错：sql 异常， delete 写错

   结果：插入成功

   说明没有进行事务的管理，我们想让他们都成功才成功，有一个失败，就都失败，我们就应该需要**事务！**

   以前我们都需要自己手动管理事务，十分麻烦，但是 Spring 给我们提供了事务管理，我们只需要配置即可。


##### Spring 进行事务的管理

> 一个使用 MyBatis-Spring 的其中一个主要原因是它**允许 MyBatis 参与到 Spring 的事务管理中**。而不是给 MyBatis 创建一个新的专用事务管理器，**MyBatis-Spring 借助了 Spring 中的 DataSourceTransactionManager 来实现事务管理**。

事务管理器：

- 无论使用Spring的哪种事务管理策略（编程式或者声明式）**事务管理器都是必须的**。
- 就是 Spring 的核心事务管理抽象，管理封装了一组独立于技术的方法。

1. 使用 Spring 管理事务，注意头文件的约束导入 : tx

   ```xml
   xmlns:tx="http://www.springframework.org/schema/tx"
   
   http://www.springframework.org/schema/tx
   http://www.springframework.org/schema/tx/spring-tx.xsd">
   ```

2. 配置声明式事务即事务管理器，这里配置的是 JDBC 事务

   ```xml
   <!--配置声明式事务-->
   <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
       <property name="dataSource" ref="dataSource"/>
   </bean>
   ```

3. 配置事务的通知和事务的切入（AOP）

   注意：使用 AOP 需要导入 AOP 的约束头文件

   ```xml
   <!--结合AOP，实现事务的织入-->
   <!--配置事务通知-->
   <tx:advice id="txAdvice" transaction-manager="transactionManager">
       <!--给哪些方法配置事务-->
       <!--配置事务的传播特性  propagation-->
       <tx:attributes>
           <tx:method name="add" propagation="REQUIRED"/>
           <tx:method name="delete" propagation="REQUIRED"/>
           <tx:method name="update" propagation="REQUIRED"/>
           <tx:method name="query" read-only="true"/>
           <tx:method name="*" propagation="REQUIRED"/>
       </tx:attributes>
   </tx:advice>
   
   <!--配置事务切入-->
   <aop:config>
       <aop:pointcut id="txPointCut" expression="execution(* com.song.mapper.*.*(..))"/>
       <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
   </aop:config>
   ```

4. 测试（其他代码不用改动）

   结果：数据库中的数据没有变动

   说明说明事务管理已经成功。

##### 注意：

spring 事务传播特性 \<tx:method/\> 中的 propagation 属性：事务传播行为就是**多个事务方法相互调用时，事务如何在这些方法间传播**。

Spring 支持 7 种事务传播行为：

- propagation_requierd：如果当前没有事务，就新建一个事务，如果已存在一个事务中，加入到这个事务中，这是最常见的选择。
- propagation_supports：支持当前事务，如果没有当前事务，就以非事务方法执行。
- propagation_mandatory：使用当前事务，如果没有当前事务，就抛出异常。
- propagation_required_new：新建事务，如果当前存在事务，把当前事务挂起。
- propagation_not_supported：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
- propagation_never：以非事务方式执行操作，如果当前事务存在则抛出异常。
- propagation_nested：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与propagation_required类似的操作

Spring 默认的事务传播行为是 **PROPAGATION_REQUIRED**，它适合于绝大多数的情况。

假设 ServiveX#methodX() 都工作在事务环境下（即都被 Spring 事务增强了），假设程序中存在如下的调用链：Service1#method1()->Service2#method2()->Service3#method3()，那么这 3 个服务类的 3 个方法通过 Spring 的事务传播机制都工作在同一个事务中。

比如，上面案例的几个方法存在调用，所以会被放在一组事务当中。

