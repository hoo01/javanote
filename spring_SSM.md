# IOC入门案例
### IOC入门案例

对于入门案例，我们得先`分析思路`然后再`代码实现`，

#### 入门案例思路分析

(1)Spring是使用容器来管理bean对象的，那么管什么?
- 主要管理项目中所使用到的类对象，比如(Service和Dao)

(2)如何将被管理的对象告知IOC容器?
- 使用配置文件

(3)被管理的对象交给IOC容器，要想从容器中获取对象，就先得思考如何获取到IOC容器?
- Spring框架提供相应的接口

(4)IOC容器得到后，如何从容器中获取bean?
- 调用Spring框架提供对应接口中的方法

(5)使用Spring导入哪些坐标?
- 用别人的东西，就需要在pom.xml添加对应的依赖

####  入门案例代码实现

> 需求分析:将BookServiceImpl和BookDaoImpl交给Spring管理，并从容器中获取对应的bean对象进行方法调用。
> 
> 1.创建Maven的java项目
> 2.pom.xml添加Spring的依赖jar包
> 3.创建BookService,BookServiceImpl，BookDao和BookDaoImpl四个类
> 4.resources下添加spring配置文件，并完成bean的配置
> 5.使用Spring提供的接口完成IOC容器的创建
> 6.从容器中获取对象进行方法调用

