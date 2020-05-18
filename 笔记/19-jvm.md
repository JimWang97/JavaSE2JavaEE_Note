# jvm



![image-20200516182513604](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516182513604.png)

## 第一章

![image-20200514111455033](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514111455033.png)

字节码文件本书具有跨平台性



![image-20200514111642614](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514111642614.png)

jvm是跨语言的平台

### 字节码

java语言编译成的字节码，任何能在jvm平台上执行的字节码格式都是一样的。不同的编译器，可以编译出相同的字节码文件。

### 虚拟机

可以分为系统虚拟机和程序虚拟机。**jvm是程序虚拟机**，专门为执行单个计算机程序而设计的。

jvm的输入就是字节码。他是一个跨语言的平台。jvm是二进制字节码的运行环境。

特点：

- 一次编译，到处运行
- 自动内存管理
- 自动垃圾回收功能

#### JVM的位置

![image-20200514141708159](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514141708159.png)

#### JDK与JRE的关系

![image-20200514141803914](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514141803914.png)

### JVM的整体结构

![image-20200514141930075](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514141930075.png)

HotSpot虚拟机是目前市面上高性能虚拟机的代表作之一，是java默认的虚拟机。上图是HotSpot的结构。

**多个线程共享方法区和堆，其他三个是每个线程独有一份。**

### java代码的执行流程

![image-20200514142619058](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514142619058.png)

反复执行的热点代码通过JIT编译成机器指令并且缓存起来，提高性能。

### JVM的架构模型

HotSpot是基于栈的指令集架构

![image-20200514143313795](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514143313795.png)

反编译指令`javap -v xxx.class`

基于栈的好处：

- 跨平台性
- 可以在性能比较低的平台上使用
- 指令集小，指令多
- 执行性能比寄存器差

### JVM的生命周期

#### 虚拟机的启动

java虚拟机的启动时通过引导类加载器创建一个初始类来完成的，这个类是由虚拟机的具体实现指定的。

#### 虚拟机的执行

程序开始执行时他就开始运行，程序结束了，他就结束。

执行java程序其实就是执行一个java虚拟机的进程

#### 虚拟机的退出

- 当程序正常结束
- 当执行过程中出现异常
- 当操作系统出现异常
- 调用runtime的exit方法

### JVM的发展历程

- Sun Classic VM没有JIT编译器，效率低
- Exact VM
- HotSpot。JDK1.3开始成为默认虚拟机。
  - 通过计数器找到最具编译价值代码，编译成机器指令，可以直接给CPU运行，触发即时编译或栈上替换。

- JRockit。
  - 专注于服务器端应用，不关注启动速度，全部用JIT。
  - 是世界上最快的JVM

- J9

## 第二章 类加载子系统

![image-20200514151756665](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514151756665.png)

- 类加载器子系统负责从文件系统或者网络中加载class文件，class文件在文件开头有特定的文件表示![image-20200514154020818](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514154020818.png)
- ClassLoader只负责class文件的加载，至于它是否可以运行，则由ExecutionEngine决定
- 加载的类信息存放于一块称为方法区的内存空间。除了类的信息外，方法区中还会存放运行时常量池信息，可能还包含字符串字面量和数字常量。

### 类的加载过程

![image-20200514152539394](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514152539394.png)

分为三个环节：

- 加载
- 链接
  - 验证
  - 准备
  - 解析
- 初始化

#### 加载

1. 通过一个类的全限定名获取定义此类的二进制字节流
2. 将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构
3. **在内存中生成一个代表这个类的java.lang.Class对象**，作为方法区这个类的各种数据的访问入库

#### 链接

##### 验证

- 目的在于确保Class文件的字节流中包含的信息符合当前虚拟机要求，保证被加载类的正确性，不会危害虚拟机自身安全
- 主要包括四种校验：文件格式验证，元数据验证，字节码验证，符号引用验证。

##### 准备

- 为**类变量（就是静态变量）**分配内存并且设置该类变量的默认初始值，即零值。默认初始值

