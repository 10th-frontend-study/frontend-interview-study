# React rendering 성능을 향상시키기 위한 방법들을 설명해주세요.

### 리렌더링 되는 조건?

-   어떤게 있을까요
    1. 부모에서 전달받은 `props`가 변경될 때
    2. 부모 컴포넌트가 리렌더링 될 때
    3. 자신의 `state`가 변경될 때

## **1. useMemo**

<aside>
💡 **useMemo는 복잡한 연산의 결과를 재사용하는 hook**

</aside>

> 오랜 시간동안 연산이 수행되는 컴포넌트 예시

```jsx
//UserList.jsx
import { useState, useMemo useRef } from "react";
import Item from "./Item";
import Average from "./Average";

function UserList() {
  let numberRef = useRef(2);

  const [users, setUsers] = useState([
    {
      id: 0,
      name: "sewon",
      age: 30,
      score: 100
    },
    {
      id: 1,
      name: "kongil",
      age: 50,
      score: 10
    }
  ]);

	// 여기서 users의 개수가 늘어난다면 엄청난 연산시간이 소요될 것이다.
  const average = (() => {
    console.log("calculate average. It takes long time !!");
    return users.reduce((acc, cur) => {
      return acc + cur.age / users.length;
    }, 0);
  })();

// => props로 넘겨주는 데이터이기 때문에 자식 컴포넌트에도 렌더링이 발생하게 된다.
  return (
      <div>
       <Average average={average} />
      </div>
  );
}

export default UserList;
```

> useMemo 예시

-   func : 캐시하고 싶은 함수
-   input_dependency : 의존성 배열(해당 배열안의 값이 변경될 때 func 호출)

```jsx
useMemo(() => func, [input_dependency]);
```

> useMemo를 사용한 예시

```jsx
// users가 변경되지 않는 한 average 값을 캐싱해놓는다.
const average = useMemo(() => {
    console.log("calculate average. It takes long time !!");
    return users.reduce((acc, cur) => {
        return acc + cur.score / users.length;
    }, 0);
}, [users]);
```

## **2. React.memo 컴포넌트 메모이제이션**

<aside>
💡 컴포넌트가 동일한 props로 동일한 결과를 렌더링한다면, `React.memo`를 호출하고 결과를 메모이징(Memoizing)함. 즉, 마지막으로 렌더링된 결과를 재사용

</aside>

부모 컴포넌트가 렌더링 되면 모든 자식 컴포넌트 또한 렌더링 되는데 props가 변경되지 않았다면 자식 컴포넌트는 렌더링 될 필요가 없다. 이때 React.memo 함수를 사용해 불필요한 렌더링을 방지해 준다.

> user가 추가될 때마다 Items 컴포넌트가 추가되는 코드

```jsx
//UserList.jsx
import { useState, useRef } from "react";
import Item from "./Item";
import Average from "./Average";

function UserList() {
  let numberRef = useRef(2);
  const [text, setText] = useState("");
  const [users, setUsers] = useState([
    {
      id: 0,
      name: "sewon",
      age: 30,
      score: 100
    },
    {
      id: 1,
      name: "kongil",
      age: 50,
      score: 10
    }
  ]);

  const average = useMemo(() => {
    console.log("calculate average. It takes long time !!");
    return users.reduce((acc, cur) => {
      return acc + cur.score / users.length;
    }, 0);
  }, [users]);


   const addUser =() => {
    setUsers([
      ...users,
      {
        id: (numberRef.current += 1),
        name: "yeonkor",
        age: 30,
        score: 90
      }
    ]);
  }

  return (
      <div>
       <input
         type="text"
         value={text}
         placeholder="아무 내용이나 입력하세요."
         onChange={(event) => setText(event.target.value)}
        />
       <Average average={average} />
       <button className="button" onClick={addUser}>
        새 유저 생성
       </button>
      {users.map((user) => {
        return (
          **<Item key={user.id} user={user} /> // 아래 코드 참고**
        );
      })}
      </div>
  );
}

export default UserList;
```

> memo 사용 예시

memo로 감쌌기 때문에 기존에 존재하던 item 이외에 새로 추가된 것만 렌더링된다.

```jsx
//Item.jsx
import React, { memo } from "react";

function Item({ user }) {
    console.log("Item component render");

    return (
        <div className="item">
            <div>이름: {user.name}</div>
            <div>나이: {user.age}</div>
            <div>점수: {user.score}</div>
            <div>등급: {result.grade}</div>
        </div>
    );
}

export default memo(Item);
```