##### 步骤1:创建Maven项目
![1629734010072.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/1629734010072.png)
##### 步骤2:添加Spring的依赖jar包
pom.xml
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.2.10.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```
##### 步骤3:添加案例中需要的类
创建BookService,BookServiceImpl，BookDao和BookDaoImpl四个类
```java
public interface BookDao {
    public void save();
}
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ...");
    }
}
public interface BookService {
    public void save();
}
public class BookServiceImpl implements BookService {
    private BookDao bookDao = new BookDaoImpl();
    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```
##### 步骤4:添加spring配置文件
resources下添加spring配置文件applicationContext.xml，并完成bean的配置
![1629734336440.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/1629734336440.png)
##### 步骤5:在配置文件中完成bean的配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <!--bean标签标示配置bean
    	id属性标示给bean起名字
    	class属性表示给bean定义类型
	-->
	<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl"/>

</beans>
```
**==注意事项：bean定义时id属性在同一个上下文中(配置文件)不能重复==**


##### 步骤6:获取IOC容器
使用Spring提供的接口完成IOC容器的创建，创建App类，编写main方法
```java
public class App {
    public static void main(String[] args) {
        //获取IOC容器
		ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml"); 
    }
}
```
##### 步骤7:从容器中获取对象进行方法调用
```java
public class App {
    public static void main(String[] args) {
        //获取IOC容器
		ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml"); 
//        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
//        bookDao.save();
        BookService bookService = (BookService) ctx.getBean("bookService");
        bookService.save();
    }
}
```
##### 步骤8:运行程序

测试结果为：
![image-20210729184337603.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/image-20210729184337603.png)
Spring的IOC入门案例已经完成，但是在`BookServiceImpl`的类中依然存在`BookDaoImpl`对象的new操作，它们之间的耦合度还是比较高，这块该如何解决，就需要用到下面的`DI:依赖注入`。

# DI入门案例
对于DI的入门案例，我们依然先`分析思路`然后再`代码实现`
#### 入门案例思路分析

(1)要想实现依赖注入，必须要基于IOC管理Bean
- DI的入门案例要依赖于前面IOC的入门案例

(2)Service中使用new形式创建的Dao对象是否保留?
- 需要删除掉，最终要使用IOC容器中的bean对象

(3)Service中需要的Dao对象如何进入到Service中?
- 在Service中提供方法，让Spring的IOC容器可以通过该方法传入bean对象

(4)Service与Dao间的关系如何描述?
- 使用配置文件
#### 入门案例代码实现

> 需求:基于IOC入门案例，在BookServiceImpl类中删除new对象的方式，使用Spring的DI完成Dao层的注入
> 
> 1.删除业务层中使用new的方式创建的dao对象
> 2.在业务层提供BookDao的setter方法
> 3.在配置文件中添加依赖注入的配置
> 4.运行程序调用方法
##### 步骤1: 去除代码中的new
在BookServiceImpl类中，删除业务层中使用new的方式创建的dao对象
```java
public class BookServiceImpl implements BookService {
    //删除业务层中使用new的方式创建的dao对象
    private BookDao bookDao;

    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```
##### 步骤2:为属性提供setter方法
在BookServiceImpl类中,为BookDao提供setter方法
```java
public class BookServiceImpl implements BookService {
    //删除业务层中使用new的方式创建的dao对象
    private BookDao bookDao;

    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
    //提供对应的set方法
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
}
```
##### 步骤3:修改配置完成注入
在配置文件中添加依赖注入的配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--bean标签标示配置bean
    	id属性标示给bean起名字
    	class属性表示给bean定义类型
	-->
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>

    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <!--配置server与dao的关系-->
        <!--property标签表示配置当前bean的属性
        		name属性表示配置哪一个具体的属性
        		ref属性表示参照哪一个bean
		-->
        <property name="bookDao" ref="bookDao"/>
    </bean>

</beans>
```
==注意:配置中的两个bookDao的含义是不一样的==
- name="bookDao"中`bookDao`的作用是让Spring的IOC容器在获取到名称后，将首字母大写，前面加set找对应的`setBookDao()`方法进行对象注入
- ref="bookDao"中`bookDao`的作用是让Spring能在IOC容器中找到id为`bookDao`的Bean对象给`bookService`进行注入
- 综上所述，对应关系如下:
![1629736314989.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/1629736314989.png)
##### 步骤4:运行程序
同之前

# IOC相关内容
## bean配置
### bean基础配置

对于bean的配置中，主要会讲解`bean基础配置`,`bean的别名配置`,`bean的作用范围配置`==(重点)==,这三部分内容：
#### bean基础配置(id与class)

对于bean的基础配置，在前面的案例中已经使用过:
`<bean id="" class=""/>`
其中，bean标签的功能、使用方式以及id和class属性的作用，我们通过一张图来描述下
![image-20210729183500978.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/image-20210729183500978.png)
这其中需要大家重点掌握的是:==bean标签的id和class属性的使用==。

**思考：**

- class属性能不能写接口如`BookDao`的类全名呢?
    答案肯定是不行，因为接口是没办法创建对象的。
- 前面提过为bean设置id时，id必须唯一，但是如果由于命名习惯而产生了分歧后，该如何解决?
    在解决这个问题之前，我们需要准备下开发环境，对于开发环境我们可以有两种解决方案:
- 使用前面IOC和DI的案例
- 重新搭建一个新的案例环境,目的是方便大家查阅代码
    - 搭建的内容和前面的案例是一样的，内容如下：
     ![1629769227068.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/1629769227068.png)
#### bean的name属性

环境准备好后，接下来就可以在这个环境的基础上来学习下bean的别名配置，
首先来看下别名的配置说明:
![image-20210729183558051.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/image-20210729183558051.png)
##### 步骤1：配置别名
打开spring的配置文件applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--name:为bean指定别名，别名可以有多个，使用逗号，分号，空格进行分隔-->
    <bean id="bookService" name="service service4 bookEbi" class="com.itheima.service.impl.BookServiceImpl">
        <property name="bookDao" ref="bookDao"/>
    </bean>

    <!--scope：为bean设置作用范围，可选值为单例singloton，非单例prototype-->
    <bean id="bookDao" name="dao" class="com.itheima.dao.impl.BookDaoImpl"/>
</beans>
```
**说明:Ebi全称Enterprise Business Interface，翻译为企业业务接口**
##### 步骤2:根据名称容器中获取bean对象
```java
public class AppForName {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        //此处根据bean标签的id属性和name属性的任意一个值来获取bean对象
        BookService bookService = (BookService) ctx.getBean("service4");
        bookService.save();
    }
}
```
##### 步骤3:运行程序

测试结果同上
#### bean作用范围scope配置

关于bean的作用范围是bean属性配置的一个==重点==内容。

看到这个作用范围，我们就得思考bean的作用范围是来控制bean哪块内容的?

我们先来看下`bean作用范围的配置属性`:
![image-20210729183628138.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/image-20210729183628138.png)
在Spring配置文件中，配置scope属性来实现bean的非单例创建

- 在Spring的配置文件中，修改`<bean>`的scope属性，将scope设置为`prototype`
```xml
<bean id="bookDao" name="dao" class="com.itheima.dao.impl.BookDaoImpl" scope="prototype"/>
```
##### scope使用后续思考

介绍完`scope`属性以后，我们来思考几个问题:

- 为什么bean默认为单例?
    - bean为单例的意思是在Spring的IOC容器中只会有该类的一个对象
    - bean对象只有一个就避免了对象的频繁创建与销毁，达到了bean对象的复用，性能高
- bean在容器中是单例的，会不会产生线程安全问题?
    - 如果对象是有状态对象，即该对象有成员变量可以用来存储数据的，
    - 因为所有请求线程共用一个bean对象，所以会存在线程安全问题。
    - 如果对象是无状态对象，即该对象没有成员变量没有进行数据存储的，
    - 因方法中的局部变量在方法调用完成后会被销毁，所以不会存在线程安全问题。
- 哪些bean对象适合交给容器进行管理?
    - 表现层对象
    - 业务层对象
    - 数据层对象
    - 工具对象
- 哪些bean对象不适合交给容器进行管理?
    - 封装实例的域对象，因为会引发线程安全问题，所以不适合。
### 总结
![1631529887695.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/1631529887695.png)

## bean实例化的四种方法

- 实例化bean的三种方式，`构造方法`,`静态工厂`和`实例工厂`
#### 环境准备

为了方便大家阅读代码，重新准备个开发环境，

- 创建一个Maven项目
- pom.xml添加依赖
- resources下添加spring的配置文件applicationContext.xml

这些步骤和前面的都一致，大家可以快速的拷贝即可，最终项目的结构如下:
![1629775585694.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/1629775585694.png)
#### 第一种创建bean的方式：构造方法实例化(重要)
在上述的环境下，我们来研究下Spring中的第一种bean的创建方式`构造方法实例化`:
##### 步骤1:准备需要被创建的类
准备一个BookDao和BookDaoImpl类
```java
public interface BookDao {
    public void save();
}

public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ...");
    }
}
```
##### 步骤2:将类配置到Spring容器
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>

</beans>
```
##### 步骤3:编写运行程序
```java
public class AppForInstanceBook {
    public static void main(String[] args) {
        ApplicationContext ctx = new 
            ClassPathXmlApplicationContext("applicationContext.xml");
        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
        bookDao.save();
    }
}
```
##### 类中提供构造函数测试
在BookDaoImpl类中添加一个无参构造函数，并打印一句话，方便观察结果。
```java
public class BookDaoImpl implements BookDao {
    public BookDaoImpl() {
        System.out.println("book dao constructor is running ....");
    }
    public void save() {
        System.out.println("book dao save ...");
    }

}
```
##### 将构造函数改成private测试
```java
public class BookDaoImpl implements BookDao {
    private BookDaoImpl() {
        System.out.println("book dao constructor is running ....");
    }
    public void save() {
        System.out.println("book dao save ...");
    }
}
```
运行程序，能执行成功,说明内部走的依然是构造函数,能访问到类中的私有构造方法,显而易见Spring底层用的是反射
##### 构造函数中添加一个参数测试
```java
public class BookDaoImpl implements BookDao {
    private BookDaoImpl(int i) {
        System.out.println("book dao constructor is running ....");
    }
    public void save() {
        System.out.println("book dao save ...");
    }
}
```
运行会报错，说明Spring底层使用的是类的无参构造方法。
![1629776331499.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/1629776331499.png)

#### 第二种创建bean的方式：静态工厂

##### 静态工厂创建对象vs.构造方法创建对象

定义接口和实现类：
```java
public interface OrderDao {
    public void save();
}

public class OrderDaoImpl implements OrderDao {
    public void save() {
        System.out.println("order dao save ...");
    }
}
```

传统：
```java
OrderDao orderDao = new OrderDaoImpl(); // 直接 new 出来
```
- 使用`new`关键字调用构造方法。
- 这种方式写起来很简单，但**耦合度高**（在用哪个实现类是硬编码写死的）。

工厂方式创建对象：
**专门用来创建 `OrderDao` 对象**，相当于一个“生产 `OrderDao` 的工厂”。
```java
public class OrderDaoFactory {
   public static OrderDao getOrderDao(){
       return new OrderDaoImpl();
   }
}
```

编写AppForInstanceOrder运行类，在类中通过工厂获取对象
```java
public class AppForInstanceOrder {
    public static void main(String[] args) {
        //通过静态工厂创建对象
        OrderDao orderDao = OrderDaoFactory.getOrderDao();
        orderDao.save();
    }
}
```
- 工厂方式**没有直接用构造方法**，而是通过一个**额外的“工厂类”**来帮你 new 对象。
- 工厂类内部还是会调用构造方法（比如 `new OrderDaoImpl()`），但对外屏蔽了这个细节。只管拿对象，不用关心怎么 new 的、new 了哪个。

##### 静态工厂方式创建bean

###### 静态工厂实例化
(1)在spring的配置文件application.properties中添加以下内容:
```java
<bean id="orderDao" class="com.itheima.factory.OrderDaoFactory" factory-method="getOrderDao"/>
```
class:工厂类的类全名
factory-mehod:具体工厂类中创建对象的方法名
对应关系如下图:
![image-20210729195248948.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/image-20210729195248948.png)

(2)在AppForInstanceOrder运行类，使用从IOC容器中获取bean的方法进行运行测试
```java
public class AppForInstanceOrder {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");

