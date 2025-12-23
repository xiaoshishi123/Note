### 1，java基础

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



### 2，集合类



### 3，java并发





### 4，JVM





### 5，Spring





### 6，Springcloud