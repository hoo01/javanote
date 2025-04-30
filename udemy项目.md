# basics of web and internet
## how api works

![微信截图_20250322150054.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250322150054.png)
## types of APIs

202 success
400 404 fail
### get request :
retrieve of GET resources from server
used only to read data
### post request:
create resources from server
### put request:
used to update something that already exits on the server
### delete request
used to detele resources from server


## REST API
- **REST APl, stands for Representational StateTransfer Application Programming Interface**
- **this uses HTTPS request to access and use data**
- **REST is stateless**
    - 每个请求都必须自包含所有信息；
    - 服务器不会存储客户端的状态；
    - 状态由客户端自己维护，服务器每次都是“重新认识你”。
    
- **can be cached 响应可以被缓存**
    - clients can cache the responses to improve performance
     📖 含义：客户端可以缓存服务器的响应结果，避免重复请求，从而提高性能、降低延迟。
     🧠 举个例子你访问一个商品详情页（GET /product/123），服务端返回了商品信息。这个商品信息短时间内不会变化，那么客户端或中间代理服务器就可以缓存这个响应，下次用户再访问时，就直接用缓存结果，不去请求服务器。
     
- **opaque in terms of layers 层之间是“透明/不可见”的**
    - which means that there can be more servers in between the client and server
     REST 架构支持**分层系统（Layered System）**。客户端并不知道自己请求的是最终服务端，还是中间代理层（如网关、缓存服务器、安全认证层等），这一切对客户端来说是“不可见”的。
     
- **uniform interface 统一接口**
    🧠 举个例子：
     无论是用户、商品还是订单，你的接口都像这样：
     `GET /users/1 
     GET /products/1001 
     GET /orders/888`
     - 使用统一的 HTTP 方法
     - 使用统一的资源 URI 命名方式

Web services built followingthe REST architectural style are known as **RESTful web services**

## http vs https
http stands for **HyperText Transfer Protocol**
https stands for HyperText Transfer Protocol Secure
HTTpS is essentiallye HTTP with security

>Both HTTP and HTTPS are protocols designed for transferring hypertextacross the World Wide Web.
>They operate based on a client-server model, where a client (web browser) sends a request to the server hosting a website
>Both protocols use similar methods to perform actions on the web server aswell as status codes.
>HTTP and HTTPS are both stateless protocols, meaning they do not inherenthremember anything about the previous web session
  Both HTTP and HTTPS can transfer data in various formats including HTML

## Status Code in API
![微信截图_20250322160157.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250322160157.png)


classification of status codes
- 1xx(international)
- 2xx(successful)
    200 successful
    201 created
    204 no content nothing to return
- 3xx(redirection)
    301 the request resource has been moved permanently
- 4xx(client error)
    400 bad request the server could not understand the request due to an incorrect
    401 unauthorized  does not have authentication credentials
    403 forbidden it understood it, but it refused to authorize it
    404 you're trying to access something that does not exist
- 5xx(server error)
    500 internal server error : you're sending a request to the server, an unexpected condition

## Resource,URI and Sub- Resource
### URl(Uniform Resource ldentifer)统一资源标识符
- A URl is a string of characters used to identify a resource on the internet either by location, name,or both
    https://example.com/users/123
    这个URL显示：
    - 协议是`https`
    - 域名是`example.com`
    - 资源路径是`/users/123`
     它指向互联网上的某个资源，可以通过它获取该资源的内容。

- It provides a mechanism for accessing the representation ofa resource over the network, typically through specifc protocols such as HTTP or HTTPS.
- URls are a broad category that includes both URLs (Uniform Resource Locators通过位置访问资源) and URNs (Uniform Resource Names**通过名称访问资源**，但现在用得不够).
### Sub-Resource
- A Sub-Resource is a resource that is hierarchically under another resource.
- lt's a part of a larger resource and can be accessed by extending the URl of the parent resource.
- Sub-resources are often used in RESTful APls to maintain a logical hierarchy of data and to facilitate easy access to related resources.
- Example: In a blogging platform, you might have a users resource identifed by a URl (/users). A specifc user could be a resource accessible at /users/{userld}.
- lf each user can have blog posts, a post would be a sub-resource of that user,identifed by something like /users/fuserld}/posts/{postld}.
    举个例子（博客系统）：
    想象你在开发一个博客系统，你有以下资源结构：
    所有用户：`/users`
    某个用户：`/users/{userId}`，比如`/users/123`
    某用户的所有帖子：`/users/123/posts`
    某用户的某一篇帖子：`/users/123/posts/456`

总结

| 网址 (URI) | 统一资源标识`https://api.com/users/123`                        |
| -------- | -------------------------------------------------------- |
| 子资源      | 某资源的子集，通过在URL中的凹陷路径表达，例如`/users/123/posts/456`表示某用户的某篇帖子 |

## spring framework basics

### introduction to tight coupling and loose coupling
Coupling refers to howclosely connected different components or systems are

#### tight coupling
![微信截图_20250322173023.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250322173023.png)

#### loose coupling
![微信截图_20250322173215.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250322173215.png)

## Inversion of Control (loC)
Inversion of Control is a design principle where the control of object creation and lifecycle management is transferred from the application code to an external container or framework
控制反转是一种**设计思想**，它把对象的创建权和管理权**交给框架或容器**，而不是由我们自己在代码中手动创建。