        OrderDao orderDao = (OrderDao) ctx.getBean("orderDao");

        orderDao.save();

    }
}
```

看到这，可能有人会问了，你这种方式在工厂类中不也是直接new对象的，和我自己直接new没什么太大的区别，而且静态工厂的方式反而更复杂，这种方式的意义是什么?

主要的原因是:

- 在工厂的静态方法中，我们除了new对象还可以做其他的一些业务操作，这些操作必不可少,如:
```java
public class OrderDaoFactory {
    public static OrderDao getOrderDao(){
        System.out.println("factory setup....");//模拟必要的业务操作
        return new OrderDaoImpl();
    }
}
```
如果是之前new对象的方式就无法添加其他的业务内容
不过，这种方式一般是用来兼容早期的一些老系统，所以==了解为主==。

#### 第三种创建bean方式：实例工厂
##### 实例工厂创建对象
###### 环境准备
(1)准备一个UserDao和UserDaoImpl类
```java
public interface UserDao {
    public void save();
}

public class UserDaoImpl implements UserDao {

    public void save() {
        System.out.println("user dao save ...");
    }
}
```
(2)创建一个工厂类OrderDaoFactory并提供一个普通方法，注意此处和静态工厂的工厂类不一样的地方是方法不是静态方法
```java
public class UserDaoFactory {
    public UserDao getUserDao(){
        return new UserDaoImpl();
    }
}
```
(3)编写AppForInstanceUser运行类，在类中通过工厂获取对象
```java
public class AppForInstanceUser {
    public static void main(String[] args) {
        //创建实例工厂对象
        UserDaoFactory userDaoFactory = new UserDaoFactory();
        //通过实例工厂对象创建对象
        UserDao userDao = userDaoFactory.getUserDao();
        userDao.save();
}
```
(4)运行查看结果
##### 实例工厂实例化
具体实现步骤为:
(1)在spring的配置文件中添加以下内容:
```xml
<bean id="userFactory" class="com.itheima.factory.UserDaoFactory"/>
<bean id="userDao" factory-method="getUserDao" factory-bean="userFactory"/>
```
实例化工厂运行的顺序是:
* 创建实例化工厂对象,对应的是第一行配置
* 调用对象中的方法来创建bean，对应的是第二行配置
  * factory-bean:工厂的实例对象
  * factory-method:工厂对象中的具体创建对象的方法名,对应关系如下:
![image-20210729200203249.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/image-20210729200203249.png)

factory-mehod:具体工厂类中创建对象的方法名

(2)在AppForInstanceUser运行类，使用从IOC容器中获取bean的方法进行运行测试
```java
public class AppForInstanceUser {
    public static void main(String[] args) {
        ApplicationContext ctx = new 
            ClassPathXmlApplicationContext("applicationContext.xml");
        UserDao userDao = (UserDao) ctx.getBean("userDao");
        userDao.save();
    }
}
```

#### FactoryBean(重要)
实例工厂实例化的方式就已经介绍完了，配置的过程还是比较复杂，所以Spring为了简化这种配置方式就提供了一种叫`FactoryBean`的方式来简化开发。
##### FactoryBean的使用
具体的使用步骤为:
(1)创建一个UserDaoFactoryBean的类，实现FactoryBean接口，重写接口的方法
```java
public class UserDaoFactoryBean implements FactoryBean<UserDao> {
    //代替原始实例工厂中创建对象的方法
    public UserDao getObject() throws Exception {
        return new UserDaoImpl();
    }
    //返回所创建类的Class对象
    public Class<?> getObjectType() {
        return UserDao.class;
    }
}
```
- `getObject()` 是核心方法，返回你想让 Spring 管理的对象。
- `getObjectType()` 告诉 Spring 返回对象的类型，Spring 会根据它决定如何注入依赖。

(2)在Spring的配置文件中进行配置
```xml
<bean id="userDao" class="com.itheima.factory.UserDaoFactoryBean"/>
```
(3)AppForInstanceUser运行类不用做任何修改，直接运行

## Bean的生命周期
- bean生命周期是什么?
    - bean对象从创建到销毁的整体过程。
- bean生命周期控制是什么?
    - 在bean创建后到销毁前做一些事情。

## 生命周期设置
为BookDao添加生命周期的控制方法，具体的控制有两个阶段:
- bean创建之后，想要添加内容，比如用来初始化需要用到资源
- bean销毁之前，想要添加内容，比如用来释放用到的资源

##### 步骤1:添加初始化和销毁方法
针对这两个阶段，我们在BooDaoImpl类中分别添加两个方法，==方法名任意==
```java
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ...");
    }
    //表示bean初始化对应的操作
    public void init(){
        System.out.println("init...");
    }
    //表示bean销毁前对应的操作
    public void destory(){
        System.out.println("destory...");
    }
}
```
##### 步骤2:配置生命周期
在配置文件添加配置，如下:
```xml
<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl" init-method="init" destroy-method="destory"/>
```
##### 步骤3:运行程序
运行AppForLifeCycle打印结果为:
![1629792339889.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/1629792339889.png)

从结果中可以看出，init方法执行了，但是destroy方法却未执行，这是为什么呢?
- Spring的IOC容器是运行在JVM中
- 运行main方法后,JVM启动,Spring加载配置文件生成IOC容器,从容器获取bean对象，然后调方法执行
- main方法执行完后，JVM退出，这个时候IOC容器中的bean还没有来得及销毁就已经结束了
- 所以没有调用对应的destroy方法

知道了出现问题的原因，具体该如何解决呢?
#### close关闭容器
- ApplicationContext中没有close方法
- 需要将ApplicationContext更换成ClassPathXmlApplicationContext
```java
ClassPathXmlApplicationContext ctx = new 
    ClassPathXmlApplicationContext("applicationContext.xml");
