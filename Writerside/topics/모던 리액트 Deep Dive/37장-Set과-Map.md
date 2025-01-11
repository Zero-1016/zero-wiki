# 37장 Set과 Map

## 1 Set

**Set 객체는 중복되지 않는 유일한 값들의 집합이다. Set 객체는 배열과 유사하지만 차이가 있다.**

| 구분                   | 배열 | Set |
|----------------------|----|-----|
| 동일한 값을 중복하여 포함할 수 있다 | O  | X   |
| 요소 순서에 의미가 있다        | O  | X   |
| 인덱스로 요소에 접근할 수 있다    | O  | X   |

Set은 수학적 집합의 특성과 일치한다. (동일한 값을 가질 수 없다.)

### 1.1 Set 객체의 생성

Set 객체는 Set 생성자 함수로 생성한다. Set 생성자 함수에 인수를 전달하지 않으면 빈 Set 객체가 생성된다.

```javascript
const set = new Set();
console.log(set); // Set(0) {}
```

**Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다. 이때 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.**

```javascript
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // 1, 2, 3

const set2 = new Set("Hello");
console.log(set2); // {'H', 'e', 'l', 'o'}
```

중복을 허용하지 않는 Set 객체의 특성을 활용하여 배열에서 중복된 요소를 제거할 수 있다.

```javascript
// 배열의 중복 요소 제거
const uniq1 = (array) => array.filter((v, i, arr) => arr.indexOf(v) === i);
console.log(uniq1([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]

// Set을 이용한 배열의 중복제거
const uniq2 = (array) => [...new Set(array)];
console.log(uniq2([2, 1, 2, 3, 4, 3, 4, 4])); // [ 2, 1, 3, 4 ]
console.log(typeof uniq2([2, 1, 2, 3, 4, 3, 4, 4])); // object

let array2 = uniq2([2, 1, 2, 3, 4, 3, 4, 4]);
array2.push(3);
console.log(array2); // [ 2, 1, 3, 4, 3 ]
```

### 1.2 요소 개수 확인

Set 객체의 요소 개수를 확인할 때는 Set.prototype.size 프로퍼티를 사용한다.

```javascript
const { size } = new Set([1, 2, 3]);
console.log(size); // 3
```

size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티이다. (즉 숫자를 할당하여 객체의 요소 개수를 변경할 수 없다.)

```javascript
const set = new Set([1, 2, 3, 3]);

console.log(Object.getOwnPropertyDescriptor(Set.prototype, "size"));
//{ get: [Function: get size],  set: undefined,  enumerable: false,  configurable: true }

set.size = 10; // 무시된다.
console.log(set.size); // 3
```

### 1.3 요소 추가

Set 객체에 요소를 추가할 때는 Set.prototype.add 메소드를 사용한다.

```javascript
const set = new Set();
console.log(set); // Set(0) {}

set.add(1).add(2).add(2);
console.log(set); // Set(2) {1, 2}
```

add 메소드는 새로운 요소가 추가된 Set 객체를 반환합니다. 따라서 add 메소드를 호출한 후에 add 메소드를 연속적으로 호출 할 수 있습니다.

일차 비교 연산자 === 을 사용하면 NaN과 NaN을 다르다고 평가합니다. 하지만 Set 객체는 NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.

```javascript
const set = new Set();

console.log(NaN === NaN); // false

set.add(NaN).add(NaN);
console.log(set); // Set(1) {NaN}

// +0과 -0도 같다고 평가하여 정보 추가를 허용하지 않는다.
set.add(0).add(-0);
console.log(set); // Set(2) {NaN, 0}
```

Set 객체는 객체나 배열과 같이 자바스크립트의 모든 값을 저장할 수 있습니다.

### 1.4 요소 존재 여부 확인

특정 요소가 존재하는지 확인하려면 Set.prototype.has 메소드를 사용합니다.

