# 실행 컨텍스트(Execution Context)

## 실행 컨텍스트란?

코드를 실행하는데 필요한 조건이나 상태를 모아둔 객체이다.

자바스크립트 엔진이 스크립트 파일을 실행하기 전에 **글로벌 실행 컨텍스트(Global Execution Context, GEC)** 가 생성되고, 함수를 호출할 때마다 **함수 실행 컨텍스트(Function Execution Context, FEC)** 가 생성된다. 주의할 점은, 글로벌의 경우 실행 이전에 생성되지만 함수의 경우 호출할 때 생성된다는 점이다.

## 실행 컨텍스트 스택

실행 컨텍스트가 생성되면 콜 스택(Call Stack)이라고도 불리는 실행 컨텍스트 스택에 쌓이게 된다.

글로벌 실행 컨텍스트는 코드를 실행하기 전에 쌓이고 모든 코드를 실행하면 제거된다. 함수 실행 컨텍스트는 호출할 때 쌓이고 호출이 끝나면 제거된다.

코드와 함께 보자.

```jsx
function cal(type, a, b) {
  if (type === 'add') {
    return a + b;
  } else if (type === 'subtract') {
    return a - b;
  } else if (type === 'multiply') {
    return a * b;
  } else {
    return a / b;
  }
}

var four = 4;
var seven = 7;
cal('add', 4, 7);
```

<img src="../../images/JavaScript/execution-context/callstack.png">

위의 그림을 간단하게 설명해보겠다.

1. cal() 함수를 호출하면
2. 새로운 전역 실행 컨텍스트가 생성되어 자바스크립트 엔진은 콜 스택(컨텍스트 스택)에 해당 실행 컨텍스트를 담는다.(전역에서 함수를 호출할 경우 해당 함수의 실행 컨텍스트를 생성하여 또 콜 스택에 담는다.) + 기본적으로 스택에 전역 실행 컨텍스트가 존재한다.
3. 콜스택에서는 가장 최근에 추가된 실행 컨텍스트만 활성화 된다(LIFO)

## 실행 컨텍스트 구성요소

- Lexical Environment
- Variable Environment
- this binding

하나씩 살펴보자.

---

## Lexical Environment(렉시컬 환경 또는 정적 환경)

렉시컬 환경은 실행 컨텍스트의 구성요소로, Lexical Environment는 Environment Record와 Outer Environment Reference를 포함한다.</br></br>

### Environment Record(환경 레코드)와 호이스팅

```jsx
console.log(a); // undefined
var a = 5;
console.log(a); // 5
```

호이스팅은 선언문이 마치 최상단에 끌어올려진 듯한 현상이다. 호이스팅이 발생하는 이유는 선언문이 있는 코드라인을 물리적으로 최상단에 끌어 올렸기 때문이 아니라, 자바스크립트 엔진이 먼저 전체 코드를 스캔하면서 **변수 같은 정보를 실행 컨텍스트의 Record에 저장**해놓기 때문이다. (간단하게 record라고 표현하겠다!)

즉, Record는 **식별자와 식별자에 바인딩된 값을 기록해두는 객체**이다.
</br></br>

### Outer Environment Reference(외부 환경 참조)와 스코프체이닝

콜스택 안에 동일한 식별자가 여러개일 때 자바스크립트 엔진은 어떻게 Outer Environment Reference를 활용해서 의사결정을 하는지 알아보겠다. (간단하게 outer라고 표현하겠다!)

```jsx
var x = 1;
function foo() {
  var y = 2;
  function bar() {
    var z = 3;
    function baz() {
      console.log(z);
      console.log(y);
      console.log(x);
      console.log(w);
    }
    baz();
  }
  bar();
}
foo();
// 3
// 2
// 1
// Reference Error: w is not defined
```

<img src="../../images/JavaScript/execution-context/outer.png">

