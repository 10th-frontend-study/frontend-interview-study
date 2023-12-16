# ES6에서 생긴 큰 변화들에 대해 설명해 주세요.

## 1. ECMAScript

- 자바스크립트가 만들어진 초기에, 브라우저마다 자바스크립트를 해석하는 방법이 달랐기 때문에 문제가 발생했다.
- 따라서 모든 브라우저가 동일한 방식으로 자바스크립트를 해석하고 실행할 수 있도록 표준을 만들었는데, 이게 `ECMAScript`이다.
  - 자바스크립트 언어가 어떤 문법을 가지고 어떤 기능을 제공하는지를 정의하는 표준이다.

## 2. 주요 변화

### 1. let, const

- ES5까지는 주로 `var`를 통해 변수를 선언했지만, ES6 부터는 더이상 `var`을 사용하지 않고 `const`와 `let`을 사용한다.
- 다음 예시를 통해 let과 const를 사용하는 이유를 알 수 있다.

  ```jsx
  if (true) {
    var x = "var로 선언한 변수입니다.";
  }
  console.log(x); // (출력값) var로 선언한 변수입니다.

  if (true) {
    const y = "const로 선언한 변수입니다.";
  }
  console.log(y); // (출력값) ReferenceError: y is not defined
  ```

  - 여기서 y는 출력이 되지 않는데, 이는 y가 블록 스코프를 가지기 때문이다.

- 그렇다면 `let`과 `const`의 차이점은?
  - `const` : 상수이기 때문에 재할당 불가
  - `let` : 재할당 가능

### 2. 템플릿 리터럴

- ES5까지는 큰 따옴표와 작은 따옴표로 문자열을 나타냈지만, ES6에서는 **백틱**을 활용하여 문자열 안에 변수를 넣는 형태로 나타낼 수 있다.

> 템플릿 리터럴 사용 예시

```jsx
// ES5
var num1 = 1;
var num2 = 2;
var result = 3;

var string1 = num1 + " 더하기 " + num2 + "는 '" + result + "'입니다.";
console.log(string1);

// ES6
const new1 = 1;
const new2 = 2;
const newResult = 3;

const string2 = `${num1} 더하기 ${num2}는 '${result}'입니다.`;
console.log(string2);
```

### 3. 객체 리터럴

> 기존 ES5 문법으로 객체를 정의

```jsx
var hello = function() {
  console.log('hello');
};

var mark = 'MARK';

**var obj = {
  inside: function() {
    console.log('객체 안에 있는 함수');
  },
  hello: hello,
};**

obj[mark + 1] = '아이언맨';

obj.inside(); // (출력값) **객체 안에 있는 함수**
obj.hello();  // (출력값) hello
console.log(obj.MARK1); // (출력값) 아이언맨
```

> ES6로 변경

```jsx
const hello = function() {
  console.log('hello');
};

const mark = 'MARK';

**const obj = {
  inside() {
    console.log('객체 안에 있는 함수');
  },
  hello,
  [mark + 1]: '아이언맨',
};**

obj.inside(); // (출력값) **객체 안에 있는 함수**
obj.hello(); // (출력값) hello
console.log(obj.MARK1); // (출력값) 아이언맨
```

- 함수 앞에 `:` 이나 `function`을 붙이지 않아도 된다.
- 함수 이름이 겹치는 경우 한 번만 쓰면 된다.
- 객체의 프로퍼티를 동적으로 생성하기 위해서는 객체 밖에서 선언했어야 했으나, 객체 내부에서 선언이 가능하다.

### 4. 화살표 함수

- 이전에 설명했으니 스킵

```jsx
const add1 = function (x, y) {
  return x + y;
};

const add2 = (x, y) => {
  return x + y;
};

const add3 = (x, y) => x + y;

const square = (x) => x ** 2;
```

### 5. 비구조화 할당

- 객체와 배열로 부터 프로퍼티를 쉽게 꺼낼 수 있는 문법이다.

> 다음 예시를 통해 비구조화 할당을 사용하는 이유를 알 수 있다.

```jsx
let obj = {
  name: "이수화",
  age: 25,
  bag: {
    item_1: "지갑",
    item_2: "맥북",
  },
};

const name = obj.name;
const item1 = obj.bag.item1;
```

> 아래와 같은 문법으로 바꿀 수 있다.

```jsx
let obj = {
  name: "이수화",
  age: 25,
  bag: {
    item_1: "지갑",
    item_2: "맥북",
  },
};

const {
  name,
  bag: { item_1 },
} = obj;
```

> 다음은 배열의 비구조화 할당 예시이다.

```jsx
const arr = ["string", {}, 1, true];

const [str, obj, , bool] = arr;
```

- 배열은 순서대로 값이 비구조화 할당이 된다.

> 다음은 비구조화 할당을 이용한 함수 선언 예시이다.

```jsx
let obj = {
  name: '이수화',
  age: 25,
  bag: {
    item_1: '지갑',
    item_2: '맥북',
  },
};

const test = (**{ name, age, bag }**) => {
  console.log(`name: ${name}, age: ${age}, bag: ${bag}`);
};

test(obj);
```

- test 함수 안에 obj를 인자로 넘기면 test 함수에서는 obj 함수의 인자를 비구조화 할당으로 받아서 따로 변수에 객체의 요소를 나눠야 하는 코드를 최소화 할 수 있다.

### 참조

https://ooeunz.tistory.com/88
