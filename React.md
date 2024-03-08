# React

- class vs function
  - 두개 다 가능하지만 이 강의에선 함수로 진행

## 1. 실습환경구축

- 우선 app으로 만들어서 시작하겠다.

```bash
npx create-react-app {내앱의 이름}
```

- 이름 지을때, react가 들어가지 않게 하자

```bash
cd {내앱의 이름}
```

- 해당 디렉토리로 들어가야함

```bash
npm start
```

- 생성한 프로젝트로 들어가서 start를 하면 예시 브라우저가 열림

## 2. 소스코드 수정방법

- index.js 파일이 메인이 되는 페이지이다.
- 페이지를 살펴보면 App을 가져와서 출력하고 있다.
- App.js를 들어가보면 App이라는 함수가 return 하고 있는 것들이 보여지고 있다.

### CSS

- CSS는 따로 .css파일을 만들어서, import하면 적용이 됨

### npm run build

```bash
npm run build
```

- 위의 명령어를 입력하면, 오직 배포만을 위한 build 파일이 만들어진다.

- 해당 파일 내에 index.html이 있는데 해당 파일이 배포할 파일이 되며, 공백하나 없이 빡빡하게 내용이 작성되어 있는데 이는, 공백조차 줄여서 효율적으로 배포하기 위함이다.

- 이 build 파일로 배포를 시작하려면

  ```bash
  npx serve -s build
  ```

  를 입력해야한다.

  - 추가사항!
    - 위 문법에서 -s 를 쓴다면, -s 뒤에 따라오는 폴더내에 있는 index.html로 연결해준다.



## 4. 컴포넌트만들기

- 리액트는 사용자정의 태그를 만드는 것이 핵심 기술이다!

  이를 기억해두어야한다.

### function (함수) 선언하기

- 리액트에서 사용자정의 태그를 만들기 위해서는 함수로 선언해야한다.

  ```react
  // App.js
  // 함수 선언시 반드시 첫 시작은 대문자로 시작
  function Header() {
    return (
      <header>
        <h1>
          <a href="/">WEB</a>
        </h1>
      </header>
    );
  }
  ```

  - 함수를 선언하고 return 값에 html코드를 넣어 사용하면 된다.

- 그 후, 화면에 표출되는 App 함수안에 호출한다

  ```react
  ...
  <div>
  	<Header></Header>
  </div>
  ```

  - 위와 같이 호출하면, 화면에선 Header 함수에서 return하는 HTML코드를 보여준다.

- 와! 사용자정의 코드가 사실 컴포넌트였다! 야호

## 5. Props

- 만약 컴포넌트에 스타일을 인라인으로 주면 어떻게 될까?

  ```react
  function App() {
    return (
      <div>
         <!-- 이 부분 -->
        <Header title="REACT"></Header>
        <Nav></Nav>
        <Article></Article>
      </div>
    );
  }
  ```

  - 놀랍게도 이렇게 넣는다고해서 화면에 출력되는 Header의 title 값이 바뀌진 않는다.

- 그렇다면 어떻게 해야할까?

- Header 컴포넌트에 props를 인자로 받아오고, 해당 props 에서 title을 꺼내쓰면 된다고한다!

  ```react
  // 인자로 props를 받음
  function Header(props) {
      // 아래 코드를 통해 props는 객체로 들어오며 title 안에 아까 인라인으로 적었던 "REACT"가 들어있는 것을 확인 할 수 있다.
    console.log("props", props, props.title);
    return (
      <header>
        <h1>
            <!-- 중괄호롤 감싸서 호출하자! -->
          <a href="/">{props.title}</a>
        </h1>
      </header>
    );
  }
  ```

- 근데 이거 왜씀?

- 이렇게 하면 컴포넌트를 여러개 사용하지만 각 컴포넌트의 값을 다르게 하고 싶을때 props로 각 컴포넌트마다 값을 다르게 주면 구성은 같아도 출력 하는 값은 다 다르게 사용 할 수 있다.

#### Props로 받은 배열 데이터를 출력하기

```js
  const topics = [
    { id: 1, title: "html", body: "html is ..." },
    { id: 2, title: "css", body: "css is ..." },
    { id: 3, title: "javascript", body: "JS is ..." },
  ];
```

- 위와 같은 데이터를 받았다고 할 때,  해당 값들을 순서대로 출력하고자 한다.

