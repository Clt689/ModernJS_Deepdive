# `30.1` Date 생성자 함수
- Date 생성자 함수는 Date 객체를 생성한다.
- Date 객체는 날짜와 시간을 나타내는 정수값을 갖는다.
- Date 객체는 1970년 1월 1일 00:00:00(UTC)를 기점으로 지난 시간을 밀리초(ms) 단위로 나타낸다.

## `30.1.1` new Data()
- new Date()로 Date 객체를 생성하면 현재 시간을 나타내는 Date 객체를 생성한다.
- new 연산자 없이 Date()로 호출하면 Date 객체가 아닌 날짜와 시간 정보를 문자열로 반환한다.

```js
const date = new Date();
console.log(date); // 2021-07-07T07:00:00.000Z

const date2 = Date();
console.log(date2); // Wed Jul 07 2021 16:00:00 GMT+0900 (대한민국 표준시)
```

## `30.1.2` new Date(milliseconds)
- Date 생성자 함수에 밀리초를 인수로 전달
- 1970년 1월 1일 00:00:00(UTC)를 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 생성한다.

```js
const date = new Date(0);
console.log(date); // 1970-01-01T00:00:00.000Z

const date2 = new Date(1000 * 60 * 60 * 24);
console.log(date2); // 1970-01-02T00:00:00.000Z

const date3 = new Date(1000 * 60 * 60 * 24 * 365);
console.log(date3); // 1971-01-01T00:00:00.000Z
```

## `30.1.3` new Date(dateString)
- Date 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수로 전달
- 인수로 전달된 문자열은 Date.parse 메서드에 의해 해석되어 밀리초로 변환된다.

```js
const date = new Date('2024-05-19T00:00:00');
console.log(date); // 2024-05-19T00:00:00.000Z

const date2 = new Date('May 19, 2024 09:26:00');
console.log(date2); // 2024-05-19T00:26:00.000Z

const date3 = newData('2024/05/19/00:00:00');
console.log(date3); // 2024-05-19T00:00:00.000Z
```

## `30.1.4` new Date(year, month[, day, hour, minute, second, millisecond])
- Date 생성자 함수에 연, 월, 일, 시, 분, 초, 밀리초를 인수로 전달하면 Date객체를 반환.
- ⚠️ 월은 0부터 시작하므로 1을 더해야 함.

```js
const date = new Date(2021, 4);
console.log(date); // 2021-05-01T00:00:00.000Z

const date2 = new Date(2024, 4, 19, 10, 00, 00, 0); // 2024년 5월 19일 10시 00분 00초
console.log(date2); // 2024-05-19T01:00:00.000Z

const date3 = new Date('2024/4/19/10:00:00'); // 가독성을 위한 표기법
console.log(date3); // 2024-05-19T01:00:00.000Z
```

# `30.2` Date 메서드
## `30.2.1` Date.now()
- Date.now 메서드는 1970년 1월 1일 00:00:00(UTC)를 기점으로 현재 시간까지 경과한 밀리초를 반환.
  
  ```js
  const now = Date.now();
  console.log(now); // 1716079059634

  new Date(now); // Sun May 19 2024 09:37:39 GMT+0900 (한국 표준시)
  ```

## `30.2.2` Date.parse()
- Date.parse 메서드는 인수로 전달받은 날짜와 시간을 나타내는 문자열을 해석
- 1970년 1월 1일 00:00:00(UTC)를 기점으로 경과한 밀리초를 반환.
  
  ```js
  Date.parse('2024-05-19T00:00:00'); // 1716044400000
  Date.parse('May 19, 2024 09:26:00'); // 1716044400000
  Date.parse('2024/05/19/00:00:00'); // 1716044400000
  ```

## `30.2.3` Date.UTC()
- 1970년 1월 1일 00:00:00(UTC)를 기점으로 경과한 밀리초를 숫자로 반환.
- Date.UTC 메서드는 인수로 연, 월, 일, 시, 분, 초, 밀리초를 받아서 밀리초로 변환.
- 로컬 타임이 아닌 UTC 기준으로 날짜와 시간을 계산.
- ⚠️ 월은 0부터 시작하므로 1을 더해야 함.
  
  ```js
  Date.UTC(2024, 4); // 1716044400000
  Date.UTC(2024, 4, 19, 0, 0, 0, 0); // 1716044400000
  Date.UTC('1970/1/2') // NaN
  ```

## `30.2.4` Date.prototype.getFullYear()
- Date 객체의 연도를 나타내는 정수값을 반환.
  
  ```js
  new Date('2024/06/19').getFullYear(); // 2024
  ```

## `30.2.5` Date.prototype.setFullYear()
- Date 객체의 연도를 나타내는 정수값을 설정.
- 연도 이외에도 월, 일을 설정할 수 있다.
  
  ```js
    const today = new Date();

    //년도 지정
    today.setFullYear(2024);
    today.getFullYear(); // 2024

    //년도, 월, 일 지정
    today.setFullYear(2024, 4, 19);
    today.getFullYear(); // 2024
  ```

## `30.2.6` Date.prototype.getMonth()
- Date 객체의 월을 나타내는 0~11 정수값을 반환.
- 1월은 0, 12월은 11로 반환. (-1)

  ```js
  new Date('2024/06/19').getMonth(); // 5
  ```

## `30.2.7` Date.prototype.setMonth()
- Date 객체의 월을 나타내는 0~11 정수값을 설정.
- 월 이외에도 일을 설정할 수 있다.

  ```js
  const today = new Date();

  //월 지정
  today.setMonth(4); // 5월
  today.getMonth();  // 4

  //월, 일 지정
  today.setMonth(4, 19); // 5월 19일
  today.getMonth();      // 4
  ```