![image-20200514154451221](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514154451221.png)

- 这里不包含用final修饰的static，因为final在编译的时候就会分配了，准备阶段会显示初始化。
- 这里不会为实例变量分配初始化，类变量会分配在方法区中，而实例变量是会随着对象一起分配到java堆中。

##### 解析

- 将常量池内的符号引用转换为直接引用的过程
- 事实上，解析操作往往会伴随着jvm在执行完初始化之后再执行
- 直接引用就是直接指向目标的指针、相对偏移量或一个间接定位到目标的句柄

#### 初始化

- 初始化阶段就是执行类构造器方法`<clinit>()`的过程，执行静态代码块
- 此方法不需要定义，是javac编译器自动收集类中的所有类变量的赋值动作和静态代码块中的语句合并而来。**注意：如果没有static，就不会生成这个方法。**

![image-20200514160028131](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514160028131.png)

![image-20200514160038351](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514160038351.png)

- 构造器方法中指令按语句在源文件中出现的顺序执行的

![image-20200514160338823](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514160338823.png)

![image-20200514160544114](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514160544114.png)

**可以把定义放在静态代码块之后，因为在linking步骤中会先进行准备，把number赋为0，然后根据代码顺序，再被赋值为20，最后是10.**

![image-20200514160649674](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514160649674.png)

可以赋值，但是不同提前调用。

- `<clinit>()`不同于类构造器`<init>()` 

- 若该类具有父类，会先执行父类的`<clinit>()`

  ![image-20200514163408775](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514163408775.png)

- 虚拟机必须保证一个类的`<clinit>()`方法在多线程下被同步加锁

  - 一个线程开始初始化类的时候，别的线程就进不来了，一个类只会被加载一次。就是运行静态代码块一次。

### 类加载器分类

JVM支持两种类加载器，引导类加载器和自定义类加载器。

所有派生于抽象类ClassLoader的类加载器都划分为自定义类加载器。所以扩展类加载器和系统类加载器都是自定义类加载器。

![image-20200514164258950](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514164258950.png)

`xxx.class.getClassLoader();`获取某个类的ClassLoader。用户自定义类默认使用系统类加载器来加载。

系统的核心类库（比如String）都是使用引导类加载器加载的。

#### 引导类加载器 Bootstrap ClassLoader

- 使用c/c++实现的，嵌套在JVM内部
- 用来加载java核心库
- 不继承自java.lang.ClassLoader，没有父加载器
- 加载扩展类和系统类加载器，并指定为他们的父类加载器
- 只加载包名为java、javax、sun等开头的类

#### 扩展类加载器 Extension ClassLoader

- java语言编写
- 派生于ClassLoader
- 父类加载器为引导类加载器
- 从java.ext.dirs系统属性所指定的目录中加载库，或从JDK的安装目录的jre/lib/ext子目录下加载类库。**如果用户创建的JAR放在此目录下，也会自动由扩展类加载器加载**

#### 系统类加载器 AppClassLoader

- java编写
- 派生于ClassLoader
- 父类加载器为引导类加载器
- 负责加载环境变量classpath或系统属性java.class.path指定路径下的类库
- 该类加载是程序中默认的类加载器

#### 用户自定义类加载器

为什么要定义类加载器？

- 隔离加载类（避免类冲突）
- 修改类加载的方式（需要的时候动态加载）
- 扩展加载源
- 防止源码泄露（在自定义类加载器中使用解密操作）

实现步骤：

1. 继承ClassLoader
2. 在JDK1.2之前要覆盖loadClass()方法，1.2之后使用findClass()重写
3. 没有过于复杂的需求，可以直接继承URLLoadClass，可以避免编写findClass。

### 关于ClassLoader

是一个抽象类，其他所有的类加载器都继承他。

![image-20200514171621699](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514171621699.png)

获取ClassLoader的途径：

