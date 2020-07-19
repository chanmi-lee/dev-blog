---
layout: post
title: "[번역] 자바스크립트 이벤트 루프"
excerpt: "#JavaScript #EventLoop #자바스크립트 #이벤트루프"
categories: [글또, JavaScript]
share: true
comments: true
---

> 이 글은 원저자 [Lydia Hallie](https://dev.to/lydiahallie) 의 허락을 받아 [✨♻️ JavaScript Visualized: Event Loop](https://dev.to/lydiahallie/javascript-visualized-event-loop-3dif) 을 번역한 글입니다

### 이벤트 루프 이해하기

`이벤트 루프`! 
모든 JavaScript 개발자들이 알아야 하는 개념 중 하나지만, 처음에는 다소 그 개념을 이해하기가 혼란스러울 수 있습니다.

저는 시각적 학습자로서 이벤트 루프를 시각적인 방법을 통해 이해하고자 합니다.

먼저, 이벤트 루프란 무엇이며 왜 관심을 가져야 할까요?

JavaScript는 `싱글 스레드`로, 한 번에 하나의 작업만 실행할 수 있습니다.
즉 하나의 함수가 실행되면 해당 함수의 실행이 끝날 때까지는 다른 어떤 작업도 중간에 끼어들지 못함을 의미합니다.
일반적으로 이는 큰 문제는 아니지만, 30초라는 긴 시간이 걸리는 어떤 작업을 실행하고 있다고 생각해봅시다.

해당 작업이 끝날 동안 다른 작업들은 대기하고 있어야 합니다.
(JavaScript는 브라우저의 메인 스레드에서 실행되므로, 전체 UI 렌더링 역시 중단됩니다 😬)

2020년인 지금- 원문에는 2019년-, 어느 누구도 느리게 반응하는 웹 사이트를 원하지 않습니다.

일반적으로 자바스크립트가 사용되는 환경을 생각해보면 동시에 많은 작업들이 처리되곤 합니다.
예를 들어, Node.js 기반의 웹 서버는 동시에 여러 개의 HTTP 요청을 처리하기도 하며,
웹 브라우저에서는 클릭이나 스크롤 등의 마우스 입력을 처리하면서 동시에 애니메이션 효과를 보여주기도 합니다.

다행이도, 브라우저는 JavaScript 엔진 자체에서 제공하지 않는 일부 기능인 `Web API`를 제공합니다.
Web API에는 `DOM API, setTimeout, HTTP requests` 등이 포함됩니다.
이들은 비동기적이고 (async), non-blocking한 동작들을 만드는데 도움이 됩니다.

함수를 호출할 때, 이는 `호출 스택 (call stack)` 이라고 불리는 곳에 추가됩니다.
호출 스택은 JavaScript 엔진의 일부로, 브라우저별로 그 명세에 차이가 있습니다.
어쨌거나, 호출 스택은 이름 그대로 **스택**으로, Last-In, First-Out (LIFO) 구조를 가집니다.
함수가 값을 반환하면, 그 값은 스택으로부터 튀어 나오게 됩니다. 👋

![event-loop-in-javascript]({{ site.url }}/assets/img/posts/event-loop-call-stack.gif)

위에서 `respond` 함수는 `setTimeout` 함수를 반환합니다.
`setTimeout`은 Web API에 의해 제공되며, 메인 스레드를 차단하지 않고 작업을 지연시킬 수 있습니다.

`setTimeout` 함수에 전달한 콜백 함수인 화살표 함수 `() => { return 'Hey' }`는 Web API에 추가됩니다.
두 번째 인자로 전달한 값에 따라, 해당 콜백 함수는 일정 시간 그 작업이 지연된 후 실행됩니다.
setTimeout 함수와 respond 함수는 스택에서 튀어 나와, 각각의 값을 반환하게 됩니다.

![event-loop-in-javascript]({{ site.url }}/assets/img/posts/event-loop-call-stack2.gif)

Web API에서, 타이머는 전달된 두 번째 인수 값인 1000ms 동안 실행됩니다.
콜백은 즉시 호출 스택에 추가되지 않는 대신, `태스크 큐 (Task Queue)`라는 곳에 전달됩니다.

![queue]({{ site.url }}/assets/img/posts/event-loop-queue.gif)

여기가 혼란스러운 부분입니다.
1000ms 후에 콜백 함수가 호출 스택에 추가된다는 것을 의미하지 않습니다.
1000ms 후에 `태스크 큐에 추가됩니다`.
태스크 큐는 말 그대로 콜백 함수들이 대기하는 큐(FIFO:First-in, First-Out) 형태의 배열입니다.

이제 우리가 기다렸던 부분입니다. **이벤트 루프는 콜백 함수가 담긴 큐를 호출 스택에 연결합니다**.
만약 호출 스택이 비어있다면 (즉 이전에 호출된 모든 함수가 그 값을 반환하고 모두 스택에서 튀어 나온 상태라면), 이벤트 루프는 큐에서 호출 스택을 꺼내와서 이를 호출 스택에 추가합니다.
이 경우에는 다른 함수는 호출되지 않았는데, 이는 콜백 함수가 이벤트 루프에 의해 큐의 첫 번째 항목으로 추가되기 전까지 호출 스택이 비어있음을 의미합니다.

> MDN의 이벤트 루프 관련 내용을 보면, 왜 이벤트 **'루프'** 라는 이름이 붙었는지를 간단한 코드와 함께 설명하고 있습니다.
> `waitForMessage` 메소드는 현재 실행 중인 작업이 없을 때 (즉, 호출 스택이 비어있을 때), 다음 작업이 태스크 큐에 추가될 때까지 대기하는 역할을 합니다. 

```js
while (queue.waitForMessage()) {
   queue.processNextMessage()
 }
```

![queue]({{ site.url }}/assets/img/posts/event-loop-queue2.gif)

콜백 함수가 호출 스택에 추가되면, 호출되고 그 결과 값을 반환한 뒤 스택으로부터 튀어나오게 됩니다.

![queue]({{ site.url }}/assets/img/posts/event-loop-queue3.gif)

### 예제

다음을 실행하면 콘솔에 어떻게 출력될까요?

```js
const foo = () => console.log("First");
const bar = () => setTimeout(() => console.log("Second"), 500);
const baz = () => console.log("Third");

bar();
foo();
baz();
```

이해 되셨나요?
브라우저에서 해당 코드를 실행할 때 어떤 일이 발생하는지 좀 더 자세히 알아봅시다.

![example]({{ site.url }}/assets/img/posts/event-loop-example.gif)

1. `bar` 함수를 호출합니다. bar는 `setTimeout` 함수를 반환합니다.
2. 전달된 setTimeout 콜백 함수는 Web API (혹은 백그라운드라고 불리기도 함)에 추가되고, setTimeout과 bar 함수는 실행 스택으로부터 튀어나오게 됩니다.
3. 타이머가 실행되고, 그 동안 `foo`가 호출되어 `First`가 콘솔에 기록됩니다. `foo`는 return 구문이 없어 그 반환값은 `undefined`입니다. 이어서 `baz`가 호출되고, 콜백 함수는 큐에 추가됩니다.
4. `baz`의 `Third`가 콘솔에 기록됩니다. 이벤트 루프는 호출 스택이 비었음을 확인하고, 큐에 있는 콜백 함수를 호출 스택에 추가합니다.
5. 콜백 함수의 `Second`가 콘솔에 기록됩니다.

위의 내용을 통해 이벤트 루프에 대해 좀 더 익숙해졌길 바랍니다 💪🏼

---

🔗 참조

📌 [JavaScript Visualized:: Event Loop](https://dev.to/lydiahallie/javascript-visualized-event-loop-3dif)

📌 [Concurrency model and the event loop:: MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)

📌 [Nodejs Event Loop :: StackOverflow](https://stackoverflow.com/questions/10680601/nodejs-event-loop)
