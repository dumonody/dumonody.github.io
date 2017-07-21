---
title: IO
---

> ## File（操作文件）

硬盘中的文件，无论是图片文件、视频文件、文本文件，其中的内容都是以**二进制**存储的！！！

使用看图工具打开图片文件、播放器打开视频文件、文本编辑器打开文本文件器，
其实都是以指定编码的格式解码二进制才出现了对应的图片、视频、文字！！！

数据**持久化**到硬盘上的体现：**文件**

### 构造&分隔符
```java
import java.io.File;

public class FileDemo {

	public static void main(String[] args)
	{
		
//		将demoTest封装成File对象
//		demoTest可以是[文件]，也可以是[文件夹]
//		demoTest可以[存在]，也可以[不存在]
//		都可以封装成一个File对象
//		[字符串路径]中的斜杠分隔符可以是单个正斜杠/,或者两个反斜杠\\（由于转义，所以反斜杠需要两个）
		 
		
		String pathName = "C:\\Users\\czkct\\Desktop\\demoTest";
		File f1 = new File(pathName);
		System.out.println(f1);	// 输出的是“C:\Users\czkct\Desktop\demoTest”
		

		String parent = "C:\\Users\\czkct\\Desktop";
		String child = "demoTest";
		File f2 = new File(parent, child);
		System.out.println(f2);	// 输出的是“C:\Users\czkct\Desktop\demoTest”
		
		
		File f3 = new File(parent);
		File f4 = new File(f3, child);
		System.out.println(f4);	// 输出的是“C:\Users\czkct\Desktop\demoTest”
		
		
//		路径的跨平台写法：使用默认名称分隔符File.separator
		String pathName2 = "czkct" + File.separator + "Desktop" + File.separator + "demoTest";
		System.out.println(pathName2);	// 输出的是“czkct\Desktop\demoTest”
	}
}

```
### 常用属性获取
```java
import java.io.File;
import java.sql.Date;
import java.text.DateFormat;

public class FileMethodDemo {

	public static void main(String[] args) {

		File f = new File("src/com/duyanhan/io/a/file/FileMethodDemo.java");
		// 获取文件对象的绝对路径
		System.out.println(f.getAbsolutePath());	// D:\MyCode\Workspace_JavaSE_Project\JavaIO\src\com\duyanhan\io\a\file\FileMethodDemo.java
		
		// 获取File对象中封装的路径，封装进去的是什么路径，获取的就是什么路径
		System.out.println(f.getPath());
		
		// 获取文件名
		System.out.println(f.getName());	// FileMethodDemo.java
		
		// 获取文件最后的修改时间   先获取毫秒数
		long time = f.lastModified();
		// 再将毫秒数转换成Date类型，然后格式化成日期字符串
		String str_date = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG).format(new Date(time));
		System.out.println(str_date);
		
		// 获取文件字节数
		System.out.println(f.length());
		
	}

}
```
### 对文件&文件夹的操作
```java
import java.io.File;
import java.io.IOException;

public class FileOpDemo {

	public static void main(String[] args) {
		
		try {
			File f = new File("C:\\test.abc");
			
//		创建文件
//			前提是：抽象路径名指向的文件(夹)不存在，才会返回true
			boolean b1 = f.createNewFile();
			System.out.println("创建"+(b1==true?"":"不")+"成功！");
			
		
//		判断文件是否存在
//			前提是：文件(夹)存在，才会返回true
			boolean b3 = f.exists();
			System.out.println("文件(夹)"+(b3==true?"":"不")+"存在！");
			
			
//		删除文件
//			前提是：文件存在，且不被占用，才会返回true
//			注意：删除的文件不会进入回收站，需要谨慎！！！
			boolean b2 = f.delete();
			System.out.println("删除"+(b2==true?"":"不")+"成功！");
		
//		对目录操作：创建、删除、判断是否存在
			File dir = new File("E:\\aa\\bb\\cc\\dd");
//		创建目录
			boolean b4 = dir.mkdirs();	// 代表在指定路径E:\\aa\\bb\\cc下创建dd目录
			boolean b5 = dir.mkdirs();	// 代表创建aa、bb、cc、dd多级目录，推荐使用这种
			System.out.println(b4);
			System.out.println(b5);
			
//		删除文件夹
//			前提是：待删除的目录中为空，没其它内容，才可以删除，返回true
			boolean b6 = dir.delete();
			System.out.println("删除"+(b6==true?"":"不")+"成功！");
			
//		判断是文件还是文件夹
//			前提是：文件(夹)已经存在，才可以判断
//			注意：这个很重要，不要认为有扩展名的就是文件，没有扩展名的就一定是目录
			File f2 = new File("E:\\java.txt");
			f2.mkdir();
			if(f2.exists())
			{
				System.out.println(f2.isFile());
				System.out.println(f2.isDirectory());
			}
			
		} catch (IOException e) {
			e.printStackTrace();
		}
		
	}

}

```
### 获取指定目录下的所有内容
```java
import java.io.File;
import java.text.DateFormat;
import java.util.Date;

public class FileOpDemo2 {

	public static void main(String[] args) {

	// 获取一个目录下的所有内容的两种方式：
		// 注意：必须已经存在，且必须是目录，否则会有空指针异常！！！
		File dir = new File("C:\\Program Files");
		
	// 健壮性判断！！
		if(dir.exists() && dir.isDirectory())
		{
			// 如果只要此目录下的所有内容的名称：
			String[] names = dir.list();
			for(String name : names)
			{
				System.out.println(name);
			}
			
			
			// 如果要此目录下的所有内容的对象：
			// （更推荐这种，因为有了对象，可以对每个对象进行更多操作）
			File[] files = dir.listFiles();
			for(File file : files)
			{
				System.out.print(file + "   ");
				System.out.println(DateFormat.getDateTimeInstance().format(new Date(file.lastModified())));
			}
		}
	}

}
```
### 获取目录中满足条件的内容
#### 一、FileNameFilter接口：获取目录中，在名称上满足指定条件的内容

