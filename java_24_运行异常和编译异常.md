- 运行时异常(也称未检查异常)

  Error 以及它的子类

  RuntimeException 以及它的子类

- 编译异常(也称检查异常)

  除掉运行时以外的所有异常，都属于编译异常

![微信截图_20250226194932.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250226194932.png)

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