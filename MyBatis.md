## **<font style="color:rgb(51,51,51);">什么是MyBatis</font>**
<font style="color:rgb(51,51,51);">（1）Mybatis是一个半ORM（对象关系映射）框架，它内部封装了JDBC，开发时只需要关注SQL </font>

<font style="color:rgb(51,51,51);">语句本身，不需要花费精力去处理加载驱动、创建连接、创建</font><font style="color:rgb(51,51,51);">statement</font><font style="color:rgb(51,51,51);">等繁杂的过程。程序员直 </font>

<font style="color:rgb(51,51,51);">接编写原生态</font><font style="color:rgb(51,51,51);">sql</font><font style="color:rgb(51,51,51);">，可以严格控制</font><font style="color:rgb(51,51,51);">sql</font><font style="color:rgb(51,51,51);">执行性能，灵活度高。</font>

<font style="color:rgb(51,51,51);">（2）MyBatis 可以使用 XML 或注解来配置和映射原生信息，将 POJO映射成数据库中的记录，避 </font>

<font style="color:rgb(51,51,51);">免了几乎所有的</font><font style="color:rgb(51,51,51);"> JDBC </font><font style="color:rgb(51,51,51);">代码和手动设置参数以及获取结果集。 </font>

<font style="color:rgb(51,51,51);">（</font><font style="color:rgb(51,51,51);">3</font><font style="color:rgb(51,51,51);">）通过</font><font style="color:rgb(51,51,51);">xml </font><font style="color:rgb(51,51,51);">文件或注解的方式将要执行的各种</font><font style="color:rgb(51,51,51);"> statement </font><font style="color:rgb(51,51,51);">配置起来，并通过</font><font style="color:rgb(51,51,51);">java</font><font style="color:rgb(51,51,51);">对象和 </font>

<font style="color:rgb(51,51,51);">statement</font><font style="color:rgb(51,51,51);">中</font><font style="color:rgb(51,51,51);">sql</font><font style="color:rgb(51,51,51);">的动态参数进行映射生成最终执行的</font><font style="color:rgb(51,51,51);">sql</font><font style="color:rgb(51,51,51);">语句，最后由</font><font style="color:rgb(51,51,51);">mybatis</font><font style="color:rgb(51,51,51);">框架执行</font><font style="color:rgb(51,51,51);">sql</font><font style="color:rgb(51,51,51);">并将结果 </font>

<font style="color:rgb(51,51,51);">映射为java对象并返回。（从执行sql到返回result的过程）。</font>

## **<font style="color:rgb(51,51,51);">MyBatis的优点和缺点</font>**
### **<font style="color:rgb(51,51,51);">优点</font>**
① 对 JDBC 进行了封装，简化数据库操作，提高开发效率；

② 自动完成 ResultSet 到 Java 对象的映射，减少模板代码；

③ 支持动态 SQL（如 `<if>`、`<where>`、`<foreach>` 等），能够根据业务需求灵活生成 SQL；

④ 默认使用 `PreparedStatement` 进行参数绑定，能够有效防止 SQL 注入，并提高执行效率；

⑤ 实现了 SQL 与 Java 代码分离，便于维护和优化；

⑥ 支持一级缓存和二级缓存，提高查询性能

### 缺点
① SQL 需要开发人员手写，开发和维护成本较高；

② 项目规模较大时，Mapper XML 文件较多，维护成本较高；

③ 复杂关联查询（如一对多、多对一）配置较繁琐，可读性和维护性相对较差；

④ 由于采用手写 SQL，不同数据库存在 SQL 方言差异，更换数据库时部分 SQL 需要调整，迁移成本相对较高；

⑤ 相比 Hibernate 等全 ORM 框架，对对象关系映射（ORM）的支持较弱，需要开发者自行维护 SQL**。**  

## 为什么 选择 MyBatis，而不是 Hibernate  
因为 MyBatis 更贴近原生 SQL，开发者可以完全控制 SQL 的编写，便于进行复杂查询和性能优化，因此在互联网企业中应用更广泛。而 Hibernate 更强调全 ORM，开发效率较高，但复杂 SQL 的控制能力相对较弱。  

## **<font style="color:rgb(51,51,51);">#{}和${}的区别是什么</font>**
`#{}` 底层使用 `PreparedStatement` 参数绑定，会将参数替换为 `?`，支持预编译，能够防止 SQL 注入，也是 MyBatis 推荐的方式；

