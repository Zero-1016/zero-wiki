# 32장 String

표준 빌트인 객체인 String은 원시 타입인 문자열을 다룰 때 유용한 프로퍼티와 메소드를 제공한다.

## 1 String 생성자 함수

String 객체는 생성자 함수 객체이다. 따라서 new 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있다. 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 빈문자열을 할당한 String 래퍼 객체를 생성한다.

```javascript
const strObj = new String();
console.log(strObj); // String { length: 0, [[PrimitiveValue]]: ""}
```

위 코드를 개발자 도구에서 실행해보면 [[PrimitiveValue]] 라는 접근할 수 없는 프로퍼티가 보인다. 이는 [[StringData]] 내부 슬롯을 가리킨다. ES5에서는 [[StringData]]를 [[Primitive Value]]라 불렀다.

String 생성자 함수의 인수로 문자열을 전달하면서 new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 인수로 전달받은 문자열을 할당한 String 래퍼 객체를 생성한다.

```javascript
const strObj = new String('Lee');
console.log(strObj);
// [String: 'Lee']
```

String 래퍼 객체는 배열과 마찬가지로 length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 유사 배열 객체이면서 이터러블이다.

```javascript
console.log(strObj[0]); // L
```

단 문자열은 원시 값이므로 변경할 수 없다.

String 생성자함수의 인수로 문자열이 아닌 인수를 전달하면 강제 변환한 후, [[StringData]]에 변환된 문자열을 할당한 String 래퍼 객체를 생성한다

new 연산자를 사용하지 않고 String 생성자 함수를 호출하면 String 인스턴스가 아닌 문자열을 반환한다. ⇒ 명시적 타입 변환

## 2 length 프로퍼티

```javascript
'Hello'.length; // => 5
'안녕하세요!'.length; // => 6
```

String 래퍼 객체는 배열과 마찬지인 유사배열, 즉 length 프로퍼티와 인덱스를 나타내는 숫자를 프로퍼티 키로, 각 문자를 프로퍼티 값으로 가지므로 String 래퍼 객체는 유사 배열 객체다.

인덱스 번호로 해당 문자열이 아닌 문자에 접근  가능하다.

## 3 String 메소드

String 메소드는 String 타입 자체가 원시 타입 즉 변경이 불가능한 원시 값이기 때문에 **String 래퍼 객체도 읽기 전용(read-only) 객체로 제공된다.**

### 3.1 String.prototype.indexOf

대상 문자열(메소드를 호출한 문자열)에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환한다. 실패하면 -1을 반환한다.

```javascript
const str = 'Hello World';

// 문자열 str에서 'l'을 검색하여 첫 번째 인덱스를 반환한다.
str.indexOf('l'); // => 2

// 문자열 str에서 'or'을 검색하여 첫 번째 인덱스를 반환한다.
str.indexOf('or'); // => 7

// 문자열 str에서 'x'을 검색하여 첫 번째 인덱스를 반환한다.
str.indexOf('x'); // => 3

// 문자열 str의 인덱스 3부터 'l'을 검색하여 첫 번째 인덱스를 반환한다.
str.indexOf('l', 3); // -> 3

if(str.indexOf('Hello') !== -1){
    // 문자열 str에 'Hello'가 들어있는 경우 처리할 내용
}

// ES6에 서 도입된 String.prototype.includes 메소드를 사용하면 가독성이 더 좋다.
if(str.includes('Hello')){
    // 문자열 str에 'Hello'가 들어있는 경우 처리할 내용
}
```

### 3.2 String.prototype.search

대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환한다.

```javascript
const str = "Hello world";

// 문자열 str에서 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환한다.
console.log(str.search(/o/)); // 4
console.log(str.search(/x/)); // -1
```

### 3.3. String.prototype.includes

ES6에서 도입된 이 메소드는 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 결과값을 불 타입으로 반환한다.

```javascript
const str = "Hello world";

console.log(str.includes("Hello")); // true
console.log(str.includes("")); // true
console.log(str.includes("x")); // false
console.log(str.includes()); // false
```

