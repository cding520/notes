## 一、继承结构图

- **String**

![image-20201111213316226](https://gitee.com/icecandy/imgbed/raw/master/Java/20201111213317.png)

- **AbstractStringBuilder、StringBuffer、StringBuilder**

![image-20201111213339288](https://gitee.com/icecandy/imgbed/raw/master/Java/20201111213340.png)

## 二、String

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence
```

- String是一个final类，不能被继承的类
- String类实现了`java.io.Serializable`接口，可以实现序列化
- String类实现了`Comparable<String>`，可以用于比较大小（按顺序比较单个字符的ASCII码）
- String类实现了`CharSequence`接口，表示是一个有序字符的序列，因为String的本质是一个char类型数组

## 三、AbstractStringBuilder

### 3.1、抽象类的定义

```java
abstract class AbstractStringBuilder implements Appendable, CharSequence
```

### 3.2、字段属性

```java
char[] value;
int count;  //字符使用的长度
```

### 3.3、构造函数

```java
AbstractStringBuilder() {
}

AbstractStringBuilder(int capacity) {
    value = new char[capacity];
}
```

### 3.4、长度和容量

```java
public int length() { //表示有多少个字符
    return count;
}
public int capacity() { //表示数组（字符串）的最大容量
    return value.length;
}
```

### 3.5、append

```java
public AbstractStringBuilder append(Object obj) {
    return append(String.valueOf(obj)); // 转为字符串
}

public AbstractStringBuilder append(String str) {
    if (str == null)
        return appendNull(); //添加null
    int len = str.length();
    ensureCapacityInternal(count + len); //确保value数组够大
    str.getChars(0, len, value, count); //复制
    count += len;
    return this;
}
//添加null
private AbstractStringBuilder appendNull() {
    int c = count;
    ensureCapacityInternal(c + 4);
    final char[] value = this.value;
    value[c++] = 'n';
    value[c++] = 'u';
    value[c++] = 'l';
    value[c++] = 'l';
    count = c;
    return this;
}
private void ensureCapacityInternal(int minimumCapacity) {
    // overflow-conscious code
    if (minimumCapacity - value.length > 0) { //count+str.length是否大于数组的容量
        value = Arrays.copyOf(value,
                newCapacity(minimumCapacity)); // 扩容
    }
}

private int newCapacity(int minCapacity) {
    // overflow-conscious code
    int newCapacity = (value.length << 1) + 2; //将value数组的容量扩大为原来的两倍+2
    if (newCapacity - minCapacity < 0) { //对比扩大之后的容量与count+length的大小
        newCapacity = minCapacity;
    }
    return (newCapacity <= 0 || MAX_ARRAY_SIZE - newCapacity < 0) //返回新数组容量
        ? hugeCapacity(minCapacity)  //Integer.MAX_VALUE - 8
        : newCapacity;
}
//是的count==length，节约数组空间
public void trimToSize() {
    if (count < value.length) {
        value = Arrays.copyOf(value, count);
    }
}
```

### 3.6、setCharAt

```java
//由于value没有想String声明为final，所以可以进行修改
public void setCharAt(int index, char ch) {
    if ((index < 0) || (index >= count))
        throw new StringIndexOutOfBoundsException(index);
    value[index] = ch;
}
```

## 四、StringBuffer

```java
public final class StringBuffer
   extends AbstractStringBuilder
   implements java.io.Serializable, CharSequence
```

 继承了AbstractStringBuilder的所有方法，方法实现基本都是调用父类的方法，在外层封装同步，线程安全

## 五、StringBuilder

```java
public final class StringBuilder
    extends AbstractStringBuilder
    implements java.io.Serializable, CharSequence
```

 继承了AbstractStringBuilder的所有方法，方法实现基本都是调用父类的方法，非线程安全

## 六、String、StringBuffer，StringBuilder三者的区别和使用

### 6.1、从是否可变的角度：

- String类中使用字符数组保存字符串，因为有“final”修饰符，所以String对象是不可变的
- StringBuffer和StringBuilder都继承自AbstractStringBuilder类，在AbstractStringBuilder中也是使用字符数组保存字符串，但没有“final”修饰符，所以两种对象都是可变的

### 6.2、是否多线程安全：

- String中的对象是不可变的，也就可以理解为常量，所以是线程安全的。
- StringBuffer对方法加了同步锁(synchronized) ，所以是线程安全的
- StringBuilder并没有对方法进行加同步锁，所以是非线程安全的，效率最高

### 6.3、使用

- 如果你要求字符串不可变，那么应该选择String类
- 如果你需要字符串可变并且是线程安全的，那么你应该选择StringBuffer类
- 如果你要求字符串可变并且不存在线程安全问题，那么你应该选择StringBuilder类

------

参考文献：

1、https://www.cnblogs.com/zsql/p/11367654.html