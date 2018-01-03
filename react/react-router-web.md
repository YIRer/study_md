react-router는 여러가지 버젼이 존재. 이중에서 웹에서 사용하는 것은 react-router-dom

`<BrowserRouter>` 와 `<HashRouter>`
`<BrowserRouter>`  일반적인 라우터 처리 동적요청처리를 사용하는 서버가 존재. 주소 전체를 사용
`<HashRouter>` 정적인 웹사이트의 라우터 처리, #뒤에 오는 주소값만 처리
history 현재 위치를 저장하고 변경될때마다 랜더링
Router는 단일 하위요소만 가지고 처리하기를 예상하고 작동하기 때문에 <App/> 컴포넌트안에 담아서
    `<Router>`
        `<App />`
    `</Router>`
등으로 처리하고 구체적인 라우팅 처리는 <App /> 컴포넌트 내에서 처리
`<Link>` `<a>`태그 대신 사용하여 라우터처리 to : 연결할 위치 또는 이름
`<Link to={{
  pathname: '/courses', :경로지정
  search: '?sort=name', :query string
  hash: '#the-hash', :해시태그
  state: { fromDashboard: true } :location에 넣을 state
}}/>`
replace : 스택에 쌓이지 않고 대체

NavLink : Link의 특수한 버전 랜더링된 url이 같을 경우 지정 스타일/클래스가 부여됨