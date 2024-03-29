##  Java 中使用 sort() 方法排序：从基本原理到用法详解

在 Java 编程中，sort() 函数是一个非常重要且常用的方法，用于对数组或集合进行排序操作。本文将深入探讨 sort() 函数的基本原理、实现机制以及其在实际应用中的用法。

# 1. sort() 函数的基本原理

Java中的`sort()`函数是一个高效的排序算法，其具体的实现原理取决于排序的数据类型和大小。在Java 7之前，`Arrays`类使用的是经典的快速排序算法。在Java 7及以后的版本中，`Arrays`类在排序小型数组时使用的是改进后的快速排序算法，而在排序大型数组时使用的是归并排序算法。

详情请看往期：

# 2. sort() 函数的用法

在 Java 中，sort() 函数可以用于对数组或集合进行排序，其用法略有不同。

## 2.1 默认排序（升序）

格式：

```
Arrays.sort(array);
Collections.sort(list);
```

例：

```
//对整型数组进行排序
int[] array = {5, 2, 9, 1, 3};
Arrays.sort(array);
System.out.println(Arrays.toString(array)); //输出：[1, 2, 3, 5, 9]

//对字符串数组进行排序
String[] names = {"John", "Alice", "Bob", "David"};
Arrays.sort(names);
System.out.println(Arrays.toString(names)); //输出：[Alice, Bob, David, John]

// 对列表进行排序
List<Integer> list = new ArrayList<>();
list.add(5);
list.add(2);
list.add(9);
list.add(1);
list.add(3);
Collections.sort(list);
System.out.println(list); // 输出：[1, 2, 3, 5, 9]
```

## 2.2 局部排序

格式：

```
Arrays.sort(数组名,start,end);//注意左闭右开！！！[start,end)
```

例：

```
int[] array = {5, 2, 9, 1, 3};
//对数组的一部分内容进行排序（索引从1到3，包括1不包括3）
Arrays.sort(array, 1, 3);
System.out.println(Arrays.toString(array)); //输出：[5, 1, 2, 9, 3]
```

## 2.2 降序排序（逆序排列）

有时我们需要对数组或集合进行倒序排序，可以通过自定义比较器实现：

### 2.2.1 使用Collections.reverseOrder()方法进行逆序排序

```
//数组：
Integer[] arr = {3, 1, 4, 1, 5, 9};
Arrays.sort(arr, Collections.reverseOrder());// 输出：[9, 5, 4, 3, 1, 1]

Collections.sort(list, nameComparator.reversed());

//对集合：
List<Integer> list = Arrays.asList(3, 1, 4, 1, 5, 9);
Collections.sort(list, Collections.reverseOrder());
System.out.println(list);  // 输出：[9, 5, 4, 3, 1, 1]
```

1. 使用`Comparator.reversed()`方法

```java
List<Student> list = Student.getStudentList();
Comparator<Student> nameComparator = (s1, s2) -> s1.getName().compareTo(s2.getName());
Collections.sort(list, nameComparator.reversed());
```

###  2.2.2 匿名内部类

```
Integer[] arr = {8, 9, 5, 2, 3, 6, 1, 0, 7, 4};
Arrays.sort(arr, new Comparator<Integer>() {
    @Override
    public int compare(Integer o1, Integer o2) {
        return o1 - o2;
    }
});
```

### 2.2.3使用Lambda表达式进行逆序排序

```java
Integer[] arr = {9, 8, 7, 2, 3, 4, 1, 0, 6, 5};
Arrays.sort(arr, (o1, o2) -> o2 - o1);
```

## 2.3多级排序

```java
List<Developer> listDevs = getDevelopers();
Comparator<Developer> comparator = Comparator.comparing(Developer::getAge)
                                              .thenComparing(Developer::getName);
listDevs.sort(comparator);
```



## 2.3 自定义对象进行排序

```
//数组自定义对象排序
Person[] people = {new Person("Alice", 25), new Person("Bob", 20), new Person("Charlie", 30)};
Comparator<Person> byName = Comparator.comparing(Person::getName);//注：Person 类中需有getName()方法
Arrays.sort(people, byName); //按照姓名排序

// 集合自定义对象排序
List<Person> persons = new ArrayList<>();
persons.add(new Person("Alice", 25));
persons.add(new Person("Bob", 30));
persons.add(new Person("David", 20));
Comparator<Person> byName = Comparator.comparing(Person::getName);
Collections.sort(persons, byName); // 使用 Collections.sort() 对 List 进行排序

//匿名内部类
public static List<People> compareTest(List<People> arr) {
    Collections.sort(arr, new Comparator<People>() {
        public int compare(People p1, People p2) {
            int a = p1.age; //比较的是age
            int b = p2.age;
            return a < b ? -1 : a == b ? 0 : 1; //当a<b返回-1，a==b返回0，a>b返回1
        }
    });

```

## 2.4 Lambda 表达式简化简化Comparator的实现

### 2.4.1 基本的Lambda表达式

```java
Comparator<Developer> byName = (Developer o1, Developer o2) -> o1.getName().compareTo(o2.getName());
```

这里，我们创建了一个Comparator来比较两个Developer对象的名字。

### 2.4.2 省略类型定义：

```java
humans.sort((h1, h2) -> h1.getName().compareTo(h2.getName()));
```

在这个例子中，我们没有指定h1和h2的类型，编译器会自动推断出它们的类型。

### 2.4.3 使用静态方法引用：

```java
humans.sort(Human::compareByNameThenAge);
```

在这里我们使用了Human类的静态方法compareByNameThenAge作为Comparator。

### 2.4.4 使用实例方法引用：

```java
Collections.sort(humans, Comparator.comparing(Human::getName));
```

## 2.5. 并行排序

Java 8引入了并行排序。如果你有一个非常大的数组，你可以使用`Arrays.parallelSort()`方法进行排序。这个方法会利用多核处理器的优势，将数组分成多个部分，然后并行地对每个部分进行排序，最后再将结果合并起来。：

```
//例1：
int[] arr = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5};
Arrays.parallelSort(arr);
System.out.println(Arrays.toString(arr));  // 输出：[1, 1, 2, 3, 3, 4, 5, 5, 5, 6, 9]

//例2
List<Integer> list = new ArrayList<>();
// 添加大量数据到列表中
list = list.parallelStream().sorted().collect(Collectors.toList());
```