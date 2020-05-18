# 1. java基础
## 1.1. 可移植性
Java中的int永远为32位的整数，而C/C++中，int可能是16位、32位。二进制数据以固定的格式进行存储和传输，消除了字节顺序的困扰。字符串是用标准的Unicode格式存储。

## 1.2. 编程基础
【类名】</br>
必须以大写字母开头，如果名字由多个单词组成，每个单词的第一个字母都应该大写

【文件名】</br>
必须类名.java</br>
可以通过这段命令执行程序：`java 类名`

类的源文件必须包含一个main方法，并且必须是public的。其声明方式如下：

```JAVA
public class ClassName
{
    public static void main(String[] args)
    {
        program
    }
}
```

### 1.2.1. 打印方法
`System.out.ptintln();`

### 1.2.2. 数据类型
共8种，其中4种整数型、2种浮点类型、1种用于表示Unicode编码的字符单元的字符类型char和1种用于用于表示真值的boolean类型

#### 1.2.2.1. 整数型
- `int` 4个字节 极限是20亿
- `short` 2个字节
- `long` 8个字节
- `byte` 1个字节

#### 1.2.2.2. 浮点型
- `float` 4字节 （3.14f）如果没有f默认double
- `double` 8字节

`Double.isNan(x)` 检测非数值</br>
浮点数会有精度错误 2.0-1.1=0.899999999999，因为二进制系统中无法精确地表示分数1/10

#### 1.2.2.3. char类型
单引号表示字符，双引号表示字符串
- `\b` 退格
- `\t` 制表符
- `\n` 换行
- `\r` 回车

【注意】</br>
Unicode转义序列会在解析代码之前得到处理。比如`"\u0022+\u0022"`，先将`\u0022`转换成`"`，这会得到`""+""`，结果就是空串</br>
注释中的\u也会引起错误

#### 1.2.2.4. boolean类型
不能讲false看做0 true看做1

#### 1.2.2.5. 变量
几乎所有的unicode除了都可以放入变量名</br>
可以一行中声明多个变量`int i,j;`</br>
必须初始化后才能使用

#### 1.2.2.6. 常量
用关键词`final`指示常量</br>
习惯上常量名使用全大写

【数学函数与常量】</br>
`import static java.lang.Math.*`</br>
`Math.pow(3,4)`

### 1.2.3. 运算符
#### 1.2.3.1. 类型转换
a+b</br>
double>float>long>int</br>
如果有一个double就会转换成double

#### 1.2.3.2. 自增与自减运算符

```
int m=7;
int n=7;
int a=2*++m; // a=16,m=8
int b=2*n++; // b=14,n=8
```

#### 1.2.3.3. 位运算符
- `& | ^ ~` 和 或 异或 非
- `>> << >>>` 右移n位用符号位填充高位 左移n位同 右移n位用0填充

#### 1.2.3.4. 运算级别
`! ~ ++ -- = += -= *= >= >>=`右结合</br>
`a += b += c` 等价于 `a += (b += c)`

#### 1.2.3.5. 枚举
```
enum Size{ SMALL, MEDIUM, LARGE};
Size s = Size.MEDIUM;
```

### 1.2.4. 字符串
**API在50页**
String是标准Java库中提供的预定义类
#### 1.2.4.1. 子串
```JAVA
String greeting = "Hello";
String s = greeting.substring(0,3); 
//"Hel" 复制了0,1,2,左闭右开
```
#### 1.2.4.2. 拼接
```JAVA
String a = "ab";
String b = "cd";
String message = a + b; // "abcd"
// 可以拼任何对象
int age = 13;
String rating = message + age; //"abcd13"
// 用定界符分割
String all = String.join("/", "S", "M", "L"); // "S/M/L"
```

#### 1.2.4.3. 不可变的字符串
字符串内容不可更改，只能创建新的字符串。但是带来的有点是可以让字符串共享

#### 1.2.4.4. 比较字符串
`s.equals(t)`</br>
`"Hello".equalsIgnoreCase("hello")` 不区分大小写的比较</br>
`==`只能判断两个字符串是否放置在同一个位置上。</br>

#### 1.2.4.5. 空串与null串
`str.length()==0` 或 `str.equals("")`</br>
`str == null`

#### 1.2.4.6. 代码单元与码点
一个Unicode字符为一个码点，除了辅助字符，都是一个码点对应一个代码单元，辅助字符一个码点对应两个代码单元

```JAVA
String greeting = "Hello";
char first = greeting.charAt(0); //'H'
char last = gteeting.charAt(4); //'o'
```

#### 1.2.4.7. 构造字符串
如果每次都构造一个新的String对象耗时浪费空间。用StringBuilder可以避免这个问题。</br>
**API在55页**

```JAVA
StringBuilder builder = new StringBuilder();
builder.append(ch);
builder.append(str);
String result = builder.toString();
```

#### 1.2.4.8. StringBuffer
- String
  - 不可变的字符序列
  - 底层用char[] 进行存储
- StringBuffer
  - 可变的字符序列
  - 线程安全 效率低
  - 底层用char[] 进行存储
- StringBuilder
  - 可变的字符序列
  - 无synchronized，线程不安全，效率高 JDK5.0新增
  - 底层用char[] 进行存储

为什么底层一样，String不可变？
```java
// 源码分析
String str = new String(); // char[] value = new char[0];
String str1 = new("abc); // char[] value = new char[]{'a','b','c'};

StringBuffer sb1 = new StringBuffer(); // char[] value = new char[16];
sb1.length(); // 数值为0
sb1.append('a'); //value[0]='a';
sb1.append('b');  //value[1]='b';

StringBuffer sb2 = new StringBuffer("abc"); //char[] value = new char["abc".length() + 16];

System.out.println(sb2.toString());

// 扩容问题：如果添加的数据底层数组放不下了，就需要扩容
// 底层用 Arrays.copyOf(value, newCapacity(个数)); 默认情况下扩容为原来容量的2倍。全部重新复制到新的数组中

```


### 1.2.5. 输入输出
通过控制台进行输入，首先需要构造一个Scanner对象，并与标准输入流System.in关联

```JAVA
import java.util.*; //Scanner类定义在java.util包中
Scanner in = new Scanner(System.in)
String name = in.nextLine();
int num = in.nextInt();
// -------------------------------------
// 为了不让密码直接显示出来 可以用Console类实现
Console cons = System.console();
String username = cons.readLine("User name:");
char[] passwd = cons.readPassword("Password:"); // 出于安全起见，用一维数组，用一个填充值覆盖数组元素。字符串不能修改。
```

#### 1.2.5.1. 格式化输出
```JAVA
double = 3333.333333333335;
System.out.printf("%8.2",x); //用八个字符的宽度和小数点后两个字符的精度打印x
// 3333.33 包括一个空格和7个字符
```
- `%d` 十进制
- `%f` 浮点数
- `%c` 字符
- `%s` 字符串
- `%b` 布尔

【格式化字符串】
`String message = String.format("Hello, %s.", name);`

#### 1.2.5.2. 文件输入与输出
```JAVA
Scanner in = new Scanner(Paths.get("myfile.text"),"UTF-8");
PrintWriter out = new PrintWriter("myfile.txt","UTF-8");
//如果文件不存在会自动创建文件
String dir = System.getProperty("user.dir"); // 获取当前路径
```

### 1.2.6. 控制流程
#### 1.2.6.1. 块作用域
变量只在块内生效`{}`

#### 1.2.6.2. if
`if / else if / else`

#### 1.2.6.3. 循环
`while`</br>

```JAVA
// 如果希望程序至少执行一次
do
{
}
while();
```

```JAVA
for(int i=1;i<10;i++)
{
}
```

#### 1.2.6.4. 多重选择
```JAVA
switch(choice)
{
	case 1:
		...
		break;
	case 2:
		...
		break;
	default:
		...
		break;
}
```

#### 1.2.6.5. 中断控制流程语句
**break 后面可以加东西，用于跳出多层循环。**

```
read_data:
while(..)
{
	for(..)
	{
		if(...)
			break read_data; //跳出read_data循环
	}
}

//可以用于任何语句
label:
{
	...
	if() break label;
	...
}
```

### 1.2.7. 大数值
java.math包中的BigInterger整数和BigDecimal浮点数。

```
BigInteger a = BigInteger.valueOf(100);
BigInteger c = a.add(b);
BigInteger d = c.multiply(a);
```
`add/substract/multiply/divide/mode/compareTo/valueOf`

### 1.2.8. 数组
`int[] a = new int[100]`

#### 1.2.8.1. for each循环
`for (variable : collection)`</br>
比如:

```
for(int element : a)
	System.out.println(element);
```

#### 1.2.8.2. 数组初始化以及匿名数组
```
int[] a = {1,2,3,4,5};
a = new int[] {3,4,5,6,7};
int[] b = new int[0]; //长度为0的数组
```

#### 1.2.8.3. 拷贝数组
```
int[] a = {1,2,3,4,5};
int[] b = a;
b[4] = 10; // a[4]=10
```

如果要拷贝需要使用copyOf</br>
`int[] b = Arrays.copyOf(a, a.length);`第二个参数用来增加大小，也可以设的更大，多出来的用0代替，也可更小，则只取前几个

#### 1.2.8.4. 命令行参数
args[0]第一个参数
```
java Message -g cruel world
/*
args[0]:"-g"
args[1]:"cruel"
args[2]:"world"
*/
```

#### 1.2.8.5. 数组排列
`Arrays.sort(a)`
**关于Arrays的API在84页**

#### 1.2.8.6. 多维数组
`double[][] balances = new double[5][5]`</br>
或者直接赋值</br>
```
int[][] a = 
{
	{1,2,3},
	{4,5,6}
};
```

```
// 使用for each
for(double[] row : a)
	for(double value : row)
		do...
		
// 快速打印二维数组
System.out.println(Arrays.deepToString(a));
```

#### 1.2.8.7. 不规则数组
```
# 构造一个三角形的数组
int[][] odds = new int[NMAX + 1][];
for(int n = 0; n <= NMAX; n++)
	odds[n] = new int[n+1];
```

## 1.3. 类与对象
### 1.3.1. 面向对象概述
#### 1.3.1.1. 类之间的关系
- 依赖</br>
	一个类的方法操纵另一个类的对象，我们就说一个类依赖于另一个类</br>
	尽可能减少相互依赖的类，用软件工程的术语来说，就是让类之间的耦合度最小
- 聚合</br>
	类A的对象包含类B的对象
- 继承

如果一个对象变量没有引用任何对象，需要显示地将对象变量设置为null

### 1.3.2. 预定义类
- `Date类`
- `LocalData类` 获取年月日

#### 1.3.2.1. 更改器方法与访问器方法
更改器方法会让对象的状态发生改变</br>
访问器方法不会更改对象状态，而是创建一个新对象并赋值

### 1.3.3. 自定义类
自定义类通常没有main方法，却有自己的实例域和实例方法，要想创建一个完整的程序，应该将若干类组合在一起，其中只有一个类有main方法

```
class Employee
{
	// instance fields
	private String name;
	private double salary;
	private LocalDate hireDay;
	
	// constructor
	public Employee(String n, double s, int year, int month, int day)
	{
		name = n;
		salary = s;
		hireDay = LocalDate.of(year, month, day);
	}
	
	// a method
	public String getName()
	{
		return name;
	}
}
```

#### 1.3.3.1. 构造器
需要记住：

- 每个类可以有一个以上的构造器
- 构造器可以有0个1个或者多个参数
- 构造器没有返回值
- 构造器总是伴随着new操作一起调用

#### 1.3.3.2. 封装
不要编写返回引用可变对象的访问器方法

```
// Date类有一个更改器方法setTime
class Employee
{
	private Date hireDay;
	...
	public Date getHireDay()
	{
		return hireDay;
	}
}

Employee harry = new Employee(...);
Date d = harry.getHireDay();
// d和harry.hireDay引用同一个对象，如果d发生改变，也会改变实例的私有状态
d.setTime(...);

// 修改
public Date getHireDay()
{
	return (Date) hireDay.clone();
}
```

#### 1.3.3.3. 访问权限
```
class Employee
{
	public boolean equals(Employee other)
	{
		return name.equals(other.name);
	}
}

if(harry.equals(boss)) ...
```
此段代码不仅访问了harry.name还访问了boss.name，而name是private的。原因是boss和harry都是Employee对象，Employee类的方法可以访问Employee类的任意一个对象的私有域。

#### 1.3.3.4. final 实例域
```
private final StringBuilder evaluations;
evaluations = new StringBuilder();
```
final关键字只是表示存储在evaluation变量中的对象引用不会再指示其他StringBuilder，不过这个对象可以更改。
```
evaluations.append("abcd");
```

### 1.3.4. 静态域与静态方法
#### 1.3.4.1. 静态域
` private static int nextId=1;`</br>
这个静态域nextId属于类，所有类共享

#### 1.3.4.2. 静态方法
不需要实例化就能用的比如`Math.pow(a,b)`</br>
静态方法使没有this参数的方法

#### 1.3.4.3. 工厂方法

```
NumberFormat currencyFormatter = NumberFormat.getCurrencyInstance();
NumberFormat parcentFormatter = NumberFormat.getParcentInstance();
double x = 0.1
System.out.println(currencyFormatter.format(x)); // $0.10
System.out.println(percentFormatter.format(x)); // 10%
```

为什么不用构造器完成这个操作？

- 无法命名构造器，构造器必须与类名一样，但是这里希望不同格式不同名字
- 当使用构造器时，无法改变所构造的对象类型。而此处返回的是NumberFormat的子类

#### 1.3.4.4. main方法
main方法使静态方法，因为在启动程序时还没有任何一个对象。

### 1.3.5. 方法参数
Java语言总是采用按值调用。也就是说，方法得到的是所有参数值的一个拷贝，特别是方法不能修改传递给它的任何参数变量的内容。

