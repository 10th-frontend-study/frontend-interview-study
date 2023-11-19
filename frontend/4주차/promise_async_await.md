# 배경: 비동기 호출이 필요한 이유

### 🚀 퀴즈

- `getAnimals()` → 서버에서 동물의 데이터를 받아오는 함수
- 콘솔에는 무엇이 출력될지 생각해보자.

```jsx
function printAnimals() {
  const animals = getAnimals();
  console.log(animals);
}
```


> [!question]- 정답
> **콘솔에는 undefined 가 출력된다.**
> 그 이유는?
> 
> - js는 동기적인 언어이지만, 대기시간 긴 작업의 경우 비동기로 작업 한다.
> - 그렇기 때문에 `getAnimals()`보다 콘솔 출력이 먼저 발생하고, undefined가 출력되는 것이다.
>   
>   ```js
>   function printAnimals() {
> 	  const animals = getAnimals();  // 대기시간이 기네?
> 	  console.log(animals);   // 우선 실행합니다~
>   }
> 	  
>   printAnimals();// undefined
>   ```
>   그럼 만약 우리가 `getAnimals()`의 응답값(_response_)를 사용하고 싶다면 어떻게 해야할까?
>   이럴 때 비동기 처리방식인 `promise`, `async await`를 사용하면 된다.

> [!tips]- 동기 vs 비동기
> ### **동기적 방식**
> 병원 진료를 볼때에 창구에 접수를 하기 위해서 번호표를 뽑습니다.
> 저의 번호표는 3번으로 저의 앞에는 1번 2번 번호표를 가진 분이 존재합니다.
> 
> 1번 손님 접수요청 -> 창구에서 응답 -> 접수완료 -> 2번 손님 접수요청 -> 창구에서 응답 -> 접수완료 -> 3번 손님 접수요청 ....
> 
> 위의 예시와 같이 동기적 방식은 순서가 확실히 정해져있습니다.
> 
> ### **비동기적 방식**
> 식당을 가서 음식을 주문할 때에 음식이 조리중이라서 주문 접수가 안되는 식당은 없을 것 입니다.
> 그리고 만일 1번째 손님이 30분 걸리는 음식을 주문하고, 2번째 손님이 5분 걸리는 음식을 주문했을 때 2번째 손님의 음식이 먼저 나올 것 입니다. 이러한 것이 `비동기적 방식`입니다.
> 
> 만일 위의 예시에서 동기적 방식이라면, 2번째 손님은 30분을 기다려 음식을 주문해야하고 5분이라는 시간이 걸려서야 밥을 먹을 수 있게됩니다.


## Callback이란?

- 다른 함수가 실행을 끝낸 뒤 실행되는 함수를 말한다.
- 즉, 코드를 통해 명시적으로 호출하는 함수가 아니라 함수를 등록해놓은 후 어떤 이벤트가 발생했거나 특정 시점에 도달했을 때 시스템에서 호출하는 함수를 말한다.
- 파라미터로 함수를 전달 받아 함수의 내부에서 실행된다.

### 콜백함수의 사용 이유

- Js에서 비동기적 프로그래밍을 할 수 있기 때문이다.
- Js는 싱글스레드를 사용하는데 멈춤을 방지해준다. 즉 블록킹을 방지하여 싱글스레드가 논블록킹으로 동작하게 한다.
    - 원주) 이건 무슨 소리지?

> 💡 싱글스레드 : 싱글스레드는 한 번에 하나의 작업만 수행할 수 있다.

> 💡 싱글스레드로 어떻게 비동기 작업이 가능할까?
> 
> - 자바스크립트의 메인스레드인 이벤트 루프가 싱글스레드이기 때문에 자바스크립트를 싱글스레드 언어라고 부른다. 그러나 자바스크립트는 이벤트 루프만 독립적으로 실행하지는 않고 웹 브라우저나, NodeJs같은 멀티 쓰레드 환경에서 실행된다.
> - 즉 동시성을 보장하는 비동기, 논블록킹 작업들은 Js엔진을 구동하는 웹 브라우저, NodeJs에서 담당한다.
> - **이하 event loop 폴더 link 걸기**




