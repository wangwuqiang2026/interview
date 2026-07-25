## **<font style="color:rgb(51,51,51);">为什么要用SpringBoot</font>**
使用 Spring Boot 的主要原因是它大幅简化了 Spring 项目的开发。

传统 Spring 项目需要编写大量 XML 或 Java 配置，手动管理依赖、配置数据源、事务、MVC 和 Tomcat，开发成本较高。Spring Boot 通过**自动配置**根据依赖和配置自动创建 Bean**，**减少了大量重复配置**；**

**通过Starter 起步依赖**统一管理第三方库版本，避免依赖冲突；同时提供**内嵌 Tomcat**，应用可以直接以 Jar 包运行，无需单独部署到服务器。

此外，Spring Boot 采用**约定大于配置**的理念，统一了项目结构和配置方式，并提供 Actuator 等生产级功能，方便监控和运维。需要注意的是，Spring Boot 并没有替代 Spring，而是在 Spring 的基础上进行了封装和增强，底层仍然依赖 Spring 的 IOC、AOP、事务管理等核心能力，因此开发效率和项目可维护性都得到了显著提升。  



## **<font style="color:rgb(51,51,51);">Spring Boot 的核心注解是哪个？它主要由哪几个注解组成的？</font>**
Spring Boot 的核心注解是 **@SpringBootApplication**，通常标注在启动类上，它是一个组合注解，主要由三个注解组成：**@SpringBootConfiguration**、**@EnableAutoConfiguration **和 **@ComponentScan**。

其中，@SpringBootConfiguration 本质上就是 @Configuration，用于声明配置类并向 Spring 容器注册 Bean；@ComponentScan 用于自动扫描启动类所在包及其子包中的组件，如 @Controller、@Service、@Repository 和 @Component；而 @EnableAutoConfiguration 是最核心的部分，它会根据项目依赖、配置文件以及运行环境，自动装配所需的 Bean，例如自动配置数据源、Spring MVC、内嵌 Tomcat 等，大大减少了手动配置工作。其底层通过导入大量自动配置类，并结合 @Conditional 条件注解按需生效，从而实现“约定大于配置”的开发模式

###  自动配置是怎么实现的？  
@EnableAutoConfiguration 导入自动配置类。

Spring Boot 会扫描所有自动配置类（如 *AutoConfiguration）。

每个自动配置类都会通过 @Conditional 系列条件注解判断是否生效。

条件满足时，自动向 Spring 容器中注册 Bean；不满足则跳过。

###  为什么启动类一般放在根包下？  
因为 @ComponentScan 默认以启动类所在包作为扫描起点，只会扫描该包及其子包。如果启动类放在子包中，其他平级包或上级包中的组件可能无法被扫描到，导致 Bean 无法注册。因此，Spring Boot 官方建议将启动类放在项目的根包下，以确保整个项目都能被正确扫描和自动装配。



## **<font style="color:rgb(51,51,51);">运行Spring Boot有哪几种方式？</font>**
| 启动方式 | 命令 | 使用场景 |
| --- | --- | --- |
| IDE | 点击 Run | 开发阶段（最常用） |
| Maven | `mvn spring-boot:run` | 本地开发、测试 |
| Gradle | `gradlew bootRun` | Gradle 项目 |
| Jar | `java -jar xxx.jar` | 生产环境（最常见） |
| WAR | 部署到 Tomcat | 传统项目，较少使用 |
| Docker | `docker run` | 容器化部署 |
| Kubernetes | K8s Deployment | 云原生、大规模集群 |


+ **IDE 直接运行**，执行启动类中的 `main` 方法，这是开发阶段最常用的方式。
+ **Maven 启动**，通过 `mvn spring-boot:run` 运行项目。 
+ **打包成可执行 Jar**，使用 `java -jar xxx.jar` 启动，这是生产环境最常见的部署方式，因为 Spring Boot 内嵌了 Tomcat。 
+ **Gradle 启动**，使用 `gradlew bootRun`，适用于 Gradle 项目。 
+ **打包成 WAR 部署到外部 Tomcat**，需要继承 `SpringBootServletInitializer`，现在使用较少。 
+ **容器化部署**，将 Jar 打包到 Docker 镜像中运行，进一步可部署到 Kubernetes 集群，这是目前企业云原生环境中的主流方式