```
double precent = 10;
harry.raiseSalary(precent);

public static void tripleValue(double x) // 不work这里的x是拷贝的结果
{
	x = x * 3;
}
```
一个方法不可能修改一个基本数据类型的参数，但是当时对象就不同了。这里是一个对象的引用。

```
public static void tripleValue(Employee x) //works
{
	x.raiseSalary(200);
}
```
x和harry引用了同一个对象。

- 一个方法不能修改一个基本数据类型的参数
- 一个方法可以改变一个对象参数的状态
- 一个方法不能让对象参数引用一个新的对象

### 1.3.6. 对象构造
#### 1.3.6.1. 重载
多个方法有相同的名字、不同的参数

#### 1.3.6.2. 无参数的构造器
如果类中提供了至少一个构造器，但是没有提供无参数的构造器，是不合法的

#### 1.3.6.3. 显式域初始化
确保不管怎么调用构造器，每个实例域都可以被设置为一个有意义的初值。</br>
在声明是就赋予一个值。

#### 1.3.6.4. 参数命名
通常在参数前加一个"a"
```
// 方法1
public Employee(String aName, double aSalary)
{
	name = aName;
	salary = aSalary;
}

// 方法2
public Employee(String name, double salary)
{
	this.name = name;
	this.salary = salary;
}
```

#### 1.3.6.5. 调用另一个构造器
```
public Employee(double s)
{ 
	// 调用Employee(String, double)
	this("Employee #" + nextId, s);
	nextId++;
}
```

#### 1.3.6.6. 初始化块
```
class Employee
{
	private static int nextId;
	private int id;
	
	// 初始化块
	{
		id = nextId;
		nextId++;
	}
	
	public Employee(...)
	{
		...
	}
}
```
首先运行初始化块，然后才会运行构造器。</br>
不常见这种方法。

#### 1.3.6.7. 对象析构与finalize方法
Java有自动的垃圾回收器，不需要人工回收内存，所以Java不支持析构器。</br>
但当某些对象使用了内存之外的资源时，例如文件或使用了系统资源的另一个对象的句柄。</br>
可以为任何一个类添加finalize方法，finalize方法将在垃圾回收器清除对象之前调用。不要依赖于使用finalize方法回收任何短缺的资源，这是因为很难知道这个方法什么时候才能调用。</br>

### 1.3.7. 包
java允许使用包将类组织起来，不同包内同名的类不起冲突。

当多个包内同类名起冲突时需要指定。

```
import java.util.*;
import java.sql.*;
// util与sql都有Date类，需要指定
import java.util.*;
// 如果两个类都要用则
java.util.Date deadline = new java.util.Date();
java.sql.Date today = new java.sql.Date(..);
```

#### 1.3.7.1. 静态导入
```
import static java.lang.System.*;
out.println(...);
```

#### 1.3.7.2. 将类放入包中
需要在源文件开头，包中定义类的代码之前。

```
package com.horstmann.corejava;
public class Employee
{
	...
}

```
如果没有package语句，默认放置在一个默认包，这个包没有名字。</br>
com.horstmann.corejava包中的所有源文件应该被放置在子目���com/中horstmann/corejava

- . 基目录
	- PackageTest.java
	- PackageTest.class
		- com/
			- corejava/
				- Employee.java
				- Employee.class

#### 1.3.7.3. 包作用域
 同一个包中的类可以看到包中其他public的类

### 1.3.8. 类路径
告诉编译器去哪里找类</br>
把类放在/home/user/classdir下，将jar文件放在/home/user/archives</br>
则`java -cp /home/user/classdir:.:/home/user/archive.jar MyProg`

### 1.3.9. 注释
#### 1.3.9.1. 类注释
必须在import语句之后，类定义之前。

```
import 
/**
 ...
 ...
*/

public class Card
{
}

```

#### 1.3.9.2. 方法注释
- `@param` 变量描述
- `@return` 返回值描述
- `@throws` 可能抛出异常

#### 1.3.9.3. 包与概述注释
两个选择
- 提供一个以package.html的HTML文件，在标记`<body>...</body>`之间所有文本都会被抽取出来
- 提供一个以package-info.java命名的java文件。


### 1.3.10. 类的设计技巧
1. 一定要保证数据私有
2. 一定要对数据初始化
3. 不要再类中使用过多的基本类型
	- 用一个address的新类替换(street,city,state,zip)
4. 不是所有的域都需要独立的域访问器和域更改器
5. 将职责过多的类进行分解
6. 类名和方法名能够体现它们的职责
7. 优先使用不可变的类
	- LocalDate和java.time包中的其他类是不可变的

## 1.4. 继承
### 1.4.1. 定义子类
```
public class Manager extends Employee
{
}
```
java中通常用的术语是超类和子类

### 1.4.2. 覆盖
```
public class Manager extends Employee
{
	public double getSalary()
	{
		return salary + bonus; // 不work
		// Manager类不能直接访问超类的私有域
	}
	
	public double getSalary()
	{
		double baseSalary = getSalary(); // 还是不work
		return baseSalary + bonus;
		// Manager类自己也有getSalary方法，所以只会循环调用自己
	}
	
	public double getSalary()
	{
		double baseSalary = super.getSalary(); // work 有效
		return baseSalary + bonus;
	}
}
```

#### 1.4.2.1. 子类构造器
```
public Manager(String name, double salary, int year, int month, int day)
{
	super(name, salary, year, month, day);
	bonus = 0;
}
```
这里的关键字super意思是调用超类Employee中含有n,s,year,month,day参数的构造器</br>
由于Manager类的构造器不能访问Employee类的私有域，必须通过super实现对超类构造器的调用。</br>
使用super调用构造器的语句必须是子类构造器的第一条语句。

#### 1.4.2.2. 多态
**java中，对象变量是多态的，一个Employee变量既可以引用一个Employee对象，也可以引用一个Employee类的任何一个子类的对象。**

```
Employee e;
e = new Employee(...);
e = new Manager(...);
```

***重要***

```
Manager boss = new Manager(...);
Employee[] staff = new Employee[3];
staff[0] = boss;

boss.setBonus(500); //ok
staff[0].setBonus(500); //Error 因为staff[0]的声明类型是Employee

Manager m = staff[i] //Error 不能将一个超类的引用赋给子类变量
```

**警告**</br>
子类数组的引用可以转换成超类数组的引用，但是会引起异常

```
Manager[] managers = new Manager[10];
Employee[] staff = managers; // 合法

staff[0] = new Employee("abc", ... );
// staff[0]与manager[0]引用同一个对象
// 一个普通职员会擅自归入经历行列中。

manager[0].setBonus(1000); //Error 调用了一个不存在的实例域 异常

```

#### 1.4.2.3. 理解方法调用
假设`x.f(args)`，x是C类的一个对象</br>
> 调用过程的详细描述：</br>
> 1. 编辑器查看对象的声明类型和方法名。一一列举C类中名为f的方法和其超类中访问属性为public且名为f的方法。</br>
> 2. 查看调用方法时提供的参数类型。该过程称为重载解析。</br>
> 3. 如果是private方法、static方法、final方法或者构造器，编译器将可以准确地知道应该调用哪个方法，这种方法称为静态绑定。</br>
> 4.当程序运行，并且采用动态绑定调用方法时，虚拟机一定调用与x所引用对象的实际类型最合适的那个累。**假设x的实际类型是D，它是C类的子类。如果D类定义了f(String)，就直接调用它，否在去C类中找f(String)。**

#### 1.4.2.4. 阻止继承：final类和方法
阻止利用某个类定义子类，不允许扩展的类被称为final类。

```
public final class Executive extends Manager
{
}
```

类中的特定方法也可以被声明为final。如果这样做，子类就不能覆盖这个方法了，确保它们在子类中不会改变语义。

```
public class Employee
{
	public final String getName()
	{
		return name;
	}
}
```

#### 1.4.2.5. 类型转换
- **从子类到父类的类型转换可以自动进行**
- 从父类到子类的转换必须通过强制类型转换实现
- 无继承关系的引用类型间的转换是非法的

`instanceof` 可以查看是对象属于哪一类</br>


#### 1.4.2.6. 抽象类
> 用处：比如Person类是超类，Employee和Student是子类。想生成一个描述"我叫***，我是学生"。而Person类中只有名字，因此getDescription()返回不了满意的值。可以将其设为抽象方法。

有一个抽象方法的类本身必须被声明为抽象类。抽象类可以有非抽象方法。抽象类不能实例化。

```
public abstract class Person
{
	private String name;
	public Person(String name)
	{
		this.name = name;
	}
	public abstract String getDescription();
}
```
下面这个程序，可能会有困惑。Person没有定义getDescription，为什么可以这么用。由于不能构造抽象类的对象，所以变量p永远不会引用Person，而是引用其具体的子类。

```
for (Person p : people)
	System.out.println(p.getDescription());
```

#### 1.4.2.7. 受保护访问
`protected` 可以让子类访问父类的变量
- `private` 仅对本类可见
- `public` 对所有类可见
- `protected` 对本包和所有子类可见

### 1.4.3. Object: 所有类的超类
可以使用Object类型的变量引用任何类型的对象：</br>
`Object obj = new Employee("...");`</br>
**但是，Object类型只能作为各种值的通用持有者，要想对其中的内容进行具体的操作，还需要清楚对象的原始类型**</br>
`Employee e = (Employee) obj;`</br>
不管是对象数组还是基本类型的数组都扩展了Object类。

#### 1.4.3.1. equals方法
在Objects类中，equals被用于判断两个对象是否具有相同的引用。</br>
一般情况下效果等同于==。但是当String,File,Date,其他包装类时，用于比较内容。

```
Person p1 = new Person();
Person p2 = new Person();
System.out.println(p1 == p2); // false 比较的是对象地址
System.out.println(p1.equals(p2)); //false


String s1 = "abc";
String s2 = "abc";
System.out.println(s1 == s2); //false
System.out.println(s1.equals(s2)); //true
```

对于大多数情况，这个equals没有意义，所以需要重写他。

```
// 最完美的equals重写定义
public class Employee
{
	public boolean equals(Object otherObject)
	{
		if(this == otherObject) return true;
		
		if(otherObject == null) return false;
		
		if(getClass() != otherObject.getClass()) return false;
		
		if(!(otherObject instanceof Employee)) return false;
		
		Employee other = (Employee) otherObject;
		
		return name.equals(other.name) 
			&& salary == other.salary
			&& hireDay.equals(other.hireDay);
			// 这里hireDay.equals(other.hireDay)需要更改
			// 为了防止null的情况。
		
		return Objects.euqlas(name, other.name)
			&& salary == other.salary
			&& Object.equals(hireDay, other.hireDay);
	}
}

// 子类需要先调用超类的equals
public class Manager extends Employee
{
	public boolean equals(Object otherObject)
	{
		if(!super.equals(otherObject)) return false;
		...
	}
}
```

可以显示的使用`@Override`标记覆盖，防止参数错误。

#### 1.4.3.2. HashCode方法
最好的方法

```
public int hashCode()
{
	return Objects.hash(name, salary, hireDay); //Objects.hash是null安全的
}


```

#### 1.4.3.3. toString方法
```
public String toString()
{
	return getClass().getName()
		+ "[name=" + name
		+ ....
		+ "]";
}
```

### 1.4.4. 泛型数组列表
java允许在运行时确定数组的大小。

```
int size = ...;
Employee[] staff = new EMployee[size];
```
但是这个方法也还未解决动态数组。提出了ArrayList的类，可以添加或删除元素，具有自动调节数组容量的功能。</br>
`ArrayList<Employee> staff = new ArrayList<>();`
`staff.add(new Employee());`

如果能够估计出数组可能存储的元素数量，可以调用`staff.ensureCapacity(100);`或者`ArrayList<Employee> staff = new ArrayList<>(100);`</br>
后者只是拥有100个元素的潜力，在最初可能根本没有这么多，这是与直接分配数组的区别。

使用.size()代替length计算元素数目
#### 1.4.4.1. 访问列表元素
`staff.set(i, harry);`
`staff.get(i);`

### 1.4.5. 对象包装器与自动装箱
Integer类对应基本类型int。这些类称为包装器，还包括：Integer,Long,Float,Double,Short,Byte,Character,Void,Boolean（前6个类派生于公共的超类Number）。对象包装器类是不可变的，还是final。</br>
`ArrayList<Integer> list = new ArrayList<>();`</br>
当调用`list.add(3);`时会自动的变成`list.add(Integer.valueOf(3));` 这种变换被称作自动装箱。</br>
`int n = list.get(i);`被翻译成`int n = list.get(i).intValue();`自动拆箱</br>

如果想修改数值参数值，需要在org.omg.CORBA包中定义的持有者类型。每个持有者类型都包含一个公有域值。
```
public static void triple(int x)
{
	x = x * 3; 不work
}
public static void triple(IntHolder x)
{
	x.value = 3 * x.value; // work
}
```

**主要的应用**

```
int i = Integer.parseInt("123");
float f = Float.parseFloat("0.40");
boolean b = Boolean.parseBoolean("false");

String istr = String.valueOf(i);
String fstr = String.valueOf(f);
String bstr = String.valueOf(true);
```

### 1.4.6. 参数数量可变的方法
System.out.printf可以输入多参数。其定义如下：

```
public class PrintScreen
{
	public PrintScreen printf(String fmt, Object... args)
	{
		return format(fmt, args);
	}
}
```
这里的...表示接受任意数量的对象，Object...参数类型与Object[]完全一样。

### 1.4.7. 枚举类
```
public static void main(String[] args)
{
	Size s = Size.SMALL;
	s.getAbbreviation(); //返回"S"
}


public enum Size
{
	SMALL("S"), MEDIUM("M"), LARGE("L");
	
	private String abbreviation;
	private Size(String abbreviation) {this.abbreviation = abbreviation;}
	public String getAbbreviation() {return abbreviation;}
}
```

