# `31.1` 정규 표현식이란?
- 정규표현식은 일정한 규칙(패턴)을 가진 문자열을 표현하는 방법.
- 문자열을 대상으로 패턴 매칭 기능 제공
- 문자열 검색, 치환, 추출 등의 작업을 할 때 사용
```js
// 사용자로부터 입력받은 휴대폰 전화번호
const tel = '010-1234-567팔';

// 정규 표현식 리터럴로 전화번호 패턴 정의
const regExp = /^\d{3}-\d{3,4}-\d{4}$/;

// tel이 정규 표현식 regExp의 패턴에 부합하는지 테스트
regExp.test(tel); // false
```

# `31.2` 정규 표현식 생성
- 정규 표현식은 RegExp 생성자 함수를 통해 생성할 수 있다.
- 정규 표현식 리터럴은 패턴과 플래그로 구성.
- `/` `패턴(pattern)` `/` `플래그(flag)`
```js
const target = 'Is this all there is?';

// 패턴: is
// 플래그: i (대소문자 구별 안함)
const regexp1 = /is/i;
regexp1.test(target); // true


/** RegExp 생성자 함수를 통해 생성
*
* pattern: 정규표현식의 패턴
* flags: 정규표현식의 플래그(g, i, m, s, u, y)
* new RegExp(pattern[, flags])
*
*/

const regexp2 = new RegExp(/is/i);
regexp2.test(target); // true

const count = (str,char) => (str.match(new RegExp(char, 'gi')) ?? []).length;

count('Is this all there is?', 'is'); // 3 =>
count('Is this all there is?', 'xx'); // 0
```

# `31.3` RegExp 메서드
## `31.3.1` RegExp.prototype.exec
- 정규 표현식과 일치하는 문자열을 검색하여 배열로 반환
- 일치하는 문자열이 없으면 null 반환
```js
const target
  const target = 'Is this all there is?';
  const regexp = /is/;

  regExp.exec(target); 
  // ["is", index: 5, input: "Is this all there is?", groups: undefined]
  // index: 일치하는 문자열의 첫 번째 문자의 인덱스
  // input: 대상 문자열
  // groups: 정규 표현식의 그룹 캡처 결과
```

## `31.3.2` RegExp.prototype.test
- 정규 표현식과 일치하는 문자열이 존재하면 Boolean 값 반환
```js
const target = 'Is this all there is?';
const regexp = /is/;
regexp.test(target); // true
```

## `31.3.3` String.prototype.match
- 문자열이 정규 표현식과 일치하면 일치하는 문자열을 배열로 반환
- 일치하는 문자열이 없으면 null 반환
- exec 메서드는 모든 패턴을 검색하는 g플래그를 지정해도 첫 번째 패턴만 반환하지만 match메서드는 모든 매칭 패턴을 배열로 반환
```js
  const target = "Is this all there is?";
  const regexp = /is/;

  target.match(regexp); 
  // ["is", index: 5, input: "Is this all there is?", groups: undefined]
  // index: 일치하는 문자열의 첫 번째 문자의 인덱스
  // input: 대상 문자열
  // groups: 정규 표현식의 그룹 캡처 결과
```

# `31.4` 플래그
- 패턴과 함께 정규 표현식을 구성하는 플래그
- 정규 표현식의 검색 방법을 설정하기 위해 사용.
- 플래그는 옵션이므로 생략 가능
- 플래그를 사용하지 않은 경우 대소문자를 구별하여 검색
- 문자열에 패턴 검색 매칭 대상이 1개 이상 존재해도 첫번째 대상만 검색.

|플래그|의미|설명|
|:--:|:--:|:--:|
|i|ignore case|대소문자 구별 안함|
|g|global|대상 문자열 내에서 모든 패턴 검색|
|m|multiline|여러 줄 검색|

