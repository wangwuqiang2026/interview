## Spring Bean的生命周期
<!-- 这是一个文本绘图，源码为：flowchart TD
    A[1. 实例化 Instantiation<br>调用构造函数/工厂方法] --> B[2. 属性赋值 Populate Properties<br>依赖注入 @Autowired / Setter]
    
    B --> C{3. Aware 接口回调}
    C -->|实现 BeanNameAware| C1[setBeanName]
    C -->|实现 BeanFactoryAware| C2[setBeanFactory]
    C -->|实现 ApplicationContextAware| C3[setApplicationContext]
    
    C1 --> D[4. BeanPostProcessor 前置处理<br>postProcessBeforeInitialization]
    C2 --> D
    C3 --> D
    
    D --> E{5. 初始化 Initialization}
    E -->|标注 @PostConstruct| E1[执行 @PostConstruct 方法]
    E1 -->|实现 InitializingBean| E2[执行 afterPropertiesSet]
    E2 -->|配置 init-method| E3[执行自定义 init-method]
    
    E3 --> F[6. BeanPostProcessor 后置处理<br>postProcessAfterInitialization<br>在此阶段生成 AOP 动态代理]
    
    F --> G[7. Bean 就绪状态<br>可以被应用程序正常使用]
    
    G --> H{8. 销毁阶段 Destruction<br>容器关闭时触发}
    H -->|标注 @PreDestroy| H1[执行 @PreDestroy 方法]
    H1 -->|实现 DisposableBean| H2[执行 destroy 方法]
    H2 -->|配置 destroy-method| H3[执行自定义 destroy-method] -->
![](https://cdn.nlark.com/yuque/__mermaid_v3/0e24b6ff3c998ad54d4b9198a9f02b1a.svg)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/70336156/1784423674245-55c4d258-34fb-45ce-99b1-1db6afd439ed.png)

**精简版：**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/70336156/1784423735874-2725fbc1-3659-4dd6-9cba-8ea1cb1590ce.png)



###  Spring Bean 生命周期中最重要的是哪一步？  
我认为最重要的是 **BeanPostProcessor 的后置处理（postProcessAfterInitialization）**。因为 Spring AOP 就是在这里为 Bean 创建代理对象，所以像 `@Transactional`、`@Async`、`@Cacheable` 等功能，最终都是通过这里生成的代理对象来实现的。  

###  BeanPostProcessor 有什么作用？  
它允许开发者在 Bean 初始化前后对 Bean 进行增强或替换，是 Spring 提供的重要扩展点。Spring 自己大量使用了它，例如 AOP、自动注入、配置属性绑定等功能都依赖 BeanPostProcessor  



## **<font style="color:rgb(51,51,51);">Spring支持的几种bean的作用域</font>**
**<font style="color:rgb(51,51,51);">singleton</font>**<font style="color:rgb(51,51,51);">：默认，每个容器中只有一个bean的实例，单例的模式由BeanFactory自身来维 </font>

<font style="color:rgb(51,51,51);">护。</font>

**<font style="color:rgb(51,51,51);">prototype</font>**<font style="color:rgb(51,51,51);">：为每一个bean请求提供一个实例。</font>

**<font style="color:rgb(51,51,51);">request</font>**<font style="color:rgb(51,51,51);">：为每一个网络请求创建一个实例，在请求完成以后，bean会失效并被垃圾回收器回 </font>

<font style="color:rgb(51,51,51);">收。</font>

**<font style="color:rgb(51,51,51);">session</font>**<font style="color:rgb(51,51,51);">：与request范围类似，确保每个session中有一个bean的实例，在session过期后， </font>

<font style="color:rgb(51,51,51);">bean会随之失效。</font>

**<font style="color:rgb(51,51,51);">global-session</font>**<font style="color:rgb(51,51,51);">：全局作用域，global-session和Portlet应用相关。当你的应用部署在Portlet </font>

<font style="color:rgb(51,51,51);">容器中工作时，它包含很多</font><font style="color:rgb(51,51,51);">portlet</font><font style="color:rgb(51,51,51);">。如果你想要声明让所有的</font><font style="color:rgb(51,51,51);">portlet</font><font style="color:rgb(51,51,51);">共用全局的存储变量的话，那 </font>

<font style="color:rgb(51,51,51);">么这全局变量需要存储在global-session中。全局作用域与Servlet中的session作用域效果相同。</font>

<font style="color:rgb(51,51,51);"></font>



## **<font style="color:rgb(51,51,51);">Autowired和Resource关键字的区别？</font>**
`@Autowired` 是 **Spring 提供的注解**，默认按照 **类型（byType）** 进行注入，如果容器中存在多个相同类型的 Bean，可以结合 `@Qualifier` 指定 Bean 名称。它支持构造器注入、Setter 注入和字段注入，是 Spring 官方推荐的注入方式。

