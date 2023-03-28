---
title: Java学习笔记03
date: 
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208121301711.jpeg
tags: Java
categories: 学习笔记
description: 网络编程、反射
updated:
mathjax:
highlight_shrink:
---

# 网络编程

## IP地址

1. 概念:用于唯一标识网络中的每台计算机/主机
2. 查看ip地址: ipconfig
3. ip地址的表示形式:点分十进制XX.XX.XX.XX
4. 每一个十进制数的范围:0~255
5. ip地址的组成=网络地址+主机地址，比如:192.168.16.69
6. IPv6是互联网工程任务组设计的用于替代IPv4的下一代IP协议，其地址数量号称可以为全世界的每一粒沙子编上一个地址。
7. 由于IPv4最大的问题在于网络地址资源有限，严重制约了互联网的应用和发展。IPv6的使用，不仅能解决网络地址资源数量的问题，而且也解决了多种接入设备连入互联网的障碍



**ipv4地址分类**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207261934445.png)



## 域名

将ip地址映射成域名（HTTP协议）

**端口号**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207261938575.png)



## 网络通信协议

TCP/IP(Transmission Control Protocol/Internet Protocol)的简写，中文译名为传输控制协议/因特网互联协议,又叫网络通讯协议，这个协议是lnternet最基本的协议、Internet国际互联网络的基础，简单地说，就是由网络层的IP协议和传输层的TCP协议组成的。

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207261948692.png)



## TCP和UDP

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207261954413.png)



## InetAddress类

1.获取本机InetAddress对象getLocalHost

2.根据指定主机名/域名获取ip地址对象getByName

3.获取InetAddress对象的主机名getHostName

4.获取InetAddress对象的地址getHostAddress

```java
import java.net.InetAddress;
import java.net.UnknownHostException;

public class API_ {
    public static void main(String[] args) throws UnknownHostException {
        //1. 获取本机的InetAddress 对象
        InetAddress localHost = InetAddress.getLocalHost();
        System.out.println(localHost);
        //2. 根据指定主机名 获取 InetAddress对象
        InetAddress host1 = InetAddress.getByName("DESKTOP-L57VJE8");
        System.out.println(host1);
        //3. 根据域名返回 InetAddress对象, 比如 www.baidu.com 对应
        InetAddress host2 = InetAddress.getByName("www.baidu.com");
        System.out.println("host2=" + host2);
        //4. 通过 InetAddress 对象，获取对应的地址
        String hostAddress = host2.getHostAddress();
        System.out.println("host2 对应的ip = " + hostAddress);
        //5. 通过 InetAddress 对象，获取对应的主机名/或者的域名
        String hostName = host2.getHostName();
        System.out.println("host2对应的主机名/域名=" + hostName);
    }
}
```



## Socket

1.套接字(Socket)开发网络应用程序被广泛采用，以至于成为事实上的标准。

2.通信的两端都要有Socket，是两台机器间通信的端点

3.网络通信其实就是Socket间的通信。

4.Socket允许程序把网络连接当成一个流，数据在两个Socket间通过IO传输。

5.一般主动发起通信的应用程序属客户端，等待通信请求的为服务端



## TCP 网络通信编程

```java
import java.io.*;
import java.net.InetAddress;
import java.net.Socket;


public class SocketTCP02Client {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket(InetAddress.getLocalHost(), 9999);

        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
        bw.write("hello,server");
        bw.newLine();
        bw.flush();
        socket.shutdownOutput();

        BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
        String line;
        while ((line = br.readLine()) != null) {
            System.out.println(line);
        }

        br.close();
        bw.close();
        socket.close();
        System.out.println("客户端退出...");

    }
}
```

```java
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class SocketTCP02Server {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(9999);
        System.out.println("服务端，在9999端口监听，等待连接..");
        Socket socket = serverSocket.accept();

        BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
        String line;
        while ((line = br.readLine()) != null) {
            System.out.println(line);
        }

        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
        bw.write("hello,client");
        bw.newLine();
        bw.flush();//注意需要手动的flush
        socket.shutdownOutput();

        bw.close();
        br.close();
        socket.close();
        serverSocket.close();

    }
}
```

