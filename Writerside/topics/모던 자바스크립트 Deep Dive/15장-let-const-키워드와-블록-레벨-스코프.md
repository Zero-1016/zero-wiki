# 15장 let, const 키워드와 블록 레벨 스코프

## 1 var 키워드로 선언한 변수의 문제점

ES5까지 var 키워드로만 변수 선언이 가능했다.

var 키워드의 특징을 적어보겠다.

### 1.1. 변수 중복 선언 허용

```javascript
// [예제 15-01]

var x = 1;
var y = 1;

// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.

var x = 100;
// 초기화문이 없는 변수 선언문은 무시된다.
var y;

console.log(x); // 100
console.log(y); // 1
```

만약 의도하지 않았다면 선언된 줄 모르고 중복 선언을 하면서 의도치 않게 값까지 변경되는 부작용이 발생한다.

### 1.2 함수 레벨 스코프

var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다. 따라서 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.

```javascript
// [예제 15-02]

var x = 1;

if (true){
	// x는 전역 변수다. 이미 선언된 전역 변수 x가 있으므로 x 변수는 중복 선언된다.
	// 이는 의도치 않게 변수 값이 변경되는 부작용을 발생시킨다.
	var x = 10;
}

console.log(x); // 10
```

함수 레벨 스코프는 전역 변수를 남발할 가능성을 높인다. 이로 인해 의도치 않게 전역 변수가 중복 선언되는 경우가 발생한다.

### 1.3 변수 호이스팅

var 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문이 선두로 끌어 올려진 것처럼 동작한다. 즉, 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다.

```javascript
// [예제 15-04]

// 이 시점에는 변수 호이스팅에 의해 이미 foo 변수가 선언되었다 (1. 선언단계)
// 변수 foo는 undefined로 초기화 된다. (2. 초기화 단계)
console.log(foo); // undefined

// 변수에 값을 할당 (3. 할당 단계)
foo = 123;

console.log(foo); // 123

// 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다.
var foo;
```

이는 에러를 발생시키지는 않지만 프로그램의 흐름상 맞지 않아 가독성이 떨어트리고 오류를 발생시킬 여지를 남긴다.

## 2 let 키워드

ES5에서의 단점을 보완하기 위해 ES6에서는 let, const 키워드를 도입하였습니다.

var 키워드와의 차이점을 중심으로 let 키워드를 살펴보겠습니다.

### 2.1 변수 중복 선언 금지

var 키워드로 이름이 동일한 변수를 중복해서 선언할시 아무런 에러가 발생하지 않는다. 이때 변수의 선언과 값이 동시에 이루어지면, 값이 재할당되어 변경되는 부작용이 발생한다.

하지만 let 키워드로 선언을 해둔다면 변수를 중복 선언 할 시 문법 에러(Syntax Error)가 발생한다.

```javascript
var foo = 123;
// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 아래 변수 선언은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.

var foo = 456;

let bar = 123;
// let이나 const 키워드로  선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
```

### 2.2 블록 레벨 스코프

var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다. 하지만 let 키워드로 선언한 변수는 모든 코드 블록을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

```javascript
// [예제 15-06]

let foo = 1; // 전역 변수

{
	let foo = 2;
	let bar = 3;
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

bar는 해당 블록레벨 스코프를 갖는 지역 변수이다. 따라서 전역에서는 bar 변수를 참조할 수 없다.

![모던 자바스크립트 15-1.png](모던 자바스크립트 15-1.png)

### 2.3 변수 호이스팅

var 키워드로 선언한 변수와 달리 let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

```javascript
// [예제 15-07]

console.log(foo); // ReferenceError: foo is not defined
let foo;
```

이전에 보았던 것과 같이 var 키워드로 선언된 변수는 “선언 단계”와 “초기화 단계”로 나뉘어 지기 때문에 선언문 이전에 접근해도 에러가 발생하지 않는다.

```javascript
// [예제 15-08]

// var 키워드로 선언한 변수는 런타임 이전에 선언 단계와 초기화 단계가 실행된다.
// 따라서 변수 선언문 이전에 변수를 참조할 수 있다.
console.log(foo); // undefined

var foo;
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```

let 키워드로 선언한 변수는 “선언 단계”와 “초기화 단계”로 분리되어 진행한다. 만약 초기화 단계가 실행되기 이전에 변수에 접근하려고 하면 참조 에러가 발생한다.

let 키워드로 선언한 변수는 스코프의 시작 지점부터 초기화 단계 시작 지점(변수 선언문)까지 변수를 참조할 수 없다. 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 **일시적 사각지대**라고 부른다.

```javascript
// [예제 15-09]

