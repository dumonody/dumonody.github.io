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

### OutputStream：所有输出字节流类的超类
- 1、操作的数据都是字节
- 2、定义了输出字节流的基本共性功能
- 3、本身是个抽象类，不能直接new出一个实例
- 4、输出流中定义的写方法都是write
- 5、**规律：它的所有子类名称后缀是OutputStream，子类的名称前缀代表功能**

#### 将数据写入到文件中
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
#### 将数据续写入到已有文件中（以及续写换行符）
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