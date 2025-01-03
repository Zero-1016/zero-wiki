# 31장 RegExp

## 1 정규 표현식이란?

일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어(format language)다.이는 자바스크립트 고유의 문법이 아니며 대부분의 프로그래밍 언어와 코드 에디터에 내장되어 있다.

정규 표현식은 문자열을 대상으로 패턴 매칭 기능을 제공한다.

**패턴 매칭 기능이란?**

특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능을 말한다.

```javascript
// 사용자로부터 입력받은 휴대폰 전화번호
const tel1 = "010-1234-5678";
const tel2 = "010-1234-567팔";

// 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의한다.
const phonePattern = /^\d{3}-\d{4}-\d{4}$/;

// tel이 휴대폰 전화번호 패턴에 매칭하는지 테스트(확인)한다.
console.log(phonePattern.test(tel1)); // true
console.log(phonePattern.test(tel2));
```

만약 정규표현식을 사용하지 않는다면 반복문과 조건문을 통해 숫자인지 문자인지 하나씩 체크를 해야한다. 반면 정규표현식을 사용하면 반복문과 조건문 없이 패턴을 정의하고 테스트하는 것으로 간단히 체크할 수 있다. 다만
주석이나 공백을 허용하지 않고 기호를 혼합하여 사용하기에 가독성이 좋지 않다.

## 2 정규 표현식의 생성

정규 표현식 객체(RegExp 객체)를 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수를 사용할 수 있습니다.

![모던 자바스크립트 31-1.png](모던 자바스크립트 31-1.png)

이처럼 정규 표현식 리터럴은 패턴과 플래그로 구성된다.

```javascript
const target = 'Is this all there is?';

// 패턴 : is
// 플래그 : i => 대소문자를 구별하지 않고 검색한다.
const regexp = /is/i;

// test 메소드는 target 문자열에 대해 정규 표현식 regexp의 패턴을 검색하여 매칭 결과를
// 불리언 값으로 반환한다.
regexp.test(target); // true
```

RegExp 생성자 함수를 사용하여 RegExp 객체를 생성할 수도 있다.

```javascript
/**
 * pattern: 정규 표현식의 패턴
 * flags: 정규 표현식의 플래그(g, i, m, u, y)
 */

new RegExp(pattern[ , flags]);
```

```javascript
const target = 'Is this all there is?';
const regexp = new RegExp(/is/i); // ES6
//const regexp = new RegExp(/is/, 'i');
//const regexp = new RegExp('is', 'i');

regexp.test(target); // true
```

RegExp 생성자 함수를 사용하면 변수를 사용해 동적으로 RegExp 객체를 생성할 수도 있다.

```javascript
const count = (str, char) => (str.match(new RegExp(char, "gi")) ?? []).length;
console.log(count("Is this all there is", "is")); // 3
console.log(count("Is this all there is", "xx")); // 0
```

## 3 RegExp 메소드

### 3.1 RegExp.prototype.exec

exec 메소드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매킹 결과를 배열로 반환한다. 결과가 없는 경우 null을 반환한다.

```javascript
const target = "Is this all there is?";
const regExp = /is/;

console.log(regExp.exec(target)); 
// [ 'is', index: 5, input: 'Is this all there is?', groups: undefined ]
```

exec 메소드는 문자열내의 모든 패턴을 검색하는 g 플래그를 지정해도 첫 번째 매칭 결과만 반환하므로 주의해야한다.

### 3.2 RegExp.prototype.test

test 메소드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.

```javascript
const target = "Is this all there is?";
const regExp = /is/;

console.log(regExp.test(target));
// true
```

### 3.3 String.prototype.match

String 표준 빌트인 객체가 제공하는 match 메소드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다,

```javascript
const target = "Is this all there is?";
const regExp = /is/;

console.log(target.match(regExp));
// [ 'is', index: 5, input: 'Is this all there is?', groups: undefined ]
```

