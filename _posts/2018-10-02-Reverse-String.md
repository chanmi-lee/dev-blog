---
layout: post
title: "Reverse String"
excerpt: "from LeetCode"
categories: [Algorithm]
share: true
comments: true
---

> #### Reverse String
* * *

Write a function that reverses a string. The input string is given as an array of characters char[].

Do not allocate extra space for another array, you must do this by **modifying the input array** [in-place](https://en.wikipedia.org/wiki/In-place_algorithm) with O(1) extra memory.

You may assume all the characters consist of printable ascii characters.


Example 1:

```
Input: ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```

Example 2:

```
Input: ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
```

* * *

My solution:

```
class Solution {
    public void reverseString(char[] s) {

        for (int i = 0; i < s.length/2; i++) {
            char temp = s[i];
            s[i] = s[s.length-i-1];
            s[s.length-i-1] = temp;
        }

    }
}
```

* * *

**Lessen learned**

> In-place algorithm

: In-place algorithm is an algorithm which transforms input using no auxiliary data structure. In-plcae algorithm updates input sequence *only through replacement or swapping of elements* (@Wikipedia)

- 원본 데이터 공간에서 Sorting, 즉 Sorting을 위한 추가 저장 공간이 필요하지 않다는 말을 의미한다
- Swap을 위한 상수크기 정도의 공간은 필요하다
- 예: 선택 정렬(Selection Sort), 퀵 정렬(Quick Sort)

---

Ref
- [Reverse String - LeetCode](https://leetcode.com/problems/reverse-string)
- [In-place algorithm - wikipedia](https://en.wikipedia.org/wiki/In-place_algorithm)