// 런타임 이전에 선언 단계가 실행된다. 아직 변수가 초기화되지 않았다.
// 초기화 이전의 일시적 사각지대에서는 변수를 참조할 수 없다.
console.log(foo); // ReferenceError: foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행된다.
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```

할당이 되지않고 선언만 된 상태에서 사용될 경우 `undefined`를 리턴. 선언도 되지 않았다면 참조 오류를 발생시킨다.

let 키워드는 호이스팅이 발생하지 않는 것일까? 그렇지 않다.



```javascript
// [예제 15-10]

let foo = 1; // 전역 변수

{
	console.log(foo); // ReferemceError : Cannot access 'foo' before initialization
	let foo = 2; // 지역 변수
}
```

let 키워드로 선언한 변수의 경우 변수 호이스팅이 발생하지 않았다면 전역 변수 foo의 값을 출력해야 한다 .하지만 호이스팅이 발생했기에 참조 에러가 발생하였다.

let과 const를 포함한 모든 선언은 호이스팅이 발생한다. 단 let. const, class는 선언되지 않은 것처럼 동작을 한다.

### 2.4 전역 객체와 let

var 키워드로 선언한 전역 변수와 전역 함수, 그리고 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역객체 window의 프로퍼티가 된다.

```javascript
// [예제 15-11]

// 이 예제는 브라우저 환경에서 실행해야 한다.

// 전역 변수
var x = 1;
// 암묵적 전역
y = 2;
// 전역 함수
function foo() {}

// var 키워드로 선언한 전역 변수는  전역 변수는 전역 객체은 window의 프로퍼티다.
console.log(window.x); // 1
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(x); // 1

// 암묵적 전역은 전역 객체 window의 프로퍼티다.
console.log(window.y); // 2
// 전역 객체 window의 프로퍼티는 전역 변수 처럼 사용할 수 있다.
console.log(x); // 1

console.log(window.y); // 2
console.log(y); // 2

// 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티다.
console.log(window.foo); // f foo() {}
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(foo); // f foo() {}
```

let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다. 즉, window.foo와 같이 접근할 수 없다.

```javascript
// [예제 15-12]

// 이 에제는 브라우저 환경에서 실행해야 한다.
let x = 1;

// let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아니다.
console.log(window.x); // undefined
console.log(x); // 1
```

## 3 const 키워드

const 키워드는 상수(constatnt)를 선언하기 위해 사용한다. 하지만 반드시 상수만을 위해 사용하지는 않는다.

### 3.1 선언과 초기화

**const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.**

```javascript
// [예제 15-13]

const foo = 1;

// 그렇지 않으면 에러가 발생한다.

// [예제 15-14]

const foo; // SyntaxError: Missing initializer in const delclaration
```

### 3.2 재할당 금지

**const로 선언된 변수는 재할당이 금지된다.**

```javascript
// [예제 15-16]

const foo = 1;
foo = 2; // TypeError: Assignment to constant Variable
```

### 3.3 상수

```javascript
// [예제 15-18]

// 세율을 의미하는 0.1은 변경할 수 없는 상수로서 사용될 값이다.
// 변수 이름을 대문자로 선언해 상수임을 명확히 나타낸다.
const TAX_RATE = 0.1;

// 세전 가격
let preTaxPrice = 100;

// 세후 가격
let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE);

console.log(afterTaxPrice); // 110
```

### 3.4 cosnt 키워드 객체

const 키워드로 선언된 변수에 원시 값을 할당한 경우 값을 변경할 수는 없다. 하지만 const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 없다.

```javascript
// [예제 15-19]

const person = {
	name : 'Lee'
};

// 객체는 변경 가능한 값이다. 따라서 재할당 없이 변경이 가능하다.
person.name = 'Kim';

console.log(person); // {name: 'Kim'}
```

**const 키워드는 재할당 키워드를 금지할 뿐 “불변”을 의미하지는 않는다.**

## 4 var vs. let vs. let

변수 선언에는 기본적으로 const를 사용하고 재할당이 필요한경우 let을 사용하는 것이 안전하다.

- ES6를 사용한다면 var 키워드를 사용하지 않는다.
- 재할당이 필요한경우 let을 사용, 이때 변수의 스코프는 최대한 좁게 만든다.
- 변경이 발생하지 않고 읽기 전용인 경우 원시 값과 객체에는 const 키워드를 사용한다.