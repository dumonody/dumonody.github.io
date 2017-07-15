---
title: java反射获取的自定义类中声明的成员变量，是否有序？
---

```
// User.java
public class User {

	private int id;
	private String name;
	private String sex;
	private String age;
	private double height;
	private double weight;
	
	
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getSex() {
		return sex;
	}
	public void setSex(String sex) {
		this.sex = sex;
	}
	public String getAge() {
		return age;
	}
	public void setAge(String age) {
		this.age = age;
	}
	public double getHeight() {
		return height;
	}
	public void setHeight(double height) {
		this.height = height;
	}
	public double getWeight() {
		return weight;
	}
	public void setWeight(double weight) {
		this.weight = weight;
	}
	
}

```

```
// Main.java
import java.lang.reflect.Field;

public class Main {

	public static void main(String[] args) {

		Class clazz = User.class;
		Field[] fields = clazz.getDeclaredFields();
		for(Field f : fields)
		{
			System.out.println(f.getName());
		}
	}

}

```

```
// 输出结果：
id
name
sex
age
height
weight

```

#### 结论：通过反射，获取自定义类中的声明的成员变量，获取到的顺序与声明的顺序一致！