|      |            |   |
| :--: | :-- | :------- | :-------- |
| 序列 | 方法名   | 作用 | 备注 |
|  1   | char charAt(int index)                                       | 返回指定索引处的 char 值。                                   |                                                              |
|  2   | int compareTo(Object o)                                      | 把这个字符串和另一个对象比较。                               |                                                              |
|  3   | int compareTo(String anotherString)                          | 按字典顺序比较两个字符串。                                   |                                                              |
|  4   | int compareToIgnoreCase(String str)                          | 按字典顺序比较两个字符串，不考虑大小写。                     |                                                              |
|  5   | String concat(String str)                                    | 将指定字符串连接到此字符串的结尾。                           |                                                              |
|  6   | boolean contentEquals(StringBuffer sb)                       | 当且仅当字符串与指定的StringBuffer有相同顺序的字符时候返回真。 |                                                              |
|  7   | static String copyValueOf(char[] data)                       | 返回指定数组中表示该字符序列的 String。                      | 源码中直接返回了一个新的String对象                           |
|  8   | static String copyValueOf(char[] data, int offset, int count) | 返回指定数组中表示该字符序列的 String。                      |                                                              |
|  9   | boolean endsWith(String suffix)                              | 判断此字符串是否以指定的后缀结束。                           |                                                              |
|  10  | boolean equals(Object anObject)                              | 将此字符串与指定的对象比较。                                 | 判断是String对象里面的值是否相等。 注意与"=="区别，"=="的作用：  1、基本数据类型的比较；  2、判断引用是否指向堆内存的同一块地址。 而Object的equals方法是比较引用是否相等。 |
|  11  | boolean equalsIgnoreCase(String anotherString)               | 将此 String 与另一个 String 比较，不考虑大小写。             |                                                              |
|  12  | byte[] getBytes()                                            | 使用平台的默认字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。 |                                                              |
|  13  | byte[] getBytes(String charsetName)                          | 使用指定的字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。 |                                                              |
|  14  | void getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin) | 将字符从此字符串复制到目标字符数组。                         |                                                              |
|  15  | int hashCode()                                               | 返回此字符串的哈希码。                                       |                                                              |
|  16  | int indexOf(int ch)                                          | 返回指定字符在此字符串中第一次出现处的索引。                 | ch为该字符的Unicode编码                                      |
|  17  | int indexOf(int ch, int fromIndex)                           | 返回在此字符串中第一次出现指定字符处的索引，从指定的索引开始搜索。 |                                                              |
|  18  | int indexOf(String str)                                      | 返回指定子字符串在此字符串中第一次出现处的索引。             |                                                              |
|  19  | int indexOf(String str, int fromIndex)                       | 返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始。 |                                                              |
|  20  | String intern()                                              | 本地方法，作用就是去字符串常量池中寻找str字符串，如果有则返回str在常量池中的引用，如果没有则在常量池中创建str对象。 | 就是把该字符串放入常量池中                                   |
|  21  | int lastIndexOf(int ch)                                      | 返回指定字符在此字符串中最后一次出现处的索引。               | ch为该字符的Unicode编码                                      |
|  22  | int lastIndexOf(int ch, int fromIndex)                       | 返回指定字符在此字符串中最后一次出现处的索引，从指定的索引处开始进行反向搜索。 |                                                              |
|  23  | int lastIndexOf(String str)                                  | 返回指定子字符串在此字符串中最右边出现处的索引。             |                                                              |
|  24  | int lastIndexOf(String str, int fromIndex)                   | 返回指定子字符串在此字符串中最后一次出现处的索引，从指定的索引开始反向搜索。 |                                                              |
|  25  | int length()                                                 | 返回此字符串的长度。                                         |                                                              |
|  26  | boolean matches(String regex)                                | 判断字符串是否匹配给定的正则表达式。                         |                                                              |
|  27  | boolean regionMatches(boolean ignoreCase, int toffset, String other, int ooffset, int len) | 判断两个字符串区域是否相等                                   | ooffset: 从第几个字符开始; len: 要比较的字符数               |
|  28  | boolean regionMatches(int toffset, String other, int ooffset, int len) | 判断两个字符串区域是否相等。                                 |                                                              |
|  29  | String replace(char oldChar, char newChar)                   | 返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 得到的。 | 参数为单个字符                                               |
|  30  | String replaceAll(String regex, String replacement)          | 使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串。 | 参数regex可以是一个正则表达式，也可以是一个字符串            |
|  31  | String replaceFirst(String regex, String replacement)        | 使用给定的 replacement 替换此字符串匹配给定的正则表达式的第一个子字符串。 | 参数regex可以是一个正则表达式，也可以是一个字符串            |
|  32  | String[] split(String regex)                                 | 根据给定正则表达式的匹配拆分此字符串。                       | 参数regex：正则表达式或者自定的特殊字符串（例如:逗号','等）。 注意： 1、分隔符为“.”(无输出),“\|”(不能得到正确结果)转义字符时,“*”,“+”时出错抛出异常,都必须在前面加必须得加"\\",如split(\\|); 2、如果用"\"作为分隔,就得写成这样：String.split("\\\\"),因为在Java中是用"\\"来表示""的,字符串得写成这样：String Str="a\\b\\c"; 转义字符,必须得加"\\";  3、如果在一个字符串中有多个分隔符,可以用"\|"作为连字符,比如：String str="Java string-split#test",可以用Str.split(" \|-\|#")把每个字符串分开; |
|  33  | String[] split(String regex, int limit)                      | 根据匹配给定的正则表达式来拆分此字符串。                     | 参数limit：分割的份数                                        |
|  34  | boolean startsWith(String prefix)                            | 判断此字符串是否以指定的前缀开始。                           |                                                              |
|  35  | boolean startsWith(String prefix, int toffset)               | 判断此字符串从指定索引开始的子字符串是否以指定前缀开始。     |                                                              |
|  36  | CharSequence subSequence(int beginIndex, int endIndex)       | 返回一个新的字符序列，它是此序列的一个子序列。               |                                                              |
|  37  | String substring(int beginIndex)                             | 返回一个新的字符串，它是此字符串的一个子字符串。             |                                                              |
|  38  | String substring(int beginIndex, int endIndex)               | 返回一个新字符串，它是此字符串的一个子字符串。               | 返回的字符串包括其实的字符，不包括结束的字符。即：左边右开。 |
|  39  | char[] toCharArray()                                         | 将此字符串转换为一个新的字符数组。                           |                                                              |
|  40  | String toLowerCase()                                         | 使用默认语言环境的规则将此 String 中的所有字符都转换为小写。 |                                                              |
|  41  | String toLowerCase(Locale locale)                            | 使用给定 Locale 的规则将此 String 中的所有字符都转换为小写。 |                                                              |
|  42  | String toString()                                            | 返回此对象本身（它已经是一个字符串！）。                     |                                                              |
|  43  | String toUpperCase()                                         | 使用默认语言环境的规则将此 String 中的所有字符都转换为大写。 |                                                              |
|  44  | String toUpperCase(Locale locale)                            | 使用给定 Locale 的规则将此 String 中的所有字符都转换为大写。 |                                                              |
|  45  | String trim()                                                | 返回字符串的副本，忽略前导空白和尾部空白。                   |                                                              |
|  46  | static String valueOf(primitive data type x)                 | 返回给定data type类型x参数的字符串表示形式。                 |                                                              |
|  47  | codePointAt(int index)                                       | 返回指定索引处的字符（Unicode 代码点）。                     | Unicode编码前128个字符就是ASCII码，之后是扩展码              |
|  48  | codePointBefore(int index)                                   | 返回指定索引之前的字符（Unicode 代码点）。                   |                                                              |
|  49  | codePointCount(int beginIndex, int endIndex)                 | 返回此 String 的指定文本范围中的 Unicode 代码点数。          |                                                              |