**下面以获取指定目录中.java文件为例：**
##### 1.先实现FileNameFilter这个过滤器接口
```java
import java.io.File;
import java.io.FilenameFilter;

public class JavaFileFileter implements FilenameFilter {

	@Override
	public boolean accept(File dir, String name) {
		return name.endsWith(".java");
	}
}
```
##### 2.使用实现了过滤器接口的实例
```java
import java.io.File;

public class FileOpDemo3 {

	public static void main(String[] args) {

		File dir = new File("C:\\Program Files");
		
		if(dir.exists() && dir.isDirectory())
		{
			File[] files = dir.listFiles(new JavaFileFileter());
			for(File f : files)
			{
				System.out.println(f);
			}
		}
		
	}
}

```
##### 3.总结:更灵活的写法
```java
import java.io.File;
import java.io.FilenameFilter;

public class FilenameFilterBySuffix implements FilenameFilter {

	private String suffix;

	public FilenameFilterBySuffix(String suffix) {
		this.suffix = suffix;
	}

	@Override
	public boolean accept(File dir, String name) {
		return name.endsWith(suffix);
	}
}
```
```java
import java.io.File;

public class FileOpDemo4 {

	public static void main(String[] args) {

		File dir = new File("C:\\Program Files");
		
		if(dir.exists() && dir.isDirectory())
		{
			File[] files = dir.listFiles(new FilenameFilterBySuffix(".java"));
			for(File f : files)
			{
				System.out.println(f);
			}
		}
		
	}
}
```
#### 二、FileFilter接口：获取目录中，在文件上满足指定条件的内容