```
调用ctx的close()方法
`ctx.close();`
就能执行destroy的内容
#### 注册钩子关闭容器
- 在容器未关闭之前，提前设置好回调函数，让JVM在退出之前回调此函数来关闭容器
- 调用ctx的registerShutdownHook()方法
`ctx.registerShutdownHook();`
注意:registerShutdownHook在ApplicationContext中也没有
这样也能执行destroy
两种方式介绍完后，close和registerShutdownHook选哪个?

相同点:这两种都能用来关闭容器
不同点:close()是在调用的时候关闭，registerShutdownHook()是在JVM退出前调用关闭。
分析上面的实现过程，会发现添加初始化和销毁方法，即需要编码也需要配置，实现起来步骤比较多也比较乱。
#### `InitializingBean`， `DisposableBean`
Spring提供了两个接口`InitializingBean`， `DisposableBean`来完成生命周期的控制，并实现接口中的两个方法`afterPropertiesSet`和`destroy`可以不用再进行配置`init-method`和`destroy-method`

```java
public class BookServiceImpl implements BookService, InitializingBean, DisposableBean {
    private BookDao bookDao;
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
    public void save() {
        System.out.println("book service save ...");
        bookDao.save(); 
    }
    public void destroy() throws Exception {
        System.out.println("service destroy");
    }
    public void afterPropertiesSet() throws Exception {
        System.out.println("service init");
    }
}
```

重新运行AppForLifeCycle类，
![1629794527419.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/1629794527419.png)

#### bean生命周期小结

(1)关于Spring中对bean生命周期控制提供了两种方式:
- 在配置文件中的bean标签中添加`init-method`和`destroy-method`属性
- 类实现`InitializingBean`与`DisposableBean`接口，这种方式了解下即可。

(2)对于bean的生命周期控制在bean的整个生命周期中所处的位置如下:
- 初始化容器
    - 1.创建对象(内存分配)
    - 2.执行构造方法
    - 3.执行属性注入(set操作)
    - ==4.执行bean初始化方法==
- 使用bean
    - 1.执行业务操作
- 关闭/销毁容器
    - ==1.执行bean销毁方法==

(3)关闭容器的两种方式:
- ConfigurableApplicationContext是ApplicationContext的子类
    - close()方法
    - registerShutdownHook()方法

# DI相关内容
### setter注入

BookDao.java
```java
public interface BookDao {
    public void save();
}
```
它只是定义了一个“规范”，说“凡是实现了 BookDao 的类，必须有 `save()` 方法”。

BookServiceImpl.java
1. 在bean中定义引用类型属性，并提供可访问的==set==方法
```java
public class BookServiceImpl implements BookService {
//- 需要一个 `BookDao` 类型的对象,用这个对象来调用 `save()` 方法
    private BookDao bookDao;
    //Spring不能直接访问 private 的字段，所以它必须通过公有的 setter 方法来进行注入。
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
}
```
2.配置xml,这一步就完成了 **依赖注入**！
```xml
<bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
	<property name="bookDao" ref="bookDao"/>
	//前者是java类中的属性名，告诉spring:要给BookServiceImpl的bookDao属性赋值，后者是告诉spring用这个id所对应的Bean来注入
