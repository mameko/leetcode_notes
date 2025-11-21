# String

```java
public class StringTest {  
    public static void main(String[] args) {
        String H = "hello";  
        String H_1 = H;  
        String H_2 = "hel";  
        String H_3 = H_2 + "lo";  
        String H_4 = H_2.concat("lo");  
              
        System.out.println(H);            // hello
        System.out.println(H_1);          // hello
        System.out.println(H_2);          // hel
        System.out.println(H_3);          // hello
        System.out.println(H_4);          // hello
        
        //==等号测试  
        System.out.println(H == H_1);     // true
        System.out.println(H == H_3);     // false
        System.out.println(H == H_4);     // false
        System.out.println(H_3 == H_4);   // false
              
        //equals函数测试  
        System.out.println(H.equals(H_1));   // true
        System.out.println(H.equals(H_3));   // true
        System.out.println(H.equals(H_4));   // true
        System.out.println(H_3.equals(H_4)); // true
              
        //StringBuilder测试  
        StringBuilder helloBuilder = new StringBuilder("hel");  
        System.out.println(helloBuilder.equals(H_2));    // false
   }   
}  
```

* StringBuilder类并没有重写equals方法，因此使用equals比较时，需要时同一个实例才会返回true。否则返回false。
* 当我们使用“+”或者“concat”方法拼接字符串的时候，会创建一个新的字符串，占用新的内存空间，因此使用“==”判断时返回false。
* String是一种不可变类，字符串一但生成就不能被改变。例如我们使用\*\*‘+’进行字符串连接，会产生新的字符串，原串不会发生任何变化；使用replace()\*\* 进行替换某些字符的时候也是产生新的字符串，不会更改原有字符串。

```java
// Some code
String demo = "Hello,world!";
1.int length = demo.length(); //获取字符串的长度
2.boolean equals = demo.equals("Hello,world"); // 比较两个字符串相等
3.boolean contains = demo.contains("word"); // 是否包含子串
4.String replace = demo.replace("Hello,", "Yeah@"); // 将指定字符串(或正则表达式)替换，返回替换后的结果
5.char little = demo.charAt(5); // 查找字符串中索引为5的字符（索引从0开始）
6.String trim = demo.trim(); // 将字符串左右空格去除，返回去除空格后的结果
7.String concat = demo.concat("Great!"); // 拼接字符串，返回拼接结果
8.char[] charArray = demo.toCharArray(); // 返回该字符串组成的字符数组
9.String upperCase = demo.toUpperCase(); // 返回该字符串的大写形式
10.String lowerCase = demo.toLowerCase(); // 返回该字符串的小写形式
```

