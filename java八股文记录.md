## 1，java基础

#### 1，泛型



#### 2，反射

直接看



#### 3，创建对象的方式

直接new   反射机制 clone  反序列化  方法句柄和unsafe分配内存(不安全 也不是面向对象 不推荐)





new / 反射 / MethodHandle → 正常走构造方法
clone / 反序列化 / Unsafe → 绕过构造方法，直接造对象

```Java

/**
 * 演示：Java 创建对象的 6 种方式
 */
public class CreateObjectDemo {

    // ===== 示例类 =====
    static class Person implements Cloneable, Serializable {
        public Person() {
            System.out.println("constructor called");
        }

        @Override
        protected Object clone() throws CloneNotSupportedException {
            return super.clone();
        }
    }

    public static void main(String[] args) throws Exception {

        // 1️⃣ 直接 new（最常见）
        // 会调用构造方法
        Person p1 = new Person();

        // 2️⃣ 反射创建对象
        // 会调用构造方法
        Person p2 = Person.class
                .getDeclaredConstructor()
                .newInstance();

        // 3️⃣ clone()
        // 不会调用构造方法（直接拷贝内存）
        Person p3 = (Person) p1.clone();

        // 4️⃣ 反序列化
        // 不会调用构造方法（对象“复活”）
        ObjectOutputStream oos =
                new ObjectOutputStream(new FileOutputStream("person.obj"));
        oos.writeObject(p1);
        oos.close();

        ObjectInputStream ois =
                new ObjectInputStream(new FileInputStream("person.obj"));
        Person p4 = (Person) ois.readObject();
        ois.close();

        // 5️⃣ 方法句柄（MethodHandle）
        // 本质是更底层的反射，会调用构造方法
        MethodHandles.Lookup lookup = MethodHandles.lookup();
        MethodHandle mh = lookup.findConstructor(
                Person.class,
                MethodType.methodType(void.class)
        );
        Person p5 = (Person) mh.invoke();

        // 6️⃣ Unsafe 分配内存
        // 不会调用构造方法，直接在内存中生成对象（非常危险）
        Field f = Unsafe.class.getDeclaredField("theUnsafe");
        f.setAccessible(true);
        Unsafe unsafe = (Unsafe) f.get(null);
        Person p6 = (Person) unsafe.allocateInstance(Person.class);
    }
}

```

#### 4，动态代理--概念，接口，区别  

动态代理 =你以为你在调用 A， 实际上你调用的是 B，但 B 会偷偷帮你把事情转给 A。

假设你**不改业务代码**，
 但你想在 **每次方法调用时** 自动做这三件事：

1. 打日志
2. 统计耗时
3. 做权限校验

你怎么办？

**最蠢但直观的方式（编写新的嵌套类--这就是代理  OrderServiceProxy是代理类）**

```
class OrderServiceProxy {
    private OrderService target;

    public void createOrder() {
        log();
        target.createOrder();
        recordTime();
    }
}

```

问题是：

- 每个方法都要写
- 所有类都要写
- 改一次要全改



所以动态代理：

> 有 100 个方法
>  这些方法在调用前后都要做同一件事（比如打日志）
>  就可以用动态代理
>  只写一个类
>  所有方法都能套用
>  如果需要方法信息，可以用反射拿到不同方法的信息

```
调用任意方法
      ↓
JVM 统一拦截
      ↓
invoke(method, args)
      ↓
反射拿方法信息
      ↓
执行通用逻辑
      ↓
反射调用真实方法

```

**官方版：**

动态代理适用于大量方法需要统一增强的场景，
 通过在运行时生成代理类，将方法调用统一拦截到一个入口，
 再结合反射获取方法信息，实现日志、事务等横切逻辑的复用，
 从而避免为每个方法手写代理代码。



| 对比点       | JDK 动态代理 | CGLIB  |
| ------------ | ------------ | ------ |
| 是否需要接口 | 必须         | 不需要 |
| 实现方式     | 实现接口     | 继承类 |
| final 类     | 不影响       | ❌      |
| final 方法   | 不影响       | ❌      |
| Spring 默认  | ✔ 优先       | 兜底   |
| 你该不该纠结 | 不用         | 不用   |



有接口（JDK 动态代理能用） 因为jdk代理就是用来实现接口的 没接口无法实现

```Java
// 接口（只规定“你能干啥”）
public interface OrderService {
    void createOrder();
}

// 类（真正干活的）
public class OrderServiceImpl implements OrderService {
    public void createOrder() {
        System.out.println("创建订单");
    }
}
```

这里就是**“有接口”**，因为：

```Java
class OrderServiceImpl implements OrderService
```



#### 5， 序列化

