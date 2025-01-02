# 29장 Math

표준 빌트인 객체인 Math는 수학적인 상수와 함수를 위한 프로퍼티와 메소드를 제공한다. Math는 생성자 함수가 아니다. 따라서 Math는 정적 프로퍼티와 정적 메소드만 제공한다.

## 1 Math 프로퍼티

### 1.1 Math.PI

원주율 PI 값(3.141592653589793)을 반환한다.

## 2 Math 메소드

### 2.1 Math.abs

abs( ) 메소드는 인수로 전달된 숫자의 절대값을 반환한다. 절대값은 항상 0 이상이어야 한다.

```javascript
console.log(Math.abs(-1)); // 1
console.log(Math.abs("-1")); // 1
console.log(Math.abs("")); // 0
console.log(Math.abs([])); // 0
console.log(Math.abs(null)); // 0
console.log(Math.abs(undefined)); // NaN
console.log(Math.abs({})); // NaN
console.log(Math.abs("string")); // NaN
console.log(Math.abs()); // NaN
```

### 2.2 Math.round

round() 메소드는 인수로 전달된 숫자를 소수점 이하에서 반올림하여 정수를 반환한다.

```javascript
console.log(Math.round(1.4)); // 1
console.log(Math.round(1.6)); // 2
console.log(Math.round(-1.4)); // -1
console.log(Math.round(-1.6)); // -2
console.log(Math.round(1)); // 1
console.log(Math.round()); // NaN
```

### 2.3 Math.ceil

ceil() 메소드는 인수로 전달된 숫자를 소수점 이하에서 올림하여 정수를 반환한다.

```javascript
console.log(Math.ceil(1.4)); // 2
console.log(Math.ceil(1.6)); // 2
console.log(Math.ceil(-1.4)); // -1
console.log(Math.ceil(-1.6)); // -1
console.log(Math.ceil(1)); // 1
console.log(Math.ceil()); // NaN
```

### 2.4 Math.floor

floor() 메소드는 인수로 전달된 숫자의 소수점 이하를 내림한 정수를 반환한다.

```javascript
console.log(Math.floor(1.4)); // 1
console.log(Math.floor(1.6)); // 1
console.log(Math.floor(-1.4)); // -2
console.log(Math.floor(-1.6)); // -2
console.log(Math.floor(1)); // 1
console.log(Math.floor()); // NaN
```

### 2.5 Math.sqrt

sqrt() 메소드는 인수로 전달된 숫자의 제곱근을 반환한다.

```javascript
console.clear();
console.log(Math.sqrt(9)); // 3
console.log(Math.sqrt(-9)); // NaN
console.log(Math.sqrt(2)); // 1.4142135623730951
console.log(Math.sqrt(1)); // 1
console.log(Math.sqrt(0)); // 0
console.log(Math.sqrt()); // NaN
```

### 2.6 Math.random

Math.random() 메소드는 임의의 난수(랜덤 숫자)를 반환한다.

```javascript
Math.random(); // 0에서 1미만의 랜덤 실수 (0.82087280123802)

/*
1에서 10 범위의 랜덤 정수 획득
1) Math.random으로 0에서 1 미만의 랜덤 실수를 구한 다음, 10을 곱해 0에서 10 미만의 랜덤 실수를 구한다.
2) 0에서 10 미만의 랜덤 실수에 1을 더해 1에서 10 범위의 랜덤 실수를 구한다.
3) Math.floor로 1에서 10 범위의 랜덤 실수의 소수점 이하를떼어 버린 다음 정수를 반환한다.
*/

const random = Math.floor(Math.random() * 10 + 1);
console.log(random); // 1에서 10범위의 정수
```

### 2.7 Math.pow

Math.pow 메소드는 첫 번째 인수를 밑(base)로, 두 번째 인수를 지수(exponent)로 거듭 제곱한 결과를 반환한다.

```javascript
console.log(Math.pow(2, 8)); // 256
console.log(Math.pow(2, -1)); // 0.5
console.log(Math.pow(2)); // NaN
```

Math.pow( ) 대신 ES7에서 도입된 지수 연산자 (**)를 사용하면 가독성이 더 좋다.

```javascript
2 ** (2 ** 2); // 16
Math.pow(Math.pow(2, 2), 2); // 16
```

### 2.8 Math.max

max( ) 메소드는 전달받은 인수중에 가장 큰 수를 반환한다. 인수가 전달되지 않으면 -Infinity를 반환한다.

```javascript
Math.max(1); // 1
Math.max(1,2); // 2
Math.max(1,2,3); // 3
Math.max(); // -Infinity
```

배열을 인수로 전달받아 사용하려면 apply( ) 메소드나 스프레드 문법을 사용해야 한다,

```javascript
// 배열 요소 중에서 최대값을 취득
Math.max.apply(null, [1, 2, 3]); // 3

// ES6 스프레드 문법
Math.max(...[1, 2, 3]);
```

### 2.8 Math.min

min( ) 메소드는 전달받은 인수중에 가장 작은 수를 반환한다. 인수가 전달되지 않으면 Infinity를 반환한다.

```javascript
Math.min(1); // 1
Math.min(1,2); // 1
Math.min(1,2,3); // 1
Math.min(); // Infinity
```

배열을 인수로 전달받아 사용하려면 apply( ) 메소드나 스프레드 문법을 사용해야 한다,

```javascript
// 배열 요소 중에서 최소값을 취득
Math.min.apply(null, [1, 2, 3]); // 1

// ES6 스프레드 문법
Math.min(...[1, 2, 3]); // 1
```