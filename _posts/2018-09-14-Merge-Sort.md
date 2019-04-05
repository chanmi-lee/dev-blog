---
layout: post
title: "Merge Sort"
excerpt: ""
categories: [Algorithm]
share: true
comments: true
---

> #### Merge Sort
* * *

분할정복법이라는 전략을 사용하는 알고리즘
* 분할: 해결하고자 하는 문제를 작은 크기의 동일한 문제들로 분할 (divide)
* 정복: 각각의 작은 문제를 순환적으로 해결 (recursively sort)
* 합병: 작은 문제의 해를 합하여 (merge) 원래 문제에 대한 해를 구함

ex) 주어진 배열의 최대값을 구해야 할 때, 반으로 나눠 (**분할**) 각각 최대값을 찾은 후 (**정복**) 값을 비교하여 최대값을 도출 (**합병**)

```
public class MergeSortExample {
  void mergeSort(int[] arr, int p, int r) {
    if (p < r) {
      int middle = (p+r)/2;

      mergeSort(arr, p, middle);
      mergeSort(arr, middle+1, r);
      merge(arr, p, middle, r);
    }
  }

  void merge(int[] data, int p, int m, int r) {
    int i = p, j = m+1, k = p;
    int[] tmp[data.length()];

    while (i <= q && j <= r) {
      if (data[i] <= data[j]) {
        tmp[k++] = data[i++];
      } else {
        tmp[k++] = data[j++];
      }
    }

    while (i <= q) {
      // 앞의 배열에만 원소가 남아있을 때
      tmp[k++] = data[i++];
    }
    while (j <= r) {
      // 뒤의 배열에만 원소가 남아있을 때
      tmp[k++] = data[j++];
    }

    for (int i = p; i <= r; i++) {
      data[i] = tmp[i];
    }
  }
}
```

* * *