</bean>

<bean id="bookDao" class="com.itheima.dao.imipl.BookDaoImpl"/>
```
- 配置中使用==property==标签==ref==属性注入引用类型对象
    Spring 会做两件事：
    1. 创建 `BookServiceImpl` 的对象
    2. 找到它的 `setBookDao(BookDao bookDao)` 方法，然后把名为 `bookDao` 的 Bean 传进去
#### 环境准备
步骤和前面的都一致
(1)项目中添加BookDao、BookDaoImpl、UserDao、UserDaoImpl、BookService和BookServiceImpl类
```java
public interface BookDao {
    public void save();
}

public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ...");
    }
}
public interface UserDao {
    public void save();
}
public class UserDaoImpl implements UserDao {
    public void save() {
        System.out.println("user dao save ...");
    }
}

public interface BookService {
    public void save();
}

public class BookServiceImpl implements BookService{
//`BookServiceImpl` 自己已经是 `BookService` 的实现类
    private BookDao bookDao;

    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }

    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```

(2)resources下提供spring的配置文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <property name="bookDao" ref="bookDao"/>
    </bean>
</beans>
```

(3)编写AppForDISet运行类，加载Spring的IOC容器，并从中获取对应的bean对象
```java
public class AppForDISet {
    public static void main( String[] args ) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        BookService bookService = (BookService) ctx.getBean("bookService");
        bookService.save();
    }
}
```
接下来，在上面这个环境中来完成setter注入的学习:
#### 注入引用数据类型
> 需求:在bookServiceImpl对象中注入userDao
> 
> 1.在BookServiceImpl中声明userDao属性
> 2.为userDao属性提供setter方法
> 3.在配置文件中使用property标签注入
##### 步骤1:声明属性并提供setter方法
在BookServiceImpl中声明userDao属性，并提供setter方法
```java
public class BookServiceImpl implements BookService{
    private BookDao bookDao;
    private UserDao userDao;
    
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }

    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
        userDao.save();
    }
}
```

##### 步骤2:配置文件中进行注入配置
在applicationContext.xml配置文件中使用property标签注入
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
    <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <property name="bookDao" ref="bookDao"/>
        <property name="userDao" ref="userDao"/>
    </bean>
</beans>
```
##### 步骤3:运行程序
运行AppForDISet类，查看结果，说明userDao已经成功注入。
#### 注入简单数据类型
需求：给BookDaoImpl注入一些简单数据类型的数据

参考引用数据类型的注入，我们可以推出具体的步骤为:
1.在BookDaoImpl类中声明对应的简单数据类型的属性
2.为这些属性提供对应的setter方法
3.在applicationContext.xml中配置
**思考:**

引用类型使用的是`<property name="" ref=""/>`,简单数据类型还是使用ref么?

ref是指向Spring的IOC容器中的另一个bean对象的，对于简单数据类型，没有对应的bean对象，该如何配置?

##### 步骤1:声明属性并提供setter方法
在BookDaoImpl类中声明对应的简单数据类型的属性,并提供对应的setter方法
```java
public class BookDaoImpl implements BookDao {

    private String databaseName;
    private int connectionNum;

    public void setConnectionNum(int connectionNum) {
        this.connectionNum = connectionNum;
    }

    public void setDatabaseName(String databaseName) {
        this.databaseName = databaseName;
    }

    public void save() {
        System.out.println("book dao save ..."+databaseName+","+connectionNum);
    }
}
```
##### 步骤2:配置文件中进行注入配置

在applicationContext.xml配置文件中使用property标签注入
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
        <property name="databaseName" value="mysql"/>
     	<property name="connectionNum" value="10"/>
    </bean>
    <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <property name="bookDao" ref="bookDao"/>
        <property name="userDao" ref="userDao"/>
    </bean>
</beans>
```
**说明:**

value:后面跟的是简单数据类型，对于参数类型，Spring在注入的时候会自动转换，但是不能写成
`<property name="connectionNum" value="abc"/>`
这样的话，spring在将`abc`转换成int类型的时候就会报错。

##### 步骤3:运行程序

运行AppForDISet类，查看结果，说明userDao已经成功注入。
**注意:**两个property注入标签的顺序可以任意。

对于setter注入方式的基本使用就已经介绍完了，

- 对于引用数据类型使用的是`<property name="" ref=""/>`
- 对于简单数据类型使用的是`<property name="" value=""/>`
### 构造器注入

#### 环境准备

构造器注入也就是构造方法注入，学习之前，还是先准备下环境:

- 创建一个Maven项目
- pom.xml添加依赖
- resources下添加spring的配置文件

这些步骤和前面的都一致
(1)项目中添加BookDao、BookDaoImpl、UserDao、UserDaoImpl、BookService和BookServiceImpl类，同之前
(2)resources下提供spring的配置文件，同之前
(3)编写AppForDIConstructor运行类，加载Spring的IOC容器，并从中获取对应的bean对象，同之前

