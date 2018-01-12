# 리액트 라우팅 예제
## 목차

<!-- TOC -->

- [리액트 라우팅 예제](#리액트-라우팅-예제)
  - [목차](#목차)
  - [basic](#basic)
  - [parameters](#parameters)
  - [인증](#인증)
  - [커스텀 링크](#커스텀-링크)
  - [404](#404)
  - [재귀적 경로](#재귀적-경로)
  - [sidebar](#sidebar)
  - [부분일치](#부분일치)
  - [route-config](#route-config)
  - [정적 코드 연결](#정적-코드-연결)

<!-- /TOC -->
## basic

    import React from 'react'
    import {
      BrowserRouter as Router,
      Route,
      Link
    } from 'react-router-dom'

    const BasicExample = () => (
      <Router>
        <div>
          <ul>
            <li><Link to="/">Home</Link></li>
            <li><Link to="/about">About</Link></li>
            <li><Link to="/topics">Topics</Link></li>
          </ul>

          <hr/>

          <Route exact path="/" component={Home}/>
          <Route path="/about" component={About}/>
          <Route path="/topics" component={Topics}/>
        </div>
      </Router>
    )

    const Home = () => (
      <div>
        <h2>Home</h2>
      </div>
    )

    const About = () => (
      <div>
        <h2>About</h2>
      </div>
    )

    const Topics = ({ match }) => (
      <div>
        <h2>Topics</h2>
        <ul>
          <li>
            <Link to={`${match.url}/rendering`}>
              Rendering with React
            </Link>
          </li>
          <li>
            <Link to={`${match.url}/components`}>
              Components
            </Link>
          </li>
          <li>
            <Link to={`${match.url}/props-v-state`}>
              Props v. State
            </Link>
          </li>
        </ul>

        <Route path={`${match.url}/:topicId`} component={Topic}/>
        <Route exact path={match.url} render={() => (
          <h3>Please select a topic.</h3>
        )}/>
      </div>
    )

    const Topic = ({ match }) => (
      <div>
        <h3>{match.params.topicId}</h3>
      </div>
    )

    export default BasicExample

## parameters

    import React from 'react'
    import {
      BrowserRouter as Router,
      Route,
      Link
    } from 'react-router-dom'

    const ParamsExample = () => (
      <Router>
        <div>
          <h2>Accounts</h2>
          <ul>
            <li><Link to="/netflix">Netflix</Link></li>
            <li><Link to="/zillow-group">Zillow Group</Link></li>
            <li><Link to="/yahoo">Yahoo</Link></li>
            <li><Link to="/modus-create">Modus Create</Link></li>
          </ul>

          <Route path="/:id" component={Child}/>
        </div>
      </Router>
    )

    const Child = ({ match }) => (
      <div>
        <h3>ID: {match.params.id}</h3>
      </div>
    )

    export default ParamsExample

## 인증 

    import React from 'react'
    import {
      BrowserRouter as Router,
      Route,
      Link,
      Redirect,
      withRouter
    } from 'react-router-dom'

    ////////////////////////////////////////////////////////////
    // 1. Click the public page
    // 2. Click the protected page
    // 3. Log in
    // 4. Click the back button, note the URL each time

    const AuthExample = () => (
      <Router>
        <div>
          <AuthButton/>
          <ul>
            <li><Link to="/public">Public Page</Link></li>
            <li><Link to="/protected">Protected Page</Link></li>
          </ul>
          <Route path="/public" component={Public}/>
          <Route path="/login" component={Login}/>
          <PrivateRoute path="/protected" component={Protected}/>
        </div>
      </Router>
    )

    const fakeAuth = {
      isAuthenticated: false,
      authenticate(cb) {
        this.isAuthenticated = true
        setTimeout(cb, 100) // fake async
      },
      signout(cb) {
        this.isAuthenticated = false
        setTimeout(cb, 100)
      }
    }

    const AuthButton = withRouter(({ history }) => (
      fakeAuth.isAuthenticated ? (
        <p>
          Welcome! <button onClick={() => {
            fakeAuth.signout(() => history.push('/'))
          }}>Sign out</button>
        </p>
      ) : (
        <p>You are not logged in.</p>
      )
    ))

    const PrivateRoute = ({ component: Component, ...rest }) => (
      <Route {...rest} render={props => (
        fakeAuth.isAuthenticated ? (
          <Component {...props}/>
        ) : (
          <Redirect to={{
            pathname: '/login',
            state: { from: props.location }
          }}/>
        )
      )}/>
    )

    const Public = () => <h3>Public</h3>
    const Protected = () => <h3>Protected</h3>

    class Login extends React.Component {
      state = {
        redirectToReferrer: false
      }

      login = () => {
        fakeAuth.authenticate(() => {
          this.setState({ redirectToReferrer: true })
        })
      }

      render() {
        const { from } = this.props.location.state || { from: { pathname: '/' } }
        const { redirectToReferrer } = this.state
        
        if (redirectToReferrer) {
          return (
            <Redirect to={from}/>
          )
        }
        
        return (
          <div>
            <p>You must log in to view the page at {from.pathname}</p>
            <button onClick={this.login}>Log in</button>
          </div>
        )
      }
    }

    export default AuthExample

## 커스텀 링크

    import React from 'react'
    import {
      BrowserRouter as Router,
      Route,
      Link
    } from 'react-router-dom'

    const CustomLinkExample = () => (
      <Router>
        <div>
          <OldSchoolMenuLink activeOnlyWhenExact={true} to="/" label="Home"/>
          <OldSchoolMenuLink to="/about" label="About"/>
          <hr/>
          <Route exact path="/" component={Home}/>
          <Route path="/about" component={About}/>
        </div>
      </Router>
    )

    const OldSchoolMenuLink = ({ label, to, activeOnlyWhenExact }) => (
      <Route path={to} exact={activeOnlyWhenExact} children={({ match }) => (
        <div className={match ? 'active' : ''}>
          {match ? '> ' : ''}<Link to={to}>{label}</Link>
        </div>
      )}/>
    )

    const Home = () => (
      <div>
        <h2>Home</h2>
      </div>
    )

    const About = () => (
      <div>
        <h2>About</h2>
      </div>
    )

    export default CustomLinkExample

## 404

    import React from 'react'
    import {
      BrowserRouter as Router,
      Route,
      Link,
      Switch,
      Redirect
    } from 'react-router-dom'

    const NoMatchExample = () => (
      <Router>
        <div>
          <ul>
            <li><Link to="/">Home</Link></li>
            <li><Link to="/old-match">Old Match, to be redirected</Link></li>
            <li><Link to="/will-match">Will Match</Link></li>
            <li><Link to="/will-not-match">Will Not Match</Link></li>
            <li><Link to="/also/will/not/match">Also Will Not Match</Link></li>
          </ul>
          <Switch>
            <Route path="/" exact component={Home}/>
            <Redirect from="/old-match" to="/will-match"/>
            <Route path="/will-match" component={WillMatch}/>
            <Route component={NoMatch}/>
          </Switch>
        </div>
      </Router>
    )

    const Home = () => (
      <p>
        A <code>&lt;Switch></code> renders the
        first child <code>&lt;Route></code> that
        matches. A <code>&lt;Route></code> with
        no <code>path</code> always matches.
      </p>
    )

    const WillMatch = () => <h3>Matched!</h3>

    const NoMatch = ({ location }) => (
      <div>
        <h3>No match for <code>{location.pathname}</code></h3>
      </div>
    )

    export default NoMatchExample

## 재귀적 경로

    import React from 'react'
    import {
      BrowserRouter as Router,
      Route,
      Link
    } from 'react-router-dom'

    const PEEPS = [
      { id: 0, name: 'Michelle', friends: [ 1, 2, 3 ] },
      { id: 1, name: 'Sean', friends: [ 0, 3 ] },
      { id: 2, name: 'Kim', friends: [ 0, 1, 3 ], },
      { id: 3, name: 'David', friends: [ 1, 2 ] }
    ]

    const find = (id) => PEEPS.find(p => p.id == id)

    const RecursiveExample = () => (
      <Router>
        <Person match={{ params: { id: 0 }, url: '' }}/>
      </Router>
    )

    const Person = ({ match }) => {
      const person = find(match.params.id)

      return (
        <div>
          <h3>{person.name}’s Friends</h3>
          <ul>
            {person.friends.map(id => (
              <li key={id}>
                <Link to={`${match.url}/${id}`}>
                  {find(id).name}
                </Link>
              </li>
            ))}
          </ul>
          <Route path={`${match.url}/:id`} component={Person}/>
        </div>
      )
    }

    export default RecursiveExample

## sidebar

    import React from 'react'
    import {
      BrowserRouter as Router,
      Route,
      Link
    } from 'react-router-dom'

    // Each logical "route" has two components, one for
    // the sidebar and one for the main area. We want to
    // render both of them in different places when the
    // path matches the current URL.
    const routes = [
      { path: '/',
        exact: true,
        sidebar: () => <div>home!</div>,
        main: () => <h2>Home</h2>
      },
      { path: '/bubblegum',
        sidebar: () => <div>bubblegum!</div>,
        main: () => <h2>Bubblegum</h2>
      },
      { path: '/shoelaces',
        sidebar: () => <div>shoelaces!</div>,
        main: () => <h2>Shoelaces</h2>
      }
    ]

    const SidebarExample = () => (
      <Router>
        <div style={{ display: 'flex' }}>
          <div style={{
            padding: '10px',
            width: '40%',
            background: '#f0f0f0'
          }}>
            <ul style={{ listStyleType: 'none', padding: 0 }}>
              <li><Link to="/">Home</Link></li>
              <li><Link to="/bubblegum">Bubblegum</Link></li>
              <li><Link to="/shoelaces">Shoelaces</Link></li>
            </ul>

            {routes.map((route, index) => (
              // You can render a <Route> in as many places
              // as you want in your app. It will render along
              // with any other <Route>s that also match the URL.
              // So, a sidebar or breadcrumbs or anything else
              // that requires you to render multiple things
              // in multiple places at the same URL is nothing
              // more than multiple <Route>s.
              <Route
                key={index}
                path={route.path}
                exact={route.exact}
                component={route.sidebar}
              />
            ))}
          </div>

          <div style={{ flex: 1, padding: '10px' }}>
            {routes.map((route, index) => (
              // Render more <Route>s with the same paths as
              // above, but different components this time.
              <Route
                key={index}
                path={route.path}
                exact={route.exact}
                component={route.main}
              />
            ))}
          </div>
        </div>
      </Router>
    )

    export default SidebarExample

## 부분일치

    <Router>
        <div>
          <ul>
            <li><Link to="/about">About Us (static)</Link></li>
            <li><Link to="/company">Company (static)</Link></li>
            <li><Link to="/kim">Kim (dynamic)</Link></li>
            <li><Link to="/chris">Chris (dynamic)</Link></li>
          </ul>

          {/*
              Sometimes you want to have a whitelist of static paths
              like "/about" and "/company" but also allow for dynamic
              patterns like "/:user". The problem is that "/about"
              is ambiguous and will match both "/about" and "/:user".
              Most routers have an algorithm to decide for you what
              it will match since they only allow you to match one
              "route". React Router lets you match in multiple places
              on purpose (sidebars, breadcrumbs, etc). So, when you
              want to clear up any ambiguous matching, and not match
              "/about" to "/:user", just wrap your <Route>s in a
              <Switch>. It will render the first one that matches.
          */}
          <Switch>
            <Route path="/about" component={About}/>
            <Route path="/company" component={Company}/>
            <Route path="/:user" component={User}/>
          </Switch>
        </div>      
    </Router>

## route-config

    import React from 'react'
    import {
      BrowserRouter as Router,
      Route,
      Link
    } from 'react-router-dom'

    // Some folks find value in a centralized route config.
    // A route config is just data. React is great at mapping
    // data into components, and <Route> is a component.

    ////////////////////////////////////////////////////////////
    // first our route components
    const Main = () => <h2>Main</h2>

    const Sandwiches = () => <h2>Sandwiches</h2>

    const Tacos = ({ routes }) => (
      <div>
        <h2>Tacos</h2>
        <ul>
          <li><Link to="/tacos/bus">Bus</Link></li>
          <li><Link to="/tacos/cart">Cart</Link></li>
        </ul>

        {routes.map((route, i) => (
          <RouteWithSubRoutes key={i} {...route}/>
        ))}
      </div>
    )

    const Bus = () => <h3>Bus</h3>
    const Cart = () => <h3>Cart</h3>

    ////////////////////////////////////////////////////////////
    // then our route config
    const routes = [
      { path: '/sandwiches',
        component: Sandwiches
      },
      { path: '/tacos',
        component: Tacos,
        routes: [
          { path: '/tacos/bus',
            component: Bus
          },
          { path: '/tacos/cart',
            component: Cart
          }
        ]
      }
    ]

    // wrap <Route> and use this everywhere instead, then when
    // sub routes are added to any route it'll work
    const RouteWithSubRoutes = (route) => (
      <Route path={route.path} render={props => (
        // pass the sub-routes down to keep nesting
        <route.component {...props} routes={route.routes}/>
      )}/>
    )

    const RouteConfigExample = () => (
      <Router>
        <div>
          <ul>
            <li><Link to="/tacos">Tacos</Link></li>
            <li><Link to="/sandwiches">Sandwiches</Link></li>
          </ul>

          {routes.map((route, i) => (
            <RouteWithSubRoutes key={i} {...route}/>
          ))}
        </div>
      </Router>
    )

    export default RouteConfigExample

## 정적 코드 연결

    import * as React from 'react'
    import { StaticRouter, Route } from 'react-router-dom'

    // This example renders a route within a StaticRouter and populates its staticContext,
    // which it then prints out.
    // In the real world you would use the StaticRouter for server-side rendering:
    //
    // import * as express from 'express'
    // import { renderToString } from 'react-dom/server'
    //
    // const app = express()
    //
    // app.get('*', (req, res) => {
    //     const staticContext = {}
    //
    //     const html = renderToString(
    //         <StaticRouter location={req.url} context={staticContext}>
    //             <App /> (includes the RouteStatus component below e.g. for 404 errors)
    //         </StaticRouter>
    //     )
    //
    //     res.status(staticContext.statusCode || 200).send(html)
    // })
    //
    // app.listen(process.env.PORT || 3000)

    const RouteStatus = (props) => (
        <Route
            render={({ staticContext }) => {
                // we have to check if staticContext exists
                // because it will be undefined if rendered through a BrowserRouter
                if (staticContext) {
                    staticContext.statusCode = props.statusCode
                }

                return (
                    <div>
                        {props.children}
                    </div>
                )
            }}
        />
    )

    const PrintContext = (props) => (
        <p>
            Static context: {JSON.stringify(props.staticContext)}
        </p>
    )

    class StaticRouterExample extends React.Component {

        // This is the context object that we pass to the StaticRouter.
        // It can be modified by routes to provide additional information
        // for the server-side render
        staticContext = {};

        render() {
            return (
                <StaticRouter location="/foo" context={this.staticContext}>
                    <div>
                        <RouteStatus statusCode={404}>
                            <p>Route with statusCode 404</p>
                            <PrintContext staticContext={this.staticContext} />
                        </RouteStatus>
                    </div>
                </StaticRouter>
            )
        }
    }

    export default StaticRouterExample