## 如何在Spring Boot启动的时候运行一些特定的代码？
Spring Boot 提供了多种在启动时执行代码的方式。最常用的是实现 CommandLineRunner 或 ApplicationRunner 接口，它们会在 Spring 容器初始化完成后执行，其中 ApplicationRunner 提供了更丰富的启动参数解析能力，因此官方更推荐使用。

此外，还可以监听 ApplicationReadyEvent 等启动事件，在应用完全启动并可以对外提供服务后执行任务；如果只是针对某个 Bean 的初始化逻辑，则可以使用 @PostConstruct 或实现 InitializingBean。

实际开发中，启动初始化任务通常使用 ApplicationRunner 或 CommandLineRunner，Bean 初始化使用 @PostConstruct，需要等待应用完全就绪时则监听 ApplicationReadyEvent

###  方式一：实现CommandLineRunner  
```java
@Component
public class MyCommandLineRunner implements CommandLineRunner {

    @Override
    public void run(String... args) throws Exception {
        System.out.println("Spring Boot 启动完成，执行初始化代码...");
    }
}
```

**启动顺序**：Spring容器初始化

                             ↓

                所有Bean创建完成

                              ↓

               ApplicationContext刷新完成

                             ↓

               CommandLineRunner.run()

### 方式二：实现 ApplicationRunner  
```java
@Component
public class MyRunner implements ApplicationRunner {

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("启动完成...");
    }
}
```

与 CommandLineRunner 的区别：

CommandLineRunner 使用 String[] args

ApplicationRunner 使用 ApplicationArguments，可以方便解析启动参数

###  方法三：监听 Spring Boot 启动事件  
```java
@Component
public class MyListener {

    @EventListener(ApplicationReadyEvent.class)
    public void ready() {
        System.out.println("应用完全启动，可以对外提供服务");
    }
}
```

常见事件：

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/70336156/1784510994071-ac954f08-f663-4f0f-bff1-a092423c083f.png)

###  方法四：使用 @PostConstruct
```java
@Component
public class UserService {

    @PostConstruct
    public void init() {
        System.out.println("Bean 初始化完成");
    }
}
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/70336156/1784511194140-49fac9fb-e6b0-411b-9223-73a612ead730.png)

###  方法五：实现InitializingBean  
```java
@Component
public class UserService implements InitializingBean {