`@Resource` 是 **Java 标准规范 JSR-250 提供的注解**，默认按照 **名称（byName）** 进行注入，如果根据名称找不到对应 Bean，再按照类型（byType）匹配。它主要用于字段注入和 Setter 方法注入。





## **为什么Spring推荐构造器注入，而不是@Autowired字段注入？**
主要原因是**构造器注入**能够保证 Bean 创建时依赖一定存在、对象不可变、代码更容易测试和维护。

字段注入的问题是：对象创建后，Spring 通过反射直接给字段赋值，导致对象在构造完成时可能处于不完整态，而且依赖关系隐藏在字段上，不容易发现类到底依赖哪些组件。同时字段注入无法用于 final 修饰的字段，降低了代码的不可变性。

而构造器注入是在创建 Bean 时，通过构造方法传入依赖，如果缺少依赖，Bean 根本无法创建，可以提前发现问题。另外构造器注入明确表达了一个类需要哪些依赖，更符合面向对象设计原则，也方便单元测试时直接传入 Mock 对象。





## <font style="color:rgb(51,51,51);">@Resource装配顺序</font>
<font style="color:rgb(51,51,51);">①如果同时指定了name和type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛出异常。 </font>

<font style="color:rgb(51,51,51);">②如果指定了</font><font style="color:rgb(51,51,51);">name</font><font style="color:rgb(51,51,51);">，则从上下文中查找名称（</font><font style="color:rgb(51,51,51);">id</font><font style="color:rgb(51,51,51);">）匹配的</font><font style="color:rgb(51,51,51);">bean</font><font style="color:rgb(51,51,51);">进行装配，找不到则抛出异常。 </font>

<font style="color:rgb(51,51,51);">③如果指定了type，则从上下文中找到类似匹配的唯一bean进行装配，找不到或是找到多个，都会抛出异常。 </font>

<font style="color:rgb(51,51,51);">④如果既没有指定name，又没有指定type，则自动按照byName方式进行装配；如果没有匹配，则回退为一个原始类型进行匹配，如果匹配则自动装配。 </font>

<font style="color:rgb(51,51,51);">@Resource的作用相当于@Autowired，只不过@Autowired按照byType自动注入。</font>

<font style="color:rgb(51,51,51);"></font>

<font style="color:rgb(51,51,51);"></font>

## **<font style="color:rgb(51,51,51);">SpringMVC常用的注解有哪些？</font>**
| 分类 | 注解 | 作用 | 使用场景 |
| --- | --- | --- | --- |
| **控制器声明** | `@Controller` | 标识一个类为 Spring MVC 控制器 | 返回页面（Thymeleaf、JSP等） |
|  | `@RestController` | 等价于 `@Controller + @ResponseBody` | 开发 RESTful 接口，返回 JSON |
|  | `@RequestMapping` | 映射请求路径 | 类和方法上定义 URL |
| **请求映射** | `@GetMapping` | 处理 GET 请求 | 查询数据 |
|  | `@PostMapping` | 处理 POST 请求 | 新增数据 |
|  | `@PutMapping` | 处理 PUT 请求 | 修改数据 |
|  | `@DeleteMapping` | 处理 DELETE 请求 | 删除数据 |
|  | `@PatchMapping` | 处理 PATCH 请求 | 部分更新数据 |
| **请求参数** | `@RequestParam` | 获取请求参数 | 接收 URL 参数（?id=1） |
|  | `@PathVariable` | 获取路径变量 | 接收 RESTful 路径参数（/user/1） |
|  | `@RequestBody` | JSON 转 Java 对象 | 接收前端 JSON 请求 |
|  | `@RequestHeader` | 获取请求头参数 | 获取 Token、Cookie 信息 |
|  | `@CookieValue` | 获取 Cookie 参数 | 获取用户 Cookie |
| **返回数据** | `@ResponseBody` | 将返回值直接写入响应体 | 返回 JSON 数据 |
|  | `@ResponseStatus` | 设置 HTTP 状态码 | 自定义响应状态 |
| **数据校验** | `@Valid` | 开启参数校验 | 校验请求对象字段 |
|  | `@Validated` | Spring 提供的增强校验 | 分组校验、方法校验 |
| **异常处理** | `@ExceptionHandler` | 处理指定异常 | Controller 内异常处理 |
|  | `@ControllerAdvice` | 全局异常处理 | 统一处理项目异常 |
|  | `@RestControllerAdvice` | `@ControllerAdvice + @ResponseBody` | 全局返回 JSON 异常 |
| **跨域处理** | `@CrossOrigin` | 处理跨域请求 | 前后端分离项目 |
| **参数绑定** | `@ModelAttribute` | 将请求参数绑定到对象 | 表单提交 |
|  | `@SessionAttribute` | 获取 Session 中属性 | 获取会话数据 |
|  | `@RequestAttribute` | 获取 Request 域属性 | 获取请求上下文数据 |
| **文件上传** | `@RequestPart` | 接收 Multipart 文件 | 文件上传接口 |






