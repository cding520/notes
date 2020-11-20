## 一、String对象的实现的变迁

 String对象是 Java 中使用最频繁的对象之一，所以 Java 公司也在不断的对String对象的实现进行优化，以便提升String对象的性能，String对象的优化过程大致如下：

![image-20201111210759475](https://gitee.com/icecandy/imgbed/raw/master/Java/基础/20201111210808.png)

### 1.1、在 Java6 以及之前的版本中

 String对象是对 char 数组进行了封装实现的对象，主要有四个成员变量：char 数组、偏移量 offset、字符数量 count、哈希值 hash。

 String对象是通过offset和count两个属性来定位char[]数组，获取字符串。这么做可以高效、快速地共享数组对象，同时节省内存空间，但这种方式很有可能会导致内存泄漏。

### 1.2、从 Java7 版本开始到 Java8 版本

 从 Java7 版本开始，Java 对String类做了一些改变。String类中不再有 offset 和 count 两个变量了。这样的好处是String对象占用的内存稍微少了些，同时 String.substring 方法也不再共享 char[]，从而解决了使用该方法可能导致的内存泄漏问题。

### 1.3、从 Java9 版本开始

 将 char[] 数组改为了 byte[] 数组，为什么需要这样做呢？我们知道 char 是两个字节，如果用来存一个字节的字符有点浪费，为了节约空间，Java 公司就改成了一个字节的byte来存储字符串。这样在存储一个字节的字符是就避免了浪费。

 在 Java9 维护了一个新的属性 coder，它是编码格式的标识，在计算字符串长度或者调用 indexOf() 函数时，需要根据这个字段，判断如何计算字符串长度。coder 属性默认有 0 和 1 两个值， 0 代表Latin-1（单字节编码），1 代表 UTF-16 编码。如果 String判断字符串只包含了 Latin-1，则 coder 属性值为 0 ，反之则为 1

## 二、String的字段属性及构造方法

### 2.1、字段属性

```java
//用来存字符串,字符串的本质，是一个final的char型数组
 private final char value[];                                         

 //缓存字符串的哈希，是String实例化的hashcode的一个缓存。因为String经常被用于比较，比如在HashMap中。如果每次进行比较都重新计算hashcode的值的话，那无疑是比较麻烦的，而保存一个hashcode的缓存无疑能优化这样的操作
 private int hash;    // Default to 0                                

 //实现序列化的标识
 private static final long serialVersionUID = -6849794470754667710L; 
```

### 2.2、构造函数

 String类有很多个构造函数，包括接收String，char[],byte[],StringBuffer等多种参数类型，其本质实现就是把参数的值根据需求（例如，起始位置，个数等）赋值给value[]变量。下面例举一部分：

```java
    /** 01
    * 这是一个经常会使用的String的无参构造函数.
    * 默认将""空字符串的value赋值给实例对象的value，也是空字符
    * 相当于深拷贝了空字符串""
    */
    public String() {
        this.value = "".value;
    }

    /** 02
    * 这是一个有参构造函数，参数为一个String对象
    * 将形参的value和hash赋值给实例对象作为初始化
    * 相当于深拷贝了一个形参String对象
    */
    public String(String original) {
        this.value = original.value;
        this.hash = original.hash;
    }

    /** 03
    * 这是一个有参构造函数，参数为一个char字符数组
    * 意义就是通过字符数组去构建一个新的String对象
    */
    public String(char value[]) {
        this.value = Arrays.copyOf(value, value.length);
    }

    /** 04
    * 这是一个有参构造函数，参数为char字符数组,offset(起始位置，偏移量),count(个数)
    * 作用就是在char数组的基础上，从offset位置开始计数count个，构成一个新的String的字符串
    * 意义就类似于截取count个长度的字符集合构成一个新的String对象
    */
    public String(char value[], int offset, int count) {
        if (offset < 0) {        //如果起始位置小于0，抛异常
            throw new StringIndexOutOfBoundsException(offset);
        }
        if (count <= 0) {
            if (count < 0) {     //如果个数小于0，抛异常
                throw new StringIndexOutOfBoundsException(count);
            }
            if (offset <= value.length) {      //在count = 0的前提下，如果offset<=len，则返回""
                this.value = "".value;
                return;
            }
        }
        // Note: offset or count might be near -1>>>1.
        //如果起始位置>字符数组长度 - 个数,则无法截取到count个字符，抛异常
        if (offset > value.length - count) { 
            throw new StringIndexOutOfBoundsException(offset + count);
        }
        //重点，从offset开始，截取到offset+count位置(不包括offset+count位置)
        this.value = Arrays.copyOfRange(value, offset, offset+count); 
    }
```

## 三、字符串的不可变形

### 3.1、定义一个字符串

```java
String s = "abcd";
```

![image-20201111211108986](https://gitee.com/icecandy/imgbed/raw/master/Java/基础/20201111211110.png)

 `s`中保存了string对象的引用。箭头可以理解为“存储他的引用”。

### 3.2、使用变量来赋值变量

```java
String s2 = s;
```

![image-20201111211206080](https://gitee.com/icecandy/imgbed/raw/master/Java/基础/20201111211207.png)

`s2`保存了相同的引用值，因为他们代表同一个对象。

### 3.3、字符串连接

```java
s = s.concat("ef");
```

![image-20201111211245305](https://gitee.com/icecandy/imgbed/raw/master/Java/基础/20201111211246.png)

`s`中保存的是一个重新创建出来的string对象的引用。

### 3.4、总结

 一旦一个string对象在内存(堆)中被创建出来，他就无法被修改。特别要注意的是，**String类的所有方法都没有改变字符串本身的值，都是返回了一个新的对象。**

 如果你需要一个可修改的字符串，应该使用`StringBuffer`或者 `StringBuilder`。否则会有大量时间浪费在垃圾回收上，因为每次试图修改都有新的string对象被创建出来。

## 四、String对“+”的重载

### 3.1、“+”连接符的实现原理

 Java语言为“+”连接符以及对象转换为字符串提供了特殊的支持，字符串对象可以使用“+”连接其他对象。其中字符串连接是通过 StringBuilder（或 StringBuffer）类及其append 方法实现的，对象转换为字符串是通过 toString 方法实现的，该方法由 Object 类定义，并可被 Java 中的所有类继承。  可以通过反编译验证一下

```java
/**
 * 测试代码
 */
public class Test {
    public static void main(String[] args) {
        int i = 10;
        String s = "abc";
        System.out.println(s + i);
    }
}

/**
 * 反编译后
 */
public class Test {
    public static void main(String args[]) {    //删除了默认构造函数和字节码
        byte byte0 = 10;      
        String s = "abc";      
        System.out.println((new StringBuilder()).append(s).append(byte0).toString());
    }
}
```

 由上可以看出，Java中使用"+"连接字符串对象时，会创建一个StringBuilder()对象，并调用append()方法将数据拼接，最后调用toString()方法返回拼接好的字符串。由于append()方法的各种重载形式会调用String.valueOf方法，所以我们可以认为

```java
//以下两者是等价的
s = i + ""
s = String.valueOf(i);
 
//以下两者也是等价的
s = "abc" + i;
s = new StringBuilder("abc").append(i).toString();
```

### 3.2、“+”连接符的效率

 使用“+”连接符时，JVM会隐式创建StringBuilder对象，这种方式在大部分情况下并不会造成效率的损失，不过在进行大量循环拼接字符串时则需要注意。

```java
String s = "abc";
for (int i=0; i<10000; i++) {
    s += "abc";
}

/**
 * 反编译后
 */
String s = "abc";
for(int i = 0; i < 1000; i++) {
     s = (new StringBuilder()).append(s).append("abc").toString();    
}
```

 这样由于大量StringBuilder创建在堆内存中，肯定会造成效率的损失，所以在这种情况下建议在循环体外创建一个StringBuilder对象调用append()方法手动拼接（如上面例子如果使用手动拼接运行时间将缩小到1/200左右）。

```java
/**
 * 循环中使用StringBuilder代替“+”连接符
 */
StringBuilder sb = new StringBuilder("abc");
for (int i = 0; i < 1000; i++) {
    sb.append("abc");
}
sb.toString();
```

 与此之外还有一种特殊情况，也就是当"+"两端均为编译期确定的字符串常量时，编译器会进行相应的优化，直接将两个字符串常量拼接好，例如：

```java
System.out.println("Hello" + "World");

/**
 * 反编译后
 */
System.out.println("HelloWorld");
/**
 * 编译期确定
 * 对于final修饰的变量，它在编译时被解析为常量值的一个本地拷贝存储到自己的常量池中或嵌入到它的字节码流中。
 * 所以此时的"a" + s1和"a" + "b"效果是一样的。故结果为true。
 */
String s0 = "ab"; 
final String s1 = "b"; 
String s2 = "a" + s1;  
System.out.println((s0 == s2)); //result = true
/**
 * 编译期无法确定
 * 这里面虽然将s1用final修饰了，但是由于其赋值是通过方法调用返回的，那么它的值只能在运行期间确定
 * 因此s0和s2指向的不是同一个对象，故上面程序的结果为false。
 */
String s0 = "ab"; 
final String s1 = getS1(); 
String s2 = "a" + s1; 
System.out.println((s0 == s2)); //result = false 
 
public String getS1() {  
    return "b";   
}
```

 综上，“+”连接符对于直接相加的字符串常量效率很高，因为在编译期间便确定了它的值，也就是说形如"I"+“love”+“java”; 的字符串相加，在编译期间便被优化成了"Ilovejava"。对于间接相加（即包含字符串引用，且编译期无法确定值的），形如s1+s2+s3; 效率要比直接相加低，因为在编译器不会对引用变量进行优化。

### 4.3、总结

- `String s = "a" + "b"`，编译器会进行常量折叠(因为两个都是编译期常量，编译期可知)，即变成 `String s = "ab"`
- 对于能够进行优化的(`String s = "a" + 变量` 等)用 `StringBuilder` 的 `append()` 方法替代，最后调用 `toString()` 方法 (底层就是一个 `new String()`)

## 五、intern 方法

 直接使用双引号声明出来的String对象会直接存储在字符串常量池中，如果不是用双引号声明的String对象，可以使用String提供的intern方法。intern方法是一个native方法，intern方法会从字符串常量池中查询当前字符串是否存在，如果存在，就直接返回当前字符串；如果不存在就会将当前字符串放入常量池中，之后再返回。  JDK1.7的改动：

- 将String常量池 从 Perm 区移动到了 Java Heap区
- String.intern() 方法时，如果存在堆中的对象，会直接保存对象的引用，而不会重新创建对象。

```java
/**
 * Returns a canonical representation for the string object.
 * <p>
 * A pool of strings, initially empty, is maintained privately by the
 * class {@code String}.
 * <p>
 * When the intern method is invoked, if the pool already contains a
 * string equal to this {@code String} object as determined by
 * the {@link #equals(Object)} method, then the string from the pool is
 * returned. Otherwise, this {@code String} object is added to the
 * pool and a reference to this {@code String} object is returned.
 * <p>
 * It follows that for any two strings {@code s} and {@code t},
 * {@code s.intern() == t.intern()} is {@code true}
 * if and only if {@code s.equals(t)} is {@code true}.
 * <p>
 * All literal strings and string-valued constant expressions are
 * interned. String literals are defined in section 3.10.5 of the
 * <cite>The Java&trade; Language Specification</cite>.
 *
 * @return  a string that has the same contents as this string, but is
 *          guaranteed to be from a pool of unique strings.
 */
public native String intern();
```

### 4.1、intern的用法

```java
static final int MAX = 1000 * 10000;
static final String[] arr = new String[MAX];

public static void main(String[] args) throws Exception {
    Integer[] DB_DATA = new Integer[10];
    Random random = new Random(10 * 10000);
    for (int i = 0; i < DB_DATA.length; i++) {
        DB_DATA[i] = random.nextInt();
    }
    long t = System.currentTimeMillis();
    for (int i = 0; i < MAX; i++) {
        //arr[i] = new String(String.valueOf(DB_DATA[i % DB_DATA.length]));
         arr[i] = new String(String.valueOf(DB_DATA[i % DB_DATA.length])).intern();
    }

    System.out.println((System.currentTimeMillis() - t) + "ms");
    System.gc();
}
```

 总之：`intern`方法节约了空间，延长了时间。

------

参考文献：

1、https://blog.csdn.net/qq_34490018/article/details/82110578
2、https://blog.csdn.net/ifwinds/article/details/80849184
3、https://www.cnblogs.com/zsql/p/11367654.html