- 우선 App 함수에 변수를 선언하자!
  ```js
  function App() {
    const topics = [
      { id: 1, title: "html", body: "html is ..." },
      { id: 2, title: "css", body: "css is ..." },
      { id: 3, title: "javascript", body: "JS is ..." },
    ];
    return (
      <div>
        <Header title="REACT"></Header>
        <Nav topics={topics}></Nav>
        <Article title="Welcome" body="Hello, WEB"></Article>
      </div>
    );
  }
  ```

  - 선언한 변수를 Nav에 Props로 주었다.

- Nav가 props를 받았으니 props를 인자로 받자

  ```react
  function Nav(props) {
    return (
      <nav>
        <ol>
          <li>
            <a href="/read/1">html</a>
          </li>
          <li>
            <a href="/read/2">css</a>
          </li>
          <li>
            <a href="/read/3">js</a>
          </li>
        </ol>
      </nav>
    );
  }
  ```

  - props로 받은 topics를 이용하여 위코드를 출력할 것이다!

- 우선 topics의 있는 데이터를 받아줄 lis를 선언하자

  ```react
  function Nav(props) {
    const lis = [];
    return (
      <nav>
        <ol>
          <li>
            <a href="/read/1">html</a>
          </li>
          <li>
            <a href="/read/2">css</a>
          </li>
          <li>
            <a href="/read/3">js</a>
          </li>
        </ol>
      </nav>
    );
  }
  ```

- 그 다음 for문을 이용하여 props로 받은 topics를 풀어서 lis에 저장하자 단, 저장할때 html 으로 작성하여 저장하여야 편하다 (주로 map을 이용한다고 하니 추후에 추가 공부해야할 듯)

  ```react
  function Nav(props) {
    const lis = [];
    for (let i = 0; i < props.topics.length; i++) {
      let t = props.topics[i];
      lis.push(
        <li key={t.id}>
          <a href={"/read/" + t.id}>{t.title}</a>
        </li>
      );
    }
    return (
      <nav>
        <ol>
          <li>
            <a href="/read/1">html</a>
          </li>
          <li>
            <a href="/read/2">css</a>
          </li>
          <li>
            <a href="/read/3">js</a>
          </li>
        </ol>
      </nav>
    );
  }
  ```

- 위 처럼 for 문을 이용해서 lis에

  ```html
  <li key={t.id}><a href={"/read/" + t.id}>{t.title}</a></li>
  ```

  라는 html 코드가 topics의 개수만큼 각데이터가 가진 정보를 담아 저장하게 된다.

- 그 후, lis에 저장한 값들을 return안에서 호출하면 기존 코드와 똑같지만 props를 이용하여 더 깔끔하게 정리할 수 있다.

  ```react
  function Nav(props) {
    const lis = [];
    for (let i = 0; i < props.topics.length; i++) {
      let t = props.topics[i];
      lis.push(
        <li key={t.id}>
          <a href={"/read/" + t.id}>{t.title}</a>
        </li>
      );
    }
    return (
      <nav>
        <ol>{lis}</ol>
      </nav>
    );
  }
  ```



## 6.이벤트

- 대표적인 이벤트인 click이벤트를 달고 싶다!

- 우선, click 했을때 실행될 함수를 만들자!
  ```react
  function App() {
    const topics = [
      { id: 1, title: "html", body: "html is ..." },
      { id: 2, title: "css", body: "css is ..." },
      { id: 3, title: "javascript", body: "JS is ..." },
    ];
    return (
      <div>
        <Header
          title="REACT"
            // 여기 추가!
          onChangeMode={() => {
            alert("Header");
          }}
        ></Header>
        <Nav
          topics={topics}
        ></Nav>
        <Article title="Welcome" body="Hello, WEB"></Article>
      </div>
    );
  }
  ```

  - header props에 onChangeMode라는 함수를 만들어 Header를 클릭하면 Header라는 alert이 뜨도록 하려고 한다!
  - 우선 props로 넘겼으니 Header 함수(컴포넌트)에서 해당 값을 이용해보자!

  ```react
  function Header(props) {
    return (
      <header>
        <h1>
          <a
            href="/"
            onClick={(event) => {
              event.preventDefault();
              props.onChangeMode();
            }}
          >
            {props.title}
          </a>
        </h1>
      </header>
    );
  }
  ```

  - a태그를 click했을때 onChangeMode함수를 실행시키고자한다 그런데, a태그는 이미 클릭했을때, "/"로 이동시키는 기능을 가지고 있다. 따라서 지금 onClick을 달아도 "/"로 이동할 것이다. 이때 쓰는 것이

    > event.preventDefault();

    를 이용하는 것이다. 이것을 이용하면 click 했을때 발생하는 일을 거부하겠다는 뜻이다. 따라서 기존 "/" 로 이동하는 기능이 실행되지않는다.

    - 코드를 보면 기존에 html 에서는

      ```html
      onclick="???"
      ```

      와 같이 적었었는데, react는 작성한 코드를 알아서 html로 바꿔주는 기능이 있기 때문에 문법이 조금 다르고, 그 다른 문법으로 써도 알아서 맞게 바꿔준다

      ```react
      onClick={(...) => {
          ...
      }}
      ```

      위와 같이 camelCase로 onClick을 호출하고 함수를 수행한다.

  - 그렇다면 onClick 안에서 props로 받은 onChangeMode를 호출하면 된다!

