#### 1.为什么要使用 spring？

-   spring 提供 ioc 技术，容器会帮你管理依赖的对象，从而不需要自己创建和管理依赖对象了，更轻松的实现了程序的解耦。
-   spring 提供了事务支持，使得事务操作变的更加方便。
-   spring 提供了面向切片编程，这样可以更方便的处理某一类的问题。
-   更方便的框架集成，spring 可以很方便的集成其他框架，比如 MyBatis、hibernate 等。

#### 2.解释一下什么是 aop？

aop 是面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。

简单来说就是统一处理某一“切面”（类）的问题的编程思想，比如统一处理日志、异常等。

#### 3.解释一下什么是 ioc？

ioc：Inversionof Control（中文：控制反转）是 spring 的核心，对于 spring 框架来说，就是由 spring 来负责控制对象的生命周期和对象间的关系。

简单来说，控制指的是当前对象对内部成员的控制权；控制反转指的是，这种控制权不由当前对象管理了，由其他（类,第三方容器）来管理。

#### 4.spring 有哪些主要模块？

-   spring core：框架的最基础部分，提供 ioc 和依赖注入特性。
-   spring context：构建于 core 封装包基础上的 context 封装包，提供了一种框架式的对象访问方法。
-   spring dao：Data Access Object 提供了JDBC的抽象层。

- spring aop：提供了面向切面的编程实现，让你可以自定义拦截器、切点等。
- spring Web：提供了针对 Web 开发的集成特性，例如文件上传，利用 servlet listeners 进行 ioc 容器初始化和针对 Web 的 ApplicationContext。
- spring Web mvc：spring 中的 mvc 封装包提供了 Web 应用的 Model-View-Controller（MVC）的实现。

#### 5.spring 常用的注入方式有哪些？

- setter 属性注入
- 构造方法注入
- 注解方式注入

#### 6.spring 中的 bean 是线程安全的吗？

spring 中的 bean 默认是单例模式，spring 框架并没有对单例 bean 进行多线程的封装处理。

实际上大部分时候 spring bean 无状态的（比如 dao 类），所有某种程度上来说 bean 也是安全的，但如果 bean 有状态的话（比如 view model 对象），那就要开发者自己去保证线程安全了，最简单的就是改变 bean 的作用域，把“singleton”变更为“prototype”，这样请求 bean 相当于 new Bean()了，所以就可以保证线程安全了。

-   有状态就是有数据存储功能。
-   无状态就是不会保存数据。

#### 7.spring 支持几种 bean 的作用域？

spring 支持 5 种作用域，如下：

-   singleton：spring ioc 容器中只存在一个 bean 实例，bean 以单例模式存在，是系统默认值；
-   prototype：每次从容器调用 bean 时都会创建一个新的示例，既每次 getBean()相当于执行 new Bean()操作；
-   Web 环境下的作用域：
-   request：每次 http 请求都会创建一个 bean；
-   session：同一个 http session 共享一个 bean 实例；
-   global-session：用于 portlet 容器，因为每个 portlet 有单独的 session，globalsession 提供一个全局性的 http session。

**注意：** 使用 prototype 作用域需要慎重的思考，因为频繁创建和销毁 bean 会带来很大的性能开销。

#### 8.spring 自动装配 bean 有哪些方式？

-   no：默认值，表示没有自动装配，应使用显式 bean 引用进行装配。
-   byName：它根据 bean 的名称注入对象依赖项。

- byType：它根据类型注入对象依赖项。


- 构造函数：通过构造函数来注入依赖项，需要设置大量的参数。
- autodetect：容器首先通过构造函数使用 autowire 装配，如果不能，则通过 byType 自动装配。

#### 9.spring 事务实现方式有哪些？

-   声明式事务：声明式事务也有两种实现方式，基于 xml 配置文件的方式和注解方式（在类上添加 @Transaction 注解）。
-   编码方式：提供编码的形式管理和维护事务。

#### 10.说一下 spring 的事务隔离？

spring 有五大隔离级别，默认值为 ISOLATION_DEFAULT（使用数据库的设置），其他四个隔离级别和数据库的隔离级别一致：

ISOLATION_DEFAULT：用底层数据库的设置隔离级别，数据库设置的是什么我就用什么；

ISOLATION**READ**UNCOMMITTED：未提交读，最低隔离级别、事务未提交前，就可被其他事务读取（会出现幻读、脏读、不可重复读）；

ISOLATION**READ**COMMITTED：提交读，一个事务提交后才能被其他事务读取到（会造成幻读、不可重复读），SQL server 的默认级别；

ISOLATION**REPEATABLE**READ：可重复读，保证多次读取同一个数据时，其值都和事务开始时候的内容是一致，禁止读取到别的事务未提交的数据（会造成幻读），MySQL 的默认级别；

