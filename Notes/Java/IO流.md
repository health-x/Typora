# IO流概括和分类

### IO流概括

- IO：输入/输出(Input/Output)

- 流：是一种抽象概念，对数据传输的总称。也就是说数据在设备间传输称为流，流的本质是数据传输。

- IO流就是用来处理设备间数据传输问题的

  ​	常见的应用：文件复制、文件下载、文件上传

### IO流分类

- 按数据流向

  - 输入流：读数据
  - 输出流：写数据

- 按数据类型分

  - 字节流

    ​	字节输入流；字节输出流

  - 字符流

    ​	字符输入流；字符输出流

IO流分类默认按照**数据类型**来分的



如果数据通过Window自带的记事本软件打开，不是乱码，就使用字符流；否则使用字节流。

如果不知道该使用哪种类型的流，就使用字节流。



# 字节流

字节流抽象基类
lnputStream：这个抽象类是表示字节输入流的所有类的超类

OutputStream：这个抽象类是表示字节输出流的所有类的超类

子类名特点:子类名称都是以其父类名作为子类名的后缀

### 字节流写数据

FileOutputStream:文件输出流用于将数据写入File（OutputStream抽象类的子类）
FileOutputStream(String name):创建文件输出流以指定的名称写入文件



```
/*
    FileOutputStream：文件输出流用于将数据写入File
        FileOutputStream(String name)：创建文件输出流以指定的名称写入文件
 */
public class FileOutputStreamDemo {
    public static void main(String[] args) throws IOException {
        /*
        创建字节输出流对象
        做了三件事情
            A:调用系统功能创建了文件
            B:创建了字节输出流对象
            C:让字节输出流对象指向创建好的文件
         */
        FileOutputStream fs = new FileOutputStream("fs.txt");
        
        //void write(int b):将指定的字节写入此文件输出流
        fs.write("Hello".getBytes());
        
        //void close():关闭此输入流并释放与流相关联的任何系统资源。
        fs.close();
    }
}
```



```
写数据的三种方法
    void write(byte[] b)：将 b.length个字节从指定的字节数组写入此文件输出流。
    一次写一个字节数组的数据

    void write(byte[] b, int off, int len)：将 len字节从位于偏移量 off的指定字节数组写入此文件输出流。
    一次写一个字节数组的部分数据

    void write(int b)：将指定的字节写入此文件输出流。
    一次写一个字节的数据
```

##### 字节流写数据时的问题

1. 字节流写数据如何实现换行

   windows:\r\n
   linux:\n
   mac:\r

2. 字节流写数据如何实现追加写入(第二次运行时添加在后面)

   FileOutputStream(File file, boolean append)：创建文件输出流以写入由指定的 File对象表示的文件。
   如果第二个参数是true ，则字节将被写入文件的末尾而不是开头

```
public class FileOutputStreamDemo3 {
    public static void main(String[] args) throws IOException {
        //创建字节输出流对象
        FileOutputStream fos = new FileOutputStream("fos.txt",true);	//实现追加写入

        for (int i = 0; i < 10; i++) {
            fos.write("hello".getBytes());
            fos.write("\r\n".getBytes());   //实现换行
        }
        fos.close();
    }
}
```

##### 字节流写数据加异常处理

```
public class FileOutputStreamDemo4 {
    public static void main(String[] args) {
        FileOutputStream fos = null;
        try {
            fos = new FileOutputStream("fos.txt",true);
            fos.write("hello".getBytes());
        }catch (IOException e){
            e.printStackTrace();
        }finally {
            if (fos!=null) {
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```



### 字节流读数据

FileInputStream:文件输入流用于从文件系统中的文件获取输入字节。（InputStream抽象类的子类）

FileInputStream(String name) ：通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的路径名  name命名。

```
/*
    需求：把文件中的内容读取出来在控制台输出

    使用字节输入流读取数据步骤
        1：创建字节输入流对象
        2：调用字节输入流对象的读数据方法
        3：释放资源
 */
public class FileInputStreamDemo {
    public static void main(String[] args) throws IOException {
        //创建字节输入流对象
        FileInputStream fis = new FileInputStream("fos.txt");
        
    /*
        //int read (byte[] b):从该输入流读取最多b.length个字节的数据到一个字节数组
        int len = fis.read(b);  //实际读取到的字节的长度，读取到末尾的话就会返回-1
    */
        
        //一次读一个字节
        int bys;
        while((bys=fis.read())!=-1){
            System.out.println(bys);
        }

        //一次读取一个字符数组
        byte[] bytes = new byte[1024];
        int lens;
        while((lens=fis.read(bytes))!=-1){
            System.out.println(new String(bytes,0,lens));
        }
        //释放资源
        fis.close();
    }
}
```



### 字节流练习