- `xxx.class.getClassLoader()`
- `Thread.currentThread().getContextClassLoader()`
- `ClassLoader.getSystemClassLoader()`
- `DriverManager.getCallerClassLoader()`

### 双亲委派机制

java虚拟机对class文件采用的是**按需加载**的方式，加载某个类的class文件时，采用双亲委派机制

![image-20200514185956474](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514185956474.png)

在当前目录下创建`java.lang.String`类。看他到底是会调用哪一个String，是核心类库还是自定义的这个String类。实际上是会调用核心类库中的类，这与双亲委派机制有关。

- 工作原理

  - 如果一个类加载器收到了类加载的请求，并不会先去加载，而是委托给父类的加载器去执行
  - 如果父类加载器还存在父类加载器，再向上委托，依次递归到达顶层的引导类加载器
  - 如果父加载器可以完成类加载任务，就成功返回，如果不行，子类才会去尝试加载。

  ![image-20200514190449206](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514190449206.png)

在这里，String被引导类加载器直接成功加载了，而引导类加载器加载的就是核心类库中的。

![image-20200514191222128](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514191222128.png)

直接通过双亲委派机制，去加载了核心类库中的String类，而那个类中是没有main方法的

#### 反向委托

有些情况，核心类只提供接口，具体的实现类由第三方提供，比如数据库jdbc的链接。这种情况下，接口有引导类加载器加载，然后反向委托给线程上下文类加载器

![image-20200514191647630](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514191647630.png)



双亲委派的优势：

- 避免类的重复加载
- 保护程序安全，防止核心API被随意篡改
  - 比如自定义类：java.lang.String
  - java.lang.xxx 也会异常，因为是java开头的，还是引导类加载器来加载。

### 沙箱安全机制

对java核心源代码的保护就叫沙箱安全机制。

### 其他

- **JVM中表示两个class对象是否为同一个类存在两个必要条件：**
  - **类的完整类名必须一致，包括包名**
  - **加载这个类的ClassLoader（指ClassLoader实例对象）必须相同**
- 如果一个类型是由自定义类加载器（非引导类加载器）加载的，那么JVM会将这个类加载器的一个引用作为类型信息的一部分保存在方法区中。

- java程序对类的使用方式分为：主动使用和被动使用
  - 主动使用分为七种情况
    - 创建类的实例
    - 访问某个类或接口的静态变量，或者对该静态变量赋值
    - 调用类的静态方法
    - 反射（Class.forName("xxx")）
    - 初始化一个类的子类
    - java虚拟机启动时被标明为启动类的类
    - JDK 7开始提供的动态语言支持
  - 被动使用，除了主动都是被动。
  - **区别：类的被动使用不会导致类的初始化**

## 第三章 运行时数据区

jvm内存布局规定了java在运行过程中内存申请、分配、管理的策略，保证了jvm的高效稳定运行。不同的jvm对于内存的划分方式和管理机制存在着部分差异。

![image-20200514195011552](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514195011552.png)

![image-20200514201926250](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514201926250.png)



方法区和堆一个虚拟机进程一份。

程序计数器、本地方法栈和虚拟机栈一个线程一份。

需要优化的是堆区（95%）与方法区（5%，JDK8之后称为元空间，使用本地内存）

一个JVM实例对应一个runtime实例，相当于运行时数据区。

### 线程

线程是一个程序里的运行单元，java运行多线程并行。HotSpot中每一个线程都与操作系统的本地线程直接映射。

### 程序计数器（PC寄存器）

JVM中的PC寄存器是堆物理PC寄存器的一种抽象模拟。

作用：用来存储指向下一条指令的地址，即将要执行的指令代码

任何时间一个线程都只有一个方法在执行，也就是所谓的当前方法。程序计数器会存储当前线程正在执行的java方法的jvm指令地址；或者，如果是在执行native方法，则是未指定值。

唯一一个在JVM虚拟机中没有规定任何OutOfMemoryError情况的区域

![image-20200514203058533](/Users/striverwang/Library/Application Support/typora-user-images/image-20200514203058533.png)

