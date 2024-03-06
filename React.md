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

  ```react
  ```

  - 음.. 컴포넌트까지 연결했으니 이제 App에서 부터 적으면 될듯!
