---
title: IO
---

> ## File

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