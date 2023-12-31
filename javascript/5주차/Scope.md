# 스코프와 스코프체인에 대해 설명해주세요.

## 1. 정의

<aside>
💡 Scope란, 우리가 변수나 함수를 선언할 때 해당 변수나 함수가 유효한 범위를 말한다.

</aside>

```jsx
var x = "global";

function foo() {
    var x = "function scope";
    console.log(x);
}

foo(); // ?
console.log(x); // ?
```

-   여기서 x가 두 번 선언되었지만, JS는 각각 x를 다른 값으로 해석한다.
-   함수 안에 있는 x는 함수 안에서만 참조 가능하고, 전역에 선언된 x는 어디서든 참조할 수 있기 때문에 다른 값으로 해석되는 것이다. 이렇게 식별자를 찾는 규칙을 `스코프`라고 한다.

## 2. 구분

1. **Global Scope**(전역 스코프) : 코드의 모든 범위에서 사용 가능
2. **Local Scope**(지역 스코프)
    1. **Function Scope**(함수 스코프) : 함수 안에서만 사용 가능
    2. **Block Scope**(블록 스코프) : if, for, switch등 특정 블록 내에서만 사용 가능

> 대부분의 C기반 언어들은 블록 레벨 스코프를 따르지만, `JS`는 `함수 레벨 스코프`를 따른다.

### 1. 전역 스코프

<aside>
💡 코드의 어느 곳에서든 참조할 수 있는 범위이다.

</aside>

```jsx
let x = "global";

function foo() {
    console.log(x);
}

foo(); // global
console.log(x); // global
```

### 2. 함수 레벨 스코프

<aside>
💡 함수 블록 내에서만 참조 가능한 범위를 말한다. ⇒ 동일 함수 내에서는 동일한 변수를 공유한다.

</aside>

```jsx
function test() {
  var x = 10;
  if(true) {
    var x = 20;
    console.log(x); // 20
  }
  **console.log(x); // 20**
}

test();
```

### 3. 블록 레벨 스코프

<aside>
💡 코드 블록 내에서만 참조 가능한 범위를 말한다. ⇒ 동일 블록 내에서는 동일한 변수를 공유한다.

</aside>

```jsx
let y = 0; // let은 블록 레벨 스코프를 가진다
{
    let y = 1; // 이렇게 해도 덮어쓰지 않는다
    console.log(y); // 1
}
console.log(y); // 0
```

## 3. JS가 스코프를 정의하는 방법

### 1. 렉시컬 스코프

<aside>
💡 함수가 선언된 위치의 컨텍스트를 사용하여 스코프를 정의한다. (대부분의 언어가 이렇게 사용)

</aside>

```jsx
var foo = "Hello";

function baz() {
    console.log(foo); // Hello => 이 함수가 선언된 위치가 전역이라서 전역 변수에 할당된 Hello가 출력된다.
}

function bar() {
    var foo = "World";
    baz();
}

bar();
```

### 2. 동적 스코프

<aside>
💡 함수가 실행된 위치의 컨텍스트를 사용하여 스코프를 정의한다. (주로 Perl, Bash shell과 같은 언어들이 사용하는 방식)

</aside>

```jsx
foo="Hello"

baz() {
     echo "$foo" # World => 여기서 baz 함수가 bar함수 안에서 실행되었기 때문에 bar 함수 안에 있는 World가 출력
}

bar() {
    foo="World"
    baz # baz call
}

bar # bar call
```

---

## 4. 스코프 체인

<aside>
💡 블럭이 여러개 쌓이고, 함수 내부에 함수를 호출하다 보면 스코프의 중첩이 발생하게 되는데, 이때 식별자 선택의 모호함을 해결하기 위해 스코프 체인을 사용한다.

</aside>

### 중첩된 코드 예시

```jsx
// 전역 스코프
var x = "global x";

// outer 함수 지역 스코프
function outer() {
    var x = "outer x";
    var y = "outer y";

    // inner 함수 지역 스코프
    function inner() {
        var x = "inner x";

        // if 블럭 지역 스코프
        if (x) {
            const x = "if block scope";
            console.log(x); // if block scope
            console.log(y); // outer y
            console.log(z); // Uncaught ReferenceError: z is not defined
        }
    }

    inner();
}

outer();
```

-   여기서 if문 안에 있는 x를 출력할 때, 후보가 되는 변수들 중에 하나의 값을 선택해야 한다.
-   이때 스코프 체인을 활용하여 값을 참조한다.

### 스코프 체인을 통한 식별자 탐색

![Untitled](../../resources/Scope/image.png)

-   최상위 스코프는 전역 스코프이다.
-   하위 ⇒ 상위 **단방향 탐색**만 수행된다.
-   식별자 참조 시 JavaScript 엔진은 다음과 같은 과정을 수행한다.
    1. 식별자 탐색은 식별자를 참조하는 코드의 스코프에서 시작한다.
    2. 현재 스코프에 참조하려는 식별자가 없으면 상위 스코프를 탐색한다.
    3. 참조하는 식별자를 찾은 경우 탐색을 중지한다.
    4. 참조하는 식별자를 전역 스코프에서도 찾지 못한 경우에는 ReferenceError을 발생시킨다.

---

### 참조

스코프 : [https://doozi0316.tistory.com/entry/JavaScript-Scope란-스코프-체인-실행-컨텍스트](https://doozi0316.tistory.com/entry/JavaScript-Scope%EB%9E%80-%EC%8A%A4%EC%BD%94%ED%94%84-%EC%B2%B4%EC%9D%B8-%EC%8B%A4%ED%96%89-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8)

스코프 체인 : https://junhyunny.github.io/javascript/javascript-scope-chain/