> **序列化 = 把对象变成“可保存 / 可传输”的数据**
>  **反序列化 = 把这些数据再变回对象**

在 Java 里：

- `Serializable` —— **最常用、最省事**
- `Externalizable` —— **更底层、你自己全权负责**



| 对比点     | Serializable | Externalizable |
| ---------- | ------------ | -------------- |
| 是否自动   | 是           | 否             |
| 谁控制数据 | JVM          | 你             |
| 字段顺序   | JVM 决定     | 你决定         |
| 可维护性   | 高           | 低             |
| 性能       | 一般         | 可更优         |
| 风险       | 小           | 大             |

#### 6，异常

和 Java 异常处理**直接相关的核心关键字只有这些**：

- `try` —— **包住可能出错的代码**
- `catch` —— **接住异常**
- `finally` —— **不管出不出错都要执行**
- `throw` —— **手动抛异常**
- `throws` —— **声明：我不处理，往上抛**
- `Exception / RuntimeException` —— **异常类型本身**

```Java
// 自定义异常（可选，但面试很常见）
class MyException extends Exception {
    MyException(String msg) {
        super(msg);
    }
}

class Demo {

    // throws：声明本方法可能抛异常，但我不处理
    static void riskyMethod(int x) throws MyException {

        if (x < 0) {
            // throw：主动制造并抛出异常
            throw new MyException("x 不能为负数");
        }

        if (x == 0) {
            // 抛运行时异常（不用 throws 也行）
            throw new RuntimeException("x 不能为 0");
        }

        System.out.println("正常执行");
    }

    public static void main(String[] args) {

        try {
            // try：包住“可能出问题”的代码
            riskyMethod(-1);

            System.out.println("这一行可能执行不到");

        } catch (MyException e) {
            // catch：捕获并处理“指定类型”的异常
            System.out.println("捕获到自定义异常：" + e.getMessage());

        } catch (Exception e) {
            // catch：兜底异常（父类）
            System.out.println("捕获到其他异常");

        } finally {
            // finally：无论是否发生异常，都会执行
            System.out.println("释放资源 / 收尾工作");
        }

        System.out.println("程序继续往下跑");
    }
}

```



执行顺序：

直接看对应八股 反正要看看

没有return就是try catch(可能) finally

有return也是 但是理解一下结果暂存的意思



**如果finally块中有return语句，则其返回值将是整个try-catch-finally结构的返回值。**

**如果finally块中没有return语句，则try或catch块中的return语句(取决于哪个执行了)将确定最终的返回值**







#### 7， static final  自己总结

`static` —— **“属于类，不属于对象”**

> **static 成员只和类有关，所有对象共享一份**



有

1，static变量 （类加载时创建   全类 **只有一份**      不依赖对象存在）

2，static方法（不能直接访问非 static 成员）

3，static代码块

4，static内部类





`final` —— **“不可改变”**

> **final = 一旦确定，就不能再改**



1，final常亮 ---**只能赋值一次  引用不可变 只可以修改对象内容**

```Java
final List<String> list = new ArrayList<>();
list.add("A"); // ✅
list = new ArrayList<>(); // ❌

```







static final----类级常量

> **全局唯一 + 不可修改**

```Java
class Constants {
    public static final int MAX_SIZE = 100;
}
```

- 类加载时初始化
- 全程序唯一
- 不能被修改

```Java
int size = Constants.MAX_SIZE;
```





#### 8. string  intern

```java 
String s1 = new String("a"); // ①
s1.intern();                 // ②
String s2 = "a";             // ③
System.out.println(s1 == s2);// ④ false
```

① `new String("a")`

发生两件事：

1. 字面量 `"a"` 会被放进/引用到 **字符串池**（如果之前没有）
2. `new String(...)` 会在 **堆** 上再创建一个新对象，`s1` 指向它

所以：

- `s1`：堆对象
- 字符串池：也有一个 `"a"`

② `s1.intern()`

- 因为池里已经有 `"a"` 了，所以 `intern()` **返回池里的引用**
- 但你没接收返回值，所以 `s1` 还是指向堆对象

③ `String s2 = "a"`

- 直接拿 **字符串池** 里的 `"a"` 引用

④ 为啥 `s1 == s2` 是 false？

- `s1`：堆上的 `"a"`对象
- `s2`：字符串池里的 `"a"`对象
- 不是同一个对象引用 → `false`



| 声明方式              | 能改引用吗 | 能改内容吗 |
| --------------------- | ---------- | ---------- |
| `String`              | ✅          | ❌          |
| `final String`        | ❌          | ❌          |
| `static String`       | ✅          | ❌          |
| `static final String` | ❌          | ❌          |



## 2，集合类



## 3，java并发





## 4，JVM





## 5，Spring





## 6，Springcloud