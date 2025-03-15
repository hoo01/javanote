为什么需要Map? 数组查找的低效

```java
import java.util.List;

public class TestMap {

    //for 循环实现，需要一次次查找，效率较低
    public static String find1(String value) {
        String[] array = new String[]{"xiaoming","xiaobai","xiaohei"};
        for (String s:array) { // s 代表数组某个元素
            if (s.equals(value)) { //字符串比较不能写成 s == value
                return s;
            }
        }
        return null; //等整个for循环都执行完
    }

    public static void main(String[] args) {
        System.out.println(find1("xiaoming"));
    }

    //list集合实现，也需要一次次比较，查找的效率也低
    public static String find2(String value) {
        List<String> list = List.of("xiaoming","xiaobai","xiaohei");
        for (String s:list) {
            if (s.equals(value)) {
                return s;
            }
        }
        return null;
    }
}
```

用map实现

```java
import java.util.Map;

/**
 * 核心库类学习 - Map
 * 1.key(键) -> value(值) 键值对 entry，key要唯一
 * 2.两个泛型，分别用来限制key和value
 * 映射关系
 */

public class TestMap {


    //find3("black")
    public static String find3(String key) {
        Map<String,String> map = Map.of (
                "bright","xiaoming",
                "white","xiaobai",
                "black","xiaohei"

        );
        return map.get (key);
    }

    public static void main(String[] args) {
        System.out.println(find3("black"));
    }
 }
```

HashMap

```java
public class TestMap {

    //find3("black")
    public static String find3(String key) {
        Map<String,String> map = new HashMap<>();//HashMap是可变Map,可以新增修改
        map.put("bright", "xiaoming");
        map.put("bright", "superming");//map既能新增也能修改
        map.put("black", "xiaohei");
        map.put("white", "xiaobai");
        map.remove("black");//根据key删除
        return map.get(key);
    }

    public static void main(String[] args) {
        System.out.println(find3("bright"));
    }
```

```java
public class TestMap {

    //find3("black")
    public static String find3(String key) {
        Map<String,String> map = new HashMap<>();//HashMap是可变Map,可以新增修改
        map.put("bright", "xiaoming");
        map.put("bright", "superming");//map既能新增也能修改
        map.put("black", "xiaohei");
        map.put("white", "xiaobai");
        map.remove("black");//根据key删除
        map.entrySet();//获取所有的键值对集合，可以用增强for循环遍历
        /**
         * for(临时变量类型 临时变量：集合) ｛｝
         * for(Map.Entry<String,String> e : map.entrySet()) {
         * }
         */
        for (Map.Entry<String, String> e : map.entrySet()) {
            System.out.println("key: " + e.getKey() + " value: " + e.getValue());
        }
        return map.get(key);
    }
}
```