- 음 그렇다면 주어지는 데이터에 전부 이벤트를 달고싶으면 어떻게 해야할까?

  ```react
  ...  
  <Nav
      topics={topics}
      onChangeMode={(id) => {
        alert(id);
      }}
    ></Nav>
  ...
  ```

  - 저번에 작성했던 topics를 순회하여 3개의 a태그를 출력한  Nav컴포넌트의 각각의 a태그에 이벤트를 작성하려고한다. 우선, Nav에도 props로 onChangeMode라는 함수를 만들어 click 이벤트가 발생했을 때, 해당 함수를 호출할 수 있도록 하자!
  - 특이한 점은 해당 함수에 id라는 인자 값이 들어간다는 점이다 그 이유는 해당 id 값을 출력하기 위함이다.

  ```react
  function Nav(props) {
    const lis = [];
    for (let i = 0; i < props.topics.length; i++) {
      let t = props.topics[i];
      lis.push(
        <li key={t.id}>
          <a
            id={t.id}
            href={"/read/" + t.id}
            onClick={(event) => {
              event.preventDefault();
              props.onChangeMode(event.target.id);
            }}
          >
            {t.title}
          </a>
        </li>
      );
    }
    return (
      <nav>
        <ol>{lis}</ol>
      </nav>
    );
  }
  ```

  - 이전에 Header에 event를 등록한것과 마찬가지로 문법을 작성하였다.

    ```react
    <a
      id={t.id}
      href={"/read/" + t.id}
      onClick={(event) => {
        event.preventDefault();
        props.onChangeMode(event.target.id);
      }}
    >
    ```

    - 위 코드를 보면 기존에 Header에 들어간 코드와 다를게 없어보이지만, props로 가져온 함수에 "event.target.id" 를 인자로 넘겨주고 있다.
      - target?
        - target은 event가 발생한 주체 즉, a 태그를 뜻한다. 따라서 a 태그에 "id = {t.id}"를 적어주어 a태그의 id 값이 인자로 들어가게 된다.
    - a 태그에서 t.id 즉, for 문에서 데이터 중 해당 인덱스 값의 id가 나오게 된다.

## 7. state

- prop은 입력값이라 할 수 있으며, 이를 이용하여 리턴 값을 받는다고 할 수 있다.
- State는 무엇일까? prop과 함께 컴포넌트 함수를 다시 실행해서 값을 만들어주는 친구이다.
- 둘의 차이점은 무엇일까?
  - prop은 컴포넌트를 사용하는 외부자를 위한 데이터
  - state는 컴포넌트를 만드는 내부자를 위한 데이터

### state 사용

- 특정 버튼을 눌렀을 때, 변수의 내용이 바뀌면서 변수의 내용에 따라 if 문을 통해 출력하는 컨텐츠를 바꾼다고 한다면?

  ```react
  function App() {
    const mode = mode[0];
    const topics = [
      { id: 1, title: "html", body: "html is ..." },
      { id: 2, title: "css", body: "css is ..." },
      { id: 3, title: "javascript", body: "JS is ..." },
    ];
    let content = null;
    // 현재 모드의 값에 따라서 출력되는 콘텐츠가 달라짐
    if (mode === "WELCOME") {
      content = <Article title="Welcome" body="Hello, WEB"></Article>;
    } else if (mode === "READ") {
      content = <Article title="READ" body="Hello, Read"></Article>;
    }
    return (
      <div>
        <Header
          title="REACT"
          onChangeMode={() => {
            // 버튼을 누르면 WELCOME 으로 모드가 바뀜
            mode = 'WELCOME';
          }}
        ></Header>
        <Nav
          topics={topics}
          onChangeMode={(id) => {
            // 버튼을 누르면 READ로 모드가 바뀜
            mode = 'READ';
          }}
        ></Nav>
        {content}
        {/* <Article title="Welcome" body="Hello, WEB"></Article> */}
      </div>
    );
  }
  ```

  - 위와 같은 코드를 이용하면 버튼을 눌렀을 때, 모드값이 바뀌면서 내용이 바뀌겠지???
  - 정답은 아니다.
  - 왜???
    - 버튼을 눌렀을 때 App 이 다시 실행 되는게 아니라서 모드의 값이 바뀌어도 출력되는 내용은 바뀌지 않는것
    - 그럼 어떻게 해야 바꿀 수 있을까?
  - 이럴 때 바로 state를 이용해야 한다.