1. 复制文本文件

   **需求：**
       把 E:\\Java\\1212.txt 复制到模块目录下的“1212.txt”
       数据源:
           E:\\Java\\1212.txt ---读数据--- InputStream --- FiLeInputStream
       目的地:
           IO流\\1212.txt ---写数据--- OutputStream --- FileOutputStream

   思路:
       1:根据数据源创建字节输八流对象
       2:根据目的地创建字节输出流对象
       3:读写数据，复制文本文件(一次读取一个字节，一次写入一个字节)
       4:释放资源

```
public class CopyTxtDemo {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream("E:\\Java\\1212.txt");
        FileOutputStream fos = new FileOutputStream("12.txt");

        //一次读取一个字节
        int by;
        while ((by=fis.read())!=-1){
            fos.write(by);
        }

        /*//一次读一个字节数组
        byte[] bytes = new byte[1024];
        int len;
        while ((len=fis.read(bytes))!=-1){
            fos.write(bytes,0,len);
        }*/
        fos.close();
        fis.close();
    }
}
```

2. 复制图片

   需求：
       把“E:\\picture\\java.jpg”复制到模块目录下的“java1.jpg”

   思路:
       1 :根据数据源创建字节输八流对象
       2:根据目的地创建字节输出流对象
       3:读写数据，复制文本文件(一次读取一个字节数组，一次写入一个字节数组)
       4:释放资源

   ```
   public class CopyJpgDemo {
       public static void main(String[] args) throws IOException{
           FileInputStream fis = new FileInputStream("E:\\picture\\java.jpg");
           FileOutputStream fos = new FileOutputStream("java2.jpg");
           byte[] bytes = new byte[1024];
           int len;
           while((len=fis.read(bytes))!=-1){
               fos.write(bytes,0,len);
           }
           fis.close();
           fos.close();
       }
   }
   ```



## 字节缓冲流

字节缓冲流:
BufferOutputStream:该类实现缓冲输出流。通过设置这样的输出流，应用程序可以向底层输出流写入字节，而不必为写入的每个字节导致底层系统的调用
BufferedlnputStream:创建BufferedInputstream将创建一个内部缓冲区数组。当从流中读取或跳过字节时，内部缓冲区将根据需要从所包含的输入流中重新填充，一次很多字节



构造方法:
字节缓冲输出流: BufferedOutputStream(OutputStream out)字节缓冲输入流: BufferedInputStream(InputStream in)
为什么构造方法需要的是字节流，而不是具体的文件或者路径呢?
字节缓冲流仅仅提供缓冲区，而真正的读写数据还得依靠基本的字节流对象进行操作

```
/*
    字节缓冲流:
        BufferOutputStream
        BufferedInputStream
    构造方法:
        字节缓冲输出流: BufferedOutputStream (OutputStream out)
        字节缓冲输入流:BufferedInputStream (inputStream in)

 */
public class BufferStreamDemo {
    public static void main(String[] args) throws IOException {
        /*
        字节缓冲输出流: BufferedOutputStream (OutputStream out)
        //FileOutputStream fos = new FileOutputStream("12.txt");
        //BufferedOutputStream bos = new BufferedOutputStream(fos);

        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("12.txt"));
        //写数据
        bos.write("hello buffered".getBytes());
        bos.write("haha".getBytes());
        bos.close();
        */

        //字节缓冲输入流:BufferedInputStream (inputStream in)
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream("12.txt"));
        /*//一次读取一个字节
        int bys;
        while((bys=bis.read())!=-1){
            System.out.print((char) bys);
        }*/

        //一次读取一个字节数组
        byte[] bytes = new byte[1024];
        int len;
        while ((len=bis.read(bytes))!=-1){
            System.out.println(new String(bytes,0,len));
        }
    }
```





## 四种方法复制视频

