## javase
**javase部分的就不多说了，springMVC部分开始再说详细点吧**
### 简单数据类型
简单类型 | 大小 |  范围/精度
-|-|-
 float|4字节|754单精度 
 double |8字节|754双精度 
 byte |1字节 |-128到127 
 short|2字节 |-32,768到32,767 
 int |4字节 |-2,147,483,648到2,147,483,647 
 long |8字节| -9,223,372,036,854,775,808到9,223,372,036, 854,775,807 
 char |2字节| 整个Unicode字符集 
 boolean|1字节| True或者false 
 ### 索引
 索引类型可以“引用”变量，分成三种，它们是：类（class）、接口（interface）和数组（array）
 #### 类
 定义方法和数据的数据类型
#### 数组
Java数组（array）是动态创建的索引对象,数组对象由元素组成,长度不变，数目可以为0。
### 封装
将对象信息，内部逻辑结构隐藏起来。封装就是将属性私有化，提供公有的方法访问私有属性。 
数据类型 |  包装类
-|-
int|integer
short|Short
long|Long
byte|Byte
float|Float
double|Double
char|Character
boolean|Boolean
### 常用api
#### Scanner
```java
import java.util.Scanner; 
public class Main { 
    public static void main(String[] args) { 
        Scanner scan = new Scanner(System.in); // 从键盘接收数据 
        System.out.println("请输入：");
        String str1 = scan.next(); 
        System.out.println("输入的数据为：" + str1);
        scan.close(); 
    } 
}
```
#### Random
用于返回一个随机数，随机数范围为 0.0 =< Math.random < 1.0。
```java
public class Main{ 
    public static void main(String args[]){ 
        System.out.println( Math.random() ); 
        System.out.println( Math.random() ); 
    }
}
```
#### String
创建字符串最简单的方式，内有很多常用方法
```java
String str = "字符串";
String str = "123";
System.out.println(str.length());//3
str=str.concat(str2);
System.out.println(str);//字符串123
System.out.println(str.charAt(2));//串
```
#### ArrayList
一个特殊的数组。通过添加和删除元素，就可以动态改变数组的长度
```java
 List<String> List1=new ArrayList<String>();
 List1.add("a");
 List1.add("b");
 System.out.println(List1);//[a,b]
 List1.add(1,"c");
 System.out.println(List1);//[a,c,b]
 List1.remove(2);
 System.out.println(List1);//[a,c]
 String str = String.join("",List1);//每个元素用""连接起来
 System.out.println(str);//ac
```
#### static
1.static可以修饰变量，方法。
2.被static修饰的变量或者方法是独立于该类的任何对象，也就是说，这些变量和方法不属于任何一个实例对象，而是被类的实例对象所共享。
3.在类被加载的时候，就会去加载被static修饰的部分。
4.被static修饰的变量或者方法是优先于对象存在的，也就是说当一个类加载完毕之后，即便没有创建对象，也可以去访问。
#### Arrays
Java的Arrays类封装了很多与数组有关的方法。
```java
int[] arr = { 9, 8, 3, 5, 2 };
Arrays.sort(arr);//排序
int num = Arrays.binarySearch(arr,3);// 查找指定元素3的索引
System.out.println(num);//1
```
#### Math
Java的Math类封装了很多与数学有关的属性和方法。
```java
 int x=1,y=2;    
 Math.sqrt(y);//计算平方根    
 Math.cbrt(y);//计算立方根    
 Math.hypot(x,y);//计算 (x的平方+y的平方)的平方根    
 Math.pow(x,y);//计算a的b次方    
 Math.exp(x);//计算e^x的值    
 Math.max(x,y);//计算最大值    
 Math.min(x,y);//计算最小值
```
### 继承
A类为子类，B类为父类。则A类具有B类拥有的属性和方法，除了构造方法、私有属性和私有方法。子类可以添加自由独有的属性和方法来拓展功能。
一个父类可以有多个子类，但是一个子类只能有一个父类。
```java

class Father{
    String name ="父亲";
}
class Son extends Father{
    String name ="儿子";
}
public class Main{
    public static void main(String args[]){
        Son s =new Son();
        System.out.println(s.name); //儿子
        //若注释掉String name ="儿子"; 结果为：父亲
    }
}
```
### 抽象类
父类中有abstract属性的函数，子类必须实现；有一个或多个abstract函数，则该类为抽象类。
### super
super.run();//调用父类的run方法
 #### 接口
好比一种模版，这种模版定义了对象必须实现的方法，其目的就是让这些方法可以作为接口实例被引用，接口不能被实例化
 ```java
interface man {
    public void run();
}
class oldman implements man{
    public void run(){
        System.out.println("oldman run!");
    }
}
public class Main{
    public static void main(String args[]){
        oldman o = new oldman();
        o.run();  //oldman run!
    }
}
```
#### 多态
多态就是同一个接口，使用不同的实例而执行不同操作
```java
interface human{

}
class man implements human{    
    public void run(){       
      System.out.println("man");    
    }
}
class woman implements human{    
    public void run(){        
      System.out.println("human");    
    }
}
```
### final
用final去修饰一个类的时候，表示这个类不能被继承,如果父类中有final修饰的方法，那么子类不能去重写。
修饰成员变量时必须要赋初始值，而且是只能初始化一次。
### 四种权限
权限|作用域
-|-
 public|全部可访问
 protected|非子类的外包不可访问
 default|只有本包可用
 private|只有本类可用
 ### 匿名内部类
假如一个局部内部类只被用一次（只用它构建一个对象），就可以不用对其命名了，这种没有名字的类被称为匿名内部类（anonymous inner class）
 
 
