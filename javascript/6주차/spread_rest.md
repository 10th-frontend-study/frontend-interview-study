# spread문법과 rest문법의 차이에 대해 설명해주세요.

<aside>
💡 두 문법 모두 … 을 사용함
</aside>

## **Spread Operator (스프레드 연산자)**

- **배열과 같은 복수개의 데이터를 가진 데이터형을 `, , ,`으로 구분되는 여러개의 요소로 `펼치는`(spread) 곳에 사용**
- **객체 혹은 배열에서 사용가능**

### 활용

**배열 복사**

```java
const arr = ["a", "b", "c", "d"]
const arr2 = [...arr] // 배열 복사

// arr2 = ["a", "b", "c", "d"]
```

**배열 연결**

```java
const arr = ["a", "b"]
const arr1 = ["c", "d"]
const arr2 = [...arr, ...arr1] // 두 배열의 요소를 합친 새로운 배열 생성

// arr2 = ["a", "b", "c", "d"]
```

**배열 중간에 요소 추가**

```java
const arr = ["b", "c"]
const arr1 = ["a", ...arr, "d"] // 중간에 다른 배열의 요소를 추가하여 새로운 배열 생성

// arr2 = ["a", "b", "c", "d"]
```

**객체에 새로운 property추가 및 수정**

```java
const obj = { a: 1, b: 2, c: 3 }
const obj1 = {
  ...obj,
  c: 100, // 기존의 property를 한 번더 정의해줌으로써 property를 수정할 수 있다.
  d: 200, // 새로운 property를 추가할 수 있다.
}

// obj1 = { a: 1, b: 2, c: 100, d: 200}
```

### 객체를 복사할 때 중복된 키 값이 있다면?

```java
const obj = {
  name : '이름',
  age : '나이'
}

const obj2 = {
  name : '아이유',
  age : 30,
}

const newObj = {
  ...obj,
  ...obj2,
}

console.log(newObj); // ???
```

- **출력값이 어떻게 될까요?**

  **동일한 키값은 마지막에 적은 객체를 기준으로 덮어쓰기**

  **{name: "아이유", age: 30} 출력 !!**

---

<br>

## Rest Parameter ( 나머지 매개변수 )

- **앞 쪽에서 받은 인자를 제외한 `나머지(rest)` 인자들을 배열로 `합쳐서` 받을 수 있게 해줌**
- **`객체`, `배열`, 함수의 `파라미터`에서 사용 가능**

<br>

**객체에서의 rest**

```jsx
const purpleCuteSlime = {
  name: '슬라임',
  attribute: 'cute',
  color: 'purple',
};

const { color, ...rest } = purpleCuteSlime;
console.log(color);
console.log(rest);

// rest 변수명이 꼭 rest일 필요는 없음
```

![Untitled](/resources/spread_rest/rest1.png)

**배열에서의 rest**

```jsx
const numbers = [0, 1, 2, 3, 4, 5, 6];

const [one, ...rest] = numbers;

console.log(one);
console.log(rest);
```

![Untitled](/resources/spread_rest/rest2.png)

\***\*함수 파라미터에서의 rest\*\***

```jsx
function foo(a, ...rest) {
  console.log(rest); // ["a","b","c"]
}
foo('a', 'b', 'c', 'd');
```

## 주의할점

- **앞쪽에서 rest를 먼저 정의할 수 없음**
- **매개변수의 가장 마지막에 작성되어야 함**

```jsx
const numbers = [0, 1, 2, 3, 4, 5, 6];
const [..rest, last] = numbers;
```

![Untitled](/resources/spread_rest/warning.png)

<br>

## 정리

- **spread operator는 `펼치고`, rest parameter는 `모은다`.**
- **spread operator는 `기존의 변수`를 사용하고, rest parameter는 `새로운 변수`를 만든다.**