### state 사용법

- 우선 state를 사용하기 위해서는 import를 할 필요가 있다.

  ```react
  import { useState } from "react";
  ```

  - 최상단에 다음과 같이 입력하면 useState 함수를 통해 state 기능을 사용할 수 있다.
  -  현재 mode의 값에 따라서 출력되는 내용이 바뀌어야하는데, mode는 그저 평범한 지역변수이다. 이를 상태(state)로 업그레이드 시켜야한다.

  ```react
  function App() {
    // _mode가 상태값으로 선언
    const _mode = useState("WELCOME");
    // _mode 출력해보기
    // ['WELCOME', F(함수)] 가 들어 있다.
    console.log(_mode);
    // 위의 출력 결과를 2개로 나눈다. 
    const mode = mode[0];
    const setMode = mode[1];
    const topics = [
      { id: 1, title: "html", body: "html is ..." },
      { id: 2, title: "css", body: "css is ..." },
      { id: 3, title: "javascript", body: "JS is ..." },
    ];
  ...
  ```

  - 위의 mode 관련 변수들은 너무 복잡하게 선언되고 있다.

    다른 방식으로 변수들을 선언해보자.

  ```react
  function App() {
    // 위의 코드가 1줄로 정리
    const [mode, setMode] = useState("WELCOME");
    const topics = [
      { id: 1, title: "html", body: "html is ..." },
      { id: 2, title: "css", body: "css is ..." },
      { id: 3, title: "javascript", body: "JS is ..." },
    ];
    let content = null;
    if (mode === "WELCOME") {
      content = <Article title="Welcome" body="Hello, WEB"></Article>;
    } else if (mode === "READ") {
      content = <Article title="READ" body="Hello, Read"></Article>;
    }
    return (
      <div>
        <Header
          title="REACT"
          onChangeMode={() => {
            setMode("WELCOME");
          }}
        ></Header>
        <Nav
          topics={topics}
          onChangeMode={(id) => {
            // useState를 이용해서 얻은 함수를 사용하여 상태를 변경
            setMode("READ");
          }}
        ></Nav>
        {content}
        {/* <Article title="Welcome" body="Hello, WEB"></Article> */}
      </div>
    );
  }
  ```

  - 현재 위의 코드는 title 값만 바뀌고 Read로 바꾸기 위해 어떤 글을 눌렀는지 까지는 아직 알 수 없다. 그럼 id 값을 상태로 바꾸어 관리하면 출력하기가 용이할 것이다.

  ```react
  function App() {
    const [mode, setMode] = useState("WELCOME");
    const [id, setId] = useState(null);
    const topics = [
      { id: 1, title: "html", body: "html is ..." },
      { id: 2, title: "css", body: "css is ..." },
      { id: 3, title: "javascript", body: "JS is ..." },
    ];
    let content = null;
    if (mode === "WELCOME") {
      content = <Article title="Welcome" body="Hello, WEB"></Article>;
    } else if (mode === "READ") {
      // mode의 상태가 "READ"일때, 출력되는 값들을 바꾸기위해 우선 title과 body의 값을 초기화
      let title,
        body = null;
      // 가지고 있는 topics를 순회하면서 setId 를 통해 얻은 id 값과 일치하는 topics의 값을 찾아 해당 내용을 title과 body에 할당
      for (let i = 0; i < topics.length; i++) {
        console.log(topics[i].id, id);
        if (topics[i].id === id) {
          title = topics[i].title;
          body = topics[i].body;
        }
      }
      content = <Article title="READ" body="Hello, Read"></Article>;
    }
    return (
      <div>
        <Header
          title="REACT"
          onChangeMode={() => {
            setMode("WELCOME");
          }}
        ></Header>
        <Nav
          topics={topics}
          onChangeMode={(_id) => {
            setMode("READ");
            // 함수내에 인자로 들어가는 _id를 인자로하여 setId 호출
            setId(_id);
          }}
        ></Nav>
        {content}
        {/* <Article title="Welcome" body="Hello, WEB"></Article> */}
      </div>
    );
  }
  
  export default App;
  ```

  - 위코드의 주석을 확인하였을때 정상적으로 출력이 될줄 알았으나 버튼을 눌러도 안의 내용이 바뀌지 않았다

  - 왜 일까?

    - for 문안에서

      ```react
      console.log(topics[i].id, id);
      ```

      를 통해 무엇이 문제인지 확인해보자.

      ```react
      1 '2'
      ```

      음...? 보니까 토픽의 id 값은 숫자형인데 setId에서 가져온 id는 문자열이다.

  - 근본적으로 부터 돌아가보자.

    ```react
    function Nav(props) {
      const lis = [];
      for (let i = 0; i < props.topics.length; i++) {
        let t = props.topics[i];
        lis.push(
          <li key={t.id}>
            <a
              id={t.id}
              href={"/read/" + t.id}
              onClick={(event) => {
                event.preventDefault();
                // 이 부분!
                props.onChangeMode(event.target.id);
                // 이 부분!
              }}
            >
              {t.title}
            </a>
          </li>
        );
      }
      return (
        <nav>
          <ol>{lis}</ol>
        </nav>
      );
    }
    ```

    - 위의 주석으로 표시한 부분을 보면 onChangeMode 함수에 인자로 event.target.id를 주고있다. 

      target의 id는 "id = {t.id}"로 값을 할당하였다.

      이때, t.id는 숫자가 맞지만, 태그의 속성으로 넘기면 문자열로 바뀐다. 즉, event.target.id = 문자열이었고, 이것 때문에 위에서 출력했을 때, setId를 통해 출력한 Id 값이 문자열이 됬던 것이다.

      따라서, onChangeMode에 인자로 넣을떄 숫자형으로 바꿔서 넣어주면 될 것이다!

    ```react
    function Nav(props) {
      const lis = [];
      for (let i = 0; i < props.topics.length; i++) {
        let t = props.topics[i];
        lis.push(
          <li key={t.id}>
            <a
              id={t.id}
              href={"/read/" + t.id}
              onClick={(event) => {
                event.preventDefault();
                // 이 부분에서 인자를 받을때 숫자형으로 변환해서 넣어줌
                props.onChangeMode(Number(event.target.id));
              }}
            >
              {t.title}
            </a>
          </li>
        );
      }
      return (
        <nav>
          <ol>{lis}</ol>
        </nav>
      );
    }
    ```

    - 위와 같이 바꾸면 버튼을 클릭할때 해당 READ의 값으로 내용이 잘 바뀐다.