```java
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class TCPFileUploadServer {
    public static void main(String[] args) throws Exception {
        //1. 服务端在本机监听8888端口
        ServerSocket serverSocket = new ServerSocket(8888);
        System.out.println("服务端在8888端口监听....");
        //2. 等待连接
        Socket socket = serverSocket.accept();


        //3. 读取客户端发送的数据
        //   通过Socket得到输入流
        BufferedInputStream bis = new BufferedInputStream(socket.getInputStream());
        byte[] bytes = StreamUtils.streamToByteArray(bis);
        //4. 将得到 bytes 数组，写入到指定的路径，就得到一个文件了
        String destFilePath = "src\\mg2.png";
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(destFilePath));
        bos.write(bytes);
        bos.close();

        // 向客户端回复 "收到图片"
        // 通过socket 获取到输出流(字符)
        BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
        writer.write("收到图片");
        writer.flush();//把内容刷新到数据通道
        socket.shutdownOutput();//设置写入结束标记

        //关闭其他资源
        writer.close();
        bis.close();
        socket.close();
        serverSocket.close();

    }
}
```

```java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.InputStream;
import java.net.InetAddress;
import java.net.Socket;


public class TCPFileUploadClient {
    public static void main(String[] args) throws Exception {
        //客户端连接服务端 8888，得到Socket对象
        Socket socket = new Socket(InetAddress.getLocalHost(), 8888);
        //创建读取磁盘文件的输入流
        String filePath = "D:\\Java_Project\\basicOfJava\\winsock_\\upload_\\mg.png";
        BufferedInputStream bis  = new BufferedInputStream(new FileInputStream(filePath));

        //bytes 就是filePath对应的字节数组
        byte[] bytes = StreamUtils.streamToByteArray(bis);

        //通过socket获取到输出流, 将bytes数据发送给服务端
        BufferedOutputStream bos = new BufferedOutputStream(socket.getOutputStream());
        bos.write(bytes);//将文件对应的字节数组的内容，写入到数据通道
        bis.close();
        socket.shutdownOutput();//设置写入数据的结束标记

        //=====接收从服务端回复的消息=====

        InputStream inputStream = socket.getInputStream();
        //使用StreamUtils 的方法，直接将 inputStream 读取到的内容 转成字符串
        String s = StreamUtils.streamToString(inputStream);
        System.out.println(s);


        //关闭相关的流
        inputStream.close();
        bos.close();
        socket.close();

    }
}
```



### netstat指令

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207262217667.png)



## UDP网络编程

1.类 DatagramSocket和 DatagramPacket[数据包/数据报]实现了基于UDP协议网络程序。

2.UDP数据报通过数据报套接字DatagramSocket发送和接收，系统不保证UDP数据报一定能够安全送到目的地，也不能确定什么时候可以抵达。

3.DatagramPacket对象封装了UDP数据报，在数据报中包含了发送端的IP地址和端口号以及接收端的IP地址和端口号。

4.UDP协议中每个数据报都给出了完整的地址信息，因此无须建立发送方和接收方的连接

```java
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;


public class UDPReceiverA {
    public static void main(String[] args) throws IOException {
        //1. 创建一个 DatagramSocket 对象，准备在9999接收数据
        DatagramSocket socket = new DatagramSocket(9999);
        //2. 构建一个 DatagramPacket 对象，准备接收数据
        //   UDP 协议一个数据包最大 64k
        byte[] buf = new byte[1024];
        DatagramPacket packet = new DatagramPacket(buf, buf.length);
        //3. 调用 接收方法, 将通过网络传输的 DatagramPacket 对象
        //   填充到 packet对象
        //   当有数据包发送到 本机的9999端口时，就会接收到数据
        //   如果没有数据包发送到 本机的9999端口, 就会阻塞等待.
        System.out.println("接收端A 等待接收数据..");
        socket.receive(packet);

        //4. 可以把packet 进行拆包，取出数据，并显示.
        int length = packet.getLength();//实际接收到的数据字节长度
        byte[] data = packet.getData();//接收到数据
        String s = new String(data, 0, length);
        System.out.println(s);


        //===回复信息给B端
        //将需要发送的数据，封装到 DatagramPacket对象
        data = "好的, 明天见".getBytes();
        //说明: 封装的 DatagramPacket对象 data 内容字节数组 , data.length , 主机(IP) , 端口
        packet =
                new DatagramPacket(data, data.length, InetAddress.getByName("192.168.12.1"), 9998);

        socket.send(packet);//发送

        //5. 关闭资源
        socket.close();
        System.out.println("A端退出...");

    }
}
```