## Dependency Injection (Dl)
Dependency injection is a design pattern commonly used inobject-oriented programming, where the dependencies of a class are provided externally rather than being created within the class itself.
依赖注入是 IoC 的一种实现方式。它的核心是：把一个类所依赖的对象（依赖）从外部“注入”进去，而不是自己在类里 `new` 出来。

### **核心思想**
- **IOC（控制反转）**：将对象的创建和管理权交给外部容器，而不是由类自己控制。
- **DI（依赖注入）**：通过构造函数、Setter方法或接口注入依赖，实现解耦。
### 为什么重要
- 提升代码的**可维护性**（轻松替换实现）。
- 提高**可测试性**（单元测试时注入 Mock 对象）。
- 支持**模块化开发**（各组件独立变化）。
### **传统方式（无IOC/DI）**
在传统方式中，`UserService` 会直接创建 `UserRepository` 的实例，导致**紧耦合**。
```java
// UserRepository.java
public class UserRepository {
    public String getUserData() {
        return "User data from database";
    }
}

// UserService.java
public class UserService {
    // UserService 直接创建 UserRepository 的实例
    private UserRepository userRepository = new UserRepository();

    public String processUser() {
        return userRepository.getUserData();
    }
}

// 测试代码
public class Main {
    public static void main(String[] args) {
        UserService userService = new UserService();
        System.out.println(userService.processUser()); // 输出：User data from database
    }
}
```

**局限：**
- 如果要将 `UserRepository` 替换为另一个实现（比如 `MockUserRepository`），必须修改 `UserService` 的代码。
- 难以进行单元测试（比如无法注入一个测试用的 `UserRepository`）。

### **使用IOC/DI（依赖注入）**

通过控制反转和依赖注入，`UserService` 不再负责创建 `UserRepository`，而是由**外部容器**（如 Spring）注入依赖。
#### 步骤1：定义接口（解耦）

```java
// UserRepository.java（接口）
public interface UserRepository {
    String getUserData() ;//抽象方法，实现类必须重写此方法
}

// DatabaseUserRepository.java（实现类）
public class DatabaseUserRepository implements UserRepository {
    @Override
    public String getUserData() {
        return "User data from real database";
    }
}

// MockUserRepository.java（另一个实现类，用于测试）
public class MockUserRepository implements UserRepository {
    @Override
    public String getUserData() {
        return "Mock user data for testing";
    }
}
```
#### **步骤2：修改UserService（通过构造函数注入依赖）**

```java
// UserService.java
public class UserService {
    // 依赖通过构造函数注入
    private final UserRepository userRepository;//声明一个成员变量，用于存储`UserRepository`的实例。后续可以注入任意实现了`UserRepository`接口的对象。

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }// 依赖是接口，由外部决定具体实现
}

// 由 Spring 或其他容器注入依赖
@Autowired
public UserService(UserRepository userRepository) {
    // Spring 自动传入 DatabaseUserRepository 或 MockUserRepository
}
```

#### **步骤3：由外部容器（如Spring）管理依赖**

```java
// 假设有一个简单的“容器”来管理对象
public class Container {
    public static void main(String[] args) {
        // 由容器决定注入哪个实现类
        UserRepository repository = new DatabaseUserRepository(); // 可随时替换为 MockUserRepository
        UserService userService = new UserService(repository); // 依赖注入

        System.out.println(userService.processUser()); // 输出：User data from real database
    }
}
```

#### 实际应用
在 Spring 中，你可以通过注解（如 `@Autowired`）自动注入依赖，无需手动创建对象：
```java
// Spring 示例
@Service
public class UserService {
    @Autowired // Spring 自动注入 UserRepository 的实现类
    private UserRepository userRepository;

    public String processUser() {
        return userRepository.getUserData();
    }
}
```

#### **为什么说传统构造方法不是依赖注入？**

| **特性**   | **传统构造方法**           | **依赖注入构造方法**                  |
| -------- | -------------------- | ----------------------------- |
| **参数类型** | 值类型（String/int）或简单对象 | 接口或复杂对象（如 Service、Repository） |
| **依赖管理** | 直接控制参数值              | 依赖由外部容器管理                     |
| **灵活性**  | 低（参数值固定，无法动态替换）      | 高（依赖可替换，如测试时注入 Mock 对象）       |
| **使用场景** | 初始化对象属性              | 解耦复杂依赖关系                      |

##### **（1）依赖的创建权未反转**
- 在 `Person` 类中，`name` 和 `age` 的值由调用者直接传入（如 `new Person("Alice", 30)`），但**控制权仍在调用者**，未交给外部容器。
- 在 DI 中，依赖的创建和注入由容器（如 Spring）管理，类本身不关心依赖如何创建。
##### **（2）未实现解耦**
- `Person` 类的参数是具体值，而 DI 的构造方法参数是**接口或抽象**，允许灵活替换实现。
```java
// DI 示例：UserService 不关心 UserRepository 的具体实现
UserRepository repository = new DatabaseUserRepository(); // 可替换为 MockUserRepository
UserService service = new UserService(repository); // ✅ 解耦
```
##### **（3）无容器管理**
- 传统构造方法需要手动调用 `new`，而 DI 依赖容器自动管理对象的创建和注入：
```java
// Spring 中无需手动 new，由容器注入
@Autowired
private UserService userService; // Spring 自动创建并注入
```
#### **4. 如何将传统类改造成依赖注入风格？**

