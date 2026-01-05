<<<<<<< HEAD
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







## 2，java并发

#### 1，threadlocal

简单来说就是每个线程都可以创建一个同名的

```java
ThreadLocal<Integer> a = new ThreadLocal<>();
```

但是每一个线程内的这个变量不一样 





底层：通过**每个线程都有一个单独的ThreadLocalMap**

里面是哈希表的形式   `key是threadlocal的引用  value就是对应的值`

```java
Thread A
 └── ThreadLocalMap A
       ├── key: tl1 → value: 10
       └── key: tl2 → value: "A"

Thread B
 └── ThreadLocalMap B
       ├── key: tl1 → value: 20
       └── key: tl2 → value: "B"

```



应用场景：解释理解



在 Spring Boot 这种同步 Web 模型里，可以等价成一句更狠的：

> **一次 HTTP 请求 ≈ 一个线程的生命周期**

##### 举例：两个人同时访问页面的分页参数

两个人访问同一个接口：

```
GET /videos?page=1&pageSize=10   （用户 A）
GET /videos?page=3&pageSize=20   （用户 B）
```

实际运行时是这样：

```
线程 T1（用户 A）：
  ThreadLocal.page = (1, 10)

线程 T2（用户 B）：
  ThreadLocal.page = (3, 20)
```

- 两个请求**走的是同一段 Java 代码**
- 调的是**同一个 mapper 方法**
- 但因为线程不同
   👉 ThreadLocalMap 不同
   👉 分页参数天然隔离



#### 2，JMM内存模型

JMM（Java Memory Model）定义了多线程环境下，

Java 程序中变量在主内存和线程工作内存之间的**可见性、有序性和原子性**规则，

 **它是一套“规范”，不是具体实现。**



接着你可以说：

> **在 JMM 中，所有共享变量都存放在主内存中，
>  每个线程都有自己的工作内存，
>  线程对变量的读写，实际上是先从主内存拷贝到工作内存，再在本地操作，
>  最后再刷新回主内存。**

然后补一句**很加分的澄清**：

> 这里的“主内存 / 工作内存”是逻辑抽象，
>  不等同于物理上的 RAM、CPU cache。





#### 3，synchronized

你可以把 `synchronized` 想成**一个只有一把钥匙的会议室**。
 每个 Java 对象天生就有这么一间会议室（monitor）。线程想执行 `synchronized` 里的代码，就得先拿钥匙。第一个拿到钥匙的线程进去开会，其他线程只能在门口等（EntryList），而且排队不保证先来先到——有时候刚来的人运气好直接抢到钥匙。
 如果正在开会的线程觉得“我现在先休息一下”，调用了 `wait()`，它会把钥匙放回去，自己去休息室等通知（WaitSet）。等有人 `notify()` 或 `notifyAll()`，它才会被叫醒，回到门口重新抢钥匙。整个过程中，**钥匙永远只有一把，谁拿到谁执行**，这就是 synchronized 锁的全部运行逻辑。







面试版本：



`synchronized` 是基于对象监视器（monitor）实现的，每个 Java 对象都关联一个 monitor。
 线程进入 synchronized 时需要获取 monitor 的所有权，获取不到的线程会进入 EntryList 等待；线程在同步块中调用 `wait()` 会释放锁并进入 WaitSet，`notify` 或 `notifyAll` 会将等待线程唤醒到 EntryList 重新竞争锁。

整个锁获取过程是非公平的，不保证 FIFO 顺序，目的是提高系统吞吐量。



wait()的意义是等条件，满足了才能进入entrylist抢锁





然后关于synchronized锁的是什么  直接去看八股吧

好理解但是不好写进笔记：

1，synchronized的`普通方法`，其实锁的是具体调用这个方法的实例对象， 所以如果打印时间戳都是一样 因为不阻塞

而synchronized的`静态方法`，其实锁的是这个方法锁属于的类对象。 所以如果十个一起打印时间戳是不一样的



2，synchronized(this)锁的就是this对象 也就是实例对象

synchronized(Xxx.class)就是类对象 所以阻塞  跟上面一样理解



#### 4，锁升级

你只需要记住这一条线：

```
无锁
 → 偏向锁
   → 轻量级锁
     → 重量级锁（monitor）
```





一个 Java 对象在刚创建出来时，就已经**自带对象头，其中包含 Mark Word**，用来记录这个对象的状态。

1，一开始对象处于无锁状态，Mark Word 里只存放对象的哈希信息和 GC 相关数据。

