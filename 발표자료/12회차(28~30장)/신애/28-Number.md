# `28.1` Number 생성자 함수
- 표준 빌트인 객체 Number객체는 생성자 함수 객체
- new 연산자와 함께 호출하여 Number 객체를 생성할 수 있다.
```js
// Number 생성자 함수는 인수로 전달된 값을 숫자로 변환하여 반환.
const numObj = new Number();
console.log(numObj) // Number {[primitive value]: 0}

const numObj2 = new Number(10);
console.log(numObj2) // Number {[primitive value]: 10}


// 숫자가 아닌 값을 전달하면 인수를 숫자로 강제 변환하거나 NaN을 반환.
const numObj3 = new Number('Hello');
console.log(numObj3) // Number {[primitive value]: NaN}

const numObj4 = new Number('10');
console.log(numObj4) // Number {[primitive value]: 10}

// new연산자 없이 생성자 함수를 호출하면 숫자를 반환(명시적 형변환)
const numObj5 = Number('10');
console.log(numObj5) // 10

const numObj6 = Number('10.53');
console.log(numObj6) // 10.53
```
<br/>

# `28.2` Number 프로퍼티
## `28.2.1` Number.EPSILON
- 1과 1보다 큰 숫자 중 가장 작은 숫자와의 차이
- 부동소수점 연산의 오차를 해결하기 위해 사용
```js
console.log(0.1 + 0.2 === 0.3); // false(0.30000000000000004)

// a와 b를 뺀 값의 절대값이 Number.EPSILON보다 작으면 같은 수로 인정.
function isEqual(a, b) {
  return Math.abs(a - b) < Number.EPSILON;
}

console.log(isEqual(0.1 + 0.2, 0.3)); // true
```

## `28.2.2` Number.MAX_VALUE
- 자바스크립트에서 표현할 수 있는 가장 큰 숫자
- Infinity보다 작은 값
```js
console.log(Number.MAX_VALUE); // 1.7976931348623157e+308
Infinity > Number.MAX_VALUE; // true
```

## `28.2.3` Number.MIN_VALUE
- 자바스크립트에서 표현할 수 있는 가장 작은 숫자
- 0보다 큰 값
```js
console.log(Number.MIN_VALUE); // 5e-324
0 < Number.MIN_VALUE; // true
```

## `28.2.4` Number.MAX_SAFE_INTEGER
- 안전한 정수의 최대값
- 2^53 - 1
```js
console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991
```

## `28.2.5` Number.MIN_SAFE_INTEGER
- 안전한 정수의 최소값
- -(2^53 - 1)
```js
console.log(Number.MIN_SAFE_INTEGER); // -9007199254740991
```

## `28.2.6` Number.POSITIVE_INFINITY
- 양의 무한대
```js
console.log(Number.POSITIVE_INFINITY); // Infinity
```

## `28.2.7` Number.NEGATIVE_INFINITY
- 음의 무한대
```js
console.log(Number.NEGATIVE_INFINITY); // -Infinity
```

## `28.2.8` Number.NaN
- 숫자가 아님을 나타내는 값
```js
console.log(Number.NaN); // NaN
```
<br/>

# `28.3` Number 메서드
## `28.3.1` Number.isFinite
- 전달받은 인수가 유한수인지 검사
- 유한수이면 true, 아니면 false
```js
// 유한수 판별
console.log(Number.isFinite(0)); // true
console.log(Number.isFinite(Number.MAX_VALUE)); // true

console.log(Number.isFinite(Infinity));  // false
console.log(Number.isFinite(-Infinity)); // false

// NaN은 유한수가 아님
console.log(Number.isFinite(NaN));   // false

//Number.isFinite는 암묵적 타입변환X, isFinite는 암묵적 타입변환O
console.log(Number.isFinite(null));  // false
console.log(isFinite(null));         // true
```

## `28.3.2` Number.isInteger
- 전달받은 인수가 정수인지 검사
- 정수이면 true, 아니면 false
```js
console.log(Number.isInteger(0)); // true
console.log(Number.isInteger(1.0)); // true
console.log(Number.isInteger(-1)); // true

console.log(Number.isInteger(0.1)); // false
console.log(Number.isInteger(NaN)); // false
console.log(Number.isInteger(Infinity)); // false
```

## `28.3.3` Number.isNaN
- 전달받은 인수가 NaN인지 검사
- NaN이면 true, 아니면 false
```js
console.log(Number.isNaN(NaN)); // true

console.log(Number.isNaN(10)); // false
console.log(Number.isNaN('Hello')); // false

// isNaN은 암묵적 타입변환을 수행(undefined -> NaN)
console.log(isNaN(undefined)); // true
```

## `28.3.4` Number.isSafeInteger
- 전달받은 인수가 안전한 정수인지 검사
- 안전한 정수이면 true, 아니면 false
```js
console.log(Number.isSafeInteger(0)); // true
console.log(Number.isSafeInteger(1.0)); // true
console.log(Number.isSafeInteger(-1)); // true

console.log(Number.isSafeInteger(0.5)); // false
console.log(Number.isSafeInteger('123')); // false
console.log(Number.isSafeInteger(Infinity)); // false
```

## `28.3.5` Number.prototype.toExponential
- 숫자를 지수 표기법으로 변환하여 문자열로 반환
```js
(77.1234).toExponential(); // '7.71234e+1'
(77.1234).toExponential(2); // '7.71e+1'
```

## `28.3.6` Number.prototype.toFixed
- 숫자를 반올림하여 문자열로 반환
```js
(12345.6789).toFixed(); // '12346'
(12345.6789).toFixed(1); // '12345.7'
```

## `28.3.7` Number.prototype.toPrecision
- 숫자를 지정한 자릿수로 반올림하여 문자열로 반환
```js
(12345.6789).toPrecision(); // '12345.6789'
(12345.6789).toPrecision(1); // '1e+4'
```

## `28.3.8` Number.prototype.toString
- 숫자를 문자열로 변환하여 반환
```js
(10).toString(); // '10'
(16).toString(2); // '1010' // 2진수
(16).toString(8); // '20' // 8진수
(16).toString(16); // '10' // 16진수
```