## `30.2.8` Date.prototype.getDate()
- Date 객체의 일을 나타내는 1~31 정수값을 반환.

  ```js
  new Date('2024/06/19').getDate(); // 19
  ```

## `30.2.9` Date.prototype.setDate()
- Date 객체의 일을 나타내는 1~31 정수값을 설정.

  ```js
  const today = new Date();

  //일 지정
  today.setDate(19);
  today.getDate(); // 19
  ```

## `30.2.10` Date.prototype.getDay()
- Date 객체의 요일을 나타내는 0~6 정수값을 반환.
- 0은 일요일, 6은 토요일을 나타낸다.

  ```js
  // 0: 일요일, 1: 월요일, 2: 화요일, 3: 수요일, 4: 목요일, 5: 금요일, 6: 토요일
  new Date('2024/06/19').getDay(); // 0 : 일요일
  ``

## `30.2.11` Date.prototype.getHours()
- Date 객체의 시간을 나타내는 0~23 정수값을 반환.

  ```js
  new Date('2024/06/19/10:00:00').getHours(); // 10
  ```

## `30.2.12` Date.prototype.setHours()
- Date 객체의 시간을 나타내는 0~23 정수값을 설정.
- 시간 이외에도 분, 초, 밀리초를 설정할 수 있다.

  ```js
  const today = new Date();

  //시간 지정
  today.setHours(10);
  today.getHours(); // 10

  //시간, 분, 초, 밀리초 지정
  today.setHours(10, 0, 0, 0); // 10시 0분 0초 0밀리초
  today.getHours(); // 10
  ```

## `30.2.19` Date.prototype.getTime()
- Date 객체의 시간을 나타내는 밀리초를 반환.

  ```js
  new Date('2024/06/19/10:00:00').getTime(); // 1716079059634
  ```

## `30.2.20` Date.prototype.setTime()
- Date 객체의 시간을 나타내는 밀리초를 설정.

  ```js
  const today = new Date();

  today.setTime(1716079059634);
  today.getTime(); // 1716079059634
  ```

## `30.2.21` Date.prototype.getTimezoneOffset()
- 현재 시간과 표준 시간과의 차이를 분 단위로 반환.
- 한국은 UTC+9이므로 -9h * 60m = -540m

  ```js
  new Date().getTimezoneOffset(); // -540
  new Data().getTimezoneOffset() / 60; // -9
  ```

## `30.2.22` Date.prototype.toDateString()
- Date 객체의 날짜 부분을 나타내는 문자열을 반환.

  ```js
  const today = new Date('2024/06/19/10:00:00')
  today.toDateString(); // 'Sun May 19 2024'
  ```

## `30.2.23` Date.prototype.toTimeString()
- Date 객체의 시간 부분을 나타내는 문자열을 반환.

  ```js
  const today = new Date('2024/06/19/10:00:00')
  today.toTimeString(); // '10:00:00 GMT+0900 (대한민국 표준시)'
  ```

## `30.2.24` Date.prototype.toISOString()
- Date 객체를 ISO 8601 형식의 문자열로 반환.

  ```js
  const today = new Date('2024/06/19/10:00:00')
  today.toISOString(); // '2024-05-19T01:00:00.000Z'

  today.toISOString().slice(0, 10); // '2024-05-19'
  today.toISOString().slice(0, 10).replace(/-/g, ''); // '20240519'
  ```

## `30.2.25` Date.prototype.toLocaleString()
- Date 객체를 로컬 시간 기준의 문자열로 반환.

  ```js
  const today = new Date('2024/06/19/10:00:00')
  today.toLocaleString(); // '2024-05-19 10:00:00'
  today.toLocaleString('ko-KR'); // '2024. 5. 19. 오전 10:00:00'
  today.toLocaleString('en-US'); // '5/19/2024, 10:00:00 AM'
  today.toLocaleString('ja-JP'); // '2024/05/19 10:00:00'
  ```

## `30.2.26` Date.prototype.toLocalTimeString()
- Date 객체를 로컬 시간 기준의 시간 문자열로 반환.

  ```js
  const today = new Date('2024/06/19/10:00:00')
  today.toLocalTimeString(); // '10:00:00'
  today.toLocalTimeString('ko-KR'); // '오전 10:00:00'
  today.toLocalTimeString('en-US'); // '10:00:00 AM'
  today.toLocalTimeString('ja-JP'); // '10:00:00'
  ```

  # `30.3` Date를 활용한 시계 예제
  ```js
  (function printNow() {
    const today = new Date();

    const dayNames = [
      '(일요일)',
      '(월요일)',
      '(화요일)',
      '(수요일)',
      '(목요일)',
      '(금요일)',
      '(토요일)'
    ];

    // getDay 메서드는 해당 요일(0~6)을 나타내는 정수를 반환.
    const day = dayNames[today.getDay()];

    const year = today.getFullYear();
    const month = today.getMonth() + 1;
    const date = today.getDate();
    let hour = today.getHours();
    let minute = today.getMinutes();
    let second = today.getSeconds();
    const ampm = hour >= 12 ? 'PM' : 'AM';

    // 12시간제로 변경
    hour %= 12;
    hour = hour || 12; // hour가 0이면 12를 재할당

    // 10 미만인 분과 초를 2자리로 변경
    minute = minute < 10 ? `0${minute}` : minute;
    second = second < 10 ? `0${second}` : second;

    // 출력
    const now = `${year}년 ${month}월 ${date}일 ${day} ${hour}:${minute}:${second} ${ampm}`;
    console.log(now);

    // 1초마다 printNow함수를 재귀 호출
    setTimeout(printNow, 1000);
  }());
  ```