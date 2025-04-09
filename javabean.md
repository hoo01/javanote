# 一、java基础中的JavaBean

在Java基础中，JavaBean指的是遵循某种命名规范的普通Java对象（POJO）。

而在**Spring**框架中，JavaBean经常被称为**Bean**，特指**由Spring容器管理、生命周期受容器控制**的对象。

JavaBean 的核心特征如下：

- 核心的定义类必须是一个公共类（public class），最好实现一个接口Serializable（**可序列化**（`implements Serializable` 让 JavaBean 支持序列化，通常用于存储到文件或网络传输））
- 所有字段必须是私有的，public方法来访问私有字段 get set is
- 必须有一个无参构造方法
- javabean里一般只封装数据，除了get set,没有其他包含复杂逻辑的方法
- 如果javabean对象的字段里只要有get、set方法其中之一，就称该java bean有一个xxx属性（property）

可以建好类和字段后，用generate - getter and setter 生成 java bean格式
![微信截图_20250223163532.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250223163532.png)

```java
public class TestJavaBean {
    public static void main(String[] args) {
        Teacher t = new Teacher("zhangsan",false);
        System.out.println(t.getName());
        System.out.println(t.isMarried());
        t.setMarried(true);//更改婚姻状态
        System.out.println(t.isMarried());
    }
}

/**
 * 以下为第一种 字段私有，public方法来访问私有字段 get set is
 */
public class Teacher implements Serializable {
    private String name; //小写
    private boolean married; //已婚
    private int age;

    //get 方法 用来获取私有字段值
    public String getName() { //get 后面首字母要大写
        return this.name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public boolean isMarried() { //对boolean类型用is
        return this.married;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setMarried(boolean married) {
        this.married = married;
    }
//无参构造方法：符合规范
    public Teacher() {
    }
//有参构造方法 更简便的方法来初始化对象
    public Teacher(String name, boolean married) {
        this.name = name;
    }
}
```

# 二、spring容器中的bean对象

## Spring创建Bean的方式

Spring创建Bean对象一般有两种方式：

- **XML配置方式（较少使用）**
- **注解配置方式（主流使用）**

### 🔹（1）XML方式示例（理解即可）：

假设你的`Teacher`类不使用任何注解：

```java
<!-- applicationContext.xml -->
<bean id="teacherBean" class="com.example.Teacher">
    <property name="name" value="zhangsan"/>
    <property name="married" value="false"/>
    <property name="age" value="30"/>
</bean>
```

- 当容器启动时，会调用`Teacher`类的**无参构造方法**创建对象。
- 然后通过**setter方法**（setName, setMarried, setAge）注入属性值。

这种方式，Spring强制要求有**无参构造器**和**setter方法**。

### 🔹（2）注解方式（主流方式）：

现代Spring更推荐用注解方式创建Bean。

```java
@Component // 加上@Component注解
public class Teacher implements Serializable {
    private String name;
    private boolean married;
    private int age;

    public Teacher() {
        System.out.println("Teacher无参构造被调用");
    }
    
    public Teacher(String name, boolean married) {
        this.name = name;
        this.married = married;
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public boolean isMarried() {
        return married;
    }
    
    public void setMarried(boolean married) {
        this.married = married;
    }
    
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        this.age = age;
    }

}
```

然后配置**组件扫描**：

```java
@Configuration
@ComponentScan("com.example") // 扫描com.example包下所有@Component
public class AppConfig {
}
```

当Spring容器启动时，会：

1. **扫描**`com.example`包下所有类，发现`Teacher`类上标记了`@Component`注解。
2. **调用无参构造方法**创建`Teacher`对象。
3. 将创建好的`Teacher`对象，**放入Spring容器**中管理，默认Bean的id为类名首字母小写的形式（如：`teacher`）。

## 在Spring中使用Teacher Bean的过程：

如果有一个类想要使用`Teacher`对象（比如控制器Controller）

```java
@RestController
public class TeacherController {

    @Autowired // Spring自动注入
    private Teacher teacher;
    
    @GetMapping("/teacher/info")
    public String getTeacherInfo() {
        teacher.setName("李四");
        teacher.setMarried(true);
        teacher.setAge(35);
    
        return teacher.getName() + ", " + teacher.isMarried() + ", " + teacher.getAge();
    }
}
```

此时Spring容器会：

- 根据类型找到对应的`Teacher`对象（已经通过无参构造创建好）。
- 注入到`TeacherController`中。

## Spring创建JavaBean（Bean对象）的核心步骤回顾：

1. **实例化（Instantiation）**：
   - 调用无参构造函数实例化`Teacher`对象。
2. **属性注入（Property Injection）**：
   - 调用setter方法（如`setName()`、`setMarried()`、`setAge()`）注入属性。
3. **Bean后置处理器调用（PostProcessor）**：
   - 可选步骤，用于Bean增强（如AOP代理）。
4. **将Bean放入Spring容器（Bean Registry）**：
   - 存入Spring IOC容器管理，方便后续自动注入和获取。

**以上所有操作都由Spring自动完成**。