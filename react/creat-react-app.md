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
    npm outdated //npm outdated 실행 후 결과 값이 없으면 성공적으로 마무리 된 것임

---
## 레퍼런스

[create-react-app](https://github.com/facebookincubator/create-react-app)

[scss 웹팩설정](https://velopert.com/3447)

[npm update](https://docs.npmjs.com/getting-started/updating-local-packages)

[npm check update](https://www.npmjs.com/package/npm-check-updates)