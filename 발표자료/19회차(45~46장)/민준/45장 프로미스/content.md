## 45장 프로미스

자바스크립트에서는 비동기 처리를 하기 위한 하나의 패턴으로 콜백 함수를 사용한다.

그러나 콜백 패턴은 다음과 같은 단점이 있다.

1. 콜백 헬
2. 에러 처리
3. 여러 개의 비동기 처리 단일화 처리
따라서 ES6에서는 비동기 처리를 위한 프로미스를 도입했다.

### 비동기 처리를 위한 콜백 패턴의 단점
45.1.1 콜백 헬
이처럼 콜백 함수를 통해 비동기 처리 결과에 대한 후속 처리를 수행하는 비동기 함수가 비동기 처리 결과를 가지고
또다시 비동기 함수를 호출해야 한다면 콜백 함수 호출이 중첩되어 복잡도가 높아지는 현상이 발생하는데, 이를 콜백 헬이라 한다.

### 프로미스의 후속 처리 메서드

1. Promise.prototype.then
- then 메서드는 2개의 콜백 함수를 인수로 전달받는다.
- fulfilled 상태일 때 호출되는 콜백 함수 -> 성공 처리
- rejected 상태일 때 호출되는 콜백 함수 -> 실패 처리

2. Promise.prototype.catch
- catch 메서드는 1개의 콜백 함수를 인수로 전달받는다.
- catch 메서드의 콜백 함수는 프로미스가 rejected 상태인 경우에만 호출된다.

3. Promise.prototype.finally
- finally 메서드는 1개의 콜백 함수를 인수로 전달받는다.
- finally 메서드의 콜백 함수는 프로미스의 성공 또는 실패와 상관없이 무조건 1번 호출된다.
- 
### Promise.allSettled vs Promise.all
- Promise.allSettled
Promise.allSettled는 전달된 모든 프로미스가 완료(fulfilled 또는 rejected)될 때까지 대기. 모든 프로미스가 완료된 후, 각 프로미스의 상태와 결과를 포함하는 객체 배열을 반환.

- Promise.all 과의 차이
  - Promise.allSettled: 모든 프로미스가 완료될 때까지 대기. 각 프로미스의 결과와 상태를 반환.
  - Promise.all: 모든 프로미스가 성공할 때까지 대기. 하나라도 실패하면 즉시 실패 이유를 반환하고 실행을 중단합니다.

- 사용
Promise.allSettled는 여러 비동기 작업의 개별 결과를 모두 확인하고 처리해야 할 때
Promise.all은 모든 비동기 작업이 성공해야만 할때
잘못된 사용
두 예제는 다른 동작 방식을 가진다

첫번째 예시
```js
  const results = await Promise.allSettled([
    await getWeather(),
    await getWeatherWarning(),
  ]);
```
- 함수들을 순차적으로 실행합니다.
- getWeather()가 완료된 후 getWeatherWarning()가 실행됩니다.
- 순차 실행합니다.
- 병렬 실행의 이점을 활용하지 않습니다.

두번째 예시
```js
  const results = await Promise.allSettled([
    getWeather(),
    getWeatherWarning(),
  ]);
```
- 함수들을 병렬로 실행합니다.
- getWeather()와 getWeatherWarning()가 동시에 실행되므로, 더 빠르게 완료될 수 있습니다.
- 비동기 작업의 병렬 실행을 활용합니다.
- 병렬 실행과 순차 실행의 장단점을 고려하여 Promise.allSettled를 사용해야 함

### 프로미스 체이닝
then, catch, finally 후속 처리 메서드는 언제나 프로미스를 반환하므로 연속적으로 호출할 수 있는데, 
이를 프로미스 체이닝이라 한다.

### 프로미스의 정적 메서드
1. Promise.resolve / Promise.reject
- 이미 존재하는 값을 래핑하여 프로미스를 생성
2. Promise.all
- 여러 개의 비동기 처리를 모두 병렬 처리
3. Promise.race
- Promise.all 처럼 모든 프로미스가 fulfilled 상태가 되는 것을 기다리는 것이 아니라 가장 먼저
  fulfilled 상태가 된 프로미스의 처리 결과를 resolve하는 새로운 프로미스를 반환한다.
4. Promise.allSettled
- 전달받은 프로미스가 모두 settled 상태가 되면 처리 결과를 배열로 반환

### 마이크로태스트 큐
이벤트 루프 실행 순서
1. 이벤트 루프는 콜 스택이 비면 먼저 마이크로태스크 큐에서 대기하고 있는 함수를 가져와 실행한다. 
2. 이후 마이크로태스크 큐가 비면 태스크 큐에서 대기하고 있는 함수를 가져와 실행

### fetch
fetch 함수는 XMLHttpRequest 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API