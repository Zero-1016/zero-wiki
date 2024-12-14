# Closure

Closure(클로져)

> 클로저는 **함수가 생성될 때의 어휘적 범위(Lexical Scope)를 기억**하여, **그 범위 밖에서도 접근할 수 있는 함수**를 의미합니다.

```JavaScript
function fn1() {
    const text = "hello world";
    
    return function () {
        console.log(text);
    };
}

const fn2 = fn1();
fn2(); // 출력: "hello world"
```

## 실행 과정

1. fn1이 실행되면 text 변수와 익명 함수가 포함된 어휘적 환경(Lexical Environment)이 생성됩니다.
2. fn1이 반환하는 익명 함수는 fn2에 저장됩니다.
3. 이때 반환된 익명 함수는 fn1의 어휘적 환경을 참조하고 있습니다. 따라서 fn2는 fn1의 text 변수에 접근할 수 있습니다.

![자바스크립트_클로저_설명.png](자바스크립트_클로저_설명.png)

위 그림처럼 반환된 익명 함수는 `Outer Execution Context` 라는 형태로 fn1의 실행 컨텍스트에 대한 정보를 유지합니다.
결과적으로 fn2는 fn1의 내부 변수인 text를 기억하고 사용할 수 있습니다.

## 요약
클로저는 함수가 생성될 때의 환경을 기억하여, 해당 환경에 접근할 수 있는 기능을 제공합니다.
클로저는 주로 데이터 캡슐화 및 상태 유지에 유용하게 사용됩니다.