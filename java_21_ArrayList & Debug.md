# 传统的数组增添元素办法

```java
import java.util.Arrays;

/**
 * 核心库类学习 - ArrayList
 */
public class TestArrayList {
    public static void main(String[] args) {
        //数组不能自动扩容
        String[] arr0 = new String[] { "a", "b", "c", "d", "e"};
        String[] arr1 = new String[6];

//        for (int i = 0; i < arr0.length; i++) {
//            System.out.println(arr0[i]);
//        }
        System.out.println(Arrays.toString(arr0));//将数组的元素拼接成字符串
        System.out.println(Arrays.toString(arr1));

        for (int i = 0; i < arr0.length; i++) {
            arr1[i] = arr0[i];
        }

        arr1[5] = "f";

        System.out.println(Arrays.toString(arr0));//将数组的元素拼接成字符串
        System.out.println(Arrays.toString(arr1));

    }
}
```

# ArrayList增加元素

```java
import java.util.ArrayList;
import java.util.Arrays;

/**
 * 核心库类学习 - ArrayList
 */
public class TestArrayList {
    public static void main(String[] args) {
       ArrayList list = new ArrayList(5); //代表初始容量是5，如果不指定默认的初始容量10
       list.add("a");
       list.add("b");
       list.add("c");
       list.add("d");
       list.add("e");
       //虽然指定的是5，但是会按1.5扩容，例：5 * 1.5向下取整 =>7，如果不够会继续按1.5倍向下取整扩容
        list.add("f");
        System.out.println(list.toString());
    }
```

# DEBUG


![微信截图_20250225191001.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250225191001.png)

点击目标代码行 - 右键 debug模式运行
![微信截图_20250225191447.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250225191447.png)

逐行执行代码



# 遍历

```java
import java.util.ArrayList;
import java.util.Arrays;

/**
 * 核心库类学习 - ArrayList
 */
public class TestArrayList {
    public static void main(String[] args) {
       ArrayList list = new ArrayList(5); //代表初始容量是5，如果不指定默认的初始容量10
       list.add("a");
       list.add("b");
       list.add("c");
       list.add("d");
       list.add("e");
       //虽然指定的是5，但是会按1.5扩容，例：5 * 1.5向下取整 =>7，如果不够会继续按1.5倍向下取整扩容
       list.add("f");
       System.out.println(list.toString());
/**
 * 遍历
 */
       for (Object e : list) { //指定为Object类型是因为ArrayList默认存储Object类型的元素，指定临时变量名为e
            System.out.println(e);
        }
    }
```

# 泛型（Generics）

## 1. **基本概念**

- **泛型类**: 泛型用于类、接口和方法中，可以使得代码更加灵活，避免类型转换错误。泛型仅支持引用类型，不支持基本数据类型（如`int`、`char`等）。并且接下来放入的元素只能是指定的类型。
- **类型参数**: 泛型使用尖括号`<>`来指定类型参数。类型参数一般是一个类的类型，例如`ArrayList<Integer>`。
- **类型安全**: 泛型提供类型检查的机制，在编译期就能够检查出类型错误，避免了运行时的`ClassCastException`。

```java
import java.util.ArrayList;

public class TestGeneric {
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<Integer>(); // <>泛型,只支持引用类型，不支持基本类型，后面的integer可以省略
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);

        int sum = 0;
        for (Integer i : list){
            sum += i;

            System.out.println(sum);
        }
    }
}
```

## 2.**泛型的局限**

- **不支持基本数据类型**: 例如，你不能写`ArrayList<int>`，因为`int`是基本类型，而泛型只支持引用类型（如`Integer`、`String`、`Object`等）。
- **自动装箱**: 在这个例子中，虽然我们往`ArrayList`中添加的是`int`类型的数字，Java会自动将它们转换为`Integer`对象。这是**自动装箱**的机制。

## 3.遍历与泛型

```java
for (Integer i : list){
    sum += i;  // 不需要类型转换，i已经是Integer类型
}
```

使用增强的`for`循环（foreach）时，Java编译器会自动知道`list`中的元素类型是`Integer`，所以你不需要显式地进行类型转换。

泛型的好处之一就是避免了在使用集合时强制转换类型（例如在没有泛型时，你需要做`(Integer)`转换）。

## 4.泛型的通配符