```java
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

public class UDPSenderB {
    public static void main(String[] args) throws IOException {

        //1.创建 DatagramSocket 对象，准备在9998端口 接收数据
        DatagramSocket socket = new DatagramSocket(9998);

        //2. 将需要发送的数据，封装到 DatagramPacket对象
        byte[] data = "hello 明天吃火锅~".getBytes(); //

        //说明: 封装的 DatagramPacket对象 data 内容字节数组 , data.length , 主机(IP) , 端口
        DatagramPacket packet =
                new DatagramPacket(data, data.length, InetAddress.getByName("192.168.12.1"), 9999);

        socket.send(packet);

        //3.=== 接收从A端回复的信息
        //(1)   构建一个 DatagramPacket 对象，准备接收数据
        //   UDP 协议一个数据包最大 64k
        byte[] buf = new byte[1024];
        packet = new DatagramPacket(buf, buf.length);
        //(2)    调用 接收方法, 将通过网络传输的 DatagramPacket 对象
        //   填充到 packet对象
        //   当有数据包发送到 本机的9998端口时，就会接收到数据
        //   如果没有数据包发送到 本机的9998端口, 就会阻塞等待.
        socket.receive(packet);

        //(3)  可以把packet 进行拆包，取出数据，并显示.
        int length = packet.getLength();//实际接收到的数据字节长度
        data = packet.getData();//接收到数据
        String s = new String(data, 0, length);
        System.out.println(s);

        //关闭资源
        socket.close();
        System.out.println("B端退出");

    }
}
```

# 反射

## 反射机制

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021612799.png)

**Java 反射机制可以完成**

1. 在运行时判断任意一个对象所属的类
2. 在运行时构造任意一个类的对象
3. 在运行时得到任意一个类所具有的成员变量和方法
4. 在运行时调用任意一个对象的成员变量和方法
5. 生成动态代理

**反射相关的主要类**

1. java.lang.Class:代表一个类，Class对象表示某个类加载后在堆中的对象
2. java.lang.reflect.Method:代表类的方法,Method对象表示某个类的方法
3. java.lang.reflect.Field:代表类的成员变量, Field对象表示某个类的成员变量
4. java.lang.reflect.Constructor:代表类的构造方法,Constructor对象表示构造器
5. 这些类在java.lang.reflection

```java
import java.io.FileInputStream;
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;
import java.util.Properties;

public class Reflection01 {
    public static void main(String[] args) throws Exception {



        //1. 使用Properties 类, 可以读写配置文件
        Properties properties = new Properties();
        properties.load(new FileInputStream("D:\\Java_Project\\basicOfJava\\reflection_\re.properties"));
        String classfullpath = properties.get("classfullpath").toString();//"com.hspedu.Cat"
        String methodName = properties.get("method").toString();//"hi"


        //2. 使用反射机制解决
        //(1) 加载类, 返回Class类型的对象cls
        Class cls = Class.forName(classfullpath);
        //(2) 通过 cls 得到你加载的类 .Cat 的对象实例
        Object o = cls.newInstance();
        System.out.println("o的运行类型=" + o.getClass()); //运行类型
        //(3) 通过 cls 得到你加载的类 .Cat 的 methodName"hi"  的方法对象
        //    即：在反射中，可以把方法视为对象（万物皆对象）
        Method method1 = cls.getMethod(methodName);
        //(4) 通过method1 调用方法: 即通过方法对象来实现调用方法
        System.out.println("=============================");
        method1.invoke(o); //传统方法 对象.方法() , 反射机制 方法.invoke(对象)

        //java.lang.reflect.Field: 代表类的成员变量, Field对象表示某个类的成员变量
        //得到name字段
        //getField不能得到私有的属性
        Field nameField = cls.getField("age"); //
        System.out.println(nameField.get(o)); // 传统写法 对象.成员变量 , 反射 :  成员变量对象.get(对象)

        //java.lang.reflect.Constructor: 代表类的构造方法, Constructor对象表示构造器
        Constructor constructor = cls.getConstructor(); //()中可以指定构造器参数类型, 返回无参构造器
        System.out.println(constructor);//Cat()



        Constructor constructor2 = cls.getConstructor(String.class); //这里传入的 String.class 就是String类的Class对象
        System.out.println(constructor2);//Cat(String name)
    }
}
```



```java
package reflection_;

public class Cat {
    private String name = "招财猫";
    public int age = 10; //public的

    public Cat() {} //无参构造器

    public Cat(String name) {
        this.name = name;
    }

    public void hi() { //常用方法
        //System.out.println("hi " + name);
    }
    public void cry() { //常用方法
        System.out.println(name + " 喵喵叫..");
    }
}
```

**反射优点和缺点**