exec 메소드는 문자열 내의 모든 패턴을 검색하는 g 플래그를 지정해도 첫 번째 매칭결과만 반환한다. 하지만 String.prototype.match 메소드는 g 플래그가 지정되면 모든 매칭 결과를 배열로
반환한다.

```javascript
const target = "Is this all there is?";
const regExp = /is/g;

console.log(target.match(regExp));
// [ 'is', 'is' ]
```

## 4 플래그

패턴과 함께 정규 표현식을 구성하는 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.

| 플래그 | 의미          | 설명                                   |
|-----|-------------|--------------------------------------|
| i   | Ignore case | 대소문자를 구별하지 않고 패턴을 검색한다.              |
| g   | Global      | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색합니다. |
| m   | Multi line  | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다,.          |

플래그는 옵션이므로 선택적으로 사용할 수 있고, 순서와 상관없이 하나 이상의 플래그를 동시에 설정할 수도 있다.어떠한 플래그를 사용하지 않은 경우에 대소문자를 구별해서 패턴을 검색한다. 그리고 매칭 대상이 1개 이상
존재해도 첫 번째 매칭한 대상만 검색하고 종료한다.

```javascript
const target = "Is this all there is?";

// target 문자열에서 is 문자열을 대소문자를 구별하여 한 번만 검색한다.
console.log(target.match(/is/));
// [ 'is', index: 5, input: 'Is this all there is?', groups: undefined ]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 한 번만 검색한다.
console.log(target.match(/is/i));
// [ 'Is', index: 0, input: 'Is this all there is?', groups: undefined ]

// target 문자열에서 is 문자열을 대소문자를 구별하여 전역 검색한다.
console.log(target.match(/is/g));
// [ 'is', 'is' ]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 전역 검색한다.
console.log(target.match(/is/gi));
// [ 'Is', 'is', 'is' ]
```

## 5 패턴

정규 표현식은 일정한 규칙(패턴)을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어 (format language)이다. 패턴과 플래그로 구성이 되며, 정규 표현식의 패턴은 문자열의 일정한 규칙을 표현하기
위해 사용하며, 정규 표현식의 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용된다.

패턴은 /로 열고 닫으며 문자열 따옴표는 생략한다. 따옴표를 포함하면 따옴표까지도 패턴에 포함되어 검색된다. 또한 패턴은 특별한 의미를 가지는 메타문자 또는 기호로 표현할 수 있고 문자열 내에 패턴과 일치하는
문자열이 존재할 때 ‘정규 표현식과 매치한다’라고 표현한다.

**패턴을 표현하는 몇 가지 방법들**

### 5.1 문자열 검색

정규 표현식의 패턴에 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열을 검색한다. 정규 표현식을 생성하는 것만으로 검색이 수행되지는 않고, RegExp 메소드를 사용하여 문자열과
정규 표현식의 매칭 결과를 구하면 검색이 수행된다.

검색 대상 문자열과 플래그를 생략한 정규 표현식의 매칭 결과를 구하면 대소문자를 구별하여 정규 표현식과 매치한 첫 번째 결과만 반환한다.

```javascript
const target = "Is this all there is?";

// 'is'문자열과 매치하는 패턴. 플래그가 생략되었으므로 대소문자를 구별한다.
const regExp = /is/;

// target과 정규 표현식이 매치하는지 테스트 한다.
console.log(regExp.test(target)); 
// true

// target의 정규 표현식의 매칭 결과를 구한다.
console.log(target.match(regExp));
// [ 'is', index: 5, input: 'Is this all there is?', groups: undefined ]
```

대소문자를 구별하지 않고 검색하려면 플래그 i를 사용한다.

```javascript
const target = "Is this all there is?";

// 'is'문자열과 매치하는 패턴.
// 플래그 i를 추가하면 대소문자를 구별하지 않는다.
const regExp = /is/i;

console.log(target.match(regExp));
// [ 'Is', index: 0, input: 'Is this all there is?', groups: undefined ]
```

