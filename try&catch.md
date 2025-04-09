# try catch throw
```java
public class Example2 {
    public static void main(String[] args) {
        try {
            checkAge(15); 
        } catch (IllegalArgumentException e) {
            System.out.println("异常提示: " + e.getMessage());
        }
    }

    static void checkAge(int age) {
        if (age < 18) {
            throw new IllegalArgumentException("未成年人禁止进入！");
        }
        System.out.println("欢迎进入！");
    }
}
```
在 Java 里，所有异常类（比如 `IllegalArgumentException`）都是继承自 `Throwable` 或它的子类（比如 `Exception`）。
而在 `Throwable` 这个基类里，定义了 `getMessage()` 方法：
```java
public String getMessage() {
    return detailMessage;
}
```
这里的 `detailMessage` 就是你**在 new 异常对象的时候传进去的字符串**。
比如你写：
```java
throw new IllegalArgumentException("未成年人禁止进入！");
```
- `"未成年人禁止进入！"` 这个字符串就被保存到了异常对象的 `detailMessage` 字段里。
- 后面你调用 `e.getMessage()`，就会把这个字符串返回出来！
如果你 **new异常对象时** **不传字符串**，那么 `getMessage()` 返回的是 `null`。

所以程序的执行过程是：
- `main()` 方法执行，进入 `try` 块。
    
- 调用 `checkAge(15)`，传进去的 `age = 15`。
    
- 在 `checkAge` 方法里：
    
    - `if (age < 18)` 条件成立（因为 15 < 18）
        
    - 所以会执行`throw new IllegalArgumentException("未成年人禁止进入！");
     也就是说，手动抛出了一个异常。
- 因为 `checkAge(15)` 里抛了异常，所以立刻跳出 `try`，进入 `catch` 块。
    
- `catch` 捕获到了 `IllegalArgumentException`，然后执行：
    `System.out.println("异常提示: " + e.getMessage());
`
# 异常的继承
## 继承关系图

![微信截图_20250226194932.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250226194932.png)

虽然Throwable是所有异常的祖宗，但是一般不会选Throwable，选的是Exception

| 区别                  | 受检异常 (IOException等)        | 非受检异常 (RuntimeException等)                       |
| ------------------- | -------------------------- | ----------------------------------------------- |
| 编译时检查？              | 要                          | 不要                                              |
| 必须try-catch或throws？ | 是                          | 否                                               |
| 主要代表                | IOException, SQLException等 | NullPointerException, IllegalArgumentException等 |
| 目的                  | 强制处理，提升程序健壮性               | 放手给程序员自己保证逻辑正确                                  |
| 特点                  |                            | 可以不处理，出错时程序崩溃                                   |

```java
public class TestThrows {
    public TestThrows() {
    }

    public static void main(String[] args) throws Exception {
        test3(0.0);
    }

    public static void test1(double p) {
        if (p <= 0.0) {
            throw new IllegalArgumentException("本金必须大于 0");
        } else {
            System.out.println(2);
        }
    }

    /**
     * 选择1：自己处理异常 try - catch 捉住异常
     * @param p
     */
    public static void test2(double p) {
        if (p <= 0.0) {
            try {
                throw new Exception("本金必须大于 0");
            } catch (Exception var3) {
            }
        }

        System.out.println(2);
    }

    /**
     * 选择2：由上一层方法来处理，用throws可能出现的异常类型
     */
    public static void test3(double p) throws Exception {
        if (p <= 0.0) {
            throw new Exception("本金必须大于 0");
        } else {
            System.out.println(2);
        }
    }
}
```
