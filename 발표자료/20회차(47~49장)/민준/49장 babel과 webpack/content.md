## 49장 Babel과 Webpack을 이용한 ES6+/ES.NEXT 개발 환경 구축

### babel
Babel은 ES6+와 ES.NEXT로 구현된 최신 사양의 소스코드를 <br>
구형 브라우저에서도 동작하는 ES5 사양의 소스코드로 변환(트랜스파일링)할 수 있다.

### webpack
Webpack은 의존 관계에 있는 자바스크립트, CSS, 이미지 등의 리소스들을 하나의 파일로 번들링하는 모듈 번들러

### babel-polyfill
- babel을 사용해 소스코드를 트랜스파일링해도 브라우저가 지원하지 않는 코드가 남아 있을 수 있다.
- Promise, Object.assign 등과 같이 ES5 사양으로 대체할 수 없는 기능은 트랜스파일링되지 않는다.
- 구형 브라우저에서도 Promise, Object.assign 등과 같은 객체나 메서드를 사용하기 위해서는 @babel-polyfill을 설치해야 한다.