ISOLATION_SERIALIZABLE：序列化，代价最高最可靠的隔离级别，该隔离级别能防止脏读、不可重复读、幻读。

**脏读** ：表示一个事务能够读取另一个事务中还未提交的数据。比如，某个事务尝试插入记录 A，此时该事务还未提交，然后另一个事务尝试读取到了记录 A。

**不可重复读** ：是指在一个事务内，多次读同一数据。

**幻读** ：指同一个事务内多次查询返回的结果集不一样。比如同一个事务 A 第一次查询时候有 n 条记录，但是第二次同等条件下查询却有 n+1 条记录，这就好像产生了幻觉。发生幻读的原因也是另外一个事务新增或者删除或者修改了第一个事务结果集里面的数据，同一个记录的数据内容被修改了，所有数据行的记录就变多或者变少了。

#### 11.说一下 spring mvc 运行流程？

- spring mvc 先将请求发送给 DispatcherServlet。
- DispatcherServlet 查询一个或多个 HandlerMapping，找到处理请求的 Controller。
- DispatcherServlet 再把请求提交到对应的 Controller。
- Controller 进行业务逻辑处理后，会返回一个ModelAndView。
- Dispathcher 查询一个或多个 ViewResolver 视图解析器，找到 ModelAndView 对象指定的视图对象。
- 视图对象负责渲染返回给客户端。

#### 12.Spring mvc 有哪些组件？

-   前置控制器 DispatcherServlet。
-   映射控制器 HandlerMapping。
-   处理器 Controller。
-   模型和视图 ModelAndView。
-   视图解析器 ViewResolver。

#### 13.@RequestMapping 的作用是什么？

将 http 请求映射到相应的类/方法上。

#### 14.@Autowired 的作用是什么？

@Autowired 它可以对类成员变量、方法及构造函数进行标注，完成自动装配的工作，通过@Autowired 的使用来消除 set/get 方法。

#### 15.什么是 spring boot？

spring boot 是为 spring 服务的，是用来简化新 spring 应用的初始搭建以及开发过程的。

#### 16.为什么要用 spring boot？

-   配置简单
-   独立运行
-   自动装配
-   无代码生成和 xml 配置
-   提供应用监控
-   易上手
-   提升开发效率

#### 17.spring boot 核心配置文件是什么？

spring boot 核心的两个配置文件：

-   bootstrap (. yml 或者 . properties)：boostrap 由父 ApplicationContext 加载的，比 applicaton 优先加载，且 boostrap 里面的属性不能被覆盖；
-   application (. yml 或者 . properties)：用于 spring boot 项目的自动化配置。

#### 18.spring boot 配置文件有哪几种类型？它们有什么区别？

配置文件有 . properties 格式和 . yml 格式，它们主要的区别是书法风格不同。

-   properties 配置如下：

    ```properties
    spring.RabbitMQ.port=5672
    ```


-   yml 配置如下：

    ```yaml
    spring:
        RabbitMQ:
            port: 5672
    ```

yml 格式不支持 @PropertySource 注解导入。

#### 19.spring boot 有哪些方式可以实现热部署？

-   使用 devtools 启动热部署，添加 devtools 库，在配置文件中把 spring.devtools. restart.enabled 设置为 true；
-   使用 Intellij Idea 编辑器，勾上自动编译或手动重新编译。

#### 20.jpa 和 hibernate 有什么区别？

jpa 全称 Java Persistence API，是 Java 持久化接口规范，hibernate 属于 jpa 的具体实现。

#### 21.什么是 spring cloud？

spring cloud 是一系列框架的有序集合。它利用 spring boot 的开发便利性巧妙地简化了分布式系统基础设施的开发，如服务发现注册、配置中心、消息总线、负载均衡、断路器、数据监控等，都可以用 spring boot 的开发风格做到一键启动和部署。

#### 22.spring cloud 断路器的作用是什么？

在分布式架构中，断路器模式的作用也是类似的，当某个服务单元发生故障（类似用电器发生短路）之后，通过断路器的故障监控（类似熔断保险丝），向调用方返回一个错误响应，而不是长时间的等待。这样就不会使得线程因调用故障服务被长时间占用不释放，避免了故障在分布式系统中的蔓延。

#### 23.spring cloud 的核心组件有哪些？

-   Eureka：服务注册于发现。
-   Feign：基于动态代理机制，根据注解和选择的机器，拼接请求 url 地址，发起请求。
-   Ribbon：实现负载均衡，从一个服务的多台机器中选择一台。
-   Hystrix：提供线程池，不同的服务走不同的线程池，实现了不同服务调用的隔离，避免了服务雪崩的问题。
-   Zuul：网关管理，由 Zuul 网关转发请求给对应的服务。



