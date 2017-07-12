

# Chapter05 : Router

## 주요내용

### 라우팅을 구현하는 단순한 방식

```
componentDidMount() {
    window.addEventListener('hashchange', () => {
      this.setState({
        route : window.location.hash.substr(1)
      });
    });
}
```

```
render(){
    var Child;
    switch(this.state.route){
      case '/about' : Child = About; break;
      case '/repos' : Child = Repos; break;
      default : Child = Home;
    }

    return (
      <div>
        <header>App</header>
        <menu>
            <ul>
              <li><a href="#/about">About</a></li>
              <li><a href="#/repos">Repos</a></li>
            </ul>
        </menu>
        <Child/>
      </div>
    )
  }
```

- 단순 방식으로 구현은 가능하나, 단순하지 않은 시나리오에서 라우팅 코드가 기하급수적으로 복잡해질 수 있다.
- URL 구문 분석을 훨씬 지능적으로 수행해야하며, 특정 URL에 대해 중첩된 컴포넌트의 분기 렌더링이 어려워 진다.



### 리액트 라우터

> 컴포넌트를 중첩 단계에 관계없이 라우터와 연결하는 방법으로 UI를 URL과 동기화 하며, 사용자가 URL을 변경하면 컴포넌트가 자동으로 언마운트/마운트 됨.

- 처음부터 웹앱에서 사용 할 모든 컴포넌트들을 먼저 불러와두고, 페이지를 이동할 때 마다 그때 그때 페이지를 처음부터 로딩하지 않고 필요한 컴포넌트만 다시 렌더링 합니다

- 아래 세가지 컴포넌트를 제공

  - Router, Route : 라우트를 선언적으로 애플리케이션의 화면계층과 매핑

    ```
    <Router>
    	<Route path="/" component={App}>
         	<Route path="about" component={About}></Route>
            <Route path="repos" component={Repos}></Route>
        </Route>
    </Router>
    ```

  - Link : 올바른 href로 완전 접근이 가능한 앵커 태그 만듬

    ```
    <menu>
    	<ul>
    		<li><Link to="/about" activeClassName="active">About</Link></li>
            <li><Link to="/repos" activeClassName="active">Repos</Link></li>
    	</ul>
    </menu>
    ```


- 인덱스 라우터

  - Home 라우트 추가를 위해 설정

    ```
    <Router>
    	<Route path="/" component={App}>
    		<IndexRoute component={Home}></Route>
         	<Route path="about" component={About}></Route>
            <Route path="repos" component={Repos}></Route>
        </Route>
    </Router>
    ```


- 매개변수를 이용하는 라우터

  - path에 매개변수를 받아 활용하게 설정

    ```
    <Route path="details/:repo_name" component={RepoDetails} />
    ```

    ```
    this.props.params.repo_name
    ```

- 자식 복제와 속성 주입

  - this.props.children 을 그대로 렌더링하는 대신, 이를 복제+추가 속성을 주입

    ```
    let child = this.props.children && React.cloneElement(this.props.children,
     	{ repositories: this.state.repositories }
    )
    ```


- 속성 전달하기

  - Route 구성의 속성

    ```
    <Route path="about" component={About} title="About Us"/>
    ```

    ```
    {this.props.route.title} 
    ```

- UI와 URL의 연결 분리

  - 상대경로가 너무 길다고 판단될 때, 짧게 절대 URL로!

- 히스토리

  - 히스토리 스택과 URL을 조작할 수 있는 공용 API를 제공
  - javascript library that lets you easily manage session history anywhere Javascript runs.
  - history objects may be used programmatically change the current location using the following methods:
    - `history.push(path, [state])`
    - `history.replace(path, [state])`
    - `history.go(n)`
    - `history.goBack()`
    - `history.goForward()`
    - `history.canGo(n)` (only in `createMemoryHistory`)
  - browser, hash, memory 



### Kanban : route

- /new - 새로운 카드를 작성하는 폼 생성

- /edit/:card_id - 현재 속성 확인 및 편집하는 폼 생성

  ​

### 참고 자료

- history 관련 
  - https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/api/history.md
  - https://www.npmjs.com/package/history

