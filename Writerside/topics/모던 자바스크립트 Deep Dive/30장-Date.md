# 30장 Date

표준 빌트인 객체인 Date는 날짜와 시간(연, 월, 일, 시, 분, 초, 밀리초 (millisecond/ms. 천분의 1초)을 위한 메소드를 제공하는 빌트인 객체이면서 생성자 함수이다.

현재 날짜와 시간은 자바스크립트 코드가 실행된 시스템의 시계에 의해 결정된다.

## 1 Date 생성자 함수

Date는 생성자 함수이다. Date 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖는다.

### 1.1 new Date( )

Date 생성자 함수를 인수없이 new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환한다.

```javascript
let date = new Date();
console.log(date); // 2023-03-09T18:15:19.067Z
```

new 연산자 없이 호출하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 문자열을 반환한다.

```javascript
let date = Date();
console.log(date); // Fri Mar 10 2023 03:16:07 GMT+0900 (대한민국 표준시)
```

### 1.2 new Date(milliseconds)

Date 생성자 함수에 숫자 타입의 밀리초를 인수로 전달하면 1월 1일 00:00:00(UTC)을 기점으로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.

```javascript
// 한국 표준시는 KST 협정 세계시 UCT에 9시간을 더한 시간이다.
let date = new Date(0); //
console.log(date); // 1970-01-01T00:00:00.000Z

/*
86400000ms는 1day를 의미한다.
1s = 1,000ms
1m = 60s * 1,000ms = 60,000ms
1h = 60m * 60,000ms = 3,600,000ms
1d = 24h * 3,600,000ms = 86,400,000ms
*/

date = new Date(86400000);
console.log(date); // 1970-01-2T00:00:00.000Z
```

### 1.3 new Date(dateString)

Date 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수로 전달하면 Date 객체를 반환한다. 단, 인수로 전달한 문자열은 Date.parse () 메소드에 의해 해석 가능한 형식이어야 한다.

```javascript
let date = new Date("May 26, 2020 10:00:00");
console.log(date); // 2020-05-26T01:00:00.000Z

date = new Date("2020/03/26 10:00:00");
console.log(date); // 2020-03-26T01:00:00.000Z
```

### 1.4 new Date(year, month[,day, hour, minute, second, millisecond])

Date 생성자 함수에 연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date객체를 반환한다.

| 인수          | 내용                                               |
|-------------|--------------------------------------------------|
| year        | 연을 나타내는 1900년 이후의 정수. 0부터 99는 1900부터 1999로 처리된다. |
| month       | 월을 나타내는 0 ~ 11까지의 정수 (주의: 0부터 시작, 0 = 1월)        |
| day         | 일을 나타내는 1 ~ 31까지의 정수                             |
| hour        | 시를 나타내는 0 ~ 23까지의 정수                             |
| minute      | 분을 나타내는 0 ~ 59까지의 정수                             |
| second      | 초를 나타내는 0 ~ 59까지의 정수                             |
| millisecond | 밀리초를 나타내는 0 ~ 999까지의 정수                          |

연, 월을 지정하지 않은 경우 1970년 1월 1일 00:00:00(UTC)을 나타내는 Date 객체를 반환한다.

```javascript
// 월을 나타내는 2는 3월을 의미한다.
date = new Date(2020, 2); 
console.log(date); // 2020-02-29T15:00:00.000Z

// 월을 나타내는 2는 3월을 의미한다.
date = new Date(2020, 2, 26, 10, 00, 00, 0);
console.log(date);// 2020-03-26T01:00:00.000Z

// 다음처럼 표현하면 가독성이 훨씬 좋다
date = new Date("2020/3/26/10:00:00:00");
console.log(date); // 2020-03-26T01:00:00.000Z
```

## 2 Date 메소드

### 2.1 Date.now

1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.

```javascript
const now = Date.now();
console.log(now); // 1678386768145

// Date 생성자 함수에 숫자 타입의 밀리초를 인수로 전달하면 1970년 1월 1일 00:00:(UTC)을
// 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.
// (1.2절 "new Date(milliseconds)"참고)
var now2 = new Date(now); //
console.log(now2); // 2023-03-09T18:34:28.875Z
```

### 2.2 Date.parse

1970년 1월 1윌 00:00:00(UTC)를 기점으로 인수로 전달된 지정 시간(new Date(dateString)의 인수와 동일한 형식)까지의 밀리초를 숫자로 반환한다.

```javascript
// UTC
console.log(Date.parse("Jan 2, 1970 00:00:00 UTC"));
// 86400000

// KST
console.log(Date.parse("Jan 2, 1970 09:00:00"));
// 86400000

// KST
console.log(Date.parse("1970/01/02/09:00:00"));
// 86400000
```

### 2.3 Date.UTC

1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.

Date.UTC 메소드는 new Date(year, month[,day, hour, minute, second, millisecond])와 같은 형식의 인수를 사용해야 한다. 또 month는 월의 의미를 하는
0 ~ 11까지의 정수이다. 0부터 시작하므로 주의해야한다.

```javascript
console.log(Date.UTC(1970, 0, 2)); // 86400000
console.log(Date.UTC("1970/0/2")); // NaN
```

### 2.4 Date.prototype.getFullYear

Date 객체에 연도를 나타내는 정수를 반환한다.

```javascript
new Date('2020/07/24').getFullYear(); // => 2020
```

### 2.5 Date.prototype.setFullYear

Date 객체에 연도를 나타내는 정수를 설정한다. 연도 이외에 옵션으로 월, 일도 설정할 수 있다.

```javascript
const today = new Date();

// 년도 지정
today.setFullYear(2000);
today.getFullYear()); // => 2000

// 년도/월/일 지정
today.setFullYear(1900, 0, 1);
today.getFullYear()); // => 1900
```

### 2.6 Date.prototype.getMonth

Date 객체의 월을 나타내는 0 ~ 11의 정수를 반환한다. 1월은 0, 12월은 11이다.

```javascript
new Date('2020/07/24').getMonth(); // => 6
```

### 2.7 Date.prototype.setMonth

Date 객체의 월을 나타내는 0 ~ 11의 정수를 설정한다. 월 이외의 옵션으로 일도 설정할 수 있다.

```javascript
const today = new Date();

// 월 지정
today.setMonth(0); // 1월
today.getMonth(); // => 0

// 월, 일 지정
today.setMonth(11, 1); // 12월 1일
today.getMonth(); // 11
```

### 2.8 Date.prototype.getDate

Date 객체의 날짜 (1~31)를 나타내는 정수를 반환한다.

```javascript
new Date('2020/07/24').getDate(); // => 24
```

### 2.9 Date.prototype.setDate

Date 객체에 날짜 (1~31)를 나타내는 정수를 설정한다.

```javascript
const today = new Date();

// 날짜 지정
today.setDate(1);
today.getDate(); // => 1
```

### 2.10 Date.prototype.getDay

Date 객체의 요일(0 ~ 6)을 나타내는 정수를 반환한다. 반환값은 다음과 같다.

| 요일  | 일요일 | 월요일 | 화요일 | 수요일 | 목요일 | 금요일 | 토요일 |
|-----|-----|-----|-----|-----|-----|-----|-----|
| 반환값 | 0   | 1   | 2   | 3   | 4   | 5   | 6   |

```javascript
new Date('2020/07/24').getDay(); // 5
```

### 2.11 Date.prototype.getHours

Date 객체의 시간(0 ~ 23)을 나타내는 정수를 반환한다.

```javascript
new Date('2020/07/24/12:00').getHours(); // 12
```

### 2.12 Date.prototype.setHours

Date 객체에 시간(0 ~ 23)을 나타내는 정수를 설정한다. 시간 이외에 옵션으로 분, 초, 밀리초도 설정할 수 있다.

```javascript
const today = new Date();

// 시간 지정
today.setHours(7);
today.getHours(); // 7

// 시간/분/초/밀리초 지정
today.setHours(0, 0, 0, 0); // 00:00:00:00
today.getHours(); // 0
```

### 2.13 Date.prototype.getMinutes

Date 객체의 분(0 ~ 59)을 나타내는 정수를 반환한다.

```javascript
new Date('2020/07/24/12:30').getMinutes(); // 30
```

### 2.14 Date.prototype.setMinutes

Date 객체의 분(0 ~ 59)을 나타내는 정수를 설정한다. 분 이외에 옵션으로 초, 밀리초도 설정할 수 있다.

```javascript
const today = new Date();

// 분 지정
today.setMinutes(50);
console.log(today.getMinutes()); // 50;

// 분/초/밀리초 지정
today.setMinutes(5, 10, 999);
console.log(today.getMinutes()); // 5;
```

### 2.15 Date.prototype.getSeconds

Date 객체의 초(0~59)를 나타내는 정수를 반환한다.

```javascript
new Date('2020/07/24/12:30:10').getSeconds(); // 10;
```

### 2.16 Date.prototype.setSeconds

Date 객체의 초(0~59)를 나타내는 정수를 반환한다. 초 이외에 밀리초도 설정할 수 있다.

```javascript
const today = new Date();

// 초 지정
today.setSeconds(50);
console.log(today.getSeconds()); // 50;

// 초/밀리초 지정
today.setSeconds(10, 999);
console.log(today.getSeconds()); // 10;
```

### 2.17 Date.prototype.getMilliseconds

Date 객체의 밀리초(0 ~ 999)를 나타내는 정수를 반환한다.

```javascript
new Date('2020/07/24/12:30:10:150').getMilliseconds(); // 150;
```

### 2.18 Date.prototype.setMilliseconds

Date 객체의 밀리초(0 ~ 999)를 나타내는 정수를 설정한다.

```javascript
const today = new Date();

// 밀리초 지정
today.setMilliseconds(50);
console.log(today.getMilliseconds()); // 50;
```

### 2.19 Date.prototype.getTime

1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환한다.

```javascript
new Date('2020/07/24/12:30').getTime(); // 1595561400000
```

### 2.20 Date.prototype.setTime

Date 객체에 1970년 1월 1일 00:00:00(UTC)를 기점으로 경과된 밀리초를 설정한다.

```javascript
const today = new Date();

// 1970년 1월 1일 00:00:00(UTC)를 기점으로 경과된 밀리초 설정
today.setTime(86400000); // 86400000은 1일을 나타낸다.
console.log(today); // 1970-01-02T00:00:00.000Z
```

### 2.21 Date.prototype.getTimezoneOffset

UTC와 Date 객체에 지정된 로캘(locale) 시간과의 차이를 분 단위로 반환한다. KST는 UTC에 9시간을 더한 시간이다. 즉, UTC = KST - 9h다.

```javascript
const today = new Date(); // today의 지정 로캘은 KST다.

// UTC와 today의 지정 로캘 KST와의 차이는 -9시간이다.
today.getTimezoneOffset() / 60; // -9
```

### 2.22 Date.prototype.toDateString

사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜를 반환한다.

```javascript
const today = new Date('2020/7/24/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toDateString(); // Fri Jul 24 2020
```

### 2.23 Date.prototype.toTimeString

사람이 읽을수 있는 형식의 Date 객체의 시간을 표현한 문자열을 반환한다.

```javascript
const today = new Date("2020/7/24/12:30");

console.log(today.toString()); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
console.log(today.toTimeString()); // 12:30:00 GMT+0900 (대한민국 표준시)
```

### 2.24 Date.prototype.toISOString

ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.

```javascript
const today = new Date("2020/7/24/12:30");

console.log(today.toString()); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
console.log(today.toISOString()); // 2020-07-24T03:30:00.000Z

console.log(today.toISOString().slice(0, 10)); // 2020-07-24
console.log(today.toISOString().slice(0, 10).replace(/-/g, "")); // 20200724
```

### 2.25 Date.prototype.toLocaleString

인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다. 인수를 생략한 경우 브라우저가 동작중인 시스템의 로캘을 적용한다.

```javascript
const today = new Date("2020/7/24/12:30");

console.log(today.toString()); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
console.log(today.toLocaleString()); // 2020. 7. 24. 오후 12:30:00
console.log(today.toLocaleString("ko-KR")); // 2020. 7. 24. 오후 12:30:00
console.log(today.toLocaleString("en-US")); // 7/24/2020, 12:30:00 PM
console.log(today.toLocaleString("ja-JP")); // 2020/7/24 12:30:00
```

### 2.26 Date.prototype.toLocaleTimeString

인수로 전달한 로캘을 기준으로 Date 객체의 시간을 표현한 문자열을 반환한다. 인수를 생략한 경우 브라우저가 동작중인 시스템의 로캘을 적용한다.

```javascript
const today = new Date("2020/7/24/12:30");

console.log(today.toString()); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
console.log(today.toLocaleTimeString()); // 오후 12:30:00
console.log(today.toLocaleTimeString("ko-KR")); // 오후 12:30:00
console.log(today.toLocaleTimeString("en-US")); // 12:30:00 PM
console.log(today.toLocaleTimeString("ja-JP")); // 12:30:00
```

## 3 Date를 활용한 시계 예제

```javascript
(function printNow() {
  const today = new Date();

  const dayNames = [
    "(일요일)",
    "(월요일)",
    "(화요일)",
    "(수요일)",
    "(목요일)",
    "(금요일)",
    "(토요일)",
  ];

  // getDay 메소드는 해당 요일 (0 ~ 6)을 나타내는 정수를 반환한다.
  const day = dayNames[today.getDay()];

  const year = today.getFullYear();
  const month = today.getMonth() + 1;
  const date = today.getDate();
  let hour = today.getHours();
  let minute = today.getMinutes();
  let second = today.getSeconds();
  const ampm = hour >= 12 ? "PM" : "AM";

  // 12시간제로 변경
  hour %= 12;
  hour = hour || 12; // hour가 0이면 12를 재할당

  // 10 미만인 분과 초를 2자리로 변경
  minute = minute < 10 ? "0" + minute : minute;
  second = second < 10 ? "0" + second : second;

  const now = `${year}년 ${month}월 ${date}일 ${day} ${ampm} ${hour}:${minute}:${second} `;

  console.log(now);

  // 1초마다 printNow 함수를 재귀 호출한다.
  setTimeout(printNow, 1000);
})();
```