##### 两个常见问题

- PC寄存器存储字节码指令地址有什么用？
  - 因为CPU需要不停的切换各个现场，这时候切换回来以后，要知道接着从哪开始继续执行。
  - JVM的字节码解释器需要通过改变PC寄存器的值来明确下一条应该执行什么样的字节码指令
- PC寄存器为什么线程私有
  - 每个线程执行的指令地址不同。所以每个线程都需要单独记录

### 虚拟机栈

#### 虚拟机栈概述

![image-20200515100747184](/Users/striverwang/Library/Application Support/typora-user-images/image-20200515100747184.png)

栈是运行时的单位，堆是存储的单位。

每一个线程都会创建一个虚拟机栈，其内部保存一个个的栈帧，对应着一次JAVA方法的调用。

栈的生命周期与线程一致

作用：

- 主管java程序的运行，它保存方法的局部变量（八种基本数据类型和对象的引用地址）、部分结果，并参与方法的调用和返回

对于栈来说不存在垃圾回收问题

可以设置栈的大小，通过`-Xss`

#### 栈的存储单位

栈中的数据以栈帧为基本单位存储的

线程上正在执行的每个方法都各自对应一个栈帧

在一个时间点上只有一个活动的栈帧，称为当前栈帧，当前方法。这个方法的类称为当前类。

java方法有两种返回函数的方法，一种是正常的函数返回，使用return指令；另外一种是抛出异常，不管是用哪种方式，都会导致栈帧被弹出。

![image-20200515104003935](/Users/striverwang/Library/Application Support/typora-user-images/image-20200515104003935.png)

每个栈帧内部存储着：

- 局部变量表
- 操作数栈
- 动态链接（指向运行时常量池的方法引用）
- 方法返回地址
- 一些附加信息

#### *局部变量表

- 定义为一个数字数组，主要用于存储方法参数和定义在方法体内的局部变量。这些数据类型包括各类基本数据类型、对象引用，以及returnAddress类型

- 由于局部变量表示建立在线程的栈上，是线程私有的，**不存在数据安全问题**

- **局部变量表所需的容量大小是在编译器确定下来的**

  

![image-20200515193914900](/Users/striverwang/Library/Application Support/typora-user-images/image-20200515193914900.png)

length和start表示作用域。与LineNumberTable对应。

##### slot槽的理解

局部变量表，最基本的存储单元是slot变量槽

局部变量表中存放编译器可知的各种基本数据类型，引用类型，returnAddress类型的变量

32位以内的类型栈一个slot，64位（long和double）需要占两个，如果是64位的用前一个索引。

**实例方法和构造器会在索引为0的slot处多一个this变量。静态方法中不会有。**

![image-20200516093118630](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516093118630.png)![image-20200516093124677](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516093124677.png)

double需要两个slot

**slot槽可重复利用**，当变量出了作用域就可以被重复利用了。

![image-20200516093321411](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516093321411.png)

![image-20200516093327758](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516093327758.png)

这里b出了作用域，他的slot被c重复利用。

##### 静态变量与局部变量的对比

- 变量的分类
  - 按照数据类型：1.基本数据类型 2.引用数据类型
  - 按照类中声明的位置：
    - 成员变量：在使用前，都经历过默认初始化赋值
      - 类变量：linking的prepare阶段：给类变量默认赋值 --> initial阶段：给类变量显示赋值即静态代码块赋值
      - 实例变量：随着对象的创建会在堆空间中分配实例变量空间，并进行默认赋值
    - 局部变量（在方法中的变量）：在使用前必须进行显示赋值！否则编译不通过。

##### 补充说明

在栈帧中，与性能调优关系最为密切的部分就是局部变量表。

局部变量表中的变量也是最重要的垃圾回收根节点，只要被局部变量表直接或间接引用的对象都不会被回收

#### *操作数栈

在方法执行过程中，根据字节码指令，往栈中写入数据或提取数据。