### 1.4.8. 反射
反射机制可以用来：

- 运行时分析类的能力
- 在运行时查看对象，例如，编写一个toString方法供所有类使用
- 实现通用的数组操作代码
- 利用Method对象，这个对象很像c++中的函数指针

反射的主要使用人员是工具构造者。

#### 1.4.8.1. class类
java运行时系统始终为所有的对象维护一个被称为运行时的类型标记。</br>
Object类中的getClass()方法将会返回一个Class类型的实例。</br>

```
Employee e;
Class cl = e.getClass();
```

如果类在包里，包的名字也会作为类名的一部分 "java.util.Random"</br>
可以通过类名获得对应的Class对象：`Class cl = Class.forName(className);`

`e.getClass().newInstance();`创建一个类的实例

#### 1.4.8.2. 捕获异常
```
// 如果类名不存在
try
{
	String name = ...;
	Class cl = Class.forName(name);
}
catch(Exception e)
{
	e.printStackTrace();
}

```

#### 1.4.8.3. 利用反射分析类的能力
反射相关的API，类内详细方法见p197:

- java.lang.Class 类
- java.lang.reflect.Method 类的方法
- java.lang.reflect.Field 成员变量
- java.lang.reflect.Constructor 构造方法

#### 1.4.8.4. 在运行时使用反射分析对象
```
Employee harry = new Employee(...);
Class cl = harry.getClass();
Field f = cl.getDeclareField("name");
Object v = f.get(harry); //出问题，由于name是私有域，不能获得其值

f.setAccessible(true); //设置过这句话后，可以访问私有的
// 如果要返回的是基本类型，不是对象则不能用get
Field f = cl.getDeclareField("salary");
double salary = f.getDouble(harry);
```

#### 1.4.8.5. 使用反射编写泛型数组代码
要编写通用数组，用`Obecjt[] new Array`是不可以的，因为java数组会记住每个元素在创建时的类型。父可以转子，子不能转父。因此需要java.lang.reflect包中Array类的一些方法。

`Object newArray = Array.newInstance(componentType, newLength)`;

扩展任意类型的数组的方法：

```
public static Object goodCopyOf(Object a, int newLength)
{
	Class cl = a.getClass();
	if(!cl.isArray()) return null;
	Class componentType = cl.getComponentType();
	int length = Array.getLength(a);
	Object newArray = Array.newInstance(componentType, newLength);
	System.arraycopy(a, 0, newArray, 0, Math.min(length,newLength)); //原数组，起始位置，目标数组，起始位置，复制的长度。
	return newArray;
}
```

#### 1.4.8.6. 调用任意方法
方法.invoke方法同类.get方法
`Method getMethod(String name, Class... parameterTypes)`

比如获取Employee中名为raiseSalary的，且参数为double的方法</br>
`Method m = Employee.class.getMethod("raiseSalary",double.class);`

举例：

```
public static void main(String[] args)
{
	Method sqrt = Math.class.getMethod("sqrt", double.class);
	printTable(1, 10, 10, square);
}

public static void printTable(double from, double to, int n, Method f)
{
	double dx = (to - from) / (n - 1);
	
	for(double x = from; x <= to; x += dx)
	{
		double y = (Double) f.invoke(null, x); //由于刁颖的是一个静态方法sqrt，所以第一个参数为null
		System.out.println(y);
	}
}
```

### 1.4.9. 继承的设计技巧
1. 将公共操作和域放在超类
2. 不要使用受保护的域
3. 使用继承实现is-a关系
4. 除非所有继承的方法都有意义，否则不要使用继承
5. 在覆盖方法时，不要改变预期的行为
6. 使用多态，而非类型信息
7. 不要过多地使用反射

### 1.4.10. 子类实例化父类对象

```java
public class Father {
    public void doing (){
        talking();
    }
    public void talking(){
        System.out.println("father is watching TV!");
    }
}
public class Son extends Father {
    public static void main(String[] args) {
        Father father = new Father();
        father.doing();
        Son son = new Son();
        son.doing();
    }
    @Override
    public void doing(){
        talking();
    }
    @Override
    public void talking(){
        System.out.println("Son is watching TV!");
    }
}

// 输出的结果
father is watching TV!
Son is watching TV!
```

```java
public class Son extends Father {
    public static void main(String[] args) {
        Father father = new Son();
        father.doing();
        Son son = new Son();
        son.doing();
    }
    @Override
    public void doing(){
        talking();
    }
    @Override
    public void talking(){
        System.out.println("Son is watching TV!");
    }
}

// 输出结果
Son is watching TV!
Son is watching TV!
```

因此可见：
1. 子类中，父类实例化父类对象，调用父类中的方法。
2. 子类中，子类实例化父类对象，调用子类中的方法。
3. 子类中，子类实例化子类对象，调用子类中的方法。

## 1.5. 接口、lambda表达式与内部类
### 1.5.1. 接口
接口是对类的一组需求描述，接口可以实现多重继承的效果。</br>

`class Employee implements Comparable`</br>
先写继承后写接口

#### 1.5.1.1. 接口和抽象类的区别
本质：从设计层面来说，抽象是对类的抽象，是一种模板设计，接口是行为的抽象，是一种行为的规范

区别：
1.接口的方法默认是public，所有方法在接口中不能有实现，抽象类可以有非抽象的方法
2.接口中的实例变量默认是final类型的，而抽象类中则不一定
3.一个类可以实现多个接口，但最多只能实现一个抽象类
4.一个类实现接口的话要实现接口的所有方法，而抽象类不一定
5.接口不能用new实例化，但可以声明，但是必须引用一个实现该接口的对象

#### 1.5.1.2. 特性
接口不能用new实例化，但是却可以声明接口变量</br>
接口变量必须引用实现了接口的类对象
```
Comparable x;
x = new Employee(..);
但是此时只能调用Comparable接口内的函数。
```

接口也可以被接口继承。</br>
接口可以包含常量。会自动被设为public static final</br>
可以在实现方法中直接使用这些常量。</br>
接口的类必须实现接口的所有方法，否则只能成为抽象类

#### 1.5.1.3. 接口与抽象类
**接口类似于抽象类，用其他类来实现接口中的函数。但是为什么不能直接用一个抽象类，其他子类继承他呢。因为接口中的函数可以经常发生变化，但是超类要尽可能的保持稳定。另外一个类可以实现多个接口，但是一个类只能继承一个超类。**

接口可以实现静态方法 

#### 1.5.1.4. 总结
抽象类是对于一类事物的高度抽象，其中既有属性也有方法</br>
接口是对方法的抽象，也就对一系列动作的抽象</br>
当需要对一类事物抽象的时候，应该是使用抽象类，好形成一个超类</br>
当需要对一系列的动作抽象，就使用接口，需要使用这些动作的类去实现相应的接口即可

### 1.5.2. 单例设计模式
实际编程过程中，逐渐总结出的一些解决问题的思考方式。</br>
在整个软件系统运行过程中，这个类只被实例化一次，以后无论在哪都只调用这个实例。</br>
例如实例化是需要大量的资源时，会使用单例设计模式。

#### 1.5.2.1. 饿汉式单例模式
```
public class Single
{
	// 私有的构造，会导致这个类无法用new来创建对象，只能在类内调用new
	private Single()
	{}
	
	private static Single single = new Single();
	
	public static Single getInstance()
	{
		return single;
	}
}

Single s = Single.getInstance();
Single s1 = Single.getInstance();
Single s2 = Single.getInstance();

```
无论这个调用几次，返回的都是那个static的single，只有那一个。所以只用了一次new。s,s1,s2都是指向同一个东西。

#### 1.5.2.2. 懒汉式单例模式
**暂时有系统安全问题 跟多线程有关**
最开始无new对象，直到有第一个人要调用时，才new一个对象，之后所有都用这个对象。

```java
public class Single
{
	// 私有的构造，会导致这个类无法用new来创建对象，只能在类内调用new
	private Single()
	{}
	
	private static Single single = null;
	
	public static Single getInstance()
	{
		if(single == null)
		{
			single = new Single();
		}
		return single;
	}
}

Single s = Single.getInstance(); // 第一次去new
Single s1 = Single.getInstance(); // 直接返回new好的single
Single s2 = Single.getInstance();
```
问题：多线程在run()方法中调用getInstance()，当多线程可能同时判断为null，创建多个对象。此时的对象相当于共享对象

**使用同步机制将单例模式改成线程安全的**
```java
public class Single
{
	// 私有的构造，会导致这个类无法用new来创建对象，只能在类内调用new
	private Single()
	{}
	
	private static Single single = null;
	
	public static Single getInstance()
	{
        // 方法一:效率差
        synchronized(Single.class){
            if(single == null)
            {
                single = new Single();
            }
            return single;
        }   

        // 方法二：效率高
        if(single == null){
            synchronized(Single.class){
                if(single == null)
                {
                    single = new Single();
                }
            } 
        }
        return single;
	}
}

Single s = Single.getInstance(); // 第一次去new
Single s1 = Single.getInstance(); // 直接返回new好的single
Single s2 = Single.getInstance();
```

### 1.5.3. lambda表达式
lambda表达式是一个可传递的代码块。

```java
(String first, String second) ->
	{
		if(first.length() < second.length()) return -1;
		else if (first.length() > second.length()) reutn 1;
		else return 0;
	}
```

如果没有参数也需要括号()->

#### 1.5.3.1. 函数式接口
只有一个抽象方法的接口叫做函数式接口</br>

```
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
 
public class Java8Tester {
   public static void main(String args[]){
      List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);
        
      // Predicate<Integer> predicate = n -> true
      // n 是一个参数传递到 Predicate 接口的 test 方法
      // n 如果存在则 test 方法返回 true
        
      System.out.println("输出所有数据:");
        
      // 传递参数 n
      eval(list, n->true);
        
      // Predicate<Integer> predicate1 = n -> n%2 == 0
      // n 是一个参数传递到 Predicate 接口的 test 方法
      // 如果 n%2 为 0 test 方法返回 true
        
      System.out.println("输出所有偶数:");
      eval(list, n-> n%2 == 0 );
        
      // Predicate<Integer> predicate2 = n -> n > 3
      // n 是一个参数传递到 Predicate 接口的 test 方法
      // 如果 n 大于 3 test 方法返回 true
        
      System.out.println("输出大于 3 的所有数字:");
      eval(list, n-> n > 3 );
   }
    
   public static void eval(List<Integer> list, Predicate<Integer> predicate) {
      for(Integer n: list) {
        
         if(predicate.test(n)) {
            System.out.println(n + " ");
         }
      }
   }
}
```
java.function包中有很多函数式接口。

#### 1.5.3.2. 方法引用
```
Timer t = new Timer(1000, event -> System.out.println(event));

Timer t = new Timer(1000, System.out::println);
```
用冒号分割对象或者类名和方法

#### 1.5.3.3. 构造器引用
类似方法引用

### 1.5.4. 内部类
内部类是定义在另一个类中的类。

- 内部类方法可以访问该类定义所在的作用域中的数据，包括私有的数据
- 内部类可以对统一包中的其他类隐藏起来
- 当想要定义一个回调函数且不想编写大量代码时，使用匿名内部类比较便捷。

#### 1.5.4.1. 使用内部类访问对象状态
自动生成一个外围类对象的引用成为outer

#### 1.5.4.2. 内部类的特殊语法
外围类引用的正规语法：OuterClass.this.变量</br>
在外围类的作用域之外，可以这样引用内部类：
`OuterClass.InnerClass`

### 1.5.5. 代理
运用代理可以在运行时创建一个实现了一组给定接口的新类.只有在编译时无法确定需要实现哪个接口时才有必要使用。

三个要素：接口，真实对象（继承接口，提供服务），代理（继承接口，要想用真实对象的服务，就要通过代理）

把一个真实对象放入一个代理类中，让代理类帮助实现。

```
1、定义抽象角色
/**
 * @author Gjing
 * 定义一个产家,提供卖货的功能
 **/
public interface Producer {
    void sell();
}
2、定义一个真实角色
/**
 * @author Gjing
 * 定义一个小卖部,帮产家卖货
 **/
public class Canteen implements Producer {
    @Override
    public void sell() {
        System.out.println("小卖部进行卖货");
    }
}
3、定义一个代理类和测试类

/**
 * @author Gjing
 * 定义产家的代理商,也具备卖货的功能
 **/
public class ProducerProxy implements Producer {
    private Producer producer;

    ProducerProxy(Producer producer) {
        this.producer = producer;
    }

    @Override
    public void sell() {
        System.out.println("--------小卖部卖货前--------");
        producer.sell();
        System.out.println("--------小卖部卖货后--------");
    }
}

/**
 * @author Gjing
 */
class StaticProxyTest {
    public static void main(String[] args) {
        Producer producer = new Canteen();
        ProducerProxy personProxy = new ProducerProxy(producer);
        personProxy.sell();
    }
}

```

#### 1.5.5.1. 动态代理
`InvocationHandler` 增强</br>
`Proxy` 调度器 负责动态

