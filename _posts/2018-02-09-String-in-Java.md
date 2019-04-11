---
layout: post
title: "String in Java"
excerpt: "The features of String Class and the difference between == and equals() when comparing object in Java"
categories: [Java]
share: true
comments: true
---

> #### String 생성
***

Java에서 `String` 은 char의 Array, 즉 Reference Type으로 두가지 생성 방식이 있습니다.

```
// 1. New 키워드를 이용한 생성
String str = new String("chanmi");

// 2. 리터럴을 이용한 방식
String literal = "chanmi";
```

`new`를 통해 `String`을 생성하면 `Heap` 영역에 존재하게 되고, literal을 이용할 경우 `string constant pool` 이라는 영역에 존재하게 됩니다.

![](../assets/img/posts/String-Pool-Java.png)

`String`을 리터럴로 선언할 경우 내부적으로는 `String` 클래스의 `intern()` 메서드가 호출되게 됩니다.

`intern()` 는 주어진 문자열이 `string constant pool`에 존재하는지 찾고 있다면 그 주소값을 반환, 없다면 `string constant pool`에 넣고 새로운 주소값을 반환하게 됩니다.

따라서, `new` 로 생성된 `String` 객체를 `intern()` 을 통해 호출하면, `Heap` 메모리에 있는 `String` 객체를 `String constant pool` 영역으로 이동할 수 있습니다.

```
System.out.println(literal == str); // false
System.out.println(literal == str.intern());  // true
```

기존의 `new` 를 통해 생성된 `str` 객체와 리터럴로 생성된 `literal` 객체를 `==` 연산하였을 경우엔 각각의 주소값이 다르기 때문에 false를 반환하였습니다.

그러나 `intern()` 메서드를 호출하면서 `string constant pool`에 `Chanmi` 라는 문자열을 검색하게 되고, 이미 저장한 `Chanmi` 문자열을 발견할테니 동일한 주소값을 반환하게 되므로 true를 반환하게 됩니다.

* * *

#### immutable

다른 Reference type과 달리 String은 `immutable` 하다는 특징을 가집니다. 즉, 생성된 객체에 저장된 값은 변하지 않습니다.

때문에, `+` 연산자나 `concat()` 메소드를 통해 이미 생성된 `String` 객체끼리 이어붙이려는 작업을 하려 한다면 어떻게 될까요?

```
String str1 = new String("Chanmi");
String str2 = new String("Lee");
String str3 = str1 + str2;
```

`str3`라는 `String` 객체를 새로 생성하고 이를 참조하게 됩니다. `immutable` 하다는 `String`의 속성 때문에 계속해서 새로운 객체를 만들어내어 *메모리 관리 측면에서 비효율적* 입니다.

* * *

#### equals() and ==

```
String s1 = new String("java");
String s2 = new String("java");

System.out.println(s1 == s2); // false
System.out.println(s1.equals(s2));  // true
```

- `equals()`는 값을 비교 (call-by-value)
- `==`는 메모리 주소(참조)값을 비교 (call-by-reference)

**hashCode()**

- `Object`의 메소드로 객체의 주소값을 이용하여 객체 고유의 해시코드를 리턴하는 함수입니다

> hashCode()를 equals()와 함께 오버라이딩 해야하는 이유

HashSet, HashMap 과 같은 Collection framework에서 객체 비교시 hashCode() 결과값을 통해 해시코드 값이 다르면 다른 객체로 판단하기 때문입니다.

즉, `equals()`로 같은 객체라면 반드시 `hashCode()`도 같은 값이어야 같은 객체로 판단할 수 있습니다.

```
class Member {
  public String id;

  @Override
  public boolean equals(Object obj) {
    if (obj instanceof Member) {
      Member member = (Member) obj;
      if (id.equals(member.id)) {
        return true;
      }
    }
    return false;
  }

  @Override
  public int hashCode() {
    return id.hashCode();
  }
}

```



* * *

Ref

- [What is java string poop](https://www.journaldev.com/797/what-is-java-string-pool)
- [stackoverflow - String Constant Pool](https://stackoverflow.com/questions/14150628/string-constant-pool)
- [stackoverflow - Why is there no primitive type for string](https://stackoverflow.com/questions/2099257/why-is-there-no-primitive-type-for-string)
- [Java String의 메모리에 대한 고찰](https://medium.com/@joongwon/string-%EC%9D%98-%EB%A9%94%EB%AA%A8%EB%A6%AC%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0-57af94cbb6bc)
