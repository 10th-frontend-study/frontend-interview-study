## **Event Loop**

`Call Stack`와 `task Queue`의 상태를 `주시`

`Call Stack`이 `빈 상태`가 되면 task Queue의 첫번째 콜백함수를 `Call Stack로` 보내주는 역할

<aside>
💡 자바스크립트는 싱글스레드 언이며 한번에 하나의 일, 콜 스택이 하나라는 것을 의미
</aside>

## 용어정리

### 힙 ( heap )

- 메모리 할당

### 콜 스택 ( Call Stack )

- 실행된 코드의 환경을 저장하는 자료구조
- `함수 호출` 시 Call Stack에 push

### Web API

- `브라우저`나 nodejs가 `제공`하는 web API
- 자바스크립트에서 호출할 수 있는 `스레드를 지원`
- setTimeout, AJAX과 같은 `비동기 작업`을 `실행`

### 블로킹( Blocking )

- 코드의 실행이 다른 코드의 실행을 막는 것
- 콜스택에 느리게 하는 동작이 남아있는 것
- 스택에 시간이 오래걸리는 작업을 쌓아서 브라우저가 할일을 막으면 안됨

![Untitled](../../resources/Event%20Loop/image.png)

### 순서

```jsx
console.log("first");

setTimeout(function cb() {
  console.log("second");
}, 1000);

console.log("third");
```

1. **call stack에 log(’first’) push**
2. **화면 출력 후 call stack에서 pop**
3. **setTimeout이 call stack에 push**
   1. **Browser가 제공하는 Web APIs에서 타이머를 실행**
   2. **call stack에서 pop**
   3. **타이머 실행이 끝나면 callback queue로 보냄**
4. **call stack에 log(’third’) push**
5. **화면 출력 후 call stack에서 pop**
6. **event loop가 call stack이 비어있는 것을 확인**
   1. **콜 스택에 setTimeout push**
   2. **call stack에 log(’second’) push**
   3. **화면 출력 후 call stack에서 pop**
   4. **setTimeout pop**

## 참조

[JavaScript 비동기 핵심 Event Loop 정리](https://medium.com/sjk5766/javascript-비동기-핵심-event-loop-정리-422eb29231a8)

[런타임 시각화 사이트](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4=)

[어쨌든 이벤트 루프는 무엇입니까?](https://www.youtube.com/watch?v=8aGhZQkoFbQ&t=80s)
