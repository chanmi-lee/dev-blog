---
layout: post
title: "useMemo in React"
excerpt: "#react #hook #useMemo #memoization"
categories: [글또, React]
share: true
comments: true
---

### useMemo란?

`useMemo` 함수는 React v16.8부터 내장되어 있는 Hooks의 하나입니다.
Hook이란 함수형 컴포넌트에서 State, LifeCycle, Reference 등의 클래스형 컴포넌트의 기능을 구현한 개념으로, 이를 통해 memoization 기법을 함수형 컴포넌트에도 쉽게 적용할 수 있습니다.

> **memoization**는 알고리즘 성능 최적화에 자주 쓰이는 개념 중 하나로, 이름 그대로 메모를 하는 것을 의미합니.
프로그래밍에서 반복되는 결과를 메모리에 저장해두고 동일한 연산이 필요한 경우 이를 재활용하는 기법을 의미합니다.
이를 적절히 활용하면 중복을 피하고 애플리케이션의 속도를 향상시킬 수 있다는 이점이 있습니다.

React에서 컴포넌트 랜더링은 빈번하게 일어나는데, 대표적인 사례로는 컴포넌트 자신의 상태 변경 혹은 부모 컴포넌트의 상태 변경의 영향으로 함께 리랜더링 되는 경우 등이 있습니다.
만약 컴포넌트의 리랜더링에 불필요하게 오랜 시간이 걸린다면, 사용자는 UI에서 지연이 발생하는 경험을 하게 됩니다.

```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

`useMemo` 함수는 **2개의 인자**를 받습니다.
첫번째 인자로는 결과값을 생성해내는 팩토리 함수, 즉 리턴 콜백 함수를 받습니다.
두번째 인자는 결과값을 재활용할 때 기준이 되는 입력값 배열입니다.
두번째 인자로 전달된 값이 이전과 다른 경우에만 첫번째 값이 실행되며, 이를 통해 함수형 컴포넌트 내에서 계산되는 값에 대한 최적화를 할 수 있습니다.

`useMemo`에 전달된 함수는 랜더링 중에 실행됩니다.
때문에 랜더링과 관련된 내용만 전달해야하며, 랜더링과 관계 없는 경우는 `useEffect`를 사용하길 권장합니다.

> 전달되는 콜백 함수의 이름이 `computeExpensiveValue`라는데서 짐작할 수 있지만,
`useMemo`를 통해 재활용되는 로직이 복잡한 경우 불필요한 연산을 막을 수 있고 성능상의 차이를 가져올 수 있습니다.

---

### useMemo 예제

간단한 예제와 함께 살펴보겠습니다.

> without useMemo

```jsx
function Counter() {
    const [count, setCount] = React.useState(0)
    const doubleCount = count * 2

    return (
        <div>
            <h1>React Hooks: useMemo example</h1>
            <button onClick={() => setCount(count)}>Double Counter: {count}</button>
        </div>
    )
}
```

버튼을 클릭할 때마다 이전보다 두 배 큰 값이 반환됩니다.
하지만 count 값과 상관없이 컴포넌트가 리랜더링 되었을 때 불필요하게 연산을 수행하게 됩니다.
간단한 사례지만, 이보다 더 복잡한 연산이거나 컴포넌트의 상태값이 많은 경우 그만큼의 리소스가 낭비될 수 있습니다.

> with useMemo

```jsx
function Counter() {
    const [count, setCount] = React.useState(0)
    const doubleCount = React.useMemo(() => counter * 2, [count])

    return (
        <div>
            <h1>React Hooks: useMemo example</h1>
            <button onClick={() => setCount(count)}>Double Counter: {count}</button>
        </div>
    )
}
```

`useMemo`를 사용한다면, 의존하는 값(count)이 변경되는 경우만 연산되므로 불필요한 연산을 막을 수 있습니다.

### useMemo, 언제 필요할까?

일반적으로 memoization의 기법을 활용한 컴포넌트 성능 최적화는 잘 사용되지 않습니다.
React의 reconciliation과 Virtual DOM이 충분히 빠르고, 대부분의 사용자가 체감할 정도의 성능 최적화가 필요한 상황이 그리 자주 있지 않기 때문입니다.
또한 실행되는 모든 코드는 그만큼의 비용을 요구하기 때문에, 무분별한 함수 호출은 리소스 낭비로 이어져 성능 개선이 아닌 악화로 이어질 수 있습니다.

만약 프론트엔드 단에서 처리해야만 하는 복잡한 연산이 필요한 경우가 있다고 하더라도,
가장 먼저 `useMemo`나 `useCallback`와 같은 최적화 기법을 도입하는 것은 좋지 않습니다.
대신, `useEffect` 함수 등을 이용해 비동기로 처리하는 방안을 먼저 고려하거나 클래스 기반의 컴포넌트인 경우 `shouldComponentUpdate`를 사용해보는건 어떨까요?

---

🔗 참조

📌 [Hook API reference - useMemo :: React Official Docs](https://reactjs.org/docs/hooks-reference.html#usememo)
📌 [React - Reconciliation :: React Official Docs](https://reactjs.org/docs/reconciliation.html)