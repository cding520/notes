[TOC]

## 一、概述

 JDK1.2 之前，一个对象只有“已被引用”和"未被引用"两种状态，这将无法描述某些特殊情况下的对象，比如，当内存充足时需要保留，而内存紧张时才需要被抛弃的一类对象。

 所以在 JDK.1.2 之后，Java 对引用的概念进行了扩充，将引用分为了：强引用（Strong Reference）、软引用（Soft Reference）、弱引用（Weak Reference）、虚引用（Phantom Reference）4 种，这 4 种引用的强度依次减弱。

## 二、强引用

​	如果一个对象具有强引用，那就类似于**必不可少的**物品，不会被垃圾回收器回收。

​	只要强引用存在，垃圾回收器将永远不会回收被引用的对象，哪怕内存不足时，JVM也会直接抛出`OutOfMemoryError`，不会去回收。如果想中断强引用与对象之间的联系，可以显示的将强引用赋值为null，这样一来，JVM就可以适时的回收对象了。

```java
Object obj = new Object(); //只要obj还指向Object对象，Object对象就不会被回收 obj = null; //手动置null
```

​	比如ArraryList类的clear方法中就是通过将引用赋值为null来实现清理工作的：

```java
public void clear() {
      modCount++;
 
      // Let gc do its work
      for (int i = 0; i < size; i++)
          elementData[i] = null;
 
      size = 0;
}
```

​	在ArrayList类中定义了一个私有的变量elementData数组，在调用方法清空数组时可以看到为每个数组内容赋值为null。不同于elementData=null，强引用仍然存在，避免在后续调用 add()等方法添加元素时进行重新的内存分配。使用如clear()方法中释放内存的方法对数组中存放的引用类型特别适用，这样就可以及时释放内存。

## 三、软引用

 软引用是用来描述一些**非必需但仍有用**的对象。在内存足够的时候，软引用对象不会被回收，**只有在内存不足时，系统则会回收软引用对象**，如果回收了软引用对象之后仍然没有足够的内存，才会抛出内存溢出异常。这种特性常常被用来实现缓存技术，比如网页缓存，图片缓存等。

 在 JDK1.2之后，用`java.lang.ref.SoftReference`类来表示软引用。

```java
import java.lang.ref.SoftReference;
 
public class SoftRef {  
 
    public static void main(String[] args){  
        System.out.println("start");            
        Obj obj = new Obj();            
        SoftReference<Obj> sr = new SoftReference<Obj>(obj);  
        obj = null;  
        System.out.println(sr.get());  
        System.out.println("end");     
    }       
}  
 
class Obj{  
    int[] obj ;  
    public Obj(){  
        obj = new int[1000];  
    }  
}
```

​	在使用软引用和弱引用的时候，我们可以显示地通过System.gc()来通知JVM进行垃圾回收，但是要注意的是，虽然发出了通知，JVM不一定会立刻执行，也就是说这句是无法确保此时JVM一定会进行垃圾回收的。

​	弱引用还可以和一个引用队列（ReferenceQueue）联合使用，如果弱引用所引用的对象被垃圾回收，Java虚拟机就会把这个弱引用加入到与之关联的引用队列中。

```java
Object o = new Object(); //只要o还指向对象就不会被回收
WeakReference<Object> wr = new WeakReference<Object>(o);
```

​	当要获得weak reference引用的object时, 首先需要判断它是否已经被回收，如果wr.get()方法为空, 那么说明weakCar指向的对象已经被回收了。

​	应用场景：如果一个对象是偶尔的使用，并且希望在使用时随时就能获取到，但又不想影响此对象的垃圾收集，那么应该用 Weak Reference 来记住此对象。或者想引用一个对象，但是这个对象有自己的生命周期，你不想介入这个对象的生命周期，这时候就应该用弱引用，这个引用不会在对象的垃圾回收判断中产生任何附加的影响。

四、弱引用

​	弱引用的引用强度比软引用要更弱一些，也是用来描述**非必需对象**的，无论内存是否足够，只要 JVM开始进行垃圾回收，那些被弱引用关联的对象都会被回收。在 JDK1.2 之后，用 `java.lang.ref.WeakReference` 来表示弱引用。

​	弱引用与软引用的区别在于**：**只具有弱引用的对象拥有更短暂的生命周期。在垃圾回收器线程扫描它所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。不过，由于垃圾回收器是一个优先级很低的线程， 因此不一定会很快发现那些只具有弱引用的对象。所以被**软引用关联的对象只有在内存不足时才会被回收，而被弱引用关联的对象在JVM进行垃圾回收时总会被回收。**

```java
import java.lang.ref.WeakReference;
 
public class WeakRef {
    public static void main(String[] args) {
        WeakReference<String> sr = new WeakReference<String>(new String("hello"));
        System.out.println(sr.get());
        System.gc();                //通知JVM的gc进行垃圾回收
        System.out.println(sr.get());
    }
}
```

## 五、虚引用

 虚引用和前面的软引用、弱引用不同，它并**不影响对象的生命周期**。在java中用`java.lang.ref.PhantomReference`类表示。如果一个对象与虚引用关联，则跟没有引用与之关联一样，**在任何时候都可能被垃圾回收器回收。**虚引用主要用来跟踪对象被垃圾回收的活动。

​	**虚引用必须和引用队列关联使用**，当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会把这个虚引用加入到与之 关联的引用队列中。程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。如果程序发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。

```java
import java.lang.ref.PhantomReference;
import java.lang.ref.ReferenceQueue;
 
 
public class PhantomRef {
    public static void main(String[] args) {
        ReferenceQueue<String> queue = new ReferenceQueue<String>();
        PhantomReference<String> pr = new PhantomReference<String>(new String("hello"), queue);
        System.out.println(pr.get());
    }
}
```

## 六、总结

| 引用类型 | 被回收时间    | 用途           | 生存时间      |
| -------- | ------------- | -------------- | ------------- |
| 强引用   | 从来不会      | 对象的一般状态 | JVM停止运行时 |
| 软引用   | 内存不足时    | 对象缓存       | 内存不足时    |
| 弱引用   | jvm垃圾回收时 | 对象缓存       | gc运行后      |
| 虚引用   | 未知          | 未知           | 未知          |

​	在实际程序设计中一般很少使用弱引用与虚引用，使用软引用的情况较多，这是因为软引用可以加速JVM对垃圾内存的回收速度，可以维护系统的运行安全，防止内存溢出（OutOfMemory）等问题的产生

​	利用软引用和弱引用解决OOM问题：假如有一个应用需要读取大量的本地图片，如果每次读取图片都从硬盘读取，则会严重影响性能，但是如果全部加载到内存当中，又有可能造成内存溢出，此时使用软引用可以解决这个问题。

​	设计思路是：用一个HashMap来保存图片的路径和相应图片对象关联的软引用之间的映射关系，在内存不足时，JVM会自动回收这些缓存图片对象所占用的空间，从而有效地避免了OOM的问题。

------

参考资料：

1、https://blog.csdn.net/qq_39192827/article/details/85611873

2、https://www.cnblogs.com/dolphin0520/p/3784171.html