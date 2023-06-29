# React

## **Frontend Framework**

시간이 지날수록 웹에서 사람들과 상호작용할 부분은 늘고 사용자가 원하는 기능 또한 많아지고 있습니다. 이러한 상황속에서 FE 개발자들이 기술적 한계를 느끼고 새로운 기술인 FE Framework가 나오게 됩니다.

![FE의 기술 성장 ](../../images/React/React%EB%9E%80/FE%20%EA%B8%B0%EC%88%A0%20%EC%84%B1%EC%9E%A5.png)

FE의 기술 성장 

### **Framework와 Library의 차이**

프레임워크과 라이브러리의 공통점은 다른 사람이 만들어 둔 코드라는 점입니다. 

둘의 차이는 Framework는 다른 사람이 만든 툴 **안으로 들어가서** 작업을 하는 것이라면 Library는 내 작업에서 다른 사람이 만든 코드를 **가져와서** 사용하는 것 입니다.

## **React**

그렇다면 React는 뭘까요?

React는 UI 구축을 위한 JS 라이브러리입니다. 
React를 사용하면 재사용 가능한 UI 구성요소를 만들 수 있습니다. 
이를 통해 컴포넌트를 레고처럼 다루어 조립하듯 UI를 개발하고 유지보수를 할 수 있습니다. 
또한 React를 사용하게 된다면 페이지 전체를 렌더링 하지 않고 필요한 부분만 렌더링 할 수 있습니다.

### **왜 React를 사용할까 ?**

1. **Virtual DOM**
    
    첫번째로 Virtual DOM입니다.
    
    프로젝트 규모가 커지고, 다양한 유저 인터랙션이 전달된다면 그만큼 DOM 요소들 또한 변화가 이루어져야 합니다. DOM 요소들이 변화한다는 것은 랜더 트리 재생성, 요소의 스타일 계산, 레이아웃 구성, 패인팅 하는 과정을 거쳐야 한다는 것과 같습니다. 결국, 유저 인터랙션이 전달되는 만큼, 이와 같은 과정이 반복되어야 한다는 것입니다.
    
    리액트를 사용하게 된다면 실제로 DOM을 제어하지 않고 중간에 Virtual DOM 이라는 가상의 DOM을 두어 Virtual DOM이 변경될 때 실제 DOM을 변경되도록 설계되어 있습니다.
    
    덕분에 개발자는 DOM관리, 산태 변화 관리는 최소화하고 기능 개발, UI 인터페이스 개발에 보다 집중할 수 있게 도와줍니다.
    
    - **Virtual DOM의 내부 작동원리**
        
        유저 인터랙션에 의해 View에 변화가 발생하여 10개의 노드를 수정해 주어야 한다면, 10번의 레이아웃 재계산, 10번의 리랜더링이 필요하다는 것입니다.
        
        Virtual DOM은 변화가 발생하면, 실제 DOM에 적용되기 전에 Virtual DOM에 우선 적용을 시켜봅니다. 실제 DOM에 바로 적용하나, Virtual DOM에 적용하나 같은 연산 비용이 필요할 거라 생각하실 수 있지만, Vitual DOM은 랜더링 과정이 필요 없기 때문에 연산 비용이 실제 DOM보다 적습니다.
        
        Virtual DOM에서 이러한 연산이 끝나고 나면, 최종적인 변화를 실제 DOM에 전달해줍니다. 즉, 10번의 작업을 하나로 묶어 딱 한 번 전달해 줍니다. 물론, 레이아웃 계산과 리랜더링하는 과정의 규모는 커지겠지만, 횟수를 줄이는 것으로 충분히 연산 비용을 적게 만들어 줍니다.
        
        또한, Virtual DOM은 어떤 게 바뀌었는지, 어떤 게 바뀌지 않았는지 자동으로 파악하여 필요한 DOM 트리만 업데이트할 수 있게 해줍니다.
        
        Virtual DOM의 값은 직접 변경할 수 없는데 그 이유는 React의 state의 불변성을 유지해야 하기 때문입니다. 컴포넌트는 setState를 비교해서 업데이트가 필요한 경우에만 render() 함수가 호출되는데 state를 직접 수정시 render() 함수를 호출하지 않아 상태 변경이 일어나도 렌더링이 일어나지 않을 수 있습니다.
        

1. **컴포넌트 단위 개발**
    
    **컴포넌트는 UI를 구성하는 개별적인 뷰 단위**로서, UI를 개발을 레고라고 한다면, 컴포넌트는 블록 역할을 하게 됩니다. 이러한 블록을 조립해 하나의 완성품을 만드는 것과 같습니다. 이러한 특징은 하나의 컴포넌트를 여러 부분에서 사용할 수 있게 해줍니다. 가령, 웹 애플리케이션의 여러 곳에 버튼이 필요하다면, 공통된 하나의 버튼 컴포넌트를 생성하고 그 컴포넌트를 필요한 곳에 가져다 사용하면 되죠.
    
    이러한 특징은 **생산성과 유지 보수를 용이하게 합니다.** 하나의 요소의 변화가 다른 요소들의 변화에 영향을 미치는 복잡한 로직을 업데이트하는 까다로운 작업에 경우, 컴포넌트의 재사용 기능으로서 보완할 수 있게 됩니다.
    

