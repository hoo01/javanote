```java
public class TestTry {
    public static void main(String[] args) {
        System.out.println(1);
        try {
            test(0.0); //没有异常一切照旧，有异常配合 catch 语句处理异常
        } catch (IllegalArgumentException e) { //捕捉，用来获取异常对象，让程序恢复正常流程。但是只能捉住类型一致/向上类型的异常。语法：异常类型
            System.out.println(e); //打印异常对象

        }
        System.out.println(3);
    }

    public static void test(double p) {
        if (p <= 0.0) {
            throw new IllegalArgumentException("p must be greater than zero");
        }
    }
}
```

# 异常的继承

![微信截图_20250226165951.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250226165951.png)

虽然Throwable是所有异常的祖宗，但是一般不会选Throwable，选的是Exception