#### 构造器注入引用数据类型

接下来，在上面这个环境中来完成构造器注入的学习:

> 需求：将BookServiceImpl类中的bookDao修改成使用构造器的方式注入。
> 1.将bookDao的setter方法删除掉
> 2.添加带有bookDao参数的构造方法
> 3.在applicationContext.xml中配置

##### 步骤1:删除setter方法并提供构造方法
在BookServiceImpl类中将bookDao的setter方法删除掉,并添加带有bookDao参数的构造方法
```java
public class BookServiceImpl implements BookService{
    private BookDao bookDao;

    public BookServiceImpl(BookDao bookDao) {
        this.bookDao = bookDao;
    }

    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```
##### 步骤2:配置文件中进行配置构造方式注入

在applicationContext.xml中配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <constructor-arg name="bookDao" ref="bookDao"/>
    </bean>
</beans>
```
**说明:**

标签`<constructor-arg>`中
name属性对应的值为构造函数中方法形参的参数名，必须要保持一致。
ref属性指向的是spring的IOC容器中其他bean对象。

##### 步骤3：运行程序
运行AppForDIConstructor类，查看结果，说明bookDao已经成功注入。

#### 构造器注入多个引用数据类型

> 需求:在BookServiceImpl使用构造函数注入多个引用数据类型，比如userDao
> 
> 1.声明userDao属性
> 
> 2.生成一个带有bookDao和userDao参数的构造函数
> 
> 3.在applicationContext.xml中配置注入

##### 步骤1:提供多个属性的构造函数

在BookServiceImpl声明userDao并提供多个参数的构造函数
```java
public class BookServiceImpl implements BookService{
    private BookDao bookDao;
    private UserDao userDao;

    public BookServiceImpl(BookDao bookDao,UserDao userDao) {
        this.bookDao = bookDao;
        this.userDao = userDao;
    }

    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
        userDao.save();
    }
}
```
##### 步骤2:配置文件中配置多参数注入

在applicationContext.xml中配置注入
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
    <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <constructor-arg name="bookDao" ref="bookDao"/>
        <constructor-arg name="userDao" ref="userDao"/>
    </bean>
</beans>
```
**说明:**这两个`<contructor-arg>`的配置顺序可以任意

##### 步骤3:运行程序

运行AppForDIConstructor类，查看结果，说明userDao已经成功注入。
#### 构造器注入多个简单数据类型

> 需求:在BookDaoImpl中，使用构造函数注入databaseName和connectionNum两个参数。
> 参考引用数据类型的注入，我们可以推出具体的步骤为:
> 1.提供一个包含这两个参数的构造方法
> 2.在applicationContext.xml中进行注入配置

##### 步骤1:添加多个简单属性并提供构造方法
修改BookDaoImpl类，添加构造方法
```java
public class BookDaoImpl implements BookDao {
    private String databaseName;
    private int connectionNum;

    public BookDaoImpl(String databaseName, int connectionNum) {
        this.databaseName = databaseName;
        this.connectionNum = connectionNum;
    }

    public void save() {
        System.out.println("book dao save ..."+databaseName+","+connectionNum);
    }
}
```
##### 步骤2:配置完成多个属性构造器注入

在applicationContext.xml中进行注入配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
        <constructor-arg name="databaseName" value="mysql"/>
        <constructor-arg name="connectionNum" value="666"/>
    </bean>
    <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <constructor-arg name="bookDao" ref="bookDao"/>
        <constructor-arg name="userDao" ref="userDao"/>
    </bean>
</beans>
```
**说明:**这两个`<contructor-arg>`的配置顺序可以任意

##### 步骤3:运行程序
上面已经完成了构造函数注入的基本使用，但是会存在一些问题:
![1629803529598.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/1629803529598.png)
- 当构造函数中方法的参数名发生变化后，配置文件中的name属性也需要跟着变
- 这两块存在紧耦合，具体该如何解决?
    

在解决这个问题之前，需要提前说明的是，这个参数名发生变化的情况并不多，所以上面的还是比较主流的配置方式，下面介绍的，大家都以了解为主。

方式一:删除name属性，添加type属性，按照类型注入
```xml
<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
    <constructor-arg type="int" value="10"/>
    <constructor-arg type="java.lang.String" value="mysql"/>
</bean>
```
- 这种方式可以解决构造函数形参名发生变化带来的耦合问题
- 但是如果构造方法参数中有类型相同的参数，这种方式就不太好实现了

方式二:删除type属性，添加index属性，按照索引下标注入，下标从0开始
```xml
<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
    <constructor-arg index="1" value="100"/>
    <constructor-arg index="0" value="mysql"/>
</bean>
```
- 这种方式可以解决参数类型重复问题
    
- 但是如果构造方法参数顺序发生变化后，这种方式又带来了耦合问题
    

介绍完两种参数的注入方式，具体我们该如何选择呢?

1. 强制依赖使用构造器进行，使用setter注入有概率不进行注入导致null对象出现
    - 强制依赖指对象在创建的过程中必须要注入指定的参数
2. 可选依赖使用setter注入进行，灵活性强
    - 可选依赖指对象在创建过程中注入的参数可有可无
3. Spring框架倡导使用构造器，第三方框架内部大多数采用构造器注入的形式进行数据初始化，相对严谨
4. 如果有必要可以两者同时使用，使用构造器注入完成强制依赖的注入，使用setter注入完成可选依赖的注入
5. 实际开发过程中还要根据实际情况分析，如果受控对象没有提供setter方法就必须使用构造器注入
6. **==自己开发的模块推荐使用setter注入==**

#### 总结
这节中主要讲解的是Spring的依赖注入的实现方式:

- setter注入
    - 简单数据类型
```xml
<bean ...>
	<property name="" value=""/>