```
// 接口
public interface Girl
{
	void date();
	void watchMovie();
}

// 真实对象
public class person implements Girl
{
	@Override
	public void date()
	{
		System.out.println("date");
	}
	
	@Override
	public void watchMovie()
	{
		System.out.println("Movie");
	}
}

// 动态代理
public class personProxy implements InvocationHandler
{
	private Girl girl; // 必须声明真实对象，以提供服务
	
	@Override
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable
	{
		doSomethingBefore(); //前置增强
		Object ret = method.invoke(girl, args); //用到了反射的知识
		doSomethingAfter(); //后置增强
		return ret;
	}
	// 这个是InvocationalHandler中的方法
	
	private void doSomethingBefore()
	{
		System.out.println("约会前 增强1");
	}
	
	private void doSomethingBefore()
	{
		System.out.println("约会后 增强2");
	}
	
	public Object getProxyInstance()
	{
		return Proxy.newProxyInstance(girl.getClass().getClassLoader(), girl.getClass().getInterfaces(), this);
		// 类加载器，指定接口，和InvocationHandler
	}
}

//主程序
public static void main(String[] args)
{
	// 有个女孩，想跟他约会
	Girl girl = new Person();
	// 不能直接照这个女孩，要先通过他的家人的同一。生成这个女孩的代理
	personProxy family = new personProxy(girl);
	// 从这个代理中，获取一个实例。这家人中他的妈妈同意了。
	Girl mother = (Girl) family.getProxyInstance();
	// 经过这个代理，才能约会
	mother.date();
}

输出：
约会前 增强1
date
约会后 增强2
```

## 1.6. 异常、断言和日志
### 1.6.1. 处理错误
关注一下错误：

- 用户输入错误
- 设备错误
- 物理限制 磁盘满了，可用存储空间已被用完
- 代码错误

#### 1.6.1.1. 异常分类
异常对象都是派生于Throwable类的一个实例。

- Throwable
	- Error
	- Exception
		- IOException
		- Runtime Exception

Error类层次结构描述了Java运行时系统的内部错误和资源耗尽错误。如果出现了这样的内部错误，除了通告给用户，并尽力使程序安全地终止以外，再也无能为力。

Exception层次结构中，由程序错误导致的异常RuntimeException和程序本身没问题的由于I/O错误这类问题导致的异常属于其他异常。

#### 1.6.1.2. 声明受查异常
`public FileInputStream(String name) throws FileNotFoundException`
表明有可能会抛出FileNotFoundException异常

需要抛出异常的情况：

- 调用一个抛出受查异常的方法 列入上述构造器
- 程序运行过程中发现错误，并且利用throw语句抛出一个受查异常
- 程序出现错误
- Java虚拟机和运行时库出现的内部错误

如果出现前两种情况，必须告诉调用这个方法的程序员有可能抛出异常。如果没有处理器捕获这个异常，当前执行的线程就会结束。

#### 1.6.1.3. 抛出异常
假设是在文件读取过程中的异常，则EOFException符合

```
String readData(Scanner in) throws EOFException
{
	while(...)
	{
		if(!is.hasNext()) //EOF encoutered
		{
			if(n<len)
				throw new EOFException();
		}
	}
}
```

也可以EXFException(String ..);

#### 1.6.1.4. 创建异常类
需要继承Exception类或其子类

```
class FileFOrmatException extends(IOException)
{
	public FileFormatException(){}
	public FileFormatException(String gripe)
	{
		super(gripe);
	}
}
```

#### 1.6.1.5. 捕获异常
如果某个异常发生的时候没有再任何地方进行捕获，那程序就会终止执行，并在控制台上打印出异常信息。

```
try
{
	...
}
// 同时捕获多个异常
catch (ExceptionType e | OtherExceptionType oe)
{
	....
}
```

#### 1.6.1.6. finally子句
当发生异常时，会终止方法剩余代码的处理，并退出这个方法的执行。如果方法获得了一些本地资源，并且只有这个方法自己知道，如果这些资源在退出前必须被回收，那么就会产生资源回收问题。</br>
不管是否有异常被捕获，finally子句的代码都被执行。

```
InputStream in = new FileInputStream(...);
try
{
	...
}
catch (IOException e_
{
	...
}
finally
{
	in.close();
}
```

#### 1.6.1.7. 带资源的try
```
try(Scanner in = new Scanner(new FileInputStream(".."), "UTF-8);
PrintWriter out = new PrintWriter("..")) //指定多个资源
{
	while(in.hasNext())
		System.out.println(in.next());
}
```
当try块退出时，会自动调用res.close()

#### 1.6.1.8. 分析堆栈轨迹元素
```
Throwable t = new Throwable();
StringWriter out = new StringWriter();
t.printStackTrace(new PrintWriter(out));
String description = out.toString();
```

```
Throwable t = new Throwable();
StackTraceElement[] frames = t.getStackTrace();
for(StackTraceElement frame:frames)
	....
```

### 1.6.2. 使用异常机制的技巧
- 异常处理不能代替简单测试（捕获异常事件远大于一次判断事件）
- 不要过分的细化异常
- 利用异常层次结构
- 不要压制异常
- 在检测错误时，苛刻要比放任更好
- 不要羞于传递异常

### 1.6.3. 使用断言
`assert 条件;`和`assert 条件:表达式;`</br>
如果不满足条件，则会抛出一个AssertionError，在第二个中会生成表达式的字符串。


#### 1.6.3.1. 启用和禁用断言
在默认情况下，断言被禁用</br>
可以`java -ea ..`打开禁言

#### 1.6.3.2. 使用断言进行参数检查
java有3中处理系统错误的机制：
- 抛出一个异常
- 日志
- 使用断言

断言失败是致命的，不可恢复的错误</br>
断言检查只用于开发和测试段

### 1.6.4. 记录日志
#### 1.6.4.1. 基本日志
`Logger.getGlobal().info("File->Open menu item selected");`
这条记录将会显示一下内容：
> May 10, 2013 10:12:15 PM LoggingImageViewer fileOpen
> INFO:File->Open menu item selected

在合适的地方调用Logger.getGlobal().setLevel(Level.OFF)将会取消所有日志

#### 1.6.4.2. 高级日志
不要将所有的日志都记录到一个全局日志记录器中，而是可以自定义日志记录器。
`private static final Logger myLongger = Longger.getLongger("com.mycompany.myapp");`

与包名类似，日志记录器也具有层次结构。

有7个日志记录器级别：

- SERVER
- WARNING
- INFO
- CONFIG
- FINE
- FINER
- FINEST

默认情况下，只记录前3个级别，可以设置如logger.setLevel(Level.FINE)

对于所有级别记录方式:`longger.warning(message);`</br>
指定级别:`logger.log(Level.FINE, message)`

#### 1.6.4.3. 修改日志管理器配置
默认情况下，配置文件存在于：jre/lib/logging.properties

#### 1.6.4.4. 本地化 过滤器 处理器 格式化器
暂时未学

### 1.6.5. 调用技巧
1. 输出变量检查
2. 每个类放置一个main用于测试
3. 日志代理
4. 利用Throwable类提供的printStackTrace方法，可以从任何一个异常对象中获得堆栈情况

## 1.7. 泛型程序设计
泛型程序设计意味着编写的代码可以被很多不同类型的对象所重用。</br>
ArrayList类维护一个Object引用的数组。需要指明这个Object，否则容易出错。

### 1.7.1. 定义简单泛型类
```
public class Pair<T>
{
	private T first;
	private T second;
	
	public Pair(){first = null; second = null;}
	public Pair(T first, T second){this.first = first; this.second = second;}
	
	public T getFirst(){return first;}
	public T getSecond(){return second;}
	
	public void setFirst(T newValue){first = newValue;}
	public void setSecond(T newValue){second = newValue;}
}
```

### 1.7.2. 泛型方法
```
class ArrayAlg
{
	public static <T>T getMiddle(T... a)
	{
		return a[a.length/2];
	}
}

//调用
String middle = ArrayAlg.<String>getMiddle("...","...");
//或
String middle = ArrayAlg.getMiddle("...","...");
```

### 1.7.3. 类型变量的限定
有时候类或方法需要对类型加以约束，这样可以实现：
`public static <T extends Comparable> T min(T[] a)...`

现在这个min方法只能被实现了Comparable接口的类的数组调用。

### 1.7.4. 泛型代码和虚拟机
#### 1.7.4.1. 类型擦除
定义一个泛型类型，都自动提供一个相应的原始类型，原始类型的名字就是删去类型参数后的泛型类型名

#### 1.7.4.2. 翻译泛型表达式 翻译泛型方法 调用遗留代码
未学

### 1.7.5. 约束与局限性
#### 1.7.5.1. 不能用基本类型实例化类型参数
没有Pair<double>，只有Pair<Double>，其原因是类型擦除，擦除后只有Object，而Object不能存储double值

#### 1.7.5.2. 运行时类型查询只适用于原始类型
**注意**

```
Pair<String> stringPair = ...;
Pair<Employee> employee = ...;
if(stringPair.getClass() == employeePair.getClass()) // 相等
```

#### 1.7.5.3. 不能创建参数化类型的数组
`Pair<String>[] table = new Pair<String>[10] //error`
因为参数擦除后变为Object;而数组会记住原始的类型即String。

#### 1.7.5.4. Varargs警告
```
public static <T> void addAll(Collection<T> coll, T... ts)
{
	for(t:ts) coll.add(t);
}

Collection<Pair<String>> table = ...;
Pair<String> pair1 = ...;
Pair<String> pair2 = ...;
addAll(table, pair1, pair2);
```
为了调用函数，java需要建立一个Pair<String>数组，这与之前的规则相违反。但是只会给一个Varargs警告。</br>
在java SE 7中科院直接用@SafrVarargs标注他

#### 1.7.5.5. 不能实例化类型变量
不用能New T(...),new T[..], T.class

#### 1.7.5.6. 不能构造泛型数组
#### 1.7.5.7. 不能抛出或捕获泛型类的实例
#### 1.7.5.8. 可以消除对受查异常的检查
#### 1.7.5.9. 注意擦除后的冲突

### 1.7.6. 泛型类型的继承规则
无论S与T有什么联系，通常，`Pair<S>`与`Pair<T>`没有什么联系。

### 1.7.7. 通配符类型
`Pair<? extends Employee>` 表明它的类型是Employee的子类，如`Pair<Manager>`

```
// 如前面所说的，这个类的输入不能是Pair<Manager> 他们并不是继承关系
public static void printBuddies(Pair<Employee> p)
{}

// 为了解决这个问题可以这么操作
public static void printBuddies(Pair<? extends Employee>p)
{}
```

#### 1.7.7.1. 通配符的超类型限定
`? super Manager`

#### 1.7.7.2. 无限定通配符
`Pair<?>`有以下方法：

- `? getFirst()`
- `void setFirst(?)`

getFirst的返回值只能赋给一个Object。setFirst方法不能被调用，甚至不能用Object调用。

#### 1.7.7.3. 通配符捕获
`public static void swap(Pair<?> p)`方法要想获取一个对象
`? t = p.getFirst();` 是错误的

可以通过一个辅助方法完成。
```
public static <T> void swapHelper(Pair<T> p)
{
	T t = p.getFirst();
	p.setFirst(p.getSecond());
	p.setSecond(t);
}

public static void swap(Pair<?> p) { swapHelper(p);}
```

### 1.7.8. 反射和泛型

## 1.8. 集合
### 1.8.1. java集合框架
#### 1.8.1.1. 将集合的接口与实现分离
队列接口有两个类实现，一个是使用循环数组ArrayDeque类，另一个是链表队列，就直接使用LinkedList类。

#### 1.8.1.2. Collection接口
集合类的基本接口是Collection接口。这个接口有两个基本方法：

```
public interface Collection<E>
{
	boolean add(E element);
	Iterator<E> iterator();
	...
}
```
add用于添加元素，iterator方法用于返回一个实现了Iterator接口的对象，可以使用这个迭代器对象一次访问集合中的元素。

#### 1.8.1.3. 迭代器
Iterator接口包含4个方法：

```
public interface Iterator<E>
{
	E next();
	boolean hasNext();
	void remove();
	default void forEachRemaining(Consumer<? super E> action); 
	//JavaSE8 中的default。自己在接口中实现，不需要每个类都实现它
}
```
通过反复调用next方法，可以逐个访问集合中的元素。但是在末尾无元素了，会抛出异常，所以要在next前调用hasNext。

可以使用forEachRemaining并提供一个lambda表达式带起for循环。
`iterator.forEachRemaining(element -> do something);`

在调用remove前需要调用next</br>
it.remove();</br>
it.next();</br>
it.remove();

#### 1.8.1.4. 泛型使用方法
Collection接口声明了很多有用的方法：

- int size()
- boolean isEmpty()
- boolean contains(Object obj)
- boolean equals(Object other)
- boolean addAll(Collection<? extends E> from)
- boolean remove(Object obj)
- boolean removeAll(Collection<?> c)
- void clear()
- boolean retainAll(Collection<?> c)
- Object[] toArray()
- <T> T[] toArray(T[] arrayToFill)

#### 1.8.1.5. 集合框架中的接口

- Iterable
	- Collection
		- List
		- Set
			- SortedSet
				- NavigableSet
		- Queue
			- Deque
- Map
	- SortedMap
		- NavigableMap
- Iterator
	- ListIterator
- RandomAccess

在map中插入用`V put(K key, V value)`，取出用`V get(K key)`

### 1.8.2. 具体的集合

- ArrayList 动态增长和缩减的索引序列
- LinkedList 可以在任何位置进行高效的插入和删除的有序序列
- ArrayDeque 循环数组实现的双端队列
- HashSet	一种没有重复元素的无序集合
- TreeSet	一种有序集
- EnumSet	一种包含枚举类型值的集
- LinkedHashSet	一种可以记住元素插入次序的集
- PriorityQueue 一种允许高效删除最小元素的集合
- HashMap	一种存储键/值关系的数据结构
- TreeMap	一种键值有序排列的映射表
- EnumMap	一种键值属于枚举类型的映射表
- LinkedHashMap	一种可以记住键/值项添加次序的映射表
- WeakHashMap	一种其值无用武之地之后可以被垃圾回收器回收的映射表
- IdentityHashMap	一种用==而不是用equals比较键值的映射表

#### 1.8.2.1. 链表
先增加3个元素，然后将第2个元素删除
```
List<String> staff = new LinkedList<>();
staff.add("Amy");
staff.add("Bob");
staff.add("Carl");
Iterator iter = staff.iterator();
String first = iter.next();
String second = iter.next();
iter.remove();
```

