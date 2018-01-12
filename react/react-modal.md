# React-Tool kit

## 목차
<!-- TOC -->

- [React-Tool kit](#react-tool-kit)
  - [목차](#목차)
  - [react-modal](#react-modal)
    - [기본사용법](#기본사용법)
    - [API](#api)
    - [주의 사항](#주의-사항)

<!-- /TOC -->
## react-modal

    npm install react-modal

리액트 내에서 모달 팝업을 쉽게 사용할 수 있게 해주는 라이브러리

### 기본사용법

    <ReactModal ...options > contents </ReactModal>

### API

`isOpen`={false} : 모달이 보여지는지 선택하는 값, boolean
  
`onAfterOpen`={handleAfterOpenFunc} : 모달이 열린후 실행할 함수, function

`onRequestClose`={handleRequestCloseFunc} : 모달이 닫히거나 esc키를 눌러서 실행할 함수, function

`closeTimeoutMS`={0} : 모달이 닫히기전에 대기할 시간, number, ms

`style`={{ overlay: {}, content: {} }} : 모달에 사용할 스타일을 나타내는 객체, 인라인으로 들어가기 때문에 클래스로 스타일 주려면 스타일을 따로 적용, object

`contentLabel`="Example Modal" : 컨텐트 컨테이너를 스크린 리더에게 읽히게 할 이름, string

`portalClassName`="ReactModalPortal" : 포털(모달 외곽)에 적용할 클래스, string

`overlayClassName`="ReactModal__Overlay" : 오버레이에 적용할 클레스명, string

`className`="ReactModal__Content" : 모달에 적용할 클래스명, string

`bodyOpenClassName`="ReactModal__Body--open" document.body에 적용할 클래스명, string

`ariaHideApp={true}` appElement를 숨겨야하는지의 여부, boolean

`shouldFocusAfterRender`={true} : 랜더링 후에 모달이 포커스 되어야하는가, boolean

`shouldCloseOnOverlayClick`={true} : 오버레이를 클릭하면 모달이 닫히는지 여부, boolean

`shouldCloseOnEsc`={true} : esc를 이용하여 모달을 닫게 할 수 있는지의 여부, 접근성 문제로 인해 활성화 권장, boolean

`shouldReturnFocusAfterClose`={true} :  모달이 열리기 이전 요소에 포커스를 해야하는가에 대한 여부, boolean

`role`="dialog" : 모달의 역할을 나타내는 role=dialog 적용의 여부, 접근성으로 인해 모달과 같은 오버레이를 통한 창을 띄우는 경우 사용을 권장, "dialog"

`parentSelector`={() => document.body} = 모달이 사용될 부모요소를 가져옴

`aria`={{ labelledby: "heading", describedby: "full_description" }} : 추가 aira속성 사용, object
>

### 주의 사항

AppElement를 설정하지 않으면 오류가 발생하므로, `ReactModal.setAppElement('#root');`등으로 설정하여 맨 위에 정의

모달이 여러개 있는 경우에는 단순이 컴포넌트를 추가하는 방법으로 작업

기본 스타일

    {
      overlay : {
        position          : 'fixed',
        top               : 0,
        left              : 0,
        right             : 0,
        bottom            : 0,
        backgroundColor   : 'rgba(255, 255, 255, 0.75)'
      },
      content : {
        position                   : 'absolute',
        top                        : '40px',
        left                       : '40px',
        right                      : '40px',
        bottom                     : '40px',
        border                     : '1px solid #ccc',
        background                 : '#fff',
        overflow                   : 'auto',
        WebkitOverflowScrolling    : 'touch',
        borderRadius               : '4px',
        outline                    : 'none',
        padding                    : '20px'
      }
    }

    ReactModal.defaultStyles.overlay.backgroundColor//기본 스타일 설정예시