1.优点:可以动态的创建和使用对象(也是框架底层核心)，使用灵活,没有反射机制,框架技术就失去底层支撑。

2.缺点:使用反射基本是解释执行，对执行速度有影响.

 **反射调用优化-关闭访问检查**

- Method和Field、Constructor对象都有setAccessible(方法)
- setAccessible作用是启动和禁用访问安全检查的开关
- 参数值为true表示反射的对象在使用时取消访问检查，提高反射的效率。参数值为false则表示反射的对象执行访问检查



## Class类

1. Class也是类,因此也继承Object类
2. Class类对象不是new出来的，而是系统创建的
3. 对于某个类的Class类对象，在内存中只有一份，因为类只加载一次
4. 每个类的实例都会记得自己是由哪个Class 实例所生成
5. 通过Class对象可以完整地得到一个类的完整结构,通过一系列API
6. Class对象是存放在堆的
7. 类的字节码二进制数据，是放在方法区的，有的地方称为类的元数据(包括方法代码,变量名，方法名，访问权限等等）

**Class 类的常用方法**

![image-20220802164207790](D:\typora图片\image-20220802164207790.png)

**获取 Class 类对象**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021702127.png)

**如下类型有 Class 对象**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021712921.png)



## 类加载

反射机制是java实现动态语言的关键，也就是通过反射实现类动态加载。

1.静态加载:编译时加载相关的类，如果没有则报错,依赖性太强

2.动态加载:运行时加载需要的类，如果运行时不用该类，即使不存在该类，则不报错，降低了依赖性



1.当创建对象时(new)//静态加载

2.当子类被加载时，父类也加载//静态加载

3.调用类中的静态成员时//静态加载

4.通过反射//动态加载



![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021729384.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021730799.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021731147.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021732249.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021732281.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021737281.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021738701.png)

```java
package reflection_.class_;

public class ClassLoad03 {
    public static void main(String[] args) throws ClassNotFoundException {

        //1. 加载 B 类，并生成 B 的 class 对象
        //2. 链接 num = 0
        //3. 初始化阶段
        // 依次自动收集类中的所有静态变量的赋值动作和静态代码块中的语句,并合并
        /*
        clinit() {
        System.out.println("B 静态代码块被执行");
        //num = 300;
        num = 100;
        }
        合并: num = 100
        */
        //new B();//类加载
        //System.out.println(B.num);//100, 如果直接使用类的静态属性，也会导致类的加载
        //看看加载类的时候，是有同步机制控制
        /*
        protected Class<?> loadClass(String name, boolean resolve)
        throws ClassNotFoundException
        {
            //正因为有这个机制，才能保证某个类在内存中, 只有一份 Class 对象
            synchronized (getClassLoadingLock(name)) {
            //.... }
        }
        */
        B b = new B();
    }
}

class B {
    static {
        System.out.println("B 静态代码块被执行");
        num = 300;
    }

    static int num = 100;

    public B() {//构造器
        System.out.println("B() 构造器被执行");
    }
}
```



## **通过反射获取类的结构信息**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021749014.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021749961.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021750461.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021750413.png)