#### 1.8.2.2. 树集
与散列集类似，但是是有序的。

#### 1.8.2.3. 队列与双端队列
Deque

#### 1.8.2.4. 优先级队列
可以按照任意的顺序插入，却总是按照排序的顺序进行检索。也就是说无论何时用remove，总是获得当前优先级队列中最小的元素。

#### 1.8.2.5. Map的四种遍历方法
```java
public static void main(String[] args) {
 
    Map<String, String> map = new HashMap<String, String>();
    map.put("1", "value1");
    map.put("2", "value2");
    map.put("3", "value3");
  
    //第一种：普遍使用，二次取值
    System.out.println("通过Map.keySet遍历key和value：");
    for (String key : map.keySet()) {
        System.out.println("key= "+ key + " and value= " + map.get(key));
    }
  
    //第二种
    System.out.println("通过Map.entrySet使用iterator遍历key和value：");
    Iterator<Map.Entry<String, String>> it = map.entrySet().iterator();
    while (it.hasNext()) {
        Map.Entry<String, String> entry = it.next();
        System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
    }
  
    //第三种：推荐，尤其是容量大时
    System.out.println("通过Map.entrySet遍历key和value");
    for (Map.Entry<String, String> entry : map.entrySet()) {
        System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
    }
 
    //第四种
    System.out.println("通过Map.values()遍历所有的value，但不能遍历key");
    for (String v : map.values()) {
        System.out.println("value= " + v);
    }
```

### 1.8.3. 映射
#### 1.8.3.1. 基本映射操作
两个通用的实现：HashMap和TreeMap。这两个类都实现了Map接口。</br>
散列映射对键进行散列，树映射用键的整体顺序对元素进行排序，并将其组织成搜索树。

```
Map<String, EMployee> staff = new HashMap<>();
Employee harry = new EMployee("Harry Hacker");
staff.put("987", harry);

String id = "987";
e = staff.get(id);
e = staff.get(id, 0); //如果这个键不存在返回0
```

```
Map<String, Integer> scores = ...;
scores.forEach((k, v) -> System.out.println("key=" + k
+ ", value=" + v));
```

#### 1.8.3.2. 映射视图

- Set<K> keySet() 返回键集
- Collection<V> values() 返回值集合
- Set<Map.Entry<K,v>> entrySet() 返回键值对

#### 1.8.3.3. 链接散列集与映射
LinkedHashSet和LinkedHashMap类用来记住插入元素项的顺序。

```
Map<String, Employee> staff = new LinkedHashMap<>();
staff.put("123", new Employee("Amy Lee"));
staff.put("456", new EMployee("Harry Hacker"));
staff.put("789", new EMployee("Gary Cooper"));

staff.keySet().iterator()
// 123
// 456
// 789

staff.values().iterator()
// Amy Lee
// Harry Hacker
// Gary Cooper
```

#### 1.8.3.4. 枚举集与映射
```
enum Weekday { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY };
EnumSet<Weekday> workday = EnumSet.range(Weekday.MONDAY, Weekday.FRIDAY);
EnumSet<Weekday> mwf = EnumSet.of(Weekday.MONDAY, Weekday.WEDNESDAY, Weekday.FRIDAY);
```

### 1.8.4. 视图与包装器
#### 1.8.4.1. 轻量级集合包装器
Array类的静态方法asList将返回一个包装了普通java数组的List包装器。这个方法可以将数组传递给一个期望得到列表或集合参数的方法。

```
Card[] cardDeck = new Card[52];
List<Card> cardList = Arrays.asList(cardDeck);
```

#### 1.8.4.2. 子范围
`List group2 = staff.subList(10,20);`取第10到19个元素

SortedSet接口声明了3个方法：

- SortedSet<E> subSet(E from, E to)
- SortedSet<E> headSet(E to)
- SortedSet<E> tailSet(E from)

有序映射SortedMap也有类似的方法。

#### 1.8.4.3. 同步视图
如果多个线程访问集合，就必须确保集不会被意外地破坏。</br>
例如，如果一个线程视图将元素添加到散列表中，另一个线程正在对散列表进行再散列，其结果将是灾难性的。</br>
Collections类的静态synchronizedMap方法 可以将任何一个映射表转换成具有同步访问方法的Map:

`Map<String, Employee> map = Collections.synchronizedMap(new HashMap<String, Employee>());`

#### 1.8.4.4. 受查视图
```
ArrayList<String> strings = new ArrayList<>();
ArrayList rawList = strings; // 不会出错，会报警
rawList.add(new Date());
```
在add时不会出错，直到调用get时，并将结果转化为String时，这个类才会抛出异常。

受查视图可以探测到这类问题，可以用一个安全列表：
`List<String> safeStrings = Collections.checkList(strings, String.class);`

### 1.8.5. 算法
```
static <T extends Comparable> T max(T[] a);
```

#### 1.8.5.1. 排序与混排
```
List<String staff = new LinkedList<>()
Collections.sort(staff);
Collections.shuffle(staff); // 打乱
```

#### 1.8.5.2. 二分查找
`i = Collections.binarySearch(c, element);`

如果集合没有采用Comparable接口的compareTo方法进行排序，就还要提供一个比较器对象。</br>
`i = COllections.binarySearch(c, element, comparator);`

#### 1.8.5.3. 简单算法
Collections提供了很多简单算法的API

#### 1.8.5.4. 批操作

- coll1.removeAll(coll2) 在coll1中删除元素coll2中的元素
- coll1.retainAll(coll2) 删除所有未在coll2中出现的元素

#### 1.8.5.5. 集合与数组的转换
```
String[] values = ...;
HashSet<String> staff = new HashSet<>(Array.asList(values));
String[] values = staff.toArray(new String[0]);
或者 staff.toArry(new String[staff.size()]);
```

#### 1.8.5.6. 编写自己的算法
将集合接口作为参数。

```
void fileMenu(JMenu menu, Collection<JMenuItem> items)
{
	for (JMenuItem item : items)
		menu.add(item);
}
```




## 1.9. 部署java应用程序
## 1.10. 并发
多进程与多线程的本质区别在于每个进程拥有自己的一整套变量，而线程则共享数据。

### 1.10.1. 什么是线程
创建单个进程

```
public interface Runnable
{
	void run();
}

Runnable r = () -> { task code };

Thread t = new Thread(r);

t.start();
```

不要调用Thread类或Runnable对象的run方法，只会执行同一个线程中的任务，不会启动新线程。

### 1.10.2. 中断线程
没有强制线程终止的方法。然而interrupt方法可以用来请求终止线程。如果一个线程被调用interrupt方法时，线程的中断状态将被置位。这是每一个线程都具有的boolean标志。每个线程都应该不时地检查这个标志，以判断线程是否被中断。

```
while( !Thread.currentThread().isInterrupted() && ...)
{
	do more work...
}
```

如果线程被阻塞，就无法检测中断状态。当一个被阻塞的线程（调用sleep或wait）上调用interrupt方法时，阻塞调用将会被异常中断。

如果在中断状态时调用sleep，它不会休眠。相反，它将清除这一状态并抛出InterruptedException。如果循环调用sleep，需要捕获这个异常。

Interrupted方法是一个静态方法，它检测当前的线程是否被中断。调用interrupted方法会清楚该线程的中断状态。isInterrupted方法是一个实例方法。

不要在低层次捕捉InterruptedException。

```
void mySubTask() throws InterruptedException
{
	...
	sleep(..);
	...
}
```

### 1.10.3. 线程状态
线程可以有如下6种状态：

- New	新创建
- Runnable 可运行
- Blocked 被阻塞
- Waiting 等待
- Timed waiting 计时等待
- Terminated 被终止

要确定一个线程的当前状态，可调用getState方法。

#### 1.10.3.1. 新创建进程
new Thread(r)

#### 1.10.3.2. 可运行线程
一旦调用start方法，线程处于runnable状态。一个可运行的线程可能正在运行也可能没有运行，这取决于操作系统给线程提供运行的时间。
事实上，运行中的线程被中断，目的是为了让其他线程获得运行机会。

#### 1.10.3.3. 被阻塞线程和等待线程
当线程处于被阻塞或等待状态时，它暂时不活动。它不运行任何代码且消耗最少的资源。直到线程调度器重新激活它。

- 当一个线程试图获取一个内部的对象锁，而该锁被其他线程持有，则该线程进入阻塞状态。当所有其他线程释放该锁，并且线程调度器允许本线程持有它的时候，该线程将变成非阻塞状态。
- 当线程等待另一个线程通知调度器一个条件时，它自己进入等待状态。在使用Object.wait方法或Thread.join方法，或者是等待java.util.concurrent库中的Lock或Condition时，就会出现这种情况。实际上，被阻塞状态与等待状态是有很大不同的。
- 有几个方法有一个超时参数。调用它们导致线程进入计时等待状态。这一状态将一直保持到超时期满或者接收到适当的通知。带有超时参数的方法有Thread.sleep和Object.wait、Thread.join、Lock.tryLock以及Condition.await的计时版。

#### 1.10.3.4. 被终止的线程
线程因为如下两个原因终止：

- 因为run方法正常退出而自然死亡
- 因为一个没有捕获的异常终止了run方法而意外死亡。

**线程状态**

新创建 -开始-> 可运行</br>
可运行 -请求锁-> 被阻塞.......被阻塞 -出现通知-> 可运行</br>
可运行 -等待通知-> 等待.......等待 -出现通知-> 可运行</br>
可运行 -等待超时或通知-> 计时等待.......计时等待 -出现超时或通知-> 可运行</br>
可运行 -运行方法exits-> 被终止</br>

### 1.10.4. 线程属性
#### 1.10.4.1. 线程优先级
每个线程有一个优先级。默认情况下，一个线程继承它的父线程的优先级。默认情况下，一个线程继承它的父线程的优先级。可以用setPriority方法提高或降低任何一个线程的优先级。可以将优先级设置为MIN\_PRIORITY(在Thread类中定义为1)与MAX\_PRIORITY(定义为10)之间的任何值。NORM_PRIORITY被定义为5.

每当线程调度器有机会选择新线程时，它首先选择具有较高优先级的线程。但是，线程优先级是高度依赖于系统的。Java线程的优先级被映射到宿主机平台的优先级上，优先级个数也许更多，也许更少。

#### 1.10.4.2. 守护线程
`t.setDaemon(true);`</br>
将线程转换为守护线程。唯一用途是为其他线程提供服务。计时线程就是一个例子，它定时地发送计时器滴答信号给其他线程或清空过时的告诉缓存项的线程。当只剩下守护线程时，虚拟机就退出了。

#### 1.10.4.3. 未捕获异常处理器
`void uncaughtException(Thread t, Throwable e)`
线程的run方法不能抛出任何受查异常，但是，非受查异常会导致线程终止。

**受查：在编译时被强制检查的**

### 1.10.5. 同步
为了避免多线程引起的对共享数据的讹误，必须要同步存取。多个线程同时更新一个对象时会出现问题。

#### 1.10.5.1. 锁对象
有两种机制防止代码块受并发访问的干扰。Java提供了synchronized关键字达到这一目的，并且引入了ReentrantLock类。synchronized关键字自动提供一个锁。

用ReentrantLock保护代码块的基本结构：

```
myLock.lock();
try
{
	critical section....
}
finally
{
	myLock.unlock(); //必须放在finally中 避免发生异常
}
```
只有一个线程进入临界区，一旦一个线程封锁了锁对象，其他任何线程都无法通过lock语句。当其他线程调用lock时，它们被阻塞，直到第一个线程释放锁���象。

#### 1.10.5.2. 条件对象
需要使用一个条件对象来管理那些已经获得了一个锁但是���不能做有用工作的线程。</br>
假设账户要有足够资金才能转账。不可写成这样：

```
if(bank.getBalance(from) >= amount)
	bank.transfer(from, to, amount);
```
因为在调用transfer之前被中断。</br>
可以通过使用锁来保护检查与转账动作：

```
public void transfer(int from, int to, int amount)
{
	bankLock.lock();
	try
	{
		while (accounts[from]<amount)
		{
			// wait 余额不足
		}
		// 转账
	}
	finally
	{
		bankLock.unlock();
	}
}
```
但是此时如果有余额不足的，锁已被拿，别的用户也没法存钱。就会卡死在这里。

[条件锁]

```
private Condition sufficientFunds;
public Bank()
{
	sufficientFunds = bankLock.newCondition();
}
```

当发现余额不足时，调用`sufficientFunds.await();`现在线程阻塞并放弃了锁，它需要直到另一个线程调用统一条件上的signalAll方法时为止。

当另一个线程转账了，调用了`sufficientFunds.signalAll();`时，因为这一条件而等待的所有线程都会重新激活，同时它们将试图重新进入该对象。

如果没有其他线程调用signalAll，将进入死锁现象。如果所有其他线程被阻塞，程序就挂起了。一般在对象的状态有利于等待线程的方向改变时调用signalAll。

#### 1.10.5.3. synchronized关键字
锁和条件的关键之处：

- 锁用来保护代码片段，任何时刻只能有一个线程执行被保护的代码
- 锁可以管理试图进入被保护代码段的进程。
- 锁可以拥有一个或多个相关的条件对象。
- 每个条件对象管理那些已经进入被保护的代码段但还不能运行的线程

使用一个synchronized关键字声明，那么对象的锁将保护整个方法。要调用该方法，线程必须获得内部的对象锁。

```
public synchronized void method()
{
	method body
}
//等价于
public void method()
{
	this.intrinsiclock.lock();
	try
	{
		method body
	}
	finally{this.intrinsiclock.unlock();}
}
```

内部锁和条件存在一些局限：

- 不能中断一个正在试图获得锁的线程。
- 试图获得锁时不能设定超时。
- 每个锁仅有单一的条件，可能是不够的

