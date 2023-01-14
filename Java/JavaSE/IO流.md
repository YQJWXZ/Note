# IO流
存储和读取数据的解决方案


## 1. 分类：
* 流的方向：
  * 输入流：文件 --> 程序
  * 输出流：程序 --> 文件
* 操作文件类型：
  * 字节流：可以操作所有类型的文件
  * 字符流：只能操作纯文本文件
     * 字符流=字节流+字符集


## 2. 体系
```
   字节流：
     InputStream:
     OutputStream:
   字符流：
     Reader:
     Writer:
   
```
``
IO流创建原则： 随用随创建    什么时候不用什么时候关闭
``
## 3. 步骤
1. 创建字节/字符输出流/输入流对象：
``
文件存在、文件不存在、追加写入
``
```
 // 细节1：参数是字符串表示的路径或者是File对象
 // 细节2：如果文件不存在会创建一个新的文件，但是要保证父级路径是存在的
 // 细节3：如果文件已经存在，则会清空文件
```
2. 读/写数据：
``
 写出整数、写出字节数组、换行写
``
```
// write方法的参数是整数，但是实际上写到本地文件中的是整数在ASCII上对应的字符
```
```java
  // bw.write(count+"");   这里写出去的是数字，而不是数字对应的字符
```
```java
        // 循环读取

        /*
        *  read: 表示读取数据，而且是读取一个数据就移动一次指针
         */

        int b;
        while((b= fis.read())!=-1){
            System.out.print((char)b);
        }
```
3. 释放资源
``
异常处理
try{
}catch{
}finally{
}
``
  * 被finally控制的语句一定会执行，除非JVM退出
## 4. 乱码问题
1. ASCII

2. GBK

3. Unicode
``
UTF-8是Unicode字符集的一种编码方式，不是字符集
``
   * 一个英文占一个字节，二进制第一位是0，转成十进制是正数 
   * 一个中文占三个字节，二进制第一位是1，第一个字节转成十进制是负数

##### 出现乱码问题原因：
1. 读取数据时未读完整个汉字
2. 编码和解码时的方式不统一 
``
   使用指定方式进行编解码
``
```java
        // 1.编码
        String str="ai你哟";
        byte[] bytes1=str.getBytes();
        System.out.println(Arrays.toString(bytes1));  // [97, 105, -28, -67, -96, -27, -109, -97]

        byte[] bytes2 = str.getBytes("GBK");   // [97, 105, -60, -29, -45, -76]
        System.out.println(Arrays.toString(bytes2));


        // 2. 解码
        String str2=new String(bytes1);     // str --> str2   UTF-8 --> UTF-8
        System.out.println(str2);   // ai你哟

        String str3=new String(bytes1,"GBK");   // str --> str3     UTF-8 --> GBK
        System.out.println(str3);   // ai浣犲摕
```
## 5.字符流原理解析
#### 字符输入流：Filereader

1. 创建字符输入流
  * 底层： 关联文件，并创建缓冲区（长度为8192的字节数组）

2. 读取数据
  * 底层： 
    * 判断缓冲区中是否有数据可以读取
    * 缓冲区没有数据： 
      * 就从文件中获取数据，装到缓冲区中，每次尽可能装满缓冲区
      * 如果文件中也没有数据了，返回-1
    * 缓冲区有数据：`` 就从缓冲区中读取 ``
      * 空参的read方法：一次读取一个字节，遇到中文一次读多个字节，把字节解码并转成十进制返回
      * 有参的read方法：把读取字节，解码，强转三步合并了，强转之后的字符放到数组中
    * 缓冲区读完后就会返回到文件看是不是还有没有没被读取的，没有的话就返回-1

#### 字符输出流：FileWriter
情况一： 装满了

情况二： flush

情况三： 释放资源

## 6.缓冲流

字节缓冲流：
  * BufferedInputStream
  * BufferedOutputStream

字符缓冲流： 
  * BufferedReader
      * readLine() : 读取一行数据，如果没有数据可读了，会返回null  (独有方法)
  * BufferedWriter
      * newLine():   跨平台的换行  (独有方法)

#### 字节缓冲流提高效率的原理
输入与输出的缓冲区不是同一个

## 7.转换流
是字符流和字节流之间的桥梁 ``字节流想要使用字符流中的方法``
* InputStreamReader
* OutputStreamWriter

## 8.序列化流与反序列化流
#### 序列化流/对象操作输出流
可以把Java中的对象写到本地文件中
* ObjectOutputStream():基本流包装成高级流
  * writeObject(): 把对象序列化（写出）到文件中去