主要用于保存计算过程的中间结果，同时作为计算过程中变量临时的存储空间。

![image-20200516095119435](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516095119435.png)

在编译器就会确定好操作数栈的深度，当方法该开始执行时被创建。

不是通过索引的方式来访问数据，只能通过入栈和出栈。

#### 代码追踪

![image-20200516100038609](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516100038609.png)

![image-20200516095912605](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516095912605.png)

![image-20200516095929578](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516095929578.png)

#### ![image-20200516100004543](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516100004543.png)

#### 栈顶缓存技术

因为是使用零地址指令，需要多次的入栈出栈，会让指令更加繁琐，影响执行速度。通过栈顶缓存技术可以解决这个问题。

**将栈顶元素全部缓存在物理CPU的寄存器中，以此降低对内存的读写次数，提升执行引擎的执行效率**

#### *动态链接

**每个栈帧内部都包含一个指向运行时常量池中该栈帧所属方法的引用。**

在java编译到字节码时，所有的变量和方法引用都作为符号引用保存在class文件的常量池中。动态链接的作用就是为了将这些符号引用转换为调用方法的直接引用

![image-20200516102126500](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516102126500.png)

常量池

![image-20200516102145446](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516102145446.png)

![image-20200516102209822](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516102209822.png)

![image-20200516102358246](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516102358246.png)

#### 方法的调用：解析与分派

- 静态链接
  - 如果一个字节码文件被装载进JVM内部是，如果被调用的目标方法在**编译器可知**，并且运行期保持不变时，这种情况下将调用方法的**符号引用转换为直接引用**的过程称之为静态链接。
- 动态链接
  - 在编译期间无法确定，在运行期间才能由符号引用转换为直接引用的，叫动态链接。

- 早期绑定 对应静态链接
- 晚期绑定 对应动态链接
  - 比如子类对父类重写，运行的时候才能知道是运行子类的方法还是父类的

##### 虚方法

![image-20200516111401914](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516111401914.png)

子类中调用父类的方法，没有显示的写`super.xxx()`会调用`invokevirtual`因为不知道你子类是否会重写他，是虚方法。

在JDK8中使用lambda表达式可以直接调用`invokedynamic`



![image-20200516113549524](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516113549524.png)

方法区内有虚方法表

![image-20200516113947284](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516113947284.png)

#### *方法返回地址

- 存放调用该方法的pc寄存器的值
- **调用者的pc寄存器的值作为返回地址**，即调用该方法的指令的下一条指令的地址。
- 如果是异常情况，并且为在方法中的异常表中没有搜索到匹配的异常处理器就会异常退出方法。

![image-20200516115459798](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516115459798.png)

第4行到第8行的异常按照第11行进行处理

#### *一些附加信息

不一定有，根据虚拟机的实现不同而不同。可能会有一些调试信息

#### 相关面试题

- 开发中遇到的异常有哪些？
  
- ![image-20200515101123545](/Users/striverwang/Library/Application Support/typora-user-images/image-20200515101123545.png)
  
- 举例栈溢出的情况 StackOverflowError

  - 通过-Xss设置栈的大小：OOM（自动扩容情况下，内存不足，栈无法再扩容）

- 调整栈大小，就能保证不出现溢出吗？

  - 不能。自己无限调用自己的情况就不行。

- 垃圾回收是否会涉及到虚拟机栈空间？

  - 不会
  - 会出现ERROR的：虚拟机栈（StackOverflowError）、本地方发栈（StackOverflowError）、堆（OOM）、方法区（溢出）
  - 有GC的：堆、方法区

- 分配的栈内存越大越好吗？

  - 不是。总量不变，会导致线程数可能会变少。

- 方法中定义的局部变量是否线程安全？

  - 具体问题具体分析。

  - ![image-20200516141750491](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516141750491.png)

    ![image-20200516141906502](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516141906502.png)

  - 第二个从外部传入，可能被多个线程使用

    第三个可能不安全，因为把s1返回了，可能被其他多个线程调用。