**下面以获取指定目录中所有目录为例：**
##### 1.先实现FileFilter这个过滤器接口
```java
import java.io.File;
import java.io.FileFilter;

public class FileFilterByDir implements FileFilter {

	@Override
	public boolean accept(File pathname) {
		return pathname.isDirectory();
	}

}
```
##### 2.使用实现了过滤器接口的实例
```java
import java.io.File;

public class FileOpDemo5 {

	public static void main(String[] args) {

		File dir = new File("C:\\Program Files");
		
		if(dir.exists() && dir.isDirectory())
		{
			File[] files = dir.listFiles(new FileFilterByDir());
			for(File f : files)
			{
				System.out.println(f);
			}
		}
		
	}
}
```
### 递归获取指定目录下所有内容
```java
import java.io.File;

public class FileOpDemo6 {

	public static void main(String[] args) {

		File dir = new File("D:\\MySoft\\MyBlog\\myhexo\\source");
		
		if(dir.exists() && dir.isDirectory())
		{
			listAllFilesByCurrentDir(dir);
		}
		
	}
	
	public static void listAllFilesByCurrentDir(File dir)
	{
		// 打印文件夹名称
		System.out.println(dir);
		File[] files = dir.listFiles();
		for(File file : files)
		{
			if(file.isDirectory())
			{
				listAllFilesByCurrentDir(file);
			}
			else 
			{
				System.out.println(file);
			}
		}
	}
}
```

### 队列获取指定目录下所有内容
**递归有时候可能会因为目录层次太深导致栈溢出，这时可以使用队列**
- 利用队列，层次遍历目录下的所有内容
- **注意：宽度优先搜索也是利用队列，进行层次遍历**

```java
package com.duyanhan.io.a.file;

import java.io.File;
import java.util.LinkedList;

public class FileOpDemo7 {

	public static void main(String[] args) {

		File dir = new File("D:\\MySoft\\MyBlog\\myhexo\\source");
		
		// 先定义一个空的队列
		Queue<File> queue = new Queue<File>(new LinkedList<File>());
		
		// 健壮性判断，dir必须存在，且是个目录
		if(dir.exists() && dir.isDirectory())
		{
			// 将当前目录放入队列中
			queue.add(dir);
			// 如果队列不为空
			while(!queue.isEmpty())
			{
				// 获取队列头部元素(获取但不删除),必定是个文件夹
				File cDir = (File) queue.getTop();
				// 输出这个目录名：
				System.out.println("目录 ： " + cDir.getName());
				// 获取这个文件夹下的所有内容，如果是文件夹就添加到队尾，如果是文件，就输出其名称
				File[] files = cDir.listFiles();
				for(File f : files)
				{
					if(f.isDirectory())
					{
						queue.add(f);
					}
					else {
						System.out.println(f.getName());
					}
				}
				// 然后删除头部元素
				queue.delete();
			}
		}
	}
	
}

/**
 * 定义一个队列
 * @author Pro.Du
 *
 * @param <E>
 */
class Queue<E> {
	// 利用LinkedList来实现队列
	private LinkedList<E> link = null;
	
	// 有参构造
	public Queue(LinkedList<E> link){
		this.link = link;
	}
	
	// 队列添加元素操作
	public void add(E e)
	{
		// 添加元素至队列尾部
		this.link.addLast(e);
	}
	
	// 获取队列头部元素
	public E getTop()
	{
		return this.link.getFirst();
	}
	
	// 队列删除元素操作
	public void delete()
	{
		// 删除队列头部元素
		this.link.removeFirst();
	}
	
	// 队列元素判空操作
	public boolean isEmpty()
	{
		return this.link.isEmpty();
	}
}
```
> ## 流（操作文件中的数据）
- **注意：流的输入和输出方向是针对内存而言的**
- **从内存到文件，是输出；从文件到内存，是输入。**

### 字节流

#### OutputStream：所有输出字节流类的超类
- 操作的数据都是字节
- 定义了输出字节流的基本共性功能
- 本身是个抽象类，不能直接new出一个实例
- 输出流中定义的写方法都是write
 - void write()方法：将指定字节写入输出流
 - void write(byte[] b)方法：将 b.length 个字节从指定 byte 数组写入输出流中
- **规律：它的所有子类名称后缀是OutputStream，子类的名称前缀代表功能**

