# 컴포넌트

컴포넌트

1. 데이터가 주어졌을 때 이에 맞추어 UI를 만들어준다.
2. 라이프사이클 API를 이용하여 변화에 따른 작업 수행한다.
3. 임의의 메서드를 이용해 특별한 기능을 수행한다. 

클래스형 컴포넌트, 함수형 컴포넌트가 있다. 



## 클래스형 컴포넌트 

* 함수형 컴포넌트와의 차이점

  * state 기능 및 라이프사이클 기능을 사용할 수 있다,
  * 임의 메서드를 정의할 수 있다. 

* 특징

  

```javascript
/*App.js*/
import React, { Component } from "react";
import "./App.css";

class App extends Component {
  render() {
    const name = "react";
    return <div className="react">{name}</div>;
  }
}

export default App;
```

| 함수형 컴포넌트                                              | 클래스형 컴포넌트               |
| ------------------------------------------------------------ | ------------------------------- |
| 선언이 쉽다.                                                 | 선언이 함수형에 비해 쉽지 않다. |
| 메모리 자원을 클래스형보다 덜 사용한다.                      |                                 |
| state, 라이프사이클 API 사용이 불가능하다. (Hooks라는 기능이 도입되면서 해결됨) |                                 |



### ES6의 클래스 문법

ES6 이전에는 prototype이라는 문법을 사용하였지만 ES6 문법부터는 class를 사용하여 prototype과 기능이 똑같은 코드를 작성할 수 있다. 

```javascript
class Dog{
  constructor(name) {
    this.name = name;
  }
  say(){
    console.log(this.name + ': 멍멍');
  }
}

const dong = new Dog('흰둥이');
dog.say(); // 흰둥이: 멍멍
```



## 컴포넌트 생성

* 순서
  1. 파일 만들기
  2. 코드 작성하기
  3. 모듈 내보내기 및 불러오기 



### ES6의 화살표 함수 

* 기존의 함수 선언 방식을 아예 대체하지는 않고  주로 **파라미터**를 전달할 때 유용하다.

```javascript
setTimeout(function(){
  console.log("hello world");
}, 1000);

setTimeout(() => {
  console.log("hello world")
}, 1000);
```

* 기존 function을 대체할 수 없는 것은 용도가 다르기 때문이다. 서로 가리키고 있는 **this ** 값이 다르다.

  일반 함수는 자신이 종속된 객체를 this로 가리키며 화살표 함수는 자신이 종속된 instance를 this로 가리킨다. 

```javascript
function BlackDog(){
  this.name = "흰둥이";
  return {
    name: '검둥이';
    bark: function(){
      console.log(this.name + ': 멍멍!');
    }
  }
}

const blackDog = new BlackDog();
blackDog.bark(); // 검둥이: 멍멍!

function WhiteDog() {
  this.name = "흰둥이";
  return {
    name: '검둥이';
    bark: () => {
      console.log(this.name + ': 멍멍!');
    }
  }
}
const whilteDog = new WhiteDog();
whiteDog.bark(); // 흰둥이: 멍멍!
```



## Props

properties를 줄인 표현으로 컴포넌트 속성을 설정할 때 사용하는 요소

해당 컴포넌트를 불러와 사용하는 부모 컴포넌트(현 상황에서는 App 컴포넌트가 부모 컴포넌트이다)에서 설정할 수 있다.

* JSX 내부에서 Props rendering

  component 함수의 파라미터로 받아와서 사용할 수 있다.

### 태그 사이의 내용을 보여주는 children

컴포넌트 태그 사이의 내용을 보여주는 props를 children이라고 한다. 



### 비구조화 할당 문법을 통해 props 내부 값 추출하기 

비구조화 할당(destructuring assignment)

props 값을 조회할 때마다 props.name, props.children 과 같이 `props`라는 키워드를 앞에 붙여주고 있는데 이러한 작업을 더 편하게 하기 위해 ES6의 비구조화 할당 문법을 사용하여 내부 값을 바로 추출하는 방법을 알아보자.

```javascript
// MyComponent.js
import React from "react";

const MyComponent = props => {
  const { name, children } = props;
  return (
    <div>
      안녕하세요. 제 이름은 {name} 입니다.
      <br />
      children 값은 {children} 입니다.
    </div>
  );
};

MyComponent.defaultProps = {
  name: "기본이름"
};

export default MyComponent;


/* 더 간단한 방법 */
const MyComponent = ({ name, children }) => {
  return (
    <div>
      안녕하세요. 제 이름은 {name} 입니다.
      <br />
      children 값은 {children} 입니다.
    </div>
  );
};
```



### propTypes를 통한 props 검증

컴포넌트의 필수 props를 지정하거나 props의 타입(type)을 지정할 때는 propTypes를 사용한다. 컴포넌트의 propTypes를 지정하는 방식은 defaultProp을 설정하는 방법과 비슷하다. 

```javascript
import React from "react";
import PropTypes from "prop-types";

const MyComponent = ({ name, children }) => {
  return (...);
};

MyComponent.defaultProps = {
  name: "기본이름"
};

MyComponent.propTypes = {
  name: PropTypes.string
};

export default MyComponent;
```

