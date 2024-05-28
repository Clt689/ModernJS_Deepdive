# `32.1` String 생성자 함수
- 표준 빌트인 객체인 String객체는 생성자 함수 객체
- new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 빈 문자열을 할당한 String 래퍼 객체를 생성하여 반환
``` js
const strObj = new String();
console.log(strObj); // String {""}

const strObj2 = new String('Jay');
console.log(strObj2); // String {"Jay"}

// String 객체는 배열과 유사하게 length 프로퍼티와 index 사용 가능
console.log(strObj2.length); // 3
console.log(strObj2[0]); // J

// String 객체는 원시 값(불변 객체)이므로 변경 불가
strObj2[0] = 'C';
console.log(strObj2); // String {"Jay"}

// String 생성자 함수에 문자열이 아닌 값을 전달하면 문자열로 강제 변환
const strObj3 = new String(123);
console.log(strObj3); // String {"123"}

const strObj4 = new String(null);
console.log(strObj4); // String {"null"}
```

# `32.2` length 프로퍼티
- String 객체는 length 프로퍼티를 갖는다.
- length 프로퍼티는 문자열의 길이를 반환한다.
``` js
'Hello'.length; // 5
```

# `32.3` String 메서드
- String 객체는 문자열과 관련된 다양한 메서드를 제공
- String.prototype 객체의 메서드는 모두 불변성을 지니기 때문에 읽기 전용(readonly)객체로 제공된다.
- String.prototype 객체의 메서드는 문자열을 직접 변경하는 것이 아니라 새로운 문자열을 생성하여 반환한다.
``` js
const strObj = new String('JAY');

console.log(Object.getOwnPropertyDescriptors(strObj));
/*
{
  0: {value: "J", writable: false, enumerable: true, configurable: false},
  1: {value: "A", writable: false, enumerable: true, configurable: false},
  2: {value: "Y", writable: false, enumerable: true, configurable: false},
  length: {value: 3, writable: false, enumerable: false, configurable: false}
}
*/
```

## `32.3.1` String.prototype.indexOf
- indexOf 메서드는 대상 문자열에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환.
- 대상 문자열에 인수로 전달받은 문자열이 존재하지 않으면 -1을 반환
``` js
const str = 'Hello World';

str.indexOf('l'); // 2
str.indexOf('or'); // 7
str.indexOf('x'); // -1

// indexOf보다 가독성이 더 좋은 includes 메서드
str.includes('Hello'); // true
```

## `32.3.3` String.prototype.includes
- includes 메서드는 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 결과를 반환
- 포함되어 있으면 true, 아니면 false
``` js
  const str = 'Hello World';
  str.includes('Hello'); // true
  str.includes('x'); // false
  str.includes(); // false
```

## `32.3.4` String.prototype.startsWith
- startsWith 메서드는 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 결과를 반환
- 시작하면 true, 아니면 false
``` js
const str = 'Hello World';
str.startsWith('Hello'); // true
str.startsWith('World'); // false
```

## `32.3.5` String.prototype.endsWith
- endsWith 메서드는 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 결과를 반환
- 끝나면 true, 아니면 false
``` js
const str = 'Hello World';
str.endsWith('World'); // true
str.endsWith('Hello'); // false
```

## `32.3.6` String.prototype.charAt
- charAt 메서드는 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 반환
- 인덱스는 0부터 시작
- 인덱스가 범위를 벗어나면 빈 문자열을 반환
``` js
const str = 'Hello World';
str.charAt(1); // e
str.charAt(100); // ''
```

## `32.3.8` String.prototype.slice
- slice 메서드는 대상 문자열의 일부를 추출하여 새로운 문자열을 반환
- 첫 번째 인수는 추출 시작 인덱스, 두 번째 인수는 추출을 종료할 인덱스
- 두 번째 인수 생략하면 대상 문자열의 끝까지 추출
- 음수를 전달하면 대상 문자열의 끝에서부터의 인덱스로 해석
``` js
const str = 'Hello World';
str.slice(0, 5); // Hello
str.slice(6); // World
str.slice(0, -1); // Hello Worl

// 인수 < 0 또는 NaN이면 0으로 취급
str.slice(0, -100); // ''

// 음수는 뒤부터 시작
str.slice(-5); // World
```

## `32.3.9` String.prototype.toUpperCase
- toUpperCase 메서드는 대상 문자열을 모두 대문자로 변경하여 반환
``` js
const str = 'Hello World';
str.toUpperCase(); // HELLO WORLD
```

## `32.3.10` String.prototype.toLowerCase
- toLowerCase 메서드는 대상 문자열을 모두 소문자로 변경하여 반환
``` js
const str = 'Hello World';
str.toLowerCase(); // hello world
```

## `32.3.12` String.prototype.repeat
- repeat 메서드는 대상 문자열을 인수로 전달받은 횟수만큼 반복하여 새로운 문자열을 반환
- 인수로 전달받은 횟수가 0이면 빈 문자열을 반환
- 인수로 전달받은 횟수가 정수가 아니면 정수로 강제 변환
``` js
const str = 'abc';
str.repeat(3); // abcabcabc
```

## `32.3.14` String.prototype.split
- split 메서드는 대상 문자열을 인수로 전달받은 문자열 또는 정규 표현식으로 분할하여 배열로 반환
``` js
const str = 'How are you doing?';

// 공백으로 구분(단어로 구분)하여 배열로 반환
str.splice(' ') // ['How', 'are', 'you', 'doing?']

// \s는 공백 문자를 의미.
str.splice(/\s/); // ['How', 'are', 'you', 'doing?']

// 인수로 빈 문자열을 전달하면 모두 분리
str.splice('') // ["H","o",'w' ... 'i','n','g','?']

// 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열 반환
str.splice(); // ['How are you doing?']

// 공백으로 구분하고 배열의 길이 지정
str.splice(' ', 2); // ['How', 'are']

// 역순으로 정렬
str.splice(' ').reverse(); // ['doing?', 'you', 'are', 'How']
```




