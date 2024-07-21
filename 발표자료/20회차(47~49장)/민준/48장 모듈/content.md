## 48장 모듈

### 정의
모듈이란 애플리케이션을 구성하는 개별적 요소로서 재사용 가능한 코드 조각

### js에서의 모듈
자바스크립트는 모듈이 성립하기 위해 필요한 파일 스코프와 import, export를 지원하지 않았다.
자바스크립트는 파일마다 독립적인 파일 스코프를 갖지 않는다. <br>

<br>
이런 상황에서 모듈을 사용하기 위해 제안된 것이 CommonJS와 AMD다.

이로써 자바스크립트의 모듈 시스템은 크게 CommonJS와 AMD로 나뉘게 되었고 브라우저 환경에서 모듈을 사용하기 위해서는 CommonJS 또는 AMD를 구현한 모듈 로더 라이브러리를 사용해야 하는 상황이 되었다.
<br>
자바스크립트의 런타임 환경인 Node.js는 모듈 시스템의 사실상 표준인 CommonJS를 채택했다.
<br>
Node.js 환경에서는 파일별로 독립적인 파일 스코프를 갖는다.

### 모듈 스코프
ESM은 독자적인 모듈 스코프를 갖는다. <br>
script 태그에 type="module" 어트리뷰트를 추가하면 로드된 자바스크립트 파일은 모듈로서 동작한 다.
```js
// module1.js
export const name = "John";
export function greet() {
  console.log("Hello, " + name);
}
export class Person {
  constructor(name) {
    this.name = name;
  }
  sayHello() {
    console.log("Hello, my name is " + this.name);
  }
}

```
### export 키워드
모듈 내부에서 선언한 식별자를 외부에 공개하여 다른 모듈들이 재사용할 수 있게 하려면 export 키워드를 사용한다.
```js
// module2.js
export default function() {
  console.log("This is the default export");
}

// main.js
import defaultFunction from './module2.js';

defaultFunction(); // This is the default export

```
###  import 키워드
다른 모듈에서 공개한 식별자를 자신의 모듈 스코프 내부로 로드하려면 import 키워드를 사용한다.
```js
// main.js
import { name, greet, Person } from './module1.js';

console.log(name); // John
greet(); // Hello, John

const person = new Person('Jane');
person.sayHello(); // Hello, my name is Jane

```