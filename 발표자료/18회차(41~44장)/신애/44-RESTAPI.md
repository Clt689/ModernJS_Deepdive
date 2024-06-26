# `44.1` REST API의 구성
- REST API는 자원(RESOURCE), 행위(Verb), 표현(Representations)의 3가지 요소로 구성된다.

| 구성 요소 | 내용 | 표현 방법 |
| --- | --- | --- |
| 자원 | 자원의 구분은 URI로 표현. | URI |
| 행위 | HTTP 메서드로 표현. | HTTP 요청 메서드 `GET` `POST` `PUT` `DELETE` |
| 표현 | 자원에 대한 행위의 결과를 응답으로 보낸다. | 페이로드 |


<br/>
<br/>

# `44.2` REST API의 설계 원칙
- URI는 리소스를 표현하는 데 집중
- 행위에 대한 정의는 HTTP 요청 메서드로 표현
- GET(조회), POST(생성), PUT(전체 수정), DELETE(삭제)

  ### 1. URI는 리소스를 표현해야 한다.
  ``` js
  // 리소스를 식별할 수 있는 이름은 동사보다는 명사를 사용.

  //bad
  GET /getTodos/1
  GET /todos/show/1

  //good
  GET /todos/1
  ```

  ### 2. 자원에 대한 행위는 HTTP 요청 메서드로 표현
  ``` js
  // 리소스를 취득하는 경우 GET, 리소스를 생성하는 경우 POST, 리소스를 수정하는 경우 PUT, 리소스를 삭제하는 경우 DELETE

  //bad
  GET /todos/delete/1

  //good
  DELETE /todos/1
  ```

<br/>
<br/>

# `44.3` JSON Server를 이용한 REST API 실습
  ### `44.3.4` GET 요청
  ```html
  <!DOCTYPE html>
  <html>
  <body>
    <pre></pre>
    <script>
      // XMLHttpRequest 객체 생성
      const xhr = new XMLHttpRequest();

      // HTTP 요청 초기화
      // todos 리소스에서 모든 todo를 취득(index) ⭐️
      xhr.open('GET', '/todos');

      // todos 리소스에서 id를 사용하여 특정 todo를 취득(retrieve) ⭐️
      xhr.open('GET', '/todos/1');

      // HTTP 요청 전송
      xhr.send();

      // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
      xhr.onload = () => {
        // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
        if (xhr.status === 200) {
          document.querySelector('pre').textContent = xhr.response;
        } else {
          console.error('Error', xhr.status, xhr.statusText);
        }
      };
    </script>
  </body>
  </html>
  ```

  <br/>

  ### `44.3.5` POST 요청

  ```html
  <!DOCTYPE html>
  <html>
  <body>
    <pre></pre>
    <script>
      // XMLHttpRequest 객체 생성
      const xhr = new XMLHttpRequest();

      // HTTP 요청 초기화
      // todos 리소스에 새로운 todo를 생성
      xhr.open('POST', '/todos');

      // 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정 ⭐️
      xhr.setRequestHeader('content-type', 'application/json');

      // HTTP 요청 전송
      // 새로운 todo를 생성하기 위해 페이로드를 서버에 전송해야 한다.
      xhr.send(JSON.stringify({ id: 4, content: 'Angular', completed: false }));

      // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
      xhr.onload = () => {
        // status 프로퍼티 값이 200(OK) 또는 201(Created)이면 정상적으로 응답된 상태다.
        if (xhr.status === 200 || xhr.status === 201) {
          document.querySelector('pre').textContent = xhr.response;
        } else {
          console.error('Error', xhr.status, xhr.statusText);
        }
      };
    </script>
  </body>
  </html>
  ```

  <br/>

  ### `44.3.6` PUT 요청

  ```html
  <!DOCTYPE html>
  <html>
  <body>
    <pre></pre>
    <script>
      // XMLHttpRequest 객체 생성
      const xhr = new XMLHttpRequest();

      // HTTP 요청 초기화
      // todos 리소스에서 id로 todo를 특정하여 id를 제외한 리소스 전체를 교체 ⭐️
      xhr.open('PUT', '/todos/4');

      // 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정
      xhr.setRequestHeader('content-type', 'application/json');

      // HTTP 요청 전송
      // 리소스 전체를 교체하기 위해 페이로드를 서버에 전송해야 한다.
      xhr.send(JSON.stringify({ id: 4, content: 'React', completed: true }));

      // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
      xhr.onload = () => {
        // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
        if (xhr.status === 200) {
          document.querySelector('pre').textContent = xhr.response;
        } else {
          console.error('Error', xhr.status, xhr.statusText);
        }
      };
    </script>
  </body>
  </html>
  ```

  <br/>

  ### `44.3.7` PATCH 요청

  ```html
  <!DOCTYPE html>
  <html>
  <body>
    <pre></pre>
    <script>
      // XMLHttpRequest 객체 생성
      const xhr = new XMLHttpRequest();

      // HTTP 요청 초기화
      // todos 리소스의 id로 todo를 특정하여 completed만 수정 ⭐️
      xhr.open('PATCH', '/todos/4');

      // 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정 ⭐️
      xhr.setRequestHeader('content-type', 'application/json');

      // HTTP 요청 전송
      // 리소스를 수정하기 위해 페이로드를 서버에 전송해야 한다.
      xhr.send(JSON.stringify({ completed: false }));

      // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
      xhr.onload = () => {
        // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
        if (xhr.status === 200) {
          document.querySelector('pre').textContent = xhr.response;
        } else {
          console.error('Error', xhr.status, xhr.statusText);
        }
      };
    </script>
  </body>
  </html>
  ```

  <br/>

  ### `44.3.8` DELETE 요청

  ```html
  <!DOCTYPE html>
  <html>
  <body>
    <pre></pre>
    <script>
      // XMLHttpRequest 객체 생성
      const xhr = new XMLHttpRequest();

      // HTTP 요청 초기화
      // todos 리소스에서 id를 사용하여 todo를 삭제한다. ⭐️
      xhr.open('DELETE', '/todos/4');

      // HTTP 요청 전송
      xhr.send();

      // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
      xhr.onload = () => {
        // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
        if (xhr.status === 200) {
          document.querySelector('pre').textContent = xhr.response;
        } else {
          console.error('Error', xhr.status, xhr.statusText);
        }
      };
    </script>
  </body>
  </html>
  ```