### 本地方法接口（不在运行时数据区内）

一个native method就是一个java调用非java代码的接口。融合不同的编程语言为java所用。

- 与java环境外交互
  - 有时java应用需要与java外面的环境交互
- 与操作系统交互
  - 操作系统底层使用C写的。
- sun的解释器使用c写的。

### 本地方法栈

用于管理本地方法的调用。

与虚拟机栈都类似。

调用本地方法库时，都会使用本地的寄存器和本地的内存，不再受虚拟机限制。

### 堆

#### 核心概念

- 一个JVM实例只存在一个堆内存，是java内存管理的核心区域
- java堆区在jvm启动的时候就被创建，大小也确定了。是jvm内最大的一块空间
- **可以处于物理上不连续的内存空间，但是逻辑必须连续**
- 所有线程共享堆，但是可以划分线程私有的缓冲区TLAB

- 几乎所有的对象实例都在这里分配内存，还有可能在栈上分配（逃逸分析）
- **在方法结束以后，堆上的信息不会马上被移除，即使没有引用了。仅在垃圾回收的时候才会被移除。**
- 是GC的重点区。

##### 内存细分

基于分代收集理论设计。

- 对空间细分为：
  - JDK7：新生区、养老区、永久区
  - JDK8：新生区（年轻代）、养老区（老年代）、元空间

堆空间的大小包括：年轻代和老年代

#### 设置堆内存大小与OOM

- `-Xms`用于表示堆区的初始内存大小
- `-Xmx` 用于表示堆区的最大内存大小

默认情况下，初始内存是(电脑内存大小/64)，最大内存是(电脑内存大小/4)。

开发中建议把这俩值设成一样的。

**如果设置了600mb，其实只有575mb，其中的原因是什么？**

- 因为年轻代中S1和S0是相同空间大小的，但是只能选一个用，因此导致另一个的空间浪费，所以实际能被用到的内存减少了，主要是为了GC算法服务。



查看设置的参数的两种方法：

- `jps / jstat -gc 进程id`，终端输入
- `-XX:+PrintGCDetails` 在java执行时当做参数输入



##### OOM OutOfMemoryError

数组或者对象太多了，导致堆内存不足。

#### 年轻代与老年代

![image-20200516161304465](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516161304465.png)

- 年轻代（生命周期较短的瞬时对象）
  - Eden空间、Survivor0（from）、Survivor1(to)
- 老年区（生命周期比较长的）

##### 新生代与老年代的比例

![image-20200516161511627](/Users/striverwang/Library/Application Support/typora-user-images/image-20200516161511627.png)

默认为：新生代占1，老年代占2. （1：2）

`-NewRatio=2`可以设置这个比例，默认为2.



Eden区与S0、S1区比例为8:1:1

`-SurvivorRatio=8`默认为8

但是默认情况下有个自适应机制，不一定就是8:1:1



- 所有的java对象都是在Eden区被new出来的
- 绝大部分的java对象的销毁都在新生代进行了
- 可以使用选项`-Xmn`设置新生代最大内存大小

#### 图解对象分配过程

内存分配算法与内存回收算法密切相关。

![image-20200517094738059](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517094738059.png)

当Eden区放满之后，触发YGC/Minor GC（同一个东西，两种叫法）。把仍有引用的放到survivor区中，并为其附加一个年龄值，刚放过来为1。这样Eden区就清空了。

![image-20200517094942434](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517094942434.png)

当Eden区再次放满时，又触发了YGC，这时会放到另一个空的survivor区，原来在S0区的也会判断是否被引用，如果还是被引用的，也会一起搬到S1区，并且年龄+1。之后S0,S1相互交替的放。

![image-20200517095228737](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517095228737.png)

到年龄达到15之后，再加一就会进入到老年代。`-XX:MaxTenuringThreshold=<N>`可以设置。默认为15.



注意：

