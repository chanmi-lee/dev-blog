---
layout: post
title: "Overloading vs Overriding"
excerpt: ""
categories: [Java]
share: true
comments: true
---

> #### Overloading vs Overriding
* * *

**Overrloading** 은 두 메서드가 같은 이름을 갖고 있으나, 인자의 수나 자료형이 다른 경우를 지칭한다.

```
public double computeArea(Circle c) { ... }
public double computeArea(Square s) { ... }
```

**Overriding** 은 상위 클래스의 메서드와 이름과 용례(signature)가 같은 함수를 하위 클래스에 재정의하는 것을 의미한다.

```
public abstract class Shape {
  public void printMe() {
    System.out.println("I am a shape.");
  }
  public abstract double computeArea();
}

public class Circle extends Shape {
  private double radious = 5;
  public void printMe() {
    System.out.println("I am a circle.");
  }

  public double computeArea() {
    return radious * radious * 3.15;
  }
}

public class Ambiguous extends Shape {
  private double area = 10;
  public double computeArea() {
    return area;
  }
}

public class IntroductionOverriding {
  public static void main (String[] args) {
    Shape[] shapes = new Shape[2];
    Circle circle = new Circle();
    Ambiguous ambiguous = new Ambiguous();

    shapes[0] = circle;
    shapes[1] = ambiguous;

    for (Shape s : shapes) {
      s.printMe();
      System.out.println(s.computeArea());
    }
  }
}
```

위 코드를 실행한 출력 결과는 다음과 같다.
```
I am a circle.  // Ambiguous는 printMe 메소드를 재정의(Overriding)
78.75
I am a shape.   // Ambiguous는 printMe 메소드를 그대로 둠
10.0
```

* * *
