# 깊은복사와 얕은 복사의 차이에 대해 설명해주세요.

### 기본형 타입

- Null
- Undefined
- Boolean
- Number
- BigInt
- String
- Symbol

<br>

`원시값`은 값을 복사할 때 복사된 값을 다른 메모리에 할당하기에 `원래의 값`과 `복사된 값`이 `서로` `영향`을 `미치지 않음`

```jsx
const a = 1;
let b = a;

b = 2;

console.log(a); //1
console.log(b); //2
```

<br>

### 참조형 타입

- Object
- Array
- Function

<br>

`참조값`은 객체의 주소를 가리키는 값이기 때문에 복사된 값이 같은 값을 가리킴 따라서 `서로` `영향을 미침`

```jsx
const a = { number: 1 };
let b = a;

b.number = 2;

console.log(a); // {number: 2}
console.log(b); // {number: 2}
```

<aside>
💡 이런 객체의 특징때문에 객체를 복사하는 방법은 크게 두가지로 나뉜다
</aside>

<br>

## 깊은 복사 ( Deep Copy )

- 기존 값의 모든 참조가 끊어지는 것
- 새로운 메모리 공간을 확보하여 완전히 복사하는 것

1. **재귀함수를 이용한 복사**

   ```jsx
   const obj = {
     a: 1,
     b: {
       c: 2,
     },
   };

   function copyObj(obj) {
     const result = {};

     for (let key in obj) {
       if (typeof obj[key] === 'object') {
         result[key] = copyObj(obj[key]);
       } else {
         result[key] = obj[key];
       }
     }

     return result;
   }

   const copiedObj = copyObj(obj);

   copiedObj.b.c = 3;

   obj.b.c === copiedObj.b.c; //false
   ```

2. \***\*JSON.parse(JSON.stringify(obj))를 사용\*\***

   ```jsx
   const obj = {
     a: 1,
     b: {
       c: 2,
     },
   };

   const copiedObj = JSON.parse(JSON.stringify(obj));

   copiedObj.b.c = 3;

   obj.b.c === copiedObj.b.c; //false
   ```

3. **lodash \_.cloneDeep(obj) 라이브러리 사용**

   ```jsx
   const obj = {
     a: 1,
     b: {
       c: 2,
     },
   };

   const copiedObj = _.cloneDeep(obj);

   copiedObj.b.c = 3;

   obj.b.c === copiedObj.b.c; //false
   ```

<br>

## 얕은 복사 ( Shallow Copy )

- 객체를 복사할 때 기존 값과 복사된 값이 같은 참조를 가리키고 있는 것
- 참조 타입 데이터가 저장한 메모리 주소값을 복사하는 것을 의미

### 방법

1. **대입연산자**

   ```jsx
   const obj = { a: 1 };
   const copyObj = obj;

   copyObj.a = 2;

   console.log(obj.a); // 2
   console.log(obj === copyObj); // true
   ```

2. **Object.assign()**

   - Object.assign(dest, [src1, src2, src3 …])
   - dest는 목표로 하는 객체
   - 이어지는 인수는 복사하고자 하는 객체
   - src1, src2 … 의 프로퍼티를 dest에 복사 후 dest를 반환함

   ```jsx
   const obj = {
     a: 1,
     b: {
       c: 2,
     },
   };

   const copiedObj = Object.assign({}, obj);

   copiedObj.b.c = 3;

   obj === copiedObj; // false
   obj.b.c === copiedObj.b.c; // true (2차원 객체는 얕은 복사가 됨)
   ```

   ```jsx
   const target = { a: 1, b: 2 };
   const source = { b: 4, c: 5 };

   const returnedTarget = Object.assign(target, source);

   console.log(target);
   // Expected output: Object { a: 1, b: 4, c: 5 }

   console.log(returnedTarget === target);
   // Expected output: true
   ```

3. **스프레드 연산자**

   ```jsx
   const obj = {
     a: 1,
     b: {
       c: 2,
     },
   };

   const copiedObj = { ...obj };

   copiedObj.b.c = 3;

   obj === copiedObj; // false
   obj.b.c === copiedObj.b.c; // true
   ```

**Object.assign()과 스프레드 연산자는 1차원 객체는 깊은 복사, 2차원 객체의 경우 얕은 복사가 된다.**

<br>

<aside>
💡 얕은 복사 후 원시값과 복사된 값 모두 똑같은 참조를 가리키고 있기 때문에 복사된 값을 수정할 시 원시값에 영향을 끼치게 되므로 주의해야 함
</aside>