- **Survivor区不会主动的触发YGC，只会当Eden满时触发YGC时，会一起回收Eden和Survior区。**
- 如果Survivor满了，会有特殊操作，直接到老年代去

总结：

- S0 S1区：复制之后有交换，谁空谁是to
- 垃圾回收：频繁在新生代，很少在老年代，不会在永久代

整体流程：

![image-20200517095914352](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517095914352.png)

#### MinorGC、Major GC、Full GC

大部分回收的都是新生代。

在HotSpot中GC按照回收区域又分为两类：

- 部分收集（Partial GC）
  - 新生代收集(Minor GC)(Eden,s0,s1)
  - 老年代收集(major GC)
    - 目前只有CMS GC会有单独收集老年代的行为
    - 注意：很多时候Major GC和Full GC混淆使用。
  - 混合收集（Mixed GC）：收集整个新生代以及部分老年代的垃圾收集。
    - 目前只有G1 GC会有这种行为
- 整堆收集（Full GC）：堆空间和方法区的垃圾回收



- 年轻代GC(Minor)触发机制：
  - Eden满了就会触发
  - 非常频繁
  - 会引发STW，暂停其他用户的线程，等待垃圾回收结束，才能恢复
- 老年代GC(Major GC/ Full GC)触发机制：
  - 在老年代空间不足时，会先尝试触发Minor GC。如果之后空间还不足则触发Major GC
  - Major GC比Minor GC慢10倍以上
  - 如果Major GC后，内存还不足就报OOM
- Full GC触发机制
  - 调用`System.gc()`
  - 老年代空间不足
  - 方法区空间不足
  - 通过Minor GC后进入老年代的平均大小大于老年代的可用内存

#### 堆空间分代思想

分代的唯一理由就是优化GC性能。

#### 内存分配策略

- 优先分配到Eden区

- 大对象直接分配到老年代

  - 大于Eden区的
  - 尽量避免过多的大对象

- 长期存活的对象分配到老年代

- 动态对象年龄判断

  - **survivor区中相同年龄的所有对象大小的总和大于survivor空间的一般，年龄大于或等于该年龄的对象直接进入老年代。**

- 空间分配担保

  - `-XX:HandlePromotionFailure`
  - survivor放不下，要放到老年代

  ![image-20200517110259416](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517110259416.png)

  这里是20m，新生代不够这么大，直接进入了老年代

#### 为对象分配内存：TLAB(Thread Local Allocation Buffer)

- 堆区是线程共享区域，任何线程都可以访问到堆区中的共享数据
- 在并发环境下线程不安全
- 为避免多个线程操作同一个地址，需要加锁，影响分配效率



什么是TLAB？

- 对Eden区域进行划分，为每个线程分配了一个私有缓存区域
- 多线程同时分配内存是可以避免线程安全问题，还可以提升内存分配的吞吐量，称之为**快速分配策略**

- TLAB作为内存分配的首选
- 可以通过`UseTLAB`设置是否开启，默认开启
- 默认情况下仅占Eden空间的1%
- 如果TLAB空间分配失败，就会使用加锁机制



![image-20200517111458046](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517111458046.png)

#### 小结堆空间的参数设置

![image-20200517111737404](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517111737404.png)

关于HandlePromotionFailure参数：

![image-20200517112150655](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517112150655.png)

但是JDK7开始，不在影响，这个参数不会再被使用。

#### 堆是分配对象的唯一选择吗

如果经过逃逸分析后发现一个对象并没有逃逸出方法的话，就可能被优化成栈上分配。这样就不需要垃圾回收了，可以提升性能。

通过逃逸分析，HotSpot编译器能够分析出一个新的对象的引用的适用范围从而决定是否要将这个对象分配到堆上。

- 当一个对象在方法中被定义后，对象只在方法内部使用，则认为没有发生逃逸
- 当一个对象在方法中被定义后，对象在外部方法所引用，则认为发生逃逸。例如调用参数传递到其他地方中。

![image-20200517113108218](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517113108218.png)

