# 33장 Symbol

## 33.1 심벌이란?

심벌(symbol)은 ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값이다. 심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다.

10장에서 “프로퍼티”에서 살펴본 바와 같이 프로퍼티 키로 사용할 수 있는 값은 빈 문자열을 포함하는 모든 문자열 또는 심벌 값이다.

## 33.2 심벌 값의 생성

### 33.2.1 Symbol 함수

심벌 값은 Symbol 함수를 호출하여 생성한다. 다른 원시값은 리터럴 표기법을 통해 값을 생성할 수 있지만 심벌 값은 Symbol 함수를 호출하여 생성해야한다. 이 때 생성된 심벌 값은 외부로 노출되지 않아 확인할
수 없으며, **다른 값과 절대 중복되지 않는 유일무이한 값이다.**

```javascript
// Symbol 함수를 호출하여 유일무이한 심벌 값을 생성한다.
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol); // Symbol()
```

언뜻 보면 생성자 함수로 객체를 생성하는 것처럼 보이지만 다른 생성자 함수와 달리 new 연산자와 함께 호출하지 않는다. new 연산자와 함께 클래스 또는 함수를 호출하면 객체가 생성되지만 심벌 값은 변경 불가능한
원시값이다.

```javascript
new Symbol(); // TypeError: Symbol is not a constructor
```

Symbol 함수에는 선택적으로 문자열을 인수로 전달할 수 있다. 이 문자열은 생성된 심벌 값에 대한 설명으로 디버깅 용도로만 사용되며, 심벌 값 생성에 영향을 주지 않는다.

```javascript
// 심벌 값에 대한 설명이 같더라도 유일무이한 심벌 값을 생성한다.
const mySymbol1 = Symbol('mySymbol');
const mySymbol2 = Symbol('mySymbol');

console.log(mySymbol1 === mySymbol2); // false
```

심벌 값도 문자열, 숫자, 불리언과 같이 객체로 접근하면 암묵적으로 래퍼 객체를 생성한다.

```javascript
const mySymbol = Symbol("mySymbol");

// 심벌도 래퍼 객체를 생성한다.
console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)
```

심벌은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다. 단 불리언 타입으로는 암묵적 타입 변환이 된다.

```javascript
const mySymbol = Symbol("mySymbol");

// 불리언 타입으로 암묵적으로 타입 변환된다.
console.log(!!mySymbol); // true

// if 문 등에서 존재 확인이 가능하다.
if(mySymbol) console.log('mySymbol is not empty');
```

### 33.2.2 Symbol.for / Symbol.keyFor 메소드

인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.

- 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환한다.
- 검색에 실패하면 새로운 심벌 값을 생성하여 Symbol.for 메소드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환한다.

```javascript
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for("mySymbol");

// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환
const s2 = Symbol.for("mySymbol");

console.log(s1 === s2); // true
```

Symbol 함수는 호출될 때마다 유일무이한 심벌 값을 생성한다. 이때 심벌 값 저장소인 전역 심벌 레지스트리에서 심벌 값을 검색할 수 있는 키를 지정할 수 없으므로 전역 심벌 레지스트리에 등록되어 관리되지 않는다.

하지만 Symbol.for 메소드를 사용하면 애플리케이션 전역에서 중복되지 않는 유일무이한 상수 심벌 값을 레지스티리를 통해 공유가 가능하다.

```javascript
// 전역 심벌 레지스트리 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s1); // mySymbol

// Symbol 함수를 호출하여  생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol('foo');
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s2); // undefined
```

## 33.3 심벌과 상수

```javascript
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다.
// 이때 값 1, 2, 3, 4에는 특별한 의미가 없고 상수 이름에 의미가 있다.

const Direction = {
  UP: 1,
  DOWN: 2,
  LEFT: 3,
  RIGHT: 4,
};

// 변수에 상수를 할당
const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log(`YOU are going up`);
}
```

```javascript
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다.
// 중복될 가능성이 없는 심벌값으로 상수 값을 생성한다.

const Direction = {
  UP: Symbol(1),
  DOWN: Symbol(2),
  LEFT: Symbol(3),
  RIGHT: Symbol(4),
};

// 변수에 상수를 할당
const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log(`YOU are going up`);
}
```

### enum