</bean>
```
引用数据类型
```xml
<bean ...>
	<property name="" ref=""/>
</bean>
```
- 构造器注入
简单数据类型
```xml
<bean ...>
	<constructor-arg name="" index="" type="" value=""/>
</bean>
```
引用数据类型
```xml
<bean ...>
	<constructor-arg name="" index="" type="" ref=""/>
</bean>
```
- 依赖注入的方式选择上
    - 建议使用setter注入
    - 第三方技术根据情况选择


### 自动配置
#### 什么是依赖自动装配?
- IoC容器根据bean所依赖的资源在容器中自动查找并注入到bean中的过程称为自动装配
#### 自动装配方式有哪些?
- ==按类型（常用）==
- 按名称
- 按构造方法
- 不启用自动装配

#### 准备下案例环境
- 创建一个Maven项目
- pom.xml添加依赖
- resources下添加spring的配置文件

(1)项目中添加BookDao、BookDaoImpl、BookService和BookServiceImpl类
```java
public interface BookDao {
    public void save();
}

public class BookDaoImpl implements BookDao {
    
    private String databaseName;
    private int connectionNum;
    
    public void save() {
        System.out.println("book dao save ...");
    }
}
public interface BookService {
    public void save();
}

public class BookServiceImpl implements BookService{
    private BookDao bookDao;

    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }

    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```
(2)resources下提供spring的配置文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <property name="bookDao" ref="bookDao"/>
    </bean>
</beans>
```
(3)编写AppForAutoware运行类，加载Spring的IOC容器，并从中获取对应的bean对象
```java
public class AppForAutoware {
    public static void main( String[] args ) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        BookService bookService = (BookService) ctx.getBean("bookService");
        bookService.save();
    }
}
```
#### 完成自动装配的配置

接下来，在上面这个环境中来完成`自动装配`的学习:
自动装配只需要修改applicationContext.xml配置文件即可:
(1)将`<property>`标签删除
(2)在`<bean>`标签中添加autowire属性
首先来实现按照类型注入的配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean class="com.itheima.dao.impl.BookDaoImpl"/>
    <!--autowire属性：开启自动装配，通常使用按类型装配-->
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl" autowire="byType"/>

</beans>
```
==注意事项:==

* 需要注入属性的类中对应属性的setter方法不能省略
* 被注入的对象必须要被Spring的IOC容器管理
* 按照类型在Spring的IOC容器中如果找到多个对象，会报`NoUniqueBeanDefinitionException`

一个类型在IOC中有多个对象，还想要注入成功，这个时候就需要按照名称注入，配置方式为:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean class="com.itheima.dao.impl.BookDaoImpl"/>
    <!--autowire属性：开启自动装配，通常使用按类型装配-->
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl" autowire="byName"/>

</beans>
```
==注意事项:==

- 按照名称注入中的名称指的是什么?
![1629806856156.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/1629806856156.png)
- - bookDao是private修饰的，外部类无法直接方法
    - 外部类只能通过属性的set方法进行访问
    - 对外部类来说，setBookDao方法名，去掉set后首字母小写是其属性名
        - 为什么是去掉set首字母小写?
        - 这个规则是set方法生成的默认规则，set方法的生成是把属性名首字母大写前面加set形成的方法名
    - 所以按照名称注入，其实是和对应的set方法有关，但是如果按照标准起名称，属性名和set对应的名是一致的
- 如果按照名称去找对应的bean对象，找不到则注入Null
- 当某一个类型在IOC容器中有多个对象，按照名称注入只找其指定名称对应的bean对象，不会报错

两种方式介绍完后，以后用的更多的是==按照类型==注入。
最后对于依赖注入，需要注意一些其他的配置特征:
1. 自动装配用于引用类型依赖注入，不能对简单类型进行操作
2. 使用按类型装配时（byType）必须保障容器中相同类型的bean唯一，推荐使用
3. 使用按名称装配时（byName）必须保障容器中具有指定名称的bean，因变量名与配置耦合，不推荐使用
4. 自动装配优先级低于setter注入与构造器注入，同时出现时自动装配配置失效

### 集合注入

前面我们已经能完成引入数据类型和简单数据类型的注入，但是还有一种数据类型==集合==，集合中既可以装简单数据类型也可以装引用数据类型，对于集合，在Spring中该如何注入呢?

先来回顾下，常见的集合类型有哪些?
- 数组
- List
- Set
- Map
- Properties

针对不同的集合类型，该如何实现注入呢?
#### 环境准备
略过
(1)项目中添加添加BookDao、BookDaoImpl类
```java
public interface BookDao {
    public void save();
}

public class BookDaoImpl implements BookDao {
    
public class BookDaoImpl implements BookDao {

    private int[] array;

    private List<String> list;

    private Set<String> set;

    private Map<String,String> map;

    private Properties properties;

     public void save() {
        System.out.println("book dao save ...");

        System.out.println("遍历数组:" + Arrays.toString(array));

        System.out.println("遍历List" + list);

        System.out.println("遍历Set" + set);

        System.out.println("遍历Map" + map);

        System.out.println("遍历Properties" + properties);
    }
	//setter....方法省略，自己使用工具生成
}
```

