# this의 의미를 설명해주세요.

## 1. this 정의

> 대부분의 경우 this의 값은 **함수를 호출한 방법**이 결정합니다. 실행하는 중 할당으로 설정할 수 없고 함수를 호출할 때 마다 다를 수 있습니다. - Mdn

- 이 말은, 일반적으로 JS에서는 this가 하나로 정해진 것이 아닌, `어디서 호출하느냐에 따라 의미가 변한다`는 것을 의미한다.

## 2. 호출 시점에 따른 this의 의미

### 1. 전역 범위

<aside>
💡 전역 범위(Global context) = window 객체

</aside>

> window 객체란?

- 현재 실행되고 있는 Javascript의 모든 변수, 함수, 객체, DOM을 포함하는 객체이다.

```jsx
// 웹 브라우저에서는 window 객체가 전역 객체
console.log(this === window); // true

a = 37;
console.log(window.a); // 37

this.b = "MDN";
console.log(window.b); // "MDN"
console.log(b); // "MDN"
```

### 2. 함수 범위

<aside>
💡 this를 함수 내에서 호출한다면, 현재 실행되고 있는 코드의 문맥에 따라 this가 달라진다.

</aside>

1. **단순 함수 호출**

   <aside>
   💡 단순 함수 호출에서는 window 객체를 가리킨다.

   </aside>

   ```jsx
   // 1. 일반 함수 범위에서 호출
   function outside() {
       **console.log(this); // this는 window**

       // 2. 함수 안에 함수가 선언된 내부 함수 호출
       function inside() {
           **console.log(this); // this는 window**
       }
       inside();
   }
   outside();
   ```

   - 함수 안에 함수를 선언하더라도 this는 여전히 window를 가리킨다.

2. **객체의 메서드**

   <aside>
   💡 객체 또는 클래스 안에서 메서드를 실행하면 this는 객체 자기 자신을 가리킨다.

   </aside>

   ```jsx
   // 1. 객체 또는 클래스 안에서 메소드를 실행한다면 this는 Object 자기 자신
   var man = {
       name: 'john',
       hello: function() {
           **// 2. 객체이므로 this는 man을 가리킴
           console.log('hello ' + this.name);**
       }
   }

   man.hello(); // 3. hello john
   ```

3. **call(), apply(), bind()**

   js에서는 각기 다른 문맥의 this를 필요에 따라 변경할 수 있도록 하는 함수를 제공한다.

   ```jsx
   var man = {
       name: 'john',
       hello: function() {
           function getName() {
   						console.log(this); // man 객체가 출력됨
               return this.name;
           }
           // 이번에는 call()을 통해 현재 문맥에서의 this(man 객체)를 바인딩해주었다
           **console.log('hello ' + getName.call(this));**
       }
   }

   // 이번에는 메소드를 실행시키면 john가 뜬다!
   // this가 man 객체로 바인딩 된 것을 확인할 수 있다
   man.hello();
   ```

   - call 함수를 통해 호출 시점에서의 this를 바인딩할 수 있다.

4. **콜백 함수**

   <aside>
   💡 콜백함수 안에서는 객체 안에 메서드로 선언되어 있어도 this는 Window 객체를 가리킨다.

   </aside>

   ```jsx
   var object = {
     callback: function () {
       setTimeout(function () {
         console.log(this); // 2. this는 window
       }, 1000);
     },
   };
   ```

5. **생성자**

   <aside>
   💡 생성자 함수나 클래스 안에서의 this는 해당 생성자를 호출한 객체에 바인딩된다.

   </aside>

   ```jsx
   // 1. Class Man 선언
   class Man {
     constructor(name) {
       this.name = name;
     }
     hello() {
       console.log("hello " + this.name);
     }
   }

   // 2. 생성자 실행
   var john = new Man("John");
   john.hello(); // 3. hello John 출력
   ```

6. **화살표 함수**

   <aside>
   💡 화살표 함수의 경우 this가 아예 없기 때문에 그 상위 환경에서의 this를 참조하게 된다.

   </aside>

   ```jsx
   const cat = {
     name: "meow",
     a: function () {
       const b = () => {
         console.log(this.name); // 화살표 함수는 this가 없어서 a 함수의 this에 접근한다.
       };

       b();
     },
   };

   cat.a(); // meow
   ```

   - 화살표 함수는 자신만의 this를 생성하지 않고, 외부 스코프의 this를 상속받는다.

---

### 정리

- 전역 환경에서 window객체를 의미한다.
- 함수 환경에서는 호출되는 환경에 따라 의미가 달라진다.

---

### 참조

https://wormwlrm.github.io/2019/03/04/You-should-know-JavaScript-this.html.html

[https://velog.io/@padoling/JavaScript-화살표-함수와-this-바인딩](https://velog.io/@padoling/JavaScript-%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98%EC%99%80-this-%EB%B0%94%EC%9D%B8%EB%94%A9)
