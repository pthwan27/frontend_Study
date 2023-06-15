# redux의 라이프 사이클

# Redux ?

Redux란 JavaScript 상태 관리 라이브러리로 복잡한 컴포넌트 구조 속에서 간편하게 모든 컴포넌트들이 state를 통해 데이터를 쉽게 공유할 수 있게 해주는 방식

컴포넌트에 집중된 리액트와 시너지가 좋아 리액트와 많이 사용하는 것이지 리액트의 라이브러리가 아니라 JS의 라이브러리입니다.

## Redux 사용 이유?

## 세 가지 원칙

1. **Single source of truth**
   - 동일한 데이터는 항상 같은 곳에서 가지고 온다!
   - 즉, 스토어라는 하나뿐인 데이터 공간이 있다는 의미
2. **State is read-only**
   - `react`에서는 `setState`메소드를 활용해야만 상태 변경이 가능하다
   - `reudx`에서도 `Action`이라는 객체를 통해서만 상태를 변경할 수 있다!!
3. **Chages are made with pure fucntions**
   - 변경은 순수함수로만 가능하다.
   - 리듀서와 연관되는 개념이다.
   - Store(스토어) - Action(액션) - Reducer(리듀서)

## Store, Action, Reducer의 의미와 특징

![간단한 라이프사이클](../../../images/CS/redux%EC%9D%98%20%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4/)

### Store

> **Store는 상태가 관리되는 오직 하나의 공간이다.**

- 컴포넌트와는 별개로 Store라는 공간이 있어 그 안에서 필요한 상태를 담는다
- 컴포넌트에서 상태정보들이 필요할 때 스토어에 접근한다!

### **Action**

> **Action은 앱에서 스토어를 운반할 데이터를 말한다
> Action은 자바스크립트 객체 형식**

```jsx
{
  type: 'ACTION_CHANGE_USER', // 필수
  payload: { // 옵션
    name: '하나몬',
    age: 100
  }
}
```

### Reducer(리듀서)

- `Action`을 `Store`에 바로 전달하는 것이 아 니 다 !
- `Action`을 `Reducer`에 전달해야한다.
- `Reducer`가 주문을 보고 `Store`의 상태를 업데이트 하는 것이다
- `Action`을 `Reducer`에 전달하기 위해서는 `Dispatch( )` 메소드를 이용해야한다

### **정리하자면!**

`Action(액션) 객체`가 `dispatch()`메소드에 전달된다.

`dispatch(액션)`을 통해 `Reducer`를 호출한다.

`Reducer`는 `Store`를 생성한다

## 왜 이런 흐름이 있는걸까 ?

바로바로 데이터가 한 방향으로만 흘러야하기 때문!

![Untitled](../../../images/CS/redux%EC%9D%98%20%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4/Redux%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4.png)

## 요런 리덕스를 사용하려면 ?

우리가 프로젝트를 하며 사용했던 `useSelector`를 통해 `State`에 데이터를 사용하고,
`useDispatch`를 통해 `Action`객체를 전달해 `Reducer`를 호출해 데이터를 변경할 수 있다!

## 리덕스의 장점 ?

- 상태를 예측 가능하게 만든다 ( 순수함수를 사용하기 때문 )
- 유지보수 (복잡한 상태관리를 줄일 수 있다)
- 디버깅에 유리
  (action, state의 log가 기록되기 때문에 redux dev tool(크롬 확장 프로그램)을 이용해 쉽게 디버깅 할 수 있다.)
