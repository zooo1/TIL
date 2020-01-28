# Prototype



자바스크립트는 프로토타입 기반 객체지향 프로그래밍 언어이다.

클래스 기반 객체지향 프로그래밍 언어는 객체 생성 이전에 클래스를 정의하고 이를 통해 객체(인스턴스)를 생성한다. 

하지만 프로토타입 기반 객체지향 프로그래밍 언어는 클래스 없이(Class-less)도 객체를 생성할 수 있다.ㄴ



## 객체 생성 방법

자바스크립트는 프로토타입 기반 객체 지향 언어로서 클래스라는 개념이 없고 별도의 객체 생성 방법이 존재한다. (최근에 클래스 생김)

> ECMAScript6 에서 새롭게 클래스가 도입되었다. 프로토타입 기반 프로그래밍은 클래스가 존재하지 않는 객체지향 프로그래밍 스타일이다. 클래스없이 프로토타입 체인과 클로저 등으로 객체지향 언어의 상속, 캡슐화 등의 개념을 구현한다. 

자바스크립트의 모든 객체는 자신의 부모 역할을 담당하는 객체와 연결되어 있다. 이것은 마치 객체 지향의 상속 개념과 같이 부모 객체의 프로퍼티 또는 메소드를 상속받아 사용할 수 있게 한다. 이러한 부모 객체를 **프로토타입객체**, **프로토타입** 이라고 한다. 

프로토타입 객체는 생성자 함수에 의해 생성된 각각의 객체에 공유 프로퍼티를 제공하기 위해 사용한다.



```javascript
var student = {
  name: 'Lee',
  score: 90
}

console.log(student.hasOwnProperty('name'));
console.dir(student);

// Object .. 
// __proto__ : Object
// -> student 객체의 프로토타입
```



자바스크립트의 모든 객체는 [[Prototype]] 이라는 인터널슬롯(internal slot) 을 가진다. 이 값은 null 또는 객체이며 상속을 구현하는데 사용된다. [[Prototype]] 객체의 데이터 프로퍼티는 get 엑세스를 위해 상속되어 자식 객체의 프로퍼티처럼 사용할 수 있다. 하지만 set 엑세스는 허용되지 않는다. 



[[Prototype]] 의 값은 Prototype 객체이며 `__proto__` accessor property 로 접근할 수 있다. 여기에 접근하면 내부적으로 `Object.getPrototypeOf ` 가 호출되어 프로토타입 객체를 반환한다. 



* 객체를 생성할 때 프로토타입이 결정된다. 결정된 프로토타입 객체는 다른 임의의 객체로 변경할 수 있다. 

* 이것은 부모 객체인 프로토타입을 동적으로 변경할 수 있다는 것을 의미한다.

* 이러한 특징을 활용하여 객체의 상속을 구현할 수 있다.



## [[Prototype]] vs prototype 프로퍼티

모든 객체는 자신의 프로토타입 객체를 가리키는 [[Prototype]] 인터널 슬롯(internal slot) 을 갖으며 상속을 위해 사용된다.

함수 또한 객체이므로 [[Prototype]] 인터널 슬롯을 가진다. 그런데 함수 객체는 일반 객체와는 다르게 prototype property 도 소유하게 된다.

> prototype property 와 [[Prototype]] 인터널 슬롯은 다른 것이다. 
>
> 모두 프로토타입 객체를 가리키지만 관점의 차이가 있다.



### [[prototype]]

* 함수를 포함한 모든 객체가 가지고 있는 인터널슬롯이다.
* 객체의 입장에서 자신의 부모 역할을 하는 프로토타입 객체를 가리키며 함수 객체의 경우 `Function.prototype ` 를 가리킨다. 



### Prototype property

* 함수 객체만 가지고 있는 프로퍼티이다.
* 함수 객체가 생성자로 사용될 때 이 함수를 통해 생성될 객체의 부모 역할을 하는 객체(프로토타입 객체)를 가리킨다. 



## constructor property

constructor property는 객체의 입장에서 자신을 생성한 객체를 가리킨다.

```javascript
function Person(name){
  this.name = name;
}

var foo = new Person('Lee');

console.log(Person.prototype.constructor === Person);	// true 
console.log(foo.constructor === Person);	//	true
console.log(Person.constructor === Function);	//	true
```



## prototype chain

자바스크립트는 특정 객체의 프로퍼티나 메소드에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티 또는 메소드가 없다면 [[Prototype]]이 가리키는 링크를 따라 자신의 부모 역할을 하는 프로토타입 객체의 프로퍼티나 메소드를 차례대로  검색한다. 

```javascript
var student = {
  name: 'Lee',
  score: 90
}

console.log(student.hasOwnProperty('name'));	// true
// student 객체에는 hasOwnProperty 라는 메소드를 갖고 있지 않지만
// student 객체의 [[Prototype]] 이 가리키는 링크를 따라가서 student 객체의 부모 역할을 하는 프로토타입 객체(Object.prototype)의 메소드 hasOwnProperty 를 호출하였기 때문에 가능했다. ㄴ
```



## 객체 리터럴 방식으로 생성된 객체의 프로토타입 체인

객체 생성 방법 3가지

1. 객체 리터럴
2. 생성자 함수
3. Object() 생성자 함수



* 객체 리터럴 방식으로 생성된 객체는 내장 함수인 Object() 생성자 함수로 객체를 생성하는 것을 단순화한 것이다. 

  Javascript 엔진은 객체 리터럴로 객체를 생성하는 코드를 만나면 내부적으로 Object() 생성자 함수를 사용하여 객체를 생성한다. 객체 리터럴을 사용하여 객체를 생성한 경우, 그 객체의 프로토타입 객체는 Object.prototype 이다.