검색 대상 문자열 내에서 패턴과 매치하는 모든 문자열을 전역 검색하려면 플래그 g를 사용한다.

```javascript
const target = "Is this all there is?";

// 'is'문자열과 매치하는 패턴.
// 플래그 g를 추가하면 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.
const regExp = /is/gi;

console.log(target.match(regExp));
// [ 'Is', 'is', 'is' ]
```

### 5.2 임의의 문자열 검색

.은 임의의 문자 한 개를 의미한다. 문자의 내용은 무엇이든 상관없다. 다음 예제의 경우 .을 3개 연속하여 패턴을 생성했으므로 문자의 내용과 상관없이 3자리 문자열과 매치한다.

```javascript
const target = "Is this all there is?";

// 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색한다.
const regExp = /.../g;

console.log(target.match(regExp));
//['Is ', 'thi', 's a', 'll ', 'the', 're ','is?']
```

### 5.3 반복 검색

{m, n}은 앞선 패턴(다음 예제의 경우 A)이 최소 m번, 최대 n번 반복되는 문자열을 의미한다. 콤마뒤에 공백이 있으면 정상 동작하지 않으므로 주의하기 바란다.

```javascript
const target = "A AA B BB Aa Bb AAA";

// 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색합니다.
const regExp = /A{1,2}/g;

console.log(target.match(regExp));
// [ 'A', 'AA', 'A', 'AA', 'A' ]
```

{n}은 앞선 패턴이 n번 반복되는 문자열을 의미한다. 즉, {n}은 {m,n}과 같다.

```javascript
const target = "A AA B BB Aa Bb AAA";

// 'A'가 2번 반복되는 문자열을 전역 검색합니다.
const regExp = /A{2}/g;

console.log(target.match(regExp));
// [ 'AA', 'AA' ]
```

{n,}은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.

```javascript
const target = "A AA B BB Aa Bb AAA";

// 'A'가 최소 2번 이상 반복되는 문자열을 전역 검색합니다.
const regExp = /A{2,}/g;

console.log(target.match(regExp));
// [ 'AA', 'AAA' ]
```

+는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미한다. 즉, +는 {1,}과 같다

```javascript
const target = "A AA B BB Aa Bb AAA";

// 'A'가 최소 한 번 이상 반복되는 문자열을 전역 검색합니다.
const regExp = /A+/g;

console.log(target.match(regExp));
// [ 'A', 'AA', 'A', 'AAA' ]
```

?는 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열을 의미한다. 즉, ? 는{0, 1}과 같다

```javascript
const target = "color colour";

// 'colo' 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고 'r'이 이어지는
//  문자열 'color', 'colour'를 전역 검색한다.
const regExp = /colou?r/g;

console.log(target.match(regExp));
// [ 'color', 'colour' ]
```

### 5.4 OR 검색

|은 or의 의미를 갖는다. 다음 예제의/A|B/는 ‘A’ 또는 ‘B’를 의미한다.

```javascript
const target = "A AA B BB Aa Bb";

// 'A' 또는 'B'를 전역 검색한다.
const regExp = /A|B/g;

console.log(target.match(regExp));
// ['A', 'A', 'A', 'B', 'B', 'B', 'A', 'B']
```

분해되지 않은 단어 레벨로 검색하기 위해서는 +를 함께 사용한다.

```javascript
const target = "A AA B BB Aa Bb";

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색합니다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp = /A+|B+/g;

console.log(target.match(regExp));
// [ 'A', 'AA', 'B', 'BB', 'A', 'B' ]
```

위예제는 패턴을 or로 한 번 이상 반복하는 것인데 이를 간단히 표현하면 다음과 같다. [ ]내의 문자는 or로 동작한다, 그 뒤에 +를 사용하면 앞선 패턴을 한 번 이상 반복한다.

```javascript
const target = "A AA B BB Aa Bb";

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색합니다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp = /[AB]+/g;

console.log(target.match(regExp));
// [ 'A', 'AA', 'B', 'BB', 'A', 'B' ]
```

