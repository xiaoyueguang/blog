# Java入门

> 本文记录当时学习`Java`的过程

## 运行方式

首先先写一个简单的`Hello World!`.

```java
// Index.java. 每一个源文件里包含类, 类里面包含方法
class Index {
  public static void main (String[] args) {
    System.out.println("Hello World!");
  }
}
```

运行之前先了解下`Java`的两个命令: `java`和`javac`.

1. `java`用于执行类或jar文件.
2. `javac`用于编译java文件.

整个流程为:

java源代码 -> javac编译为字节码 -> 通过jvm执行

```bash
javac Index.java // 按照类名生成Index.class
java Index // 输出 Hello World!
```

或者我们可以将Java程序打包成jar文件, 再运行.

这种方式可以让开发者灵活编写代码, 组织依赖库, 封装到一个包, 保证了包的可移植性等优势.

```bash
// jar 文件名 入口类名 打包的类
jar -cvfe Index.jar Index Index.class
java -jar Index.jar // 输出 Hello World!
```

## 变量

`Java`的变量分为主数据类型和引用.

声明变量必须要有一个类型和名称

```java
// int 为类型. a为名称.
int a;
```

起名规律:

1. 名称以 (a-zA-Z_ $) 开头, 不能数字开头
2. 避开保留字. 比如`if`, `while`等.

### 主数据类型

内置数据类型:  
`Java`提供了八个基本类型, 即主数据类型.

* byte: 8位的有符号二进制整数.
* short:  16位有符号的以二进制补码表示的整数.
* int: 32位的有符号二进制整数.
* long: 64位的有符号二进制整数.
* float: 单精度的32位浮点数.
* double: 双精度的64位浮点数.
* boolean: 真假值.
* char: 字符.

#### 表格
|类型|位数|值域|
|:--:|:--:|:--:|
|boolean|java虚拟机决定|true或false|
|char|16bits|字符|
|byte|8bits|-128~127|
|short|16bits|-32768~32767|
|int|32bits|-2147483648~2147483647|
|long|64bits|-很大~+很大|
|float|32bits|规模可变|
|double|64bits|规模可变|

#### 例子

```java
boolean isB = true;
// 注意 这是字符, 不是字符串!
char c = 'c';
byte b = 1;
short s = 1;
int i = 1;
long l = 1;
float f = 1.1f;
double d = 1.1d;
```

### 引用

我们通过对一个类实例化, 获得一个对象, 而引用该对象的变量, 称之为引用变量.

```java
// dog 则为引用变量.
Dog dog = new Dog();
```

## 类和对象

类用来描述一类对象的状态和行为, 通过对类的实例化, 得到一个全新的对象.

比如说设置一个描述狗的类.

```java
class Dog {
  // 描述状态: 年龄
  int age;
  // 描述行为: 叫
  speak () {
  }
}

// 以下生成两个不同的对象.
Dog AA = new Dog();
Dog BB = new Dog();
```

`Java`三大特性:

* 多态: 一个类可能会被多个子类继承, 使得对象会有不同的数据类型或表现出不同的行为.
* 继承: 子类可以继承自父类的一些成员(数据或函数).
* 封装: 将一些有规律性的数据或函数封装到一个类对象上, 达到复用.

## 修饰符

`Java`分两种修饰符, 访问修饰符和非访问修饰符.

### 访问修饰符

1. default: 默认.
2. public: 对所有类公开.
3. private: 只在自己类中可见.
4. protected: 在自己类和子类都可见.

### 非访问修饰符

1. static: 静态修饰符, 类实例化后, 始终指向该属性或方法. 同时可通过[类名.(属性|方法)]来访问.
2. final: 声明后确保该变量值不会被改变, 同时不能被继承.
3. abstract: 抽象类. 继承抽象类的非抽象类必须要实现父类的所有抽象方法.
4. synchronized: 声明后只允许一个线程访问.
5. volatile: 修饰的成员每次被线程访问时, 都会强制性从共享内存中重新读取值. 变化时会强制性写到共享内存. 保证不同线程读到该值都是一致的.
6. transient: 标记后该成员将不会参与序列化过程.

### static

静态方法无法访问非静态上的方法和属性, 需要在静态方法里实例化后, 才能访问对象上的方法或属性, 同时静态属性和方法很难被垃圾回收.

### final

经过`final`修饰后.

1. 值无法被修改.
2. 方法无法被覆盖.
3. 类无法被继承.

## 运算符

* 算术运算符: + - * / % ++ --
* 关系运算符: == != > < >= <=
* 位运算符: & | ^ ~ << >> >>>
* 逻辑运算符: && || !
* 赋值运算符: = += -= *= /= (%)= <<= >>= &= ^= |=
* 其他运算符:
  1. 条件运算符: 又名 三元运算符. ?:
  2. instanceof 运算符: 检查对象是否为一个特定类型.

## 循环结构

```java
while (条件) {
  循环执行代码
}

do {
  循环执行代码
} while (条件)

for (初始值; 条件; 更新) {
  循环执行代码
}

for(声明语句 : 集合) {
  循环执行代码
}
```

