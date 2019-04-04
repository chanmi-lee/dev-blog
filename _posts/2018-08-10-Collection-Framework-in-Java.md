> #### Collection framework
* * *

**ArrayList** <br />
동적으로 크기가 조절되는 배열

```
ArrayList arr = new ArrayList();
arr.add("one");
arr.add("two");
System.out.println(arr.get(0));
```


**Vector** <br />
모두 List 인터페이스를 구현하고, 삽입 순서를 유지한다는 점, 동적으로 크기가 조절되는 배열을 필요로 할 때 사용된다는 점에서 ArrayList와 비슷하다.

> **Vector는 동기화(synchronnize)되어 있다는 점에서 차이가 있다.**
*즉, 한 번에 하나의 스레드만 Vector 메서드를 호출할 수 있다. (Thread-safe)*


**LinkedList** <br />
순환자(iterator)를 어떻게 사용하는지 잘 보여줌

```
LinkedList likedList = new LinkedList();
linkedList.add("two");
linkedList.addFirst("one");
Iterator iter = linkedList.iterator();
while(iter.hasNext()) {
  System.out.println(iter.next());
}
```

**HashMap** <br />
key-value 쌍으로 이루어진다. <br />
검색과 삽입에 O(1) 시간이 소요된다. <br />
키를 기준으로 순회할 때 키의 순서는 무작위로 섞여있다.

> Q2-1. TreeMap, HashMap, LinkedHashMap의 차이?

세 가지 모두 key-value의 대응 관계가 있고, key를 기준으로 순회(iterate) 가능하다. <br />
시간 복잡도와 키가 놓이는 순서에 차이가 있다.

| HashMap | Tree Map | LinkedHashMap |
| ---- | ---- | ---- |
| 검색/삽입에 O(1) | 검색/삽입에 O(logN) | 검색/삽입에 O(1) |
| 키 순서 무작위 | 정렬된 순서로 키 순회 가능 | 키는 삽입된 순서대로 정렬 <br /> 양방향 연결 버킷(double-linked bucket)으로 구현 |


**Stack** <br />
말 그대로 데이터를 쌓아올린다(stack)는 의미 <br />
LIFO(Last-In-First-Out)에 따라 자료를 배열

- ```pop()```: 가장 위에 있는 항목을 제거
- ```push()```: item 하나를 가장 윗 부분에 추가
- ```peek()```: 가장 위에 있는 항목을 반환
- ```isEmpty()```: 스택이 비어있을 때 true 반환

*같은 방향에서 item을 추가/삭제한다는 조건 하에 LinkedList로 구현할 수도 있다.*

```
public class MyStack {
  private static class StackNode {
    private T data;
    private StackNode next;
    public StackNode(T data) {
      this.data = data;
    }
  }

  private StackNode top;
  public T pop() {
    if (top == null) throw new EmptyStackException();
    T item = top.data;
    top = top.next;
    return item;
  }

  public void push(T item) {
    StackNode t = new StackNode(item);
    t.next = top;
    top = t;
  }

  public T peek() {
    if (top == null) throw new EmptyStackException();
    return top.data;
  }

  public boolean isEmpty() {
    return top == null;
  }
}
```


**Queue** <br />
FIFO (First-In-First-Out) like 매표소 줄

- ```add(item)```: item을 리스트의 끝 부분에 추가
- ```remove()```: 리스트의 첫 번째 항목 제거
- ```peek()```: 가장 위에 있는 항목 반환
- ```isEmpty()```: 큐가 비어있을 때 true 반환

* * *