# Promise

### Promise란?

- `Promise`는 비동기 처리를 가능하게 해주는 객체이다.
- ES6에서 도입된 비동기 처리 방식의 문법이다.
- `Promise` 에는 3가지 상태가 있는데
    - **Pending** (대기)
    - **Fulfilled** (이행)
    - **Rejected** (실패)

비동기 처리가 완료 되지 않았다면 `Pending`, 완료 되었다면 `Fulfilled`, 실패하거나 오류가 발생하였다면 `Rejected` 상태를 갖는다.

<aside> 💡 **Promise가 콜백을 대체하는 걸까?**

promise가 콜백을 대체하지는 않지만 콜백을 예측가능한 패턴으로 사용할 수 있게 하며 콜백만 사용했을 때의 예상치 못한 동작을 막아주거나 힘든 버그를 상당 수 해결해 준다.

</aside>

### Promise 만들기

```jsx
const promise = new Promise((resolve, reject) => {
	// 비동기 작업 성공시 resolve()를 호출하고,
  	// 비동기 작업 실패시 reject()를 호출하도록 한다.
})
```

```jsx
const promise = new Promise((resolve, reject) => {
	// 처리 내용
})

promise.then(
	//resolve가 호출되면 then이 실행
)
.catch(
	// reject가 호출되면 catch가 실행
)
.finally(
	// 콜백 작업을 마치고 무조건 실행되는 finally(생략가능)
)
```

- `Promise` 다음엔 `then()`과 `catch()`를 사용한다.
- `then()`은 생성한 프로미스 객체에서 인수로 전달한 `resolve`가 호출되면 실행된다.
- `catch()`는 생성한 프로미스 객체에서 인수로 전달한 `reject`가 호출되면 실행된다.

### Promise 객체로 비동기 처리 연결하기

then()과 catch()뒤에 또다른 then()과 catch를 연결함으로써 비동기 처리를 연결할 수 있다.

```jsx
const promise = new Promise((resolve, reject) => {
	// 처리 내용
})

promise.then( (data) => {
	return new Promise((resolve, reject) => {
    	// 처리 내용
    }). then((data2) => {
    	// 처리 내용
    }).catch(
    	console.log(ErrorMessage)
    )
})
.catch(
	console.log(ErrorMessage)
)
```

# async & await

### async & await란?

- 기존의 비동기 처리 방식인 콜백함수의 단점을 보완하기 위해 `Promise`를 사용했지만, 코드가 장황하다는 단점이 있었다.
- 이러한 단점을 해결하기 위해 ES8에서 도입된 비동기 처리 방식의 가장 최신 문법이다.
- `async & await`는 `Promise`객체를 반환한다.
    - 따라서 `async&await`문에서도 `then()` 을 사용할 수 있다.

### async & await 기본 문법

```jsx
async function 함수명(){
	await 비동기_처리_메소드_이름()
}
```

- 함수의 앞에 `async`라는 예약어를 붙인다.
- 함수의 내부 로직중 HTTP통신을 하는 비동기 처리 코드 앞에 `await`을 붙인다.
    - 비동기 처리 메서드가 꼭 프로미스 객체를 반환해야 `await`가 의도한 대로 동작한다.
    - 일반적으로 `await`의 대상이 되는 비동기 처리 코드는 `Axios`등 프로미스를 반환하는 API호출 함수이다.

# Promise와 async & await은 어떻게 다를까

## ⚡️ async & await은 간결하다

- 앞에서 풀었던 퀴즈에서 undefined가 출력되었던 문제를 `Promise`를 이용해서 해결해보자.

```jsx
function printAnimals() {
  getAnimals().then((data) => {
    console.log(data);
  })
}
```

- 그럼 `async await`로 어떻게 표현할까?
- 함수에 `async` 키워드를 적고, 비동기 대상에 `await`를 추가해주면 된다.
- 비동기 대상 함수에 `await`를 추가하면, _**'이 함수의 처리를 기다려!'**_ 라는 의미가 되기에
- `await` 함수 아래에 해당 함수의 응답값을 사용하는 구문을 추가해주면 된다.