在代码中应该使用哪一种？Lock和Condition对象还是同步方法？建议如下：

- 最好既不是用Lock/Condition也不使用synchronized关键字。在很多情况下可以使用java.util.concurreent包中的一种机制处理所有的加锁
- 如果synchronized关键字适合程序，那么尽量使用它
- 如果特别需要Lock/Condition结构提供的独有特性时，才使用他们

#### 1.10.5.4. 同步阻塞
#### 1.10.5.5. 监视器概念

- 监视器是只包含私有域的类
- 每个监视器类的对象有一个相关的锁
- 使用该锁对所有的方法进行加锁。换句话说，如果客户端调用obj.method()，那么obj对象的锁是在方法调用开始时自动获得，并且当方法返回时自动释放该锁。因为所有的域是私有的，这样的安排可以确保一个线程在对对象操作时，没有其他线程能访问该域。
- 该锁可以有任意多个相关条件

#### 1.10.5.6. *Volatile域
volatile关键字为实例域的同步访问提供了一种免锁机制。如果声明一个域为volatile，那么编译器和虚拟机就知道该域是可能被另一个线程并发更新的。</br>
如果一个对象有一个boolean标记，它的值被一个线程设置，却被另一个线程查询，我们可以使用锁：

```
private boolean done
public synchronized boolean isDone(){return done;}
public synchronized void setDone(){ done = true;}
```

如果另一个线程已经对该对象加锁，isDone和setDone方法可能阻塞。可以将域声明为volatile:

```
public volatile boolean done;
public boolean isDone() {return done;}
public void setDone(){done = true;}
```

#### 1.10.5.7. 原子性
java.util.concurrent.atomic包中有很多类使用了很高效的机器级指令来保证其他操作的原子性。

```
原子自增
public static AtomicLong nextNumber = new AtomicLong();
long id = nextNumber.incrementAndGet();
```

#### 1.10.5.8. 线程局部变量
#### 1.10.5.9. 锁测试与超时
tryLock可以申请一个锁，在成功获得锁后返回true。

```
if(myLock.tryLock(100, TimeUnit.SECONDS)) //使用超时参数
{
	try{..}
	finally{myLock.unlock();}
}
else
...
```

用超时的tryLock，如果在等待期间被中断，将会抛出InterruptedException异常。这是一个非常有用的特性，因为允许程序打破死锁。

#### 1.10.5.10. 读/写锁
java.util.concurrent.locks包含两个锁，一个是ReentrantLock类和ReentrantReadWriteLock类。如果很多线程从一个数据结构读取数据而很少线程修改其中数据的话，后者是十分有用的。

```
private ReentrantReadWriteLock rwl = new ReentrantReadWriteLock();

private Lock readLock = rwl.readLock();
private Lock writeLock = rwl.writeLock();

public double getTotalBalance()
{
	readLock.lock();
	try {...};
	finally{readLock.unlock();}
}

public void transfer(...)
{
	writeLock.lock();
	try {...};
	finally{writeLock.unlock();}
}
```

### 1.10.6. 队列阻塞
使用队列，可以安全地从一个线程向另一个线程传递数据。一个线程将任务对象插入一个队列中，而不是直接访问对象。另一个线程从队列中取出指令执行。只有该线程可以访问该银行对象的内部，因此不需要同步。

### 1.10.7. 线程安全的集合
#### 1.10.7.1. 高效的映射、集和队列
ConcurrentHashMap、ConcurrentSkipListMap、ConcurrentSkipListSet和ConcurrentLinkedQueue

#### 1.10.7.2. 映射条目的原子更新
#### 1.10.7.3. 对并发散列映射的批操作
在JAVA SE8中，即使有其他线程在处理映射，这些操作也能安全地执行。

- 搜索 为每个键或值提供一个函数，直到函数生成一个非null的结果
- 归约 组合所有键或值
- forEach为所有键或值提供一个函数。

每个操作都有4个版本：
- operationKeys 处理键
- operationValues 处理值
- operation 处理键和值
- operationEntries 处理Map.Entry对象

#### 1.10.7.4. 并发集视图
#### 1.10.7.5. 写数组的拷贝
#### 1.10.7.6. 并行数组算法
Arrays类提供了大量并行化操作，静态的Arrays.parallelSort方法可以对一个基本类型值或对象的数组排序。</br>
排序时，可以提供一个Comparator
`Arrays.parallelSort(words Comparator.comparing(String::length));`

#### 1.10.7.7. 较早的线程安全集合
#### 1.10.7.8. Callable与Future
Runnable封装一个异步运行的任务，Callable与Runnable类似，但是有返回值。Callable接口是一个参数化的类型，只有一个call方法。

Future保存异步计算的结果

### 1.10.8. 执行器
构建一个新的线程是有一定代价的，因为涉及与操作系统的交互，如果程序中创建了大量的生命期很短的线程，应该使用线程池。一个线程池中包含许多准备运行的空闲线程。当Runnable对象交给线程池时，就会有一个线程调用run，当run方法退出时，线程不会死亡，而是在池中准备为下一个请求提供服务。</br>
另一个使用线程池的理由是减少并发线程的数目。创建大量线程会大大降低性能甚至使虚拟机崩溃。如果有一个会创建许多现成的算法，应该使用一个线程数固定的线程池以限制并发线程综述。

- newCachedThreadPool 必要时创建新线程；空闲线程会被保留60秒
- newFixedThreadPool 该池包含固定数量的线程；空闲线程会一直保留
- newSingleThreadExecutor 只有一个线程的池，该线程顺序执行每一个提交的任务
- newScheduledThreadPool 用于预定执行而构建的固定线程池
- newSingleThreadScheduledExecutor 用于预定执行而构建的单线程池

#### 1.10.8.1. 线程池
```
Future<?> submit(Runnable task)
Future<T> submit(Runnable task, T result)
Future<T> submit(Callable<T> task)
```

1. 调用Executors静态方法newCachedThreadPool或newFixedThreadPool
2. 调用submit提交Runnable或Callable对象
3. 如果想要取消一个任务，那就要保存好返回的Future对象
4. 当不再提交任务时，调用shutdown

#### 1.10.8.2. 预定执行
ScheduledExecutorService具有预定执行或重复执行任务的方法

#### 1.10.8.3. 控制任务组
#### 1.10.8.4. Fork-Join框架
支持一些应用对每个处理器内核分别使用一个线程，来完成计算密集型任务。

#### 1.10.8.5. 可完成Future

### 1.10.9. 同步器
#### 1.10.9.1. 信号量
#### 1.10.9.2. 倒计时门栓
#### 1.10.9.3. 障栅
#### 1.10.9.4. 交换器
当两个线程在同一个数据缓冲区的两个实例上工作，就可以使用变换器。

#### 1.10.9.5. 同步队列
将生产者与消费者线程配对的机制。当一个线程调用SynchronousQueue的put方法时，它会阻塞知道另一个线程调用take位置

## 1.11. 多线程
### 1.11.1. 基本概念
进程是程序的一次执行过程。</br>
线程时一个程序内部的一条执行路径，一个进程如果并行可以有多个线程。</br>
线程作为调度和执行的单位，每个线程拥有独立的运行栈，每个线程拥有独立的运行栈和程序计数器</br>
多个线程共享相同的内存单元，可以访问相同的变量和对象。

- 并行：多个CPU同时执行多个任务
- 并发：一个CPU同时执行多个任务

多线程的优点：
1. 提高应用程序的响应，对可视化图形界面有意义
2. 提高cpu利用率
3. 改善程序结构

需要多线程
- 程序需要同时执行两个或多个任务
- 程序需要实现一些需要等待的任务时，如用户输入、文件读写操作
- 需要一些后台运行的程序时

### 1.11.2. 创建线程
通过java.lang.Thread

#### 1.11.2.1. 方法一
```java
/**
 * 创建多线程
 * 1. 创建一个继承与Thread类的子类
 * 2. 重写Thread类的run（）
 * 3. 创建Thread类的子类的对象
 * 4. 通过此对象调用start()
 */

// 1.创建继承于Thread类的子类
class MyThread extends Thread{
    // 2.重写Thread类的run()
    @Override
    public void run(){
        for (int i = 0; i < 100; i++) {
            if(i%2==0){
                System.out.println(i);
            }
        }
    }
}

public class ThreadTest{
    public static void main(String[] args) {
        //3. 创建Thread类的子类对象
        MyThread t1 = new MyThread();
        //4. 通过此对象调用start方法 I.启动当前线程 II.调用当前线程的run()
        //问题一：不能用对象.run()，这样不会启动一个新的线程
        t1.start();

        //问题二：在启动一个线程。一个线程只能start一次。不可以让已经start的线程再执行
        //需要重新创建一个
        MyThread t2 = new MyThread();
        t2.start();

        for (int i = 0; i < 100; i++) {
            if(i%2==1){
                System.out.println(i);
            }
        }
    }
}
```

```java
//也可以通过匿名子类的方式
new Thread(){
    @Override
    public void run(){
        ...
    }.start();
}
```

#### 1.11.2.2. 方法二

    ```java
    /**
    * 创建多线程的第二种方法：实现Runnable接口
    * 1. 创建一个实现了Runnable接口的类
    * 2. 实现类去实现Runnable中的抽象方法：run()
    * 3. 创建实现类的对象
    * 4. 将此对象作为参数传递到Thread类的构造器中，创建Thread类的对象
    * 5. 通过Thread类的对象调用start()
    */
    
    //1.创建一个实现了Runnable接口的类
    class MThread implements Runnable{
        //2. 实现类去实现Runnable中的抽象方法：run()
        @Override
        public void run() {
            for (int i = 0; i < 100; i++) {
                if(i%2==0){
                    System.out.println(Thread.currentThread().getName() + i);
                }
            }
        }
    }
    public class ThreadTest1 {
        public static void main(String[] args) {
            //3. 创建实现类的对象
            MyThread myThread = new MyThread();
            //4. 将此对象作为参数传递到Thread类的构造器中，创建Thread类的对象
            Thread t1 = new Thread(myThread);
            //5. 通过Thread类的对象调用start() I.启动线程 II.调用当前线程的run方法
            // 为什么Thread类的run会跑到Runnable接口的实现类去
            // 源码：如果Runnable不为null则调用Runnable中的run()
            t1.start();
    
            // 在启动第二个线程，Runnable接口实现类可以共用
            Thread t2 = new Thread(myThread);
            t2.start();
        }
    }
    ```

#### 1.11.2.3. 比较创建线程的两种方式
开发中优先选择实现Runnable接口的方式原因
- 实现的方式没有类的单继承性的局限性，如果一个类自己本来就有父类就无法用Thread类实现
- 实现的方式更适合处理多个线程有共享数据的情况

相同点：
- 都需要重写run方法



#### 1.11.2.4. 常用的方法
```java
import com.sun.deploy.net.proxy.ProxyUnavailableException;

/**
 * 测试Thread的常用方法
 * 1. start()
 * 2. run()
 * 3. currentThread() 返回当前代码的线程
 * 4. getName() 获取当前线程的名字
 * 5. setName() 设置当前线程的名字
 * 6. yield(); 释放cpu执行权，但是可能下个cpu又被这个线程抢到了。等于return的结果，但是下一次在调用的时候，从yield的下一句开始，而不是从头开始
 * 7. join() 在线程A中调用线程B的join()方法，线程A就进入阻塞状态，直到线程B完全执行完以后，线程A才结束阻塞状态
 * 8. stop() 强制线程声明周期结束，不推荐使用
 * 9. sleep() 阻塞毫秒
 * 10. isAlive() 判断线程是否还存活
 */

class MyThread1 extends Thread{
    // 给线程设置名字的另一种方法
    public MyThread1(String name){
        super(name);
    }
    @Override
    public void run(){
        for (int i = 0; i < 100; i++) {
            if(i%2==0){
                System.out.println(Thread.currentThread().getName() + i);
            }
        }
    }
}

public class ThreadMethodTest {
    public static void main(String[] args) {
        MyThread1 thread = new MyThread1("线程一");
        // 给线程设置名字
        thread.setName("线程一");
        thread.start();
        for (int i = 0; i < 100; i++) {
            if(i%2==0){
                System.out.println(Thread.currentThread().getName() + i);
            }
            try {
                thread.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        System.out.println(thread.isAlive());

    }
}
```

### 1.11.3. 线程的调度
- 调度策略
  - 时间片
  - 抢占式：高优先级的线程抢占CPU

- Java的调度方法
  - 通优先级线程组成先进先出队列（先到先服务），使用时间片策略
  - 对高优先级，使用优先调度的抢占式策略

- 线程的优先等级
  - MAX_PRIORITY: 10
  - MIN_PRIORITY: 1
  - NORM_PRIORITY: 5  默认

- 涉及方法:
  - getPriority() 返回线程优先级
  - setPriority() 改变线程的优先级

**说明：高优先级抢占低优先级，只是从概率上来讲，高优先级有更高概率，并不意味着只有当高优先级的线程执行完以后才轮到低优先级。**

### 1.11.4. 线程的生命周期
Thread.State记录了几种生命周期状态
- new
- runnable
- blocked
- waiting
- timed_waiting
- terminated 

通常经历五个状态：
- 新建
- 就绪 
  - 新建的线程被start()之后，已经具备运行条件，只是没有分配到cpu
  - 从阻塞到就绪 sleep()的时间到
  - 从阻塞到就绪 join()结束
  - 从阻塞到就绪 获取同步锁
  - 从阻塞到就绪 notify()/notifyAll()
  - 从阻塞到就绪 resume()
- 运行 
  - 从就绪获取cpu变成运行，如果失去了cpu回到就绪状态
- 阻塞
  - 运行时调用sleep();
  - join();
  - 等待同步锁
  - wait() 同时会释放锁
  - suspend() 挂起
- 死亡
  - 从运行执行完run();
  - 调用线程的stop();
  - error/exception 且没有处理