```js
const target = 'Is this all there is?';

// target 문자열에서 is 문자열을 대소문자 구별 한 번만 검색
target.match(/is/); // ["is", index: 5, input: "Is this all there is?", groups: undefined]

// target 문자열에서 is 문자열을 대소문자 구별 하지 않고 한 번만 검색
target.match(/is/i); // ["Is", index: 0, input: "Is this all there is?", groups: undefined]

// target 문자열에서 is 문자열을 대소문자 구별해서 모두 검색
target.match(/is/g); // ["is", "is"]

// target 문자열에서 is 문자열을 대소문자 구별 하지 않고 모두 검색
target.match(/is/ig); // ["Is", "is", "is"]
```

# `31.5` 패턴
- 정규 표현식의 패턴은 일정한 규칙을 가진 문자열
- 패턴은 문자열을 검색할 때 사용
## `31.5.1` 문자열 검색
- 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열을 검색
- 플래그를 사용하지 않으면 대소문자를 구별하여 검색 후 첫 번째 결과만 반환
```js
const target = 'Is this all there is?';

// 'is'문자열과 매치하는 패턴, 플래그가 생략되어 대소문자 구별하여 검색
const regExp = /is/;

// target과 정규표현식이 매치하는지 테스트
regExp.test(target); // true

// target과 정규표현식의 매칭 결과를 반환
target.match(regExp); // ["is", index: 5, input: "Is this all there is?", groups: undefined]

// 대소문자 구별하지 않는 검색을 위해 i플래그 사용
const regExp2 = /is/i;
regExp2.match(target); // ["Is", index: 0, input: "Is this all there is?", groups: undefined]

// 패턴가 매치하는 모든 문자열 검색을 위해 g플래그 사용
const regExp3 = /is/ig;
target.match(regExp3); // ["Is", "is", "is"]
```

## `31.5.2` 임의의 문자열 검색
- `.`은 임의의 문자 1개를 의미
```js
const target ="Is this all there is?";

// 임의의 3자리 문자열을 대소문자 구별하여 검색
const regExp = /.../g;
target.match(regExp); // ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```

## `31.5.3` 반복 검색
- `{m,n}`은 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열을 의미
- `{m,}`와 같이 콤마 뒤에 공백이 있으면 정상 동작하지 않으니 주의.
```js
const target = 'A AA B BB Aa Bb AAA';

// A가 1번 이상 반복되는 패턴을 대소문자 구별하여 검색
const regExp = /A{1,2}/g;
target.match(regExp); // ["A", "AA", "A", "A"]


// {n}은 {n,n}과 같음
// A가 2번 반복되는 패턴을 대소문자 구별하여 검색
const regExp2 = /A{2}/g;
target.match(regExp2); // ["AA"]


// {m,}은 최소 m번 이상 반복되는 패턴을 의미
// A가 2번 이상 반복되는 패턴을 대소문자 구별하여 검색
const regExp3 = /A{2,}/g;
target.match(regExp3); // ["AA", "AAA"]


// +는 {1,}과 같음
// A가 1번 이상 반복되는 패턴을 대소문자 구별하여 검색
const regExp4 = /A+/g;
target.match(regExp4); // ["A", "AA", "A", "A", "AAA"]


// ?는 {0,1}과 같음
// A가 0번 또는 1번 반복되는 패턴을 대소문자 구별하여 검색
const target2 = "color colour";
const regExp5 = /colou?r/g;
target2.match(regExp5); // ["color", "colour"]
```

## `31.5.4` OR 검색
- `|`는 OR 연산자로 사용되어 패턴을 OR로 연결
```js
const target = 'A AA B BB Aa Bb';

// A|B 전역 검색
const regExp = /A|B/g;
target.match(regExp); // ["A", "A", "A", "B", "B", "A", "B"]

// 분해되지 않는 단어 레벨로 검색하려면 +를 사용
const regExp2 = /A+|B+/g;
target.match(regExp2); // ["A", "AA", "B", "BB", "A", "B"]

// []내의 문자는 or로 동작, []+는 []내의 문자가 1번 이상 반복되는 패턴을 의미
const regExp3 = /[AB]+/g;
target.match(regExp3); // ["A", "AA", "B", "BB", "A", "B"]

// 대소문자 구별하지 않는 검색
const target3 = 'AA BB Aa Bb 12'
const regExp4 = /[A-Za-z]+/g;

target3.match(regExp4); // ["AA", "BB", "Aa", "Bb"]

// 숫자만 검색
const regExp5 = /[0-9]+/g;
target3.match(regExp5); // ["12"]

// 쉼표로 매칭 결과 분리
const target4 = 'AA BB 12,345';

// 0~9 또는 ','가 1번 이상 반복되는 패턴을 대소문자 구별하여 검색
const regExp6 = /[0-9,]+/g;
target4.match(regExp6); // ["12,345"]

// \d는 숫자와 동일
const regExp7 = /[\d,]+/g;
target4.match(regExp7); // ["12,345"]

// \D는 숫자가 아닌 문자와 동일
const regExp8 = /[\D]+/g;
target4.match(regExp8); // ["AA BB ", ","]

// \w는 영문자, 숫자, 밑줄 문자와 동일
const target5 = 'AA BB 12_345 _$^&';
const regExp9 = /[\w]+/g;
target5.match(regExp9); // ["AA", "BB", "12_345", "_"]
```

