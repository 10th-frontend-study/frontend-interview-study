## 함수 선언식 - Function Declarations

일반적인 프로그래밍 언어에서의 함수 선언과 비슷한 형식이다.

```jsx
function 함수명() {
  구현 로직
}
```

```jsx
// 예시
function funcDeclarations() {
  return 'A function declaration';
}
funcDeclarations();// '함수 선언식'
```

<br/>

## 함수 표현식 - Function Expressions

유연한 자바스크립트 언어의 특징을 활용한 선언 방식

```jsx
let 함수명 = function () {
  구현 로직
};
```

```jsx
// 예시
var funcExpression = function () {
    return 'A function expression';
}
funcExpression();// '함수 표현식'
```

<br/>

## 함수 선언식과 표현식의 차이점

#### 💡 함수 선언식은 호이스팅에 영향을 받지만, 함수 표현식은 호이스팅에 영향을 받지 않는다.


함수 선언식은 코드를 구현한 위치와 관계없이 자바스크립트의 특징인 **호이스팅**의 영향으로 브라우저가 자바스크립트를 해석할 때 맨 위로 끌어 올려진다.

예를 들어, 아래의 코드를 실행할 때

```jsx
// 실행 전
logMessage();
sumNumbers();

function logMessage() {
  return 'worked';
}

var sumNumbers = function () {
  return 10 + 20;
};
```

호이스팅에 의해 자바스크립트 해석기는 코드를 아래와 같이 인식한다.

```jsx
// 실행 시
function logMessage() {
  return 'worked';
}

var sumNumbers;

logMessage();  // 'worked'
sumNumbers();  // Uncaught TypeError: sumNumbers is not a function

sumNumbers = function () {
  return 10 + 20;
};
```

위 코드 결과는 아래와 같다.

!https://joshua1988.github.io/images/posts/web/javascript/function-expressions-declarations/error.png

함수 표현식 `sumNumbers` 에서 `var` 도 호이스팅이 적용되어 위치가 상단으로 끌어올려졌다.

```jsx
var sumNumbers;

logMessage();
sumNumbers();
```

하지만 실제 `sumNumbers` 에 할당될 `function` 로직은 호출된 이후에 선언되므로, `sumNumbers` 는 함수로 인식하지 않고 변수로 인식한다.

`호이스팅`을 제대로 모르더라도 함수와 변수를 가급적 코드 상단부에서 선언하면, `호이스팅`으로 인한 스코프 꼬임 현상은 방지할 수 있다.

<br/>

## 함수 표현식의 장점

`함수 표현식이 호이스팅에 영향을 받지 않는다` 는 특징 이외에도 함수 선언식보다 유용하게 쓰이는 경우는 다음과 같다.

- 클로저로 사용
- 콜백으로 사용 (다른 함수의 인자로 넘길 수 있음)

### 함수 표현식으로 클로저 생성하기

> 클로저란?
'클로저'란 다른 함수의 스코프에 있는 변수에 접근 가능한 함수
> 

```jsx
function createComparisonFunction(propertyName) {
	
    return function(object1, object2) {
    	var value1 = object1[propertyName];
        var value2 = object2[propertyName];
        
        if (value1 < value2) {
        	return -1;
        } else if ( value1 > value2) {
        	return 1;
        } else {
        	return 0;
        }
    }
}

// @ 함수 표현식으로 사용된 클로저
var compareNames = createComparisonFunction("name");

var result = compareNames({ name : "Nick" }, { name : "Greg" });

console.log(result);

compareNames = null;
```

실행 결과)

```
1
```

<br/>

### 함수 표현식을 다른 함수의 인자 값으로 넘기기

함수 표현식은 일반적으로 임시 변수에 저장하여 사용한다.

```jsx
// doSth 이라는 임시 변수를 사용
var doSth = function () {
	// ...
};
```

함수 표현식을 임시 변수에 넣지 않고도 아래와 같이 콜백함수로 사용할 수 있다.

```jsx
$(document).ready(function () {
  console.log('An anonymous function');  // 'An anonymous function'
});
```

jQuery 를 사용할 때 많이 보던 문법으로 위와 아래의 코드 결과는 같다.

```jsx
var logMessage = function () {
  console.log('An anonymous function');
};

$(document).ready(logMessage);// 'An anonymous function'
```

자바스크립트 내장 API 인 `forEach()` 를 사용할 때도 콜백함수를 사용할 수 있다.

```jsx
var arr = ["a", "b", "c"];
arr.forEach(function () {
	// ...
});
```

<br/>

## 결론

함수 표현식이 선언식에 비해 가지는 장점이 많지만, 결국에는 이러한 차이점을 인지한 상태에서 일관된 코딩 컨벤션으로 코드를 작성하는 게 중요하다. [AirBnb 의 JS Style 가이드](https://github.com/airbnb/javascript) 에서도 함수 선언식보다는 함수 표현식을 지향하고 있는 것을 볼 수 있다.