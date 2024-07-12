# `46.1` 제너레이터란?

- 제너레이터란 코드 블록의 실행을 일시 중지했다가 재개할 수 있는 함수.

  > 1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
  > 2. 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.
  > 3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.(이터러블 + 이터레이터)

# `46.2` 제너레이터 함수의 정의

- `function*` 키워드로 선언하며 하나 이상의 `yield` 표현식을 포함.
- 제너레이터 함수는 화살표 함수로 정의 불가능.
- 제너레이터 함수는 new 연산자와 함께 생성자 함수 호출 불가능.

  ```javascript
  // 제너레이터 함수 선언문
  function* genDecFunc() {
    yield 1;
  }

  // 제너레이터 함수 표현식
  const genExpFunc = function* () {
    yield 1;
  };

  // 제너레이터 메서드
  const obj = {
    *genObjMethod() {
      yield 1;
    },
  };

  // 제너레이터 클래스 메서드
  class MyClass {
    *genClsMethod() {
      yield 1;
    }
  }
  ```

# `46.3` 제너레이터 객체

- 제너레이터 함수를 호출하면 일반 함수처럼 함수코드블럭을 실행하는 것이 아니라 제너레이터 객체를 생성해서 반환.
- 제너레이터 객체는 이터러블이면서 이터레이터이다.

  ```javascript
  // 제너레이터 함수
  function* genFunc() {
    yield 1;
    yield 2;
    yield 3;
  }

  // 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
  const generator = genFunc();

  // 제너레이터 객체는 이터러블이면서 동시에 이터레이터.
  console.log(Symbol.iterator in generator); // true

  // next : yield 표현식까지 코드 블록을 실행하고 yield된 값을 반환.
  // {value: 1, done: false}
  console.log(generator.next());

  // return : 인수로 전달받은 값을 value로, true를 done으로 갖는 iterator result 객체 반환.
  // {value: "End!", done: true}
  console.log(generator.return('End!'));

  // throw : 제너레이터 함수 내부에서 예외를 던지면 제너레이터 객체의 throw 메서드 호출.
  // {value: undefined, done: true}
  console.log(generator.throw('Error!'));
  ```

  ![generator](./assets/generator.png)

<br/>
<br/>
<br/>

# `46.4` 제너레이터의 일시 중지와 재개

- 제너레이터는 `yeild` 키워드와 `next` 메서드를 통해 실행을 일시 중지했다가 재개할 수 있음.
- 일반 함수는 호출 이후 제어권을 함수가 독점, 제너레이터 함수는 호출자에게 제어권을 양도(yield)
- `yield` 키워드는 제너레이터의 함수 실행을 중지시키거나 값을 반환.

  ```javascript
  function* genFunc() {
    // 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
    // 이때 yield된 값 1은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
    // x 변수에는 아직 아무것도 할당되지 않았다. x 변수의 값은 next 메서드가 두 번째 호출될 때 결정된다.
    const x = yield 1;

    // 두 번째 next 메서드를 호출할 때 전달한 인수 10은 첫 번째 yield 표현식을 할당받는 x 변수에 할당된다.
    // 즉, const x = yield 1;은 두 번째 next 메서드를 호출했을 때 완료된다.
    // 두 번째 next 메서드를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지된다.
    // 이때 yield된 값 x + 10은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
    const y = yield x + 10;

    // 세 번째 next 메서드를 호출할 때 전달한 인수 20은 두 번째 yield 표현식을 할당받는 y 변수에 할당된다.
    // 즉, const y = yield (x + 10);는 세 번째 next 메서드를 호출했을 때 완료된다.
    // 세 번째 next 메서드를 호출하면 함수 끝까지 실행된다.
    // 이때 제너레이터 함수의 반환값 x + y는 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
    // 일반적으로 제너레이터의 반환값은 의미가 없다.
    // 따라서 제너레이터에서는 값을 반환할 필요가 없고 return은 종료의 의미로만 사용해야 한다.
    return x + y;
  }

  // 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
  // 이터러블이며 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖는다.
  const generator = genFunc(0);

  // 처음 호출하는 next 메서드에는 인수를 전달하지 않는다.
  // 만약 처음 호출하는 next 메서드에 인수를 전달하면 무시된다.
  // next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 첫 번째 yield된 값 1이 할당된다.
  let res = generator.next();
  console.log(res); // {value: 1, done: false}

  // next 메서드에 인수로 전달한 10은 genFunc 함수의 x 변수에 할당된다.
  // next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 두 번째 yield된 값 20이 할당된다.
  res = generator.next(10);
  console.log(res); // {value: 20, done: false}

  // next 메서드에 인수로 전달한 20은 genFunc 함수의 y 변수에 할당된다.
  // next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 제너레이터 함수의 반환값 30이 할당된다.
  res = generator.next(20);
  console.log(res); // {value: 30, done: true}
  ```

<br/>
<br/>

# `46.5` 제너레이터의 활용

## `46.5.1` 이터러블의 구현

- 이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 간단히 이터러블 구현
- 제너레이터 함수를 사용해 이터러블을 생성하면 이터러블을 생성하는 별도의 이터러블 객체를 생성할 필요 없음.

  ```javascript
  // 무한 이터러블을 생성하는 함수
  const infiniteFibonacci = (function () {
    let [pre, cur] = [0, 1];

    return {
      [Symbol.iterator]() {
        return this;
      },
      next() {
        [pre, cur] = [cur, pre + cur];
        // 무한 이터러블이므로 done 프로퍼티를 생략한다.
        return { value: cur };
      },
    };
  })();

  // infiniteFibonacci는 무한 이터러블이다.
  for (const num of infiniteFibonacci) {
    if (num > 10000) break;
    console.log(num); // 1 2 3 5 8...2584 4181 6765
  }
  ```

  ```javascript
  // 무한 이터러블을 생성하는 제너레이터 함수
  const infiniteFibonacci = (function* () {
    let [pre, cur] = [0, 1];

    while (true) {
      [pre, cur] = [cur, pre + cur];
      yield cur;
    }
  })();

  // infiniteFibonacci는 무한 이터러블이다.
  for (const num of infiniteFibonacci) {
    if (num > 10000) break;
    console.log(num); // 1 2 3 5 8...2584 4181 6765
  }
  ```