## `31.5.5` NOT 검색
- `^`는 NOT 연산자로 사용되어 패턴을 NOT으로 연결
```js
const target = 'AA BB 12 Aa Bb';

// 숫자를 제외한 문자열 전역 검색
const regExp = /[^0-9]+/g;
target.match(regExp); // ["AA BB ", " Aa Bb"]
```

## `31.5.6` 시작 위치 검색
- `[]` 밖의 `^`는 문자열의 시작 위치를 의미
```js
const target = 'https://poiemaweb.com';

// https로 시작하는 패턴을 대소문자 구별하여 검색
const regExp = /^http/;
regExp.test(target); // true
```

## `31.5.7` 끝 위치 검색
- `$`는 문자열의 끝 위치를 의미
```js
const target = 'https://poiemaweb.com';

// com으로 끝나는 패턴을 대소문자 구별하여 검색
const regExp = /com$/;
regExp.test(target); // true
```

# `31.6` 자주 사용하는 정규표현식
## `31.6.1` 특정 단어로 시작하는지 검사
```js
const url = 'https://example.com';

// http 또는 https로 시작하는지 검사
const regExp = /^https?:\/\//;
regExp.test(url); // true

const regExp2 = new RegExp('^http|https://');
regExp2.test(url); // true
```

## `31.6.2` 특정 단어로 끝나는지 검사
```js
  const fileName = 'index.html';

  // .html로 끝나는지 검사
  const regExp = /.html$/;
  regExp.test(fileName); // true
```

## `31.6.3` 숫자로만 이루어진 문자열인지 검사
- `[]` 바깥의 `^`는 문자열의 시작
- `$`은 문자열의 끝
- `\d`는 숫자
- `+`는 앞선 패턴을 1번 이상 반복
```js
const target = '12345';

// 처음과 끝이 숫자이고 최소 1번 이상 반복되는 문자열
const regExp = /^\d+$/;
regExp.test(target); // true
```

## `31.6.4` 하나 이상의 공백으로 시작하는지 검사
- `\s`는 공백 문자
```js
const target = '  Hi';

// 공백으로 시작하는지 검사
const regExp = /^[\s]+/;
regExp.test(target); // true
```

## `31.6.5` 아이디로 사용 가능한지 검사
```js
const id = 'abc123';

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10자리인지 검사
const regExp = /^[A-Za-z0-9]{4,10}$/;
regExp.test(id); // true
```

## `31.6.6` 이메일 주소 형식인지 검사
```js
const email = 'jay@javascript.com';

// 이메일 주소 형식인지 검사
const regExp = /^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,3}$/;
regExp.test(email); // true
```

## `31.6.7` 전화번호 형식인지 검사
```js
const tel = '010-1234-5678';

// 전화번호 형식인지 검사
const regExp = /^\d{3}-\d{3,4}-\d{4}$/;
regExp.test(tel); // true
```

## `31.6.8` 특수문자 포함 여부 검사
```js
const target = 'abc123!@#';

// 특수문자 포함 여부 검사
const regExp = /[^A-Za-Z0-9]/gi;
const regExp2 = /[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi;
regExp.test(target); // true

// 특수문자 제거를 위한 replace메서드
target.replace(regExp, ''); // abc123
```