2，当第一个线程进入 synchronized 时，如果没有任何竞争，JVM 会启用`偏向锁机制`。

此时 JVM 会在对象的 Mark Word 中记录“偏向于某个线程”，也就是记住第一次使用这个锁的线程是谁。

偏向锁的本质是 JVM 基于经验判断：这个对象大概率长期只会被同一个线程使用。

3，当第二个线程尝试进入 synchronized 时，JVM 发现锁不再是单线程私有的，于是会撤销偏向锁。撤销后，锁会升级为`轻量级锁。轻量级锁通过 CAS 和自旋的方式让多个线程竞争锁`，线程不会立即阻塞，而是在用户态短暂自旋等待，适用于竞争不激烈、临界区很短的场景。

4，如果竞争进一步加剧，比如自旋多次仍然无法获取锁，或者线程在同步块中调用了 wait 方法，`JVM 会将锁升级为重量级锁`。

**此时对象的 Mark Word 会指向一个 monitor 结构**，线程会进入阻塞状态，通过操作系统进行调度。

重量级锁依赖 EntryList 和 WaitSet 来管理线程，开销最大，但能保证在高并发场景下的正确性和稳定性。

整个过程中，锁只会从低级形态向高级形态升级，不会再降级，这是 JVM 在性能与稳定性之间做出的权衡。







#### 5，volatile只满足有序性和可见性

不满足原子性

```java

public class TestValitile {
    AtomicInteger number1 = new AtomicInteger(0);
    int number = 0;

    public void increase1() {
        number1.incrementAndGet();
    }

    public void increase() {
        number++;
    }

    public static void main(String[] args) throws InterruptedException {
        TestValitile demo = new TestValitile();

        Thread[] threads = new Thread[10];

        for (int j = 0; j < 10; j++) {
            threads[j] = new Thread(() -> {
                for (int i = 0; i < 1000; i++) {
                    demo.increase();
                }
            });
            threads[j].start();
        }

        // 等待所有线程执行完
        for (Thread t : threads) {
            t.join();
        }

        System.out.println(
                Thread.currentThread().getName() +
                        " final number result = " + demo.number
        );
    }
}

```

结果并非10000  



#### 6，AQS

AQS（AbstractQueuedSynchronizer）是 JDK 并发包中用于构建锁和同步器的基础框架，

它的核心思想是`用一个整型的 state 状态来表示资源占用情况，并配合一个 FIFO 的等待队列来管理线程的获取与释放`。



线程在获取资源时，会通过 CAS 操作尝试修改 state，成功则直接执行；失败则被封装成节点加入等待队列，并在合适时机被阻塞和唤醒。



AQS 本身并不关心具体的同步语义，只负责线程排队和调度，具体的加锁和释放规则由子类通过重写 acquire 和 release 相关方法来定义。



JDK 中的 **ReentrantLock、Semaphore、CountDownLatch、ReentrantReadWriteLock 等并发工具**，底层都是基于 AQS 实现的，它们通过不同的 state 语义和获取策略，复用了同一套线程排队和同步机制。

------

如果你想再**保险一点**，可以在最后补一句收口用的：

> 相比 synchronized 的 JVM 内建 monitor，AQS 是 Java 层的可扩展同步框架，更适合实现公平锁、可中断锁和多条件队列等复杂并发控制。









AQS有两种队列，**条件队列和同步队列**

```
同步队列：没抢到锁，在这排队
条件队列：抢到锁但条件不对，在这等
```

##### 面试回答：

> AQS 中的同步队列用于管理线程对锁的获取与释放，线程获取锁失败后`会自动进入同步队列等待`；
>
> 而条件队列是通过 Condition 实现的，`线程在持有锁的情况下调用 await 会释放锁并进入条件队列等待`，signal 或 signalAll 会将条件队列中的线程转移回同步队列重新竞争锁。







#### 7，CAS  比较替换   aba问题  

1️⃣ CAS（Compare And Swap）

- CAS 是一种 **无锁的原子操作**
- 核心逻辑：
   **只有当内存值 == 期望值时，才更新为新值**
- 原子性由 **CPU 指令** 保证，不依赖 synchronized
- 常见应用：
   `AtomicInteger`、`AtomicLong`、AQS 内部状态控制

👉 **CAS 解决的是“并发更新的原子性问题”**

------

2️⃣ ABA 问题

- CAS 只比较“当前值”，**不关心中间是否被修改**
- 典型问题：
   值从 A → B → A，CAS 仍然认为“没变”