## 8. CRUD - 1 . CREATE

- CRUD의 CREATE를 만들고자 한다.

- 먼저 CREATE 페이지를 만들어야 한다.

  - 현재까지 react에선 따로 페이지를 만드는 것이 아닌, 현재 mode의 값에 따라 현재 페이지에서 출력되는 것이 다르게 하였고, create 페이지도 이와 같이 진행할 것이다.

  ```react  
  function App() {
      ...
        <a href="/create" onClick={event =>{
          event.preventDefault();
          setMode("CREATE");
          }
        }>Create</a>
      ...
  }
  ```

  - 위와 같이 App 최하단에 create 페이지를 호출할 a태그를 생성한다. mode 값을 통해 페이지에 출력되는 값을 바꿀 것이므로 이미 사용하던 setMode를 이용해 mode의 값을 CREATE로 바꾼다.

  ```react
  if (mode === "WELCOME") {
      content = <Article title="Welcome" body="Hello, WEB"></Article>;
    } else if (mode === "READ") {
      let title,
        body = null;
      for (let i = 0; i < topics.length; i++) {
        console.log(topics[i].id, id);
        if (topics[i].id === id) {
          title = topics[i].title;
          body = topics[i].body;
        }
      }
      content = <Article title={title} body={body}></Article>;
    } else if (mode === "CREATE") {
      content = <Create></Create>
    }
  ```

  - mode를 통해 출력하는 내용을 바꾸는 조건문에서 CREATE 일때의 경우를 추가한다.

    이 때, Create 컴포넌트를 만들어서 해당 컴포넌트를 출력하기로 한다.

  ```react
  function Create(props) {
    return (
      <article>
        <h2>Create</h2>
        {/* form에서submit 이벤트가 발생하면 페이지를 새로고침 해버리기 때문에event.preventDefault를 사용해야한다. */}
        <form
          onSubmit={(event) => {
            event.preventDefault();
            // event.target.title은 그냥 태그고 해당 태그의 value에 접근해야 그 값을 받을 수 있다.
            const title = event.target.title.value;
            const body = event.target.body.value;
            // App에서 Create 컴포넌트에 props로 보내준 onCreate 함수를 호출하여 위에서 저장한 제목과 내용을 인자로 보내준다.
            props.onCreate(title, body);
          }}
        >
          <p>
            <input type="text" name="title" placeholder="title"></input>
          </p>
          <p>
            <textarea body="content" placeholder="body"></textarea>
          </p>
          <p>
            <input type="submit" value="Create"></input>
          </p>
        </form>
      </article>
    );
  }
  ```

  - 위 주석의 내용을 기반으로 Create 컴포넌트를 작성한다.
  - Create 컴포넌트가 작성되었다면, 해당 컴포넌트를 이용하는 조건문으로 가서 onCreate에 인자로 준 값을 이용해 topics를 갱신해주면된다.

  ```react
    } else if (mode === "CREATE") {
      content = (
        <Create
          onCreate={(_title, _body) => {
            const newTopic = {
              title: _title,
              body: _body,
            };
            setTopics(newTopic);
          }}
        ></Create>
      );
    }
  ```

  - 음... 그런데 위와 같이 적어도 topics의 값이 바뀌지않는다 엥...? 왜 일까?

  ### 데이터 타입 별 상태 변경 방법

  - 상태를 만들 때, 상태의 데이터는 원시 데이터 타입이라면 기존의 방식대로 상태를 만들면 된다.

    > 원시데이터 타입
    >
    > 1. String
    > 2. number
    > 3. boolean
    > 4. null
    > 5. undefined
    > 6. bigint
    > 7. symbol

  - 하지만 상태를 만들 때 들어가는 데이터 타입이 범 객체라면?

    > 범 객체 타입
    >
    > 1. object
    > 2. array

    - 상태로 만들 때 다른 방식으로 접근해야한다.

      ```react
      // 1. 기존 value를 복제
      newValue = {...value}
      // 기존의 변수를 만들지 말고 복제해서 변경한 다음 setValue 해야한다.
      // 2. 복제한 value에 변경된 value를 할당
      newValue = changeValue
      // 3. 변경된 value로 상태변경
      setValue(newValue)
      ```

      - 각각의 범객체를 복제 할때는

        ```react
        // 객체를 복제 할때는
        = {...value}
        // 배열을 복제 할때는
        = [...value]
        ```

  - 그렇다면 왜 갱신되지 않았던 걸까?

    - setValue()는 value값이 변경이 됬는지 확인을 하여 확인이 되야 화면을 변경하지 원래 value와 바뀌지않았으면 굳이 화면을 변경하지 않는다고 한다.

      

