## Redux

### <span style="color:red;  font-weight:bold"> Q. </span>

> Redux에 대해 간단한 설명

<span style="color:#00ff00;  font-weight:bold"> A. </span> <br/>

> Redux란 JavaScript 상태 관리 라이브러리로 복잡한 컴포넌트 구조 속에서 간편하게 모든 컴포넌트들이 state를 통해 데이터를 쉽게 공유할 수 있게 해주는 방식

> JavaScript 상태(데이터) 관리 라이브러리로 Flux의 개념인 단뱡향으로 state를 변경할 수 있는 라이브러리 입니다.

---

### <span style="color:red;  font-weight:bold"> Q. </span>

> Redux를 왜 써야할까?

<span style="color:#00ff00;  font-weight:bold"> A. </span> <br/>

> 규모가 커질수록 데이터가 많아지고 컴포넌트들간의 관계가 복잡해집니다.
> 이 때 많은 데이터들이 공유하는 상황에서 단방향 데이터 흐름을 통해 데이터들을 효율적이고 간단하게 관리하기 위해 사용합니다.

---

### <span style="color:red;  font-weight:bold"> Q. </span>

> Redux의 장점

<span style="color:#00ff00;  font-weight:bold"> A. </span> <br/>

> 단방향 흐름(Flux 아키텍처)을 통해 코드의 유지보수를 쉽게 만들 수 있다.

> 훌륭한 미들웨어들이 존재한다!
> 특히 비동기 액션을 처리할 때 리덕스 미들웨어를 통해 깔끔한 코드를 만들 수 있다.<br/>-> 뷰에 해당하는 컴포넌트에 비동기 코드가 붙는다면 재사용하기 힘들다

> 상태를 예측 가능하게 만든다 !<br/>( 순수함수를 사용하기 때문 )

> 디버깅에 유리
> (action, state의 log가 기록되기 때문에 redux dev tool(크롬 확장 프로그램)을 이용해 쉽게 디버깅 할 수 있다.)

---

### <span style="color:red;  font-weight:bold"> Q. </span>

> Redux의 특징, 구조 ?

<span style="color:#00ff00;  font-weight:bold"> A. </span> <br/>

> Redux는 크게 Store, Action, Reducer로 이루어져 있고
> Store는 상태가 관리되는 하나의 공간이고 </br>
> Action 은 Reducer에 어떤 작업을 할 지에 대해 보내주는 것 입니다.</br>
> 마지막으로 Reducer는 Action에게 받은 객체를 통해 state를 업데이트해주는 것입니다.

> 간편버전 </br>
> 상태들을 관리하는 하나의 Store에 dispatch를 통해 action객체를 전달하고 이를 통해 Reducer가 Store에 저장되어있는 데이터들을 바꾸는 것입니다.

### <span style="color:red;  font-weight:bold"> Q. </span>

> Redux가 순수함수를 쓰는 이유?

<span style="color:#00ff00;  font-weight:bold"> A. </span> <br/>

> 결국 우리가 순수함수를 써야하는 이유는 Side Effect를 막고
> 이전 State 주소값과 리턴되는 state의 주소값을 비교해서 변경을 감지해야 하기 때문입니다.