outer는 이전 렉시컬 환경을 가리킨다. 즉, 함수는 자신이 선언된 위치를 기억하는데, 이것을 기억하고 있는 것이 바로 outer이다.</br></br>

<img src="../../images/JavaScript/execution-context/outer2.png">

이 outer를 통해 함수는 자신이 선언된 렉시컬 환경과 그 환경의 외부 환경을 참조할 수 있으며, 이를 통해 스코프 체인을 형성하게 된다.</br></br>

<img src="../../images/JavaScript/execution-context/lexical.png">

이러한 메커니즘 덕분에 함수는 자신이 어디서 호출되었는지가 아니라 어디에서 선언되었는지에 따라 상위 스코프를 결정하게 되는데 이를 렉시컬 스코핑 이라고한다.</br></br>

**스코프 체이닝** <br/>
실행 컨텍스트가 쌓여있을 때 식별자를 결정하기 위해 활용하는 스코프들의 연결 리스트

**변수 쉐도잉**<br/>
식별자가 같은 것이 있을 때 상위 스코프의 선언된 식별자의 값이 가려지는 현상

---

## Variable Environment(변수 환경)

실행 컨텍스트 내에서 변수 식별자와 그에 해당하는 값들을 추적하고 관리한다.

### Variable Environment와 Lexical Environment 뭐가 다른거지?

사실 둘 다 식별자와 해당 값들을 추적하고 관리하며 동일한 특성과 구성 요소를 가지고 있다. 그렇기 때문에 Variable Environment에도 Record와 Outer가 그대로 적용이 된다. 그렇다면 뭐가 다른것일까? ES6에서부터 Variable Environment는 var 변수와 함수 선언문을 처리하고 변수에 접근하고 값을 할당하는 데 사용되고 Lexical Environment는 let, const 변수, 함수 선언문, 블록 스코프와 관련된 작업을 처리한다.</br></br>

---

## this binding(this 바인딩)

`this` 식별자가 바라봐야 할 대상 객체.

ES6부터 this의 바인딩이 Lexical Environment 안에 있는 Record 안에서 일어난다

- 글로벌 실행 컨텍스트의 경우,strict mode라면 `undefined` 로 바인딩되고 아니라면 글로벌 객체로 바인딩된다. (브라우저에선 window, 노드에선 global)
- 함수 실행 컨텍스트의 경우, 해당 함수가 어떻게 호출되었는지에 따라 바인딩된다.</br></br>

---

## 실행 컨텍스트 과정

1. 생성단계(Creation Phase)
   - Lexical Environment 정의:let과 const 키워드로 선언된 변수를 매핑하며 초기값을 할당하지 않습니다. 또한 함수 전체가 메모리에 할당된다.
   - Variable Environment 정의: var 키워드로 선언된 변수만 매핑되며 초기값으로 undefined가 할당된다.
   - this 바인딩: this 키워드의 값이 결정된다
   - Outer Environment 연결: 외부 환경에 대한 참조(Outer Environment)가 형성되어 스코프 체인을 구성합니다.
2. 실행단계(Execution Phase)
   - 코드 실행하며 Envrionment Records에 저장된 식별자의 메모리에 값을 수정 혹은 할당 시켜주는 작업을 진행한다.

# 출처

[https://www.youtube.com/watch?v=EWfujNzSUmw](https://www.youtube.com/watch?v=EWfujNzSUmw)

[https://betterprogramming.pub/execution-context-lexical-environment-and-closures-in-javascript-b57c979341a5](https://betterprogramming.pub/execution-context-lexical-environment-and-closures-in-javascript-b57c979341a5)

[https://github.com/baeharam/Must-Know-About-Frontend/blob/main/Notes/javascript/execution-context.md](https://github.com/baeharam/Must-Know-About-Frontend/blob/main/Notes/javascript/execution-context.md)

[https://velog.io/@wooogie/실행-컨텍스트-Execution-Context](https://velog.io/@wooogie/%EC%8B%A4%ED%96%89-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8-Execution-Context)