`${}` 采用字符串直接拼接，不支持预编译，存在 SQL 注入风险，一般只用于动态表名、字段名或排序字段等特殊场景。  

###  既然 MyBatis 默认使用 `PreparedStatement`，为什么 `${}` 还是会有 SQL 注入？  
 虽然 MyBatis 默认使用 `PreparedStatement`，但 `${}` 会先进行字符串替换，再把拼接后的完整 SQL 交给 `PreparedStatement`。也就是说，恶意内容已经成为 SQL 语句的一部分，因此 `PreparedStatement` 无法再起到防注入的作用。而 `#{}` 是先生成带 `?` 的 SQL，再单独绑定参数，所以不会发生 SQL 注入。  

## **<font style="color:rgb(51,51,51);">当实体类中的属性名和表中的字段名不一样 ，怎么办 ？</font>**
如果实体类属性名和数据库字段名不一致，可以通过三种方式解决：

第一种是在 SQL 中使用别名，例如 user_name as userName；

第二种是使用 resultMap 显式配置字段与属性的映射关系，这是企业开发中最常用的方式，也适用于复杂对象映射；

```java
<resultMap id="userMap" type="User">

<id property="id" column="id"/>

<result property="userName"column="user_name"/>

<result property="userAge"column="user_age"/>

</resultMap>
```

```java
<select id="findById" resultMap="userMap">
    select *
    from user
</select>
```

第三种，如果只是数据库采用下划线命名、Java 采用驼峰命名，可以开启 MyBatis 的驼峰命名自动映射功能（mapUnderscoreToCamelCase=true），MyBatis 会自动将 user_name 映射为 userName。

```xml
mybatis.configuration.map-underscore-to-camel-case=true
```

### resultType 和 resultMap 有什么区别
resultType 适用于简单查询，只需要指定返回对象类型，MyBatis 会根据字段名和属性名自动完成映射。

resultMap 则适用于字段名与属性名不一致，以及一对一、一对多等复杂对象关系映射，可以显式指定字段与属性之间的对应关系，功能更加灵活。

可以把 resultType 理解为 MyBatis 自动生成的一个简单版 resultMap，而 resultMap 则是开发者自定义的完整映射规则。

## **<font style="color:rgb(51,51,51);">Mybatis是如何进行分页的？</font>**
MyBatis 常见的分页方式有三种。

第一种是直接在 SQL 中使用 LIMIT 实现物理分页，由数据库完成分页，效率最高；

第二种是使用 **PageHelper **等分页插件，它基于 MyBatis 插件机制，在执行 SQL 前拦截 SQL，并自动拼接 LIMIT，这是企业开发中最常用的方式；



```java
PageHelper.startPage(1,10);
List<User> list=userMapper.listUser();
```