범위를 지정하려면 [ ] 내에 -를 사용한다. 다음 예제의 경우 대문자 알파벳을 검색한다.

```javascript
const target = "A AA B BB ZZ Aa Bb";

// 'A' ~ 'Z'가 한 번 이상 반복되는 문자열을 전역 검색합니다.
// 'A', 'AA', 'AAA', ... 'B', 'BB', 'BBB', ... 'Z', 'ZZ', 'ZZZ',...

const regExp = /[A-Z]+/g;

console.log(target.match(regExp));
// ["A", "AA", "B", "BB", "ZZ", "A", "B"]
```

대소문자를 구별하지 않고 알파벳을 검색하는 방법은 다음과 같다.

```javascript
const target = "AA BB Aa Bb 12";

// 'A' ~ 'Z' 또는 'a' ~ 'z' 가 한 번 이상 반복되는 문자열을 전역 검색합니다.
const regExp = /[A-Za-z]+/g;

console.log(target.match(regExp));
// [ 'AA', 'BB', 'Aa', 'Bb' ]
```

숫자를 검색하는 방법은 다음과 같다.

```javascript
const target = "AA BB Aa Bb 12,345";

// '0' ~ '9'가 한 번 이상 반복되는 문자열을 전역 검색합니다.
const regExp = /[0-9]+/g;

console.log(target.match(regExp));
// [ '12', '345' ]
```

위 예제의 경우 쉼표 때문에 매칭 결과가 분리되므로 쉼표를 패턴에 포함시킨다.

```javascript
const target = "AA BB Aa Bb 12,345";

// '0' ~ '9'가 ','가 한 번 이상 반복되는 문자열을 전역 검색합니다.
let regExp = /[\d,]+/g;

console.log(target.match(regExp));
// [ '12,345' ]

// '0' ~ '9'가 아닌 문자(숫자가 아닌 문자) 또는 ','가 한 번 이상
// 반복되는 문자열을 전역 검색합니다.
regExp = /[\D,]+/g;

console.log(target.match(regExp));
//[ 'AA BB Aa Bb ', ',' ]
```

\w는 알파벳, 숫자, 언더스코어를 의미한다. 즉 \w는 [A-Za-z0-9]와 같다. \W는 \w와 반대로 동작한다 즉, \W는 알파벳, 숫자, 언더스코어가 아닌 문자를 의미한다.

```javascript
const target = "Aa Bb 12,345 _$%&";

// 알파벳, 숫자, 언더스코어, ','가 한 번 이상 반복되는 문자열을 전역 검색합니다.
let regExp = /[\w,]+/g;

console.log(target.match(regExp));
// [ 'Aa', 'Bb', '12,345', '_' ]

// 알파벳, 숫자, 언더스코어가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색합니다.
regExp = /[\W,]+/g;
console.log(target.match(regExp));
// [ ' ', ' ', ',', ' ', '$%&' ]
```

### 5.5 NOT 검색

[ … ] 내의 ^은 not의 의미를 갖는다. 예를 들어. [^0-9]는 숫자를 제외한 문자를 의미한다. 따라서 [0-9]와 같은 의미와 \d와 반대로 동작하는 \D는 [^0-9]와 같고, [A-Za-z0-9_]와
같은 의미의 \w와 반대로 동작하는 \W는 [^A-Za-z0-9_]와 같다.

```javascript
const target = "AA BB 12 Aa Bb";

// 숫자를 제외한 문자열을 전역 검색합니다.
const regExp = /[^0-9]+/g;

console.log(target.match(regExp));
// [ 'AA BB ', ' Aa Bb' ]
```

### 5.6 시작 위치로 검색

[ … ] 밖의 ^은 문자열의 시작을 의미합니다. 단, [ … ]안에 ^은 not을 의미하므로 주의해야한다,