- 그렇다면 위의 내용을 코드에 적용해보자

  ```react
    } else if (mode === "CREATE") {
      content = (
        <Create
          onCreate={(_title, _body) => {
            // onCreate로 받은 데이터를 객체로 만듬
            const newTopic = {
              id: nextId,
              title: _title,
              body: _body,
            };
            // 기존 배열를 newTopics에 복제
            const newTopics = [...topics];
            // 복제한 배열 newTopics에 onCreate의 데이터를 넣음
            newTopics.push(newTopic);
            // 새롭게 만들어진 배열로 상태 설정
            setTopics(newTopics);
          }}
        ></Create>
      );
    }
  ```

  - 주석에 내용에 맞추어 작성하면 create가 잘 생성된다.
  - 그 다음, create 기믹을 더 정교하게 만들자
    1. 글 작성시 해당 글 상세페이지로 가자.
    2. 다음 글 작성을 위해 id 값 갱신

  ```react
    } else if (mode === "CREATE") {
      content = (
        <Create
          onCreate={(_title, _body) => {
            // onCreate로 받은 데이터를 객체로 만듬
            const newTopic = {
              id: nextId,
              title: _title,
              body: _body,
            };
            // 기존 배열를 newTopics에 복제
            const newTopics = [...topics];
            // 복제한 배열 newTopics에 onCreate의 데이터를 넣음
            newTopics.push(newTopic);
            // 새롭게 만들어진 배열로 상태 설정
            setTopics(newTopics);
            // 글 생성 시 상세페이지로 넘어가게 한다.
            setMode("READ");
            // 현재 아이디를 방금 생성한 아이디로 변경한다.
            setId(nextId);
            // 다음 아이디 생성 때, 아이디 값이 겹치지 않게 하기 위해 상태 변경
            setNextId(nextId + 1);
          }}
        ></Create>
      );
    }
  ```



