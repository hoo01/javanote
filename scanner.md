### **1. Scanner 是什么？**

`Scanner` 是 **Java SE 5** 引入的一个 **轻量级**、**方便** 的输入工具，提供了简洁的方法来读取和解析输入。

它的 **主要作用**：

- **从输入流中获取数据**（如键盘、文件、字符串等）
- **自动解析为不同的数据类型**（`int`、`double`、`String` 等）
- **支持分隔符（Delimiter）**（默认是空格、换行等）

------

### **2. Scanner 的基本用法**

```java
import java.util.Scanner;  // 导入 Scanner 类

public class ScannerExample {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);  // 创建 Scanner 对象，读取用户输入
        
        System.out.print("请输入你的名字: ");
        String name = scanner.nextLine();  // 读取整行字符串
        
        System.out.print("请输入你的年龄: ");
        int age = scanner.nextInt();  // 读取整数
        
        System.out.print("请输入你的身高(m): ");
        double height = scanner.nextDouble();  // 读取浮点数
        
        System.out.println("你好, " + name + "！你的年龄是 " + age + " 岁，身高是 " + height + "m。");

        scanner.close();  // 关闭 Scanner（推荐）
    }
}

```

**示例输入**

```
makefile复制编辑请输入你的名字: Alice
请输入你的年龄: 25
请输入你的身高(m): 1.65
```

**示例输出**

```
复制编辑
你好, Alice！你的年龄是 25 岁，身高是 1.65m。
```

------

### **3. Scanner 提供的常用方法**

| 方法              | 说明                                             | 例子                         |
| ----------------- | ------------------------------------------------ | ---------------------------- |
| `next()`          | 读取**一个单词**（遇到**空格**、**换行**就停止） | `scanner.next();`            |
| `nextLine()`      | 读取**一整行**（包括空格，直到回车结束）         | `scanner.nextLine();`        |
| `nextInt()`       | 读取一个 `int` 类型的整数                        | `scanner.nextInt();`         |
| `nextDouble()`    | 读取一个 `double` 类型的小数                     | `scanner.nextDouble();`      |
| `hasNextInt()`    | **检查**下一个输入是否是 `int` 类型              | `scanner.hasNextInt();`      |
| `hasNextDouble()` | **检查**下一个输入是否是 `double` 类型           | `scanner.hasNextDouble();`   |
| `useDelimiter()`  | 改变默认分隔符（默认是空格、换行）               | `scanner.useDelimiter(",");` |

------

### **4. Scanner 的数据来源**

#### **（1）从键盘输入**

```
Scanner scanner = new Scanner(System.in);
```

💡 **读取用户输入（最常见）**。

#### **（2）从字符串读取**

```java
String data = "Hello 100 3.14";
Scanner scanner = new Scanner(data);

String word = scanner.next();  // 读取 "Hello"
int num = scanner.nextInt();   // 读取 100
double pi = scanner.nextDouble(); // 读取 3.14
```

💡 **适用于解析字符串中的数据**。

#### **（3）从文件读取**

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class ReadFileExample {
    public static void main(String[] args) throws FileNotFoundException {
        File file = new File("data.txt");
        Scanner scanner = new Scanner(file);
        
        while (scanner.hasNextLine()) {
            System.out.println(scanner.nextLine()); // 逐行读取文件内容
        }
        scanner.close();
    }
}
```

💡 **适用于读取 `.txt` 文件中的数据**。

------

### **5. Scanner 使用注意事项**

1. **避免输入错误导致异常**

   - 如果用户输入了 **非数字**，但调用 `nextInt()` / `nextDouble()`，会报错：

   ```
   Exception in thread "main" java.util.InputMismatchException
   ```

   - 解决方法：使用 `hasNextInt()` / `hasNextDouble()` 先检查：

   ```java
   if (scanner.hasNextInt()) {
       int number = scanner.nextInt();
   } else {
       System.out.println("输入不是整数");
   }
   ```

2. **避免 `nextLine()` 和 `nextInt()` 连用问题**

   ```
   System.out.print("请输入年龄: ");
   int age = scanner.nextInt();  // 读取整数
   System.out.print("请输入姓名: ");
   String name = scanner.nextLine();  // 直接跳过，不会等待用户输入！
   ```

   **原因**：`nextInt()` 只读取 **数值部分**，但回车符 `\n` 仍留在缓冲区，导致 `nextLine()` 直接读取这个 **回车符**，不会等待用户输入。

   **解决方法**：

   ```java
   scanner.nextLine();  // 吸收回车符
   ```

3. **建议关闭 Scanner**

   ```java
   scanner.close();
   ```

   **原因**：`Scanner` 依赖底层 `InputStream`，如果长时间不关闭，可能导致资源泄漏（但 `System.in` 一般无需手动关闭）。

------

### **6. 总结**

- **Scanner 是 Java 标准库中的输入处理类**，位于 `java.util` 包中。
- **用于读取数据**（支持 `int`、`double`、`String` 等）。
- **可以从多种来源读取数据**（键盘、字符串、文件）。
- **支持检查输入类型**（`hasNextInt()` 等）。
- **使用时注意 `nextInt()` / `nextLine()` 的换行问题**。