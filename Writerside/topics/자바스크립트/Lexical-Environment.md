# Lexical Environment

어휘적 환경 (Lexical Environment)

정의
> 변수나 함수 등의 식별자를 정의할 때 사용되는 명세서의 역할을 합니다.

- 중첩된 어휘적 환경에 기반해 동작합니다.

```javascript
function a(){
    function b(){
        // 중첩된 어휘적 환경
    }
}
```

- Environment Record와 outer 속성을 포함하고 있습니다.

Environment Record는 식별된 변수들의 정보를 가지고 있습니다.

outer는 외부 환경을 가리키고 있습니다.

```JavaScript
const a = 1;

function foo(){
    const a = 10;
    const b = "ten";
    function fn(){};
}
```

위 코드는 다음과 같은 값들을 포함하고 있습니다.

```Plain Text
Global Lexical Environment

Environment Record:
    - a = 1
    - foo = function

outer: null
```

선언된 a의 값을 1로 명세하고 있고, foo의 값을 함수로서 명세하고 있습니다.

foo 함수의 Lexical Environment를 확인하면 다음과 같습니다.

```plain text
Environment Record:
- a = 10
- b = "tex"
- fn = function

outer: Global Lexical Environment
```

foo 함수로 실행이 되었기 떄문에 foo에 대한 정보를 가지고 있습니다.

선언되어있는 명세로는 변수의 값과 함수를 기록하고 있습니다.

outer의 값으로는 글로벌 내에 선언이 되어있기 때문에 외부인 글로벌을 가리키고 있습니다.