include 메소드의 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

```javascript
const str = "Hello world";

console.log(str.includes("l", 3)); // true
console.log(str.includes("H", 3)); // false
```

### 3.4 String.prototype.startsWith

ES6에서 도입된 이 메소드는 대상 문자열이 인수로 저달받은 문자열로 시작하는지 확인하여 결과 값을 불 타입으로 가져온다.

```javascript
const str = "Hello world";

// 문자열 str이 'He'로 시작하는지 확인
console.log(str.startsWith("He")); // true
// 문자열 str이 'x'로 시작하는지 확인
console.log(str.startsWith("x")); // false
```

startsWith의 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

```javascript
const str = "Hello world";

// 문자열 str의 인덱스 5부터 시작하는 문자열이 ' ' 로 시작하는지 확인
console.log(str.startsWith(" ", 5)); // true
```

### 3.5 String.prototype.endsWith

ES6에서 도입된 endsWith 메소드는 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 그 결과를 true 또는 false로 반환한다.

```javascript
const str = "Hello world";

// 문자열 str이 ld로 끝나는지 확인.
console.log(str.endsWith("ld")); // true
// 문자열 str이 x로 끝나는지 확인.
console.log(str.endsWith("x")); // false
```

endsWith 메소드의 2번째 인수로 검색할 문자열의 길이를 전달할 수 있다.

```javascript
const str = "Hello world";

// 문자역 str의 처음부터 5자리까지 ('Hello')가 'lo'로 끝나는지 확인
console.log(str.endsWith("lo", 5)); // true
```

### 3.6 String.prototype.charAt

charAt 메소드는 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환한다.

```javascript
const str = "Hello";

for (i in str) {
  console.log(str.charAt(i));
}
// H e l l o
```

인덱스는 문자열의 범위, 즉 0 ~ (문자열 길이 - 1) 사이의 정수이어야 한다. 인덱스가 문자열의 범위를 벗어난 정수인 경우 빈 문자열을 반환한다.

```javascript
// 인덱스가 문자열의 범위(0 ~ str.length-1)을 벗어난 경우 빈 문자열을 반환한다.
str.charAt(5); // => ''
```

비슷한 메소드로는 String.prototype.charCodeAt과 String.prototype.codePointAt이 있다.

### 3.7 String.prototype.substring

substring메소드는 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열을 반환한다.

```javascript
const str = 'Hello world';

// 인덱스 1번부터 인덱스 4 이전까지의 부분 문자열을 반환한다.
str.substring(1, 4);
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bcaa5059-4b47-4b2a-9d30-603d5cd2ae6f/Untitled.png)

substring의 메소드의 두번째 인수를 생략한 경우 첫 번째 인수로 전달한 인덱스에 위치하는 문자 부분부터 마지막 문자까지 부분 문자열을 반환한다.

```javascript
const str = "Hello world";

// 인덱스 1번부터 마지막 문자까지 부분 문자열을 반환한다.
console.log(str.substring(1)); // ello world
```

substring 메소드의 첫 번째 인수는 두 번째 인수보다 작은 정수이어야 정상이다 하지만 다음과 같은 경우에서도 작동이 된다.

1. 첫 번째 인수 > 두 번째  인수인 경우 두 인수는 교환이 된다.
2. 인수 < 0 또는 NaN일 경우 0으로 취급이 된다.
3. 인수 > 문자열의 길이(str.length)인 경우 인수는 문자열의 길이 (str.length)로 취급된다.

```javascript
const str = "Hello world";

// 첫 번째 인수 > 두번째 인수인 경우 두 인수는 교환된다.
console.log(str.substring(4, 1)); // ell

// 인수 < 0 또는 NaN인 경우 0으로 취급된다.
console.log(str.substring(-2)); // Hello world

