## 一、常用操作

java.util.Arrays类能方便地操作数组，它提供的所有方法都是静态的。 具有以下功能：

- 给数组赋值：通过 fill 方法。
- 对数组排序：通过 sort 方法,按升序。
- 比较数组：通过 equals 方法比较数组中元素值是否相等。
- 查找数组元素：通过 binarySearch 方法能对排序好的数组进行二分查找法操作。

具体方法如下：

| 序号 | 方法                                                   | 说明                                                         |
| :--: | :----------------------------------------------------- | :----------------------------------------------------------- |
|  1   | public static int binarySearch(Object[] a, Object key) | 用二分查找算法在给定数组中搜索给定值的对象(Byte,Int,double等)。数组在调用前必须排序好的。如果查找值包含在数组中，则返回搜索键的索引；否则返回 (-(插入点) - 1)。 |
|  2   | public static boolean equals(long[] a, long[] a2)      | 如果两个指定的 long 型数组彼此相等，则返回 true。如果两个数组包含相同数量的元素，并且两个数组中的所有相应元素对都是相等的，则认为这两个数组是相等的。换句话说，如果两个数组以相同顺序包含相同的元素，则两个数组是相等的。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。 |
|  3   | public static void fill(int[] a, int val)              | 将指定的 int 值分配给指定 int 型数组指定范围中的每个元素。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。 |
|  4   | public static void sort(Object[] a)                    | 对指定对象数组根据其元素的自然顺序进行升序排列。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。 |

## 二、示例

```java
public class TestArrays { 
        public static void main(String[] args) { 
                int[] i = new int[10]; 
                //填充数组 
                Arrays.fill(i, 2); 
                //遍历数组 
                for (int x : i) { 
                        System.out.print(x + " "); 
                } 
                //toString()数组 
                System.out.println("\n" + Arrays.toString(i)); 
                //复制数组 
                int[] b = new int[12]; 
                System.arraycopy(i, 0, b, 2, 5); 
                System.out.println(Arrays.toString(b)); 
                //一维数组的比较 
                int[] c = new int[3]; 
                int[] d = new int[3]; 
                Arrays.fill(c, 3); 
                Arrays.fill(d, 3); 
                System.out.println(c.equals(d)); 
                System.out.println(Arrays.equals(c, d)); 
                System.out.println("-------------"); 
                int[][] a1 = {{1, 2, 3}, {4, 5, 6}}; 
                int[][] a2 = {{1, 2, 3}, {4, 5, 6}}; 
                System.out.println(a1.equals(a2)); 
                System.out.println(Arrays.equals(a1, a2)); 
                System.out.println(Arrays.deepEquals(a1, a2)); 
                //深度toString() 
                System.out.println(Arrays.toString(a1)); 
                System.out.println(Arrays.deepToString(a1)); 

                //数组的排序 
                int[] a3 = {3, 2, 5, 4, 1}; 
                System.out.println(Arrays.toString(a3)); 
                Arrays.sort(a3); 
                System.out.println(Arrays.toString(a3)); 
                //一维数组数值检索 
                int index1 = Arrays.binarySearch(a3, 4); 
                int index2 = Arrays.binarySearch(a3, -12); 
                int index3 = Arrays.binarySearch(a3, 8); 
                System.out.println(index1 + " " + index2 + " " + index3); 
        } 
}
```

执行结果：

```java
2 2 2 2 2 2 2 2 2 2    
[2, 2, 2, 2, 2, 2, 2, 2, 2, 2] 
[0, 0, 2, 2, 2, 2, 2, 0, 0, 0, 0, 0] 
false 
true 
------------- 
false 
false 
true 
[[I@3e25a5, [I@19821f] 
[[1, 2, 3], [4, 5, 6]] 
[3, 2, 5, 4, 1] 
[1, 2, 3, 4, 5] 
3 -1 -6 
```

## Arrays.asList获得的List使用时需要注意什么

- asList 得到的只是一个 Arrays 的内部类，一个原来数组的视图 List，因此如果对它进行增删操作会报错
- 用 ArrayList 的构造器可以将其转变成真正的 ArrayList