## 9. CRUD_2. UPDATE

- 먼저 update로 가는 링크를 추가하자

  ```react
    return (
      <div>
        <Header
          title="REACT"
          onChangeMode={() => {
            setMode("WELCOME");
          }}
        ></Header>
        <Nav
          topics={topics}
          onChangeMode={(_id) => {
            setMode("READ");
            setId(_id);
          }}
        ></Nav>
        {content}
        {/* <Article title="Welcome" body="Hello, WEB"></Article> */}
        <ul>
          <li>
            <a
              href="/create"
              onClick={(event) => {
                event.preventDefault();
                setMode("CREATE");
              }}
            >
              Create
            </a>
          </li>
          <li><a href="/update">Update</a></li>
        </ul>
      </div>
    );
  }
  
  export default App;
  ```

  - App의 최하단에 Update 버튼을 추가했다! 그런데... update는 수정 버튼인데 메인페이지, 생성페이지에도 다 노출이 된다. 별로 보기가 안좋다. 수정하도록 하자

  ```react
  // App 변수 선언부분
    // update 가 상세페이지에서만 나올 수 있게 하기 위한 변수
    let contextControl = null;
  
  // App 조건문에서 mode 값이 READ 일때 부분
  	...
      contextControl = (
        <li>
          <a href="/update">Update</a>
        </li>
      );
  
  // App 최하단 부분
        <ul>
          <li>
            <a
              href="/create"
              onClick={(event) => {
                event.preventDefault();
                setMode("CREATE");
              }}
            >
              Create
            </a>
          </li>
          {contextControl}
        </ul>
  ```

  - 위와 같이 코드를 바꾸면, 평소에는 contextControl이 null 값이라 아무것도 출력되지 않지만, mode 값이 READ 일때 contextControl 안에 HTML 코드가 추가되어 화면에 출력된다.

- 그 다음 update 링크를 눌렀을 때, 올바른 주소로 이동하고, mode 값을 바꾸기 위해 contextControl의 코드를 수정한다.

  ```react
  // mode === 'READ'의 최하단 부분
  contextControl = (
        <li>
          <a href={"/update/" + id} 
          onClick={event => {
            event.preventDefault();
            setMode("UPDATE");
          }}>
            Update</a>
        </li>
      );
  ```