## **<font style="color:rgb(51,51,51);">谈谈你对Spring的AOP理解</font>**
Spring AOP，也就是面向切面编程，是 Spring 用来解决横切关注点问题的一种技术。它的核心思想是在不修改原有业务代码的情况下，将一些公共功能，比如日志记录、权限校验、事务管理等代码抽取出来，通过动态代理的方式织入到目标方法执行过程中，从而实现业务代码和公共逻辑的解耦。

Spring AOP 中主要有切面、切入点、连接点、通知和目标对象几个核心概念，其中**切面**表示封装的公共逻辑，**切入点**用于指定哪些方法需要增强，**通知**表示增强逻辑执行的时机，比如方法执行前、执行后或者异常时执行。

从底层实现来看，Spring AOP 主要依赖动态代理，如果目标类实现了接口，Spring 默认使用 JDK 动态代理；如果没有实现接口，则使用 CGLIB 动态代理生成代理对象。当调用代理对象的方法时，会先执行 AOP 增强逻辑，再调用目标对象的方法。

比如 Spring 中的 `@Transactional` 注解，本质上就是通过 AOP 实现的，代理对象会在方法执行前开启事务，执行成功后提交事务，发生异常时进行回滚。

```java
@Around("execution(* com.demo.service.*.*(..))")
public Object around(ProceedingJoinPoint point){

    System.out.println("方法执行前");

    Object result = point.proceed();

    System.out.println("方法执行后");

    return result;
}
```

| 概念 | 说明 | 举例 |
| --- | --- | --- |
| Aspect（切面） | 封装横切逻辑的类 | 日志切面 |
| JoinPoint（连接点） | 可以被增强的位置 | 方法执行 |
| Pointcut（切入点） | 定义哪些方法需要增强 | 所有Service方法 |
| Advice（通知） | 增强逻辑执行时机 | 方法前执行 |
| Target（目标对象） | 被代理对象 | UserService |
| Proxy（代理对象） | Spring生成的增强对象 | UserService代理类 |


通知类型：

| 类型 | 注解 | 执行时机 | 使用场景 |
| --- | --- | --- | --- |
| 前置通知 | @Before | 方法执行前 | 权限校验 |
| 后置通知 | @After | 方法结束后 | 资源释放 |
| 返回通知 | @AfterReturning | 正常返回后 | 记录结果 |
| 异常通知 | @AfterThrowing | 异常发生后 | 异常日志 |
| 环绕通知 | @Around | 方法执行前后 | 事务、性能监控 |


 **Spring AOP 本质**： 通过动态代理生成一个代理对象，在代理对象中加入增强逻辑。 

<!-- 这是一个文本绘图，源码为：flowchart TD
    A[客户端调用] --> B[代理对象 Proxy]
    B --> C[执行 AOP 增强逻辑]
    C --> D[调用目标对象 Target]
    D --> E[返回结果] -->