    @Override
    public void afterPropertiesSet() {
        System.out.println("Bean 初始化完成");
    }
}
```

作用与 @PostConstruct 类似，也是针对单个 Bean 的初始化

| **方式** | **执行时机** | **适用场景** |
| --- | --- | --- |
| `CommandLineRunner` | 应用启动完成后 | 启动初始化任务、加载数据 |
| `ApplicationRunner` | 应用启动完成后 | 需要解析启动参数，官方更推荐 |
| `ApplicationReadyEvent` | 应用完全就绪，可对外提供服务 | 启动后通知、预热缓存、注册服务 |
| `@PostConstruct` | 单个 Bean 初始化完成后 | Bean 初始化逻辑 |
| `InitializingBean` | Bean 初始化完成后 | 与 `@PostConstruct` 类似，但接口耦合更高 |




##  多个 Runner 如何控制执行顺序？  
如果存在多个 CommandLineRunner 或 ApplicationRunner，可以使用 @Order 或实现 Ordered 接口

```java
@Component
@Order(1)
public class Runner1 implements CommandLineRunner {
    @Override
    public void run(String... args) {
        System.out.println("Runner1");
    }
}
```

```java
@Component
@Order(2)
public class Runner2 implements CommandLineRunner {
    @Override
    public void run(String... args) {
        System.out.println("Runner2");
    }
}
```

输出：Runner1

   Runner2

数值**越小**，优先级**越高  **

****

## **<font style="color:rgb(51,51,51);">Spring Boot 需要独立的容器运行吗？</font>**
Spring Boot 默认不需要独立的 Servlet 容器运行，因为它采用了内嵌 Servlet 容器的设计。

以 spring-boot-starter-web 为例，它会默认引入 spring-boot-starter-tomcat，将嵌入式 Tomcat 作为依赖打包到应用中。启动时，SpringApplication.run() 不仅会初始化 Spring 容器，还会创建并启动内嵌的 Web 服务器，因此只需要执行 java -jar 就可以独立运行应用。

当然，Spring Boot 也支持打包成 WAR 部署到外部 Tomcat、Jetty 等 Servlet 容器中，以兼容传统部署方式，但目前生产环境更多采用 Jar + 内嵌容器，再结合 Docker 或 Kubernetes 进行部署。



##  Spring Boot 是如何启动 Tomcat 的？  
<!-- 这是一个文本绘图，源码为：flowchart TD
    Main([主程序入口 main]) --> Run[1. 启动 Spring 引导程序<br>SpringApplication.run]
    
    Run --> InitContext[2. 初始化应用上下文<br>创建 ApplicationContext]
    InitContext -.->|建立 Spring IOC 容器，准备加载 Bean| AutoConfig
    
    InitContext --> AutoConfig[3. 执行自动配置机制<br>自动配置 Auto-Configuration]
    AutoConfig -.->|扫描类路径，触发 ServletWebServerFactoryAutoConfiguration| WebContext
    
    AutoConfig --> WebContext[4. 创建 Web 专用上下文<br>创建 ServletWebServerApplicationContext]
    WebContext -.->|判定当前为 Web 应用，准备构建 Web 容器环境| Factory
    
    WebContext --> Factory[5. 寻找 Web 服务器工厂<br>获取 ServletWebServerFactory]
    Factory -.->|从 IOC 容器中获取 TomcatServletWebServerFactory 实例| CreateTomcat
    
    Factory --> CreateTomcat[6. 实例化内嵌容器<br>创建 Tomcat]
    CreateTomcat -.->|调用工厂方法，创建并配置原生 Tomcat 实例对象| StartTomcat
    
    CreateTomcat --> StartTomcat[7. 启动 Web 服务<br>Tomcat.start]
    StartTomcat -.->|绑定网络端口 默认 8080，开始监听并接收 HTTP 请求| End([启动完成])

    %% 样式美化
    style Main fill:#f9f,stroke:#333,stroke-width:2px
    style End fill:#bbf,stroke:#333,stroke-width:2px -->
![](https://cdn.nlark.com/yuque/__mermaid_v3/56f0fbeaf7ffaabd243bb93951812cd9.svg)





##  怎么换成 Jetty 或 Undertow？  
 先排除 Tomcat：  

```xml
<exclusions>
  <exclusion>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
  </exclusion>