##### 将数据写入到文件中
**下面以FileOutputStream为例：将数据写入到文件中**
FileOutputStream后缀是OutputStream，意味着从内存写出
前缀是File，意味着功能是对文件操作，所以**FileOutputStream就是从内存写出到文件的输出流**
```java
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamDemo {

	public static void main(String[] args) throws IOException {

		// 创建一个临时目录对象
		File dir = new File("testDir");
		if(!dir.exists())
		{
			dir.mkdirs();	// 如果目录不存在，则临时创建
		}
		
		// 在上面的临时目录下创建一个测试文件
		File testFile = new File(dir, "testFile");
		
		// 为testFile文件创建文件输出流（注意：从内存流向文件）
		FileOutputStream fos = new FileOutputStream(testFile);
		// 获取内存中的数据
		byte[] data = "内存中的数据".getBytes();
		// 将内存中的数据借助testFile文件的文件输出流写入到testFile文件中
		fos.write(data);
		
		// 关闭资源
		fos.close();
	}
}
```
##### 将数据续写入到已有文件中（以及续写换行符）
如果仍然使用new FileOutputStream(File file)的方式获取到file的输出字节流，
那么调用write方法后写入的内容会覆盖file文件中已有的内容
查找jdk文档，发现有一个**FileOutputStream(File file, boolean append)构造方法**，
这个方法的**第二个参数指定为true时**，意味着file可以追加内容，
**学会查找JDK文档！！！**
```java
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamDemo2 {

	// 定义一个可以跨平台的换行符常量
	private static final String LINE_SEPARATOR = System.getProperty("line.separator");

	public static void main(String[] args) throws IOException {

		File dir = new File("testDir");
		if(!dir.exists())
		{
			dir.mkdirs();	// 如果目录不存在，则临时创建
		}
		
		File testFile = new File(dir, "testFile");
		
		
		// 注意此处的构造多了一个是否允许追加内容的参数
		FileOutputStream fos = new FileOutputStream(testFile, true);
		
		
		byte[] data = "追加的内容".getBytes();
		
		fos.write(data);
		
		// 注意此处：向已有文件中追加换行的写法
		// 这里不用\r\n或者\n，因为前者是windows下的换行，后者是unix下的换行，为了跨平台使用如下写法
		byte[] data2 = (LINE_SEPARATOR + "换行后追加的内容").getBytes();
		
		fos.write(data2);
		
		fos.close();
	}
}
```
#### IO异常的处理
使用try/catch块处理IO异常
- **习惯将流的引用，声明定义初始化在try/catch块的外面，在try里面进行流对象的重新赋值**
- **关闭流对象时，如果出现异常通常建议转换成运行时异常**，让程序停掉，意味着调用系统资源时出现了意外情况
- 假设流对象初始化时由于文件不存在而导致流对象为null，则关闭流对象时会出现空指针异常
- 所以**习惯在关闭流对象时，先判断流对象是否为空，不为空才关闭流对象**

```java
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamDemo3 {

	public static void main(String[] args){

		File testFile = new File("testDir\\testFile");
		
		// 在try/catch块外面声明定义初始化流对象
		FileOutputStream fos = null;
		try {
			// 在try块里面给流对象重新赋值
			fos = new FileOutputStream(testFile);
			byte[] data = "内容".getBytes();
			fos.write(data);
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			// 判断流对象是否为空
			if(fos!=null)
			{
				try {
					fos.close();
				} catch (IOException e) {
					// 转换成运行时异常
					throw new RuntimeException();
				}
				fos = null;
			}
		}
	}
}
```
#### InputStream：所有输入字节流类的超类
- 操作的数据都是字节
- 定义了输入字节流的基本共性功能
- 本身是个抽象类，不能直接new出一个实例
- 输出流中定义的读方法都是read
 - int read()方法：读取一个**字节的值**并返回，如果没有读取到，则返回-1
 - int read(byte[] b)方法：读取一定量的字节数，并存储到字节数组中，返回读取到的**字节数**
- **规律：它的所有子类名称后缀是InputStream，子类的名称前缀代表功能**

##### 将文件中的数据读取到内存中
**下面以FileInputStream为例：将文件中的数据读取到内存中**
FileInputStream后缀是InputStream，意味着从外面读入到内存
前缀是File，意味着功能是对文件操作，所以**FileInputStream就是从文件读入到内存中的文件输入流**