假设 `Person` 类需要依赖一个 `Address` 服务，可以这样改造：
```java
// 定义接口（解耦）
public interface AddressService {
    String getAddress();
}

// 实现类
public class DatabaseAddressService implements AddressService {
    @Override
    public String getAddress() {
        return "Real address from database";
    }
}

// 改造后的 Person 类（依赖注入）
public class Person {
    private final String name;
    private final int age;
    private final AddressService addressService; // 依赖接口

    // 构造方法注入
    public Person(String name, int age, AddressService addressService) {
        this.name = name;
        this.age = age;
        this.addressService = addressService; // 外部传入依赖
    }

    public void printInfo() {
        System.out.println(name + ", " + age + ", " + addressService.getAddress());
    }
}

// 使用示例
public class Main {
    public static void main(String[] args) {
        AddressService addressService = new DatabaseAddressService(); // 可替换为 Mock 实现
        Person person = new Person("Alice", 30, addressService); // 注入依赖
        person.printInfo();
    }
}
```

## Maven repository
https://mvnrepository.com/
1.加spring-code
https://mvnrepository.com/artifact/org.springframework/spring-core/6.2.5
![微信截图_20250323152404.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250323152404.png)
![微信截图_20250323152602.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250323152602.png)
2.加spring-context
![微信截图_20250323153011.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250323153011.png)
3.reload maven changes

## creating your first bean
### beans
#### Bean Defnition
A bean defnition includes confguration metadata that the container needs to know to create and manage the bean
#### Bean Confguration
- Bean defnitions can be provided in various ways, including XML confguration fles, annotations, and Java-based confguration.
- Beans are confgured using XML fles, where each bean is defned within`<bean>`tags with attributes specifying class, properties, and dependencies.
- Beans can be confgured using annotations like @Component, @Service,@Repository, etc., which are scanned by Spring and managed as beans.
![微信截图_20250323164815.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250323164815.png)

### 练习
1.新建class和.xml
![微信截图_20250323160807.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250323160807.png)


2.复制The equivalent file in the XML Schema-style would be…​中的内容到xml
https://docs.spring.io/spring-framework/docs/4.2.x/spring-framework-reference/html/xsd-configuration.html
3.继续复制
![微信截图_20250323161114.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250323161114.png)
giving the bean defination
![微信截图_20250323161415.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250323161415.png)
4.再新建一个app class测试下
![微信截图_20250323163033.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250323163033.png)

# Spring Annotations
## Components 

Component refers to a java class that is managed by the Spring IOC container
component is a special kind of a bean that is designed to be auto detected
![微信截图_20250328092501.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250328092501.png)

### using xml
```xml
<bean id="myComponent" class="com.example.MyComponent"/>
```
### using annotations
```java
import org.springframework.stereotype.Component
@Component //Mark the class as a Spring component
public class MyComponent{
  //Class implementation
}
```
## Component Scanning
Component scanning is a feature helps to automatically detect and register beans from predefned package paths

### using xml
```xml
<!-- Enable component scanning -->
<context:component-scan base-package="car.example.componentscan"/>
```
Everything will be scanned within this particular package including sub packages.

# structuring thoughts

![微信截图_20250330095000.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250330095000.png)
![微信截图_20250330095714.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250330095714.png)
![微信截图_20250330095910.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250330095910.png)

## 目前的项目结构

```bash
Postman 请求（如 GET /categories）
     ↓
Controller（接收请求）
     ↓
Service（处理业务逻辑）
     ↓
模型 Category（数据对象）

```

# validation in Spring Boot
@NotNull
@NotEmpty
@Size(min=x,max=y)
@Email
@Min(value) and @Max(value)
![微信截图_20250401095555.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250401095555.png)

# DTO
![微信截图_20250403093015.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250403093015.png)

## Category 和 CategoryDTO 的区别 & 协作关系

| 比较维度 | `Category`                                 | `CategoryDTO`         |
| ---- | ------------------------------------------ | --------------------- |
| 来自哪里 | 实体类（Entity）                                | 数据传输对象（DTO）           |
| 用途   | 跟数据库打交道：存、查、删、改                            | 跟前端交互：收请求、返回响应        |
| 字段   | 所有字段（可能还有密码、创建时间等）                         | 只保留需要暴露给前端的字段         |
| 注解   | `@Entity`、`@Id`、`@GeneratedValue` 等 JPA 注解 | 没有 JPA 注解，只是普通 Java 类 |
| 安全性  | 直接暴露可能泄漏隐私字段                               | DTO 可自由控制暴露哪些字段       |
| 灵活性  | 改字段容易影响数据库结构                               | 改字段不会影响数据库，解耦         |

## 它们是如何配合使用的？

1. **Controller 接收的是 DTO：**
```java
@PostMapping(...)
public ResponseEntity<CategoryDTO> createCategory(@RequestBody CategoryDTO categoryDTO) {
    ...
}
```
2. Service 层中把 DTO 映射成 Entity：
```java
Category category = modelMapper.map(categoryDTO, Category.class);
```
3.保存进数据库：
```java
categoryRepository.save(category);
```
4.再把 Entity 映射回 DTO，返回给前端：
```java
CategoryDTO savedDTO = modelMapper.map(savedEntity, CategoryDTO.class);
return savedDTO;
```

