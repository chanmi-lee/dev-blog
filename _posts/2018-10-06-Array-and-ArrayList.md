---
layout: post
title: "Array & ArrayList"
excerpt: "The difference between Array and ArrayList in Java and the way how to convert Array to ArrayList and vice versa with some simeple example"
categories: [DataStructure, Java]
share: true
comments: true
---

> #### Array & ArrayList
* * *

Array와 ArrayList는 Java에서 가장 많이 사용되는 자료구조다. 이름은 유사하나, 각각의 특성이 다르기 때문에 차이점을 살펴보며 변환하는 방법을 간단한 예제와 함께 알아보자.

| Array | ArrayList |
| --- | --- |
| 초기화시 고정 (fixed-size) | 유동적(re-sizable) |
| 사이즈 변경 불가 | 추가, 삭제 가능 (add(), remove()) |
| primitive type과 Object를 원소로 함 | Object만 원소로 함 |

* * *

> Array를 ArrayList로 변환

1. Arrays.asList()

```
public class ArrayToArrayListExample {
  public static void main(String[] args) {
    String[] array = {"Android", "Java", "Python", "Swift", "Ruby"};

    ArrayList<String> list = new ArrayList<>(Arrays.asList(array));

    System.out.println(list);
  }
}
```

output: ```[Android, Java, Python, Swift, Ruby]```

[ **Caution** ]
**Arrays.asList()** 는 Arrays의 private 정적 클래스인 ArrayList를 리턴한다. 이는 ```java.util.ArrayList``` 클래스와는 다른 클래스인 ```java.util.Arrays.ArrayList``` 이다.

```
// Do Not
ArrayList<String> list = Arrays.asList(array);

// Do
ArrayList<String> list = new ArrayList<>(Arrays.asList(array));
```

```java.util.Arrays.ArrayList``` 는 set(),  get(), contains() 메소드가 있으나, 원소를 추가하기 위한 메소드는 갖고 있지 않다. 즉 사이즈가 고정되어있다.

2. Collections.addAll()

```
public class ArrayToArrayListExample {
  public static void main(String[] args) {
    String[] array = {"Android", "Java", "Python", "Swift", "Ruby"};

    ArrayList<String> list = new ArrayList<>();
    Collections.addAll(list, array);

    System.out.println(list); // [Android, Java, Python, Swift, Ruby]
  }
}
```

3. ArrayList.addAll()

```
public class ArrayToArrayListExample {
  public static void main(String[] args) {
    String[] array = {"Android", "Java", "Python", "Swift", "Ruby"};

    List<String> list = new ArrayList<>();
    list.addAll(Arrays.asList(array));

    System.out.println(list); // [Android, Java, Python, Swift, Ruby]
  }
}
```

4. Streams from Java 8

```
public class ArrayToArrayListExample {
  public static void main(String[] args) {
    List<Object> list = Arrays.streams(array).collect(Collectors.toList());

    System.out.println(list); // [Android, Java, Python, Swift, Ruby]
  }
}
```

* * *

> ArrayList를 Array로 변환

```
public class ArrayListToArrayExample {
  public static void main(String[] args) {
    ArrayList<String> list = new ArrayList<>();
    list.add("Android");
    list.add("Java");
    list.add("Python");
    list.add("Swift");
    list.add("Ruby");

    String[] array = new String[list.size()];
    list.toArray(array);

    for (String str : array) {
      System.out.println(str);
    }
  }
}
```

output:
```
Android
Java
Python
Swift
Ruby
```


* * *

Ref
- [Java Concept of The Day - Array to ArrayList and ArrayList to Array in Java with examples](http://javaconceptoftheday.com/array-to-arraylist-in-java-with-examples/)
- [Top 10 Mistakes Java Developers Make](https://www.programcreek.com/2014/05/top-10-mistakes-java-developers-make/)
- [Arrays - Oracle Docs](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html)