이렇게 설정해주면 name 값은 무조건 문자열(string) 형태로 전달해야 된다는 것을 의미한다. 



#### isRequired를 사용하여 필수 propTypes 설정

propTypes를 지정하지 않았을 때 경고 메세지를 띄워 주는 작업

propTypes를 지정할 때 뒤에 isRequired를 붙여주면 된다.



#### propTypes 종류

* array: 배열

* arrayOf: 특정 prototype으로 이루어진 배열 

  ex) arrayOf(PropTypes.number) : 숫자로 이루어진 배열 

* bool: true, false

* func: 함수

* number: 숫자

* object: 객체

* string: 문자열

* symbol: ES6의 symbol

* node: 렌더링 할 수 있는 모든 것(숫자, 문자열, 혹은 JSX code, children)

* instanceOf(클래스): 특정 클래스의 인스턴스

* oneOf(['dog', 'cat']): 주어진 배열 요소 중 값 하나

* oneOfType([React.PropTypes.string, PropTypes.number]): 주어진 배열 안의 종류 중 하나

* objectOf(React.PropTypes.number): 객체의 모든 키 값이 인자로 주어진 PropTypes인 객체

* shape({name: PropTypes.string, num: PropTypes.number}): 주어진 스키마를 가진 객체

* any: 아무 종류



### Class형 컴포넌트에서 props 사용하기

render 함수에서 this.props를 조회하면 된다. defaulProps와 propsTypes는 똑같은 방식으로 설정할 수 있다. 

```javascript
class MyComponent extends Component {
  render() {
    const { name, favoriteNumber, children } = this.props;
    return (
      <div>
        안녕하세요. 제 이름은 {name}입니다. <br />
        children 값은 {children} 입니다. <br />
        제가 좋아하는 숫자는 {favoriteNumber}입니다.
      </div>
    );
  }
}
```



클래스형 컴포넌트에서 defaultProps와 propsTypes를 설정할 때 class 내부에서 지정하는 방법도 있다.



## State

컴포넌트 내부에서 바뀔 수 있는 값 

props는 컴포넌트가 사용되는 과정에서 부모 컴포넌트가 설정하는 값이면 컴포넌트 자신은 해당 props를 읽기 전용으로만 사용할 수 있다. props를 바꾸려면 부모 컴포넌트에서 바꾸어 주어야 한다.

```react
import React, {Component} from 'react';

class Counter extends Component{
  state ={
    number: 0
  }

	handleIncrease = () => {
    this.setState({
      number: this.state.number + 1
    });
  }
  
  handleDecrease = () => {
    this.setState({
      number: this.state.number - 1
    });
  }
  
  render(){
    return(
    <div>
     	<h1>카운터</h1> 
        <div>값: {this.state.number}</div>
        <button onClick={this.handleIncrease}>+</button>
        <button onClick={this.handleDecrease}>-</button>
      </div>
      
    )
  }
}

export default Counter;
```

* component의 state를 정의할 때는 class fields 문법을 사용한다.
* class fields를 사용하지 않는다면 constructor에 넣어야한다. (귀찮음)



### 메소드 작성

arrow 함수를 사용한다. 그렇지 않으면 constructor에서 bind 해주는 작업을 거쳐야한다.(귀찮음)



### setState

`this.setState`: state에 있는 값을 바꾸기 위해서는 반드시 이 함수를 거쳐야한다. 이 함수가 호출되면 컴포넌트가 리랜더링 되도록 설계되어 있다.

이 함수는 객체로 전달되는 값만 업데이트를 해준다. 객체의 깊숙한 곳 까지 확인할 수 없다. 

```react
state ={
  number: 0,
  foo: {
    bar: 0,
    foobar: 1
  }
}

this.setState({
  foo:{
    foobar: 2
  }
}) 
/*
state = {
  number: 0,
  foo: {
    foobar: 2
  }
}
*/
```

foobar 값이 업데이트가 되는 것이 아닌 foo 객체가 바뀌어버린다. 

다음과 같이 해주어야 한다.

```react
this.setState({
  number: 0,
  foo: {
    ...this.state.foo,
    foobar: 2
  }
})
```

`...` : 전개연산자. 기존의 객체안에 있는 내용을 해당 위치에 풀어준다.



#### setState에 객체 대신 함수 전달하기

```react
// 1. this.state 를 조회하지 않아도 되는 방법
this.setState(
	(state) => ({
    number: state.number + 1
  })
)

// 2. 비구조화 할당
this.setState(
	({number}) => ({
    number: number+1
  })
)

```

기존에 작성했던 함수를 다른 방식으로 구현해보자!

```react
	handleIncrease = () => {
    const {number} = this.state
    this.setState({
      number: number + 1
    });
  }
  handleDecrease = () => {
    this.setState(
      ({number}) => ({
      number: number -1
    })
   );
  }
```



### 이벤트 설정

리액트서 이벤트 함수를 설정할 때 html과 다른 사항이 있다. 이 점을 꼭 주의해야 한다.

1. 이벤트 이름은 반드시 **camelCase**

   onclick, onmousedown => onClick, onMouseDown

2. 이벤트에 전달해주는 값은 **함수** 여야 한다. 호출해주지 말 것!