</exclusions>
```

 然后加入：   spring-boot-starter-jetty  或者 spring-boot-starter-undertow



##  Jar 和 War 有什么区别？  
JAR（Java Archive） 是 Java 程序最常见的打包格式，本质上就是一个压缩包（ZIP）。

里面可以包含：

+ `.class` 文件 
+  配置文件 
+  图片等资源文件 
+  第三方依赖（Spring Boot 可打成 Fat Jar）

**WAR（Web Archive）** 是 Java Web 项目的标准部署格式 ， 不能直接运行， 必须部署到 Servlet 容器  

 里面主要包括：  

                      WEB-INF/

                               classes/

                               lib/

                      index.jsp

                       html

                       css

                       js



##  为什么 Spring Boot 推荐 Jar？  
+ 部署简单
+  不依赖服务器环境 
+  避免 Tomcat 版本冲突 
+  易于 Docker 化 
+  易于微服务部署



## **<font style="color:rgb(51,51,51);">Spring Boot中的监视器是什么？</font>**
Spring Boot 中的监视器一般指 Spring Boot Actuator，它是 Spring Boot 提供的应用监控和管理组件。

通过引入 spring-boot-starter-actuator，可以暴露一系列监控端点，例如 /actuator/health 查看应用健康状态、/actuator/metrics 查看 JVM 和 HTTP 请求等指标、/actuator/beans 查看 Spring Bean、/actuator/env 查看环境配置、/actuator/mappings 查看接口映射等。

Actuator 底层通过自动配置注册各种 Endpoint，并与 Micrometer 集成，能够将监控指标导出到 Prometheus、Grafana 等监控平台，便于线上应用的运维和性能分析。

常见的监控端点：

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/70336156/1784516137679-b0e042e1-5331-467d-96b8-5c15a7d7c513.png)

###  Spring Boot Actuator 默认暴露哪些 Endpoint？为什么有些访问不到？  
默认只有少数端点（如 health）会通过 Web 暴露，其他需要management.endpoints.web.exposure.include 配置开放，以降低安全风险。

###  如何自定义一个 Actuator Endpoint？  
可以使用 @Endpoint、@ReadOperation、@WriteOperation 等注解定义自己的监控端点。

###  Actuator 和 Micrometer 有什么区别？  
 Actuator 提供管理端点和监控入口；Micrometer 提供统一的指标采集 API，并支持将指标导出到 Prometheus、InfluxDB 等监控系统。  



## **<font style="color:rgb(51,51,51);">如何使用Spring Boot实现异常处理？</font>**
Spring Boot 中常见的异常处理方式有四种：第一种是在代码中使用 try-catch，但会造成大量重复代码；第二种是在 Controller 中使用 @ExceptionHandler，只能处理当前 Controller 的异常；第三种也是企业开发最常用的方式，使用 **@RestControllerAdvice **配合 **@ExceptionHandle**r 实现全局异常处理，可以统一捕获异常并返回统一格式的 JSON 数据；第四种是 Spring Boot 默认的 BasicErrorController，当没有其他异常处理器时，会处理 /error 请求并返回默认错误信息。

底层上，Spring MVC 通过 HandlerExceptionResolver 机制处理异常。DispatcherServlet 捕获异常后，会依次调用各个异常解析器，其中 ExceptionHandlerExceptionResolver 会查找 @ExceptionHandler 方法进行处理。如果都无法处理，则由 BasicErrorController 返回默认错误响应。企业项目中通常会定义业务异常（如 BusinessException），再结合全局异常处理器，实现统一的错误码和错误信息返回，便于前后端统一处理。



### 使用 @ExceptionHandler  
 只能处理当前Controller  

```java
@RestController
public class UserController {

    @ExceptionHandler(Exception.class)
    public String handler(Exception e){
        return "服务器异常";
    }
}
```

###  使用@ControllerAdvice  
 所有Controller 发生异常都会进入 GlobalExceptionHandler  

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public Result error(Exception e){

        return Result.fail(e.getMessage());

    }

}
```

###  ErrorController  
 Spring Boot还提供  BasicErrorController ， 当没有任何异常处理器时  ， 最终都会进入：  /error

```json
{
  "timestamp":"",
  "status":500,
  "error":"Internal Server Error"
}
```



### 异常处理流程
 **业务角度**：Spring Boot 异常处理标准流程图  

