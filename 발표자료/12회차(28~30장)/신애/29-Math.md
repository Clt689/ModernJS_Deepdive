# `29.1` Math 프로퍼티
## `29.1.1` Math.PI
- 원주율을 나타내는 상수.
- 3.141592653589793

```js
console.log(Math.PI); // 3.141592653589793
```
<br/>

# `29.2` Math 메서드
## `29.2.1` Math.abs
- 인수의 절대값을 반환.
- 인수가 양수이면 인수를 반환하고, 인수가 음수이면 -인수를 반환.

```js
Math.abs(-1); // 1
Math.abs(1);  // 1

Math.abs(0);    // 0
Math.abs(null); // 0
Math.abs('');   // 0

Math.abs(undefined); // NaN
Math.abs('string');  // NaN
Math.abs();          // NaN
```

## `29.2.2` Math.round
- 인수의 소수점 이하를 반올림한 정수를 반환.

```js
Math.round(1.4); // 1
Math.round(1.5); // 2
```


## `29.2.3` Math.ceil
- 인수의 소수점 이하를 올림한 정수를 반환.

```js
Math.ceil(1.4); // 2
Math.ceil(1.5); // 2
```

## `29.2.4` Math.floor
- 인수의 소수점 이하를 내림한 정수를 반환.

```js
Math.floor(1.4); // 1
Math.floor(1.5); // 1
```

## `29.2.5` Math.sqrt
- 인수의 제곱근을 반환.

```js
Math.sqrt(9); // 3
Math.sqrt(-9); // NaN`
Math.sqrt(2); // 1.4142135623730951
```

`
## `29.2.6` Math.random
- 0 이상 1 미만의 부동소수점 의사 난수를 반환.
- 0 <= Math.random() < 1 (0은 포함, 1은 미포함)

```js
Math.random(); // 0.12345678901234567 (0에서 1미만의 부동소수점 난수)

// 1에서 10 사이의 무작위 정수를 생성
Math.floor(Math.random() * 10) + 1
```

## `29.2.7` Math.pow
- 첫 번째 인수를 밑으로, 두 번째 인수를 지수로하여 거듭제곱한 결과를 반환.

```js
Math.pow(2, 8); // 256
Math.pow(2, -1); // 0.5
Math.pow(2) // NaN

// ES7: 거듭제곱 연산자
2 ** 8; // 256
```

## `29.2.8` Math.max
- 전달받은 인수 중 가장 큰 수를 반환.
- 인수가 전달되지 않으면 -Infinity를 반환.

```js
Math.max(); // -Infinity
Math.max(1, 2, 3); // 3
Math.max(1, 2, 'string'); // NaN

// 배열을 인수로 전달할 때는 Function.prototype.apply 메서드를 사용
Math.max.apply(null, [1, 2, 3]); // 3

// Spread operator ⭐️
Math.max(...[1, 2, 3]); // 3
```

## `29.2.9` Math.min
- 전달받은 인수 중 가장 작은 수를 반환.
- 인수가 전달되지 않으면 Infinity를 반환.

```js
Math.min(); // Infinity
Math.min(1, 2, 3); // 1
Math.min(1, 2, 'string'); // NaN

// 배열을 인수로 전달할 때는 Function.prototype.apply 메서드를 사용
Math.min.apply(null, [1, 2, 3]); // 1

// Spread operator ⭐️
Math.min(...[1, 2, 3]); // 1
```