// 인수 > 문자열의 길이(str.length)인 경우 인수는 문자열의 길이(str.length)로 취급된다.
console.log(str.substring(1, 100)); // ello world
console.log(str.substring(20)); // ''
```

String.prototype.indexOf 메소드와 함께 사용하면 특정 문자열을 기준으로 앞뒤에 위치한 부분 문자열을 취득할 수 있다.

```javascript
const str = "Hello world";

// 스페이스를 기준으로 앞에 있는 부분 문자열 취득
console.log(str.substring(0, str.indexOf(" "))); // Hello

// 스페이스를 기준으로 뒤에 있는 부분 문자열 취득
console.log(str.substring(str.indexOf(" ") + 1, str.length)); // world
```

### 3.8 String.prototype.slice

substring 메소드와 동일하게 동작한다 단, slice 메소드에는 음수인 인수를 전달할 수 있다. 음수인 인수를 전달하면 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환한다.

```javascript
const str = "hello world";

// substring과 slice 메소드는 동일하게 동작한다.
// 0번째부터 5번째 이전 문자까지 잘라내어 반환
console.log(str.substring(0, 5)); // hello world
console.log(str.slice(0, 5)); // hello world

// 인덱스가 2인 문자부터 마지막 문자까지 잘라내어 반환
console.log(str.substring(2)); // llo world
console.log(str.slice(2)); // llo world

// 인수 < 0 또는 NaN인 경우 0으로 취급된다.
console.log(str.substring(-5)); // hello world
// slice 메소드는 음수인 인수를 전달할 수 있다. 뒤에서 5자리를 잘라내어 반환한다.
console.log(str.slice(-5)); // world
```

### 3.9 String.prototype.toUppperCase

대상 문자열을 모두 대문자로 변경한 문자열을 반환한다.

```javascript
const str = "Hello world";

console.log(str.toUpperCase()); // HELLO WORLD
```

### 3.10 String.prototype.toLowerCase

대상 문자열을 모두 소문자로 변경한 문자열을 반환한다.

```javascript
const str = "Hello world";

console.log(str.toLowerCase()); // hello world
```

### 3.11 String.prototype.trim

trim 메소드는 대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환한다.

```javascript
const str = "     foo     ";

console.log(str.trim()); // foo
```

2021년 1월, stage 4에 제안되어 있는 String.prototype.trimStart, String.prototype.trimEnd를 사용하면 대상 문자열 앞 또는 뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환한다.

```javascript
const str = "     foo     ";

console.log(str.trimStart()); // 'foo   '
console.log(str.trimEnd()); // '    foo'
```

String.prototype.replace 메소드에 정규 표현식을 인수로 전달하여 공백 문자를 제거할 수도 있다.

```javascript
const str = "     foo     ";

// 첫 번째 인수로 전달한 정규 표현식에 매치하는 문자열을 두 번째 인수로 전달한 문자열로 치환한다.
console.log(str.replace(/\s/g, "")); // 'foo'
console.log(str.replace(/^\s+/g, "")); // 'foo     '
console.log(str.replace(/\s+$/g, "")); // '     foo'
```

### 3.12 String.prototype.repeat

ES6에서 도입된 repeat 메소드는 인수로 전달받은 정수만큼 반복해 새로운 문자열을 반환한다. 전달받은 정수가 0이면 빈 문자열을 반환하고, 음수면 RangeError를 발생시킨다. 생략하면 0이다.

```javascript
const str = "abc";

console.log(str.repeat()); // ''
console.log(str.repeat(0)); // ''
console.log(str.repeat(1)); // 'abc'
console.log(str.repeat(2)); // 'abcabc'
console.log(str.repeat(2.5)); // 'abcabc' 2.5 => 2
// console.log(str.repeat(-1)); // RangeError: Invalid count value
```

### 3.13 String.prototype.replace

대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환한다.

```javascript
const str = "Hello world";

// str에서 첫 번째 인수 'world'를 검색하여 두 번째 인수 'Lee'로 치환한다.
console.log(str.replace("world", "Lee")); // Hello Lee
```

검색된 문자열이 여럿 존재할 경우 첫 번째로 검색된 문자열만 치환한다.

```javascript
const str = "Hello world world";

