[TOC]

## Java的I/O标准库类

### File

主要用于处理目录问题

* 目录列表器

  通过对`File`对象调用`list()`方法，该方法有两种使用方式，一个不带参数，返回所有文件名的数组，一个带参数`FileNameFilter`，返回过滤后的文件数组。

* 目录使用工具

* 目录检查及创建

  `getAbsolutePath、canRead、canWrite、getName、getParent、getPath、length、lastModified、isFile、isDirectory、mkdirs`

**注意:**`mkdir`用于创建一级目录，`mkdirs`创建多级目录

### RandomAccessFile

java提供的对文件内容的访问，既可以读文件，也可以写文件。

Java文件模型在硬盘上的文件时字节存储的，是数据的集合，通过`RandomAccessFile`打开文件有两种模式==rw和r==。

## 字节读取（InputStream\OutputStream）

`InputStream\OutputStream`是基础类，通过装饰者模式可以在该类的基础上进行扩展。

### FileInputStream\FileOutputStream

对文件内容进行读取，每次只读取一个字节，也就是8位。

### DataInputStream\DataOutputStream

对`FileInputStream\FileOutputStream`进行装饰，对单个字节的读取进行了封装，能够多字节进行读取

### BufferInputStream\BufferOutputStream

对`FileInputStream\FileOutputStream`进行装饰，通过对字节读取缓冲，大幅度提示IO性能。

## 字符读取（Writer\Reader）

### InputStreamWriter\InputStreamReader

对字节和字符进行转换，但是要注意编码格式

### BufferReader\BufferWriter

带缓冲的字符读取

### PrintWriter\PrintReader

更容易读取字符



## IO和NIO的区别

| IO     | NIO        |
| ------ | ---------- |
| 面向流 | 面向缓冲区 |
| 阻塞式 | 非阻塞时   |
| 无     | 选择器     |

### NIO

* 通道

  通道只是打开将要传输的两个目标之间的连接，不是数据传输存放的地方 

  1. 主要实现类
     * FileChannel
     * SocketChannel
     * ServerSocketChannel
     * DatagramChannel
  2. 获取通道
     * 通过getChannel()方法
     * 静态方法open()
     * Files工具类的newByteChannel()
  3. 通道之间的数据传输
     * transferFrom()
     * transferTo()

* 缓冲区

  数据存放在缓冲区并通过通道进行传输

  1. 缓冲区类型

     ![image-20200304100131319](C:\Users\87360\AppData\Roaming\Typora\typora-user-images\image-20200304100131319.png)

  2. 缓冲区空间申请

     通过每个缓冲区类型的静态方法`allocate`

  3. Buffer中重要的四个属性

     > mark:标记，表示记录当前Position的位置。可以通过reset()恢复到mark的位置
     >
     > position:当前位置
     >
     > limit:可操作性容量，确定后不可改变
     >
     > capacity:最大容量，确定后不可改变

  4. 缓冲区的读取

     ![image-20200304101641580](C:\Users\87360\AppData\Roaming\Typora\typora-user-images\image-20200304101641580.png)

     (1)通过`allocate`申请空间，此时`position=0,capcity=limit=空间大小`

     (2)`put`方法存入数据，此时`position`会随着存入的数据大小变化

     (3)通过`flip()`切换为读模式，`position归0,limit`变为数据大小

     (4)通过`rewind`重复读

     (5)通过`clear`清空缓冲区，但是处于被遗忘状态

* 直接缓冲区与非直接缓冲区

  通过`allocate`创建的是非直接缓冲区。

  通过` allocateDirect`创建直接缓冲区。
  
* 分散读取和聚集写入

  分散读取就是把一个通道的数据依次写入多个缓冲区；聚集写入就是把多个缓冲区数据写入同一个通道。 

* 阻塞和非阻塞

  NIO通过选择器（Selector）进行通道注册以及监控。
  
  