## `46.5.2` 비동기 처리

- 제너레이터 함수는 비동기 처리를 동기 처리처럼 구현 가능.
- 후속처리 메서드 `then/catch/finally` 없이 비동기 처리결과를 반환.

  ```javascript
  const async = (generatorFunc) => {
    const generator = generatorFunc(); // ②

    const onResolved = (arg) => {
      const result = generator.next(arg); // ⑤

      return result.done
        ? result.value // ⑨
        : result.value.then((res) => onResolved(res)); // ⑦
    };

    return onResolved; // ③
  };

  async(function* fetchTodo() {
    // ①
    const url = 'https://jsonplaceholder.typicode.com/todos/1';

    const response = yield fetch(url); // ⑥
    const todo = yield response.json(); // ⑧
    console.log(todo);
  })(); // ④
  ```

<br/>
<br/>

# `46.6` async/await

- `async/await`는 제너레이터의 단점을 보완하고 비동기 처리를 동기 처리처럼 구현.
- 프로미스를 기반으로 동작하며 `async` 함수는 프로미스를 반환.

  ```javascript
  async function fetchTodo() {
    const url = 'https://jsonplaceholder.typicode.com/todos/1';

    const response = await fetch(url);
    const todo = await response.json();
    console.log(todo);
    // {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
  }

  fetchTodo();
  ```

  <br/>

## `46.6.1` async 함수

- await 키워드는 반드시 async 함수 내부에서 사용.
- async 함수는 암묵적으로 반환 값을 resolve하는 프로미스를 반환.

  ```javascript
  // async 함수 선언문
  async function foo(n) {
    return n;
  }
  foo(1).then((v) => console.log(v)); // 1

  // async 함수 표현식
  const bar = async function (n) {
    return n;
  };
  bar(2).then((v) => console.log(v)); // 2

  // async 화살표 함수
  const baz = async (n) => n;
  baz(3).then((v) => console.log(v)); // 3

  // async 메서드
  const obj = {
    async foo(n) {
      return n;
    },
  };
  obj.foo(4).then((v) => console.log(v)); // 4

  // async 클래스 메서드
  // class의 constructor 메서드는 async 함수로 정의 불가능.(언제나 promise를 반환하기 때문)
  class MyClass {
    async bar(n) {
      return n;
    }
  }
  const myClass = new MyClass();
  myClass.bar(5).then((v) => console.log(v)); // 5
  ```

<br/>

## `46.6.2` await 키워드

- await 키워드는 프로미스가 settled 상태가 될 때까지 기다린 후 resolve된 값으로 평가.
- await 키워드는 async 함수 내부에서만 사용 가능.
  <br/>
  <br/>

  ### ✅ await 키워드의 사용

  ```javascript
  const getGithubUserName = async (id) => {
    const res = await fetch(`https://api.github.com/users/${id}`); // ①
    const { name } = await res.json(); // ②
    console.log(name); // Ungmo Lee
  };

  getGithubUserName('ungmo2');
  ```

  <br/>

  ### ✅ 프로미스가 sattled 상태가 되면 다시 재개하는 await 키워드

  ```javascript
  // 비동기적으로 병렬 실행되는 foo 함수
  async function foo() {
    const res = await Promise.all([
      new Promise((resolve) => setTimeout(() => resolve(1), 3000)),
      new Promise((resolve) => setTimeout(() => resolve(2), 2000)),
      new Promise((resolve) => setTimeout(() => resolve(3), 1000)),
    ]);

    console.log(res); // [1, 2, 3]
    foo(); // 약 3초 소요.
  }
  ```

  <br/>

  ### ✅ await 키워드 순차 처리 예시

  ```javascript
  async function bar(n) {
    const a = await new Promise((resolve) => setTimeout(() => resolve(n), 3000));
    // 두 번째 비동기 처리를 수행하려면 첫 번째 비동기 처리 결과가 필요하다.
    const b = await new Promise((resolve) => setTimeout(() => resolve(a + 1), 2000));
    // 세 번째 비동기 처리를 수행하려면 두 번째 비동기 처리 결과가 필요하다.
    const c = await new Promise((resolve) => setTimeout(() => resolve(b + 1), 1000));

    console.log([a, b, c]); // [1, 2, 3]
  }

  bar(1); // 약 6초 소요.
  ```

## `46.6.3` 에러 처리

- async 함수에서 발생한 에러는 async 함수를 호출한 쪽에서 `try...catch` 문으로 처리 가능.

  ```javascript
  const foo = async () => {
    try {
      const wrongUrl = 'https://wrong.url';

      const response = await fetch(wrongUrl);
      const data = await response.json();
      console.log(data);
    } catch (err) {
      console.error(err); // TypeError: Failed to fetch
    }
  };

  foo();
  ```

- async 함수 내에서 catch 문을 사용해서 에러처리를 하지 않으면 async 함수는 에러를 reject하는 프로미스를 반환함.

  ```javascript
  const fetch = require('node-fetch');

  const foo = async () => {
    const wrongUrl = 'https://wrong.url';

    const response = await fetch(wrongUrl);
    const data = await response.json();
    return data;
  };

  foo().then(console.log).catch(console.error); // TypeError: Failed to fetch
  ```