### 1.11.5. 解决线程的安全问题
- 多个线程执行的不确定性引起执行结果的不稳定
- 多个线程对账本的共性，会造成操作的不完整性，会破坏数据

```java
import static java.lang.Thread.sleep;

/**
 * 创建三个窗口卖票，共100张
 * 问题：卖票过程中出现了重票、错票 --> 出现了线程的安全问题
 * 出现问题的原因：某个线程操作车票的过程中，尚未操作完成时，其他线程参与进来，也操作车票
 * 如何解决：当一个线程在操作ticket的时候，其他线程不能参与进来，直到线程A操作完ticket时，其他线程才可以开始操作。
 *          这种情况，即使线程A出现了阻塞也不能被改变。
 * 在java中通过同步机制，来解决线程的安全问题
 * 方式一：同步代码块
 * synchronized(同步监视器){
 *     需要同步的代码
 * }
 * 说明：操作共享数据的代码，即为需要被同步的代码
 *      同步监视器，俗称：锁。任何一个类的对象，都可以充当锁。
 *          要求：多个线程必须共用同一个锁。Runnable可以用this，因为就一个对象唯一
 *              但是Thread不能用this，因为要创建多个Thread对象，不唯一，可以使用类名.class。这个是唯一的
 *
 * 方法二：同步方法
 * 如果操作共享数据的代码完整的声明在一个方法中，我们不妨将次方法声明同步的。
 *
 *
 * 使用同步的方式，解决了线程的安全问题，但是操作同步代码时只能有一个线程，相当于单线程，效率低。
 */

class Window implements Runnable{
    // 这里不需要
    private  int ticket = 100;
    Object obj = new Object();

    // 方法一
    /*
    @Override
    public void run(){
        while(true){
//            synchronized (obj){
//            synchronized (this) {
            synchronized (Window.class){
                try {
                    sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                if (ticket > 0) {
                    System.out.println(Thread.currentThread().getName() + ":卖票，票号为：" + ticket);
                    ticket--;
                } else {
                    break;
                }
            }
        }
    }

     */
// 方法二
    @Override
    public  void run(){
        while(true){
            show();
        }
    }

    private synchronized  void show(){ // 同步监视器是this !当时Thread继承类的时候，这里需要声明为静态的，同步监视器是类.class
        if (ticket > 0) {
            try {
                sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + ":卖票，票号为：" + ticket);
            ticket--;
        } else {
            return;
        }
    }
}

public class WindowTest {
    public static void main(String[] args) {
        Window w1 = new Window();

        Thread t1 = new Thread(w1);
        Thread t2 = new Thread(w1);
        Thread t3 = new Thread(w1);

        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");

        t1.start();
        t2.start();
        t3.start();
    }
}
```

### 1.11.6. 死锁问题
不同的线程分别占用对方需要的同步资源不放弃，都在等待对方放弃自己需要的同步资源，造成死锁
```java
import static java.lang.Thread.sleep;
/**
 * 演示程序的死锁
 */

public class ThreadTest{
    public static void main(String[] args) {
        StringBuffer s1 = new StringBuffer();
        StringBuffer s2 = new StringBuffer();
        new Thread(){
            @Override
            public  void run(){
                synchronized (s1){
                    s1.append("a");
                    s2.append("1");
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized (s2){
                        s1.append("b");
                        s2.append("2");

                        System.out.println(s1);
                        System.out.println(s2);
                    }
                }
            }
        }.start();

        new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized (s2){
                    s1.append("c");
                    s2.append("3");
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized (s1){
                        s1.append("d");
                        s2.append("4");

                        System.out.println(s1);
                        System.out.println(s2);
                    }
                }
            }
        }).start();
    }
}
```

解决方法：
- 专门的算法、原则
- 尽量减少同步资源的定义
- 尽量避免嵌套同步

### 1.11.7. Lock(锁)
JDK5.0开始提供了更强大的线程同步机制，同步锁使用Lock对象充当

ReentrantLock类实现了Lock，有用与synchronized相同的并发性和内存语义。
```java
import java.util.concurrent.locks.ReentrantLock;
import static java.lang.Thread.sleep;

/**
 * 解决线程安全问题的方式：Lock锁 ---JDK5.0新增
 * 1. synchronized 与 lock方式的异同
 *      相同：都可以解决线程的安全问题
 *      不同：synchronized机制在执行完同步代码后自动的释放
 *          lock需要手动的启动同步，手动的释放同步
 * 优先考虑用Lock->同步代码块->同步方法
 */

class SellTicket implements Runnable{
    private int ticket = 100;
    // 实例化lock
    private ReentrantLock lock = new ReentrantLock(); //参数为是否公平如果公平先进先出，默认false

    @Override
    public void run() {
        while(true) {
            try {
                //调用lock方法
                lock.lock();

                if (ticket > 0) {
                    try {
                        sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + ":卖票，票号为：" + ticket);
                    ticket--;
                } else {
                    break;
                }
            }finally {
                lock.unlock();
            }
        }
    }
}

public class LockTest {
    public static void main(String[] args) {
        SellTicket st = new SellTicket();

        Thread t1 = new Thread(st);
        Thread t2 = new Thread(st);
        Thread t3 = new Thread(st);

        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");

        t1.start();
        t2.start();
        t3.start();
    }
}

```

### 1.11.8. 线程的通信
```java
/**
 * 线程通信举例
 *
 * 涉及到的三个方法：
 * wait(); 一旦执行，线程进入阻塞，并释放同步监视器
 * notify();  一旦执行此方法，就会唤醒被wait的一个线程。如果有多个线程被wait，唤醒优先级高的。
 * notifyAll(); 唤醒所有wait的线程
 *
 *  前提：
 *  只能出现在同步代码块或同步方法中，LOCK不可
 *  这三个方法的调用者必须是同步方法或同步代码块中的同步监视器
 *
 *  这三个方法定义在Object对象中 
 */

class Num implements Runnable{
    private int num = 1;
    public void run(){
        while(true) {
            synchronized (this) {

                // 唤醒
                notify(); // 其实是this.notify()。 如果上面synchronized(obj) 这里要obj,notify

                if (num < 100) {
                    try {
                        Thread.sleep(10);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + ":" + num);
                    num++;

                    // 使得线程进入阻塞状态 同时会释放锁。而sleep不会释放锁
                    try {
                        wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                } else {
                    break;
                }
            }
        }
    }
}

public class CommunicationTest {
    public static void main(String[] args) {
        Num n = new Num();

        Thread t1 = new Thread(n);
        Thread t2 = new Thread(n);

        t1.setName("线程1");
        t2.setName("线程2");

        t1.start();
        t2.start();
    }
}
```

### 1.11.9. 消费者生产者问题
```java
/**
 * 生产者消费者问题
 *
 * 生产者将产品交给店员，消费者从店员取走商品，一次只能持有20个，如果少于20个，店员让生产者生成，如果没有商品了，让消费者等待
 */

class Clerk{
    private int productNum = 0;

    public synchronized void produceProduct() { //因为对象是唯一的，因此消费和生产用的是同一个锁
        if(productNum<=20){
            System.out.println(Thread.currentThread().getName() +":开始生成第" + productNum + "个产品");
            productNum++;
            notify();
        }else{
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

    }

    public synchronized void comsumeProduct() {
        if(productNum>0){
            System.out.println(Thread.currentThread().getName() +":开始消费第" + productNum + "个产品");
            productNum--;
            notify();
        }else{
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class Producter extends Thread{
    private Clerk clerk;
    public Producter(Clerk clerk){
        this.clerk = clerk;
    }

    @Override
    public void run(){
        System.out.println(getName() + "开始生产产品");
        while(true){
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            
            clerk.produceProduct();
        }
    }
}

class Consumer extends Thread{
    private Clerk clerk;
    public Consumer(Clerk clerk){
        this.clerk = clerk;
    }

    @Override
    public void run(){
        System.out.println(getName() + "开始消费产品");
        while(true){
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            clerk.comsumeProduct();
        }
    }
}

public class ProductTest {
    public static void main(String[] args) {
        Clerk clerk = new Clerk();
        Producter p1 = new Producter(clerk); // 生产者1
        Consumer c1 = new Consumer(clerk); // 消费者1
        p1.start();
        c1.start();
    }


}
```

### 1.11.10. JDK5.0新增的创建多线程方式
#### 1.11.10.1. 方法一 实现接口Callable
```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

/**
 * 创建线程的方法三：实现Callable接口。 ---JDK5.0新增
 *
 * 如何理解callable接口比runnable接口强大
 * 1. call方法可以有返回值
 * 2. call方法可以抛出异常
 * 3. callable支持泛型
 */

// 1.创建实现callable接口的实现类
class NumThread implements Callable{
    // 2.实现call方法，将子线程的操作实现，可以有返回
    @Override
    public Object call() throws Exception {
        int sum = 0;
        for (int i = 1; i <= 100 ; i++) {
            if(i%2==0){
                sum+=i;
            }
        }
        return sum;
    }
}

public class ThreadNew {
    public static void main(String[] args) {
        // 3.创建实现类对象
        NumThread nt = new NumThread();
        // 4.将对象作为参数传递到futuretask中
        FutureTask futureTask = new FutureTask(nt);
        // 5.执行线程
        new Thread(futureTask).start();

        // 6. 获取结果
        // get返回值即为futuretask构造器参数callable类重写的call()的返回值
        try {
            Object sum = futureTask.get();
            System.out.println(sum);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}
```

#### 1.11.10.2. 方法二 使用线程池
创建和销毁、使用量特别大的资源，对性能影响大。</br>
思路：提前创建多个线程，放入线程池中，使用时直接获取，使用完放回池中。避免频繁创建销毁、实现重复利用</br>
好处：
- 提高响应速度
- 降低资源消耗
- 便于线程管理

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.ThreadPoolExecutor;

/**
 * 创建线程的方式四：使用先线程池
 */

class NumberThread implements Runnable{

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if(i%2==0){
                System.out.println(Thread.currentThread().getName() + ":" + i);
            }
        }
    }
}

class NumberThread1 implements Runnable{

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if(i%2!=0){
                System.out.println(Thread.currentThread().getName() + ":" + i);
            }
        }
    }
}

public class ThreadPool {
    public static void main(String[] args) {
        // 1.提供指定线程数量的线程池
        ExecutorService service = Executors.newFixedThreadPool(10);
        ThreadPoolExecutor service1 = (ThreadPoolExecutor) service;
        // 设置线程池属性
        service1.setCorePoolSize();
        service1.setMaximumPoolSize();
        service1.setKeepAliveTime();

        // 2.执行指定的线程的操作，需要提供指定runnable接口或callable接口
        service.execute(new NumberThread()); // 使用runnable
        service.execute(new NumberThread1()); // 使用runnable
        //service.submit(); //适合callable

        // 3.关闭连接池
        service.shutdown();
    }
}
```

## 1.12. I/O流
字节流可以处理一切，字符流只能处理char。</br>
java.io包内

### 1.12.1. File
- 文件
  - 文件夹o
  - 普通文件

#### 1.12.1.1. 创建文件夹
```java
import java.io.File;
import java.io.IOException;

