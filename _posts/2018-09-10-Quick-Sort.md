---
layout: post
title: "Quick Sort"
excerpt: ""
categories: [Algorithm]
share: true
comments: true
---

> #### Quick Sort
* * *

무작위로 선정된 한 원소를 사용하여 배열을 분할하는데, 선정된 원소보다 작은 원소들은 앞에, 큰 원소들은 뒤로 보낸다.

*배열 분할에 사용되는 원소가 중간값(median), 적어도 중간값에 가까운 값이 되리라는 보장이 없기 때문에, 정렬 알고리즘이 느리게 동작할 수도 있다.
**최악의 경우ㅡpivot 값을 기준으로 모든 값들이 한 편으로 몰리는 경우, 수행 시간이 O(n^2)이 될 수도 있다.*

```
public class QuickSortExample {
  void quickSort(int[] arr, int left, int right) {
    if (left >= right) return;  // 원소 1개인 경우

    int index = partition(arr, left, right);

    if (left < index - 1) {  // 왼쪽 절반 정렬
      quickSort(arr, left, index - 1);
    }
    if (index < right) {    // 오른쪽 절반 정렬
      quickSort(arr, index, right);
    }
  }

  int partition(int[] arr, int left, int right) {
    int pivot = arr[(left+right)/2]; // 분할 기준 원소 생성

    while (left <= right) {
      // 왼쪽에서 오른쪽으로 옮겨야 하는 원소 탐색
      while (arr[left] < pivot) left++;

      // 오른쪽에서 왼쪽으로 옮겨야 하는 원소 탐색
      while (arr[right] > pivot) right--;

      // 원소를 스왑한 뒤 left와 right를 이동
      if (left <= right) {
        swap(arr, left, right);
        left++;
        right--;
      }
    }
    return left;
  }

  void swap(int[] arr, int left, int right) {
    int temp = arr[left];
    arr[left] = arr[right];
    arr[right] = temp;
  }
}
```

* * *
