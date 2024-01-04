## 리액트의 상태 관리

<aside>
💡 리액트에서 상태(state)는 데이터이다.

</aside>

리액트에서는 컴포넌트에 저장한 데이터(상태)가 변경되면 해당 컴포넌트가 리렌더링된다.

따라서 리액트를 사용하면서 상태값을 효율적으로 관리하고 상태값에 따라 화면이 불필요하게 업데이트되지 않도록 주의해야 한다.

### 상태를 관리하는 두 가지 방법

1. `Local State`
2. `Global State`

### Local State

- 특정 컴포넌트 내에서만 데이터를 관리하는 것
- 리액트 `hook` 과 `props` 를 사용하여 상태를 관리한다.
- 애플리케이션의 크기가 커질 수록 `props drilling`이 발생할 수 있다.

### Global State

- Global State(전역 상태)는 프로젝트 전체의 상태 관리를 총괄
- 전역에서 관리하기 때문에, 어떤 컴포넌트에서든 상태 값을 반영하고, 변경시키는 것이 가능
- `props` 과정이 생략되어, 작업이 적고 유지 보수가 간편해지는 장점이 있으나,
- 상태관리 오류시 나타나는 `side effect`가 커지는 단점이 존재한다.

<br/>

## Context API

리액트에서 만든 상태 관리 라이브러리

`Context`, `Provider`, `Consumer` 세 가지를 숙지해야 한다.

- **Context**
  - 전역 상태를 **저장**하는 역할
  - `Context` 내부에 Provider와 Consumer가 정의되어있고, `Consumer`는 Context를 통해서 상태에 접근이 가능하다.
- **Provider**
  - 전역 상태를 **제공**하는 역할
  - 제공된 상태에 접근하기 위해서는 `Provider` 하위에 컴포넌트가 포함되어 있어야 한다.
  - 보통 루트 컴포 넌트에서 `Provider`를 정의한다.
- **Consumer**
  - 제공받은 전역 상태를 받아서 **사용**하는 역할
  - `Context`는 Consumer 사이에 있는 첫 객체를 Context에 인자로 전달하기 때문에, 바로 JSX를 작성하면 안되고, 빈 객체를 작성하고나서 JSX를 작성해야한다.
- 자세한 사용 방법이 궁금하다면
  https://velog.io/@velopert/react-context-tutorial

<aside>
😀 장점

React에서 기본적으로 제공하기 때문에 별도 설치 필요하지 않다.

🥲 단점

컨텍스트가 둘러싼 모든 컴포넌트가 리렌더링된다. (memo 사용으로 방지 가능)

</aside>

<br/>

## Redux

`Store` 에 상태들을 저장하고, 해당 어떠한 변화가 필요할 때 `Action` 을 `Dispatch` 하여 `Reducer` 에서 이를 받아 정해놓은 흐름으로 상태를 변화시키는 방식

### **Store**

- 전역상태를 저장한다.
- 자바스크립트 객체 형태로 저장되어 있으며, 오직 Reducer를 통해서만 접근할 수 있다.
- `Store`는 1개만 존재할 수 있다.

### **Action**

- Reducer에게 보내는 Store에 대한 행동을 정의한다.
- Action을 Reducer에게 전달하기 위해서는 `dispatch` 메소드를 사용해야한다.
- dispatch는 Store의 내장 함수 중 하나로 액션 객체를 넘겨줘서 상태를 업데이트한다.

### **Reducer**

- 이전 상태와 액션을 받아, 다음 상태를 반환하는 역할
- Reducer를 통해서만 전역 `상태를 변경하고 업데이트`할 수 있다.
- 이전 상태를 변경한다는 점이 아니라 새로운 상태 객체를 생성해서 반환해야 한다.

### **dispatch**

- `action`을 발생시키는 것
- 상태를 업데이트하는 유일한 방법은 `store.dispatch()` 메서드를 부르고 `action` 객체를 넘겨주는 것이다.

<aside>
😀 장점

- 외부에서 관리 Context API처럼 리랜더링 x

🥲 단점

- 리덕스 스토어 환경 설정의 복잡성
- 리덕스를 제대로 사용하기 위해 붙는 여러개의 미들웨어
- 보일러플레이트, 즉 어떤 일을 하기 위해 꼭 작성해야 하는 (상용구)코드가 많이 요구된다.
- 설계 철학에 요구되는 코딩 방식을 꼭 추구해야해서 오히려 코드가 복잡해질수 있다.
</aside>