```java
package reflection_;

import org.junit.jupiter.api.Test;

import java.lang.annotation.Annotation;
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

/**
 * 演示如何通过反射获取类的结构信息
 */

public class ReflectionUtils {
    public static void main(String[] args) {

    }

    @Test
    public void api_02() throws ClassNotFoundException, NoSuchMethodException {
        //得到Class对象
        Class<?> personCls = Class.forName("reflection_.Person");
        //getDeclaredFields:获取本类中所有属性
        //规定 说明: 默认修饰符 是0 ， public  是1 ，private 是 2 ，protected 是 4 , static 是 8 ，final 是 16
        Field[] declaredFields = personCls.getDeclaredFields();
        for (Field declaredField : declaredFields) {
            System.out.println("本类中所有属性=" + declaredField.getName()
                    + " 该属性的修饰符值=" + declaredField.getModifiers()
                    + " 该属性的类型=" + declaredField.getType());
        }

        //getDeclaredMethods:获取本类中所有方法
        Method[] declaredMethods = personCls.getDeclaredMethods();
        for (Method declaredMethod : declaredMethods) {
            System.out.println("本类中所有方法=" + declaredMethod.getName()
                    + " 该方法的访问修饰符值=" + declaredMethod.getModifiers()
                    + " 该方法返回类型" + declaredMethod.getReturnType());

            //输出当前这个方法的形参数组情况
            Class<?>[] parameterTypes = declaredMethod.getParameterTypes();
            for (Class<?> parameterType : parameterTypes) {
                System.out.println("该方法的形参类型=" + parameterType);
            }
        }

        //getDeclaredConstructors:获取本类中所有构造器
        Constructor<?>[] declaredConstructors = personCls.getDeclaredConstructors();
        for (Constructor<?> declaredConstructor : declaredConstructors) {
            System.out.println("====================");
            System.out.println("本类中所有构造器=" + declaredConstructor.getName());//这里只是输出名

            Class<?>[] parameterTypes = declaredConstructor.getParameterTypes();
            for (Class<?> parameterType : parameterTypes) {
                System.out.println("该构造器的形参类型=" + parameterType);
            }


        }

    }

    //第一组方法API
    @Test
    public void api_01() throws ClassNotFoundException, NoSuchMethodException {

        //得到Class对象
        Class<?> personCls = Class.forName("reflection_.Person");
        //getName:获取全类名
        System.out.println(personCls.getName());//reflection_.Person
        //getSimpleName:获取简单类名
        System.out.println(personCls.getSimpleName());//Person
        //getFields:获取所有public修饰的属性，包含本类以及父类的
        Field[] fields = personCls.getFields();
        for (Field field : fields) {//增强for
            System.out.println("本类以及父类的属性=" + field.getName());
        }
        //getDeclaredFields:获取本类中所有属性
        Field[] declaredFields = personCls.getDeclaredFields();
        for (Field declaredField : declaredFields) {
            System.out.println("本类中所有属性=" + declaredField.getName());
        }
        //getMethods:获取所有public修饰的方法，包含本类以及父类的
        Method[] methods = personCls.getMethods();
        for (Method method : methods) {
            System.out.println("本类以及父类的方法=" + method.getName());
        }
        //getDeclaredMethods:获取本类中所有方法
        Method[] declaredMethods = personCls.getDeclaredMethods();
        for (Method declaredMethod : declaredMethods) {
            System.out.println("本类中所有方法=" + declaredMethod.getName());
        }
        //getConstructors: 获取所有public修饰的构造器，包含本类
        Constructor<?>[] constructors = personCls.getConstructors();
        for (Constructor<?> constructor : constructors) {
            System.out.println("本类的构造器=" + constructor.getName());
        }
        //getDeclaredConstructors:获取本类中所有构造器
        Constructor<?>[] declaredConstructors = personCls.getDeclaredConstructors();
        for (Constructor<?> declaredConstructor : declaredConstructors) {
            System.out.println("本类中所有构造器=" + declaredConstructor.getName());//这里只是输出名
        }
        //getPackage:以Package形式返回 包信息
        System.out.println(personCls.getPackage());//reflection_
        //getSuperClass:以Class形式返回父类信息
        Class<?> superclass = personCls.getSuperclass();
        System.out.println("父类的class对象=" + superclass);//
        //getInterfaces:以Class[]形式返回接口信息
        Class<?>[] interfaces = personCls.getInterfaces();
        for (Class<?> anInterface : interfaces) {
            System.out.println("接口信息=" + anInterface);
        }
        //getAnnotations:以Annotation[] 形式返回注解信息
        Annotation[] annotations = personCls.getAnnotations();
        for (Annotation annotation : annotations) {
            System.out.println("注解信息=" + annotation);//注解
        }


    }
}

class A {
    public String hobby;

    public void hi() {

    }

    public A() {
    }

    public A(String name) {
    }
}

interface IA {
}

interface IB {

}

@Deprecated
class Person extends A implements IA, IB {
    //属性
    public String name;
    protected static int age; // 4 + 8 = 12
    String job;
    private double sal;

    //构造器
    public Person() {
    }

    public Person(String name) {
    }

    //私有的
    private Person(String name, int age) {

    }

    //方法
    public void m1(String name, int age, double sal) {

    }

    protected String m2() {
        return null;
    }

    void m3() {

    }

    private void m4() {

    }
}
```



##  通过反射创建对象

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021832476.png)