## 流程转换图
```css
[ 前端 JSON ] 
   ⇅     ⇅
[ CategoryDTO ] ⇄ ModelMapper ⇄ [ Category ]
                                    ⇅
                                [ 数据库 ]

```

# JPA entity

![微信截图_20250404093418.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250404093418.png)
## JPA定义
**JPA（Java Persistence API）**，全名叫“Java 持久化 API”。

官方定义是：

> 一套 Java 官方标准，用来把**Java对象**（比如实体类）和**数据库表**（比如MySQL、H2）之间进行**映射和管理**。

翻成大白话就是：

> 你写Java对象（比如`SocialUser`），它帮你自动存到数据库里变成一行数据，查询时又能自动从数据库拿回Java对象。

它是**规范**，不是具体的实现。

---

## 谁在具体执行JPA规范呢？

JPA是个标准，真正干活的是**实现JPA规范的框架**，比如：

|实现|简介|
|---|---|
|Hibernate|最流行的JPA实现，基本默认大家都用它|
|EclipseLink|也是JPA实现，但用得没Hibernate多|
|OpenJPA|Apache出的实现|

在SpringBoot里，默认就是用**Hibernate**来实现JPA的。

所以你项目里用了`@Entity`、`@OneToOne`这些注解，底下其实都是Hibernate在帮你生成SQL、操作数据库。

---

## 举例

比如你写了这个实体类：
```java
@Entity
public class SocialUser {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String username;
}

```

不用自己写SQL，JPA+Hibernate就会帮你生成：
```sql
CREATE TABLE social_user (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255)
);
```

当你在代码里保存对象：
```java
socialUserRepository.save(new SocialUser("Tom"));
```
JPA底层就执行了INSERT语句，把数据存到表里。

完全屏蔽了SQL细节，你直接面向对象操作就行！

## bi-directional mapping in one to one relationship 双向一对一的例子

### 主动方：SocialUser.java（有 `@JoinColumn`）
```java
@Entity
public class SocialUser {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToOne
    @JoinColumn(name = "social_profile_id") // 这里建外键,主动方负责在数据库中创建外键列，比如`social_profile_id`
    private SocialProfile socialProfile;// 这里是指向SocialProfile
}
```

### 被动方：SocialProfile.java（有 `mappedBy`）
```java
@Entity
public class SocialProfile {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToOne(mappedBy = "socialProfile") //mappedBy的值必须准确写成对方实体类里对应的字段名，区分大小写！！！被动方不会额外创建外键列，只是通过mappedBy指向主动方的维护
    private SocialUser user;// 这里是指向SocialUser
}
```

### @OneToOne
#### `@OneToOne` **必须两边都写吗？**
- **单向一对一关系**：只需要**一边**写`@OneToOne`，另一边不用管。如果单向关联，也要在维护方写 @JoinColumn，否则JPA不知道怎么存外键。
- **双向一对一关系**：才需要**两边都写**，并且一边用`mappedBy`指向对方。

- `@OneToOne`注解**必须贴在**指向**另一个实体类（Entity）的字段**上。
- 这个字段，类型就是**对方的类**。
- 两个字段通过 `mappedBy` 和 `JoinColumn` 连接起来，告诉JPA它们之间有一对一关系。

## 单向一对一的例子
比如我们有一个系统，**一个用户 (User)** 绑定了**一个地址 (Address)**，  
但是**地址不需要知道用户是谁**。（用户关心地址，地址不关心用户）

这就是典型的**单向一对一关系**！
### Address.java（被指向方，不需要关联User）
```java
@Entity
public class Address {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String street;
    private String city;
}
```
### User.java（主动方，持有Address）
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @OneToOne
    @JoinColumn(name = "address_id") // 在User表建一个外键列，指向 Address 表里的 `id` 主键
    private Address address;
}
```
### 最后的数据库
`user`表

| id | name  | address_id |
|----|-------|------------|
| 1  | Tom   | 10         |
`address`表

| id | street      | city       |
|----|-------------|------------|
| 10 | 123 Main St | Springfield|

## many to many relationships

避免重复记录，使用set而不是list

### SocialUser.java
```java
@ManyToMany  
@JoinTable(  
        name = "user_group",  
        joinColumns = @JoinColumn(name = "user_id"),  
        inverseJoinColumns = @JoinColumn(name = "group_id")  
)  
private Set<SocialGroup> groups = new HashSet<>();
```
说明：
- 一个用户可以加入**多个组**。
- `user_group` 是一张中间表（多对多关系一定有中间表！）
- `joinColumns = user_id`：自己的外键
- `inverseJoinColumns = group_id`：对方的外键

### SocialGroup.java
```java
@Entity  
public class SocialGroup {  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private Long id;  
  