<!-- 这是一个文本绘图，源码为：flowchart TD
    Req([1. 发送请求]) --> Controller[2. 执行业务逻辑]
    Controller --> HasError{3. 是否抛出异常？}
    
    HasError -- 无异常 --> Success([返回正常结果])
    
    HasError -- 抛出异常 --> AdviseCheck{4. 是否定义<br/>全局异常处理器？}

    %% 左右并行分支，压缩纵向高度
    AdviseCheck -- 是 --> MatchHandler{5. 是否匹配<br/>@ExceptionHandler？}
    AdviseCheck -- 否 --> FilterChain

    MatchHandler -- 匹配成功 --> ExecHandler[6. 执行处理方法]
    ExecHandler --> FormattedResp[7. 封装响应结果<br/>如 Result.fail]
    FormattedResp --> RespToClient([8. 返回 JSON 结果])

    %% 默认兜底分支
    MatchHandler -- 匹配失败 --> FilterChain[9. 向上抛至<br/>Servlet 容器]
    FilterChain --> BasicErrorController[10. 转发至默认<br/>BasicErrorController]
    
    BasicErrorController --> ReqType{11. 判断 Accept<br/>请求头类型}
    ReqType -- HTML --> WhiteLabel[12. 渲染默认<br/>错误页面]
    ReqType -- JSON --> DefaultJson[13. 返回默认<br/>JSON 结构]
    
    WhiteLabel --> RespToClient
    DefaultJson --> RespToClient

    %% 样式调整
    style Req fill:#f9f,stroke:#333,stroke-width:2px
    style RespToClient fill:#bbf,stroke:#333,stroke-width:2px
    style HasError fill:#fbe,stroke:#333 -->