- 그런 다음 update 컴포넌트를 만들자

  ```react
  function Update(props) {
    const [title, setTitle] = useState(props.title);
    const [body, setBody] = useState(props.body);
    return (
      <article>
        <h2>Update</h2>
        {/* form에서submit 이벤트가 발생하면 페이지를 새로고침 해버리기 때문에event.preventDefault를 사용해야한다. */}
        <form
          onSubmit={(event) => {
            event.preventDefault();
            // event.target.title은 그냥 태그고 해당 태그의 value에 접근해야 그 값을 받을 수 있다.
            const title = event.target.title.value;
            const body = event.target.body.value;
            props.onUpdate(title, body);
          }}
        >
          <p>
            {/* props로 받은 데이터를 그대로 넣으면 input값을 바꿀수가 없다.
            props로 내려준 값은 꼭 지켜져야하는 규칙이기 때문 - 부모가 내려준 값은 변경 불가 */}
            <input
              type="text"
              name="title"
              placeholder="title"
              // 기존 props.title을 value로 쓰던것을 위에서 상태로 선언한 title로 교체
              value={title}
              // onChange 함수를 통해 event(키입력)가 발생한 값을 title 상태의 새로운 값으로 변경
              onChange={(event) => {
                setTitle(event.target.value);
              }}
            ></input>
          </p>
          <p>  
            <textarea
              // 위와 동일하게 작성
              name="body"
              body="content"
              placeholder="body"
              value={body}
              onChange={(event) => {
                setBody(event.target.value);
              }}
            ></textarea>
          </p>
          <p>
            <input type="submit" value="Update"></input>
          </p>
        </form>
      </article>
    );
  }
  ```

  - 수정에 입장했을 때 기존에 작성한 데이터를 유지하여 보여주기 위해 props로 내려준 값을 각 input의 value 값으로 넣었다.

    그러자, input에 아무리 키입력을 해도 내용이 바뀌질 않았다.

    그 이유는, props로 부모가 내려보내준 값은 변경할 수 없기 때문이었다. 그에따라, props로 내려받은 두 개의 데이터를 상태로 변경하여 진행할 필요가 있었다.

    > ```react
    >   const [title, setTitle] = useState(props.title);
    >   const [body, setBody] = useState(props.body);
    > ```

    위와 같이 하여 value값을 상태로 바꾼 title로 바꾸고, 키입력시 바뀐 내용을 새로운 상태값으로 선언하며 값을 바꾸었다.

    그리고 그 값들을 submit 이벤트가 발생했을 때, onUpdate 함수에 인자로 보내주었다.

  - Update 컴포넌트에서 onUpdate로 보내준 값을 이용하여 해당 게시글의 내용을 바꿀 차례다.

    ```react
    } else if (mode === "UPDATE") {
        // 현재 게시글의 제목과 내용을 담기위해 변수선언
        let title,
          body = null;
        // topics를 순회하면서 현재 글의 아이디와 같은 topics의 객체를 찾아서
        // 해당 객체의 값을 위에 선언한 값에 넣는다.
        for (let i = 0; i < topics.length; i++) {
          if (topics[i].id === id) {
            title = topics[i].title;
            body = topics[i].body;
          }
        }
        // 변수에 저장한 값을 props로 보내줌
        content = (
          <Update
            title={title}
            body={body}
            // create 컴포넌트에서 보내준 _title, _body 값을 이용하여 현재 게시글의 내용 변경
            onUpdate={(_title, _body) => {
              // topics 배열 안에 있는 값을 수정해야 함으로 우선 복제
              const newTopics = [...topics];
              // 현재 아이디와 컴포넌트에서 받은 수정된 값을 객체로 만듬
              const updatedTopic = {
                id: id,
                title: _title,
                body: _body,
              };
              // 복제한 newTopics를 순회하면서, 현재 게시글의 id 값이 같은 객체를 찾으면
              // 위의 updatedTopic으로 교체
              for (let i = 0; i < newTopics.length; i++) {
                if (newTopics[i].id === id) {
                  newTopics[i] = updatedTopic;
                  break;
                }
              }
              // 변경한 newTopics 값을 상태로 변경 
              setTopics(newTopics);
              // 변경한 게시글 상세로 가기위해 mode상태를 READ로 변경
              setMode("READ");
            }}
          ></Update>
        );
      }
    ```

    1. onUpdate에서 인자로 받은 값으로 topics의 값을 변경하기 위해서 topics를 복제한 newTopics 선언
    2. 인자로 받은 값들을 현재 게시글의 id 번호와 함께 객체 updatedTopic로 만듬
    3. newTopics를 순회하면서, 현재 게시글의 id와 똑같은 id를 가진 데이터를 찾고, 찾은 후 해당 데이터와 updatedTopic을 교체
    4. 변경한 newTopics의 값을 새로운 상태로 변경
    5. 변경한 게시글 상세로 돌아가기 위해 mode상태를 READ로 변경 (단, 이미 게시글 id가 지정되어 있으므로 setId는 할필요없음)

    - 위와 같이 진행하여 update 기능을 구현할 수 있었다!



## 10. CRUD_3.DELETE

- DELETE를 구현하기위해서 update 링크를 추가한 곳에 delete 버튼을 추가

  - 버튼이어야함! delete는 따로 페이지(컴포넌트)가 필요없기 때문!

  ```react
  contextControl = (
        // 아무것도 들어있지 않은 태그는 안에 있는 태그들을 묶기 위한 용도로만 쓰이는 태그
        <>
          {/* 업데이트 버튼 부분 */}
          <li>
            <a
              href={"/update/" + id}
              onClick={(event) => {
                event.preventDefault();
                setMode("UPDATE");
              }}
            >
              Update
            </a>
          </li>
          {/* 삭제 버튼 부분 */}
          <li>
      	  {/* 버튼 선언 */}
            <input
              type="button"
              value="Delete"
              onClick={() => {
                // 삭제 기믹
                // 1. 빈 배열을 선언
                const newTopics = [];
                // 2. for 문을 돌아 topics에 현재 id와 다른 데이터들만 newTopics에 담음
                for (let i = 0; i < topics.length; i++) {
                  if (topics[i].id !== id) {
                    newTopics.push(topics[i]);
                  }
                }
               // 3. 현재 게시글만 없는 newTopics를 상태로 선언
                setTopics(newTopics);
              // 4. 현재 글이없으니 메인화면으로 이동을 위해 상태를 "WELCOME"으로 선언
                setMode("WELCOME");
              }}
            ></input>
          </li>
        </>
      );
  ```

  - 위와 같이 하면 DELETE 기능이 구현 가능하다