    @ManyToMany(mappedBy = "groups")  
    private Set<SocialUser> socialUsers =new HashSet<>();  
}
```
说明：
- 一个组也可以有**多个用户**。
- `mappedBy = "groups"`：告诉JPA，这边（SocialGroup）是**被维护方**，关联由SocialUser控制。
- `mappedBy`后面写的是SocialUser类里维护这个关系的字段名。

# cascading types

![微信截图_20250404184951.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250404184951.png)

## Cascading Types定义
**Cascading** 是 JPA（Hibernate）里的一个特性：
对一个实体（比如 SocialUser）执行操作（比如保存、删除）时，顺便对它关联的实体（比如 SocialProfile、Post）也做相同的操作。

##  `SocialUser.java` 中三种关联关系：
### 1. `@OneToOne` （一对一）关系：SocialUser 和 SocialProfile

```java
@OneToOne(mappedBy = "user", cascade = {CascadeType.REMOVE, CascadeType.PERSIST, CascadeType.MERGE})
private SocialProfile socialProfile;
```
- 这里 `cascade` 配置了：
    - `PERSIST`：保存 `SocialUser` 时，**顺便保存** `SocialProfile`
    - `MERGE`：更新 `SocialUser` 时，**顺便更新** `SocialProfile`
    - `REMOVE`：删除 `SocialUser` 时，**顺便删除** `SocialProfile`

✅ 举例：
- 保存新用户时，新建的 `SocialProfile` 也会自动存到数据库。
- 更新用户信息时，`SocialProfile` 信息也会跟着一起更新。
- 删除用户时，关联的 `SocialProfile` 也会被自动删掉！
### 2. `@OneToMany` （一对多）关系：SocialUser 和 Post
```java
@OneToMany(mappedBy = "socialUser", cascade = {CascadeType.PERSIST, CascadeType.MERGE})
private List<Post> posts = new ArrayList<>();
```
- 这里 `cascade` 配置了：
    - `PERSIST`：保存 `SocialUser` 时，**顺便保存**关联的所有 `Post`
    - `MERGE`：更新 `SocialUser` 时，**顺便更新**关联的所有 `Post`

⚠️ 注意：**没有配置 `REMOVE`**，也就是说：
- 删除 `SocialUser` 时，**不会自动删除** `Post`。
- 通常因为一篇Post可能属于多个逻辑关系，所以删除用户时不直接删除Post比较安全。
### 3. `@ManyToMany` （多对多）关系：SocialUser 和 SocialGroup

- 这里**没有配置** `cascade`，默认是**不级联任何操作**。
- 所以操作 `SocialUser`，不会自动影响 `SocialGroup`。

# FetchType
FetchType plays a crucial role indefning how and when relatedentities are loaded from the databasein relation to the parent entity
![微信截图_20250405112304.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250405112304.png)

![微信截图_20250405112523.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250405112523.png)


# product module
![微信截图_20250406095538.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250406095538.png)
![微信截图_20250406095913.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250406095913.png)

## double vs Double int vs.Integer
**老师为什么用 `double` 而不是 `Double` 呢？原因可能有这些：**

1. **基本类型 `double` 性能更好**
    
    - `double` 是基本类型，直接存储数值，内存占用小，操作更快。
        
    - `Double` 是对象类型，用起来要有装箱（boxing）和拆箱（unboxing）的过程，会稍微影响性能，尤其是在大量数据运算的时候。
        
2. **业务逻辑上价格一般不会是 `null`**
    
    - `price` 这种字段通常是必须有数值的（比如商品价格不可能是“空”），所以用基本类型 `double`，避免了还要判断 `null` 的麻烦。
        
    - 如果用 `Double`，在保存、计算时，还要小心处理 `null`，否则容易出空指针异常（`NullPointerException`）。
        
3. **数据库和JPA处理上**
    
    - JPA（比如 Hibernate）可以同时支持 `double` 和 `Double`，但是如果你用 `double`，默认就是不可空（NOT NULL），符合价格字段的常规需求。
        
    - 如果字段是 `Double`，对应数据库列就允许是 NULL，有时这不是我们想要的。
        

---

**总结一句话**：

> **如果字段一定有值，比如价格、数量，就用基本类型 `double`、`int`；如果字段可以为null，比如“折扣”、“优惠描述”，可以考虑用包装类 `Double`、`Integer`。**


# spring security
![微信截图_20250407145209.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250407145209.png)

![微信截图_20250407145511.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250407145511.png)

![微信截图_20250407150007.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250407150007.png)

![微信截图_20250407150144.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250407150144.png)

![微信截图_20250407150411.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250407150411.png)

### 整体流程：

1. **请求进入（Request）**  
    用户发起一个 HTTP 请求，首先会进入到 **Filter Chain**（过滤器链）。
    
2. **Filter Chain（过滤器链）**
    - Spring Security 在应用中通过一堆过滤器(Filter)来保护你的应用。
    - 这些过滤器一层层地处理请求，像传送带一样。
    - **Authentication Filter** 是其中专门负责认证（比如登录验证）的过滤器。
    
3. **Authentication Filter**
    - 这个过滤器（比如 `UsernamePasswordAuthenticationFilter`）专门拦截登录请求。
    - 它会把请求交给 **AuthenticationManager** 来处理。

4. **AuthenticationManager**
    - `AuthenticationManager` 是一个接口，最常用的实现是 `ProviderManager`。
    - 它负责协调真正去认证的 **Authentication Provider**（认证提供者）。
    - 简单理解，AuthenticationManager就像一个大总管，它不会自己干活，而是把认证交给下面的Provider。
        
5. **Authentication Provider（DaoAuthenticationProvider）**
    - 真正的认证逻辑发生在这里。
    - `DaoAuthenticationProvider`是常见的实现，专门处理基于用户名+密码的认证。
    - 它会调用 `UserDetailsService` 来根据用户名查询用户信息。
        
6. **UserDetailsService**
    - `UserDetailsService` 接口有一个方法：`loadUserByUsername(username)`。
    - 这个方法就是根据用户名去数据库（或者其他地方）找用户。
    - 查到的用户信息（如用户名、密码、角色）会返回回来。
        
7. **数据库查询**
    - `UserDetailsService` 里面通常是用 `findByUsername(username)` 方法去查数据库，把用户数据拿到。
        
8. **密码比对（PasswordEncoder）**
    - 查询到用户数据后，`DaoAuthenticationProvider` 会用 `PasswordEncoder` 来比对密码是否正确。
    - 通常是把登录时输入的密码加密，然后和数据库里存的加密密码比较。
        
9. **认证成功后**
    - 如果密码匹配，认证成功，用户的信息就会被存到 **Security Context**（安全上下文）。
    - `SecurityContext` 相当于保存了这次请求是谁（哪个用户）在操作，后面可以拿来做鉴权（比如判断用户是否有权限访问某个接口）。
        
10. **放行到你的 Controller**
    - 如果认证通过，才会继续走到你的 Controller（业务代码）。
    - 如果认证失败，比如用户名密码不对，就直接返回错误响应了。
        
11. **响应（Response）返回**
    - 最后控制器处理完，响应回到前端。
# hashing
![微信截图_20250408100918.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250408100918.png)

bcrypt involves using salting
salting helps security
![微信截图_20250408101225.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250408101225.png)

# JWT

jwt = json web token
## token
json web tokens are an open,industry standard
![微信截图_20250408111455.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250408111455.png)

how is token sent?

tokens are sent using HTTP Authorization header

format:
Authorization:Bearer`<token>`

an example for token

![微信截图_20250408111942.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250408111942.png)
![微信截图_20250408112019.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250408112019.png)

网站：
jwt.io
## JWT的文件组成
![微信截图_20250408114113.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250408114113.png)

### JwtUtils--“JWT工具人”
>Contains utility methods for generating, parsing, andvalidating JWTs.
>Include generating a tokenfrom a username, validating a JWT, and extracting the username from a token

- 负责**生成**、**解析**、**校验** JWT（JSON Web Token）。
- 通常提供的方法有：
    - `generateToken(username)` 👉 传入用户名，生成一个 JWT。
    - `validateToken(token)` 👉 校验传进来的 JWT 是否有效（没过期、签名正确等）。

    - `getUsernameFromToken(token)` 👉 从 JWT 中提取出用户名。
🔧 **负责“造Token、验Token、解读Token”的工具类。**

### AuthTokenFilter--"每次检查的请求员"
>Filters incoming requests to check for a valid JWT in the header, setting the authentication context if the token is valid.
>Extracts JWT from request header, validates it, and configures the Spring Security context with user details if the token is valid

在每次 HTTP 请求进来时：
- **从请求头**（`Authorization` 里面）**提取JWT**。
- **用 JwtUtils 验证这个JWT**是否有效。
- 如果有效，就**把用户信息存到Spring Security的认证上下文**（SecurityContext）里，让后续代码知道：✅这个人已经登录过了。

收到请求 ->
提取请求头里的token ->
拿给 JwtUtils 验证 ->
如果合法，就设置Authentication ->
后面Controller就能知道是谁发的请求了

### AuthEntryPointJwt——“拦截非法请求的守门员”
>Provides custom handling for unauthorized requests, typically when authentication is required but not supplied or valid.
>When an unauthorizedrequest is detected, it logs the error and returns a JSON response with an error message status code, and the path attempted.
- 如果用户请求了需要登录的资源，但：
    - 没带JWT
    - 或者JWT无效
- 那么Spring Security会调用`AuthEntryPointJwt`，统一返回一个**自定义的错误响应**，比如：
    - 状态码：`401 Unauthorized`
    - 错误信息：`"Authentication required or token invalid"`

它通常还会**记录日志**方便排查问题。

🚫 **负责“拦截未授权的请求，返回友好错误提示”的组件。**

### SecurityConfig
>Configures Spring Security filters and rules for the application
>Sets up the security filter chain, permitting or denying access based on paths and roles. It also configures session management to stateless, which is crucial for JWT usage.



# JPQL和SQL的写法

JPQL里
来自CartRepository.java
```java
@Query("SELECT c FROM Cart c JOIN FETCH c.cartItems ci JOIN FETCH ci.product p WHERE p.productId = ?1")  
List<Cart> findCartsByProductId(Long productId);
```
这一段JPQL，就是传统SQL里用**多表联查**，**根据product_id筛选**，一次拿到cart信息！

sql等价写法
```java
SELECT c* 
FROM carts c
JOIN cart_items ci ON c.cart_id = ci.cart_id
JOIN products p on ci.product_id = p.product_id
WHERE p.product_id = ?
```

对应关系：

| JPQL中的东西       | SQL中的东西                              | 含义        |
| -------------- | ------------------------------------ | --------- |
| Cart c         | 表 `carts` 起别名 `c`                    | 购物车表      |
| c.cartItems ci | 表 `cart_items` 起别名 `ci`，外键 `cart_id` | 购物车和商品项关联 |
| ci.product p   | 表 `products` 起别名 `p`，外键 `product_id` | 购物项和商品关联  |
| p.id = ?1      | p.product_id = ?                     | 根据商品ID筛选  |
```
carts         cart_items          products
--------      ------------        --------------
cart_id  <---- cart_id             (product info)
(user_id)     product_id  ----->   product_id
(total_price) quantity             name
              price                stock_quantity
