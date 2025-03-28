# 20장 strict mode

## 1 strict mode란?

ES5부터 추가된 strict mode(엄격 모드)로서 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

## 2 strict mode의 적용

strict mode를 적용하려면 전역의 선두 또는 함수 몸체의 선두에 ‘use strict’;를 추가한다.

```javascript
// [예제 20-02

"use strict";

function foo() {
  x = 10; // ReferenceError : x is not defined
}

foo();
```

## 3 전역에 strict mode를 적용하는 것은 피하자

전역에 적용한 strict mode는 스크립트 단위로 적용된다.

```html
<!DOCTYPE html>
<html>
  <script></script>
  <body>
    <script>
      x = 1; // isNot ReferenceError
      console.log(x); // 1
    </script>
    <script>
      "use strict";

      y = 1; // ReferenceError : y is not defined
      console.log(y);
    </script>
  </body>
</html>
```

해당 strict mode는 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 사용한다.

## 4 함수 단위로 strict mode를 적용하는 것도 피하자

함수마다 strict mode를 적용할 수도 있다. 그러나 어떤 함수는 strict mode를 적용하고 어떤 함수는 적용하지 않는 것은 바람직하지 않으며 모든 함수에 일일이 적용하는 것도 번거로운 일이다.

따라서 strict mode는 즉시 실행함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

```javascript
(function(){
  // 비엄격 모드
  var let = 10; // 에러가 발생하지 않는다.

  function foo(){
    'use strict';

    let = 20; // SyntaxError : Unexpected strict mode reserved word
  }
  foo();
}());
```

## 5 strict mode가 발생시키는 에러

### 5.1 암묵적 전역

선언하지 않은 변수를 참조하면 ReferenceError가 발생한다.

### 5.2 변수, 함수, 매개변수의 삭제

delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError 가 발생한다.

### 5.3 매개변수 이름의 중복

중복된 매개변수 이름을 사용하면 SyntaxError가 발생한다.

### 5.4 with 문의 사용

with문을 사용하면 SyntaxError가 발생한다. with문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서 코드가 간단해지는 효과가 있지만 성능과 가독성이 나빠진다.

## 6 strict mode 적용에 의한 변화

### 6.1 일반 함수의 this

stirct mode 에서 함수를 일반함수로서 호출하면 this에는 undefined가 바인딩된다. 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문이다.

```javascript
(function(){
  'use strict';

  function foo(){
    console.log(this); // undefined
  }
  foo();

  function Foo(){
    console.log(this); // Foo
  }
  new Foo();
}())
```

### 6.2 arguments 객체

strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

```javascript
(function (a) {
  "use strict";
  // 매개변수에 전달된 인수를 재할당하여 변경;

  a = 2;

  // 변경된 인수가 arguments 객체에 반영되지 않는다.
  console.log(arguments); // { 0: 1, length: 1 }
})(1);
```