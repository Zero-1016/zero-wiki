# This

This

> `this`는 함수가 호출되는 방식에 따라 값이 결정됩니다.

JavaScript의 `this`는 특정한 문맥(`context`)에 따라 다른 객체를 가리키는 키워드입니다. `this`의 동작은 코드가 실행되는 방식, 호출되는 방법, 그리고 선언된 위치에 따라 달라집니다.

## 상황에 따른 `this`의 값

### 전역 컨텍스트에서

- `this`는 전역 객체를 참조합니다.

    - 브라우저 환경: window
    - Node.js 환경: global

```JavaScript
console.log(this); // 브라우저에서는 window 출력
```

### 함수 내부에서 (엄격 모드가 아닐 때)

- 일반 함수에서 `this`는 전역 객체를 가리킵니다.

```JavaScript
function showThis() {
  console.log(this);
}

showThis(); // 브라우저에서는 window 출력
```

**엄격 모드 ('use strict')** 에서는 `this`는 `undefined`가 됩니다.

```JavaScript
'use strict';

function showThis() {
  console.log(this);
}

showThis(); // undefined
```

### 메서드 내부에서

- 객체의 메서드에서 this는 그 메서드를 호출한 객체를 참조합니다.

```JavaScript
const obj = {
  name: 'JavaScript',
  getName: function () {
    return this.name;
  },
};

console.log(obj.getName()); // 'JavaScript'
```

### 화살표 함수 내부에서

- 화살표 함수는 자신만의 `this`를 가지지 않고, 상위 스코프의 `this`를 그대로 사용합니다.

```JavaScript
const obj = {
  name: 'JavaScript',
  getName: () => {
    console.log(this);
  },
};

obj.getName(); // 전역 객체 (window or global)
```

### 생성자 함수 또는 클래스에서

- 생성자 함수나 클래스에서 `this`는 새로 생성되는 객체를 참조합니다.

```JavaScript
function Person(name) {
  this.name = name;
}

const person = new Person('Zero');
console.log(person.name); // 'Zero'
```

### `bind`, `call`, `apply`를 사용한 명시적 바인딩

- `this`를 명시적으로 설정할 수 있습니다.

```JavaScript
const obj = { name: 'JavaScript' };

function showName() {
  console.log(this.name);
}

showName.call(obj);  // 'JavaScript'
showName.apply(obj); // 'JavaScript'

const boundFunc = showName.bind(obj);
boundFunc(); // 'JavaScript'
```

> bind가 되어있는 개체에 한번 더 bind를 할 경우 무시하게 됩니다.

### 이벤트 핸들러에서

- DOM 이벤트 핸들러에서 `this`는 이벤트를 발생시킨 요소를 참조합니다.

```JavaScript
const button = document.querySelector('button');
button.addEventListener('click', function () {
  console.log(this); // 클릭된 버튼 요소
});
```

- 화살표 함수 사용 시, 상위 스코프의 this를 사용합니다.

```JavaScript
button.addEventListener('click', () => {
  console.log(this); // 전역 객체 (window)
});
```

## 요약

- `this`는 함수 호출 방식에 따라 값이 달라집니다.
- 일반 함수: 전역 객체 (strict 모드에서는 `undefined`).
- 메서드: 호출한 객체.
- 화살표 함수: 상위 스코프의 `this`.
- 생성자: 새로 생성된 객체.
- 명시적 바인딩 (`call`, `apply`, `bind`): `this`를 강제 설정.
- 이벤트 핸들러: 이벤트를 발생시킨 DOM 요소.