```

注意！
- JPQL是面向**对象（Entity）的，所以它写的是**对象属性（比如c.cartItems）
- SQL是面向**表结构**的，所以它必须指明：
    - 表名
    - 外键关系
- JPQL里**FETCH**表示：查的时候马上把关联对象也查出来（防止懒加载）
- SQL里没FETCH这个说法，它一次JOIN完结果自然就拿到了


# JPA and Database
![微信截图_20250414185451.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250414185451.png)
![微信截图_20250414185923.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250414185923.png)

# database schema management

![微信截图_20250415120159.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250415120159.png)
![微信截图_20250415120431.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250415120431.png)

# address
![微信截图_20250415163412.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250415163412.png)
![微信截图_20250415163533.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250415163533.png)


# deployments
![捕获.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E6%8D%95%E8%8E%B7.PNG)

部署后
![捕获2.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E6%8D%95%E8%8E%B72.PNG)

# react
新建文件夹
`npx create-react-app my-react-app`
`npx create-react-app my-react-app --template typescript`

```java
Success! Created my-react-app at G:\React\my-react-app
Inside that directory, you can run several commands:

  npm start
    Starts the development server.

  npm run build
    Bundles the app into static files for production.

  npm test
    Starts the test runner.

  npm run eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd my-react-app
  npm start

