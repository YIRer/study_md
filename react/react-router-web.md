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
`<Link>` `<a>`태그 대신 사용하여 라우터처리 to : 연결할 위치 또는 이름 string or object
`<Link to={{
  pathname: '/courses', :경로지정
  search: '?sort=name', :query string
  hash: '#the-hash', :해시태그
  state: { fromDashboard: true } :location에 넣을 state
}}/>`
replace : 스택에 쌓이지 않고 대체

NavLink : Link의 특수한 버전 랜더링된 url이 같을 경우 지정 스타일/클래스가 부여됨 ativeClassName : 적용할 클레스이름 activeStyle: 적용할 스타일 object 
exact : 정확한 위치인가 bool /one /one/two : no
strict : 슬래시에 따라서 bool /one/ /one/two : yes
isActive : 링크가 활성 상태인지 확인후 활성화 func
location 객체를 비교하기 위해 전달가능

Prompt : 특정 값이 true/false일때 먼저 질문을 하고 서브밋을 그후에한다 when 체크할 값 bool message 첨부할 메세지 string or func

MemoryRouter : 현재 URL을 메모리에 저장, 테스트 및 비 브라우저 환경에서 사용
initialEntries : location들의 배열, { pathname, search, hash, state } 혹은 간단한 문자열 배열 array
initialIndex: 배열의 초기 위치 number
getUserConfirmation 탐색을 확인하는 데 사용하는 함수입니다. 

Redirect : 랜더링하면 새로운 위치로 보냄 to : 리디렉션할 위치 string or object   pathname: '/login',
  search: '?utm=your+face',
  state: { referrer: currentLocation } from : 해당 라우트로 접근시 to로 보냄,switch와 사용 : string push: 리다이렉팅대신, 이동할 곳을 대체함
Route : path로 이동할 경우 컴포넌트등을 랜더링함 라우팅시 component render children 을 사용하여 랜더링
<ul>
  <ListItemLink to="/somewhere"/>
  <ListItemLink to="/somewhere-else"/>
</ul>

const ListItemLink = ({ to, ...rest }) => (
  <Route path={to} children={({ match }) => (
    <li className={match ? 'active' : ''}>
      <Link to={to} {...rest}/>
    </li>
  )}/>
)
<Route children={({ match, ...rest }) => (
  {/* Animate will always render, so you can use lifecycles
      to animate its child in and out */}
  <Animate>
    {match && <Something {...rest}/>}
  </Animate>
)}/>

match, history, location 객체를 같이 받아옴

path : URL 경로 string
exact : bool
strict : bool
location : 다린 위치의 path와 일치시켜야 하는 경우에 사용(switch)
sensitive : 대소문자 구분

Router: 라우트를 감싸 라우팅 구현
<BrowserRouter> :동적서버
<HashRouter>: 스태틱서버
<MemoryRouter> : 테스트 및 비 브라우저
<NativeRouter> : 안드로이드,ios
<StaticRouter>: 서버측 랜더링

Switch : 특정 라우팅에따라 컴포넌트를 안보이게 처리하거나 하위 컴포넌트로 이동
<Fade>
  <Switch>
    {/* there will only ever be one child here */}
    <Route/>
    <Route/>
  </Switch>
</Fade>

<Fade>
  <Route/>
  <Route/>
  {/* there will always be two children here,
      one might render null though, making transitions
      a bit more cumbersome to work out */}
</Fade>

history : history 패키지를 지칭하며, 다양한 환경에서 세션기록을 관리
createBrowserHistory : HTML5 API
createMemoryHistory : React Native
createHashHistory : legacy web browser

length: 기록된 스택의 수
action: 핸제 문자열의 액션 push, replace, pop
location : 현재의 위치
    pathname : url 경로
    search : 쿼리 문자열
    hash: 해시조각
    state : push() 됐을때 제공되는 state 상태 브라우저 혹은 메모리상태에서만
push(path,[state]): 히스토리 스택에 새 항목을 푸시
replace(path,[state]) : 히스토리 스택에 현제 엔트리 대체
go(n) : 스택의 포인터로 이동
goBack() : 뒤로이동
goForward() : 앞으로 이동
block : 이동하기전에 prompt등으로 떠나는 의사 여부 결정후 실행
const unblock = history.block('Are you sure you want to leave this page?')

history.block((location, action) => {

  if (input.value !== '')
    return '떠나시겠습니까?'
})

unblock()// 블록해제

location : 현재 위치를 저장하는 곳
{
  key: 'ac3df4', // not with HashHistory!
  pathname: '/somewhere'
  search: '?some=search-string',
  hash: '#howdy',
  state: {
    [userDefined]: true
  }
}
아래 객체에 값을 전달하여 사용가능, history.location은 값이 변화 할 수 있으므로, location객체를 사용할 것
Route component as this.props.location
Route render as ({ location }) => ()
Route children as ({ location }) => ()
withRouter as this.props.location

아래 객체에 string 대신 location 객체를 전달할 수 있음
Web Link to
Native Link to
Redirect to
history.push
history.replace

// usually all you need
<Link to="/somewhere"/>

// but you can use a location instead
const location = {
  pathname: '/somewhere',
  state: { fromDashboard: true }
}

<Link to={location}/>
<Redirect to={location}/>
history.push(location)
history.replace(location)

Route와 Switch 컴포넌트에 전달 가능

Match : Route path와 얼마나 일치하는 지에 대한 정보가 들어가 있음
params : 파싱된 키/값의 URL 파라미터
isExact : 일치하는가
path : 일치하는지 파악하는데 사용된 경로 Route
url : url이 일치하는 부분 Linck 

아래의 객체에서 사용 가능

Route component as this.props.match
Route render as ({ match }) => ()
Route children as ({ match }) => ()
withRouter as this.props.match
matchPath as the return value

withRouter의 경우에는 가장 가까운 parent 의 값을 첨부하여 사용
matchPath : 매치되는 정보를 담아두고 매치되는지 확인하여 적용
import { matchPath } from 'react-router'

const match = matchPath('/users/123', {
  path: '/users/:id',
  exact: true,
  strict: false
})
또는
서버사이드랜더링
const match = routes.reduce((acc, route) => matchPath(req.url, route, { exact: true }) || acc, null);
    if (!match) {
        res.status(404).send(render(<NoMatch />));
        return;
    }

    withRouter : history 객체속성과 가까운 route의 랜더링 될때마다 업데이트된 부분을 전달받아 반영