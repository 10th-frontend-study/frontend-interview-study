# require vs import

require과 import는 모두 외부의 파일이나 라이브러리 코드를 불러오는 문법이다.

- `require` `exports` → NodeJs에서 사용되고 있는 **CommonJS** 키워드
    - Ruby 언어와 비슷함
- `import` `export` → ES6(ES2015)에서 새롭게 도입된 키워드
    - Java와 Python과 비슷함

아래는 두 가지 문법이 각각의 방식으로 모듈을 불러오는 코드의 예시이다.

```jsx
/* CommonJS  */
const name = require('./module.js');
```

```jsx
/* ES6 */
import name from './module.js'
```

<aside>
💡 `require` 는 파일 어느 지점에서나 호출할 수 있지만 `import`는 파일의 시작 부분에 작성되어야 함

</aside>

<br/>

## 모듈 전체 내보내기/가져오기

모듈 전체를 export하는 경우 파일내 한번만 사용가능하다.

아래 exportFile.js 파일 안의 obj 객체를 내보내고 가져오는 방식을 각각 알아보자.

```jsx
// 📁 exportFile.js

const obj = {
   num: 100,
   sum: function (a, b) {
      console.log(a + b);
   },
   extract: function (a, b) {
      console.log(a - b);
   },
};
```

### CommonJS 문법(require/exports)

- `exports`

```jsx
module.exports = obj;
```

- `require`

```jsx
const obj = require('./exportFile.js');

obj.num; // 100
obj.sum(10, 20); // 30
obj.extract(10, 20); // -10
```

### ES6 문법(import/export)

- `export`

```jsx
 export default obj;
```

- `import`

```jsx
// export default로 가져온 모듈을 import 할 경우 중괄호 없이 그냥 씀
import obj from './exportFile.js';

obj.num; // 100
obj.sum(10, 20); // 30
obj.extract(10, 20); // -10
```

<br/>

## 모듈 개별 내보내기/가져오기

### CommonJS 문법(require/exports)

- `exports`

```jsx
// 멤버를 직접 일일히 export
exports.name = 'Ann'; // 변수의 export
exports.sayHello = function() { // 함수의 export
    console.log('Hello World!');
}
exports.Person = class Person { // 클래스의 export
    constructor(name){
        this.name = name;
    }
}
```

- `require`

```jsx
const { name, sayHello, Person } = require('./exportFile.js');

console.log(name); // Ann
sayHello(); // Hello World!
const person = new Person('inpa');
```

### ES6 문법(import/export)

- `export`

```jsx
// 멤버를 직접 일일히 export
export const name = 'Ann'; // 변수의 export
export function sayHello() { // 함수의 export
    console.log('Hello World!');
}
export class Person { // 클래스의 export
    constructor(name){
        this.name = name;
    }
}

// ----------------------- OR ----------------------- //

// 멤버를 따로 묶어서 export
const name = 'Ann';
function sayHello() {
    console.log('Hello World!');
}
class Person {
    constructor(name){
        this.name = name;
    }
}
export {name, sayHello, Person}; // 변수, 함수, 클래스를 하나의 객체로 구성하여 export
```

- `import`

```jsx
import { name, sayHello, Person } from './exportFile.js';

console.log(name); // Ann
sayHello(); // Hello World!
const person = new Person('inpa');

// ----------------------- OR ----------------------- //

// 개별로 export 된걸 * 로 한번에 묶고 as 로 별칭을 줘서, 마치 전체 export default 된걸 import 한 것 처럼 사용 가능
import * as obj from './exportFile.js'; 

console.log(obj.name); // Ann
obj.sayHello(); // Hello World!
const person = new obj.Person('inpa');
```