- ABA 是 **语义问题，不是原子性问题**
- 在计数场景通常无影响，在引用/指针结构中可能出错

**解决方式：**

- `给值加版本号`
- 常用类：`AtomicStampedReference`

------

3️⃣ 自旋锁（Spin Lock）

- 自旋锁是一种 **等待策略**
- 核心行为：
   **获取锁失败后，不阻塞线程，而是循环尝试**
- 适合：
  - 临界区短
  - 低竞争场景
- 缺点：
  - 消耗 CPU

------

4️⃣ CAS 和自旋锁的关系（重点）

- **CAS 是原子操作手段**
- **自旋是失败后的重试方式**
- 关系总结：

> **自旋锁 = CAS + 循环重试**

- CAS ≠ 自旋锁
- 但自旋锁几乎一定基于 CAS 实现

------

5️⃣ 三者一句话定型

- **CAS**：原子比较并交换
- **ABA**：CAS 的语义缺陷
- **自旋锁**：基于 CAS 的非阻塞等待策略

------

6️⃣ 面试一句话版（超稳）

> CAS 是一种基于 CPU 原子指令的无锁同步机制，存在 ABA 问题，可通过版本号解决；自旋锁通常基于 CAS 实现，通过循环 CAS 的方式避免线程阻塞，但会消耗 CPU。







另外 CAS一般也都会采用自旋的方式。当CAS失败的时候，会尝试短暂的自旋重复执行



**当然 要衡量占用的CPU资源**





#### 8， unsafe

你可以这样理解：

> **Unsafe 是 JVM 暴露给 Java 层的“后门接口”，
>  用于做普通 Java 语法做不到的事情。**

它能干的事包括：

- 直接操作内存
- 绕过构造器创建对象
- 修改对象字段偏移量
- 执行 CAS（compareAndSwap）

👉 名字叫 Unsafe，不是吓唬你，是真的“容易把 JVM 玩炸”。







##### 面试回答：

> CAS 是由 CPU 提供的原子指令支持的，Java 需要**通过 Unsafe 或 VarHandle 与底层交互来实现 CAS**。业务代码不需要也不应该直接使用 Unsafe，而是通过 Atomic 类或并发工具间接使用 CAS。





#### 9，reentranlock

ReentrantLock 是一个基于 AQS 实现的、可重入的显式锁，提供了比 synchronized 更灵活的加锁能力。



关键词你先记住三个： **AQS / 显式 / 可重入**





和 synchronized 的区别在于：

- synchronized
   👉 JVM 自动加锁、自动释放
- ReentrantLock
   👉 你手动 `lock()`、手动 `unlock()`





 可重入性一句话解释

> **同一个线程，在已经持有锁的情况下，可以再次获取同一把锁，而不会被阻塞。**

想象一种`不存在可重入性`的锁：

- 线程 A 获取了锁
- 在线程 A 的代码里，又调用了一个方法
- 这个方法也要获取同一把锁
- 结果：**线程 A 把自己锁死了**





ReentrantLock 是如何实现“可重入”的？

> **锁不仅记录“有没有被占”，
>  还记录“是谁占的 + 占了几次”。**



说白了就是有一个state变量去记录呗



##### ReentrantLock 和 synchronized 的“可重入”是一样的吗？

结论先给：

> **是的，它们都是可重入的，只是实现方式不同。**

- synchronized
  - JVM 在 monitor 里维护重入次数
- ReentrantLock
  - AQS 的 state 记录重入次数

语义完全一致，机制不同。



##### 面试回答

ReentrantLock 是 JDK 提供的基于 AQS 实现的显式锁，它通过 state 记录锁的持有状态和重入次数，因此支持可重入性，即同一线程可以多次获取同一把锁而不会发生自我阻塞。与 synchronized 相比，ReentrantLock 提供了更灵活的加锁方式，如可中断获取锁、可尝试获取锁、公平锁策略以及多个条件队列，适用于对并发控制有更高要求的场景。





#### 10，公平锁、非公平锁

ReentrantLock 支持公平和非公平两种模式，区别在于获取锁时是否尊重同步队列中的等待顺序。



1，非公平锁在锁空闲时会直接通过 CAS 尝试获取锁，即使队列中已有等待线程，从而提高吞吐量；

2，公平锁在尝试获取锁前会检查是否存在排队线程，只有在没有前驱节点时才允许获取锁，从而保证获取顺序的公平性。

底层实现基于 AQS，同步队列负责线程的排队与













## 3，集合类



## 4，JVM





## 5，Spring





## 