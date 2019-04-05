---
layout: post
title: "Selection Sort"
excerpt: ""
categories: [Algorithm]
share: true
comments: true
---

> #### Selection Sort
* * *

> 시간복잡도 T(n) = O(n<sup>2</sup>) = (n-1)+(n-2)+...+2+1

*최악, 최선, 평균 모두 Q(n<sup>2</sup>)*

배열을 선형 탐색(linear scan)하며 가장 작은 원소를 배열 맨 앞으로 보낸다 (맨 앞에 있던 원소와 자리를 바꾼다).

```
public class SelectionSortExample {
  public static void main(String[] args) {
    int[] arr = {3,2,30,45,230,1,503};
    int temp = 0;

    for (int i = 0; i < arr.length-1; i++) {
      for (int j = i+1; j < arr.length; j++) {
        if (arr[i] > arr[j]) {
          temp = a[j];
          a[j] = a[i];
          a[i] = temp;
        }
      }
    }
  }
}
```

* * * 
