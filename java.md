# 多态
&ensp;&ensp;&ensp;&ensp;多态是同一个行为具有多个不同表现形式或形态的能力。多态就是同一个接口，使用不同的实例而执行不同操作。  
&ensp;&ensp;&ensp;&ensp;多态的条件：继承，重写，父类引用指向子类对象。
```
Person P = new Child();
```

# 为什么静态方法不能调用非静态方法

&ensp;&ensp;&ensp;&ensp;静态方法是属于类的，动态方法属于实例对象，在类加载的时候就会分配内存，可以 通过类名直接去访问，非静态成员（变量和方法）属于类的对象，所以只有该对象初始化之后才存在，然后通过类的对象去访问。

也就是说如果我们在静态方法中调用非静态成员变量会超前，可能会调用了一个还未初始化的变量。因此编译器会报错。

这是一张类加载的生命周期图

![](./picture/7849b099d46f457cb37f2e80452f24ee.jfif)


```
1、加载
”加载“是”类加机制”的第一个过程，在加载阶段，虚拟机主要完成三件事：
（1）通过一个类的全限定名来获取其定义的二进制字节流
（2）将这个字节流所代表的的静态存储结构转化为方法区的运行时数据结构
（3）在堆中生成一个代表这个类的Class对象，作为方法区中这些数据的访问入口。
注意此时会扫描到我们的代码中是否有静态变量或者是静态方法等等这些静态数据结构，还未分配内存。
2、验证
验证的主要作用就是确保被加载的类的正确性。
3、准备
准备阶段主要为类变量分配内存并设置初始值。这些内存都在方法区分配。注意此时就会为我们的类变量也就是静态变量分配内存，但是普通成员变量还没。
4、解析
解析阶段主要是虚拟机将常量池中的符号引用转化为直接引用的过程。
5、初始化
这是类加载机制的最后一步，在这个阶段，java程序代码才开始真正执行。我们知道，在准备阶段已经为类变量赋过一次值。在初始化阶端，程序员可以根据自己的需求来赋值了。初始化时候才会为我们的普通成员变量赋值。
```

# 为什么需要基本数据类型的包装类
```
1、java是面向对象的语言，很多地方需要使用的是对象而不是基本数据类型。例如
List, Map等容器类中基本数据类型是放不进去的。
2、包装类在原先的基本数据类型上，新增加了很多方法。例如Integer.valueOf(String s)。
```

# 为什么需要基本数据类型
```
1、基本数据类型基于数值，对象类型基于引用。基本数据类型存储在栈的局部变量表中。对象类型的变量存储在堆中应用，实例放在堆中，因此对象类型的变量需要占用更多的内存空间。
```

# JSR-330
Java Specification Requests Java规范提案
# set
元素不可重复。
# list
元素有放入顺序，元素可重复。  
ArrayList：基于动态数组实现，支持随机访问。  
Vector：和 ArrayList 类似，但它是线程安全的。   
LinkedList：基于双向链表实现，只能顺序访问，但是可以快速地在链表中间插入和删除元素。不仅如此，LinkedList 还可以用作栈、队列和双向队列。 

list 元素去重
# HashMap
HashMap 底层是hash数组和单向链表实现，数组中的每个元素都是链表(Node<K,V>[] table) 。当链表长度超过8时，链表转为红黑树。  
Hashmap遍历
```
for (Map.Entry entry: map.entrySet()) {
    System.out.println("key:" + entry.getKey() +", value:" +  entry.getValue());
}

for (String key: map.keySet()) {
    System.out.println("key:" + key + ", value:" + map.get(key));
}

Iterator<Map.Entry<String, String >> entries = map.entrySet().iterator();
while (entries.hasNext()) {
    Map.Entry<String, String> entry = entries.next();
    System.out.println("key:" + entry.getKey() +", value:" +  entry.getValue());
}
```
# String StringBuffer StringBuild
都是操作字符串
StringBuffer是线程安全的
# io
bio 同步阻塞  
nio 同步非阻塞  
aio 异步非阻塞  


# 线程池
ExecutorService executorService = Executors.newSingleThreadExecutor();
ThreadPoolExecutor类有4个构造方法对应4中线程池（ExecutorService）
```
public static ExecutorService newSingleThreadExecutor() {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
                                    new LinkedBlockingQueue<Runnable>()));
    }
```

减少线程创建和销毁在时间个资源上的消耗。