该对象没有发生逃逸，可以直接放到栈空间中，栈帧会自动弹出，不需要GC。



![image-20200517113203943](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517113203943.png)

第一种发生了逃逸，第二种没有发生。



从JDK7开始逃逸分析默认开启。

##### 逃逸分析：代码优化

- 栈上分配。将堆分配转化为栈分配。

- 同步省略。如果一个对象被发现只能从一个线程被访问到，那么对于这个对象的操作可以不考虑同步

  - 在动态编译同步块的时候，JIT编译器可以借助逃逸分析来判断同步块所使用的锁对象是否只能够被一个线程访问，这个取消同步的过程叫同步省略，或锁消除
  - ![image-20200517141906835](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517141906835.png)

  因为这里的同步锁不是同一个，没有意义。

- 分离对象或标量替换。有的对象可能不需要作为一个连续的内存结构存储也可以被访问到，那么对象的部分（或全部）就可以不存储在内存，而是存储在CPU寄存器中。

  - 放到java中就是可以不放到堆当中，可以放到栈中。
  - 标量就是基本数据类型
  - 放到栈帧中的局部变量表
  - `-XX:+EliminateAllocations`默认打开

##### 总结

- HotSpot暂时没有用栈上分配，而是用了标量替换。

### 方法区

注意：永久代或者元空间其实是方法区的一种实现。

#### 栈、堆、方法区的交互关系

![image-20200517145514926](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517145514926.png)

![image-20200517145551554](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517145551554.png)

#### 方法区的理解

- 方法区看做是一块独立于java堆的内存空间。

- 在虚拟机启动的实际就被创建了。

- 定义了太多的类，会抛出内存溢出错误

#### 设置方法区大小与OOM

- JDK7以前：
  - `-XX:PermSize`来设置永久代初始分配空间。默认是20.75M
  - `-XX:MaxPermSize`来设定永久代最大可分配空间。32位默认64M，64位默认82M
- JDK8以后：
  - `-XX:MetaspaceSize`默认21M
  - `-XX:MaxMetaspaceSize`，值为-1表示没有限制。
  - MetaspaceSize为初始化的高水位线，如果一旦触及，Full GC就会被触发并卸载没用的类。然后这个高水位线会被重置，新的高水位线取决于GC后释放了多少空间。



- 在JDK7以前，习惯上把方法区称为永久代。在JDK8开始，使用元空间取代了永久代
- **元空间使用本地内存。永久代使用java虚拟机内存**。另外内部的细节也改变了

解决OOM：

- 分析内存快照文件dump。分析是否出现了内存泄露还是内存溢出

#### 方法区的内部结构

它用于存储已被虚拟机加载的**类型信息、常量、静态变量、即使编译器编译后的代码缓存**等。

- 类型信息

  - 类型的全类名
  - 直接父类的全类名(对于接口或是java.lang.Object是没有父类的)
  - 类型的修饰符
  - 类型直接接口的一个有序列表

  ![image-20200517164027035](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517164027035.png)

- 域信息（属性信息）

  - jvm必须在方法区中保存类型的所有域的相关信息以及域的声明顺序
  - 域的相关信息包括：域名称、域类型、域修饰符

  ![image-20200517164147021](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517164147021.png)

- 方法信息

  - 方法名称
  - 方法的返回类型
  - 方法参数的数量和类型（按顺序）
  - 方法的修饰符
  - 方法的字节码、操作栈、局部变量及大小
  - 异常表

  ![image-20200517164224055](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517164224055.png)

![image-20200517164343542](/Users/striverwang/Library/Application Support/typora-user-images/image-20200517164343542.png)



全局常量：static final

- 被声明为final的类变量的处理方法与普通的类变量不同，**每个全局常量在编译的时候就会被分配。而普通的类变量是在链接的准备阶段赋值为默认值，然后在初始化阶段赋值。**

##### 运行时常量池

#### 方法区使用举例

#### 方法区的演进细节

#### 方法区的垃圾回收

#### 总结

