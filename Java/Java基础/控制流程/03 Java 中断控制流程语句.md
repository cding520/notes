[TOC]

## 一、continue

continue适用于任何循环控制结构中，作用是让程序立刻跳转到下一次循环的迭代。

在for循环中，continue语句使程序立即跳转到更新语句。

在while或者do…while循环中，程序立即跳转到布尔表达式的判断语句。

即：**跳出本次循环。**

> 语法

continue就是循环体中一条简单的语句：

```java
continue;
```

实例

```java
public class Test {

   public static void main(String[] args) {
      int [] numbers = {10, 20, 30, 40, 50};

      for(int x : numbers ) {
         if( x == 30 ) {
	      continue;
         }
         System.out.print( x );
         System.out.print("\n");
      }
   }
}
```

以上实例编译运行结果如下：

```
10
20
40
50
```

## 二、break

break主要用在循环语句或者switch语句中，用来跳出整个语句块。

**break跳出本层的循环**，并且继续执行该循环下面的语句。

### 2.1、跳出本层循环

> 语法

break的用法很简单，就是循环结构中的一条语句：

```java
break;
```

实例

```java
public class Test {

   public static void main(String[] args) {
      int [] numbers = {10, 20, 30, 40, 50};

      for(int x : numbers ) {
         if( x == 30 ) {
	      break;
         }
         System.out.print( x );
         System.out.print("\n");
      }
   }
}
```

以上实例编译运行结果如下：

```
10
20
```

### 2.1、跳出多层循环

> 语法

```java
label:
..... //循环体
    if(条件) break label;
```

注意：标签必须放在希望跳出的最外层循环之前， 并且必须紧跟一个冒号 。

将标签应用到任何语句中， 甚至可以应用到 if语句或者块语句中，如下所示： 

```java
label:
{
    if (dondition) break label; //跳转到
}
```

实例

```
public class Test {

    public static void main(String[] args) {
        //标号
        label:
        while (true) {
            System.out.println("while");
            for (int j = 0; j <= 2; j++) {
                System.out.println("j:" + j);
                if (j == 2) {
                    break label;
                }
            }
        }
        System.out.println("*****");

    }

}
```

运行结果：

```
while
j:0
j:1
j:2
*****
```