// str에서 첫 번째 인수 'world'를 검색하여 두 번째 인수 'Lee'로 치환한다.
console.log(str.replace("world", "Lee")); // Hello Lee world
```

특수한 교체 패턴을 사용할 수 있다. $&는 검색된 문자열을 의미한다.

- 교체 패턴에 대한 자세한 내용은 MDN의 함수 설며을 참고할 것

```javascript
const str = "Hello world";

// 특수한 교체 패턴을 사용할 수 있다. ($& => 검색된 문자열)
console.log('world', '<strong>$&</strong>');
```

replace 메소드의 첫 번째 인수로 정규 표현식을 전달할 수도 있다.

```javascript
const str = "Hello Hello";

// 'hello'를 대소문자를 구별하지 않고 전역 검색한다.
console.log(str.replace(/hello/gi, "Lee")); // Lee Lee
```

replace 메소드의 두 번째 인수로 치환 함수를 전달할 수 있다.

### 카멜케이스를 스네이크 케이스로, 스네이크 케이스를 카멜케이스로 변겨하는 함수

```javascript
// 카멜 케이스를 스네이크 케이스로 변환하는 함수
function camelToSnake(camelCase) {
  // .[A-Z]/g 는 임의의 한 문자와 대문자로 이루어진 문자열에 매치한다.
  // 치환 함수의 인수로 매치 결과가 전달되고, 치환 함수가 반환한 결과와 매치 결과를 치환한다.
  return camelCase.replace(/.[A-Z]/g, (match) => {
    // console.log(match); // 'oW'
    return match[0] + "_" + match[1].toLowerCase();
  });
}

const camelCase = "helloWorld";
console.log(camelToSnake(camelCase)); // hello_world

// 스네이크 케이스를 카멜 케이스로 변환하는 함수
function snakeToCamel(snakeCase) {
  // /_[a-z]/g는 _와 소문자로 이루어진 문자열에 매치한다.
  // 치환 함수의 인수로 매치 결과가 전달되고, 치환 함수가 반환한 결과와 매치 결과를 치환한다.
  return snakeCase.replace(/_[a-z]/g, (match) => {
    // console.log(match); // '_w'
    return match[1].toUpperCase();
  });
}

const snakeCase = "hello_world";
console.log(snakeToCamel(snakeCase)); // helloWorld
```

### 3.14 String.prototype.split

대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분 한 후 분리된 각 문자열로 이루어진 배열을 반환한다. 인수로 빈 문자열을 전달하면 모두 분리하고 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.

```javascript
const str = "How are you doing?";

// 공백으로 구분(단어로 구분)하여 배열로 반환한다.
str.split(" "); // => ['How', 'are', 'you', 'doing?']

// \s는 여러 가지 공백 문자(스페이스, 탭 등)를 의미한다. 즉, [\t\r\n\v\f]와 같은 의미다.
str.split(/\s/); // => ['How', 'are', 'you', 'doing?']

// 인수로 빈 문자열을 전달하면 각 문자를 모두 분리한다.
console.log(str.split(""));
// 'H', 'o', 'w', ' ', 'a', 'r', 'e', ' ', 'y', 'o', 'u', ' ', 'd', 'o', 'i', 'n', 'g', '?'

// 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.
console.log(str.split()); // [ 'How are you doing?' ]
```

두 번째 인수로 배열의 길이를 지정할 수 있다.

```javascript
// 공백으로 구분하여 배열로 반환한다. 단, 배열의 길이는 3이다.
str.split(' ', 3); // => ["How", "are", "yoe"]
```

split  메소드는 배열을 반환한다 따라서 배열의 고차함수와 함께 문자열을 역순으로 뒤집을 수 있다.

```javascript
// 인수로 전달받은 문자열을 역순으로 뒤집는다.
function reverseString(arr) {
  return arr.split("").reverse().join("");
}

console.log(reverseString("Hello world!"));
// !dlrow olleH
```