# 28장 Number

표준 빌트인 객체(standard built-in object)인 Number는 원시 타입인 숫자를 다룰 때 유용한 프로퍼티와 메소드를 제공한다.

## 1 Number 생성자 함수

표준 빌트인 객체인 Number 객체는 생성자 함수 객체다. 따라서 new 연산자와 함께 호출하여 Number 인스턴스를 생성할 수 있다.

Number 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 0을 할당한 Number 래퍼 객체를 생성한다.

```javascript
const numObj = new Number();
console.log(numObj); // [Number: 0]
```

문자를 전달할 경우 NaN을 할당한다.

```javascript
const numObj = new Number("Hello");
console.log(numObj); // [Number: NaN]
```

## 2 Number 프로퍼티

### 2.1 Number.EPSILON

Number.EPSILON는 1과 1보다 가장 큰 숫자중에서 가장 작은 숫자와의 차이와 같다.

Number.EPSILON은 약 2.2204460492503130808472633361816 x 10 **-16이다.

자바스크립트에서는 부동소수점 산술 연산은 정확한 결과를 기대하기 어렵다. 정수는 2진법으로 오차없이 저장이 가능하지만 부동소수점을 표현하기 위해 가장 널리 쓰이는 표준인 IEEE 754는 2진법으로 변환했을 때 무한
소수가 되어 오차가 발생할 수밖에 없는 구조적 한계가 있다.

```javascript
console.log(0.1 + 0.2); // 0.30000000000000004
console.log(0.1 + 0.2 === 0.3); // false
```

### 2.2 Number.MAX_VALUE

Number.MAX_VALUE는 자바스크립트에서 표현할 수 있는 가장 큰 양수 값이다. 이보다 큰 값은 Infinity이다.

### 2.3 Number.MIN_VALUE

Number.MIN_VALUE는 자바스크립트에서 표현할 수 있는 가장 작은 양수 값이다. 이보다 작은 값은 0이다.

### 2.4 Number.MAX_SAFE_INTEGER

Number.MAX_SAFE.INTEGER은 자바스크립트에서 가장 안전하게 표현할 수 있는 가장 큰 정수값이다.

```javascript
console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991
```

### 2.5 Number.MIN_SAFE_INTEGER

Number.MIN_SAFE_INTEGER은 자바스크립트에서 가장 안전하게 표현할 수 있는 가장 작은 정수값이다.

```javascript
console.log(Number.MIN_SAFE_INTEGER); // -9007199254740991
```

### 2.6 Number.POSITIVE_INFINITY

Number.POSITIVE_INFINITY 은 양의 무한대를 나타내는 숫자값 Infinity와 같다.

```javascript
console.log(Number.POSITIVE_INFINITY); // Infinity
```

### 2.7 Number.NEGATIVE_INFINITY

Number.NEGATIVE_INFINITY는 음의 무한대를 나타내는 숫자값 -Infinity와 같다.

```javascript
console.log(Number.NEGATIVE_INFINITY); // -Infinity
```

### 2.8 Number.NaN

Number.NaN은 숫자가 아님을 나타내는 숫자값이다.

## 3 Number 메소드

### 3.1 Number.isFinite

Number 타입의 인수로 전달된 값이 유한수인지 검사. 맞으면 결과를 불리언 값으로 나타낸다.

```javascript
console.log(Number.isFinite(0)); // true
console.log(Number.isFinite(Infinity)); // false
```

isFinite는 숫자를 암묵적으로 변환한다.

```javascript
console.log(Number.isFinite(null)); // false
console.log(isFinite(null)); // true
```

### 3.2 Number.isInteger

인수가 정수인지 확인한 후 불리언 값으로 반환한다. 숫자를 암묵적 타입변환을 하지 않는다.

### 3.3 Number.isNaN

인수가 NaN인지 확인한 후 불리언 값으로 반환한다.

```javascript
console.log(Number.isNaN(undefined)); // true
console.log(isNaN(undefined)); // false
```

Number.isNaN 메소드는 전달받은 인수를 암묵적으로 변환하지 않는다 따라서 숫자가 아닌 인수가 주어졌을 경우 언제나 반환값은 false이다.

### 3.4 Number.isSafeInteger

인수가 안전한 정수인지 검사하여 결과를 불리언 값으로 반환한다.

안전한 정수값은 -(2**53 -1)과 2**53 -1 사이의 정수값이다. 검사전에 암묵적으로 타입변환하지 않는다.

```javascript
console.log(Number.isSafeInteger(100000000000000000)); // false
console.log(Number.isSafeInteger(1)); // true
```

### 3.5 Number.prototype.toExponential

숫자를 지수표기법으로 변환하여 문자열을 반환한다.

### 지수표기법

지수표기법이란 매우 크거나 작은 숫자를 표기할 때 주로 사용하며 e(Exponent) 앞에 있는 숫자에 10의 n승을 곱하는 형식으로 수를 나타내는 방식이다. 인수로 소수점 이하 자릿수를 지정할 수 있다.

```javascript
console.log((77.1234).toExponential()); // 7.71234e+1
console.log((77.1234).toExponential(4)); // 7.7123e+1
console.log((77.1234).toExponential(2)); // 7.71e+1
```

숫자 리터럴에 Number 프로토타입 메소드를 사용할 경우 에러가 발생한다.

### 3.6 Number.prototype.toFixed

toFixed 메소드는 숫자를 반올림하여 문자열로 반환한다. 0 ~ 20사이의 정수 값을 반올림하는 소수점 이하 자릿수의 인수로 전달할 수 있고 Default 값은 0이다.

```javascript
console.log((77.1234).toFixed()); // 77
console.log((77.1234).toFixed(1)); // 77.1
console.log((77.1234).toFixed(3)); // 77.123
```

### 3.7 Number.prototype.toPrecision

toPrecision( ) 메소드는 인수로 전달받는 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다.

전체 자릿수를 나타내는 0 ~ 21 사이의 정수값을 인수로 전달할 수 있다. Default 값은 0이다.

```javascript
// 전체 자릿수 유효
console.log((77.1234).toPrecision()); // 77.1234
// 전체 1자릿수 유효, 나머지 반올림
console.log((77.1234).toPrecision(1)); // 8e+1
// 전체 3자릿수 유효, 나머지 반올림
console.log((77.1234).toPrecision(3)); // 77.1
```

### 3.8 Number.prototype.toString

숫자를 문자열로 변환하여 반환한다. 진법을 나타내는 2 ~ 36 사이의 정수값을 인수로 전달할 수 있다. 생략하면 기본값은 10진법이 지정된다.

```javascript
// 인수를 생략하면 10진수 문자열을 반환한다.
(10).toString(); // => 10
// 2진수 문자열을 반환한다.
(16).toString(2); // => "10000"
// 8진수 문자열을 반환한다.
(16).toString(2); // => "20"
// 16진수 문자열을 반환한다.
(16).toString(2); // => "10"
```