![](https://cdn.nlark.com/yuque/__mermaid_v3/abe5ac8b5b1f9384b33448d4b07ae272.svg)



 **源码层级：**DispatcherServlet 内部异常处理机制（时序图）  

<!-- 这是一个文本绘图，源码为：sequenceDiagram
    autonumber
    actor Client as 客户端
    participant DS as DispatcherServlet
    participant Controller as Controller 业务方法
    participant CompositeResolver as HandlerExceptionResolverComposite
    participant CustomResolver as ExceptionHandlerExceptionResolver
    participant BasicController as BasicErrorController

    Client->>DS: 1. 发起 HTTP 请求
    DS->>Controller: 2. 通过 HandlerAdapter 调用目标方法
    
    rect rgb(255, 235, 235)
        Controller-->>DS: 3. 业务代码抛出 Exception
    end

    DS->>DS: 4. 捕获异常，调用 processHandlerException()
    DS->>CompositeResolver: 5. 遍历 HandlerExceptionResolver 链

    rect rgb(235, 245, 255)
        CompositeResolver->>CustomResolver: 6. 优先交给标注了 @ExceptionHandler 的解析器
        alt 找到匹配的 @ExceptionHandler
            CustomResolver->>CustomResolver: 7. 执行异常处理方法并序列化结果
            CustomResolver-->>DS: 8. 返回已处理的 ModelAndView / ResponseBody
            DS-->>Client: 9. 返回自定义错误 JSON/视图
        else 未找到匹配的处理器
            CustomResolver-->>CompositeResolver: 返回 null
            CompositeResolver-->>DS: 最终无匹配的 Resolver 处理
        end
    end

    rect rgb(245, 245, 245)
        DS-->>Client: 10. 抛出原始异常给 Servlet 容器
        Client->>BasicController: 11. 容器重定向/转发到 /error
        BasicController-->>Client: 12. 返回默认 Whitelabel 页面或默认 JSON
    end -->
![](https://cdn.nlark.com/yuque/__mermaid_v3/cfbd78056fb32819bb4e45b0d9d358ed.svg)







## **<font style="color:rgb(51,51,51);">如何理解 Spring Boot 中的 Starters？</font>**
Starter 是 Spring Boot 提供的一种依赖管理机制，本质上是一组针对某项功能的依赖描述符。开发者只需要引入一个 Starter，就可以获得该功能所需的全部依赖，而不需要手动管理大量 Jar 包及其版本。例如 spring-boot-starter-web 会自动引入 Spring MVC、内嵌 Tomcat、Jackson、Validation 等依赖。

Starter 不仅负责依赖管理，还会结合 Spring Boot 的自动配置机制发挥作用。当应用启动时，@EnableAutoConfiguration 会加载对应的自动配置类，例如 WebMvcAutoConfiguration，并在满足条件时自动注册相关 Bean，实现开箱即用。

此外，Spring Boot 通过统一的依赖版本管理减少了依赖冲突，企业中还可以根据自身业务封装自定义 Starter，将 Redis、JWT、日志、统一异常处理等公共能力打包，提升项目的复用性和开发效率。

 Starter 的优点  ：

+ **简化依赖管理**：无需手动引入大量相关依赖。
+ **自动配置**：根据项目中已有的类和配置，自动创建所需 Bean。 
+ **统一版本管理**：依赖版本由 Spring Boot 管理，减少版本冲突。 
+ **开箱即用**：引入 Starter 后，通常只需少量配置即可使用相关功能。 
+ **便于扩展**：可以根据业务需要开发自定义 Starter，沉淀团队通用能力。



## **<font style="color:rgb(51,51,51);">SpringBoot 实现热部署有哪几种方式？</font>**
Spring Boot 实现热部署主要有四种方式。

第一种是官方提供的 **Spring Boot DevTools**，也是最常用的方式。它通过监听 Class 文件变化，在代码修改后自动重启应用上下文，从而实现快速生效。为了提高速度，它采用双 ClassLoader 机制，只重新加载应用代码，而不会重新加载第三方依赖。

第二种是 **JRebel**，它是一款商业热部署工具，可以直接替换运行中的字节码，很多情况下无需重启应用，热更新能力更强。

第三种是 **IDEA 自动编译配合 DevTools**，这是企业开发中最常见的组合。IDEA 自动编译生成新的 Class 文件，DevTools 检测到变化后自动重启应用。

第四种是 **DCEVM 配合 HotswapAgent**，它增强了 JVM 的 HotSwap 能力，支持更多类型的类结构修改，但配置较复杂，使用相对较少。

在实际开发中，我通常会使用 **IDEA 自动编译 + Spring Boot DevTools**，配置简单、免费，而且已经能够满足大多数 Spring Boot 项目的开发需求。

###  Spring Boot DevTools  
添加依赖

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-devtools</artifactId>
  <scope>runtime</scope>
  <optional>true</optional>
</dependency>
```

IDEA设置中开启Build Project Automatically、 compiler.automake.allow.when.app.running  

**工作原理**： 不修改运行中的代码，自动重启应用

     采用两个 ClassLoader ：

BaseClassLoader

       │

       ├── 第三方Jar（不会重新加载）

       │

RestartClassLoader

       │

       ├── 自己写的代码（重新加载）



#### **Spring Boot DevTools 是如何实现热部署的？**
DevTools 并不是真正意义上的热替换，它的实现方式是**自动重启应用**。它会监听项目中 Class 文件的变化，当检测到代码重新编译后，会自动关闭当前 ApplicationContext，再重新创建一个新的 ApplicationContext，从而让修改后的代码生效。

为了提高重启速度，DevTools 使用了双 ClassLoader 机制：一个 BaseClassLoader 加载第三方 Jar 包，这部分不会重新加载；另一个 RestartClassLoader 加载项目自己的类，每次代码变更时只重新加载这一部分，因此比完整重启 JVM 快很多。

####  为什么 DevTools 启动这么快？  
DevTools 启动速度快，是因为它并没有重新启动整个 JVM，而是采用了**双 ClassLoader**机制。它将类分成两部分：

+ **BaseClassLoader**：负责加载 Spring、Tomcat、MyBatis、Jackson 等第三方依赖，这些类在整个开发过程中基本不会变化，因此只加载一次。 
+ **RestartClassLoader**：负责加载项目中的 Controller、Service、Mapper 等业务代码。当代码发生变化时，只销毁并重新创建 `RestartClassLoader`，重新加载业务类，同时重新创建 Spring 容器，而第三方依赖无需再次加载。

###  JRebel  
 真正意义上的热替换 ，直接替换字节码  

###  IDEA 自动编译 + DevTools  
 IDEA负责 保存代码 、自动编译  

 DevTools负责 检测class变化 、自动Restart  

![画板](https://cdn.nlark.com/yuque/0/2026/jpeg/70336156/1784525980218-97de43a8-15dc-4ef9-bb61-9764299dd09e.jpeg)

###  DCEVM + HotswapAgent  
 只能修改方法体，不能增加字段、增加方法、 修改类结构  



## **<font style="color:rgb(51,51,51);">如何理解 Spring Boot 配置加载顺序？</font>**
Spring Boot 支持外部化配置，可以从配置文件、环境变量、JVM 参数、命令行参数等多个来源加载配置。

当同一个配置项在多个地方存在时，会按照优先级进行覆盖，一般来说，命令行参数优先级最高，其次是 JVM 参数、系统环境变量、外部配置文件，最后是项目中的 `application.yml` 或 `application.properties`。

此外，Spring Boot 还支持 Profile 配置，例如 `application-dev.yml`、`application-prod.yml`，激活对应环境后，环境配置会覆盖默认配置，便于开发、测试和生产环境使用不同参数。

底层上，Spring Boot 会在启动时创建 `Environment`，并将各类配置封装成 `PropertySource`，按照优先级加入到 `MutablePropertySources` 中。应用获取配置时，会按顺序查找，因此高优先级配置能够覆盖低优先级配置。

### application.properties 和 application.yml 可以同时存在吗？哪个优先？
可以同时存在。如果位于同一位置且定义了相同配置项，通常 .properties 的优先级高于 .yml

### application.yml 和 application-dev.yml 的加载顺序是什么？
先加载默认配置 application.yml，再加载当前激活环境对应的 application-{profile}.yml，相同配置由后者覆盖

###  如何切换不同环境（dev、test、prod）？  
Spring Boot 可以通过多种方式切换运行环境。最常见的是在配置文件中通过 spring.profiles.active 指定，也可以通过 JVM 参数 -Dspring.profiles.active=prod、命令行参数 --spring.profiles.active=prod 或环境变量 SPRING_PROFILES_ACTIVE=prod 来指定，其中命令行参数优先级**最高**。

当激活某个 Profile 后，Spring Boot 会先加载默认配置 application.yml，再加载对应的 application-{profile}.yml，相同配置项由后者覆盖。这样就可以为开发、测试和生产环境维护不同的数据库、端口、Redis 等配置，而无需修改代码。在企业中，部署到 Docker 或 Kubernetes 时，通常通过环境变量或启动参数来切换环境，而不是直接修改配置文件

```yaml
spring:
  profiles:
    active: dev
```

```bash
java -Dspring.profiles.active=prod -jar app.jar
```

```bash
java -jar app.jar --spring.profiles.active=prod
```

```bash
//Linux
export SPRING_PROFILES_ACTIVE=prod

//windows
SPRING_PROFILES_ACTIVE=prod
```

```yaml
environment:
  SPRING_PROFILES_ACTIVE=prod
```

### 除了 `application-dev.yml`，还能不能在一个 `application.yml` 中同时配置多个环境？  
可以。

Spring Boot 支持在一个 `application.yml` 中使用 YAML 多文档语法，通过 `---` 分隔多个配置块，再结合 `spring.config.activate.on-profile` 指定对应的环境。例如可以在同一个文件中分别定义 `dev`、`test` 和 `prod` 的配置，启动时通过 `spring.profiles.active` 激活对应环境即可。  
不过在实际企业开发中，更常见的做法还是使用 `application.yml` 存放公共配置，再分别使用 `application-dev.yml`、`application-test.yml` 和 `application-prod.yml` 存放各环境的差异化配置，这样结构更清晰，也更便于团队维护和版本管理  

###  多个 Profile 能不能一起激活？  
Spring Boot 支持同时激活多个 Profile，可以通过 `spring.profiles.active=dev,test` 或启动参数 `--spring.profiles.active=dev,test` 来指定。

启动时会先加载默认的 `application.yml`，然后按照**指定顺序**依次加载 `application-dev.yml`、`application-test.yml`。对于不同的配置项会进行合并，而对于相同的配置项，后加载的 Profile 会**覆盖**前面的配置。实际开发中，这种机制常用于配置复用，比如将公共配置、开发环境配置和本地个性化配置组合使用，提高配置的灵活性。  

### @Value 和 @ConfigurationProperties 获取配置有什么区别？
@Value 和 @ConfigurationProperties 都可以读取配置文件中的属性，但适用场景不同。

@Value 更适合读取单个配置项，例如数据库用户名或端口号，需要通过 ${} 指定每一个属性；如果配置项较多，就需要写很多 @Value 注解，维护起来比较麻烦。

@ConfigurationProperties 则可以根据前缀一次性将一组配置绑定到一个 Java 对象，还支持对象嵌套、List、Map 等复杂类型，并且具备类型转换和 JSR-303 数据校验能力。因此，在企业开发中，如果是一组相关配置，通常推荐使用 @ConfigurationProperties；如果只是读取一两个配置项，则使用 @Value 更简单



## **<font style="color:rgb(51,51,51);">Spring Boot 的核心配置文件有哪几个？它们的区别是什么？</font>**
如果只是 Spring Boot，本身支持的核心配置文件是 application.properties 和 application.yml（或 application.yaml）。

如果是 Spring Cloud 老版本项目，还会使用 bootstrap.yml。它会比 application.yml 更早加载，主要用于配置应用名称、配置中心地址等启动阶段就需要的信息。

不过从 Spring Boot 2.4 和 Spring Cloud 2020 开始，官方已经不再默认使用 bootstrap.yml，推荐通过 spring.config.import 来导入外部配置，因此现在很多新项目只有 application.yml



## 如何集成 Spring Boot 和 RabbitMQ？
首先引入 spring-boot-starter-amqp 依赖，然后在 application.yml 中配置 RabbitMQ 的地址、端口、用户名和密码。接着通过配置类声明 Exchange、Queue 和 Binding。

发送消息时，可以直接使用 Spring Boot 自动配置好的 RabbitTemplate 调用 convertAndSend() 方法；消费消息时，只需要在方法上使用 @RabbitListener 注解监听指定队列即可，收到消息后会自动回调对应方法。

底层是因为 Spring Boot 提供了 RabbitAutoConfiguration 自动配置类，它会根据配置文件创建 ConnectionFactory、RabbitTemplate 和监听容器等 Bean，因此开发者几乎不需要编写底层连接代码，只需关注业务逻辑即可



##  Exchange 有哪几种类型？  
<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/70336156/1784547423830-347fb5e9-9af7-448b-91ed-29a9138bc59a.png)

RabbitMQ 的 Exchange 主要有四种类型：Direct、Topic、Fanout 和 Headers。

+ **Direct Exchange** 根据 RoutingKey 精确匹配进行消息路由，是实际开发中最常用的类型。 
+ **Topic Exchange** 支持通配符匹配，其中 `*` 表示匹配一个单词，`#` 表示匹配零个或多个单词，适用于日志分类、事件通知等需要灵活路由的场景。 
+ **Fanout Exchange** 会将消息广播到所有绑定的队列，不关心 RoutingKey，适用于群发通知、缓存同步等广播场景。 
+ **Headers Exchange** 根据消息头（Header）而不是 RoutingKey 进行匹配，使用相对较少。 

在实际企业项目中，**Direct 和 Topic 使用最广泛**，Fanout 用于广播，而 Headers 一般较少使用



##  一个 Queue 可以绑定多个 Exchange 吗？  
可以。RabbitMQ 中 Queue 和 Exchange 是**多对多**关系。一个 Queue 可以绑定多个 Exchange，从不同的 Exchange 接收消息；一个 Exchange 也可以绑定多个 Queue，将消息路由到多个队列。它们之间通过 Binding 建立关联，Exchange 根据自身类型（如 Direct、Topic、Fanout）和 BindingKey 或 Header 规则决定消息路由。实际开发中，一个日志队列同时绑定多个 Exchange 来汇总不同业务系统的日志，就是比较常见的应用场景。  