```java
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class FileInputStreamDemo {

	public static void main(String[] args)
	{
		File testFile = new File("testDir\\testFile");
		
		FileInputStream fis = null;
		try {
			// 将 字节读取流 与 数据源 关联起来
			fis = new FileInputStream(testFile);
			
			// 每次读取一个字节并且输出
			int b = 0;
			while((b = fis.read())!=-1)
			{
				System.out.println(b);
			}
			
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			if(fis != null)
			{
				try {
					fis.close();
				} catch (IOException e) {
					throw new RuntimeException();
				}
				fis = null;
			}
		}
	}
}
```
上面这种是一次从文件中读取一个字节到内存中，利用read()
下面这种是一次从文件中读取多个字节到缓冲数组中，利用read(byte[] b)
```java
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class FileInputStreamDemo2 {

	public static void main(String[] args)
	{
		File testFile = new File("testDir\\testFile");
		
		FileInputStream fis = null;
		try {
			// 将 字节读取流 与 数据源 关联起来
			fis = new FileInputStream(testFile);
			
			// 创建一个缓冲字节数组   习惯用1024Byte   刚好是1M， 当然可以更大，一般都是1024的整数倍
			// 例如：2048、 4096、等等   直接算出来，不要写成2*1024、 4*1024 这样不好！！！
			byte[] buf = new byte[1024];
			
			// 读取内容到缓冲字节数组中
			int len = 0; 
			while((len = fis.read(buf))!=-1)
			{
				System.out.println(new String(buf, 0, len));
			}

		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			if(fis != null)
			{
				try {
					fis.close();
				} catch (IOException e) {
					throw new RuntimeException();
				}
				fis = null;
			}
		}
	}
}
```
**后一种写法比前一种写法效率更高！！！**
- **举个例子：如果是要读取一个视频文件，前一种写法，每读取一个字节都要循环一次，简直要爆炸，而后面的要少循环很多很多次**

#### 文件拷贝(以字节形式拷贝)
**注意：为什么以字节的形式拷贝？因为数据在文件(硬盘)中的存储形式本来就是字节，用字节形式拷贝完全不用考虑编码问题！完全没有必要将字节先转换成字符串然后再将字符串存储到文件中，否则反而会出现乱码问题！**
```java
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyFileTest {

	public static void main(String[] args) throws IOException {

		// 拷贝文件
		// 原理：读取一个文件中的数据，并将这些读入到的数据写入另一个文件中
		
		// 1、明确源文件和目的文件
		File srcFile = new File("testDir\\testFile");
		File destFile = new File("testDir\\testFile2");
		
		// 2、明确字节流输入流和源文件关联，字节输出流和目的文件关联
		FileInputStream fis = new FileInputStream(srcFile);
		FileOutputStream fos = new FileOutputStream(destFile);
		
		// 3、定义一个缓冲数组
		byte[] buf = new byte[2048];
		int len = 0;
		// 每次从源文件读取一个字节数组，并将字节数组写入到目的文件中
		while((len = fis.read(buf)) != -1)
		{
			fos.write(buf, 0, len);
		}
		
		// 4、关闭资源
		fos.close();
		fis.close();
	}

}
```
上面的文件拷贝，也可以一个字节一个字节的拷贝，但是同理，效率很低，数据量很大时，感觉很爆炸~~~~~
**注意：文件拷贝不分文件类型（无论文本、图片、视频、音频），只要是文件即可，因为文件中的数据都是字节存储的**

#### 字节输入流的avaliable方法
字节输入流的avaliable方法的作用是返回一个数值，表示与当前输入流绑定的数据源的字节数，
- **如果字节数不多，可以一次定义一个字节数确定的缓冲数组，read一次就够了**
- **如果是大文件，字节数很多很多，则不能使用上面的思路，否则，一次定义一个太大的缓冲数组，可能导致堆内存溢出**
针对第一个如果，演示如下：
```java
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class FileInputStreamDemo3 {

	public static void main(String[] args)
	{
		File testFile = new File("testDir\\testFile");
		
		FileInputStream fis = null;
		try {
			fis = new FileInputStream(testFile);
			
			// 如果数据源文件很大，字节数很多，会导致堆内存溢出
			byte[] buf = new byte[fis.available()];	
			fis.read(buf);
			// 读取内容到缓冲字节数组中
			System.out.println(new String(buf));

		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			if(fis != null)
			{
				try {
					fis.close();
				} catch (IOException e) {
					throw new RuntimeException();
				}
				fis = null;
			}
		}
	}
}
```
### 编码表
**ascii：**
 - 只用了一个字节的后7位来存储，第一位一定是0。即表示的字节一定是正数。