<br/>

## Recoil

- Facebook에서 개발한 React 상태 관리 라이브러리
- Redux와 유사한 전역 상태 관리 방법을 제공하면서도, Context API와는 달리 상태 변경 시 컴포넌트를 리렌더링하지 않고 필요한 부분만 업데이트한다.
- 기존의 `useState` 와 유사하게 사용되기 때문에 러닝커브가 낮음

<aside>
😀 장점

- Redux 대비 `store` 구성을 위한 코드가 간편하다.
- 이미 hook을 사용해본 경험이 있는 개발자라면 익숙하게 사용할 수 있다.
- 공식적으로 typescript를 지원한다.

🥲 단점

- 다른 상태관리 라이브러리보다 등장 시기가 늦어 실제 반영하여 사용하는 회사들이 많지 않다.
- 다른 라이브러리와의 호환성 문제가 나타날 수 있다는 리스크를 가지고 있다.
</aside>

<br/>

## Zustand

zustand는 `Flux 패턴`을 사용한다.

!https://velog.velcdn.com/images/jyooj08/post/fcb2a8c5-2d8f-4535-bf25-af39246c42d6/image.png

Flux 패턴이란 사용자 입력을 기반으로 `Action`을 만들고 그것을 `Dispatcher`에 전달하여 `Store(Model)`의 데이터를 변경한 뒤 `View`에 반영하는 단방향의 흐름으로 애플리케이션을 만드는 아키텍처이다.

각 요소들은 단방향 흐름에 따라 순서대로 역할을 수행하고, `View`로부터 새로운 데이터 변경이 생기면 처음부터 다시 이 순서대로 실행한다. 이렇게 함으로써 예외 없이 데이터를 처리할 수 있게 된다.

### **1. Store 선언**

```jsx
import create from "zustand";

// set method로 상태 변경 가능
const useStore = create((set) => ({
  count: 0,
  increaseCount: () => set((state) => ({ count: state.count + 1 })),
  setThree: (input) => set({ count: input }),
}));
```

### **2. Store 사용**

```jsx
function App() {
  const { count, increaseCount, setThree } = useStore();
  return (
    <div className="App">
      <div>Zustand ! {count}</div>
      <button onClick={increaseCount}>+1</button>
      <button onClick={() => setNum(3)}>set3</button>
    </div>
  );
}
```

### **3. devtools를 사용하여 디버깅하기**

`Redux devtools`를 크롬 웹 스토어에서 설치해준 후, 아래처럼 `store`와 `devtools`를 연결해준다.

애플리케이션을 브라우저로 띄우고 개발자도구 창에서 `Redux devtools`를 확인하면 `store`의 상태를 확인할 수 있다.

```jsx
import create from "zustand";
import { devtools } from "zustand/middleware";

const store = (set) => ({
  bears: 0,
  increasePopulation: () => set((state) => ({ bears: state.bears + 1 })),
  removeAllBears: () => set({ bears: 0 }),
});

const useStore = create(devtools(store));

export default useStore;
```

- 심화: 클로저 사용
  ### 클로저
  zustand는 스토어를 생성하는 함수를 호출할 때 클로저를 활용한다. 클로저는 간단히 말해 '함수가 선언될 시 그 주변 환경을 기억하는 것'으로, 스토어의 상태는 스토어를 조회하거나 변경하는 함수 바깥 스코프에 항상 유지되도록 만들어져있다. 그러면 상태의 변경, 조회, 구독 등의 인터페이스를 통해서만 스토어를 다루고 실제 상태는 애플리케이션의 생명 주기 처음부터 끝까지 의도치 않게 변경되는 것을 막을 수 있다.

## 참고

- [https://velog.io/@beberiche/React의-상태관리-방법에-대해-설명해주세요](https://velog.io/@beberiche/React%EC%9D%98-%EC%83%81%ED%83%9C%EA%B4%80%EB%A6%AC-%EB%B0%A9%EB%B2%95%EC%97%90-%EB%8C%80%ED%95%B4-%EC%84%A4%EB%AA%85%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94)
- https://velog.io/@velopert/react-context-tutorial