public class FileDemo1 {
    public static void main(String[] args) {
        File file1 = new File("./x9504.text");
        File file2 = new File("./x9504");
        File file3 = new File("./wyj/wyj");
        try {
            boolean flag1 = file1.createNewFile();//创建一个新的普通文件，如果已经存在了，则会创建失败，不创建
            System.out.println(flag1?"创建成功":"创建失败");
            boolean flag2 = file2.mkdir(); // 创建文件夹
            System.out.println(flag2?"创建成功":"创建失败");
            boolean flag3 = file3.mkdirs(); // 创建层次的文件夹
            System.out.println(flag3?"创建成功":"创建失败");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 1.12.1.2. 删除文件与查询文件
```java
// 删除文件
import java.io.File;

public class FileDemo1 {
    public static void main(String[] args) {
        File file1 = new File("./x9504.text");
        boolean flag1 = file1.delete(); //删除文件
        System.out.println(flag1?"删除成功":"删除失败");
    }
}
```
```java
// 查询文件
import java.io.File;

public class FileDemo1 {
    public static void main(String[] args) {
        // 1. 判断文件是否存在
        File file1 = new File("./x9504");
        boolean flag1 = file1.exists(); // 查询某个文件是否存在
        if(flag1){
            file1.delete();
        }

        // 2.获取文件的大小
        File file2 = new File("./wyj/wyj");
        long size = file2.length(); //获取文件大小
        System.out.println(size);

        // 3. 获取文件名
        String name = file1.getName();
        System.out.println(name);
        String path = file1.getPath(); // getAbsolutePath 返回绝对路径
        System.out.println(path);
    }
}
```

#### 1.12.1.3. 递归*
```java
import java.io.File;
/**
 * 显示文件夹下的所有文件
 */
public class FileDemo1 {
    public static void showFile(String pathName){
        File file1 = new File(pathName);
        if(file1.isDirectory()){ // 判断是否为文件夹
            File[] files = file1.listFiles();
            // 递归
            for (File f : files) {
                if(f.isDirectory()){
                    showFile(f.getPath());
                }else{
                    System.out.println(file1.getPath());
                }
            }
        }else{
            System.out.println(file1.getPath());
        }
    }

    public static void main(String[] args) {
        showFile("/Users/striverwang/IdeaProjects/多线程/");

    }
}
```

### 1.12.2. IO 输入输出流
#### 1.12.2.1. 字节流
##### 1.12.2.1.1. 字节输入流
utf-8 一个字占用3个字节。GB-2312 一个字占用2个字节

- `InputStream`输入流祖宗类
  - `FileInputStream` 一个一个字符
    ```java
        import java.io.File;
        import java.io.FileInputStream;

        public class IODemo1 {
            public static void main(String[] args) {
                try {
                    // 1. 在文件和程序之间铺设管道
        //            FileInputStream fis = new FileInputStream("./text.txt");
                    // 1. 第二种实现方式 推荐，可以先判断文件是否有效
                    File file = new File("./text.txt");
                    FileInputStream fis = null;
                    if(file.exists() && file.length()>0) {
                        fis = new FileInputStream(file);
                    }

                    // 2. 开始输入
                    int ch = 0;
                    while((ch = fis.read())!=-1){ //一个一个字节读 不能读中文，因为一个中文是多个字节
                        System.out.println((char)ch);
                    }

                    // 3. 关闭流
                    fis.close();
                } catch (Exception e) {
                    System.out.println("文件找不到");
                    e.printStackTrace();
                }
            }
        }
    ```
  - `BufferedInputStream` 字节缓冲输入流*
    ```java
    import java.io.*;
    public class IODemo1 {
        public static void main(String[] args) {
            try {
                // 设置数据源
                InputStream is = new FileInputStream("./text.txt");
                // 铺设管道
                BufferedInputStream bis = new BufferedInputStream(is);
                // 创建容器接收数据 一次性接收多少字节数据
                byte[] tmp = new byte[1024];
                int len = 0;
                while((len=bis.read(tmp))!=-1){
                    System.out.println(len);
                }
                bis.close();
            } catch (Exception e) {
                System.out.println("文件找不到");
                e.printStackTrace();
            }
        }
    }
    ```

##### 1.12.2.1.2. 字节输出流
- `OutputStream` 输出流祖宗类
  - `FileOutputStream` 一个一个字符 效率低
    ```java
        import java.io.File;
        import java.io.FileInputStream;
        import java.io.FileNotFoundException;
        import java.io.FileOutputStream;

        public class IODemo1 {
            public static void main(String[] args) {
                // 1. 创建数据
                String data = "hello java!!";
                // 2. 铺设管道
                try {
                    // 3. 打开文件
                    FileOutputStream fos = new FileOutputStream("./text.txt"); //如果文件不存在会自动创建，后面带一个append参数，如果为true则是追加，否则覆盖
                    fos.write(data.getBytes());
                    // 4. 关闭文件
                    fos.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }

    ```
  - `BufferedOutputStream`
    ```java
    import java.io.*;

    public class IODemo1 {
        public static void main(String[] args) {
            try {
                // 设置数据源
                InputStream is = new FileInputStream("./text.txt");
                // 铺设管道
                BufferedInputStream bis = new BufferedInputStream(is);
                BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("./text_cp.txt"));
                // 创建容器接收数据 一次性接收多少字节数据
                byte[] tmp = new byte[1024];
                int len = 0;
                while((len=bis.read(tmp))!=-1){
                    bos.write(tmp, 0, len);
                }
                bis.close();
                bos.close();
            } catch (Exception e) {
                System.out.println("文件找不到");
                e.printStackTrace();
            }
        }
    }
    ```
#### 1.12.2.2. 字符流
读取纯文本文件比较方便，已经帮我们处理了乱码问题

##### 1.12.2.2.1. 字符输入流
- `Reader` 字符输入流祖宗类
  - `FileReader`
  - `BufferedReader`

##### 1.12.2.2.2. 字符输出流
- `Writer` 字符输出流祖宗类
  - `FileWriter`
    ```java
    import java.io.File;
    import java.io.FileReader;
    import java.io.FileWriter;
    import java.io.IOException;

    public class Demo6 {

        public static void main(String[] args) throws IOException{
            FileReader fr=new FileReader(new File("1.txt"));
            FileWriter fw=new FileWriter(new File("2.txt"));
            char[] ca=new char[1024];
            int count;
            while((count=fr.read(ca))!=-1) {
                fw.write(ca,0,count);
            }
            fr.close();
            fw.close();
        }
    }
    ```
  - `BufferedWriter`
    ```java
    import java.io.BufferedReader;
    import java.io.BufferedWriter;
    import java.io.FileReader;
    import java.io.FileWriter;
    import java.io.IOException;

    public class Demo8 {

        public static void main(String[] args)throws IOException {
            BufferedWriter bw=new BufferedWriter(new FileWriter("3.txt"));
            BufferedReader br=new BufferedReader(new FileReader("1.txt"));
            String value="";
            while((value=br.readLine())!=null) { // readLine! 返回为null
                bw.write(value);
                bw.newLine();
            }
            bw.close();
            br.close();
        }
    }
    ```

### 1.12.3. 序列化
允许内存中的java对象转换成平台无关的二进制，才可以持久化的保存，比如String。

### 1.12.4. AIO/BIO/NIO
- BIO 同步阻塞 基于线程驱动模型
  - Block I/O ， 同步并阻塞的IO。BIO就是传统的java.io包下面的代码实现。
  - 为了实现多客户端同时相应的操作，就需要多线程。会影响性能，内存消耗大
  - 通过InputStream与OutputStream只能单向
  
- NIO（**IO的多路复用**）同步非阻塞 基于事件驱动模型
  - NIO 与原来的 I/O 有同样的作用和目的, 他们之间最重要的区别是数据打包和传输的方式。原来的 I/O 以流的方式处理数据，而 NIO 以块的方式处理数据。
  - 使用Channel，双向可写可读，通过ByteBuffer。selector多路复用器。通常设置为非阻塞
  - 在数据准备的时候是非阻塞的，但是在数据拷贝的时候是阻塞的。数据拷贝的时候使用内核的epoll函数，是零拷贝，使用拷贝内存地址的方法。
  - **与BIO的本质区别：NIO是一个线程对付多个客户多，BIO是多个线程对付多个客户端**
- AIO
  - Java AIO即Async非阻塞，是异步非阻塞的IO。

#### 1.12.4.1. 区别与联系
BIO （Blocking I/O）：同步阻塞I/O模式，数据的读取写入必须阻塞在一个线程内等待其完成。这里假设一个烧开水的场景，有一排水壶在烧开水，BIO的工作模式就是， 叫一个线程停留在一个水壶那，直到这个水壶烧开，才去处理下一个水壶。但是实际上线程在等待水壶烧开的时间段什么都没有做。

NIO （New I/O）：同时支持阻塞与非阻塞模式，但这里我们以其同步非阻塞I/O模式来说明，那么什么叫做同步非阻塞？如果还拿烧开水来说，NIO的做法是叫一个线程不断的轮询每个水壶的状态，看看是否有水壶的状态发生了改变，从而进行下一步的操作。

AIO （ Asynchronous I/O）：异步非阻塞I/O模型。异步非阻塞与同步非阻塞的区别在哪里？异步非阻塞无需一个线程去轮询所有IO操作的状态改变，在相应的状态改变后，系统会通知对应的线程来处理。对应到烧开水中就是，为每个水壶上面装了一个开关，水烧开之后，水壶会自动通知我水烧开了。

#### 1.12.4.2. 同步与异步
同步，是所有的操作都做完，才返回给用户结果。即写完数据库之后，再响应用户，用户体验不好。

异步，不用等所有操作都做完，就相应用户请求。即先响应用户请求，然后慢慢去写数据库，用户体验较好。

#### 1.12.4.3. 阻塞与非阻塞
阻塞，一直等待某个资源，直到满足条件，才下一步。

非阻塞，不等待，一直往下走。无限的循环执行，总有一次会拿到需要的资源。不常使用，系统负担大。**但是NIO并非这样，他其实是多路复用。**

#### 1.12.4.4. 适用场景
BIO方式适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4以前的唯一选择，但程序直观简单易理解。当数据量比较大时，并不在意线程，而在以带宽，推荐使用BIO

NIO方式适用于连接数目多且连接比较短（轻操作）的架构，比如聊天服务器，并发局限于应用中，编程比较复杂，JDK1.4开始支持。

AIO方式适用于连接数目多且连接比较长（重操作）的架构，比如相册服务器，充分调用OS参与并发操作，编程比较复杂，JDK7开始支持。

## 1.13. 网络编程

### 1.13.1. 两个问题
如何找到特定的主机和应用

如何高效的数据传输
### 1.13.2. 网络编程的两个要素
- 通信地址
  - 通过IP定位主机，通过端口定位特定的应用。
- 规则 网络通信协议
  - OSI协议 七层
    - 应用层
    - 表示层
    - 会话层
    - 传输层
    - 网络层
    - 数据链路层
    - 物理层
  - TCP/IP协议 四层
    - 应用层
      - HTTP/FTP/Telnet/DNS
    - 传输层
      - TCP/UDP
    - 网络层
      - IP/ICMP/ARP
    - 物理+数据链路层
      - Link
上层和下层是可以进行数据传输的

### 1.13.3. 通讯要素
- 要素1
  - IP地址 InetAddress类
    - 本地回环地址：127.0.0.1 localhost
    - IPv4（4个字节） IPv6（16个字节，8个16进制）
    - 192.168.0.0-192.168.255.255 私网地址，局域网使用
    - `InetAddress inet1 = InetAddress.getByName("IP或者域名");`
  - 端口号 标识正在计算机上运行的进程
    - 公认端口：0-1023。被预先定义的服务通信占用
    - 注册端口：1024-49151。分配给用户进程或应用程序
    - 动态私有端口：49152-65535
  - 端口与IP地址组合得出一个网络套接字：socket
- 要素2
  - TCP/UDP协议 是传输层协议中的重要协议
  - TCP
    - 使用TCP协议前，必须先建立TCP连接，形成传输数据通道
    - 传输前，采用“三次握手”方式，点对点通信，是可靠的
      - 客户端(seq=x SYN=1)
      - 服务端(ack=x+1, seq=y, SYN=1)
      - 客户端(ACK=y+1, seq=z)
    - 进行通信的两个应用进程：客户端、服务端
    - 连接中可进行大数据量的传输
    - 传输完毕，需释放已建立的连接，效率低，四次挥手
      - 主动方(Fin=1,ACK=z,seq=x)
      - 被动方(ACK=x+1,seq=z)
      - 被动方(Fin=1,ack=x,seq=y)
      - 主动方(ACK=y,seq=x)
  - UDP
    - 将数据、源、目的封装成数据包，不需要建立连接
    - 每个数据报的大小限制在64k内
    - 发送不管对方是否准备好，接收方收到也不确认，故是不可靠的
    - 可以广播发送
    - 发送数据结束时不需释放资源，开销小，速度快

### 1.13.4. TCP网络编程
```java
import org.junit.Test;

import java.io.*;
import java.net.InetAddress;
import java.net.ServerSocket;
import java.net.Socket;
import java.net.UnknownHostException;

/**
 * tcp的网络编程
 */
public class TCPTest {
    @Test
    public void client(){
        InetAddress inet = null;
        Socket socket = null;
        OutputStream os =null;
        try {
            // 1. 创建socket对象，指明服务器端的ip和端口号
            inet = InetAddress.getByName("127.0.0.1");
            socket = new Socket(inet,8899);
            // 2. 获取一个输出流，用于输出数据
            os = socket.getOutputStream();
            os.write("你好，我是客户端".getBytes());
            // 关闭数据输出 否则客户端会一直堵塞等待数据读取。
            socket.shuwdownOoutput();
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            // 3. 资源关闭
            if(os!=null){
                try {
                    os.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(socket!=null){
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }
    @Test
    public void server(){
        ServerSocket ss = null;
        Socket socket = null;
        InputStream is = null;
        ByteArrayOutputStream baos = null;
        try {
            // 1. 创建服务器端的socket，指明自己的端口号
            ss = new ServerSocket(8899);
            // 2. 调用accept方法，接收来自客户端的socket
            socket = ss.accept();
            is = socket.getInputStream();
            // 3. 读取输入流数据
            baos = new ByteArrayOutputStream();
            byte[] buffer = new byte[5];
            int len;
            while((len=is.read(buffer))!=-1){
                baos.write(buffer, 0, len);
            }
            System.out.println(baos.toString());
            System.out.println("收到来自："+socket.getInetAddress().getHostAddress()+"的数据");
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            // 4. 关闭资源
            try {
                baos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                is.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                socket.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                ss.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

### 1.13.5. UDP网络编程
```java
import org.junit.Test;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

public class UDPTest {
    @Test
    public void sender() throws Exception {
        DatagramSocket socket = new DatagramSocket();
        String str = "我是UDP发送的包";
        byte[] data = str.getBytes();
        InetAddress inet = InetAddress.getLocalHost();
        DatagramPacket packet = new DatagramPacket(data, 0, data.length, inet, 9090);
        socket.send(packet);
        socket.close();
    }

    @Test
    public void receiver() throws Exception {
        DatagramSocket socket = new DatagramSocket(9090);
        byte[] buffer = new byte[100];
        DatagramPacket packet = new DatagramPacket(buffer,0,buffer.length);
        socket.receive(packet);
        System.out.println(new String(packet.getData(), 0, packet.getLength()));
        socket.close();
    }
}
```

### 1.13.6. URL编程
统一资源定位符

组成部分：`<传输协议>://<主机名>:<端口号>/<文件名>#片段名?参数列表`

#### 1.13.6.1. 从网络下载

```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.URL;
import java.net.URLConnection;

public class URLTest {
    public static void main(String[] args) {
        URL url = null;
        HttpURLConnection urlConnection = null;
        InputStream is = null;
        FileOutputStream fos = null;
        try {
            url = new URL(".../.1.jpg");  //URL路径
            url.getProtocol();
            url.getHost();
            url.getPort();
            url.getPath();

            urlConnection = (HttpURLConnection)url.openConnection();
            urlConnection.connect();
            is = urlConnection.getInputStream();
            fos = new FileOutputStream("1.jpg");

            byte[] buffer = new byte[1024];
            int len;
            while((len=is.read(buffer))!=-1){
                fos.write(buffer,0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                is.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            urlConnection.disconnect();
        }
    }
}

```

-----
