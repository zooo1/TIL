# JSX

코드 번들링(bundling): 파일을 묶듯이 연결한다. 

## JSX란?

자바스크립트의 확장 문법(공식적인 자바스크립트 문법은 아니다.)

브라우저에서 실행되기 전에 코드가 번들링되는 과정에서 바벨을 사용하여 일반 자바스크립트 형태의 코드로 변환된다. 

## JSX 문법

### 감싸인 요소

컴포넌트에 여러 요소가 있다면 반드시 부모 요소 하나로 감싸야 한다. 

왜? Virtual DOM에서 컴포넌트 변화를 감지해 낼 때 효율적으로 비교할 수 있도록 컴포넌트 내부는 하나의 DOM 트리 구조로 이루어져야 한다는 규칙이 있기 때문

꼭 div 요소를 사용하지 않아도 된다. 

```javascript
import React, {Fragment} from 'react';

function App(){
  return (
  <Fragment>
  	<h1>리액트 안녕!</h1>
    <h2>잘 작동하니?</h2>
  </Fragment>
  )
}

export default App;
```

```javascript
import React, {Fragment} from 'react';

function App(){
  return (
  <>
  	<h1>리액트 안녕!</h1>
    <h2>잘 작동하니?</h2>
  </>
  )
}

export default App;
```

### 자바스크립트 표현 

```javascript
import React, {Fragment} from 'react';

function App(){
  const name = "리액트";
  return (
  <>
  	<h1>{name} 안녕!</h1>
    <h2>잘 작동하니?</h2>
  </>
  )
}

export default App;
```



### if 문 대신 조건부 연산자

JSX 내부의 자바스크립트 표현식에서 if 문을 사용할 수는 없다. 하지만 다른 내용을 렌더링해야 할 때는 JSX 밖에서 if 문을 사용하여 사전에 값을 설정하거나 {} 안에 조건부 연산자(삼항 연산자)를 사용하면 된다. 

```javascript
import React from 'react';

function App(){
  const name = "react";
  return (
  <div>
    {name === "react" ? (<h1>리액트입니다.</h1>): (<h2>리액트가 아닙니다.</h2>)}
    {name === "react" ? (<h1>리액트입니다.</h1>): null}
  </div>
  )
}
```



### AND 연산자(&&)를 사용한 조건부 렌더링

```javascript
import React from 'react';

function App(){
  const name = "react";
  return <div>{name === "react" && <h1>리액트입니다.</h1>}</div>
}
```



### undefined를 렌더링하지 않기

### 인라인 스타일링

DOM 요소에 스타일을 적용할 때는 문자열 형태로 넣는 것이 아니라 객체 형태로 넣어 주어야 한다. 이 때, 카멜 표기법으로 작성해야 한다. 

```javascript
import React from 'react';

function App(){
  const name = "리액트";
  const style = {
    backgroundColor : 'black',
    color : 'aqua',
    fontSize: '28px',
   	padding: 16 // 단위를 생략하면 px로 지정된다.
  }
}
```



### class 대신 className

```css
// src/App.css
.react {
  background : black;
  color : aqua;
  font-size: 28px;
}
```



```javascript
// src/App.js
import React from 'react';

function App(){
  const name = "react";
  return <div className="react">{name}</div>
}
```



### 꼭 닫아야 하는 태그

HTML 코드를 작성할 때 `<br>` `<input>` 등의 태그는 닫지 않아도 되었지만 JSX에서는 꼭 닫아주어야 한다. 



### 주석

JSX 내부에서 주석을 작성할 때는 {/* ... */ }와 같은 형식으로 작성한다. 

```javascript
import React from 'react';

function App(){
  const name = "react";
  return (
  <div>
      {/* 주석은 이렇게 작성한다. */}
      <div
        className="react" // 시작 태그를 여러 줄로 작성한다면 여기에 주석을 작성할 수 있다. 
        >
      </div>
      // 그러나 이러한 주석은 화면에 다 나타난다. 
  </div>
  )
}
```