循环时

  1. 可通过`break`来中止循环.  
  2. 可通过`continue`跳到下一循环.

## 条件

```java
if (条件) {
  满足条件时执行代码
}

if (条件) {
  满足条件时执行代码
} else {
  未满足条件时执行代码
}

if (条件1) {
  满足条件1时执行代码
} else if (条件2) {
  满足条件2时执行代码
} else {
  都不满足时执行代码
}

```

## switch

```java
switch(变量){
  case 值1:
    变量 == 值1时, 运行代码
    break;
  case 值2:
    变量 == 值2时, 运行代码
    break;
  default:
    运行到这里没被break的话, 执行代码
}
```

## 序列化

将对象表示为一个字节序列, 该字节序列包括该对象的数据、有关对象的类型的信息和存储在对象中数据的类型. 就叫序列化.

对象实现了`Serializable`, 说明这个对象是可以序列化了, 只有允许序列化, 才能够将对象序列化.

如果对象里引用了其他对象, 而其他对象没有实现序列化, 则会报错.

可通过关键字`transient`修饰后, 对应的值将不参与序列化.

```java
// 序列化
FileOutputStream fileStream = new FileOutputStream("./aaa.txt");
ObjectOutputStream os = new ObjectOutputStream(fileStream);
Person LiLei = new Person("LiLei", 18);
Person HanMei = new Person("HanMei", 16);
// 按顺序序列化
os.writeObject(LiLei);
os.writeObject(HanMei);
os.close();


// 解序列化
FileInputStream fileStream = new FileInputStream("./aaa.txt");
ObjectInputStream os = new ObjectInputStream(fileStream);
// 按顺序解序列化
Person LiLei = (Person) os.readObject();
Person HanMei = (Person) os.readObject();
System.out.println(LiLei.name + "年龄:" + LiLei.age);
System.out.println(HanMei.name + "年龄:" + HanMei.age);
os.close();
```

## 文件I/O

```java
// 写入
FileWriter writer = new FileWriter("./aaa.txt");
writer.write("Hello");
writer.close();

// 文件夹创建
File dir = new File("test-dir");
dir.mkdir();

// 读取文件夹下的所有文件
File dir = new File("./test-dir");
if (dir.isDirectory()) {
    String[] dirContents = dir.list();
    for (int i = 0; i < dirContents.length; i++) {
        System.out.println(dirContents[i]);
    }
}

// 获取文件的绝对路径
dir.getAbsolutePath();

// 删除文件. 文件夹下必须为空才能删除
boolean isDeleted = dir.delete();
```

因为`Java`的`I/O`操作都会比较费时, 在有大量的`I/O`操作时, 效率就会很低.

因此每次写入时, 先对写入的内容进行缓存, 存到内存中, 到一定的时机, 一次性写入到文件中. 能节省大量的时间.

而`Java`的`I/O`操作基本上都会有一个缓冲区, 只是`BufferedWriter`比`FileWriter`有更好的缓冲, 更适合大文件读写. 缓冲的时候内容不会实时写入到文件中, 需要`writer.flush()`才会马上写入到文件.

```java
FileWriter writer = new FileWriter("./aaa.txt");
writer.write("Hello");
// 取消下一行代码的注释, 在等待的时候发现 文件里有 Hello 文字.
// writer.flush();
try {
  // 故意等待5秒, 确保运行时有时间去看文件
  // 而在没有执行 flush 的情况下, 文件里为空. 没有 Hello 文件
  Thread.currentThread().sleep(5000);
} catch (InterruptedException e) {
}
writer.close();
```

```java
File file = new File("./aaa.txt");
FileReader fileReader = new FileReader(file);
BufferedReader reader = new BufferedReader(fileReader);
String line = null;
// 开始读取.
while ((line = reader.readLine()) != null) {
  System.out.println(line);
}
reader.close();
```

## 网络编程

### 实现一个 `SOCKET`

`socket`为数据单向, 因此不能输入输出同时进行.

```java
// 服务器发送数据 客户端接收数据
class Server {
  public static void main (String[] args) {
    try {
      ServerSocket serverSocket = new ServerSocket(4000);
      while (true) {
      // 有连接时, 会建立新的socket
        Socket sock = serverSocket.accept();
        PrintWriter writer = new PrintWriter(sock.getOutputStream());
        // 发送数据给服务端
        writer.println("服务端发送数据给客户端" + msg + "\n");
        writer.close();
      }
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
class Client {
  public static void main (String[] args) {
    try {
      Socket socket = new Socket("127.0.0.1", 4000);
      // 接受服务端返回内容
      InputStreamReader streamReader = new InputStreamReader(socket.getInputStream());
      BufferedReader reader = new BufferedReader(streamReader);
      String advice;
      while ((advice = reader.readLine()) != null) {
        System.out.println(advice);
      }
      // 内容读取完成后断开连接.
      reader.close();
      socket.close();
    } catch (IOException e) {
    }
  }
}

// 服务端接收数据 客户端发送数据
class Server {
  public static void main (String[] args) {
    try {
      ServerSocket serverSocket = new ServerSocket(4000);
      System.out.println("服务器即将启动，等待客户端的连接...");
      while (true) {
        // 有连接时, 会建立新的socket
        Socket sock = serverSocket.accept();
        // 服务器接受客户端信息
        InputStreamReader streamReader = new InputStreamReader(sock.getInputStream());
        BufferedReader reader = new BufferedReader(streamReader);
        String advice = reader.readLine();
        System.out.println("接收到客户端信息: " + advice);
      }
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
class Client {
  public static void main (String[] args) {
    try {
      Socket socket = new Socket("127.0.0.1", 4000);
      PrintWriter writer = new PrintWriter(socket.getOutputStream());
      // 客户端带数据请求过去
      writer.println("发送数据\n");
      // 发送完一定要关闭. 保证代码继续执行
      writer.close();
      socket.close();
    } catch (IOException e) {
    }
  }
}
```

