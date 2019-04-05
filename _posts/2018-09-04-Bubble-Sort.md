---
layout: post
title: "Bubble Sort"
excerpt: ""
categories: [Algorithm]
share: true
comments: true
---

> #### Bubble Sort
* * *

*서로 인접한 두 원소를 검사하여 정렬하는 알고리즘*

> 실행시간 : O(n<sup>2</sup>) = (n-1)+(n-2)+...+2+1

*최악, 최선, 평균 모두 Q(n<sup>2</sup>)*

배열의 첫 원소부터 순차적으로 진행하며, 현재 원소가 그 다음 원소의 값보다 크면 두 원소를 바꾸는 작업을 반복한다.

```
public class BubbleSortExample {
  static void bubbleSort(int[] arr) {
    int len = arr.length;
    int temp = 0;

    for (int i = 0; i < len; i++) {
      for (int j = 1; j < (len-1); j++) {
        if (arr[j-1] > arr[j]) {
          // swap elements
          temp = arr[j-1];
          arr[j-1] = arr[j];
          arr[j] = temp;
        }
      }
    }
  }

  public static void main(String[] args) {
    int[] arr= {3,2,30,45,230,1,503};

    System.out.println("Array Before Bubble Sort");
    for (int i : arr) {
      System.out.print(i + " ");
    }

    bubbleSort(arr);

    System.out.println("Array After Bubble Sort");
    for (int j : arr) {
      System.out.print(j + " ");
    }
  }
}
```

Output:
```
Array Before Bubble Sort
3 2 30 45 230 1 503
Array After Bubble Sort
1 2 3 30 45 230 503
```

* * *