Happy hacking!
```

ctrl+c终止进程

## vite
### 创建项目流程
https://cn.vite.dev/guide/
1. 定位到父目录
`cd G:\React`
2.创建项目
`npm create vite@latest`

```
✔ Project name: » my-react-app
✔ Select a framework: › React
✔ Select a variant: › JavaScript or TypeScript
```

也可以是`npm create vite@latest vite-react -- --template react`

3.进入项目文件夹
`cd my-react-app`
4.安装依赖
`npm install`
5. 启动
`npm run dev`
## jsx
- jsx is a syxtax extension for javascript that looks like html
- it allows you to write html-like code within javascript, which react then transforms into javascript objects
- you can embed any valid javascript expression inside curly braces{} in JSX.
- JSX attributes use camelCase syntax

## prop

```jsx
<ul>
                {hobbies.map((hobby,index) => {
                   return <li key={index}
                   onClick={() => onHobbyClick(hobby)}>
                    {hobby}
                    </li>;
                })}
 </ul>
```

### 第一步：`hobbies.map(...)`

这是 JavaScript 数组的 `map` 方法，意思是“**对 hobbies 数组里的每一个元素，都执行一次某个函数”然后返回一个新数组。**
`hobbies.map((hobby, index) => {...})`
如果 `hobbies = ["reading", "cooking"]`，那么 `map` 会：

```
|第几次|hobby的值|index的值|
|第一次|"reading"|0|
|第二次|"cooking"|1|
```

### 第二步：返回一个 `<li> `元素
```jsx
return <li key={index}
                   onClick={() => onHobbyClick(hobby)}>
                   {hobby}
                   </li>;
