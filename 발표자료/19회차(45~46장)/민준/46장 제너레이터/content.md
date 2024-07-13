## 46장 제너레이터 와 async / await
제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수

특징
1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도
2. 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을수 있다.
3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환

### 제너레이터 함수의 정의
- function* 키워드로 선언
- 하나 이상의 yield 표현식을 포함
```js
function* getFunc() { yield 1; }
```

### 제너레이터 객체
제너레이터 함수를 호출시 일반 함수처럼 함수 코드 블록을 실행하는 것이 아닌
제너레이터 객체를 생성해 반환
<br>
제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터.

### 제너레이터의 일시 중지와 재개
yield 키워드와 next 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있다.
제네레이터 객체는 next 메서드를 갖는다 <br>
제너레이터 객체의 next 메서드를 호출하면 제너레이터 함수의 코드 블록을 실행한다 <br>
일반 함수처럼 한 번에 코드 블록의 모든 코드를 일괄 실행하는 것이 아니라 yield 표현식까지만 실행

이터레이터의 next 메서드와 달리 제너레이터 객체의 next 메서드에는 인수를 전달할 수 있다.

### 제너레이터의 활용
- 비동기 처리
  - 제네레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다.
  - 이러한 특성을 활용하면 프로미스를 사용한 비동기 처리를 동기 처리처럼 구현할 수 있다.

## async/await 
async/await는 프로미스를 기반으로 동작하며, async/await를 사용하면 프로미스의 then/catch/finally 후속 처리 메서드에 콜백 함수를 전달해서 비동기 처리 결과를 후속 처리할 필요 없이 마치 동기 처리처럼 프로미스를 사용할 수 있다.

### async
1. await 키워드는 반드시 async 함수 내부에서 사용해야 한다.
2. async 함수는 async 키워드를 사용해 정의하며 언제나 프로미스를 반환한다.
3. async 함수가 명시적으로 프로미스를 반환하지 않더라도 async 함수는 암묵적으로 반환값을 resolve 하는 프로미스를 반환한다

### await
- await 키워드는 프로미스가 settled 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과 반환한다.
- 비동기 처리의 처리 순서가 보장되어야 하는 경우 await 키워드를 써서 순차적으로 처리

### 에러처리
async/await에서 에러 처리는 try...catch 문을 사용할 수 있다. 
콜백 함수를 인수로 전달받는 비동기 함수와는 달리 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 호출자가 명확하다.

### allSettled , try/catch  예외 처리 예시
```js
try {
    const data = (await initCommon.ajax(posturl, "post")).result;
    const operations = [
        {name: 'initAirSituation', promise: initAirSituation(data.air)},
        {name: 'initWeatherSituation', promise: initWeatherSituation(data.weather, data.maxweather)},
        {name: 'initRainFallSituation', promise: initRainFallSituation(data.rainfall)},
        {name: 'initSatellite', promise: initSatellite()},
        {name: 'initWeatherWarning', promise: initWeatherWarning(data.warning)},
        {name: 'initMesureDnsty', promise: initMesureDnsty(data.mesureDnsty)},
    ];

    const results = await Promise.allSettled(operations.map(op => op.promise));

    results.forEach((result, index) => {
        if (result.status !== 'fulfilled') {
            console.error(`${operations[index].name} data operation failed:`, result.reason);
        }
    });

    if (results.every(result => result.status === 'fulfilled')) {
        sateliteInterval = setInterval(initSatellite, 15000);
    }
} catch (error) {
    console.error('An error occurred:', error);
}
```
