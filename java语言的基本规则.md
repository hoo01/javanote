# java语言的基本规则

   1.Java 是一种面向对象的语言，所有的代码必须写在某个类里，不能直接写在文件外部（不像 Python、C 允许全局函数）。

   2.Java 不能有单独存在的方法，必须放在类里

   3.所有的可执行代码（如 `System.out.println(...)`）必须放在方法里

   - 不能直接把可执行代码放在类的成员变量声明区域。

   - 只能写**变量声明**，不能写**代码逻辑**。

     ❌错误示例（会报错）：

     ```java
     public class TestCircle {
         Circle c1 = new Circle(1.0);
         System.out.println(c1.area());  // ❌ 错误：可执行语句不能直接放在类里
     }
     ```

     ✅正确示例（放入方法）：

     ```java
     public class TestCircle {
         public static void main(String[] args) {  // 代码必须写在方法里
             Circle c1 = new Circle(1.0);
             System.out.println(c1.area());  // ✅ 正确
         }
     }
     ```

   4.**静态 (`static`) 方法只能调用静态方法或变量**

   - `main` 方法是 `static` 的，不能直接调用**非静态成员**。

   - **如果想调用非静态方法或变量，必须创建对象**：

     ```java
     public class TestCircle {
         double r = 1.0;
     
         public double area() {
             return 3.14 * r * r;
         }
         
         public static void main(String[] args) {
             TestCircle c = new TestCircle();  // 必须创建对象
             System.out.println(c.area());  // ✅ 这样才能调用非静态方法
         }
     }
     ```

   - 如果 `area()` 方法与实例无关，可以改成 `static` 方法，直接调用：

     ```java
     public class TestCircle {
         static double area(double r) {  // 变成 static
             return 3.14 * r * r;
         }
     
         public static void main(String[] args) {
             System.out.println(area(1.0));  // ✅ 直接调用，无需创建对象
         }
     
     }
     ```

再比较：

错误做法1

```java
public class TestCircle {
    double r = 1.0;  // 实例变量 r

    static double area() {  // ❌ 错误：静态方法不能访问实例变量
        return 3.14 * r * r;  // ❌ 这里会报错
    }
    
    public static void main(String[] args) {
        System.out.println(area(1.0));  // ❌ area() 方法没有参数，但是 area(1.0) 传了 1.0，方法签名不匹配，编译会报错。
    }
}
```

正确做法1

```java
public class TestCircle {
    static double r = 1.0;  // ✅ 变成静态变量，所有对象共享

    static double area() {  // ✅ 现在可以访问 r
        return 3.14 * r * r;
    }
    
    public static void main(String[] args) {
        System.out.println(area());  // ✅ 直接调用
    }
}
```

正确做法2

```java
double r = 1.0;

double area() { 
    return 3.14 * r * r;
}

public static void main(String[] args) {
    TestCircle c = new TestCircle();
    System.out.println(c.area());
}
```

正确做法3

```java
static double area(double r) { 
    return 3.14 * r * r;
}

public static void main(String[] args) {
    System.out.println(area(1.0));  // 现在可以传入参数
}
```



