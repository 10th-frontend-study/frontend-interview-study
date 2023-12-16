# 이벤트 버블링과 캡처링에 대해 설명해주세요.

</br>

## 이벤트 버블링 ( Event Bubbling )

**한 요소에서 발생한 이벤트가 최상단의 부모 요소를 만날 때 까지 반복되면서 핸들러가 동작하는 현상 (기본값)**

```html
<body>
  <div class="DIV1">
    DIV1
    <div class="DIV2">
      DIV2
      <div class="DIV3">DIV3</div>
    </div>
  </div>
</body>
```

```jsx
const divs = document.querySelectorAll('div');

const clickEvent = (e) => {
  console.log(e.currentTarget.className);
};

divs.forEach((div) => {
  div.addEventListener('click', clickEvent);
});
```

**⇒ DIV3을 클릭하면 콘솔에는 DIV3, DIV2, DIV1 순서대로 출력**

![Untitled](/resources/bubbling.png)

**버블링 되지 않는 이벤트**

- 포커스 이벤트: focus/blur
- 리소스 이벤트: load/unload/abort/error
- 마우스 이벤트: mouseenter/mouseleave

</br>

## 이벤트 캡처링 ( Event Capturing )

**버블링과는 반대로 최상단의 부모 요소부터 시작하여 이벤트가 발생한 자식 요소까지 반복되면서 핸들러가 동작하는 현상**

```jsx
const divs = document.querySelectorAll('div');

const clickEvent = (e) => {
  console.log(e.currentTarget.className);
};

divs.forEach((div) => {
  div.addEventListener('click', clickEvent, { capture: true });
});
```

**⇒ DIV3을 클릭하면 콘솔에는 DIV1, DIV2, DIV3 순서대로 출력**

`addEventListener` 의 옵션 객체에 `{ capture: true }` 또는 `true` 를 설정하여 캡처링을 구현

![Untitled](/resources/capturing.png)

</br>

### 버블링 + 캡처링 동시 설정

가장 안쪽의 자식 요소를 클릭하면 이벤트 리스너 호출 순서는 마치 순회하는 형태

```jsx
const divs = document.querySelectorAll('div');

const clickEvent = (e) => {
  console.log(e.currentTarget.className);
};

divs.forEach((div) => {
  div.addEventListener('click', clickEvent, { capture: true });
  div.addEventListener('click', clickEvent);
});
```

1. DIV1 → DIV2 (캡처링 단계)
2. DIV3 ( 타깃 단계, 캡쳐링과 버블링 둘 다에 리스너를 설정했기 때문에 두번 호출)
3. DIV2 → DIV1 (버블링 단계)

**⇒ DIV3을 클릭하면 콘솔에는 DIV1, DIV2, DIV3, DIV3, DIV2, DIV1 순서대로 출력**