**iso8859-1：**
 - 拉丁码表：只用了一个字节的8位。即一个字节可能为正数，可能为负数。

**GBK：**
 - 目前最常用的中文码表：其中包含2万的中文和符号。用两个字节表示。其中一部分文字对应的两个字节都是负数。
 - 另一部分文字对应的两个字节是：第一个字节为负数，第二个字节为正数

**unicode：**
 - 国际标准码表：无论是什么文字，都用两个字节存储。Java中的char类型用的就是这个码表。例如：char c = 'a';虽然'a'是一个字节，但这里依旧是存的两个字节。
 - 在Java中，字符串是按照系统默认码表来解析的。简体中文版字符串默认的码表是GBK。

**UTF-8：**
 - 基于unicode，第一个U就代表unicode，这个码表的特点在于：如果一个字节就可以存储数据，则不会用两个字节来存储。
 - 而且这个码表更加标准化，在每一个字节头加入了编码信息 。

**常用码表：**
 - GBK、UTF-8、ISO-8859-1、

#### 编码：
**文字---->二进制(数字)：编码**，看得懂的变成看不懂的就是编码！！！
#### 解码：
**二进制(数字)---->文字：解码**，看不懂的变成看得懂的就是解码！！！


### 字符流
字符流：用来操作字符文件的便捷类。**它的内部使用：字符集+字节缓冲区**
- **字节流针对字节**进行操作
- **字符流针对字符**进行操作，既然是针对字符操作，那么就比字节流多了个字符集，并且由这个字符集来确定一个适当的字节缓冲区，否则会乱码
 - 即如果文件中存的是中文，那么假设字符流内部使用的字符集就是GBK，而字节缓冲区就会是2的整数倍，因为字符流会根据这个GBK字符集得知：读取到的每两个字节转换成一个字符！！！
- 字节流和字符流的方法都差不多，例如：字节流使用read()方法每次读取的是一个字节，字符流使用read()方法每次读取的是一个字符！
- **近似地可以认为：字符流=字节流+字符集+字节缓冲区 ， 字节流基本上都有个对应的字符流，就是说这些字符流的底层都有个与之对应的字节流。**
- **Reader 对应 InputStream，Writer 对应 OutputStream**
- **flush()方法的作用：**写出字符流比写出字节流，多了一个flush()方法，**flush方法的作用是**：“刷新该流的缓冲。如果该流已保存缓冲区中各种 write() 方法的所有字符，则立即将它们写入预期目标。”   **原因是**：写出字符流相当于写出字节流+字符集+字节缓冲区，写出字符流调用write方法写出内容时，是将这些内容先放到字节缓冲区中的，只有字节缓冲区满了，才会自动将缓冲区中保存的内容写入目标文件中。所以flush()方法的效果就是不等待缓冲区满，也自动将缓冲区中保存的内容写入目标文件。
- **写出字符流的close()方法**在关闭流之前，会默认调用一次flush()方法。
 - flush()和close()的区别：
   - flush()：将流中的缓冲区中保存的数据刷新到目标中，刷新后，流还可以继续使用！
   - close(): 关闭资源，但在关闭前会将缓冲区中的数据先刷新到目标，否则会丢失数据，然后再关闭流，之后，流将不可用。
   - 关联：如果写入数据多，一定要边写边flush()，最后一次可以不用flush()，直接由close()来刷新并关闭！！

#### FileReader
根据上面的规则：
- FileReader  相当于  FileInputStream+字符集
- FileReader读一个字符，FileInputStream读一个字节
- FileReader、FileInputStream都是功能都是操作File