```java
<select id="listUser">
    select * from user
</select>
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/70336156/1784438614760-ace97b94-5828-440b-b4bd-58031e98c982.png)

第三种是 RowBounds，它属于逻辑分页，会先查询所有数据，再在内存中截取需要的数据，数据量大时性能较差，因此实际项目中很少使用。

###  为什么 PageHelper 能自动分页？  
 因为它实现了 MyBatis 的插件（Interceptor）机制，在 `StatementHandler.prepare()` 等执行 SQL 的阶段拦截 SQL，自动修改为带 `LIMIT` 的 SQL，再交给数据库执行。  

###  为什么 RowBounds 性能差？  
 因为它属于逻辑分页，会先查询全部数据，再由 MyBatis 在内存中截取指定范围的数据，数据量较大时会占用更多内存和网络传输资源，因此一般不推荐使用  

## **<font style="color:rgb(51,51,51);">Mybatis是如何将sql执行结果封装为目标对象并返回的？</font>**
<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/70336156/1784439242518-b5824846-a000-4dfe-a449-1d82e2d14092.png)

 MyBatis 在 SQL 执行完成后，会通过 JDBC 获取 `ResultSet`，然后由 `ResultSetHandler` 根据 `resultType` 或 `resultMap` 确定目标对象类型。接着利用反射创建对象实例，遍历 `ResultSet`，将查询结果中的每一列映射到对象对应的属性，最后返回封装好的 Java 对象或对象集合。如果使用 `resultType`，MyBatis 会按字段名和属性名自动映射；如果字段名不一致或涉及复杂对象关系，则会根据 `resultMap` 中配置的映射规则完成封装。  

###  为什么 MyBatis 能自动给对象赋值？  
利用了** 反射机制**：

+ 先通过反射创建对象实例；
+  再通过反射调用 Setter 方法（或直接设置属性）完成赋值。

## **<font style="color:rgb(51,51,51);">如何执行批量插入？</font>**
MyBatis 批量插入主要有两种方式：一种是使用 <foreach> 拼接一条批量 INSERT 语句，这是企业开发最常用的方法；另一种是使用 ExecutorType.BATCH，底层通过 JDBC 的批处理机制（addBatch()、executeBatch()）执行多条 SQL，更适合大数据量导入。

### 方法一：使用 <foreach>
`<foreach>` 会遍历集合，将多组 `values` 拼接成**一条 SQL**，数据库一次执行完成  

```xml
<insert id="batchInsert">
    insert into user(name, age)
    values
    <foreach collection="list"
             item="user"
             separator=",">
        (#{user.name}, #{user.age})
    </foreach>
</insert>
```

```java
List<User> list = ...;

userMapper.batchInsert(list);
```

```sql
insert into user(name, age)
values
('Tom',20),
('Jack',21),
('Lucy',18);
```

### 方法二：ExecutorType.BATCH
```java
SqlSession session = sqlSessionFactory.openSession(ExecutorType.BATCH);

UserMapper mapper = session.getMapper(UserMapper.class);

for (User user : list) {
    mapper.insert(user);
}

session.commit();
```



## **<font style="color:rgb(51,51,51);">Xml映射文件中，除了常见的select|insert|updae|delete 标签之外，还有哪些标签？</font>**
<font style="color:rgb(51,51,51);">MyBatis XML 除了 CRUD 标签外，还包括结果映射标签（resultMap、association、collection）、动态 SQL 标签（if、where、set、foreach、choose、trim）、SQL 复用标签（sql、include）以及缓存标签（cache、cache-ref）等</font>



## **<font style="color:rgb(51,51,51);">MyBatis实现一对一有几种方式?具体怎么操作的？</font>**
MyBatis 实现一对一关联映射主要有两种方式。

第一种是**关联查询**，通过 SQL 的 `JOIN` 一次查询出主表和关联表数据，再利用 `resultMap` 中的 `association` 标签完成对象封装，这种方式只执行一条 SQL，效率较高。

第二种是**分步查询**，在 `association` 中通过 `select` 属性指定另一个查询语句，先查询主对象，再根据外键查询关联对象。这种方式支持延迟加载，但会执行多条 SQL，如果数据量较大，可能出现 N+1 查询问题 

### 关联查询
```xml
<select id="findOrder" resultMap="orderMap">
    SELECT o.id,
           o.price,
           u.id AS uid,
           u.name
    FROM orders o
    LEFT JOIN user u
    ON o.user_id=u.id
</select>
```

```xml
<resultMap id="orderMap" type="Order">

    <id property="id" column="id"/>

    <result property="price"
            column="price"/>

    <association property="user"
                 javaType="User">

        <id property="id" column="uid"/>

        <result property="name"
                column="name"/>

    </association>

</resultMap>
```

 association 的属性：

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/70336156/1784447059361-5631e6fe-56dd-4fff-b7f5-fa71a33a4e69.png)  

**优点**：

+ 只执行一次 SQL。
+  查询效率高。 
+  适合经常需要一起查询的数据。

### 分步查询
```xml
<select id="findOrderById" resultMap="orderMap">

select *

from orders

where id=#{id}

</select>
```

```xml
<resultMap id="orderMap" type="Order">

    <id property="id" column="id"/>

    <result property="price"
            column="price"/>

    <association property="user"
                 column="user_id"
                 select="findUserById"/>

</resultMap>
```

```xml
<select id="findUserById"
        resultType="User">

select *

from user

where id=#{id}

</select>
```

**优点**：

+ SQL 更简单。
+  可以开启延迟加载。 
+  查询更加灵活



## 什么是延迟加载
 延迟加载是指查询主对象时，不立即查询关联对象，而是在真正访问关联对象时，才发送 SQL 查询关联数据。MyBatis 主要在嵌套查询中配合 `association` 或 `collection` 使用。

它能够减少不必要的数据库访问，提高查询效率；但如果大量访问关联对象，可能产生 N+1 查询问题，因此需要根据业务场景合理使用。  

```xml
<settings>

    <setting name="lazyLoadingEnabled"
             value="true"/>

    <setting name="aggressiveLazyLoading"
             value="false"/>

</settings>
```

+ `lazyLoadingEnabled=true`：开启延迟加载。
+ `aggressiveLazyLoading=false`：按需加载，只有访问某个关联对象时，才加载该对象（通常推荐这种配置）

优点：

+ 减少不必要的数据库查询。
+  提高查询效率。 
+  节省内存。



### 延时加载和立即加载的区别
| **延迟加载** | **立即加载** |
| --- | --- |
| 使用时才查询关联对象 | 查询主对象时一起查询关联对象 |
| SQL 少 | SQL 多 |
| 性能较好 | 响应更快拿到完整数据 |
| 可能产生 N+1 问题 | 不会产生延迟加载导致的 N+1（但联表查询也可能带来其他性能问题） |


## **<font style="color:rgb(51,51,51);">Mybatis是否支持延迟加载？</font>**
 MyBatis 支持延迟加载，主要用于一对一和一对多的嵌套查询。它的思想是查询主对象时，不立即查询关联对象，而是在真正访问关联对象时才执行关联 SQL。底层通过代理对象实现延迟加载，查询主对象后，先为关联对象创建代理对象，当第一次调用关联对象的方法时，代理对象会拦截该调用，执行对应的查询 SQL，将结果封装成真正的对象并返回，后续访问则直接使用已经加载的数据  

 MyBatis 的延迟加载依赖于代理机制，在配置 `lazyLoadingEnabled=true` 后生效。代理对象会保存关联查询所需的信息（如 SQL、参数等），当首次访问关联属性时，再触发查询并完成对象填充。  

## Mybatis的缓存机制
 MyBatis 提供了两级缓存机制，分别是一级缓存（本地缓存）和二级缓存（全局缓存）。

一级缓存默认开启，作用范围是同一个 SqlSession；二级缓存默认关闭，作用范围是同一个 Mapper 的多个 SqlSession。缓存的目的是减少数据库访问次数，提高查询性能。  

 **一级缓存**作用范围为同一个 SqlSession  

 **二级缓存**的作用范围为同一个 Mapper 的多个 SqlSession  

###  一级缓存什么时候失效？  
 ① 执行增删改  

 ② 手动清空缓存    

```java
session.clearCache();
```

 ③ 提交事务  

```java
session.commit();
```

 ④ 关闭 SqlSession  

### 如何开启二级缓存
```xml
<cache/>
```

或者

```xml
<cache eviction="LRU"
       flushInterval="60000"
       size="512"
       readOnly="false"/>
```

 实体类通常需要实现 `Serializable` 接口，便于缓存对象的序列化  

 二级缓存不是查询完立刻可见。通常需要当前 `SqlSession` 提交或关闭后，一级缓存中的数据才会写入二级缓存，供其他 `SqlSession` 使用  

###  为什么一级缓存默认开启，而二级缓存默认关闭？  
** 一级缓存：**  

+ 生命周期短（仅当前 `SqlSession`）。
+  数据一致性容易保证。 
+  风险较小，所以默认开启。

** 二级缓存 :**

+ 多个 `SqlSession` 共享数据。
+  数据更新后更容易出现脏数据。 
+  并不是所有业务都适合缓存，因此默认关闭，需要开发者按需开启。



## Mybatis中的设计模式
<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/70336156/1784456442334-35a2618d-c29c-4a5c-891e-58d2709cde7c.png)

### 代理模式
Mapper接口

      │

      ▼

MapperProxy（动态代理）

      │

      ▼

执行SQL

 Mapper 接口没有实现类，MyBatis 在运行时通过 **JDK 动态代理**生成代理对象，当调用 Mapper 方法时，代理对象会解析 Mapper 映射信息并执行对应的 SQL。  

###  工厂模式  
 MyBatis 使用 `SqlSessionFactory` 创建 `SqlSession` 对象，屏蔽了对象创建细节  

###  建造者模式  
```java
SqlSessionFactoryBuilder builder =new SqlSessionFactoryBuilder();

SqlSessionFactory factory =builder.build(inputStream);
```

 MyBatis 使用 `SqlSessionFactoryBuilder` 根据配置文件逐步构建 `SqlSessionFactory`，体现了建造者模式  

###  模板方法模式  
 MyBatis 在 `BaseExecutor` 中定义了数据库操作的整体流程，而具体执行方式由不同 Executor 子类实现  

###  责任链模式  
分页插件：

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/70336156/1784457041315-db255d3d-e9d9-4d8d-8974-c130a9d56d34.png)

 MyBatis 的插件机制采用责任链模式，多个拦截器按照顺序依次执行，每个拦截器都可以对 SQL 执行过程进行增强  