```
package com.health.字节流.练习.复制视频;

import java.io.*;

/*
    需求:
        把E:\\video\\iu.mp4复制到模块目录下的files文件夹下

    思路:
        1:根据数据源创建字节输入流对象
        2:根据目的地创建字节输出流对象
        3:读写数据，复制图片(一次读取一个字节数组，一次写入一个字节数组)
        4:释放资源
    四种方式实现复制视频，并记录每种方式复制视频的时间
        1 :基本字节流一次读写一个字节
        2:基本字节流一次读写一个字节数组
        3:字节缓冲流—次读写一个字节
        4:字节缓冲流一次读写一个字节数组
 */
public class CopyAviDemo {
    public static void main(String[] args) throws IOException {
        //记录开始时间
        long startTime = System.currentTimeMillis();
        //复制视频
        method1();
        //记录结束时间
        long endTime = System.currentTimeMillis();
        System.out.println("共耗时："+(endTime-startTime)+"毫秒");
    }

    //基本字节流，一次读写一个字节    共耗时：121015毫秒
    public static void method1() throws IOException {
        FileInputStream fis = new FileInputStream("E:\\video\\iu.mp4");
        FileOutputStream fos = new FileOutputStream("files\\iu.mp4");
        int bys;
        while((bys=fis.read())!=-1){
            fos.write(bys);
        }
        fis.close();
        fos.close();
    }

    //基本字节流，一次读写一个字节数组  共耗时：172毫秒
    public static void method2() throws IOException {
        FileInputStream fis = new FileInputStream("E:\\video\\iu.mp4");
        FileOutputStream fos = new FileOutputStream("files\\iu.mp4");
        int len;
        byte[] bytes = new byte[1024];
        while((len=fis.read(bytes))!=-1){
            fos.write(bytes,0,len);
        }
        fis.close();
        fos.close();
    }
    //字节缓冲流域，一次读写一个字节   共耗时：408毫秒
    public static void method3() throws IOException {
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream("E:\\video\\iu.mp4"));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("files\\iu.mp4"));
        int bys;
        while((bys=bis.read())!=-1){
            bos.write(bys);
        }
        bis.close();
        bos.close();
    }

    //字节缓冲流域，一次读写一个字节数组 共耗时：31毫秒
    public static void method4() throws IOException {
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream("E:\\video\\iu.mp4"));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("files\\iu.mp4"));
        int len;
        byte[] bytes = new byte[1024];
        while((len=bis.read(bytes))!=-1){
            bos.write(bytes,0,len);
        }
        bis.close();
        bos.close();
    }


}
```



## 编码解码

```
package com.health.编码解码;

import java.io.UnsupportedEncodingException;
import java.util.Arrays;

/*
    编码:
        byte[ ] getBytes():使用平台的默认字符集将该string编码为一系列字节，将结果存储到新的字节数组中
        byte[ ] getBytes(String charsetName)、使用指定的字符集将该String编码为一系列字节，将结果存储到新的字节数组中
    解码:
        String(byte[ ] bytes):通过使用平台的默认字符集解码指定的字节数组来构造新的String
        String(byte[ ] bytes，String charsetName):通过指定的字符集解码指定的字节数组来构造新的 string
    注意：编码和解码要使用同一种字符集

 */
public class StringDemo {
    public static void main(String[] args) throws UnsupportedEncodingException {
        //定义一个字符串
        String s = "中国";

        //byte[ ] getBytes():使用平台的默认字符集将该string编码为一系列字节，将结果存储到新的字节数组中
        byte[] bys = s.getBytes();    //[-28, -72, -83, -27, -101, -67]
        //byte[] bys = s.getBytes("UTF-8");   //[-28, -72, -83, -27, -101, -67]
        //byte[] bys = s.getBytes("gbk"); //[-42, -48, -71, -6]
        System.out.println(Arrays.toString(bys));

        //解码：String(byte[ ] bytes):通过使用平台的默认字符集解码指定的字节数组来构造新的String
        String ss = new String(bys);

        //String(byte[ ] bytes，String charsetName):通过指定的字符集解码指定的字节数组来构造新的 string
        //String ss = new String(bys,"utf-8");
        //String ss = new String(bys,"gbk");
        System.out.println(ss);

    }
}
```







# 字符流

字符流抽象基类

- Reader:字符输入流的抽象类
- Writer:字符输出流的抽象类

具体子类：

1. FileReader：字符输入流
2. FileWriter：字符输出流

1. Reader的常用方法
   1. close();
   2. read();
   3. read(char[] arr);
2. Writer的常用方法
   1. close();
   2. flush();
   3. write(int c)：写出一个字符
   4. write(String str):写出一个字符串
   5. write(char[] arr):
   6. wirte(char[] arr, int offset,int len)：len是个数



## 字符缓冲流

使用类似字节缓冲流

- BufferedReader：字符缓冲输入流

- BufferedWriter：字符缓冲输出流



字符缓冲流的特有的方法

- BufferedReader：
  - readLine()：可以从输出流中，一次读取一行文本，返回一个String对象，读取到为文件末尾的时候返回null

- BufferedWriter：
  - newLine():写出一个换行符



# 转换流

1、GBK：国标码，英文占一个字节，中文两个字节

2、UTF-8 万国码表，定义了全球所有语言的符号，英文占一个字节，中文占三个字节



转换流

1、InputStreamReader:是字节流转向字符流的桥梁，可以指定转换时使用的字符集

- InputStreamReader(InputStream is): 将指定的字节流is转为字符流。返回值：获取的是InputStreamReader对象，是Reader子类
- InputStreamReader(InputStream is, String charsetName):将指定字节流转为字符流，并指定读取时采用的编码集

2、OutputStreamWriter:是字节流转向字符流的桥梁，可以指定转换时的字符集

- OutputStreamWriter(OutputStream os) : 将字节流转为字符流，是Writer的子类
- OutputStreamWriter(OutputStream os, String charsetName):将字节流转为字符流，同时写出指定的字符集   