1. **JSX의 지원**
    
    JSX 는 JavaScript와 xml이 합쳐진 JS에 대한 확장 구문으로 리액트에서 element를 제공해 줍니다. 
    아래 return ( ) 로 감싸져 있는 HTML 문법과 매우 유사한 코드가 JSX입니다.
    
    ```jsx
    import React from "react";
    import logo from "./logo.svg";
    import "./App.css";
    
    const App = () => {
      return (
        <div className="App">
          <header className="App-header">
            <img src={logo} className="App-logo" alt="logo" />
            <h1 className="App-title">Welcome to React</h1>
          </header>
          <p className="App-intro">
            To get started, edit <code>src/App.js</code> and save to reload.
          </p>
        </div>
      );
    };
    
    export default App;
    ```
    
    이를 create-react-app을 통해 생성 할 때 포함되어 있는 Babel이 아래와 같은 코드로 변환하여 컴파일 해줍니다.
    
    ```jsx
    "use strict";
    
    Object.defineProperty(exports, "__esModule", {
      value: true
    });
    exports.default = void 0;
    
    var _react = _interopRequireDefault(require("react"));
    
    var _logo = _interopRequireDefault(require("./logo.svg"));
    
    require("./App.css");
    
    function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }
    
    const App = () => {
      return /*#__PURE__*/_react.default.createElement("div", {
        className: "App"
      }, /*#__PURE__*/_react.default.createElement("header", {
        className: "App-header"
      }, /*#__PURE__*/_react.default.createElement("img", {
        src: _logo.default,
        className: "App-logo",
        alt: "logo"
      }), /*#__PURE__*/_react.default.createElement("h1", {
        className: "App-title"
      }, "Welcome to React")), /*#__PURE__*/_react.default.createElement("p", {
        className: "App-intro"
      }, "To get started, edit ", /*#__PURE__*/_react.default.createElement("code", null, "src/App.js"), " and save to reload."));
    };
    
    var _default = App;
    exports.default = _default;
    ```
    
    덕분에 우리는 익숙한 HTML문법과 유사한 JSX를 통해 컴포넌트를 생성할 수 있게 됩니다.
    
2. **SSR , CSR 지원**
    - **SSR 
    서버쪽에서 렌더링 준비를 끝마친 상태로 클라이언트에 전달하는 방식입니다.**
    
    ![SSR](../../images/React/React%EB%9E%80/ssr.png)
    
    1. User가 Website 요청을 보냅니다.
    2. Server는 'Ready to Render'. 즉, 즉시 렌더링 가능한 html파일을 만듭니다.(리소스 체크, 컴파일 후 완성된 HTML 컨텐츠로 만든다.)
    3. 클라이언트에 전달되는 순간, 이미 렌더링 준비가 되어있기 때문에 HTML은 즉시 렌더링이 됩니다. 
    그러나 사이트 자체는 조작이 불가능합니다. (Javascript가 읽히기 전)
    4. 클라이언트가 자바스크립트를 다운받습니다.
    5. 다운 받아지고 있는 사이에 유저는 컨텐츠는 볼 수 있지만 사이트를 조작 할 수는 없고, 이때의 사용자 조작을 기억하고 있습니다.
    6. 브라우저가 Javascript 프레임워크를 실행합니다.
    7. JS까지 성공적으로 컴파일 되었기 때문에 기억하고 있던 사용자 조작이 실행되고 이제 웹 페이지는 상호작용 가능해집니다.
    
    - **CSR**
        
        **SSR과 달리 렌더링이 클라이언트 쪽에서 일어납니다.
        즉, 서버는 요청을 받으면 클라이언트에 HTML과 JS를 보내줍니다.
        클라이언트는 그것을 받아 렌더링을 시작합니다.**
        
    ![CSR](../../images/React/React%EB%9E%80/csr.png)
        
        1. User가 Website 요청을 보냅니다.
        2. CDN이 HTML 파일과 JS로 접근할 수 있는 링크를 클라이언트로 보냅니다.
            
            <aside>
            💡 CDN : aws의 cloudflare를 생각하면 됩니다.
             엔드 유저의 요청에 '물리적'으로 가까운 서버에서 요청에 응답하는 방식
            
            </aside>
            
        3. 클라이언트는 HTML과 JS를 다운로드 받습니다.(이때 SSR과 달리 유저는 아무것도 볼 수 없습니다.)
        4. 다운이 완료된 JS가 실행되고 데이터를 위한 API가 호출됩니다.
        (이때 유저들은 placeholder를 보게됩니다. )
        5. 서버가 API로부터의 요청에 응답합니다.
        6. API로부터 받아온 data를 placeholder 자리에 넣어주고, 이제 페이지는 상호작용이 가능해집니다.
        

참고 
[https://velog.io/@youthfulhps/React-React를-사용하는-이유](https://velog.io/@youthfulhps/React-React%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)

[https://hymndev.tistory.com/45](https://hymndev.tistory.com/45)