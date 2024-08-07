# 리액트 ES6 문법 액기스

## 템플릿 문자열

템플릿 문자열은 작은 따옴표(' ') 대신 백틱(`)으로 문자열을 표현합니다. 또한 템플릿 문자열에 특수 기호 $를 사용하여 변수 또는 식을 포함할 수도 있습니다.

```Javascript
var string1 = "안녕하세요";
var string2 = "반갑습니다";
var greeting = `${string1} ${string2}`;
var product = { name: '도서', price: '4200원' };
var message = `제품 ${product.name}의 가격은 ${product.price}`;
var multiLine = `문자열1
문자열2`;
var value1 = 1;
var value2 = 2;
var boolValue = false;
var operator1 = `곱셈값은 ${value1 * value2}입니다.`;
var operator2 = `${boolValue ? '참' : '거짓'} 입니다.`;
```

1. 템플릿 문자열에 $기호를 사용하여 변수를 포함했습니다.
2. 템플릿 문자열에서 Enter를 눌러 줄을 바꾸는 것도 허용되며, 이스케이프 시퀀스를 사용하지 않아도 됩니다.
3. 템플릿 문자열에 $ 기호를 사용하여 연산을 포함시킬 수도 있습니다.

## 전개 연산자

전개 연산자(spread operator)는 독특하면서 매우 유용한 문법입니다. 전개 연산자는 나열형 자료를 추출하거나 연결할 때 사용합니다. 사용 방법은 배열이나 객체, 변수명 앞에 마침표 세 개(…)를
입력합니다. 다만 배열, 객체, 함수 표현식([], {}, ()) 안에서만 사용해야 합니다.

### ES6 배열 전개 연산자 사용 방법 알아보기

기존의 배열에서는 잘라내거나 연결하려면 배열 인덱스와 함께 배열 내장 함수들을 사용해야 했습니다. 이는 배열 내장 함수를 일일이 기억해야 하고 코드가 길어서 불편합니다.

```Javascript
var array1 = ['one', 'two'];
var array2 = ['three', 'four'];
var combined = [array1[0], array1[1], array2[0], array2[1]]; // (1)
var combined = array1.concat(array2); // (2)
var combined = [].concat(array1, array2); // (3)
var first = array1[0];
var second = array1[1]; // (4)
var three = array1[2] || 'empty'; // (5)

function func() {
    var args = Array.prototype.slice.call(arguments); // (6)
    var first = args[0];
    var others = args.slice(1, args.length); // (7)
}
```

1. 배열의 각 요소를 추출하여 새로운 배열을 만들었습니다.
2. 2, 3 concat()함수로 두 배열을 합쳤습니다.
3. 인덱스로 배열 요소를 추출했습니다.
4. || 연산자와 조합하면 추출할 배열 요소가 없을 때 기본값을 지정할 수 있습니다.
5. 특수 변수 (arguments)를 사용해 함수 내 인자 항목들을 배열로 변환하였습니다.
6. 인덱스 범위에 해당하는 배열 요소를 추출합니다. 여기서 args.length가 전체 배열의 길이이므로 인덱스 범위가 두 번째 범위부터 마지막 범위까지 추출됩니다.

전개 연산자를 사용한 방법입니다.

```Javascript
var array1 = ['one', 'two'];
var array2 = ['three', 'four'];
const combined = [...array1, ...array2]; // (1)
const [first, second, three = 'empty', ...others] = array1; // (2)
function func(...args) {
    var [first, ...others] = args; // (3)
}
```

1. 두 배열 항목을 전개 연산자로 합쳤습니다.
2. 배열 요소를 추출하고 기본값과 함께 배열 요소를 추출했습니다.
3. 함수 인자 배열을 args 변수에 할당했습니다.

### ES2015의 객체 전개 연산자 사용 방법 알아보기

객체도 배열과 마찬가지로 전개 연산자를 사용할 수 있습니다. 이전에는 객체의 키나 값을 추출할 때 객체 내장 함수를 사용했습니다.

```Javascript
const objectOne = { one: 1, two: 2, other: 0 };
const objectTwo = { three: 3, four: 4, other: -1 };

var combined = {
  one: objectOne.one, // (1)
  two: objectOne.two,
  three: objectTwo.three,
  four: objectTwo.four,
};

var combined = Object.assign({}, objectOne, objectTwo);
// combined = { one: 1, two: 2, other: -1, three: 3, four: 4 } // (2)
var combined = Object.assign({}, objectTwo, objectOne);
// combined = { three: 3, four: 4, other: 0, one: 1, two: 2 } // (3)

var other = Object.assign({}, combined);
delete other.other;
// other = { three: 3, four: 4, one: 1, two: 2 } // (4)
```

1. 키에 해당하는 값을 추출했습니다.
2. 객체 내장 함수 assign()을 사용하여 두 객체를 병합했습니다. assign()의 첫 번째 인자는 결과로 반환할 객체이며({}), 나머지 인자는 병합할 객체입니다. 이때 병합할 개체는 앞에 있는 객체부터
   덮어씁니다.
3. 중복되는 키값이 있으면 이후에 전달된 객체의 값으로 덮어씁니다.
4. 삭제 연산자(delete)를 사용하여 특정 데이터를 추출한 다음 새로운 객체로 만들었습니다.

이제 ES6의 전개 연산자를 사용하여 위의 코드를 간결하게 바꾸겠습니다.

```Javascript
const objectOne = { one: 1, two: 2, other: 0 };
const objectTwo = { three: 3, four: 4, other: -1 };

var combined = {
  ...objectOne,
  ...objectTwo,
};
// combined = { one: 1, two: 2, other: -1, three: 3, four: 4 }

var combined = {
  ...objectTwo,
  ...objectOne,
};
// combined = { three: 3, four: 4, other: 0, one: 1, two: 2 }

var { other, ...others } = combined;
// others = { three: 3, four: 4, one: 1, two: 2 }

// combined에서 other만 키 이름으로 값을 빼오고
// 나머지는 전개 연산자를 통해 others에 할당했습니다.
```

1. 객체를 병합할 때 중복된 키값들은 마지막에 사용한 객체의 값으로 덮어씁니다.
2. 객체에서 특정 값을 추출할 때는 추출하려는 키 이름을 맞추고 나머지는 전개 연산자로 선언된 변수에 할당할 수 있습니다.

## 가변 변수와 불변 변수

### 가변 변수 사용 방법 알아보기

가변 변수는 let 키워드로 선언합니다. let으로 선언한 변수는 읽거나 수정이 가능합니다. let 키워드로 선언할 경우 변수가 수정이 가능한지 코드만 읽어도 판별이 되어 유용합니다.

### 불변 변수 사용 방법 알아보기

불변 변수는 const 키워드로 선언합니다. const로 선언한 변수는 읽기만 가능합니다. 그런데 불변 변수는 값을 다시 할당할 수 없는 것이지 값을 변경할 수는 있습니다.

```Javascript
const arr2 = [];
arr2.push(1); // arr2 = [1]
arr2.splice(0, 0, 0); // arr2 = [0, 1]
arr2.pop(); // arr2 = [0]
const obj2 = {};
obj2['name'] = '내이름'; // obj2 = { name: '내이름' }
Object.assign(obj2, { name: '새이름' }); // obj2 = { name: '새이름' }
delete obj2.name; // obj2 = {}
```

const는 불변 변수로서 값을 수정할 수 없다고 했는데 위에 보시는 바와 같이 값이 계속해서 바뀌는 것을 볼 수 있습니다. 이런 것을 ‘무결성 제약 조건에 위배되었다’라고 합니다. 다만 무결성을 유지하면서 불변
변수의 값을 수정해야 하는 경우가 종종 있습니다. 그럴 땐 수정할 불변 변수를 새로 만들어 새 값을 할당하는 방법으로 수정을 하면 수정한다기보다는 새로 정의한다는 개념에 가깝지만, 값을 수정하면서 무결성 제약
조건까지 지킬 수 있습니다.

```Javascript
const num1 = 1;
const num2 = num1 * 3; // num2 = 3
const str1 = "문자";
const str2 = str1 + "추가"; // str2 = '문자 추가'
const arr3 = [];
const arr4 = arr3.concat(1); // arr4 = [1]
const arr5 = [...arr4, 2, 3]; // arr5 = [1, 2, 3]
const arr6 = arr5.slice(0, 1); // arr6 = [1], arr5 = [1, 2, 3]
const [first, ...arr7] = arr5; // arr7 = [2, 3], first = 1
const obj3 = { name: "내이름", age: 20 };
const objValue = { name: "새이름", age: obj3.age };
const obj4 = { ...obj3, name: "새이름" }; 
// obj4 = { name: '새이름', age: 20 }
const { name, ...obj5 } = obj4; // obj5 = { age: 20 }
```

배열에 새로운 값을 추가할 때는 push() 함수 대신 concat() 함수를, 삭제할 때는 pop(), shift() 함수 대신 slice(), concat() 함수에 전개 연산자를 조합하여 사용한 점을 유심히
살펴보세요. 이렇게 하면 새 값을 할당하는 것이 아니라 기존의 값을 이용(추출)하여 새 불변 변수에 할당하는 것이므로 괜찮습니다.

다음은 기존 자바스크립트의 가변 내장 함수와 무결성 내장 함수를 병렬로 나열한 표입니다. 무결성 함수는 객체나 배열을 직접 수정하는 것이 아니라 새 결과를 반환하는 (무결성 제약 조건을 지키는) 함수를 말합니다.

| 가변 내장 함수	             | 무결성 내장 함수                                     |
|-----------------------|-----------------------------------------------|
| push(…items)          | 	concat(…items)                               |
| splice(s, c, …items)	 | slice(0, s).concat(…items).concat(slice(s+c)) |
| pop()                 | 	slice(0, len-1)                              |
| shift()               | 	slice(1)                                     |

불변 변수를 사용하면 무결성 제약 규칙에 의해 변수가 변하는 시점을 쉽게 파악할 수 있고, 수정 전과 후의 변수값을 비교할 수 있어 가변 변수보다 더 유용합니다.

## 클래스

기존 자바스크립트 문법은 클래스 표현식이 없어서 prototype으로 클래스를 표현하였습니다. 하지만 ES6부터는 클래스를 정의하여 사용할 수 있습니다.

### 기존 자바스크립트의 클래스 표현 방법 알아보기

기존 자바스크립트에서는 함수를 생성자(constructor) 형태로 선언한 다음 상속이 필요한 변수나 함수를 prototype 객체에 할당하는 방법을 사용했습니다. prototype 객체는 new 연산자로 생성되는
객체 안에서 this 연산자의 함수 및 변수 선언 위치를 참조하는 요소인데 이 특징을 이용한 것입니다.

```Javascript
function Shape(x, y) {
  this.name = "Shape";
  this.move(x, y);
}

// Static 함수를 선언하는 예제
Shape.create = function (x, y) {
  return new Shape(x, y);
};
// 인스턴스 함수를 선언하는 예제

Shape.prototype.move = function (x, y) {
  this.x = x;
  this.y = y;
};

Shape.prototype.area = function () {
  return 0;
};

// 혹은
Shape.prototype = {
  move: function (x, y) {
    this.x = x;
    this.y = y;
  },
  area: function () {
    return 0;
  },
};

var s = new Shape(0, 0);
s.area();
```

위와 같이 마지막에 new Shape(0, 0)와 같이 함수를 호출하면 this 객체에 해당하는 변수 및 함수가 prototype 객체에 선언된 변수와 함수를 함께 참조합니다. 예를 들어 Shape() 함수에는
this.move를 정의하지 않았지만 prototype 객체에 move() 함수가 정의되어 있으므로 new 연산자로 Shape() 함수를 호출하여 Shape 객체 s를 만듭니다. 그러면 s는 prototype 안에
있는 move() 함수를 참조할 수 있습니다. 또한 s는 prototype 내에 추가로 정의된 area 함수도 참조할 수 있습니다.

클래스의 상속은 prototype 객체의 값을 객체 내장 함수를 사용해 덮어씌우는 방식을 이용하면 됩니다.

```Javascript
function Circle(x, y, radius) {
  Shape.call(this, x, y);
  this.name = "Circle";
  this.radius = radius;
}

Object.assign(Circle.prototype, Shape.prototype, {
  area: function () {
    return this.radius * this.radius;
  },
});
var c = new Circle(0, 0, 10);
console.log(c.area()); // 100
```

위 코드는 Circle() 함수를 만드는 방법으로 자식 클래스를 생성한 것을 보여줍니다. 자식 클래스 Circle은 내장 함수 call()을 통해 부모의 생성자를 호출하여 초깃값을 상속받습니다.

또 부모 클래스 함수를 상속하는 방법으로는 Object에 내장된 assign() 함수를 이용했습니다. 또한 assign() 함수에 전달한 area() 함수는 Shape.prototype에 정의된 area() 함수를
덮어씁니다. 그래서 호출하면 부모 클래스에서 상속받은 area()가 수정이 되어 결과값인 100이 출력됩니다.

### 클래스 사용 방법 알아보기

ES6에서부터 class 키워드로 클래스를 정의하므로 코드가 간결해집니다.

앞의 코드를 ES6 클래스 표현식으로 변환한 코드입니다.

```Javascript
class Shape {
  static create(x, y) {
    return new Shape(x, y);
  }
  name = "Shape";
  constructor(x, y) {
    this.move(x, y);
  }
  move(x, y) {
    this.x = x;
    this.y = y;
  }
  area() {
    return 0;
  }
}

var s = new Shape(0, 0);
s.area();
```

위 코드는 class 키워드로 Shape 클래스를 정의하였으며, Shape 클래스 안에 생성자 함수도 추가되었습니다. 클래스 정의 표현식에는 객체가 생성될 때 함께 만들어질 변수나 클래스 변수(static)를 클래스
선언 블록 안에 같이 정의할 수 있게 변경되었습니다. 생성자, 클래스 변수, 클래스 함수 정의에는 변수 선언을 위한 키워드 (var, let, const, …)를 사용하지 않는다는 점도 주목해주세요.

앞에서 구현한 상속은 아래와 같이 간결하게 작성할 수 있습니다.

```Javascript
class Shape {
  static create(x, y) {
    return new Shape(x, y);
  }
  name = "Shape";
  constructor(x, y) {
    this.move(x, y);
  }
  move(x, y) {
    this.x = x;
    this.y = y;
  }
  area() {
    return 0;
  }
}

var s = new Shape(0, 0);
s.area(); //

class Circle extends Shape {
  constructor(x, y, radius) {
    super(x, y);
    this.radius = radius;
  }
  area() {
    if (this.radius === 0) return super.area();
    return this.radius * this.radius;
  }
}

var c = new Circle(0, 0, 10);
c.area(); // 100
```

코드를 본다면 Circle 클래스는 extends 키워드를 사용하여 Shape 클래스를 상속합니다. 그리고 부모의 함수를 참조할 경우 super(상위 클래스를 호출)를 사용합니다.

## 화살표 함수

화살표 함수(arrow function)은 ES6에 추가된 표현식을 사용하는 함수로 화살표 기호 =>로 함수를 선언합니다.

### 기존의 자바스크립트의 함수 사용 방법 알아보기

```Javascript
function add(first, second) {
  return first + second;
}

// typeof add === 'function'

var add = function (first, second) {
  return first + second;
};

// typeof add === 'function'
```

첫 번째 함수는 typeof(타입을 추출하는 메소드)로 add와 'function'이라는 문자열을 비교해보면 true가 나올 겁니다. 하지만 첫 번째 방식은 이름이 있고 두 번째 방식은 이름이 없습니다.

### 화살표 함수 사용 방법 알아보기

화살표 함수는 익명 함수를 선언하여 변수에 대입하는 방법과 유사합니다. 하지만 다른 점은 function 키워드를 생략하고 인자 블록 ()과 본문 블록 {} 사이에 => 를 표기한다는 것입니다.

```Javascript
var add = (first, second) => {
  return first + second;
}; // (1)

var add = (first, second) => first + second; // (2)

var addAndMultiply = (first, second) => 
({ add: first + second, multiply: first * second }); // (3)
```

1. 앞에서 본 기존의 함수 표현식을 화살표 함수로 변경했습니다.
2. 본문 블록이 비어 있고 결괏값을 바로 반환하는 경우 중괄호를 생략하고 => 뒤에 반환 표현식을 넣을 수 있습니다.
3. 반환값이 객체라면 괄호로 결괏값을 감싸 간결하게 표현할 수 있습니다.

화살표 함수는 함수 표현식을 간결히 할 수 있습니다.

```Javascript
function addNumber(num) { // (1)
  return function (value) {
    return num + value;
  };
}

var addNumber = (num) => (value) => num + value; // (2)
```

1. 함수 표현식을 반환합니다.
2. 화살표 함수를 사용하여 간결한 코드로 함수를 반환할 수 있습니다.

또한 화살표 함수는 콜백 함수의 this 범위로 생기는 오류를 피하기 위해 bind() 함수를 사용하여 this 객체를 전달하는 과정을 포함하고 있습니다.

```Javascript
class MyClass {
  value = 10;
  constructor() {
    var addThis2 = function (first, second) {
      return this.value = first + second;
    }.bind(this);
    var addThis3 = (first, second) => this.value + first + second;
  }
}
```

코드를 보면 addThis2() 함수는 this를 bind() 함수에 전달하여 this의 범위를 유지하고 있습니다. addThis3() 함수의 경우 화살표 함수이므로 이 과정이 생략되어 있습니다.

## 객체 확장 표현식과 구조 분해 할당

### 기존 자바스크립트의 객체 확장 표현식 사용 방법 알아보기

기존 자바스크립트에서는 개체와 객체의 값을 선언하기 위해 키 이름과 값을 각각 할당해야 했습니다.

```Javascript
var x = 0;
var y = 0;
var obj = { x: x, y: y }; // (1)
var randomKeyString = "other";
var combined = {};
combined["one" + randomKeyString] = "some value"; // (2)

var obj = { // (3)
  x: x,
  methodA: function () {
    console.log("method A");
  },
  methodB: function () {
    return 0;
  },
};
```

1. obj에 대입한 객체를 보면 키 이름이 키값과 동일(각각 x, y)합니다.
2. 연산 결과로 키값을 대입할 때는 키 값을 지정할 코드를 추가로 작성해야 합니다.
3. 객체에 함수를 추가할 때는 변수에 함수를 할당해야 합니다.

### 객체 확장 표현식 사용 방법 알아보기

```Javascript
var x = 0;
var y = 0;
var obj = { x, y }; // (1)
var randomKeyString = "other";
var combined = {
  ["one" + randomKeyString]: "some value", 
  // (2) { oneother: 'some value' }
};
var obj2 = {
  x,
  methodA() {
    console.log("method A"); // (3)
  },
  methodB() {
    return 0;
  },
};
```

1. 객체의 변수를 선언할 때 키값을 생략하면 자동으로 키의 이름으로 키값을 지정합니다.
2. 객체 생성 블록 안에 대괄호 [ ]를 사용하여 표현식을 작성하면 추가로 계산된 키 값을 생성할 수 있습니다.
3. function 키워드를 생략하여 함수를 선언할 수 있습니다.

### 기존 자바스크립트의 구조 분해 사용 방법 알아보기

기존의 자바스크립트에서는 객체나 배열의 특정 자료를 다음과 같이 분해했습니다.

```Javascript
var list = [0, 1];
var item1 = list[0]; // (1)
var item2 = list[1];
var item3 = list[2] || -1; // (2)
var temp = item2; // (3)
item2 = item1;
item1 = temp;

var obj = {
  key1: "one",
  key2: "two",
};

var key1 = obj.key1; // (4)
var key2 = obj.key2;
var key3 = obj.key3 || "default key value"; // (5)
var newKey1 = obj.key1; // (6)
```

1. 배열의 인덱스를 사용하여 변수에 할당합니다.
2. || 연산자를 이용하여 배열에 해당 인덱스에 값이 없으면 기본값으로 할당합니다.
3. 배열의 두 값을 치환했습니다.
4. 객체의 키 값을 변수에 할당했습니다.
5. || 연산자를 이용하여 객체의 해당 키값이 없으면 기본값을 할당합니다.
6. 콜론 : 부호와 함께 새 변수명을 선언하여 추출된 키 값을 다른 변수명으로 할당합니다.

구조 할당은 앞에서 배운 전개 연산자를 함께 사용합니다. ES6의 구조 분해와 구조 할당은 함수 인자값을 다루거나 JSON 데이터를 변환할 때 유용하게 사용하므로 꼭 기억하는 것이 좋습니다.

```Javascript
var [item1, ...otherItems] = [0, 1, 2]; // (1)
var { key1, ...others } = { key1: 'one', key2: 'two' }; // (2)
// otherItems = [ 1, 2 ]
// others = { key2: 'two' }
```

1. 구조 할당 표현식을 이용하여 배열에서 첫 위치의 인덱스 값 item1을 추출하고 나머지 값을 전개 연산자로 otherItems에 할당합니다.
2. 객체의 key1 키값을 추출하고 나머지 키값을 포함한 새 객체를 others에 할당합니다.

## 라이브러리 의존성 관리

어떤 파일이나 코드가 다른 파일이나 코드를 필요로 하는 것을 의존성이라고 합니다. 기존 자바스크립트는 라이브러리나 모듈을 관리하는 방법이 불편했지만 ES6에서는 이런 문제를 해결했습니다.

### 기존 자바스크립트의 라이브러리 의존성 관리 방법 알아보기

기존 자바스크립트에서는 라이브러리나 모듈의 의존성을 script 엘리먼트를 이용하여 관리했습니다.

```Javascript
// lib/math.js

LibMath = {};
LibMath.sum = function (x, y) {
  return x + y;
};
LibMath.pi = 3.141593;

// app.js

var math = LibMath;
$("#pi-label").text("2π = " + math.sum(math.pi, math.pi));
```

위와 같은 코드들을 script 엘리먼트를 이용하여 관리한다고 가정해 봅시다. 만약 html에서 app.js를 먼저 참조할 경우 정의되지 않는 함수를 참조하는 의존성 문제가 발생합니다. 여기까지만 보았을 때
script 엘리먼트의 선언 순서가 매우 중요하다는 것을 알 수 있습니다.

### 라이브러리 의존성 관리 방법 알아보기

ES6는 앞에서 언급된 참조 순서에 대한 의존성 문제를 간단히 해결하였습니다. import 구문을 사용하여 script 엘리먼트 없이 연결된 파일 및 의존 파일을 모두 내려받고 코드를 구동하도록 변경한 것입니다.

```Javascript
import MyModule from './Mymodule.js'; // (1)
import { ModuleName } from './MyModule.js'; // (2)
import { ModuleName as RenameModuleName } from './Mymodule.js'; // (3)
function Func() {
  MyModule();
}

export const CONST_VALUE = 0; // (4)
export class MyClass { } // (5)
export default new Func(); // (6)
```

1. import 구문을 사용해 지정된 파일(MyModule.js)의 기본 (default)으로 공유하는 모듈을 불렀습니다.
2. 이름을 맞춰 모듈 안의 특정 함수 혹은 변수를 참조할 수도 있습니다.
3. 객체 구조 할당과 유사하게 특정 모듈을 가져올 때 이름이 충돌할 경우 다른 이름으로 변경하여 불러들일 수 있습니다.
4. 변수나 클래스를 따로 참조할 수 있도록 정의합니다.
5. 현재 파일이 다른 파일에서 기본(default)으로 참조하게 되는 항목을 정의합니다.

## 배열 함수

### forEach() 함수 사용 방법 알아보기

쿼리 스트링: 웹 주소에 포함시키는 문자열을 뜻합니다.

다음은 기존 자바스크립트로 쿼리 스트링 'banana=10&apple=20&orange=30'을 &문자를 기준으로 분리하여 객체에 반환하는 parse() 함수를 선언한 것입니다.

```Javascript
function parse(qs) {
  var queryString = qs.substr(1);
  // querystring = ‘banana=10&apple=20&orange=30’
  var chunks = qs.split('&');
  var result = {};
  for (var i = 0; i < chunks.length; i++) {
    var parts = chunks[i].split('=');
    var key = parts[0];
    var value = parts[1];
    result[key] = value; // result = { banana: '10'}
  }
  return result;
}
```

코드를 보면 for문을 이용하여 banana, apple, orange와 10, 20, 30을 분리합니다.

위 코드를 ES6의 forEach() 함수를 사용하면 반복문의 순번(i++)과 배열의 크기를 따로 변수에 저장하는 과정을 생략할 수 있습니다.

```Javascript
function parse(qs) {
  const queryString = qs.substr(1);
  // querystring = ‘banana=10&apple=20&orange=30’
  const chunks = queryString.split("&");
  // chunks = ['banana=10', 'apple=20', 'orange=30']
  let result = {};
  chunks.forEach((chunk) => {
    const [key, value] = chunk.split("=");
    // key = 'banana', value = '10'
    result[key] = value;
    // {key: 'banana', value: '10'}
  });
  return result;
}
```

### map()함수 사용 방법 알아보기

앞의 예제는 가변 변수(let)를 사용했습니다. 만약 불변 변수(const)만을 사용하려면 map()함수를 사용하면 됩니다. map() 함수는 각 배열 요소를 정의된 함수를 통해 변환한 결괏값들로 새 배열을
반환합니다. 쉽게 말해 배열을 가공하여 새 배열을 만드는 함수입니다.

```Javascript
function parse(qs) {
  const queryString = qs.substr(1);
  // querystring = ‘banana=10&apple=20&orange=30’
  const chunks = qs.split('&');
  // chunks = ['banana=10', 'apple=20', 'orange=30']
  const result = chunks.map((chunk) => {
    const [key, value] = chunk.split('=');
    // key = 'banana', value = '10'
    return { key: key, value: value }; // {key: 'banana', value: '10'}
  });
  return result;
}

// parse('banana=10&apple=20&orange=30') 실행 결과
// result = [
// { key: 'banana', value: '10'},
// { key: 'apple', value: '20'},
// { key: 'orange', value: '30'},
// ];
```

### reduce() 함수 사용 방법 알아보기

앞에서 작성한 코드로 얻은 결괏값은 객체가 아닌 배열입니다. 만약 배열을 객체로 변환하고 싶다면 reduce() 함수를 사용하면 됩니다.

```Javascript
function sum(numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}
sum([1, 2, 3, 4, 5, 6, 7, 8, 9, 10]); // 55
```

reduce() 함수를 보면 첫 번째 인자에는 변환 함수 ((total, num) => total + num), 두 번째 인자에는 초깃값(0)을 전달합니다. 그러면 reduce() 함수는 변환 함수의 이전 결괏값과
배열(numbers)의 각 요솟값(1, 2, 3…)을 순환하면서 함수를 실행합니다. 초기값으로 전달한 0은 이전 결괏값에 할당됩니다. 위 과정을 통해 배열이 숫자로 변환됩니다. 배열의 총합을 구하는 예제는 자주
사용됩니다. 그러나 실무에서는 reduce 함수는 보통 배열의 특정 자료열을 변환하는 데 사용됩니다.

```Javascript
function parse(qs) {
  const queryString = qs.substr(1);
  // querystring = ‘banana=10&apple=20&orange=30’
  const chunks = qs.split('&');
  // chunks = ['banana=10', 'apple=20', 'orange=30']
  return chunks
    .map((chunk) => {
      const [key, value] = chunk.split('=');
      // key = 'banana', value = '10'
      return { key: key, value: value }; 
      // {key: 'banana', value: '10'}
    })
    .reduce((result, item) => {
      result[item.key] = item.value;
      return result;
    }, {});
}
```

map() 함수가 반환한 배열에는 { key: ..., value: ... }의 구조로 구성된 객체들이 들어있을 것입니다. reduce() 함수는 key를 키값으로, value를 값으로 하는 하나의 객체를
반환합니다.

## 비동기 함수

비동기 함수는 비동기 처리를 위해 사용합니다. 비동기 처리란 작업 시간이 많이 필요한 A를 처리하느라 다른 작업들(B, C, D…)이 대기하고 있는 상태라면 일단 다른 작업들을 먼저 진행하고 작업 A와 연관된 작업을
이후에 처리하는 방식을 말합니다. 비동기 처리의 대표적인 예는 서버에 데이터를 요청하고 결과를 기다리는 과정입니다. 서버에 데이터를 요청하는 동안 다른 작업을 진행하다가 데이터 요청이 완료되면 본격적으로 작업을
진행합니다.

### 기존 자바스크립트 비동기 함수의 처리 방법 알아보기

기존 자바스크립트는 비동기 작업을 위해 지연 작업이 필요한 함수에 setTimeout() 함수를 이용했습니다. 그리고 지연 작업 완료 이후 실행되어야 하는 함수는 지연 작업 함수의 인자로 (콜백 함수로) 전달하여
처리했습니다.

다음은 지연 작업을 위해 setTimeout() 함수를 사용하여 work1(), work2(), work3() 함수를 정의한 것입니다. 지연 작업과 상관없이 바로 실행되어야 하는 함수는 urgentWork()입니다.

```Javascript
function work1(onDone) {
  setTimeout(() => onDone("작업1 완료"), 100); // (1)
}
function work2(onDone) {
  setTimeout(() => onDone("작업2 완료"), 200); // (2)
}
function work3(onDone) {
  setTimeout(() => onDone("작업3 완료"), 300); // (3)
}
function urgentWork() {
  console.log("긴급 작업");
}

work1(function (msg1) { // (4)
  console.log("done after 100ms: " + msg1);
  work2(function (msg2) {
    console.log("done after 300ms: " + msg2);
    work3(function (msg3) {
      console.log("done after 600ms" + msg3);
    });
  });
});

urgentWork(); // (5)
```

1. 100ms 지연 작업을 하는 work1() 함수를 선언했습니다. 지연 작업 완료 후에는 onDone() 함수가 실행됩니다.
2. 200ms 지연 작업을 하는 work2() 함수를 선언했습니다. 지연 작업 완료 후에는 onDone() 함수가 실행됩니다.
3. 300ms 지연 작업을 하는 work3() 함수를 선언했습니다. 지연 작업 완료 후에는 onDone() 함수가 실행됩니다.
4. 지연 작업 함수를 실행합니다.
5. 지연 작업 완료 시간을 기다리지 않고 바로 실행되는 함수입니다.

위 3개의 지연 작업 함수들은 비동기 함수이므로 urgentWork() 함수가 바로 실행됩니다.

그런데 위의 코드를 보면 콜백(callback) 함수가 총 3개의 계단 모양으로 되어 있음을 알 수 있습니다. 이런 형태를 흔히 콜백 지옥(callback hell)이라 부릅니다.

### 비동기 함수 처리 방법 알아보기

ES6에서는 이러한 콜백 지옥을 해결할 수 있는 비동기 함수 표현법이 추가되었습니다. 실제로 Promise 클래스 함수는 다음과 같이 구현됩니다.

```Javascript
class Promise {
  constructor(fn) {
    const resolve = (...args) => {
      if (typeof this.onDone === "function") {
        this.onDone(...args);
      }
      if (typeof this.onComplete === "function") {
        this.onComplete();
      }
    };
    const reject = (...args) => {
      if (typeof this.onError === "function") {
        this.onError(...args);
      }
      if (typeof this.onComplete === "function") {
        this.onComplete();
      }
    };
    fn(resolve, reject);
  }
  then(onDone, onError) {
    this.onDone = onDone;
    this.onError = onError;
    return this;
  }
  catch(onError) {
    this.onError = onError;
    return this;
  }
  finally(onComplete) {
    this.onComplete = onComplete;
    return this;
  }
}
```

promise 객체를 생성할 때는 다음과 같이 클래스의 resolve() 함수 또는 reject() 함수에 해당하는 콜백 함수를 직접 전달해야 합니다.

```Javascript
const work1 = () =>
  new Promise((resolve) => {
    setTimeout(() => resolve("작업1 완료"), 100);
  });
const work2 = () =>
  new Promise((resolve) => {
    setTimeout(() => resolve("작업2 완료"), 200);
  });
const work3 = () =>
  new Promise((resolve) => {
    setTimeout(() => resolve("작업3 완료"), 300);
  });
```

resolve() 함수는 이후 then() 함수에 인자로 전달할 콜백 함수 onDone()과 일치합니다. 한편 여기서는 예외를 처리하는 reject() 함수는 콜백 함수로 전달하지 않았습니다.

Promise 객체를 반환하는 함수를 정의했으므로 work1(), work2(), work3() 순서로 지연 작업을 비동기로 수행해 보겠습니다. 실행 결과는 앞에서 기존 자바스크립트로 만든 것과 동일할 것입니다.
다만 차이가 있다면 '콜백 지옥이 없어졌다'라는 것입니다.

```Javascript
const work1 = () =>
  new Promise((resolve) => {
    setTimeout(() => resolve("작업1 완료"), 100);
  });
const work2 = () =>
  new Promise((resolve) => {
    setTimeout(() => resolve("작업2 완료"), 200);
  });
const work3 = () =>
  new Promise((resolve) => {
    setTimeout(() => resolve("작업3 완료"), 300);
  });

function urgentWork() {
  console.log("긴급 작업");
}

const nextWorkOnDone = (msg1) => {
  console.log("done after 100ms: " + msg1);
  return work2();
};

work1()
  .then(nextWorkOnDone)
  .then((msg2) => {
    console.log("done after 200ms:" + msg2);
    return work3();
  })
  .then((msg3) => {
    console.log("done after 600ms:" + msg3);
  });
urgentWork();
```

work1().then() 함수의 인자에 전달된 nextWorkOnDone() 함수가 Promise 안에 정의된 resolve() 함수로 전달됩니다. 그래서 resolve('작업1 완료!')는 실제로
nextWorkOnDone('작업1 완료!')와 동일하게 실행되는 것입니다.

이후 work2 구문을 실행하고 Promise 객체를 반복하고 이후 work3 구문을 실행하고 Promise 객체를 반복하고 이어지는 지연 작업들을 콜백 지옥 없이 구현할 수 있다는 것이 Promise 클래스의
장점입니다.

then() 함수가 Promise 객체를 반환하므로 이를 응용하면 각 지연 작업들을 나누거나 다시 합칠 수 있습니다.

```Javascript
const work1 = () =>
  new Promise((resolve) => {
    setTimeout(() => resolve("작업1 완료"), 100);
  });
const work2 = () =>
  new Promise((resolve) => {
    setTimeout(() => resolve("작업2 완료"), 200);
  });
const work3 = () =>
  new Promise((resolve) => {
    setTimeout(() => resolve("작업3 완료"), 300);
  });

function urgentWork() {
  console.log("긴급 작업");
}

const nextWorkOnDone = (msg1) => {
  console.log("done after 100ms: " + msg1);
  return work2();
};

work1()
  .then(nextWorkOnDone)
  .then((msg2) => {
    console.log("done after 200ms:" + msg2);
    return work3();
  })
  .then((msg3) => {
    console.log("done after 600ms:" + msg3);
  });
urgentWork();
```

이렇게 ES6의 다양한 기능을 통해 코드를 더 간결하고 효율적으로 작성할 수 있습니다.