* Object() 생성자 함수는 물론 함수이기 때문에 prototype property 가 있다. 

  

```javascript
var person = {
  name: 'Lee',
  gender: 'male',
  sayHello: function(){
    console.log('Hi! My name is '+ this.name);
  }
};

console.dir(person);

//	true
console.log(person.__proto__ === Object.prototype);
console.log(Object.prototype.constructor === Object);
console.log(Object.__proto__ === Function.prototype);
console.log(Function.prototype.__proto__ === Object.prototype);
```



## 생성자 함수로 생성된 객체의 프로토타입

생성자 함수로 객체를 생성하기 위해서는 우선 생성자 함수를 정의하여야 한다.

함수 정의 방식 3가지

1. 함수 선언식(Function Declaration) 
2. 함수 표현식(Function Expression)
3. Function() 생성자 함수



```javascript
//	함수 표현식 => 함수 리터럴 방식
var square = function(number){
  return number * number;
}

//	함수 선언식 => 자바스크립트 엔진이 내부적으로 기명 함수표현식으로 변환
var square = function square(number){
  return number * number;
}

```

결국 함수 선언식, 표현식 모두 함수 리터럴 방식을 사용한다. 함수 리터럴 방식은 function 생성자 함수로 함수를 생성하는 것을 단순화 시킨 것이다. 



```javascript
function Person(name, gender){
  this.name = name;
  this.gender = gender;
  this.sayHello = function(){
    console.log('Hi, my name is ' + this.name);
  };
}

var foo = new Person('Lee', 'male');

console.dir(Person);
console.dir(foo);

console.log(foo.__proto__ === Person.prototype);
console.log(Person.prototype.__proto__ === Object.prototype);
console.log(Person.prototype.constructor === Person);
console.log(Person.__proto__ === Function.prototype);
console.log(Function.prototype.__proto__ === Object.prototype);
```

foo 객체의 프로토타입 객체 Person.prototype 객체와 Person() 생성자 함수의 프로토타입 객체인 Function.prototype 의 프로토타입 객체는 Object.prototype 객체이다.

이는 객체 리터럴 방식이나 생성자 함수 방식이나 결국은 모든 객체의 부모 객체인 Object.prototype 객체에서 프로토타입 체인이 끝난다. 이 때, Object.prototype 객체를 **프로토타입 체인의 종점(End of prototype chain)** 이라고 한다.



## 프로토타입 객체의 확장

프로토타입 객체도 객체이므로 일반 객체와 같이 프로퍼티를 추가/삭제할 수 있다. 이렇게 추가/삭제된 프로퍼티는 즉시 프로토타입 체인에 반영된다. 

```javascript
function Person(name){
  this.name = name;
}

var foo = new Person('Lee');

Person.prototype.sayHello = function(){
  console.log('Hi! my name is' + this.name);
};

foo.sayHello();
```



## 원시 타입(primitive data type)의 확장 

자바스크립트에서 원시 타입(숫자, 문자열,  boolean, null, undefined) 을 제외한 모든 것은 객체이다. 

원시 타입인 문자열이 객체와 유사하게 동작한다. 

```javascript
var str = 'test';
console.log(typeof(str));
console.log(str.constructor === String)
console.dir(str);

var strObj = new String('test');
console.log(typeof(strObj));
console.log(strObj.constructor === String);
console.log(strObj);

console.log(str.toUpperCase());
console.log(strObj.toUpperCase());
```

원시타입 문자열과 String() 생성자 함수로 생성한 문자열 객체의 타입은 분명히 다르다. 

원시타입은 객체가 아니므로 프로퍼티나 메소드를 가질 수 없다. 하지만 원시타입으로 프로퍼티나 메소드를 호출할 때 원시 타입과 연관된 객체로 일시적으로 변환되어 프로토타입 객체를 공유하게 된다. 

원시타입은 객체가 아니므로 프로퍼티, 메소드를 직접 추가할 수 없지만 String 객체의 프로토타입 객체 String.prototype 에 메소드를 추가하면 원시 타입, 객체 모두 메소드를 사용할 수 있다. 

```javascript
var str = 'test';

// 에러가 발생하지 않는다.
str.myMethod = function () {
  console.log('str.myMethod');
};

str.myMethod(); // Uncaught TypeError: str.myMethod is not a function


String.prototype.myMethod = function () {
  return 'myMethod';
};

console.log(str.myMethod());      // myMethod
console.log('string'.myMethod()); // myMethod
console.dir(String.prototype);
```





## 프로토타입 객체의 변경

객체를 생성할 때 프로토타입은 결정된다. 결정된 프로토타입 객체는 다른 임의의 객체로 변경할 수 있다. 이것은 부모 객체인 프로토타입을 동적으로 변경할 수 있다는 것을 의미한다. 이러한 특징을 활용하여 객체의 상속을 구현할 수 있다. 

이 때, 주의할 것은 프로토타입 객체를 변경하면

* 프로토타입 객체 변경 시점 이전에 생성된 객체

  기존 프로토타입 객체를 [[prototype]] 에 바인딩한다. 

* 프로토타입 객체 변경 시점 이후에 생성된 객체

  변경된 프로토타입 객체를 [[Prototype]] 에 바인딩한다. 



## 프로토타입 체인 동작 조건

객체의 프로퍼티를 참조하는 경우, 해당 객체에 프로퍼티가 없는 경우 프로토타입 체인이 동작한다. 

객체의 프로퍼티에 값을 할당하는 경우, 프로토타입 체인이 동작하지 않는다. 이는 객체에 해당 프로퍼티가 있는 경우, 값을 재할당하고 해당 프로퍼티가 없는 경우는 해당 객체에 프로퍼티를 동적으로 추가하기 때문이다. 