下面用FileReader来读取文件中的中文：
```java
import java.io.FileReader;
import java.io.IOException;

public class FileReaderDemo {

	public static void main(String[] args) throws IOException {

		// 字符流和字节流的方法都很相似！！！构造方法也是
		FileReader fr = new FileReader("testDir\\testFile");	// FileReader这个字符流的底层就是FileInputStream字节流
		
		// 定义一个字符的值
		int ch = 0;
		// 使用字符流每读取一个字符，底层实现是：读取多个字节(具体读多少，是将字符集拿到码表中去查询然后再确定)，然后将这些字节转换成一个字符
		while((ch = fr.read()) != -1)
		{
			// 将每个字符值转换成字符
			System.out.print((char)ch);
		}
	}
}
```
#### FileWriter
下面用FileWriter来写出中文到文件中：
```java
import java.io.FileWriter;
import java.io.IOException;

public class FileWriterDemo {

	public static void main(String[] args) throws IOException {

		FileWriter fw = new FileWriter("testDir\\testFile3");
		
		// 写出这行文字的底层实现为：将这行文字先根据字符集查找码表，
		// 然后编码成字节，并且保存到缓冲字节数组中,并不是直接写到目标文件中
		fw.write("你好，谢谢，再见！");
		
		// 这里只写一次，可以省略此行代码
		fw.flush();
		
		// 关闭资源
		fw.close();
	}

}
```
#### 小总结
FileReader和FileWriter中的字符集都是默认的，不可以手动改变的，由系统语言环境决定。

### 字符转换流（字节流与字符流的桥梁）
#### OutputStreamWriter

**OutputStreamWriter：是自行指定编码格式和底层字节流的字符输出流**

**FileWriter是它的采用默认编码表的子类，所以FileWriter是个便捷类**

**OutputStreamWriter：字符流通向字节流的桥梁：可使用【指定的字符集】将要写出到目标的字符编码成字节。它使用的字符集可以由名称指定或显式给定，否则将接受平台默认的字符集。**
注意：前面说了可以近似地认为字符流=字节流+字符集+字节缓冲区，而字节缓冲区又由字符集来确定。所以我们要是自行指定字符集，来构造出这个OutputStreamWriter，那么还缺一个字节流(OutputStream)，所以在构造字符流(OutputStreamWriter)，我们要传入一个合适的OutputStream，具体什么类型的要看具体情况。

**下面用OutputStreamWriter来【按一定的编码格式】向文件中写出字符内容**
```java
import java.io.FileOutputStream;
import java.io.OutputStreamWriter;

public class OutputStreamWriterDemo {

	public static void main(String[] args) throws Exception {

		// 我们要向文件中按指定编码格式UTF-8输出字符内容，必须有个底层的字节流;
		// 因为是文件，所以用FileOutputStream最合适
		FileOutputStream fos = new FileOutputStream("testDir\\testFile4");
		
		// 再指定要求的编码格式UTF-8，构造出这个输出字符流
		OutputStreamWriter osw = new OutputStreamWriter(fos, "UTF-8");
		
		// 输出内容:底层实现是：write方法将这一句话写到字节缓冲区中，
		// 这些字节是按照指定的编码格式UTF-8编码然后保存进去的，暂时还没有写到文件中
		osw.write("你好，谢谢，再见！");
		
		// 这里可以不用调用flush()方法，因为只输出一次，直接利用close()方法的刷新即可。
		
		// 关闭资源：先刷新，将字节缓冲区(字节数组)中的内容通过FileOutputStram来write到文件中，然后关闭流
		osw.close();
	}

}
```

#### InputStreamReader

**InputStreamReader：是自行指定编码格式和底层字节流的字符输入流** 

**FileReader是它的采用默认编码表的子类，所以FileReader是个便捷类**

**InputStreamReader：字节流通向字符流的桥梁：它使用【指定的字符集】将要输入到内存的字节内容解码为字符。它使用的字符集可以由名称指定或显式给定，或者可以接受平台默认的字符集。**

**InputStreamReader的构造与OutputStreamWriter同理**

