# 22장 this

### 1 this 키워드

동작을 나타내는 메소드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 한다. 이때 속한 객체의 프로퍼티를 참조하려면 **자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.**

```javascript
// [예제 22-01]

const circle = {
  // 프로퍼티 : 객체 고유의 상태 데이터
  radius: 5,
  // 메소드 : 상태 데이터를 참조하고 조작하는 동작
  getDiameter() {
    // 이 메소드가 자신이 속한 객체의 프로퍼티나 다른 메소드를 참조하려면
    // 자신이 속한 객체인 circle을 참조할 수 있어야 한다.
    return 2 * this.radius;
  }
};

console.log(circle.getDiameter()); // 10
```

하지만 이처럼 자신이 속한 객체를 재귀적으로 참조하는 방식은 일방적이지 않으며 바람직하지 않다. 생성자 함수 방식으로 인스턴스를 하는 경우를 생각해보자.

```javascript
// [예제 22-02]

function Circle(radius) {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
}

Circle.prototype.getDiameter = function () {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  return 2 * this.radius;
};

// 생성자 함수로 인스턴스를 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

생성자 함수를 정의하는 시점에는 아직 인스턴스를 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다. 따라서 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 특수한 식별자
this가 필요하다.

**this는 자신이 속한 객체 또는 자신이 생설한 인스턴스를 가리키는 자기 참조 변수(self-referencing variable)이다. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나
메소드를 알 수 있다.**

함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달된다. **단, this를 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.**

### 바인딩이란

바인딩이란 식별자와 값을 연결하는 과정을을 의미한다. 예를 들어. 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것이다. this 바인딩은 this(키워드로 분류되지만 식별자 역할을 한다)
와 this가 가리킬 객체를 바인딩 하는 것이다.

```javascript
// [예제 22-04]

// 생성자 함수
function Circle(radius){
	// this는 생성자 함수가 생성할 인스턴스를  가리킨다.
	this.radius = radius;
}

Circle.prototype.getDiameter = function () {
	// this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

자바나 C++과 달리 **자바스크립트의 this는 함수가 호출되는 방식에 따라 this에 바인딩될 값, 즉 this 바인딩이 동적으로 결정된다.**

```javascript
// [예제 22-05]

// this는 어디서든 참조 가능하다
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
	// 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
	console.log(this); // window
	return number * number;
}
square(2);

const person = {
	name : 'Lee',
	getName() {
		// 메소드 내부에서 this는 메소드를 호출한 객체를 가리킨다.
		console.log(this); // {name: "Lee", getName: f}
		return this.name;
	}
};
console.log(person.getName()); // Lee

function Person(name) {
	this.name = name;
	// 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	console.log(this); // Person {name: "Lee"}
}

const me = new Person('Lee');
```

### 4 Function.prototype.apply/call/bind 메소드에 의한 간접 호출

apply, call 메소드는 본질적인 기능은 함수를 호출 하는 것이다. 첫 번째 인수로 전달한 특정 객체를 호출한 숨의 this에 바인딩 한다.

```javascript
// [예제 22-19]

function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 
// this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // { a: 1 }
console.log(getThisBinding.call(thisArg)); // { a: 1 }
```

call: 모든 함수에서 사용할 수 있으며, this를 특정값으로 지정할 수 있다.

```javascript
const mike = {
  name: "Mike",
};

const tom = {
  name: "Tom",
};

function showThisName() {
  console.log(this.name);
}

showThisName(); // 해당 this = window
showThisName.call(mike); // Mike
/*
함수를 사용하면서 call을 사용하고 this로 접근을 한다면
해당 함수가 주어진 객체의 메소드인것처럼 사용이 가능하다.
call의 첫번째 매개변수는 this로 사용할 값이고, 더있으면 매개변수를 호출하는 함수로 
사용된다. */
showThisName.call(tom); // Tom
```

apply: 함수 매개변수를 처리하는 방법을 제외하면 call과 같다.

call은 일반적인 함수와 마찬가지로 매개변수를 직접 받지만, apply는 매개변수를 배열로 받는다.

```javascript
update.call(mike, 1999, "singer");
console.log(mike); // { name: 'Mike', birthYear: 1999, occupation: 'singer' }

update.call(tom, 2002, "teacher");
console.log(tom); // { name: 'Tom', birthYear: 2002, occupation: 'teacher' }

// =>

update.apply(mike, [1999, "singer"]);
console.log(mike); // { name: 'Mike', birthYear: 1999, occupation: 'singer' }

update.apply(tom, [2002, "teacher"]);
console.log(tom); // { name: 'Tom', birthYear: 2002, occupation: 'teacher' }
```

bind: 함수 this.값을 영구히 바꿀 수 있다.

```javascript
// [bind 예제]

const user = {
  name: "Mike",
  showName: function () {
    console.log(`hello, ${this.name}`);
  },
};

user.showName(); // hello Mike
let fn = user.showName;

fn.call(user); // hello, Mike
fn.apply(user); // hello, Mike

let boundFn = fn.bind(user);

boundFn(); // hello, Mike
```

```JavaScript
// [예제 22-24]

const person = {
  name: "Lee",
  foo(callback) {
    setTimeout(callback.bind(this), 100);
  },
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}`);
});
```

### 함수 호출 방식

| 함수 호출 방식                                         | this 바인딩                                               |
|--------------------------------------------------|--------------------------------------------------------|
| 일반 함수호출                                          | 전역 객체                                                  |
| 메소드 호출                                           | 메소드를 호출한 객체                                            |
| 생성자 호출                                           | 생성자 함수가 (미래에) 생성할 인스턴스                                 |
| Function.prototype.apply/call/bind 메소드에 의한 간접 호출 | Function.prototype.apply/call/bind 메소드에 첫번째 인수로 전달한 객체 |