![](https://cdn.nlark.com/yuque/__mermaid_v3/07ac16363eca8569e300699d731a0556.svg)

<font style="color:rgb(51,51,51);"></font>

<font style="color:rgb(51,51,51);"></font>

## <font style="color:rgb(51,51,51);"> Spring AOP 和 AspectJ区别  </font>
| <font style="color:rgb(51,51,51);">区别</font> | <font style="color:rgb(51,51,51);">Spring AOP</font> | <font style="color:rgb(51,51,51);">AspectJ</font> |
| --- | --- | --- |
| <font style="color:rgb(51,51,51);">实现方式</font> | <font style="color:rgb(51,51,51);">动态代理</font> | <font style="color:rgb(51,51,51);">字节码织入</font> |
| <font style="color:rgb(51,51,51);">织入时间</font> | <font style="color:rgb(51,51,51);">运行时</font> | <font style="color:rgb(51,51,51);">编译期/类加载期</font> |
| <font style="color:rgb(51,51,51);">支持范围</font> | <font style="color:rgb(51,51,51);">方法级</font> | <font style="color:rgb(51,51,51);">更全面</font> |
| <font style="color:rgb(51,51,51);">性能</font> | <font style="color:rgb(51,51,51);">相对低</font> | <font style="color:rgb(51,51,51);">更高</font> |
| <font style="color:rgb(51,51,51);">使用难度</font> | <font style="color:rgb(51,51,51);">简单</font> | <font style="color:rgb(51,51,51);">复杂</font> |






## **<font style="color:rgb(51,51,51);">说说你对Spring的IOC是怎么理解的？</font>**
IOC是 Spring 框架的核心思想，它将对象的创建、依赖管理和生命周期管理交给 Spring 容器负责，而不是由程序员主动创建和维护对象之间的依赖关系。

IOC 的核心实现就是 **IOC 容器**，Spring 中主要通过 `BeanFactory` 和 `ApplicationContext` 两种容器来实现。其中 `ApplicationContext` 是实际项目中更常用的容器，它负责 Bean 的创建、初始化、依赖注入、生命周期管理等。  

Spring 实现 IOC 主要依靠 依赖注入，常见方式有构造器注入、Setter 注入和字段注入。比如一个 Service 依赖 Dao，Spring 会根据配置自动找到对应的 Dao 实例，并注入到 Service 中。

**Spring IOC 的优势 ：**

| 优势 | 说明 |
| --- | --- |
| 降低耦合 | 对象不需要主动创建依赖对象 |
| 提高扩展性 | 更换实现类更加方便 |
| 方便测试 | 可以注入 Mock 对象 |
| 生命周期管理 | Spring 统一管理 Bean 创建、销毁 |
| 支持 AOP | AOP 代理对象依赖 IOC 创建和管理 |
| 支持事务 | `@Transactional` 基于 IOC Bean 代理实现 |






## Spring IOC 的实现原理是什么？  
<font style="color:rgb(51,51,51);">Spring IOC 的实现原理主要是通过</font>**控制反转容器 + 依赖注入 + Bean 生命周期管理**<font style="color:rgb(51,51,51);">来实现的。</font>

<font style="color:rgb(51,51,51);">Spring 启动时会创建 IOC 容器，通过扫描配置文件或者注解获取 Bean 的定义信息，然后根据 BeanDefinition 创建和管理 Bean 对象。创建过程中，Spring 会通过反射实例化对象，并根据依赖关系完成属性注入，比如通过构造器注入、Setter 注入或者字段注入。为了管理 Bean 的生命周期，Spring 还提供了 BeanPostProcessor 等扩展机制，在 Bean 初始化前后执行相关处理。同时，Spring 通过三级缓存解决单例 Bean 的循环依赖问题。最终，应用程序不再主动创建对象，而是从 IOC 容器中获取已经创建好的 Bean，实现了对象创建和业务逻辑的解耦。  </font>

<!-- 这是一个文本绘图，源码为：flowchart TD
    subgraph Phase1 [1. 容器启动与解析]
        Start([Spring 启动]) --> CreateContainer[创建 IOC 容器]
        CreateContainer --> Scan[扫描配置文件 / 注解]
        Scan --> GetBD[获取 Bean 定义信息 BeanDefinition]
    end

    subgraph Phase2 [2. Bean 的创建与依赖注入]
        GetBD --> ReflectInst[反射实例化对象]
        ReflectInst --> CircularCheck{是否存在循环依赖?}
        
        CircularCheck -->|是| SolveCircular[通过三级缓存解决循环依赖]
        CircularCheck -->|否| Inject[进行依赖注入<br>构造器/Setter/字段注入]
        SolveCircular --> Inject
    end

    subgraph Phase3 [3. Bean 生命周期管理]
        Inject --> BPPBefore[BeanPostProcessor 前置处理]
        BPPBefore --> Init[执行 Bean 初始化逻辑]
        Init --> BPPAfter[BeanPostProcessor 后置处理]
    end

    subgraph Phase4 [4. 应用解耦使用]
        BPPAfter --> Ready[Bean 交付容器管理]
        Ready --> AppFetch[应用程序从 IOC 容器获取 Bean]
        AppFetch --> End([实现对象创建与业务逻辑解耦])
    end -->
![](https://cdn.nlark.com/yuque/__mermaid_v3/de90e7773079419d51a0ed9a3f67fcd9.svg)





## Spring IOC 和 DI 有什么区别？  
IOC（控制反转）是一种设计思想，它表示将对象的创建和管理权从业务代码交给 Spring 容器管理；DI（依赖注入）是 IOC 的具体实现方式，Spring 通过依赖注入将对象所需要的依赖自动传递给它。简单来说，IOC 解决的是“对象由谁创建和管理”的问题，而 DI 解决的是“对象之间的依赖如何传递”的问题。在 Spring 中，IOC 容器通过管理 Bean 的生命周期，并利用构造器注入、Setter 注入、字段注入等方式完成依赖注入，从而实现对象之间的解耦。

<font style="color:rgb(51,51,51);"></font>

<font style="color:rgb(51,51,51);"></font>

## **<font style="color:rgb(51,51,51);">Spring框架中都用到了哪些设计模式</font>**
**<font style="color:rgb(51,51,51);">简单工厂模式</font>**<font style="color:rgb(51,51,51);">：Spring 中的 BeanFactory 就是简单工厂模式的体现。根据传入一个唯一的标识来获得 Bean 对象，但是在传入参数后创建还是传入参数前创建，要根据具体情况来定。 </font>

**<font style="color:rgb(51,51,51);">工厂模式</font>**<font style="color:rgb(51,51,51);">：Spring 中的 FactoryBean 就是典型的工厂方法模式，实现了 FactoryBean 接口的 bean 是一类叫做 factory 的 bean。其特点是，spring 在使用 getBean() 调用获得该 bean 时，会自动调 用该 bean 的 getObject() 方法，所以返回的不是 factory 这个 bean，而是这个 bean.getOjbect() 方法的返回值。 </font>

**<font style="color:rgb(51,51,51);">单例模式</font>**<font style="color:rgb(51,51,51);">：在 spring 中用到的单例模式有： scope="singleton" ，注册式单例模式，bean 存放于 Map 中。bean name当做 key，bean 当做 value。 </font>

**<font style="color:rgb(51,51,51);">原型模式</font>**<font style="color:rgb(51,51,51);">：在 spring 中用到的原型模式有： scope="prototype" ，每次获取的是通过克隆生成的新 实例，对其进行修改时对原有实例对象不造成任何影响。 </font>

**<font style="color:rgb(51,51,51);">迭代器模式</font>**<font style="color:rgb(51,51,51);">：在 Spring 中有个 CompositeIterator 实现了 Iterator，Iterable 接口和 Iterator 接 口，这两个都是迭代相关的接口。可以这么认为，实现了 Iterable 接口，则表示某个对象是可被迭 代的。Iterator 接口相当于是一个迭代器，实现了 Iterator 接口，等于具体定义了这个可被迭代的 对象时如何进行迭代的。 </font>

**<font style="color:rgb(51,51,51);">代理模式</font>**<font style="color:rgb(51,51,51);">：</font><font style="color:rgb(51,51,51);">Spring </font><font style="color:rgb(51,51,51);">中经典的</font><font style="color:rgb(51,51,51);"> AOP</font><font style="color:rgb(51,51,51);">，就是使用动态代理实现的，分</font><font style="color:rgb(51,51,51);"> JDK </font><font style="color:rgb(51,51,51);">和</font><font style="color:rgb(51,51,51);"> CGlib </font><font style="color:rgb(51,51,51);">动态代理。 </font>

**<font style="color:rgb(51,51,51);">适配器模式</font>**<font style="color:rgb(51,51,51);">：Spring 中的 AOP 中 AdvisorAdapter 类，它有三个实现： MethodBeforAdviceAdapter、AfterReturnningAdviceAdapter、ThrowsAdviceAdapter。Spring 会根据不同的 AOP 配置来使用对应的 Advice，与策略模式不同的是，一个方法可以同时拥有多个 Advice。Spring 存在很多以 Adapter 结尾的，大多数都是适配器模式。 </font>

**<font style="color:rgb(51,51,51);">观察者模式</font>**<font style="color:rgb(51,51,51);">：Spring 中的 Event 和 Listener。spring 事件：ApplicationEvent，该抽象类继承了 EventObject 类，JDK 建议所有的事件都应该继承自 EventObject。spring 事件监听器： ApplicationListener，该接口继承了 EventListener 接口，JDK 建议所有的事件监听器都应该继承 EventListener。 </font>

**<font style="color:rgb(51,51,51);">模板模式</font>**<font style="color:rgb(51,51,51);">：Spring 中的 org.springframework.jdbc.core.JdbcTemplate 就是非常经典的模板模式的应用，里面的 execute 方法，把整个算法步骤都定义好了</font>

<font style="color:rgb(51,51,51);"></font>

### 代理模式
 Spring AOP 主要有两种代理方式：**JDK** 动态代理和 **CGLIB **动态代理。

JDK 动态代理基于接口实现，要求目标类必须实现接口；CGLIB 动态代理基于继承实现，通过生成目标类的子类并重写方法来织入增强逻辑，因此不需要接口，但不能代理 `final` 类和 `final` 方法。

Spring 默认会根据目标对象是否实现接口来选择代理方式：实现了接口时默认使用 JDK 动态代理，没有实现接口时使用 CGLIB；如果配置了 `proxyTargetClass=true`，则会强制使用 CGLIB  

| 区别 | JDK动态代理 | CGLIB |
| --- | --- | --- |
| 实现方式 | 反射机制 | 字节码生成 |
| 代理对象 | 接口代理对象 | 目标类子类 |
| 是否需要接口 | 必须实现接口 | 不需要 |
| 底层API | java.lang.reflect.Proxy | Enhancer |
| 性能 | 较慢（早期） | 较快（早期） |
| 现代版本性能 | 差距很小 | 差距很小 |
| final类 | 支持 | 不支持 |
| final方法 | 支持 | 不支持 |
| private方法 | 支持 | 不支持 |
| Spring默认选择 | 优先使用 | 无接口时使用 |


####  为什么 Spring 要使用代理？  
 因为 Spring AOP 是基于代理实现的  



### 适配器模式
<font style="color:#000000;">Spring MVC 使用 HandlerAdapter 实现适配器模式。</font>

<font style="color:#000000;">DispatcherServlet 不直接调用 Controller，而是通过 HandlerAdapter 将不同类型的 Handler（如 Controller、HttpRequestHandler、@RequestMapping 方法等）适配为统一的 handle() 调用接口，从而实现了解耦、符合开闭原则，并方便框架扩展新的 Handler 类型。</font>

<font style="color:#000000;"></font>

### 观察则模式
<font style="color:#000000;">Spring 中观察者模式最典型的应用是事件机制（Event）。</font>

<font style="color:#000000;">当业务代码调用 ApplicationEventPublisher.publishEvent() 发布事件后，Spring 会通过 ApplicationEventMulticaster 将事件广播给所有匹配的 ApplicationListener 或 @EventListener 监听器。这样发布者无需关心监听者的数量和实现，实现了业务解耦，符合开闭原则，并具有良好的扩展性。</font>

<font style="color:#000000;"></font>

### 责任链模式
<font style="color:#000000;">Spring 中责任链模式应用非常广泛。</font>

<font style="color:#000000;">最典型的是 BeanPostProcessor，Spring 会把所有 BeanPostProcessor 组成一条链，在 Bean 初始化前后依次调用它们。</font>

<font style="color:#000000;">其次是 Spring MVC 的 HandlerInterceptor，多个拦截器按顺序执行 preHandle()，请求完成后再按逆序执行 postHandle() 和 afterCompletion()。</font>

<font style="color:#000000;">此外，Spring Security 的 SecurityFilterChain 和 Servlet FilterChain 都是责任链模式的经典实现，请求会依次经过多个过滤器，每个过滤器既可以处理请求，也可以通过 chain.doFilter() 将请求传递给下一个过滤器。这些都是 Spring 中责任链模式的典型应用。</font>

**<font style="color:#000000;">优点</font>**

<font style="color:#000000;">降低耦合：发送请求者不需要知道最终由谁处理</font>

<font style="color:#000000;">扩展方便：新增处理者只需加入责任链即可，无需修改已有代码。</font>

<font style="color:#000000;">职责单一：每个处理者只关注自己的处理逻辑。</font>

<font style="color:#000000;">符合开闭原则：增加新处理逻辑时通常无需修改原有处理者</font>

<font style="color:#000000;"></font>

<font style="color:#000000;"></font>

## **<font style="color:rgb(51,51,51);">Spring 中 ApplicationContext 和 BeanFactory 的区别</font>**
<font style="color:#000000;">BeanFactory 是 Spring 最基础的 IoC 容器，只提供 Bean 的创建、依赖注入和获取等核心功能，默认采用懒加载，即第一次调用 getBean() 时才创建 Bean。</font>

<font style="color:#000000;">ApplicationContext 是 BeanFactory 的增强版，它默认会在容器启动时创建所有单例 Bean（@Lazy 的 Bean 除外），能够提前发现配置错误。</font>

<font style="color:#000000;">此外，ApplicationContext 还提供了国际化、事件发布、资源加载、环境配置、自动注册 BeanPostProcessor 和 BeanFactoryPostProcessor、支持注解扫描等企业级功能。</font>

<font style="color:#000000;">因此，在 Spring Boot 和日常开发中基本都使用 ApplicationContext，而 BeanFactory 更多用于 Spring 底层框架和源码实现。</font>

<font style="color:#000000;"></font>

<font style="color:#000000;"></font>

## **<font style="color:rgb(51,51,51);">Spring 框架中的单例 Bean 是线程安全的么？</font>**
<font style="color:#000000;">Spring 的单例 Bean 默认并不是线程安全的。</font>

<font style="color:#000000;">Spring 只负责管理 Bean 的生命周期，并不会对 Bean 的并发访问进行同步控制。</font>

<font style="color:#000000;">如果单例 Bean 中没有可变成员变量（即无状态 Bean），那么多个线程共享它通常是线程安全的；如果 Bean 中保存了可变成员变量（即有状态 Bean），多个线程同时访问时就可能发生线程安全问题。因此，在 Spring 开发中，通常建议将单例 Bean 设计为无状态 Bean。</font>

<font style="color:#000000;"></font>

<font style="color:#000000;"></font>

## **<font style="color:rgb(51,51,51);">Spring 是怎么解决循环依赖的？</font>**
<!-- 这是一个文本绘图，源码为：flowchart TD
    Start([开始创建 Bean A]) --> A_Inst[1. 实例化 A]
    A_Inst --> A_3rd[2. A 的 ObjectFactory 存入三级缓存]
    A_3rd --> A_Pop[3. A 注入属性，发现依赖 B]
    
    A_Pop --> B_Inst[4. 实例化 B]
    B_Inst --> B_3rd[5. B 的 ObjectFactory 存入三级缓存]
    B_3rd --> B_Pop[6. B 注入属性，发现依赖 A]
    
    B_Pop --> Get_A_3rd[7. 从三级缓存获取 A 的 ObjectFactory]
    Get_A_3rd --> AOP{需要 AOP 代理?}
    
    AOP -->|是| Proxy[返回 A 的代理对象]
    AOP -->|否| Raw[返回 A 的原始半成品]
    
    Proxy --> A_2nd[8. A 的引用存入二级缓存，移出三级缓存]
    Raw --> A_2nd
    
    A_2nd --> B_Inject[9. B 注入 A 的引用，完成初始化]
    B_Inject --> B_1st[10. B 存入一级缓存，移出二/三级缓存]
    
    B_1st --> A_Inject[11. A 获取完整的 B，完成依赖注入与初始化]
    A_Inject --> A_1st[12. A 存入一级缓存，移出二/三级缓存]
    A_1st --> End([完成]) -->
![](https://cdn.nlark.com/yuque/__mermaid_v3/bd687930881999730e24560f97a2c485.svg)

<font style="color:#000000;">Spring 主要通过</font>**<font style="color:#000000;">三级缓存</font>**<font style="color:#000000;">解决单例 Bean 的 Setter 注入或字段注入导致的循环依赖。</font>

<font style="color:#000000;">三级缓存分别是：一级缓存 singletonObjects，保存完全初始化好的单例 Bean；二级缓存 earlySingletonObjects，保存提前暴露的半成品 Bean；三级缓存 singletonFactories，保存能够创建提前暴露对象的 ObjectFactory。</font>

<font style="color:#000000;">当 Bean A 依赖 Bean B，而 Bean B 又依赖 Bean A 时，Spring 会在实例化 A 后，将其 ObjectFactory 放入三级缓存。</font>

<font style="color:#000000;">创建 B 时，如果需要 A，就会从三级缓存获取 A 的提前引用，从而完成依赖注入，避免无限递归。</font>

<font style="color:#000000;">之所以使用三级缓存而不是两级缓存，是因为 Spring 需要结合 AOP，在获取提前引用时决定返回原始对象还是代理对象，保证整个容器中引用的是同一个最终 Bean。需要注意的是，Spring 只能解决单例 Bean 的 Setter 或字段注入循环依赖，构造器注入和 Prototype Bean 的循环依赖无法通过三级缓存解决。</font>

<font style="color:#000000;"></font>

## **<font style="color:rgb(51,51,51);">事务的传播级别</font>**
<font style="color:rgb(51,51,51);">1.</font>**<font style="color:rgb(51,51,51);">PROPAGATION_REQUIRED</font>**<font style="color:rgb(51,51,51);">:默认的Spring事物传播级别，若当前存在事务，则加入该事务，若不存在事务，则新建一个事务。 </font>

<font style="color:rgb(51,51,51);">2. </font>**<font style="color:rgb(51,51,51);">PAOPAGATION_REQUIRE_NEW</font>**<font style="color:rgb(51,51,51);">:若当前没有事务，则新建一个事务。若当前存在事务，则新建一个事务，新老事务相互独立。外部事务抛出异常回滚不会影响内部事务的正常提交。 </font>

<font style="color:rgb(51,51,51);">3. </font>**<font style="color:rgb(51,51,51);">PROPAGATION_NESTED</font>**<font style="color:rgb(51,51,51);">:如果当前存在事务，则嵌套在当前事务中执行。如果当前没有事务，则新建一个事务，类似于REQUIRE_NEW。 </font>

<font style="color:rgb(51,51,51);">4. </font>**<font style="color:rgb(51,51,51);">PROPAGATION_SUPPORTS</font>**<font style="color:rgb(51,51,51);">:支持当前事务，若当前不存在事务，以非事务的方式执行。 </font>

<font style="color:rgb(51,51,51);">5. </font>**<font style="color:rgb(51,51,51);">PROPAGATION_NOT_SUPPORTED</font>**<font style="color:rgb(51,51,51);">:以非事务的方式执行，若当前存在事务，则把当前事务挂起。 </font>

<font style="color:rgb(51,51,51);">6. </font>**<font style="color:rgb(51,51,51);">PROPAGATION_MANDATORY</font>**<font style="color:rgb(51,51,51);">:强制事务执行，若当前不存在事务，则抛出异常. </font>

<font style="color:rgb(51,51,51);">7. </font>**<font style="color:rgb(51,51,51);">PROPAGATION_NEVER</font>**<font style="color:rgb(51,51,51);">:以非事务的方式执行，如果当前存在事务，则抛出异常。</font>

<font style="color:rgb(51,51,51);"></font>

<font style="color:rgb(51,51,51);"></font>

## <font style="color:rgb(51,51,51);">Spring事务</font>
<font style="color:#000000;">Spring AOP 使用动态代理实现事务增强。</font>

<font style="color:#000000;">如果使用 JDK 动态代理，需要目标类实现接口；如果使用 CGLIB，则通过继承目标类实现增强，因此 final 类和 final 方法不能被 CGLIB 增强。</font>

<font style="color:#000000;">在 Spring Boot 中默认通常使用 CGLIB，也可以通过配置切换为 JDK 动态代理。</font>

<font style="color:#000000;"></font>

### 实现方式
**<font style="color:#000000;">编程式事务</font>**<font style="color:#000000;">需要开发者通过 TransactionTemplate 或 PlatformTransactionManager 手动控制事务的开启、提交和回滚，灵活但代码较多，业务逻辑与事务逻辑耦合</font>

**<font style="color:#000000;">声明式事务</font>**<font style="color:#000000;">是企业开发的主流方式，只需在方法或类上使用 @Transactional 即可。Spring 会基于 AOP 为目标对象创建代理，在方法执行前开启事务，执行成功后提交事务，发生符合回滚规则的异常时回滚事务。其底层依赖 TransactionInterceptor 和 PlatformTransactionManager 完成事务管理</font>

<font style="color:#000000;"></font>

### 声明式事务失效原因
<font style="color:#000000;">① 同类方法自调用（不经过代理对象）</font>

<font style="color:#000000;">② 方法不是 public</font>

<font style="color:#000000;">③ 方法是 final（使用的是CGLIB代理）</font>

<font style="color:#000000;">④ 方法是 static (方法属于类，不属于对象）</font>

<font style="color:#000000;">⑤ 捕获异常，没有继续抛出</font>

<font style="color:#000000;">⑥ 抛出的不是默认回滚异常（只回滚RuntimeException、Error）</font>

<font style="color:#000000;">⑦Bean 没有交给 Spring 管理（没有代理）</font>

<font style="color:#000000;">⑧ 数据库或事务管理器不支持事务</font>

<font style="color:#000000;"></font>

<font style="color:#000000;"></font>

## **<font style="color:rgb(51,51,51);">Spring框架的事务管理有哪些优点</font>**
<font style="color:#000000;">第一，提供了统一的事务抽象 PlatformTransactionManager，屏蔽了不同持久化技术的差异；</font>

<font style="color:#000000;">第二，支持声明式事务，通过 @Transactional 就可以完成事务管理，减少了大量样板代码；</font>

<font style="color:#000000;">第三，基于 AOP 实现事务增强，使事务逻辑与业务逻辑解耦，提高了代码的可维护性；</font>

<font style="color:#000000;">第四，支持 7 种事务传播行为和 4 种事务隔离级别，能够满足复杂业务场景；</font>

<font style="color:#000000;">第五，可以自定义事务回滚规则，并支持编程式事务和声明式事务两种管理方式。</font>

<font style="color:#000000;">此外，Spring 还能方便地与 MyBatis、JPA、Hibernate 等框架集成，因此在企业开发中应用非常广泛。</font>

<font style="color:#000000;">Spring 事务本质上是基于 AOP 和动态代理实现的，事务真正由 PlatformTransactionManager 完成，而 @Transactional 只是声明事务规则</font>

<font style="color:#000000;"></font>

<font style="color:#000000;"></font>

## **<font style="color:rgb(51,51,51);">事务三要素是什么</font>**
**<font style="color:rgb(51,51,51);">数据源</font>**<font style="color:rgb(51,51,51);">：表示具体的事务性资源，是事务的真正处理者，如MySQL等。 </font>

**<font style="color:rgb(51,51,51);">事务管理器</font>**<font style="color:rgb(51,51,51);">：像一个大管家，从整体上管理事务的处理过程，如打开、提交、回滚等。 </font>

**<font style="color:rgb(51,51,51);">事务应用和属性配置</font>**<font style="color:rgb(51,51,51);">：像一个标识符，表明哪些方法要参与事务，如何参与事务，以及一些相关属 </font>

<font style="color:rgb(51,51,51);">性如隔离级别、超时时间等。</font>

<font style="color:rgb(51,51,51);"></font>

<font style="color:rgb(51,51,51);"></font>

## **<font style="color:rgb(51,51,51);">事务注解的本质是什么</font>**
<font style="color:rgb(51,51,51);">@Transactional 这个注解仅仅是一些（和事务相关的）元数据，在运行时被事务基础设施读取消 费，并使用这些元数据来配置bean的事务行为。 大致来说具有两方面功能，一是表明该方法要参 与事务，二是配置相关属性来定制事务的参与方式和运行行为 。</font>

<font style="color:rgb(51,51,51);">声明式事务主要是得益于</font><font style="color:rgb(51,51,51);">Spring AOP</font><font style="color:rgb(51,51,51);">。使用一个事务拦截器，在方法调用的前后</font><font style="color:rgb(51,51,51);">/</font><font style="color:rgb(51,51,51);">周围进行事务性 </font>

<font style="color:rgb(51,51,51);">增强（</font><font style="color:rgb(51,51,51);">advice</font><font style="color:rgb(51,51,51);">），来驱动事务完成。 </font>

<font style="color:rgb(51,51,51);">@Transactional</font><font style="color:rgb(51,51,51);">注解既可以标注在类上，也可以标注在方法上。当在类上时，默认应用到类里的所 </font>

<font style="color:rgb(51,51,51);">有方法。如果此时方法上也标注了，则方法上的优先级高。 另外注意方法一定要是public的。</font>

