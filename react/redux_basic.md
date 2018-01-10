# redux의 기본과 redux-actions를 이용한 state 변경 및 전달

## 목차
<!-- TOC -->

- [redux의 기본과 redux-actions를 이용한 state 변경 및 전달](#redux의-기본과-redux-actions를-이용한-state-변경-및-전달)
  - [목차](#목차)
  - [기본환경 설치](#기본환경-설치)
  - [기본적인 redux 사용하기](#기본적인-redux-사용하기)
  - [여러개의 리듀서를 나눠서 관리하고, 하나의 스토어에 합쳐서 사용](#여러개의-리듀서를-나눠서-관리하고-하나의-스토어에-합쳐서-사용)
  - [redux API](#redux-api)
    - [StoreAPI](#storeapi)
    - [ReducerAPI](#reducerapi)
    - [Wrapper API](#wrapper-api)
    - [Connect API](#connect-api)
    - [bindActionCreators](#bindactioncreators)
    - [컴포넌트 연결](#컴포넌트-연결)
  - [DUCKS 구조](#ducks-구조)
  - [리덕스 관리 툴](#리덕스-관리-툴)
    - [redux-actions 액션을 자동을 생성해주고, 리듀서를 switch문대신에 객체화하여 사용](#redux-actions-액션을-자동을-생성해주고-리듀서를-switch문대신에-객체화하여-사용)
    - [액션생성자](#액션생성자)
    - [리듀서 생성후 전달](#리듀서-생성후-전달)
    - [redux-actions 액션들과 리듀서 결합](#redux-actions-액션들과-리듀서-결합)
  - [redux-thunk 어플리케이션의 비동기 처리를 위한 도구, 기본 베이스](#redux-thunk-어플리케이션의-비동기-처리를-위한-도구-기본-베이스)
    - [스토어에 적용](#스토어에-적용)
    - [간단한 예제](#간단한-예제)
  - [redux-promise-middleware redux-thunk기반으로 promise 및 비동기 처리](#redux-promise-middleware-redux-thunk기반으로-promise-및-비동기-처리)
    - [기본 사용](#기본-사용)
    - [여러개의 promise 발생](#여러개의-promise-발생)
    - [resolve, reject 체크](#resolve-reject-체크)
    - [error 캐치](#error-캐치)
    - [asyn, await 를 이용한 비동기 처리](#asyn-await-를-이용한-비동기-처리)
    - [리듀서에 적용하기](#리듀서에-적용하기)
    - [접미사 커스터마이징](#접미사-커스터마이징)
  - [redux-logger 리덕스의 상태 변경 사항을 로그형태로 기록](#redux-logger-리덕스의-상태-변경-사항을-로그형태로-기록)
    - [미들웨어 적용](#미들웨어-적용)
  - [axios 비동기 http 요청 처리](#axios-비동기-http-요청-처리)
    - [기본사용](#기본사용)
    - [파라미터 등을 사용](#파라미터-등을-사용)
  - [query-string 쿼리문을 사용하기 쉽게](#query-string-쿼리문을-사용하기-쉽게)
    - [사용법](#사용법)
    - [axios와 같이 사용](#axios와-같이-사용)

<!-- /TOC -->

## 기본환경 설치
    npm install --save redux react-redux

## 기본적인 redux 사용하기
    { createStore } from redux

action : 객체를 반환하며, 특정한 행동을 타입값에 정의

reducer : 액션이 발생하면 받은 데이터를 어떻게 처리 하는지 설정, 여러개의 리듀서를 하나로 사용 할 수 있지만, 리듀서 간의 직접적인 관계는 없어야함

## 여러개의 리듀서를 나눠서 관리하고, 하나의 스토어에 합쳐서 사용
    const counterApp = combineReducers({
        a: counter,
        b: extra
    });


## redux API

    {'APINAME' } from 'redux'

### StoreAPI

createStrore : state들이 모인 객체, 단 한 개만 존재해야함

    const store = createStore(Reducer);

store.getState() : 현재 스토어의 데이터 반환

store.dispatch(ACTION) : 액션에 인자전달

store.subscribe(LISTENER) : datach 가 실행되면 LISTENER 실행

### ReducerAPI

combineReducers : 여러개로 나누어 관리하는 리듀서들을 하나로 합침

      const useCombine = combineReducers({
          reducer1, reducer2..... or
          a : reducer , b: reducer
      });


### Wrapper API

Provider : store에 listening 하고 있는 컴포넌트에 componentWillUnmount 와 this.state = {...}를 constructor에 정의 할 필요가 없어짐, 어플리케이션에 스토어를 연결할때 사용

    <Provider store={store}> 
      <div className="App">
        ...
      </div>
    </Provider>

### Connect API

connect : 컴포넌트를 store에 연걸

    connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])(전달할 곳)

mapStateToProps(state, [ownProps]) : 컴포넌트 props 에 전달할 상태, ownProps 인수가 명시될 경우, 이를 통해 함수 내부에서 컴포넌트의 props 값에 접근 할 수 있습니다.

    let mapStateToProps = (state) => {
        return {
            value: state.key.value
        };
    }

mapDispatchToProps(dispatch, [ownProps]) : 컴포넌트의 특정 함수형 props 를 실행 했을 때, 개발자가 지정한 action을 dispatch 하도록 설정합니다.

    let mapDispatchToProps = (dispatch) => {
        return {
            componet(this).props.method : () => dispatch(ACTION()),
            componet(this).props.method2 : () => dispatch(ACTION2())
        }
    }


### bindActionCreators

리덕스와 연관되어 기능 하지 않는 컴포넌트에 액션 생산자를 넘기지만, dispatch나 스토어를 넘기지 않기 위하여 사용

  bindActionCreators(actionCreators, dispatch)
  
    export default connect(
      (state) => ({ //state 설정
          number: state.counter,
          post: state.post.data,
          loading : state.post.pending,
          error : state.post.error
      }),
      (dispatch) => ({ // 액션 전달
          CounterActions: bindActionCreators(counterActions, dispatch),
          PostActions: bindActionCreators(postActions, dispatch)
      })
    )(App);

### 컴포넌트 연결

    ComponentName = connect(mapStateToProps, mapDispatchToProps)(ComponentName);

redux-actions
ducks구조 하나의 모듈안에 담아서 작업을 처리

## DUCKS 구조

분리된 액션과 리듀서를 하나의 파일로 관리

액션네이밍 규칙 : Reducer/ACTION_TYPE

    const ACTION_NAME = "POST/GET_POST";

모든 액션은 export로 내보내야하고 리듀셔는 export default로 내보내야함. 리듀서의 경우에는 가변네이밍을 받아야하므로 []로 감싼다

    export const ACTION = create(ACTION_NAME);

    export default const Reducer_Name = handleActions({
      [ACTION] : (state,actions) =>{
        return ...
      }
    },초기 상태값)

## 리덕스 관리 툴

### redux-actions 액션을 자동을 생성해주고, 리듀서를 switch문대신에 객체화하여 사용

    npm install --save redux-actions

### 액션생성자 
액션을 자동으로 생성하고, 전달받는 액션값이 있을 경우 payload안에 담아서 전달

createActon(액션이름, payload에 전달할 값, metaCreatro) ||

createActions(액션이름, payload에 전달할 값, metaCreatro)

    export const ACTION = create(ACTION_NAME);
    /* 
      export function ACTION() {
          return {
              type: ACTION_NAME
          };
      }
      인자를 전달 할시, payload에 값을 담아서 전달 따라서 타입체크를 할 경우에는 액션 발생전에 체크
    */

### 리듀서 생성후 전달

handleAction(액션이름, 리듀서(맵), 기본 상태값) || 

handleActions(리듀서 맵, 기본 상태값)

    export default const Reducer_Name = handleActions({
      [ACTION] : (state,actions) =>{
        return ...
      }
    },초기 상태값)
    /*
      actions으로 받은 값은 payload에 값으로 담겨 전달 되므로,
      action.payload.전달받은 값의 키값 혹은 값으로 접근
      ex)action.payload || action.payload.data
      현재의 상태는 state로 받아서 적용하면 된다. 
    */

### redux-actions 액션들과 리듀서 결합 

combineActions(types,reducer) : 액션들과 리듀서들을 한번에 정의

    const { increment, decrement } = createActions({
      INCREMENT: amount => ({ amount }),
      DECREMENT: amount => ({ amount: -amount })
    });

    const reducer = handleActions({
      [combineActions(increment, decrement)](state, { payload: { amount } }) {
        return { ...state, counter: state.counter + amount };
      }
    }, { counter: 10 });

## redux-thunk 어플리케이션의 비동기 처리를 위한 도구, 기본 베이스

dispatch와 getState를 인자로 받아 비동기적으로 처리

### 스토어에 적용
    import { createStore, applyMiddleware } from 'redux';
    import thunk from 'redux-thunk';
    import rootReducer from './reducers/index';
    const store = createStore(
      rootReducer,
      applyMiddleware(thunk) 
      // 같이 전달할 api가 있을 경우 hunk.withExtraArgument({ api, whatever })
    );
    /*
      function fetchUser(id) {
        return (dispatch, getState, { api, whatever }) => {
          // 전달받은 리듀서 사용가능
        }
      }
    */

### 간단한 예제

    export const ACTION_ASYNC = () => dispatch =>{
        setTimeout(
            ()=>{
                dispatch(ACTION())
            }, 1000
        );
    }
    // 1초뒤에 액션 실행

## redux-promise-middleware redux-thunk기반으로 promise 및 비동기 처리
promise : 어떠한 동작을 비동기로 처리하고 성공 여부에 따라 다르게 처리

propmise 상태 : pendding 대기 resolve 성공 reject 실패 error 에러

redux-promise-middleware : redux-thunk 위에서 promise 상태를 처리, redux-actions와 결합하여 사용 가능

### 기본 사용

    const foo = () => {
      return dispatch => {
        return dispatch({
          type: 'TYPE',
          payload: new Promise()
        }).then(() => dispatch(bar()));
      };
    }
### 여러개의 promise 발생
    const foo = () => {
      return dispatch => {

        return dispatch({
          type: 'TYPE',
          payload: Promise.all([
            dispatch(bar()),
            dispatch(baz())
          ])
        });
      };
    }

### resolve, reject 체크

  const foo = () => { // 성공시 사용할 액션
    return dispatch => {

      return dispatch({
        type: 'FOO',
        payload: new Promise(resolve => {
          resolve('foo'); // 리턴한 값을 foo로 받음
        })
      }).then(({ value, action }) => {
        console.log(value); // => 'foo'
        console.log(action.type); // => 'FOO_FULFILLED' promise 미들웨어에 설정된 값에 따라 _FULFILLED가 붙음
      });
    };
  }
  const bar = () => { //실패시 사용할 액션
    return dispatch => {

      return dispatch({
        type: 'BAR',
        payload: new Promise(() => {
          throw new Error('foo'); // 에러 발생
        })
      }).then(() => null, error => { //메세지 출력
        console.log(error instanceof Error) // => true
        console.log(error.message); // => 'foo'
      });
    };
  }

### error 캐치

    export function foo() {
      return dispatch => dispatch({
        type: 'FOO_ACTION',
        payload: new Promise(() => {
          throw new Error('foo');
        })
      }).catch(error => {
        console.log(error.message);
        //에러발생시 실행할 함수
        dispatch(bar());
      });
    }

### asyn, await 를 이용한 비동기 처리

    {
      type: 'TYPE',
      async payload () {
        const fooData = await getFooData();
        const barData = await getBarData(fooData);

        return barData;
      }
    }

### 리듀서에 적용하기

    const FOO_TYPE = 'FOO';

    // 액션 생성
    const fooActionCreator = () => ({
      type: FOO_TYPE
      payload: Promise.resolve('foo')
    });

    //리듀서 생성
    const fooReducer = (state = {}, action) => {
      switch(action.type) {
        //redux-promise-middleware에 정의된 값에 따라 상태가 _상태로 붙어서 처리
        case `${FOO_TYPE}_PENDING`:
          return;

        case `${FOO_TYPE}_FULFILLED`:
          return {
            isFulfilled: true,
            data: action.payload
          };

        case `${FOO_TYPE}_REJECTED`:
          return {
            isRejected: true,
            error: action.payload
          };

        default: return state;
      }
    }

### 접미사 커스터마이징

미들 웨어 적용전에 미리 설정

    import promiseMiddleware from 'redux-promise-middleware';

    const customizedPromiseMiddleware = promiseMiddleware({
      promiseTypeSuffixes : ['LOADING','SUCCESS','FAILURE']
    });

## redux-logger 리덕스의 상태 변경 사항을 로그형태로 기록

    npm install --save redux-logger

### 미들웨어 적용

적용전에 미리 생성

  import { createLogger } from 'redux-logger';
  const logger = createLogger();

## axios 비동기 http 요청 처리

인터넷 익스플로러 8이상의 환경에서 동작하는 비동기 http요청 처리

    npm install --save axios

### 기본사용
    axios.get('/user?ID=12345')
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });

### 파라미터 등을 사용

    axios({
      method: 'post',
      url: '/user/12345',
      data: {
        firstName: 'Fred',
        lastName: 'Flintstone'
      }
    });

    axios({
      method:'get',
      url:'http://bit.ly/2mTM3nY',
      responseType:'stream'
    })
    .then(function(response) {
      response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
    });

## query-string 쿼리문을 사용하기 쉽게

    npm install --save query-string

### 사용법
    
    const queryString = require('query-string'); ||
    import queryString from 'query-string'

    console.log(location.search);
    //=> '?foo=bar'

    const parsed = queryString.parse(location.search);
    console.log(parsed);
    //=> {foo: 'bar'}

    console.log(location.hash);
    //=> '#token=bada55cafe'

    //쿼리문을 객체로 파싱
    const parsedHash = queryString.parse(location.hash);
    console.log(parsedHash);
    //=> {token: 'bada55cafe'}

    parsed.foo = 'unicorn';
    parsed.ilike = 'pizza';

    //파싱된 갹체 데이터를 쿼리문으로
    const stringified = queryString.stringify(parsed);
    //=> 'foo=unicorn&ilike=pizza'

    location.search = stringified;
    // note that `location.search` automatically prepends a question mark
    console.log(location.search);
    //=> '?foo=unicorn&ilike=pizza'

### axios와 같이 사용

    var querystring = require('querystring');||
    import queryString from 'query-string'
    
    axios.post('http://something.com/', querystring.stringify({ foo: 'bar' }));