```java
package reflection_;

import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

public class ReflecCreateInstance {
    public static void main(String[] args) throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException, InvocationTargetException {

        //1. 先获取到 User 类的 Class 对象
        Class<?> userClass = Class.forName("reflection_.User");
        //2. 通过 public 的无参构造器创建实例
        Object o = userClass.newInstance();
        System.out.println(o);
        //3. 通过 public 的有参构造器创建实例
        /*
        constructor 对象就是
        public User(String name) {//public 的有参构造器
            this.name = name;
        }
        */
        //3.1 先得到对应构造器
        Constructor<?> constructor = userClass.getConstructor(String.class);
        //3.2 创建实例，并传入实参
        Object fzy = constructor.newInstance("fzy");
        System.out.println("fzy=" + fzy);
        //4. 通过非 public 的有参构造器创建实例
        //4.1 得到 private 的构造器对象
        Constructor<?> constructor1 = userClass.getDeclaredConstructor(int.class, String.class);
        //4.2 创建实例
        //暴破【暴力破解】 , 使用反射可以访问 private 构造器/方法/属性, 反射面前，都是纸老虎
        constructor1.setAccessible(true);
        Object user2 = constructor1.newInstance(100, "张三丰");
        System.out.println("user2=" + user2);

    }
}

class User { //User 类
    private int age = 10;
    private String name = "Java学习";

    public User() {//无参 public
    }

    public User(String name) {//public 的有参构造器
        this.name = name;
    }

    private User(int age, String name) {//private 有参构造器
        this.age = age;
        this.name = name;
    }

    public String toString() {
        return "User [age=" + age + ", name=" + name + "]";
    }
}
```



## 通过反射访问类中的成员

### 访问属性

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021859707.png)

```java
import java.lang.reflect.Field;

/**
 * 演示反射操作属性
 */
public class ReflecAccessProperty {
    public static void main(String[] args) throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchFieldException {

        //1. 得到Student类对应的 Class对象
        Class<?> stuClass = Class.forName("reflection_.Student");
        //2. 创建对象
        Object o = stuClass.newInstance();//o 的运行类型就是Student
        System.out.println(o.getClass());//Student
        //3. 使用反射得到age 属性对象
        Field age = stuClass.getField("age");
        age.set(o, 88);//通过反射来操作属性
        System.out.println(o);//
        System.out.println(age.get(o));//返回age属性的值

        //4. 使用反射操作name 属性
        Field name = stuClass.getDeclaredField("name");
        //对name 进行暴破, 可以操作private 属性
        name.setAccessible(true);
        //name.set(o, "老韩");
        name.set(null, "老韩~");//因为name是static属性，因此 o 也可以写出null
        System.out.println(o);
        System.out.println(name.get(o)); //获取属性值
        System.out.println(name.get(null));//获取属性值, 要求name是static

    }
}

class Student {//类
    public int age;
    private static String name;

    public Student() {//构造器
    }

    public String toString() {
        return "Student [age=" + age + ", name=" + name + "]";
    }
}
```

### 访问方法

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208021904233.png)

```java
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

/**
 * 演示通过反射调用方法
 */
public class ReflecAccessMethod {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InstantiationException, InvocationTargetException {

        //1. 得到Boss类对应的Class对象
        Class<?> bossCls = Class.forName("reflection_.Boss");
        //2. 创建对象
        Object o = bossCls.newInstance();
        //3. 调用public的hi方法
        //Method hi = bossCls.getMethod("hi", String.class);//OK
        //3.1 得到hi方法对象
        Method hi = bossCls.getDeclaredMethod("hi", String.class);//OK
        //3.2 调用
        hi.invoke(o, "Java学习~");

        //4. 调用private static 方法
        //4.1 得到 say 方法对象
        Method say = bossCls.getDeclaredMethod("say", int.class, String.class, char.class);
        //4.2 因为say方法是private, 所以需要暴破，原理和前面讲的构造器和属性一样
        say.setAccessible(true);
        System.out.println(say.invoke(o, 100, "张三", '男'));
        //4.3 因为say方法是static的，还可以这样调用 ，可以传入null
        System.out.println(say.invoke(null, 200, "李四", '女'));

        //5. 在反射中，如果方法有返回值，统一返回Object , 但是他运行类型和方法定义的返回类型一致
        Object reVal = say.invoke(null, 300, "王五", '男');
        System.out.println("reVal 的运行类型=" + reVal.getClass());//String


        //在演示一个返回的案例
        Method m1 = bossCls.getDeclaredMethod("m1");
        Object reVal2 = m1.invoke(o);
        System.out.println("reVal2的运行类型=" + reVal2.getClass());//Monster


    }
}

class Monster {
}

class Boss {//类
    public int age;
    private static String name;

    public Boss() {//构造器
    }

    public Monster m1() {
        return new Monster();
    }

    private static String say(int n, String s, char c) {//静态方法
        return n + " " + s + " " + c;
    }

    public void hi(String s) {//普通public方法
        System.out.println("hi " + s);
    }
}
```

### 