## 3. useCallback

<aside>
💡 함수 선언 자체를 memorize하는데 사용되는 hook

</aside>

> useMemo와의 비교

```jsx
useMemo(() => console.log(), [test]);
const memoizedCallback = useCallback(() => console.log(), [test]);
```

## 4. 자식 컴포넌트의 props로 객체를 넘겨줄 경우 변형하지 않고 넘겨주기

```jsx
// 생성자 함수
<Component prop={new Obj("x")} />
// 객체 리터럴
<Component prop={{property: "x"}} />
```

-   이러한 경우 생성된 객체가 props로 들어가기 때문에 컴포넌트가 렌더링 될 때마다 새로운 객체가 생성되어 자식 컴포넌트로 전달됨
-   이때 props가 전달된 객체가 동일한 값이어도 새로운 객체를 생성하면 이전 객체와 다른 참조 주소를 가지기 때문에 자식 컴포넌트에 React.memo를 사용해도 메모이제이션이 일어나지 않음

> 메모이제이션이 되지 않는 안좋은 예시

```jsx
// UserList.jsx
function UserList() {
{...}

 const getResult = useCallback((score) => {
    if (score <= 70) {
      return { grade: "D" };
    } else if (score <= 80) {
      return { grade: "C" };
    } else if (score <= 90) {
      return { grade: "B" };
    } else {
      return { grade: "A" };
    }
  }, []);

return(
 <div>
 {users.map((user) => {
    return (
			// 새로 생성되는 결과값이 props로 전달 => getResult 반환하는 값은 매번 새로운 참조를 가짐
      <Item key={user.id} user={user} result={getResult(user.score)} />
        );
      })}
 </div>

)
export default memo(UserList);

// Item.jsx
function Item({ user, result }) {
  console.log("Item component render");

  return (
    <div className="item">
      <div>이름: {user.name}</div>
      <div>나이: {user.age}</div>
      <div>점수: {user.score}</div>
      <div>등급: {result.grade}</div>
    </div>
  );
}

export default Item;
```

> 데이터 처리를 하위 컴포넌트에서 해주어 props는 동일한 값을 유지한다.

```jsx
// UserList.jsx
function UserList() {
{...}

return(
 <div>
 {users.map((user) => {
    return (
      <Item key={user.id} user={user} />
        );
      })}
 </div>

)
export default memo(UserList);

// Item.jsx

function Item({ user }) {
  console.log("Item component render");

  const getResult = useCallback((score) => {
    if (score <= 70) {
      return { grade: "D" };
    }
    if (score <= 80) {
      return { grade: "C" };
    }
    if (score <= 90) {
      return { grade: "B" };
    } else {
      return { grade: "A" };
    }
  }, []);

  const { grade } = getResult(user.score);

  return (
    <div className="item">
      <div>이름: {user.name}</div>
      <div>나이: {user.age}</div>
      <div>점수: {user.score}</div>
      <div>등급: {grade}</div>
    </div>
  );
}

export default memo(Item);
```

## 5. 컴포넌트를 매핑할 때는 key값으로 index를 사용하지 않는다.

-   리액트에서 매핑을 할 대 반드시 고유 Key를 부여하도록 강제하고 있는데, 이렇게 index값으로 key값을 부여하면 좋지 않다.
-   이유
    -   어떤 배열의 중간에 어떤 요소가 삽입될 때, 그 중간 이후에 위치한 요소들은 전부 index가 변경된다.
    -   이로 인해 key값이 변경되어 React는 key가 동일할 경우 동일한 DOM Element를 보여주기 때문에 예상치 못한 문제가 발생한다.
    -   또한 key와 매치가 안되어 서로 꼬이는 부작용도 발생한다.

> [https://medium.com/sjk5766/react-배열의-index를-key로-쓰면-안되는-이유-3ce48b3a18fb](https://medium.com/sjk5766/react-%EB%B0%B0%EC%97%B4%EC%9D%98-index%EB%A5%BC-key%EB%A1%9C-%EC%93%B0%EB%A9%B4-%EC%95%88%EB%90%98%EB%8A%94-%EC%9D%B4%EC%9C%A0-3ce48b3a18fb)

### 참조

[[React] 렌더링 성능 최적화하는 7가지 방법 (Hooks 기준)](https://velog.io/@shin6403/React-렌더링-성능-최적화하는-7가지-방법-Hooks-기준)
