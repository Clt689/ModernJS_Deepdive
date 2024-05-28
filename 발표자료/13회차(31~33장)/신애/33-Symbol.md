# `33.1` 심벌이란?
- 심벌은 ES6에서 추가된 7번째 타입으로 변경 불가능한 원시 타입의 값
- 주로 이름 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용.

# `33.2` 심벌 생성
## `33.2.1` Symbol 함수
- Symbol 함수는 심벌 값을 생성
- 생성된 Symbol 값은 외부로 노출되지 않아 확인할 수 없으며, 다른 값과 중복되지 않음.
``` js
// Symbol 함수를 호출하여 심벌 값을 생성
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol); // Symbol()

// new 연산자와 사용 불가
new Symbol(); // TypeError: Symbol is not a constructor

// 심벌 값에 대한 설명이 같더라도 유일무이한 심벌 값 생성
const mySymbol1 = Symbol('mySymbol');
const mySymbol2 = Symbol('mySymbol');
console.log(mySymbol1 === mySymbol2); // false

// 심벌 값은 변경 불가능한 원시 값이다.
mySymbol1.description = 'symbol value';
console.log(mySymbol1.description); // undefined
console.log(mySymbol1 + ''); // TypeError: Cannot convert a Symbol value to a string

// 심벌 값은 형변환 불가
String(mySymbol1); // 'Symbol(mySymbol)'
mySymbol1 + ''; // TypeError: Cannot convert a Symbol value to a string
```

## `33.2.2` Symbol.for, Symbol.keyFor
- Symbol.for 메서드는 전역 심벌 레지스트리에서 심벌 값을 생성
- Symbol.for 메서드는 전역 심벌 레지스트리에 심벌 키가 존재하면 해당 심벌 값을 반환하고, 존재하지 않으면 심벌 키로 심벌 값을 생성.
- Symbol.keyFor 메서드는 전역 심벌 레지스트리에 등록된 심벌 값의 키를 반환
``` js
// Symbol.for 메서드는 전역 심벌 레지스트리에 심벌 키가 존재하면 해당 심벌 값을 반환하고, 존재하지 않으면 심벌 키로 심벌 값을 생성
const s1 = Symbol.for('mySymbol');

// 전역 심벌 레지스트리에 심벌 키가 존재하면 해당 심벌 값을 반환
const s2 = Symbol.for('mySymbol');
console.log(s1 === s2); // true

// Symbol.keyFor 메서드는 전역 심벌 레지스트리에 등록된 심벌 값의 키를 추출
console.log(Symbol.keyFor(s1)); // mySymbol
```

# `33.4` 심벌과 프로퍼티키
- 심벌은 유일무이한 프로퍼티 키를 만들기 위해 사용
- 심벌 값은 외부로 노출되지 않아 확인할 수 없으며, 다른 값과 중복되지 않음.
``` js
const obj = {
  [Symbol.for('mySymbol')]: 1
};

// 심벌 값은 유일한 값이므로 다른 프로퍼티 키와 충돌하지 않는다.
obj[Symbol.for('mySymbol')]; // 1
```

# `33.5` 심벌과 프로퍼티 은닉
- 심벌은 프로퍼티 키를 외부로 노출되지 않아 프로퍼티 키를 심볼로 만들어 프로퍼티를 은닉할 수 있음.
``` js
const obj = {
  // 심벌 값으로 프로퍼티 생성
  [Symbol.for('mySymbol')]: 1
}

for (const key in obj) {
  console.log(key); // 심벌 값은 열거되지 않음.
}

object.keys(obj); // []
Object.getOwnPropertyNames(obj); // []

// getOwnPropertySymbols 메서드는 객체 자신의 심벌 프로퍼티 키를 배열로 반환하므로 완벽한 은닉은 불가
Object.getOwnPropertySymbols(obj); // [Symbol(mySymbol)]
```