enum은 명명된 숫자 상수의 집합으로 열거형(enumerated type)이라고 부른다. 자바스크립트는 enum을 지원하지 않지만 C, 자바, 파이썬 등 여러 프로그래밍 언어와 자바스크립트의 상위 확장인
타입스크립트에서는 enum을 지원한다.

자바스크립트에서 enum을 흉내 내어 사용하려면 다음과 같이 객체의 변경을 방지하기 위해 객체를 동결하는 Object.freeze 메소드와 심벌 값을 사용한다.

```javascript
// JavaScript enum
// Direction 객체는 불변 객체이며 프로퍼티 값은 유일무이한 값이다.
const Direction = Object.freeze({
  UP: Symbol("up"),
  DOWN: Symbol("down"),
  LEFT: Symbol("left"),
  RIGHT: Symbol("right"),
});

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log("You are going UP");
} // You are going UP
```

## 33.4 심벌과 프로퍼티 키

객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있으며, 동적으로 생성 할 수도 있다.

심벌 값으로 프로퍼티 키를 동적 생성하여 프로퍼티를 만들어 보자. 심벌 값을 프로퍼티 키로 사용하려면 프로퍼티 키로 사용할 심벌 값에 대괄호를 사용해야 한다.

```javascript
const obj = {
  // 심벌 값으로 프로퍼티 키를  생성
  [Symbol.for("mySymbol")]: 1,
};

console.log(obj[Symbol.for("mySymbol")]);
```

**심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.**

## 33.5 심벌과 프로퍼티 은닉

심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 메소드를 사용해 찾을 수 없습니다. 심벌 값을 프로퍼티 키로 사용하면 외부에 노출할 필요가 없는 프로퍼티를 은닉할 수 있다.

```javascript
const obj = {
  // 심벌 값으로 프로퍼티 키를  생성
  [Symbol.for("mySymbol")]: 1,
};

for (const key in obj) {
  console.log(key); // ...
}

console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []
```

하지만 프로퍼티를 완전하게 숨길 수 있는 것은 아니다. ES6에서 도입된 Object.getOwnPropertySymbols 메소드를 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티를 찾을 수 있다.

```javascript
const obj = {
  // 심벌 값으로 프로퍼티 키를  생성
  [Symbol.for("mySymbol")]: 1,
};

// getOwnPropertySymbols 메소드는 인수로 전달한 객체의 심벌 프로퍼티 키를 배열로 반환한다.
console.log(Object.getOwnPropertySymbols(obj)); // [ Symbol(mySymbol) ]

// getOwnPropertySymbols 메소드로 심벌 값을 찾을 수 있다.
const symbolKey1 = Object.getOwnPropertySymbols(obj)[0];
console.log(obj[symbolKey1]); // 1
```

## 33.6 심벌과 표준 빌트인 객체 확장

표준 빌트인 객체에 사용자 정의 메소드를 직접 추가하여 확장하는 것은 권장하지 않는다. 빌트인 객체는 읽기 전용으로 사용하는 것이 좋다.

```javascript
// 표준 빌트인 객체를 확장하는 것은 권장하지 않는다.
Array.prototype.sum = function () {
  return this.reduce((prev, next) => prev + next, 0);
};

console.log([1, 2].sum()); // 3
```

그 이유는 개발자가 직접 추가한 메소드와 미래에 표준 사양으로 추가될 메소드의 이름이 중복될 수 있기 때문이다. 이는 새롭게 추가된 ES버전에서 새로 생긴 메소드의 이름과 중복될경우 이전에 추가했던 사용자 정의
메소드가 덮어 쓸 수 있어 문제가 된다.

하지만 중복된 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 기존 프로퍼티 키와 충돌하지 않는 것은 물론, 버전 업데이트와 충돌할 위험 없이 객체를 확장 할 수 있다.

```javascript
// 심벌 값으로 프로퍼티 키를 동적 생성하면 다른 프로퍼티 키와 절대 충돌하지 않아 안전하다.
Array.prototype[Symbol.for("sum")] = function () {
  return this.reduce((prev, next) => prev + next, 0);
};

console.log([1, 2][Symbol.for("sum")]()); // 3
```

## 33.7 Well-known Symbol

