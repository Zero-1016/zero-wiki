# Lexical Scope

Lexical Scope(어휘적 범위)

## 정의
> 어휘적 범위(Lexical Scope)란 코드 작성 시점에 결정된 변수의 접근 범위를 의미합니다.

```JavaScript
function fn() {
    const number = 0;
    console.log(number); // 0
}

fn();
```

위 코드에서 `fn` 함수 내부에서는 `number` 변수에 접근할 수 있어 값을 출력할 수 있습니다.
그러나 함수 외부에서 `number` 변수에 접근하려고 하면 **참조 에러(Reference Error)** 가 발생합니다.

```JavaScript
function fn() {
    const number = 0;
    console.log(number); // 0
}

fn();
console.log(number); // Reference Error
```

`number` 변수는 `fn` 함수 내부에서만 유효하기 때문에, 함수 외부에서는 해당 변수에 접근할 수 없습니다.

## 예제: 외부 변수 접근

만약 변수가 함수 외부에서 정의되어 있다면, 함수 내부에서도 해당 변수에 접근할 수 있습니다.

```JavaScript
const number = 0;

function fn() {
    console.log(number); // 0
}

fn();
```

이처럼 블록 내부에서는 외부 범위에 접근할 수 있지만, 외부에서는 블록 내부 범위에 접근할 수 없습니다.

이러한 현상은 어휘적 범위(Lexical Scope)의 특징 때문입니다.

## 범위의 구분
> JavaScript에서 범위를 구분하는 주요 요소는 다음과 같습니다.

- 함수(Function)
- 블록문(Block Statement): if, for, while 등
- 예외 처리문(Try-Catch)

### 호이스팅

`var` 키워드는 변수 선언 시 몇 가지 예측하기 어려운 문제를 일으킬 수 있습니다. 대표적인 문제가 **호이스팅(Hoisting)** 입니다.

```JavaScript
function fn() {
  console.log(text);
}

fn();

const text = 'Hello World!';
```

위 코드는 `fn` 함수가 실행되기 이전 text 변수가 정의되어 있지 않기 때문에 참조 에러를 발생합니다.

하지만 `var` 키워드를 사용할 경우 의도한 대로 동작하지 않습니다.

### const 또는 let과의 차이점

예제: const 또는 let 사용

```JavaScript
function fn() {
    console.log(text); // Reference Error
}

fn();

const text = 'Hello World!';
```
해당 코드는 개발자의 의도와 달리 `text`의 주소가 먼저 선언이 되기 때문에 값이 할당되지 않은채로 `undefined` 의 값을 출력을 하게 됩니다. 이러한 현상을 **호이스팅**이라고 합니다.

### 예제: var 사용

```JavaScript
function fn() {
    console.log(text);
}

fn(); // undefined

var text = 'Hello World!';
```

위 코드는 **호이스팅(Hoisting)** 현상 때문에 `text` 변수가 선언만 된 상태에서 함수가 실행됩니다.
따라서 `text` 변수는 값이 초기화되기 전의 상태(`undefined`)로 출력됩니다.

### 호이스팅 동작 설명

JavaScript 엔진은 `var` 키워드로 선언된 변수를 다음과 같이 처리합니다.

```JavaScript
var text = undefined; // 선언부가 호이스팅됨

function fn() {
    console.log(text); // undefined
}

fn();

text = 'Hello World!'; // 할당부는 호이스팅되지 않음
```

`var`로 선언된 변수는 선언부만 위로 끌어올려지고, 초기화는 나중에 이루어지기 때문입니다.

## 권장 사항
`var` 키워드는 예측 가능한 코드를 작성하기 어렵게 만들기 때문에 사용하지 않는 것이 좋습니다.
대신 `let` 또는 `const` 키워드를 사용하여 변수의 유효 범위를 명확히 정의하세요.

`const`: 재할당이 필요 없는 상수 선언 시 사용
`let`: 값의 재할당이 필요한 경우 사용