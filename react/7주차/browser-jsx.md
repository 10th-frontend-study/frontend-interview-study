# JSX란

- JSX(JavaScript XML)는 Javascript에 XML을 추가한 확장한 문법이다.
- JSX는 리액트로 프로젝트를 개발할 때 사용되므로 공식적인 자바스크립트 문법은 아니다.
- 브라우저에서 실행하기 전에 바벨을 사용하여 일반 자바스크립트 형태의 코드로 변환된다.

**예: 실제 작성할 JSX 예시**

```jsx
function App() {
	return (
      <h1>Hello, Wonju!</h1>
    );
}
```

위와 같이 작성하면, 바벨이 다음과 같이 자바스크립트로 해석하여 준다.

```jsx
function App() {
	return React.createElement("h1", null, "Hello, Wonju!");
}
```

<aside>
💡 - JSX는 하나의 파일에 자바스크립트와 HTML을 동시에 작성하여 편리하다.
- 자바스크립트에서 HTML을 작성하듯이 하기 때문에 가독성이 높고 작성하기 쉽다.

</aside>

<br/>

## JSX 문법

### 1. 반드시 부모 요소 하나가 감싸는 형태여야 한다.

```jsx
function App() {
	return (
		<div>Hello</div>
		<div>Wonju!</div>
	);
}
```

```jsx
function App() {
	return (
		<div>
			<div>Hello</div>
			<div>Wonju!</div>
		</div>
	);
}
```

### **2. 자바스크립트 표현식**

- JSX 안에서도 자바스크립트 표현식을 사용할 수 있다.
- 자바스크립트 표현식을 작성하려면 JSX내부에서 코드를 { }로 감싸주면 된다.
- 유효한 모든 JavaScript 표현식을 넣을 수 있다.

```jsx
function App() {
	const name = 'Wonju';
	return (
		<div>
			<div>Hello</div>
			<div>{name}!</div>
		</div>
	);
}
```

### 3. if문(for문) 대신 삼항 연산자(조건부 연산자) 사용

- if 구문과 for 루프는 JavaScript 표현식이 아니기 때문에 JSX 내부 자바스크립트 표현식에서는 사용할 수 없다.
- 그렇기 때문에 조건부에 따라 다른 렌더링 시 JSX 주변 코드에서 if문을 사용하거나, {}안에서 삼항 연산자(조건부 연산자)를 사용 한다.

**방법1: 외부에서 사용하기**

```jsx
function App() {
	let desc = '';
	const loginYn = 'Y';
	if(loginYn === 'Y') {
		desc = <div>Wonju 입니다.</div>;
	} else {
		desc = <div>비회원 입니다.</div>;
	}
	return (
		<>
			{desc}
		</>
	);
}
```

**방법2: 내부에서 사용하기**

```jsx
function App() {
	const loginYn = 'Y';
	return (
		<>
			<div>
				{loginYn === 'Y' ? (
					<div>Wonju 입니다.</div>
				) : (
					<div>비회원 입니다.</div>
				)}
			</div>
		</>
	);
}
```

**방법3: AND연산자(&&) 사용**

```jsx
// 조건이 만족하지 않을 경우 아무것도 노출되지 않는다.
function App() {
	const loginYn = 'Y';
	return (
		<>
			<div>
				{loginYn === 'Y' && <div>Wonju 입니다.</div>}
			</div>
		</>
	);
}
```

**방법4: 즉시실행함수 사용**

```jsx
function App() {
	const loginYn = 'Y';
	return (
		<>
			{
			  (() => {
				if(loginYn === "Y"){
				  return (<div>GodDaeHee 입니다.</div>);
				}else{
				  return (<div>비회원 입니다.</div>);
				}
			  })()
			}
		</>
	);
}
```

<br/>

### 4. React DOM은 HTML 어트리뷰트 이름 대신 camelCase 프로퍼티 명명 규칙을 사용한다.

1. JSX 스타일링
    1. JSX에서 자바스크립트 문법을 쓰려면 {}를 써야 하기 때문에, 스타일을 적용할 때에도 객체 형태로 넣어 주어야 한다.
    2. css 속성명은 카멜 케이스로 작성해야 한다. (font-size → fontSize)
    
    ```jsx
    function App() {
    	const style = {
    		backgroundColor: 'green',
    		fontSize: '12px'
    	}
    	return (
    		<div style={style}>Hello, Wonju!</div>
    	);
    }
    ```
    
2. class 대신 className
    1. JSX에서는 class가 아닌 className을 사용한다.
    
    ```jsx
    function App() {
    	const style = {
    		backgroundColor: 'green',
    		fontSize: '12px'
    	}
    	return (
    		<div className="testClass">Hello, Wonju!</div>
    	);
    }
    ```
    

### 5. JSX내에서 주석 사용 방법

- JSX 내에서 `{/* … */}` 와 같은 형식을 사용한다.

```jsx
function App() {
	return (
		<>
			{/* 주석사용방법 */}
			<div>Hello, Wonju!</div></>
	);
}
```

- 시작 태그를 여러줄 작성할 때에는, 내부에서 `//` 의 형식을 사용할 수 있다.

```jsx
function App() {
	return (
		<>
			<div
			// 이렇게는 쓸 수 있습니다
			>Hello, Wonju!</div>
		</>
	);
}
```

<br/>

## 브라우저는 JSX 파일을 읽을 수 있을까

브라우저는 JSX파일을 직접 읽을 수는 없다.

JSX파일을 읽기 위해서는 JSX를 자바스크립트 객체로 변환해야 하며, 그 변환 작업은 바벨과 같은 컴파일러를 통해 이루어진다.

- TMI
    
    이전에는 JSX 파일 내에서 React를 import해야 했다. 컴파일러를 통해 JSX 코드가 자바스크립트 코드로 변환이 되면 React.createElement()를 호출해야 했고, 정상적인 호출을 위해 React가 스코프 내에 존재해야 했었기 때문이다.
    
    하지만 2020년에 릴리즈된 React17부터는 컴파일 시 React.createElement()로 변환하는 방식이 아닌 컴파일러가 JSX변환에 필요한 특수한 함수를 자동적으로 import해오는 방식을 도입했기 때문에 React를 import하지 않아도 되게 되었음