```jsx
async function printAnimals() {
  const animals = await getAnimals();// 이 함수처리 끝날때까지 기다려!
console.log(animals)// 응답값이 출력됨!
}
```

- **🙋🏻‍♀️ 근데, 간결함 면에서는 둘 다 비슷한데요?**
    
    - 짧은 예시로만 봤을 때 큰 차이를 못 느낄 것이다.
    - 그렇다면 데이터를 요청하여 처리까지 한다면? `promise`의 `then`을 사용하면 아래와 같다.
    
    ```jsx
    function printAnimals() {
      return getAnimals()
        .then(data => {
          if (data.property) {
            return sampleFunc1(data)
              .then(anotherData => {
                console.log(anotherData)
              })
          }else {
            console.log(data)
          }
        })
    }
    ```
    
    - `then`방식을 보면 라인 수가 많은 것은 물론 들여쓰기도 많아 복잡한 느낌이 든다.
    - 그럼 `async await`를 쓰면 어떨까?
    
    ```jsx
    async function printAnimals() {
      const animals = await getAnimals();
      if (animals.property) {
        const sampleData = await sampleFunc1(animals);
        console.log(sampleData);
      }else {
        console.log(animals);
      }
    }
    ```
    
    - `then`에 비해 많은 들여쓰기는 물론 라인도 차지 않는다.
    - 또한 응답값을 명시적인 변수에 담아 사용하므로 직관적으로 변수를 인식할 수 있다.
    - 이처럼 `async await`는 `then` 방식에 비해 간결하다는 장점을 가지고 있다.

## ⚡️ async & await은 에러 핸들링에 유리하다

- 서버에 데이터를 요청하는 작업을 하다보면, 에러가 발생할 수 있다.
- 이 때문에 우리는 에러 핸들링도 해주어야 한다.
- 이번에는 `printAnimals()`에서 에러가 발생한 게 아니라, `JSON.parse`에서 에러가 발생했다고 가정하자.
- 이 경우, `then`을 사용하면 내부에 추가적인 `catch`문을 적어줘야한다.

```jsx
function printAnimals() {
  try {
      getAnimals()
      .then((response) => {
	      const data = JSON.parse(response);// 여기서 에러 발생한다고 가정
				console.log(data);
    })
    .catch((err)=> {// 추가적인 에러
				console.log(err)
    })
  }
  catch(err) {
    console.log(err)
  }
}
```

- 이 방식은 직관적이지 않을 뿐더러 에러를 처리하는 catch문이 중복된다.
- 하지만 `async await`를 사용하게 되면 하나의 `catch`만 해주면된다!
- 해당 `catch`문에서는 `try` 내부에서 발생하는 모든 에러를 접근할 수 있다.

```jsx
async function printAnimals() {
  try {
      const data = await JSON.parse((getAnimals())
    console.log(data);
  }
  catch(err) {
    console.log(err)
  }
}
```

## ⚡️ async는 에러 위치를 찾기가 쉽다

- 만약 프로미스를 연속으로 호출한다고 해보자.
- 이때 만약 어느 지점에서 에러가 발생하면 어떤 `then`에서 에러가 발생했는지 찾기가 어렵다.

```jsx
function sample() {
  return sampleFunc()
    .then(data => return data)
    .then(data2 => return data2)
    .then(data3 => return data3)
    .catch(err => console.log(err))// 결과적으로 문제가 발생했다
}
```

- 하지만 `async`를 사용하게 되면, 어떤 지점에서 에러가 발생했는지 쉽게 찾을 수 있다.

```jsx
async function sample() {
  const data1 = await sampleFunc();// 문제 발생시 data1값이 유효치 않음const data2 = await sampleFunc2(data1);
  return data2;
}
```

## 읽어보면 좋은 글

[https://springfall.cc/article/2022-11/easy-promise-async-await](https://springfall.cc/article/2022-11/easy-promise-async-await)