```javascript
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

### 1.5 요소 삭제

Set 객체의 특정 요소를 삭제하려면 Set.prototype.delete 메소드를 사용한다. delete 메소드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다.

단, delete 메소드에는 인덱스가 아니라 삭제하려는 값을 인수로 전달해야 한다. Set 객체는 순서에 의미가 없기에 인덱스를 갖지 않는다.

```javascript
const set = new Set([1, 2, 3]);

console.log(set); // Set(3) {1, 2, 3}

// 요소 2를 삭제한다.
set.delete(2);
console.log(set); // Set(2) {1, 3}

// 요소 1을 삭제한다.
set.delete(1);
console.log(set); // Set(1) {3}
```

만약 존재하지 않는 Set 객체의 요소를 삭제하려 하면 에러 없이 무시된다.

```javascript
const set = new Set([1, 2, 3]);

// 요소 1을 삭제한다.

console.log(set.delete(1)); // true
console.log(set); // Set(1) {3}
```

delete는 반환값으로 삭제 성공 여부를 나타내기도 한다.

### 1.6 요소 일괄 삭제

Set 객체의 모든 요소를 일괄 삭제하려면 Set.prototype.clear 메소드를 사용한다. clear 메소드는 항상 undefined를 반환한다.

```javascript
const set = new Set([1, 2, 3]);

console.log(set.clear()); // undefined
console.log(set); // Set(0) {}
```

### 1.7 요소 순회

Set 객체의 요소를 순회하려면 Set.prototype.forEach 메소드를 사용한다. 이는 Array.prototype.forEach와 유사하게 this로 사용될 객체 옵션을 인수로 전달한다.

- 첫 번째 인수 : 현재 순회중인 요소 값
- 두 번째 인수 : 현재 순회중인 요소 값
- 세 번째 인수 : 현재 순회중인 Set 객체 자체

첫 번째 인수와 두 번째 인수는 같은 값이다. 이처럼 동작하는 이유는 Array.prototype.forEach와 통일하기 위함이며 다른 의미는 없다.

```javascript
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
/*
1 1 Set(3) { 1, 2, 3 }
2 2 Set(3) { 1, 2, 3 }
3 3 Set(3) { 1, 2, 3 }
*/
```

**또한 Set 객체는 이터러블이다.** 따라서 for…of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링 할당의 대상이 될 수도 있다.

```javascript
const set = new Set([1, 2, 3]);

// Set 객체는 Set.prototype의 Symbol.iterator 메소드를 상속받는 이터러블이다.
console.log(Symbol.iterator in set) // true

// 이터러블인 Set 객체는 for ... of 문으로 순회할 수 있다.
for(const value of set){
  console.log(value); // 1 2 3
}

// 이터러블인 Set 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...set]); // [1, 2, 3]

// 이터러블인 Set 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, ...rest] = set;
console.log(a, rest); // 1, [2, 3]
```

Set 객체는 인덱스 번호가 없지만 순회하는 순서는 요소가 추가된 순서를 따른다. 이는 다른 이터러블의 순회와 호환성을 유지하기 위함이며, 엄격한 규칙은 아니다.

### 1.8  집합 연산

Set 객체는 수학적 집합을 구현하기 위한 자료구조이다. 따라서 Set 객체를 통해 교집합, 합집합, 차집합 등을 구현 할 수 있다.

```javascript
// 교집합

Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const value of set) {
    // 2개의 set 요소가 공통되는 요소이면 교집합의 대상이다.
    if (this.has(value)) result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) { 2, 4 }
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) { 2, 4 }
```

```javascript
// 합집합

Set.prototype.union = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 합집합은 2개의 Set 객체의 모든 요소로 구성된 집합이다. 중복된 요소는 포함되지 않는다.
    result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) { 1, 2, 3, 4 }
// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) { 2, 4, 1, 3 }
```

```javascript
// 차집합

