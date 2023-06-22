# 고차 컴포넌트(HOC)와 hooks

[label](command:workbench.trust.manage)

## HOC

### 고차 컴포넌트란?

고차 컴포넌트(HOC, Higher Order Component)는 컴포넌트 로직을 재사용하기 위한 React의 고급 기술입니다. 고차 컴포넌트(HOC)는 React API의 일부가 아니며, React의 구성적 특성에서 나오는 패턴입니다 <br/><br/>

**다시말해!!**

**고차 컴포넌트는 컴포넌트를 매개 변수로 받아 특정 로직을 포함하는 새로운 컴포넌트를 반환하는 함수입니다!!**

```jsx
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

**요런식으로 !** <br/><br/>

구체적인 예시로

### withLogined

```jsx
import { useContext } from "react";
import { LoginContext } from "../context/LoginContext";

export function withLogined(InputComponent) {
  return () => {
    const { isLogined } = useContext(LoginContext);

    return isLogined ? <InputComponent /> : <h3>로그인 부탁 드림!</h3>;
  };
}
```

로그인이 되어있는지 상태 체크 HOC 코드일 때

```jsx
export default withLogined(UserInfo);
```

로그인 체크 로직을 각 컴포넌트마다 작성할 필요없이 위와 같이 HOC를 활용해 해결할 수 있습니다.

또한 hover되어있는지도 hoc를 통해 확인할 수도 있습니다! <br/><br/>

### withHover

```jsx
import { useState } from "react";

export default function withHover(InnerComponent) {
  return (props) => {
    const [isHovered, setIsHovered] = useState(false);

    function handleMouseEnter() {
      setIsHovered(true);
    }

    function handleMouseLeave() {
      setIsHovered(false);
    }

    return (
      <InnerComponent
        {...{
          ...props,
          handleMouseEnter,
          handleMouseLeave,
          isHovered,
        }}
      />
    );
  };
}
```

해당 withHover 코드는 useState를 통해 마우스를 올리면 특정 로직을 실행시킬 수 있도록 합니다.

```jsx
import withHover from "../hoc/withHover";
import "../styles/ImageBox.css";

function ImageBox({ imageUrl, imageTitle, isHovered, handleMouseEnter, handleMouseLeave }) {
  return (
    <div>
      {isHovered && <div id="hover">{imageTitle}</div>}
      <img
        src={imageUrl}
        alt={imageTitle}
        width="400px"
        onMouseEnter={handleMouseEnter}
        onMouseLeave={handleMouseLeave}
      />
      <h5>이미지에 마우스를 올리면 이미지 제목이 표시됩니다.</h5>
    </div>
  );
}

export default withHover(ImageBox);
```

ImageBox 컴포넌트에 withHover를 적용한 코드입니다!

이미지에 마우스를 올릴 경우 이미지 타이틀이 페이지 상단에 표시되는 것을 확인할 수 있습니다.

하지만 이런 HOC를 대신하여 CustomHook을 사용할 수도 있습니다!!

<br/>

## React Hooks

### Hooks란???

기존 React 컴포넌트들이 작성되던 방식인 Class의 형태는 사람과 기계 양측 모두에게 좋지 않은 점들이 있었고 끊임없이 React 개발자들에게 문제를 야기해왔습니다.

하지만, function을 이용한 함수형 React 컴포넌트에서는 state를 관리하기 어려운 큰 문제점이 있었고 그에 따라 어쩔 수 없이 수많은 레거시 컴포넌트들이 Class로 작성되어 왔습니다.

이러한 문제들을 해결하고자 함수형 컴포넌트에서 state를 관리할 수 있도록 도와주는 recompose라는 라이브러리가 등장하였고, React 팀이 이 recompose를 인수하여 Hooks가 탄생하게 되었습니다.

**★ Hooks의 역할은 대표적으로 useState, useEffect 등과 같은 라이브러리를 제공하여 함수형 React 컴포넌트에서 state를 관리할 수 없었던 문제를 해결하는 것이며, 그로 인해 React 개발자들이 좀 더 함수형 프로그래밍을 지향할 수 있게 되었습니다.**

위에 HOC를 사용해 withLogined를 적용한 코드를
Hooks를 이용해 만들어보면

```jsx
import { useContext } from "react";
import { LoginContext } from "../context/LoginContext";

export default function useLogin() {
  const { isLogined, setIsLogined } = useContext(LoginContext);

  function loginAlert() {
    if (!isLogined) {
      alert("로그인 후 진행해주세요.");
    } else {
      alert("로그인이 완료된 상태에요^^");
    }
  }

  return { isLogined, setIsLogined, loginAlert };
}
```

이렇게 Hook을 만들고

```jsx
import useLogin from "../hook/useLogin";

function UserInfo() {
  const { isLogined } = useLogin();
  return isLogined ? (
    <div>
      <h4>환영합니다! 유저분!</h4>
      <h5>해당 내용은 로그인한 상태에서만 보입니다!</h5>
    </div>
  ) : (
    <h3>로그인 부탁 드림!</h3>
  );
}

export default UserInfo;
```

이런식으로 사용하면 됩니다!

### HOC vs Hooks

그렇다면 HOC, hooks는 무슨 차이일까요?

HOC를 사용하다보면 컴포넌트 구조의 복잡성을 유발할 수 도 있습니다.

```jsx
export default withLogined(withHover(withStyles(ImageBox)));
```

이런식으로 계속해서 HOC를 늘려나간다면 디버깅 시 불필요하게 depth가 커질수도 있습니다.

hooks를 사용한다면 이런 불필요한 depth를 줄일 수 있습니다.

또한 HOC를 사용한다면 암묵적인 props의 전달이 일어나는데 hooks를 사용한다면 이런 점도 해소할 수 있습니다.

<br/>

### 그러면 hooks만 쓰면 되는거 아닌가?

그렇지만 Hooks가 HOC보다 나은 개념은 아니며, 두 기법의 장단점을 가지고 있습니다. 따라서 HOC를 적절히 활용해 중복코드를 제거하고, 컴포넌트의 조합을 통해 유연한 구조를 만들 수 있습니다.

출처 : https://ostarblog.tistory.com/12