```

`key={index}` —— 用来给每个 `<li>` 一个“身份ID”
`onClick={() => onHobbyClick(hobby)}` —— 绑定点击事件
- `onClick` 是 HTML 的点击事件（Click Event）。
- 当你点击 `<li>` 的时候，这段代码会被执行：
`() => onHobbyClick(hobby)`
意思是：
> “当这个列表项被点击，就执行 `onHobbyClick` 这个函数，并把当前这个 hobby 的值传进去”。

举例：
```js
hobby = 'reading'
onClick = () => onHobbyClick('reading')
```
再看一下你在 `App.jsx` 里传入的函数：
```js
const handleHobbyClick = (hobby) => {
  alert(`You clicked on: ${hobby}`);
}
```

所以当你点击 “reading” 这一行，效果是：
💥 弹出框：
```js
You clicked on: reading
```
## `{hobby}` —— JSX 插入变量
- 这是 JSX 的语法规则：**用大括号 `{}` 来嵌入变量、表达式**。
- 所以：
```jsx
<li> {hobby} </li>
```
的意思是：
```jsx
<li>reading</li>    // 如果 hobby 是 'reading'
```

## State和Props
![04261.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/04261.PNG)

### 287节课useState例子
导入部分
```jsx
import './App.css'
import { useState } from 'react';
//useState
```

```jsx
function App(){
  const [counters,setCounters] = useState([{id:1,value:0}]);
  const addCounter = () => {
    setCounters([...counters,{id:counters.length + 1,value:0}]);
  };

  const incrementCounter = (id) => {
    setCounters(counters.map(counter =>
      counter.id === id ? {...counter,value:counter.value + 1} : counter
    )
  );
  };
  
  return (
    <div>
      <button onClick={addCounter}>Add Counter</button>
      <ul>
        {counters.map(counter => (
          <li key={counter.id}>
            Counter {counter.id}:{counter.value}
            <button onClick={() => incrementCounter(counter.id)}>Increment</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
export default App;
```

```jsx
const [counters, setCounters] = useState([{id:1,value:0}]);
```
语法`const [状态变量, 更新状态的方法] = useState(初始值);`
#### 1. `useState([{id:1,value:0}])`
- `useState` 是 React 给函数组件提供的一个 **"状态钩子"**，让你可以在函数内部创建状态。
- `useState(初始值)` 会**返回一个数组**，数组里有两项：
    - 第一项是**当前的状态值**。    
    - 第二项是**修改这个状态的函数**。    

这里的初始值是 `[{id:1,value:0}]`，是一个**只有一个对象的数组**。
- 每个对象代表一个计数器，有两个字段：
    - `id: 1`（计数器编号）    
    - `value: 0`（当前计数值）

#### 2. `const [counters, setCounters] = ...`
这是**数组解构赋值**的语法，意思是：

|变量名|代表的内容|
|---|---|
|`counters`|当前的状态值（也就是 `[ { id:1, value:0 } ]`）|
|`setCounters`|用来更新 `counters` 的函数|
**`setCounters` 是由 React 固定生成的方法**，**专门配套** `useState` 使用的。
`setCounters` 这个名字本身**不是React固定的名字**，而是因为你写的这行：
`const [counters, setCounters] = useState([{id:1,value:0}]);`
在这一行里，**你自己命名了它叫 `setCounters`**！
React `useState` 返回的是一个**数组**，里面两个元素：
- 第一个元素是当前状态（你这里叫 `counters`）
- 第二个元素是更新状态的方法（你自己取了名字叫 `setCounters`）
所以其实你完全可以换名字，比如：
`const [abc, setAbc] = useState([{id:1,value:0}]);`
#### ...counter
`{...counter, value: counter.value + 1}`：
- 这也是展开运算符。
- 它复制这个 `counter` 的所有属性（比如 `id`），然后**用新的 `value` 覆盖原来的 `value`**。

举个例子：`{id:2, value:3}` 点一次后变成 `{id:2, value:4}`。


## useEffect
```jsx
import './App.css';
import { useEffect, useState } from 'react';  

function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `Count:${count}`;
  },[count]);

  const incrementCount = () => {
    setCount(count + 1);
  };
  return (
    <div>
      <h1>useEffect Hook</h1>
      <button onClick={incrementCount}>Increment</button>
    </div>
  );
}  
export default App;
```

```jsx
useEffect(() => {
  document.title = `Count:${count}`;
}, [count]);
```
最后这个 `[count]`，叫做 **依赖数组**（Dependency Array）。`count` 的**初始值**，确实是在`const [count, setCount] = useState(0);`这里设置的 0
它的作用就是：
告诉 React：
> **"只有当 `count` 变化的时候，才重新执行上面那个函数。"**

**如果没有[count]会怎样？**
**不加依赖数组**，每次**组件更新（不管什么变化）**都会执行 `useEffect`，即使跟 `count` 无关，也要重新执行一次。
这样其实**浪费性能**。

**如果[count]是空的[]呢？**
**空数组**，意思是：
- **只在第一次组件加载的时候执行一次**。
- 以后不管 `count` 怎么变化，都不会再执行这个 `useEffect` 了。
- 适合那种“只需要在页面一开始设置一次”的情况，比如页面加载时发送一次请求。


## useRef 和 useState
```jsx
import './App.css';
import React,{ useRef,useState } from 'react'; 

function App() {
  const [stateCount, setStateCount] = useState(0);
  const refCount = useRef(0);
  const incrementStateCount = () => {
    setStateCount(stateCount + 1);
  };
  const incrementRefCount = () => {
    refCount.current += 1;
    console.log(`Ref Count: ${refCount.current}`);
  };

  return (
    <div>
      <p>State Count: {stateCount}</p>
      <buttoon onClick={incrementStateCount}>Increment State Count</buttoon>
      <p>Ref Count: {refCount.current}</p>
      <button onClick={incrementRefCount}>Increment Ref Count</button>
    </div>
  );
}

export default App;
```

### useState 特点
- 用于定义**会触发组件重新渲染的状态变量**。
- 每次你用 `setStateCount` 修改 `stateCount` 时，组件会自动重新渲染，页面也会更新。
```jsx
const incrementStateCount = () => {
  setStateCount(stateCount + 1); // 会触发 UI 更新
};
```
在页面中：
```jsx
<p>State Count: {stateCount}</p>
```
点击按钮时，会立刻看到数字增加。

### useRef 特点
- 用于**保存可变的值但不会触发重新渲染**。
- 改变 `refCount.current` 不会让组件刷新。
- 更适合保存 DOM 元素引用或计数器等临时值。

```jsx
const incrementRefCount = () => {
  refCount.current += 1;
  console.log(`Ref Count: ${refCount.current}`); // 控制台会更新，但 UI 不变
};
```
在页面中：
```jsx
<p>Ref Count: {refCount.current}</p>
```
点击按钮时，页面上的数字**不会改变**，但控制台会打印更新后的值，说明确实变了，只是 UI 没更新。

### 点击Increment Ref Count很多次UI没反应，但是再点击一次Increment State Count,Ref Count就有反应

因为useRef不会触发组件更新，useState会触发渲染，当组件被重新渲染时，React 会重新执行 `return (...)` 里面的 JSX，也就重新渲染了：
`<p>Ref Count: {refCount.current}</p>`
就“突然看到了”之前累积的 `refCount.current` 的值。