(2)resources下提供spring的配置文件，applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
</beans>
```
(3)编写AppForDICollection运行类，加载Spring的IOC容器，并从中获取对应的bean对象
```java
public class AppForDICollection {
    public static void main( String[] args ) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
        bookDao.save();
    }
}
```
接下来，在上面这个环境中来完成`集合注入`的学习:

下面的所以配置方式，都是在bookDao的bean标签中使用`<property>`进行注入
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
        
    </bean>
</beans>
```
#### 注入数组类型数据
```xml
<property name="array">
    <array>
        <value>100</value>
        <value>200</value>
        <value>300</value>
    </array>
</property>
```
#### 注入List类型数据
```xml
<property name="list">
    <list>
        <value>itcast</value>
        <value>itheima</value>
        <value>boxuegu</value>
        <value>chuanzhihui</value>
    </list>
</property>
```
#### 注入Set类型数据
```xml
<property name="set">
    <set>
        <value>itcast</value>
        <value>itheima</value>
        <value>boxuegu</value>
        <value>boxuegu</value>
    </set>
</property>
```
#### 注入Map类型数据
```xml
<property name="map">
    <map>
        <entry key="country" value="china"/>
        <entry key="province" value="henan"/>
        <entry key="city" value="kaifeng"/>
    </map>
</property>
```
#### 注入Properties类型数据
```xml
<property name="properties">
    <props>
        <prop key="country">china</prop>
        <prop key="province">henan</prop>
        <prop key="city">kaifeng</prop>
    </props>
</property>
```
![1629808046783.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/1629808046783.png)
**说明：**
- property标签表示setter方式注入，构造方式注入constructor-arg标签内部也可以写`<array>`、`<list>`、`<set>`、`<map>`、`<props>`标签
- List的底层也是通过数组实现的，所以`<list>`和`<array>`标签是可以混用
- 集合中要添加引用类型，只需要把`<value>`标签改成`<ref>`标签，这种方式用的比较少

# 容器
## 容器的创建方式
案例中创建`ApplicationContext`的方式为:
```java
ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
```
这种方式翻译为:==类路径下的XML配置文件==

除了上面这种方式，Spring还提供了另外一种创建方式为:
```java
ApplicationContext ctx = new FileSystemXmlApplicationContext("applicationContext.xml");
```
这种方式翻译为:==文件系统下的XML配置文件==
使用这种方式，运行，会出现如下错误:

![1629983245121.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/1629983245121.png)
从错误信息中能发现，这种方式是从项目路径下开始查找`applicationContext.xml`配置文件的，所以需要将其修改为:
```java
ApplicationContext ctx = new FileSystemXmlApplicationContext("D:\\workspace\\spring\\spring_10_container\\src\\main\\resources\\applicationContext.xml"); 
```
**说明:大家练习的时候，写自己的具体路径。
这种方式虽能实现，但是当项目的位置发生变化后,代码也需要跟着改,耦合度较高,不推荐使用。
## Bean的三种获取方式

方式一，就是目前案例中获取的方式:
```java
BookDao bookDao = (BookDao) ctx.getBean("bookDao");
```
这种方式存在的问题是每次获取的时候都需要进行类型转换，有没有更简单的方式呢?

方式二：
```java
BookDao bookDao = ctx.getBean("bookDao"，BookDao.class);
```
这种方式可以解决类型强转问题，但是参数又多加了一个，相对来说没有简化多少。

方式三:
```java
BookDao bookDao = ctx.getBean(BookDao.class);
```
这种方式就类似我们之前所学习依赖注入中的按类型注入。必须要确保IOC容器中该类型对应的bean对象只能有一个。
## 容器类层次结构
![微信截图_20250326144235.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250326144235.png)
从图中可以看出，容器类也是从无到有根据需要一层层叠加上来的，大家重点理解下这种设计思想。
## BeanFactory的使用
使用BeanFactory来创建IOC容器的具体实现方式为:
```java
public class AppForBeanFactory {
    public static void main(String[] args) {
        Resource resources = new ClassPathResource("applicationContext.xml");
        BeanFactory bf = new XmlBeanFactory(resources);
        BookDao bookDao = bf.getBean(BookDao.class);
        bookDao.save();
    }
}
```

为了更好的看出`BeanFactory`和`ApplicationContext`之间的区别，在BookDaoImpl添加如下构造函数:
```java
public class BookDaoImpl implements BookDao {
    public BookDaoImpl() {
        System.out.println("constructor");
    }
    public void save() {
        System.out.println("book dao save ..." );
    }
}
```
如果不去获取bean对象，打印会发现：
- BeanFactory是延迟加载，只有在获取bean对象的时候才会去创建
- ApplicationContext是立即加载，容器加载的时候就会创建bean对象
- ApplicationContext要想成为延迟加载，只需要按照如下方式进行配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"  lazy-init="true"/>
</beans>
```
## **小结**

这一节中所讲的知识点包括:

- 容器创建的两种方式
    - ClassPathXmlApplicationContext[掌握]
    - FileSystemXmlApplicationContext[知道即可]
- 获取Bean的三种方式
    - getBean("名称"):需要类型转换
    - getBean("名称",类型.class):多了一个参数
    - getBean(类型.class):容器中不能有多个该类的bean对象
    
    上述三种方式，各有各的优缺点，用哪个都可以。
    
- 容器类层次结构
    - 只需要知晓容器的最上级的父接口为 BeanFactory即可
- BeanFactory
    - 使用BeanFactory创建的容器是延迟加载
    - 使用ApplicationContext创建的容器是立即加载
    - 具体BeanFactory如何创建只需要了解即可。

# 注解开发
