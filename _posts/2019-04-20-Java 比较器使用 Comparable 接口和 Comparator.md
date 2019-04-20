Java 比较器使用 `Comparable` 接口和 `Comparator`	

- `Comparable` 接口是在自 `java.lang`中的一个接口，`java.lang`是一个提供基础类的Java包，因为`Comparable`是基础类中都实现了该接口，例如`string`和`Integer`基础类型。一个实现了该接口的类就是一个可以比较的类，而比较的方法就是按照重写的`compareTo`方法进行比较的。注意，这个`compareTo`方法只接收一个形参。

```java
public final class java.lang.String implements java.io.Serializable, java.lang.Comparable, java.lang.CharSequence {
    ...
        public int compareTo(java.lang.String arg0){
        ...
    }
}
```

和

```java
public final class java.lang.Integer extends java.lang.Number implements java.lang.Comparable{
    ...
        public int compareTo(java.lang.Integer arg0){
        ...
    }
}
```

- `Comparator`接口是在`java.util`中的一个接口，他是一个单独的比较器，可以单独作为一个类封装比较算法，可以和数据类型解耦。

### 具体举例

例如实现一个根据年龄比较的`Student`类，从小到大排序，两种不同的实现代码为：

- 使用`Comparable`方法

```java
import java.util.Arrays;

public class Student2 implements Comparable<Student2>{
	
	public int age;
	public String name;
	Integer i;
	Student2(int age, String name){
		this.age = age;
		this.name = name;
	}

	public static void main(String[] args) {

		Student2 s1 = new Student2(24,"gk");
		Student2 s2 = new Student2(16,"gx");
		Student2[] ss = new Student2[] {s1,s2};
		Arrays.sort(ss);

		for(Student2 s:ss)
			System.out.println(s.age);
		
	}

	@Override
	public int compareTo(Student2 o) {
		return  o.age - this.age ;
	}

}
```

- 实现`Comparator` 

```java
import java.util.Arrays;
import java.util.Comparator;

public class Student1 {
	
	public int age;
	public String name;
	
	public Student1(int age, String name) {

		this.age = age;
		this.name = name;
	}

	public static void main(String[] arg) {
		
		Student1 s1 = new Student1(24,"guoshikun");
		Student1 s2 = new Student1(16,"guoshixiang");
		Student1[] ss = new Student1[] {s1,s2};
		Arrays.sort(ss,new minAgeCom<Student1>());
		Arrays.sort(ss, new maxAgeCom<Student1>());
		for(Student1 s:ss)
			System.out.println(s.age);
	}
}


import java.util.Arrays;
import java.util.Comparator;

public class Student1 {
	
	public int age;
	public String name;
	
	public Student1(int age, String name) {

		this.age = age;
		this.name = name;
	}

	public static void main(String[] arg) {
		
		Student1 s1 = new Student1(24,"g");
		Student1 s2 = new Student1(16,"g");
		Student1[] ss = new Student1[] {s1,s2};
     //  Student2 s3 = new Student1(24,"g");
	//	Student2 s4 = new Student1(16,"g");
	//	Student2[] sss = new Student2[] {s3,s4};
		Arrays.sort(ss,new minAgeCom());
	//	Arrays.sort(sss, new minAgeCom());
		for(Student1 s:ss)
			System.out.println(s.age);
	}
}



class minAgeCom implements Comparator<Student1>{

	@Override
	public int compare(Student1 o1, Student1 o2) {
		return o1.age - o2.age;
	}
}


class maxAgeCom implements Comparator<Student1>{

	@Override
	public int compare(Student1 o1, Student1 o2) {
		return o2.age - o1.age;
	}
	
}
```

下面来分析一下不同，首先实现思路是不一样的，首先`Comparable`和其字面意思一样，直接规定实现该接口的数据类型（也就是类）是可以可比较`Comparable`的数据类型。所以在对`Student`数组排序的时候，直接调用即可，因为每一个元素都是可以比较的。即`Arrays.sort(ss)`。但是对于第二种`Comparator`接口，实现该接口的类是一个比较器，就是一个专门为比较而生的一个类，~~可以应用于一类相似的数据类型，在本例中只要是一个类中有年龄这个属性就可以。~~我们可以随意使得我们的数据类型根据给定的比较器方法排序，实现了比较和数据类型解耦，当我们需要新的排序方法时候，只需要增添一下新的比较器类即可，但是如果第一种方法，要更改比较类型，就需要更改`Student1`的源代码，没有实现数据类型和比较器的解耦。