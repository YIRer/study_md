# create-react-app을 이용한 react 기본환경설정
>## 목차
<!-- TOC -->

- [create-react-app을 이용한 react 기본환경설정](#create-react-app을-이용한-react-기본환경설정)
  - [기본 환경 설치](#기본-환경-설치)
    - [create-react-app을 글로벌로 설치](#create-react-app을-글로벌로-설치)
    - [프로젝트 생성](#프로젝트-생성)
    - [설치 후 개발서버 열기(http://localhost:3000)](#설치-후-개발서버-열기httplocalhost3000)
  - [scss 설정하기](#scss-설정하기)
    - [webpack 설정을 위한 패키지 해제](#webpack-설정을-위한-패키지-해제)
    - [scss 사용을 위한 sass-loader, node-sass 설치](#scss-사용을-위한-sass-loader-node-sass-설치)
    - [webpack 설정 변경하기(./config/webpack.config.dev.js)](#webpack-설정-변경하기configwebpackconfigdevjs)
    - [webpack 설정 변경하기(./config/webpack.config.prod.js)](#webpack-설정-변경하기configwebpackconfigprodjs)
    - [전역적으로 사용되는 scss 패스 설정(./config/path.js)](#전역적으로-사용되는-scss-패스-설정configpathjs)
    - [전역적으로 사용되는 scss 패스 설정(./config/webpack.config.dev.js),(./config/webpack.config.prod.js)](#전역적으로-사용되는-scss-패스-설정configwebpackconfigdevjsconfigwebpackconfigprodjs)
    - [include-media 미디어 쿼리 관리](#include-media-미디어-쿼리-관리)
    - [사용법](#사용법)
  - [CSS Module](#css-module)
    - [webpack.config.dev.js, webpack.config.prod.js 설정](#webpackconfigdevjs-webpackconfigprodjs-설정)
    - [사용법 css,scss](#사용법-cssscss)
    - [컴포넌트에서 불러올 때](#컴포넌트에서-불러올-때)
  - [classnames 여러개의 클래스 관리](#classnames-여러개의-클래스-관리)
    - [사용방법](#사용방법)
  - [styled-components 자바스크립트 내부에 스타일링 선언](#styled-components-자바스크립트-내부에-스타일링-선언)
    - [사용법](#사용법-1)
  - [moment 날짜 및 시간 관리 라이브러리](#moment-날짜-및-시간-관리-라이브러리)
  - [의존성 모듈 업데이트](#의존성-모듈-업데이트)
    - [npm-check-updates를 글로벌로 설치 후 패키지 폴더로 이동 후 업데이트](#npm-check-updates를-글로벌로-설치-후-패키지-폴더로-이동-후-업데이트)
    - [패키지 폴더로 이동 후 업데이트](#패키지-폴더로-이동-후-업데이트)
  - [레퍼런스](#레퍼런스)

<!-- /TOC -->

----
## 기본 환경 설치

### create-react-app을 글로벌로 설치

    npm install -g create-react-app


### 프로젝트 생성
    create-react-app project-name

### 설치 후 개발서버 열기(http://localhost:3000)
      //프로젝트폴더내에서 
      npm start

---
## scss 설정하기

### webpack 설정을 위한 패키지 해제
    npm run eject
### scss 사용을 위한 sass-loader, node-sass 설치
    npm install --save node-sass sass-loader

### webpack 설정 변경하기(./config/webpack.config.dev.js)
<pre>
    <code>
        {
          test:/\.css$/,
          (...)
        },
        {
            test: /\.scss$/,
            use: [
              require.resolve('style-loader'),
              {
                loader: require.resolve('css-loader'),
                options: {
                  importLoaders: 1,
                },
              },
              {
                loader: require.resolve('postcss-loader'),
                options: {
                  ident: 'postcss',
                  plugins: () => [
                    require('postcss-flexbugs-fixes'),
                    autoprefixer({
                      browsers: [
                        '>1%',
                        'last 4 versions',
                        'Firefox ESR',
                        'not ie < 9', // React doesn't support IE8 anyway
                      ],
                      flexbox: 'no-2009',
                    }),
                  ],
                },
              },
              {
                loader:require.resolve('sass-loader')
              }
            ],
          },
    </code>
</pre>

### webpack 설정 변경하기(./config/webpack.config.prod.js)

<pre>
    <code>
        {
            test: /\.css$/
            ...
        },
        {
            test: /\.scss$/,
            loader: ExtractTextPlugin.extract(
              Object.assign(
                {
                  fallback: {
                    loader: require.resolve('style-loader'),
                    options: {
                      hmr: false,
                    },
                  },
                  use: [
                    {
                      loader: require.resolve('css-loader'),
                      options: {
                        importLoaders: 1,
                        minimize: true,
                        sourceMap: shouldUseSourceMap,
                      },
                    },
                    {
                      loader: require.resolve('postcss-loader'),
                      options: {
                        ident: 'postcss',
                        plugins: () => [
                          require('postcss-flexbugs-fixes'),
                          autoprefixer({
                            browsers: [
                              '>1%',
                              'last 4 versions',
                              'Firefox ESR',
                              'not ie < 9', // React doesn't support IE8 anyway
                            ],
                            flexbox: 'no-2009',
                          }),
                        ],
                      },
                    },
                    {
                      loader: require.resolve('sass-loader')
                    }
                  ],
                },
                extractTextPluginOptions
              )
            ),
          },
    </code>
</pre>

<p>
웹팩 설정을 변경하고 css대신 scss를 사용하면 정상적으로 적용된다. 전역적으로 공통된 변수나 함수들을 설정하여 필요할때마다 불러오고 싶을 경우 다음과 같이 설정한다.
</p>

### 전역적으로 사용되는 scss 패스 설정(./config/path.js)
    module.exports{
        (...),
        styles: resolveApp('전역적으로 사용될 scss파일의 위치설정')
    }

### 전역적으로 사용되는 scss 패스 설정(./config/webpack.config.dev.js),(./config/webpack.config.prod.js)
    //loader 부분에 options 추가
    {
        loader: require.resolve('sass-loader'),
        options: {
              includePaths: [paths.styles]
        }
    }

### include-media 미디어 쿼리 관리

    npm install --save include-media

### 사용법
    @import '~include-media/dist/include-media'; // node_modules 접근시 ~ 로 접근
    $breakpoints: ( //include-media 라이브러리가 사용하는 종단점 변수
      small: 376px, 
      medium: 768px, 
      large: 1024px, 
      huge: 1200px
    );

    //미디어쿼리를 사용
    
    @include media("<huge") {
      width: 1024px;
    }

    @include media("<large") {
      width: 768px;
    }

    @include media("<medium") {
      width: 90%;
    }
    
    //여러개 사용
    @include media(">phone", "<=tablet") {
      width: 50%;
    }
    // > < = >= =<
    // 자주 사용하는 값들은 따로 저장하여 사용가능
    $my-weird-bp: ">=tablet", "<815px", "landscape", "retina3x";
    @include media($my-weird-bp...) {
      display: inline-block;
    }
    //높이에 따른 미디어 쿼리 적용

    $breakpoints: (small: 320px, medium: 768px, large: 1024px);
    @include media("height>small", "height<=medium") {
      height: 50%;
    }

    // 내부 요소의 미디어 쿼리를 다르게 적용

    @include media-context(('custom': 678px)) {
      .my-component {
        @include media(">phone", "<=custom") {
          border-radius: 100%;
        }
      } 
    }
---

## CSS Module
지역적으로 사용되는 css 클래스명을 고유적인 클래스 스코프로 만들어서 제한.

    box: 'src-App__box--mjrNr'

### webpack.config.dev.js, webpack.config.prod.js 설정

    test: /\.css$/,
                use: [
                  require.resolve('style-loader'),
                  {
                    loader: require.resolve('css-loader'),
                    options: {
                      importLoaders: 1,
                      modules:true,
                      localIdentName : '[path][name]__[local]--[hash:base64:5]'
                    },
                  },
    // sass 사용시, 동일하게 sass에도 적용시킨다.

CSS Loader 설정.

modules: true : 모듈 활성화

localIdentName : `'[path][name]__[local]--[hash:base64:5]'` : 고유적으로 생성될 클래스 네임규칙

### 사용법 css,scss

    .box {
      display: inline-block;
      width: 100px;
      height: 100px;
      border: 1px solid black;
      position: fixed;
      top: 50%;ㅍ
      left: 50%;
      transform: translate(-50%, -50%);
    }

    .blue {
      background: blue;
    }

### 컴포넌트에서 불러올 때

    import React, { Component } from 'react';
    import styles from './App.css'; // styles로 따로 스타일을 정의

    class App extends Component {
      render() {
        return (
          <div className={[styles.box, styles.blue].join(' ')}> // 클래스가 여러개 일때, 배열에 담고 공백을 사이 넣어서 합침

          </div>
        );
      }
    }

export default App;

---
## classnames 여러개의 클래스 관리

    npm install --save classnames

### 사용방법
    const cx = classNames.bind(styles);
    <div className={cx('box', 'blue')}>
    
    //그외 기능
    classNames('foo', 'bar'); // => 'foo bar'
    classNames('foo', { bar: true }); // => 'foo bar'
    classNames({ 'foo-bar': true }); // => 'foo-bar'
    classNames({ 'foo-bar': false }); // => ''
    classNames({ foo: true }, { bar: true }); // => 'foo bar'
    classNames({ foo: true, bar: true }); // => 'foo bar'
    classNames(['foo', 'bar']); // => 'foo bar'

    // 동시에 여러개의 타입으로 받아올 수 도 있습니다.
    classNames('foo', { bar: true, duck: false }, 'baz', { quux: true }); // => 'foo bar baz quux'

    // false, null, 0, undefined 는 무시됩니다.
    classNames(null, false, 'bar', undefined, 0, 1, { baz: null }, ''); // => 'bar 1'
---

## styled-components 자바스크립트 내부에 스타일링 선언

    npm install --save styled-components

자바스크립트 내부에 스타일링을 선언하여, props등을 전달하여 적용이가능

### 사용법

    import React from 'react';
    import styled from 'styled-components';

      const Wrapper = styled.div`
      border: 1px solid black;
      display: inline-block;
      padding: 1rem;
      border-radius: 3px;
      font-size: ${(props)=>props.fontSize};
      &:hover {
        background: black;
        color: white;
      }
    `;

      const StyledButton = ({children, ...rest}) => {
        return (
          <Wrapper fontSize="1.25rem" {...rest}>
            {children}
          </Wrapper>
        );
      };

      export default StyledButton;


---
## moment 날짜 및 시간 관리 라이브러리

    npm install --save moment

날짜와 시간 계산을 위한 라이브러리 [문서](https://momentjs.com/docs/)

---
## 의존성 모듈 업데이트
<p>
  의존성 모듈의 업데이트를 처리해주는 npm-check-update 설치
</p>

  npm install --save react-router-dom sass-loader node-sass redux react-redux react-thunk react-logger react-promise-middleware axio


### npm-check-updates를 글로벌로 설치 후 패키지 폴더로 이동 후 업데이트
    npm i -g npm-check-updates
    ncu -u 혹은 npm-check-updates -u
    npm install

<p>혹은 npm을 이용하여 업데이트</p>

### 패키지 폴더로 이동 후 업데이트
    npm update //global로 설치한 패키지라면 npm update -g 실행
    npm outdated //npm outdated 실행 후 결과 값이 없으면 성공적으로 마무리 된 것

---
## 레퍼런스

[create-react-app](https://github.com/facebookincubator/create-react-app)

[scss 웹팩설정](https://velopert.com/3447)

[npm update](https://docs.npmjs.com/getting-started/updating-local-packages)

[npm check update](https://www.npmjs.com/package/npm-check-updates)