```java
   //序列化流中的小细节：
        // 使用对象输出流将对象保存到文件时会出现NotSerializableException异常

   //解决方案：
        // 需要让Javabean类实现Serializable接口

/**
 * Serializable接口里面是没有抽象方法，标记型接口
 * 一旦实现了这个接口，那么就表示当前的Student类可以被序列化
 *
 * 可以这么理解：
 *          一个物品的合格证
 */
```
#### 反序列化流/对象操作输入流
可以把序列化到本地文件中的对象，读取到程序中来
* ObjectInputStream():把基本流变成高级流
  * readObject(): 把序列化到本地文件中的对象，读取到程序中来
```java
// 文件中的版本号，与Javabean的版本号不匹配
// 方法一：
// private static final long serialVersionUID=1L;
// 方法二：
// 1.Settings --> 搜索Serializable --> 勾选Serializable class without 'serialVersionUID'  &&  Transient field is not initialized on deserialization
// 2.鼠标移到Student会给出提示，然后Alt+回车，自动添加
```
```java
    // 如果有一个成员变量的值不想序列化到本地
    //transient: 瞬态关键字
    // 作用：不会把当前属性序列化到本地文件当中
    private transient String address;
```
### 序列化流/反序列化流的细节汇总
1. 使用序列化流将对象写到文件时，需要让Javabean类实现Serializable接口，否则，会出现NotSerializableException异常
2. 序列化流写到文件中的数据是不能修改的，一旦修改就无法再次读回来了
3. 序列化对象后，修改了Javabean类，再次被反序列化，会不会有问题？
   * 会出问题，会抛出InvalidClassException异常
   * 解决方案：给Javabean类添加serialVersionUID(序列号、版本号)
4. 如果一个对象中的某个成员变量不想被序列化，又该如何实现呢？
   * 解决方案：给该成员变量加transient关键字修饰，该关键字标记的成员变量不参与序列化过程
```java
// 序列化流
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;
import java.util.ArrayList;

public class ObjectStreamExpress01 {

    public static void main(String[] args) throws IOException {

        // 用对象流读写多个对象

        // 需求：
                // 将多个自定义对象序列化到文件中，但是由于对象的个数不确定，反序列化流该如何读取


        // 1.序列化多个对象
        Student s1 = new Student("zhangsan", 23, "南京");
        Student s2 = new Student("lisi", 24, "重庆");
        Student s3 = new Student("wangwu", 25, "北京");

        ArrayList<Student>  list=new ArrayList<>();
        list.add(s1);
        list.add(s2);
        list.add(s3);

        ObjectOutputStream oos=new ObjectOutputStream(new FileOutputStream("myIO\\a.txt"));
        oos.writeObject(list);


        oos.close();
    }
}

```
```java
// 反序列化流
import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.util.ArrayList;

public class ObjectStreamExpress02 {

    public static void main(String[] args) throws IOException, ClassNotFoundException {

        // 1.创建反序列化流的对象
        ObjectInputStream ois=new ObjectInputStream(new FileInputStream("myIO\\a.txt"));

        // 2.读取数据
        ArrayList<Student> list=(ArrayList<Student>) ois.readObject();

        for (Student student : list) {
            System.out.println(student);
        }
        // 3.释放资源
        ois.close();
    }
}

```
## 9.打印流
打印流只有写，没有读
打印流一般是指：PrintStream,PrintWriter两个类

特点一：打印流只操作文件目的地，不操作数据源
特点二：特有的写出方法可以实现，数据原样写出
特点三：特有的写出方法，可以实现自动刷新，自动换行
   * 打印一次数据=写出+换行+刷新

``
    字节流底层没有缓冲区，开不开自动刷新都一样
``

``
    字符流底层有缓冲区，想要自动刷新需要开启
``
## 10.解压缩流/压缩流
解压缩流：

