# Execution Context

실행 컨텍스트(Execution Context)

## 정의

> **실행 컨텍스트(Execution Context)**란 자바스크립트 코드가 실행되는 환경을 의미하며, 모든 자바스크립트 코드는 실행 컨텍스트 내부에서 실행됩니다.

주요 실행 컨텍스트:

- 전역 실행 컨텍스트 (Global Execution Context)

  스크립트 전체를 감싸는 최상위 실행 컨텍스트.
  애플리케이션이 종료될 때까지 유지됩니다.

- 함수 실행 컨텍스트 (Function Execution Context)

  함수가 호출될 때마다 새로 생성됩니다.

## 예제

```javascript
const text = "Hello";
console.log(text); // Hello

function fn() {
    const label = "world";
    console.log(label); // world
}
fn();
```

---

### 전역 실행 컨텍스트(Global Execution Context)
위 코드의 전역 실행 컨텍스트는 다음과 같이 구성됩니다.

- Global Object: 전역 객체(`window` 또는 `globalThis`)를 가리킵니다.
- Outer: 외부 환경 정보를 나타내며, 전역 컨텍스트에서는 `null`을 가집니다.
- Function: 함수 실행으로 생성된 컨텍스트가 아니므로 `null`을 가집니다.

![Execution_Context.png](자바스크립트_Execution_Context.png)

`Function`은 만들어진 실행 컨텍스트가 함수로 인해 만들어진 경우 그때 함수 객체를 가리킵니다. 하지만 글로벌 컨텍스트의 경우 함수로 만들어진 경우가 아니기 때문에 `null`을 간직합니다.

`Outer`의 경우 글로벌 컨텍스트이기 때문에 `null`을 간직하고 있습니다.

또한 함수들은 `Enviorment`라는 속성을 가지고 있는데 해당 함수들은 자신이 선언된 환경에 대한 정보들을 가지고 있습니다.

---

### 함수 실행 컨텍스트(Function Execution Context)

`fn` 함수의 실행 컨텍스트는 다음과 같은 구조를 가집니다.

- Environment

  - 함수가 선언된 환경에 대한 정보를 가집니다.
  - `fn`의 경우, 전역 실행 컨텍스트의 정보를 참조합니다.
  
- Outer

  - `fn`은 전역 컨텍스트에서 선언되었으므로 `Outer`는 전역 컨텍스트의 정보를 가리킵니다.

fn은 전역 컨텍스트에서 선언되었으므로 Outer는 전역 컨텍스트의 정보를 가리킵니다.

![OuterEnvironment.png](자바스크립트_OuterEnvironment.png)

때문에 fn의 경우 자신이 선언된 글로벌 컨텍스트의 정보를 가지고 있습니다.

![자바스크립트_글로벌_컨텍스트_정보_설명.png](자바스크립트_글로벌_컨텍스트_정보_설명.png)

## 요약
- 전역 실행 컨텍스트: 자바스크립트 코드가 실행되기 시작할 때 생성됩니다.
- 함수 실행 컨텍스트: 함수 호출 시 생성되며, 호출이 종료되면 제거됩니다.
  - Environment: 함수가 선언된 외부 환경 정보를 저장하며, 이 정보로 변수나 함수에 접근합니다.
  - Outer: 현재 컨텍스트의 외부 실행 컨텍스트를 참조합니다.