Set.prototype.difference = function (set) {
  // this(set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 차집합은 어느 한쪽 집합에는 존재하지만 다른 한쪽 집합에는 존재하지 않는 요소로 구성된 집합이다.
    result.delete(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) { 1, 3 }
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

### 부분 집합과 상위 집합

집합 A가 집합 B에 포함되는 경우 집합 A는 집합 B의 부분 집합이며, 집합 B는 집합 A의 상위 집합이다.

```javascript
// this가 subset의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  for (const value of subset) {
    // superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인.
    if (!this.has(value)) return false;
  }
  return true;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```

## 2 Map

**Map 객체는 키와 값의 쌍으로 이루어진 컬렉션이다, Map 객체는 객체와 유사 하지만 다음과 같은 차이가 있다.**

| 구분             | 객체                      | Map 객체       |
|----------------|-------------------------|--------------|
| 키로 사용할  수 있는 값 | 문자열 또는 심벌 값             | 객체를 포함한 모든 값 |
| 이터러블           | X                       | O            |
| 요소 개수 확인       | Object.keys(obj).length | map.size     |

### 2.1 Map 객체의 생성

Map 객체는 Map 생성자 함수로 생성한다. 인수를 전달하지 않으면 빈 Map 객체가 생성된다.

만약 Map 생성자 함수로 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값이 덮어써진다. 따라서 중복된 키를 갖는 요소가 존재할 수 없다.

```javascript
const map = new Map([['key1', 'value1'],['key2', 'value1'], ['key1', 'value2']]);
console.log(map);

const map2 = new Map([1, 2]); // 키와 값이 나뉘지 않은 일반적인 배열이라 오류가 발생한다
const map2 = new Map([[1, 2]]); // 이렇게 하면 사용이 가능하다.
```

### 2.2 요소 개수 확인

Set 객체와 똑같이 Size 프로퍼티로 접근한다. 여기서도 Setter 함수없이 접근만 가능한 getter 함수만 존재하는 접근자 프로퍼티이다.

```javascript
const map = new Map([
  ["key1", "value1"],
  ["key2", "vaule1"],
  ["key1", "value2"],
]);

console.log(Object.getOwnPropertyDescriptor(Map.prototype, "size"));
// { get: [Function: get size],  set: undefined,  enumerable: false,  configurable: true }

map.size = 10; // 무시된다.
console.log(map.size); // 2
```

### 2.3 요소 추가

Map 객체에 요소를 추가할 때는 Map.prototype.set 메소드를 사용한다.

```javascript
const map = new Map();

map.set("key1", "value1").set("key2", "value2");

console.log(map);
// Map(2) { 'key1' => 'value1', 'key2' => 'value2' }
```

동일한 키를 추가할 경우 값이 덮어 씌워지고, NaN, (0, -0)도 값만 덮어 씌워진다.

객체는 문자열 또는 심벌 값만 키로 사용할 수 있다. 하지만 Map 객체는 키 타입에 제한이 없다 따라서 객체를 포함한 모든 값을 키로 사용할 수 있다.

```javascript
const map = new Map();

const lee = { name: "Lee" };
const kim = { name: "Kim" };

// 객체도 키로 사용할 수 있다.
map.set(lee, "developer").set(kim, "designer");
console.log(map);
// Map(2) { { name: 'Lee' } => 'developer', { name: 'Kim' } => 'designer' }
```

### 2.4 요소 취득

Map 객체에서 특정 요소를 취득하려면 Map.prototype.get 메소드를 사용한다. 키를 전달하면 인수로 전달한 키를 갖는 값을 반환한다. 해당 요소가 존재하지 않으면 undefined를 반환한다.

```javascript
const map = new Map();

const lee = { name: "Lee" };
const kim = { name: "Kim" };

map.set(lee, "developer").set(kim, "designer");

console.log(map.get(lee)); // developer
console.log(map.get("key")); // undefined
```

### 2.5 요소 존재 여부 확인

Map 객체에서는 해당 요소가 존재하는지 확인하려면 Map.prototype.has 메소드를 사용하고 메소드는 존재 여부를 불리언 값으로 반환한다.

```javascript
const map = new Map();

const lee = { name: "Lee" };
const kim = { name: "Kim" };

map.set(lee, "developer").set(kim, "designer");

console.log(map.has(lee)); // true
console.log(map.has("key")); // false
```

### 2.6 요소 삭제

Map 객체의 요소를 삭제하려면 Map.prototype.delete 메소드를 사용한다. 삭제 성공 여부를 불리언 값으로 반환한다.

```javascript
const map = new Map();

const lee = { name: "Lee" };
const kim = { name: "Kim" };

map.set(lee, "developer").set(kim, "designer");

console.log(map.delete(lee)); // true
```

존재하지 않는 키를 삭제하려 하면 무시된다. 삭제 성공여부를 나타내는 불리언 값을 반환해 set 메소드와 달리 연속적으로 사용이 불가능하다.

### 2.7 요소 일괄 삭제

Map 객체의 요소를 일괄 삭제하려면 Map.prototype.clear 메소드를 사용한다. clear 메소드는 항상 undefined를 반환한다.

```javascript
const map = new Map();

const lee = { name: "Lee" };
const kim = { name: "Kim" };

map.set(lee, "developer").set(kim, "designer");

console.log(map.clear()); // undefined
console.log(map); // Map(0) {}
```

### 2.8 요소 순회

Map 객체의 요소를 순회하려면 Map.prototype.forEach 메소드를 사용한다 Array.prototype.forEach와 유사하게 this로 사용될 객체(옵션)을 인수로 전달한다.

- 첫 번째 인수 : 현재 순회 중인 요소 값
- 두 번째 인수 : 현재 순회 중인 요소 키
- 세 번째 인수 : 현재 순회 중인 Map 객체 자체

```javascript
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "kim"],
]);

map.forEach((v, k, map) => console.log(v, k, map));
/* 
developer { name: 'Lee' } Map(2) { 
  { name: 'Lee' } => 'developer', 
  { name: 'Kim' } => 'kim' 
}

kim { name: 'Kim' } Map(2) {
   { name: 'Lee' } => 'developer', 
   { name: 'Kim' } => 'kim' 
}
*/
```

**Map 객체는 이터러블이다.** 따라서 for…of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링 할당의 대상이 될 수도 있다.

```javascript
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "kim"],
]);

// Map 객체는 Map.prototype의 Symbol.iterator 메소드를 상속받는 이터러블이다.
console.log(Symbol.iterator in map); // true

// 이터러블인 Map 객체는 for ... of 문으로 순회할 수 있다.
for (const entry of map) {
  console.log(entry); // [ { name: 'Lee' }, 'developer' ] [ { name: 'Kim' }, 'kim' ]
}

// 이터러블인 Map 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...map]); // [ [ { name: 'Lee' }, 'developer' ], [ { name: 'Kim' }, 'kim' ] ]

// 이터러블인 Map 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, b] = map;
console.log(a, b); // [ { name: 'Lee' }, 'developer' ] [ { name: 'Kim' }, 'kim' ]
```

Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메소드를 제공한다.

| Map 메소드               | 설명                                                     |
|-----------------------|--------------------------------------------------------|
| Map.prototype.keys    | Map 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다.      |
| Map.prototype.values  | Map 객체에서 요소값을 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다.          |
| Map.prototype.entries | Map 객체에서 요소키와 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다. |

```javascript
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "kim"],
]);

// Map.prototype.keys는 Map 객체에서 요소키를 값으로 갖는 이터레이터를 반환한다.
for (const key of map.keys()) {
  console.log(key); // { name: 'Lee' } { name: 'Kim' }
}

// Map.prototype.values는 Map 객체에서 요소값을 값으로 갖는 이터레이터를 반환한다.
for (const value of map.values()) {
  console.log(value); // developer kim
}

// Map.prototype.entries는 Map 객체에서 요소키와 요소값을 값으로 갖는 이터레이터를 반환한다.
for (const entry of map.entries()) {
  console.log(entry); // [ { name: 'Lee' }, 'developer' ] [ { name: 'Kim' }, 'kim' ]
}
```

Map 객체는 Set과 같이 순서에 의미가 없지만 순회하는 순서는 요소가 추가된 순서를 따른다 이는 다른 이터러블의 순회와 호환성을 유지하기 위함이다.