```javascript
const target = "https://poiemaweb.com";

// 'https'로 시작하는지 검사한다.
const regExp = /^https/;

console.log(regExp.test(target)); // true
```

### 5.7 마지막 위치로 검색

$는 문자열의 마지막을 의미합니다.

```javascript
const target = "https://poiemaweb.com";

// 'com'로 끝나는지 검사한다.
const regExp = /com$/;

console.log(regExp.test(target)); // true
```

## 6 자주 사요하는 정규표현식

### 6.1 특정 단어로 시작하는지 검사

다음 예제는 검색 대상 문자열이 ‘http://’ 또는 ‘https://’로 시작하는지 검사한다.

[…] 바깥의 ^은 문자열의 시작을 의미하고, ?은 앞선 패턴(다음 예제의 경우 ’s’)이 최대 한 번(0번 포함) 이상 반복되는지를 의미한다. 다시 말해, 검색 대상 문자열에 앞선 패턴 (’s’)이 있어도 없어도
매치된다.

```javascript
const target = "https://poiemaweb.com";

// 'http://' 또는 'https://'로 시작하는지 검사한다.
console.log(/^https?:\/\//.test(target)); // true
```

다음 방법도 동일하게 동작한다,

```javascript
/^(http|https):\/\//.test(target); // => true
```

### 6.2 특정 단어로 끝나는지 검사

다음 예제는 검색 대상 문자열이 ‘html’로 끝나는지 검사한다. ‘$’는 문자열의 마지막을 의미한다.

```javascript
const fileName = 'index.html';

// 'html'로 끝나는지 검사한다.
/html$/.test(fileName); // => true
```

### 6.3 숫자로만 이루어진 문자열인지 검사

[ … ] 바깥의 ^은 문자열의 시작을, $는 문자열의 마지막을 의미한다. \d는 숫자를 의미하고 +는 앞선 패턴이 최소 한 번 이상 반복되는 문자열을 의미한다. 즉, 처음과 끝이 숫자이고 최소 한 번 이상 반복되는
문자열과 매치한다.

```javascript
const target = '12345';

// 숫자로만 이루어진 문자열인지 검사한다.
/^\d+$/.test(target); // => true
```

### 6.4 하나 이상의 공백으로 시작하는지 검사

다음 예제는 검색 대상 문자열이 하나 이상의 공백으로 시작하는지 검사한다.

\s는 여러 가지 공백 문자(스페이스, 탭 등)를 의미한다. 즉,\s는 [\t\r\n\v\f]와 같은 의미다.

```javascript
const target = ' Hi!';

// 하나 이상의 공백으로 시작하는지 검사한다.
console.log(/^[\s]+/.test(target)); // => true
```

### 6.5 아이디로 사용 가능한지 검사

다음 예제는 검색 대상 문자열이 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4 ~ 10자리인지 검사한다. {4, 10}은 앞선 패턴 (알파벳 대소문자 또눈 숫자)이 최소 4번, 최대 10번 반복되는 문자열을
의미합니다.

```javascript
const id = 'abc123';

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10자리인지 검사한다.
/^[A-Za-z0-9]{4,10}$/.test(id); // => true
```

### 6.6 메일 주소 형식에 맞는지 검사

```javascript
const email = 'ungmo2@gmail.com';

console.log(/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email);
// true
```

**정밀한 이메일 주소 체크**

```javascript
/(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))/
```

### 6.7 핸드폰 번호 형식에 맞는지 검사

```javascript
const cellphone = '010-1234-5678';

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // true
```

### 6.8 특수 문자 포함 여부 검사

```javascript
const target = 'abc#123';

(/[^A-Za-z0-9]/gi).test(target); // => true
```

아래 코드로 대체해 사용할 수도 있다.

```javascript
(/[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi).test(target); // => true
```

특수 문자를 제거할 때는 String.protype.replace 메소드를 사용한다.

```javascript
// 특수 문자를 제거한다.
target.replace(/[^A-Za-z0-9]/gi, ''); // -> abc123
```