``
解压本质：把每一个ZipEntry按照层级拷贝到本地另一个文件夹中(zipentry对象)
``
```java
import java.io.*;
import java.util.zip.ZipEntry;
import java.util.zip.ZipInputStream;

public class ZipStreamDemo01 {
    public static void main(String[] args) throws IOException {

        // 1.创建一个File表示要解压的单压缩包
        File src=new File("D:\\aaa.zip");

        // 2.创建一个File表示解压的目的地
        File dest=new File("D:\\");

        // 调用方法
        unzip(src,dest);

    }

    // 定义一个方法用来解压
    public static void unzip(File src,File dest) throws IOException {
        // 解压的本质：把压缩包里面的每一个文件或者文件夹读取出来，按照层级拷贝到目的地当中

        // 创建一个解压缩流用来读取压缩包中的数据
        ZipInputStream zip=new ZipInputStream(new FileInputStream(src));

        // 要先获取到压缩包里面的每一个zipentry对象

        // 表示当前在压缩包中获取到的文件或者文件夹
        ZipEntry entry;
        while((entry=zip.getNextEntry())!=null){

            if(entry.isDirectory()){
                // 文件夹： 需要在目的地dest处创建一个同样的文件夹
                File file=new File(dest,entry.toString());
                file.mkdirs();
            }else {
                // 文件： 需要读取到压缩包中的文件，并把他存放在目的地dest文件夹中(按照层级目录进行存放)
                FileOutputStream fos=new FileOutputStream(new File(dest,entry.toString()));
                int b;
                while((b=zip.read())!=-1){
                    // 写到目的地
                    fos.write(b);
                }
                fos.close();
                // 表示在压缩包中的一个文件处理完毕了
                zip.closeEntry();
            }
        }
        zip.close();
    }
}

```
压缩流：
``
压缩本质：把每一个（文件/文件夹）看成ZipEntry对象放到压缩包中
``
```java
import java.io.*;
import java.util.zip.ZipEntry;
import java.util.zip.ZipOutputStream;

public class ZipStreamDemo03 {
    public static void main(String[] args) throws IOException {

        /**
         * 压缩流：  压缩文件夹
         *     需求：
         *            把D:\\aaa文件夹压缩成一个压缩包
         */

        // 1. 创建File对象表示要压缩的文件夹
        File src=new File("D:\\aaa");

        // 2. 创建File对象表示压缩包放在哪里（压缩包的父级路径）
        File destParent=src.getParentFile();   // D:\\

        // 3. 创建File对象表示压缩包的路径
        File dest=new File(destParent,src.getName()+".zip");

        // 4. 创建压缩流关联压缩包
        ZipOutputStream zos=new ZipOutputStream(new FileOutputStream(dest));

        // 5. 获取src里面的每一个文件，变成ZipEntry对象，放入到压缩包中
        tozip(src,zos,src.getName());   // 难点：src.getName() --> \\aaa

        // 6. 释放资源
        zos.close();

    }

    /**
     * 作用： 获取src里面的每一个文件，变成ZipEntry对象，放入到压缩包中
     * 参数一： 数据源
     * 参数二： 压缩流
     * 参数三： 压缩包内部的路径
     */
    public static void tozip(File src,ZipOutputStream zos,String name) throws IOException {
        // 1.进入src文件夹
        File[] files=src.listFiles();

        // 2.遍历数组
        for (File file : files) {
            if(file.isFile()){
                // 判断：是文件，变成ZipEntry对象。放入到压缩包当中
                ZipEntry entry=new ZipEntry(name+"\\"+file.getName());  // 重难点：要的aaa\\xxx,而不是D:\\aaa\\xxxx
                zos.putNextEntry(entry);

                // 读取文件中的数据，写到压缩包
                FileInputStream fis=new FileInputStream(file);
                int b;
                while ((b=fis.read())!=-1){
                    zos.write(b);
                }

                fis.close();
                zos.closeEntry();

            }else{
                // 判断：是文件夹，递归
                tozip(file,zos,name+"\\"+file.getName());
            }
        }
    }
}
```
## 11.常用工具包
### Commons-io
作用：提高IO流的开发效率

FileUtils类：
  * copyFile(File srcFile,File destFile)
  * copyDirectory(File srcDir,File destDir)
  * copyDirectoryToDirectory(File srcDir,File destDir)
  * deleteDirectory(File directory)
  * cleanDirectory(File directory)
  * readFileToString(File file,charset encoding)
  * write(File file,CharSequence data,String encoding)

IOUtils类：
  * int copy(InputStream input,OutputStream output)
  * int copyLarge(Reader input,Writer output)
  * String readLine(Reader input)
  * write(String data,OutputStream output)

### Hutool糊涂包
难得糊涂！

FileUtil类：
  * file：根据参数创建一个file对象
  * touch: 根据参数创建文件
  * writeLines: 把集合中的数据写出到文件中，覆盖模式
  * appendLines: 把集合中的数据写出到文件中，续写模式
  * readLines: 指定字符编码，把文件中的数据，读到集合中
  * readUtf8Lines: 按照UTF-8的形式，把文件中的数据，读到集合中
  * copy
```java
 // 糊涂包的相对路径，不是相对于当前项目而言的，而是相对于class文件而言
```