자바스크립트에서 기본 제공하는 빌트인 심벌 값이 있다. Symbol 함수의 프로퍼티에 할당되어 있다. 브라우저 콘솔에서 Symbol 함수를 참조하여 보자.

![모던 자바스크립트 33-1.png](모던 자바스크립트 33-1.png)

자바스크립트가 기본 제공하는 빌트인 심벌 값을 ECMAScirpt 사양에서는 Well-known Symbol 이라고 부른다. 이는 자바스크립트 엔진의 내부 알고리즘에 사용된다.

예를 들어, Array, String, Map, Set, TypedArray, arguments, NodeList, HTMLCollection과 같이 for … of 문으로 순회 가능한 빌트인 이터러블은
Well-known Symbol인 Symbol.iterator를 키로 갖는 메소드를 가지며, Symbol.iterator 메소드를 호출하려면 이터레이터를 반환하도록 ECMAScirpt 사양에 규정되어 있다.

만약 빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면 이터레이션 프로토콜을 따르면 된다.

ECMAScript 사양에 규정되어 있는 대로 Well-known Symbol인 Symbol.iterator를 키로 갖는 메소드를 객체에 추가하고 이터레이터를 반환하도록 구현하면 그 객체는 이터러블이 된다.

```javascript
// 1 ~ 5 범위의 정수로 이루어진 이터러블
const iterable = {
  // Symbol.iterator 메소드를 구현하여 이터러블 프로토콜을 준수
  [Symbol.iterator]() {
    let cur = 1;
    const max = 5;
    // Symbol.iterator 메소드는 next 메소드를 소유한 이터레이터를 반환
    return {
      next() {
        return { value: cur++, done: cur > max + 1 };
      },
    };
  },
};

for (const num of iterable) {
  console.log(num);
}
```

### 추가 공부

심벌값을 왜 사용해야 할까. 그냥 변수로 ID를 만들어 사용하면 안 되나요?

장점

1. 유일성 : 심벌 값은 항상 유일하므로, 객체의 프로퍼티 키로 사용할 경우 다른 프로퍼티와 충돌하지 않습니다.
2. 은닉성 : 심벌 값은 외부에서 접근할 수 없습니다. 따라서 심벌 값을 사용하여 객체의 프로퍼티를 정의하면해당 프로퍼티에 대해 접근을 제한할 수 있습니다.
3. 확장성 : 심벌 값은 동적으로 생성할 수 있습니다. 런타임 중에 객체의 프로퍼티 키를 동적으로 생성하여 사용할 수 있습니다.

    ```javascript
    const symbolName = "mySymbol";
    const symbol = Symbol(symbolName);
    console.log(symbol); // Symbol(mySymbol)
    
    const obj = {};
    obj[symbol] = "value";
    console.log(obj[symbol]); // value
    ```

4. 이름 충돌 방지 : 다른 변수나 함수와 이름이 충돌하지 않아 프로그램 안정성을 높일 수 있습니다.
5. 메소드 오버라이딩 : 객체의 메소드를 오버라이딩 할 때, 심벌 값을 사용하면 기존 메소드와 구별할 수 있습니다.
6. 반복문에서 제외 :심벌 값을 사용하여 객체의 프로퍼티를 제외하면, for…in 반복문에서 해당 프로퍼티를 제외할 수 있습니다.

단점

1. 가독성 : 심벌 값은 항상 유일하므로 코드를 이해하기 어려울 수 있습니다. 다른 변수와 충돌하지 않도록 유일한 이름을 지정해줘야 하기에 가독성이 낮아질 수 있습니다.
2. 디버깅: 심벌값은 유일하므로, 디버깅 과정에서 문제가 발생할 시 원인을 파악하기 어려울 수 있습니다. 두 개의 객체가 있고 각 객체의 프로퍼티 키로 심벌 값을 사용하고 있다면, 두 객체의 구별이 어려울 수
   있습니다.
3. 메모리 사용: 심벌 값은 항상 유일하므로, 매번 새로운 심벌 값을 생성해야 합니다. 이는 메모리 사용에 큰 영향을 미칩니다.
4. JSON 직렬화 : 심벌 값은 JSON 에서 직렬화 할 수 없습니다. 직렬화할 때 심벌 값을 포함하면 무시되며, JSON으로 변환되지 않습니다.