## 线程

`Java`可以新建一个线程, 来启动另外的任务, 整体流程如下:

1. 一个类实现`Runnable`接口. 这个类就是即将要跑的任务.
2. 通过实例化`Thread`, 而他的参数为上面的任务.
3. 调用`Thread`对象上的`start`即可执行.

```java
class index {
  public static void main (String[] args) {
    Thread t = new Thread(new TestRun());

    t.start();
    System.out.println("我在index中!");
  }
}

class TestRun implements Runnable {
  public void run () {
    System.out.println("我在线程中!");
  }
}
```

线程会导致出现并发问题, 新的进程, 可能会导致有一个时间差, 从而读取数据时会出现读取的数据已经不是当时所要的数据.

这就需要给他们加一个锁.

## Java 数据结构

* 枚举(Enumeration): 枚举集合中的元素. 被迭代器取代.
* 位集合(BitSet): 保存位值的特殊数组.
* 向量(Vector): 一个动态的数组.(Java的大部分数组一开始要确定大小)
* 栈(Stack): 基于向量实现的一个动态数组, 实现后进先出.
* 字典(Dictionary): 抽象类, 存储键值对.
* 哈希表(Hashtable): 字典的具体实现.
* 属性(Properties): 继承哈希表, 键值对的键和值都为字符串.

## Java 集合框架

[文章来源](https://www.cnblogs.com/xiaoxi/p/6089984.html)

Java的大部分集合类由`Collection`和`Map`两种派生而出.

### Collection

一个接口, 高度抽象的集合, 包含了集合的基本操作和属性.

#### List

`List`是一个有序的队列, 里面的元素都有各自的索引. 从0开始.

##### ArrayList

`ArrayList`是一个动态数组, 比较常用的集合. 初始容量为10, 容量随着元素不断增加. 每次的增加都会进行容量检查, 判断是否溢出, 从而进行扩容操作.

比较擅长随机访问, 同时是非同步的.

##### LinkedList

`LinkedList`是一个双向链表, 因此不能随机访问. 非同步. 可实现同步访问.

##### Vector

线程安全的动态数组, 同步.

##### Stack

后进先出的堆栈.

#### Set

`Set`是一个不允许有重复元素的集合.

##### HashSet

没有重复元素的集合, 由`HashMap`实现. 元素没有一定的顺序(输入的顺序与输出的顺序不一致). 允许有一个`null`. 非同步.

采用`Hash`算法存储集合, 会有较好的存储和查找性能. 元素的位置都是固定的.

##### LinkedHashSet

继承`HashSet`, 基于`LinkedHashMap`实现. 有序非同步. 用链表维护元素的次序, 导致看起来会有一定的顺序(输入的顺序与输出的顺序一致)

##### TreeSet

有序集合, 基于`TreeMap`实现, 非线程安全. 支持两种排序(自然排序, 定制排序)

### Map

`Map`为一个映射接口, 即键值对(key-value)

`AbstractMap`是个抽象类, 实现了`Map`的大部分API.

`HashMap`, `TreeMap`, `WeakHashMap`都继承自`AbstractMap`.

#### HashMap

#### LinkedHashMap

`HashMap`的子类. 保留插入顺序. 不同步.

#### TreeMap

有序键值对, 非同步, 基于红黑树实现. 支持两种排序(自然排序, 定制排序)

### Iterator

迭代器. 遍历集合中的元素. 可通过`hasNext`, `next`, `remove`操作. 因此迭代是单向的, 同时一次`next`只允许一次`remove`.

### ListIterator

继承`Iterator`, 比`Iterator`更强大.

可操作的接口有 `hasNext`, `next`, `hasPrevious`, `previous`, `nextIndex`, `previousIndex`, `remove`, `set`, `add`

可看出`ListIterator`可以双向移动.

## 总结

本文仅仅是一个前端以及稍微有点后端知识的开发者, 通过[菜鸟教程](https://www.runoob.com/java/java-tutorial.html)以及书籍**Head First Java**, 学习`Java`的过程, 为了快速入门, 上手`SSH`以及`SSM`, 记录起来就是随心所欲, 初步上手框架后, 还需慢慢打磨基础部分!