**下面用InputStreamReader来【按一定的编码格式】从文件中读取字符内容**
```java
import java.io.FileInputStream;
import java.io.InputStreamReader;

public class InputStreamReaderDemo {

	public static void main(String[] args) throws Exception {

		// 我们要从文件中按指定解码格式UTF-8读出字符内容，必须有个底层的字节流;
		// 因为是文件，所以用FileInputStream最合适
		FileInputStream fis = new FileInputStream("testDir\\testFile4");
		
		// 再指定要求的解码格式UTF-8，构造出这个输入字符流
		InputStreamReader isr = new InputStreamReader(fis, "UTF-8");
		
		// 定义读取字符个数
		int len = 0;
		char[] buf  = new char[1024];
		
		// 循环读取字符：底层实现是：
		// 先通过FileInputStream读取文件中的字节到内部字节缓冲数组中
		// 然后将字节缓冲数组中的字节根据UTF-8码表查找出对应的文字
		// 再将这些文字放到buf字符数组中
		while((len = isr.read(buf)) != -1)
		{
			// 输出字符内容
			System.out.println(new String(buf, 0, len));
		}
		
		// 关闭资源：注意：输入流不像输出流，不需要flush，自然也没有这个方法
		isr.close();
	}

}
```

#### 文件拷贝练习
```java
import java.io.FileReader;
import java.io.FileWriter;

public class CopyFile {

	public static void main(String[] args) throws Exception {

		/*
		 * 练习：复制文本文件
		 * 思路：
		 * 1、既然是文本文件，涉及到编码表，要使用字符流（用字节流也可以复制，但是如果想要拿到指定文字内容则不方便，所以这里使用字符流）
		 * 2、操作的是文本文件，涉及到硬盘
		 * 3、有指定码表吗？没有，所以使用默认
		 * 依上可知：操作的是文件，使用默认码表：那么使用字符流操作文件的便捷类（FileReader和FileWriter）最合适
		 */
		// 输入流绑定数据源，输出流绑定目标文件
		FileReader fr = new FileReader("testDir\\testFile4");
		FileWriter fw = new FileWriter("testDir\\testFile4_copy");
		
		// 为了提高效率，定义一个字符数组
		int len = 0; // 获取的字符个数
		char[] buf = new char[1024];
		
		// 开始拷贝
		while((len = fr.read(buf)) != -1)
		{
			fw.write(new String(buf, 0, len));
		}
		
		// 关闭资源
		fw.close();
		fr.close();
	}

}
```
### 缓冲字符流
**BufferedReader :** 从字符输入流中读取文本，缓冲各个字符，从而实现字符、数组和行的高效读取。 
**BufferedWriter :** 将文本写入字符输出流，缓冲各个字符，从而提供单个字符、数组和字符串的高效写入。 
- 问题：自定义数组就可以解决缓冲区问题，并提高效率，那么为什么还要使用缓冲字符流对象呢？
 - 答：因为缓冲区对象中除了封装数组以外，还提供了更多的操作，比如跨平台的“行”功能！！！
 
**注意：**既然是建立在字符流的基础上，所以**构造缓冲字符流对象时，要先有字符流对象**

下面用缓冲字符流对象进行文件拷贝：
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;

public class CopyFileByBufferedCharacterStream {

	public static void main(String[] args) throws Exception {

		BufferedReader bufr = new BufferedReader(new FileReader("testDir\\testFile5"));
		BufferedWriter bufw = new BufferedWriter(new FileWriter("testDir\\testFile5_copy"));
		
		// 循环读写一行数据
		String line = null;
		while((line = bufr.readLine()) != null)
		{
			bufw.write(line);
			// 写入换行(跨平台的换行)
			bufw.newLine();
			// 写多行，一定要用flush刷新
			bufw.flush();
		}
		
		// 关闭流
		bufw.close();
		bufr.close();
	}
}
```



### 总结
总结几句话：
- 写数据到文件：输出流，绑定目的文件
- 读数据到内存：输入流，绑定源文件
- 对有条件的文件操作：使用过滤器
- 对目录下的文件操作，且包含子目录下的：使用递归/队列
- 使用默认系统字符集读写文件：使用FileReader和FileWriter
- 按指定编码格式读写文件，使用InputStreamReader和OutputStreamWriter
- 编码与解码格式一致，才不会乱码