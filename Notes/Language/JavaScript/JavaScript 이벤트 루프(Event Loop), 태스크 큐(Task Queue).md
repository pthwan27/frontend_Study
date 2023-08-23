# JavaScript 이벤트 루프(Event Loop), 태스크 큐(Task Queue)

## JS 엔진 : 싱글 스레드

브라우저에 내장된 자바스크립트 엔진은 한 번에 태스크 1개만 처리하는 싱글스레드 방식으로 동작합니다. 그런데 브라우저를 보면 애니메이션 효과를 보여줌과 동시에 유저의 요청 데이터도 가지고 옵니다.

이와 같이 여러가지 일을 동시에 실행할 수 있는 JS의 동시성은 이벤트 루프가 있기 때문에 가능합니다.

## 콜 스택(Call Stack), 메모리 힙(Memory Heap)

![js engine.png](../../../images/Language/JavaScript/JavaScript%20이벤트%20루프\(Event%20Loop\),%20태스크%20큐\(Tas%209a72d89707f34e0e81428a3e8485010b/js_engine.png)

위는 JS의 엔진을 간단하게 표현한 사진입니다.

### 메모리 힙(Memory Heap)

원시타입, 객체 타입이 선언되면 메모리힙에 할당되고 끝나면 자동으로 해제됩니다.

`콜 스택`에서 실행되는 실행 컨텍스트는 힙에 저장된 객체를 참조합니다.

### 콜 스택(Call Stack)

실행해야 할 코드들이 쌓이는 곳입니다.
함수를 호출하면 함수 실행 컨텍스트가 순차적으로 콜 스택에 푸시되고 순차적으로 실행됩니다.
실행 중인 컨텍스트가 종료되어 `콜 스택`에서 제거되기 전에는 다른 태스크는 실행하지 않습니다.

다음과 같은 코드가 실행된다면 스택에 다음과 같이 쌓이며 동작하게 됩니다.

```html
function tenSum(x){
    return x + 10;
}

function printSum(x){
    const sum = tenSum(x);
    console.log(sum);
}
printSum(5);
```

![CallStack.png](../../../images/Language/JavaScript/JavaScript%20이벤트%20루프\(Event%20Loop\),%20태스크%20큐\(Tas%209a72d89707f34e0e81428a3e8485010b//CallStack.png)

## 태스트 큐(Task Queue), 이벤트 루프(Event Loop)

### 태스크 큐(Task Queue)

`setTimeout`이나 `setInterval`과 같은 비동기 함수의 콜백 함수, 이벤트 핸들러가 일시적으로 보관되는 영역입니다.

### 이벤트 루프

`이벤트 루프`는 `콜 스택`에 현재 실행 중인 실행 컨텍스트가 있는지, `태스크 큐`에 대기 중인 함수(콜백 함수, 이벤트 핸들러)들이 있는지 계속해서 확인합니다.

`콜 스택`이 비어 있고 `태스큐 큐`에 대기 중인 함수가 있다면 `이벤트 루프`는 `태스크 큐`에 있는 함수를 `콜 스택`으로 이동시킵니다.

그럼 다음과 같은 코드를 실행한다면 어떻게 될까요?

```jsx
console.log("start", new Date());

setTimeout(()=> { 
    console.log("setTimeout", new Date())
    }, 0);

const wakeUpTime = Date.now() + 1000;
while (Date.now() < wakeUpTime) {}
console.log("test1");
console.log("test2");
console.log("test3");
console.log("end", new Date())
```

![taskQueue.png](../../../images/Language/JavaScript/JavaScript%20이벤트%20루프\(Event%20Loop\),%20태스크%20큐\(Tas%209a72d89707f34e0e81428a3e8485010b/taskQueue.png)

다음과 같은 순서로 실행됩니다.

`setTimeout`메소드는 2번째에 있고 동작 시간을 0으로 처리했지만 콜백 함수 안에 있는 `console.log`는 가장 나중에 출력되었습니다.

`while`문을 통해 블로킹 처리까지 하였음에도 불구하고 왜 콜백 함수는 마지막에 출력되었을까요?

`태스크 큐`는 콜백함수가 대기하는 `Queue`입니다. 즉 선입 선출 구조를 가지고 있습니다.

1. `콜 스택`에 `console.log(”start”)`가 push되고 출력되면서 pop됩니다.
2. `setTimeout`이 `콜스택`에 푸시되고 콜백 내용이 `태스크 큐`에 등록되면서 pop됩니다.
0ms가 지나고 `이벤트 큐`는 `콜 스택`이 비워질 때 까지 기다리며 콜백을 실행시킬 준비를 합니다.
3. wakeUpTime의 변수가 메모리 힙에 올라가고 이 후 `콜스택`들이 동작됩니다.

이처럼 비동기 처리가 발생하면 콜백 함수는 `콜 스택`에서 처리되는게 아닌 별도의 `테스크 큐`에서 쌓여있다 `이벤트 루프`에 의해 실행되기 때문에 다음과 같은 결과가 나오게 되는 것입니다.

ex ) 

```JavaScript
console.log('first');
setTimeout(
    function f2() {
        console.log('second');
    }
, 0);

f3();
console.log('third');

function f3() {
    // do Something
}
```

→ first , third, second 순으로 실행됩니다.

---

## 정리

### JavaScript Engine

JavaScript Engine은 `Memory Heap`과 `Call Stack`으로 구성되어 있고, 싱글 스레드 방식으로 하나의 Call Stack으로 동작합니다.

- Memory Heap: 메모리 할당이 이뤄지는 곳입니다.
- Call Stack: 코드가 실행될 때 호출 스택이 쌓이는 곳입니다. (호출된 함수가 call stack에 push 됩니다.)

### Task Queue, Event Loop

`Task Queue`는 비동기적으로 이벤트 발생 시 실행해야 할 callback 함수가 추가 되는 곳입니다.

`Event Loop` 는 `Call Stack`과 `Task Queue`의 상태를 체크하고, `Call Stack`이 빈 상태가 되면 `Task Queue`의 첫 번째 콜백 함수를 `Call Stack`으로 밀어넣어 줍니다.

`Event Loop`와 `Task Queue`는 자바스크립트 엔진이 하나의 코드 조각을 하나씩 처리할 수 있도록 작업을 스케줄하는 동시에 자바스크립트에서 비동기 작업을 할 수 있도록 해줍니다.
