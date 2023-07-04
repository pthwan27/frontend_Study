# 클로저(Closure)

- **함수와 그 함수가 선언된 렉시컬 환경과의 조합**
- 어떤 함수 A에서 선언한 변수 a를 참조하는 내부 함수 B를 외부로 전달할 경우 A의 실행 컨텍스트가 종료된 이후에도 변수 a가 사라지지 않는 현상<br/><br/>

## 클로저 예시1

```javascript
function doSomething() {
  const x = 10;
  function sum(y) {
    return x + y;
  }
  return sum;
}

const something = doSomething();
console.log(something(3)); // 13
```

위 예시에서 클로저는 `sum`함수와 그 함수가 선언된 렉시컬 환경과의 조합이다. `sum`함수는 `x`라는 외부 변수를 참조하고 있으며, 이 `x` 값은 `sum`함수가 선언된 시점의 값(10)을 **기억**하고 있다.

즉, `doSomething` 함수에서 선언한 x변수를 참조하는 내부함수를 외부로 전달할 경우 x변수가 사라지지 않는다.

`something` 상수에서는 `sum` 함수의 참조가 저장되어 있다. 이 함수를 호출할 때 `x`는 여전히 10이라는 값을 기억하므로 `something(3)`은 10 + 3, 즉 13을 반환한다.
결론적으로, 이 코드는 클로저를 사용하여 내부 함수가 외부 함수의 지역 변수를 계속 참조하고 사용할 수 있음을 보여준다.<br/><br/>

## 클로저 예시2

```javascript
function makeMultiplier(x) {
  return function (y) {
    return x * y;
 링: 클로저를 사용하여 동적으로 함수를 생성할 수 있다. 즉, 함수를 공장처럼 사용하여 필요에 따라 다른 함수를 반환할 수 있다. 부분 적용을 통해 함수의 일부 인자를 미리 고정하여 새로운 함수를 생성하는데 사용할 수 있고, 커링을 통해 함수를 단일 인자만 받는 함수들의 연속으로 변환할 수도 있다.

  ```javascript
  // 부분 적용
  function multiply(a) {
    return function (b) {
      return a * b;
    };
  }

  const double = multiply(2);
  console.log(double(4)); // 출력: 8

  // 커링
  function multiply(a) {
    return function (b) {
      return function (c) {
        return a * b * c;
      };
    };
  }

  const multiplyBy2 = multiply(2); // a를 2로 고정
  const multiplyBy2And3 = multiplyBy2(3); // b를 3으로 고정
  console.log(multiplyBy2And3(4)); // c를 4로 하여, 2 * 3 * 4를 계산, 출력: 24
  ```

  <br/><br/>

- 모듈 패턴: JavaScript에서 객체를 통해 공개(public) 메서드와 비공개(private) 메서드를 만들고, 이를 캡슐화하는데 사용된다. 클로저가 여기서 중요한 역할을 하는데, 그 이유는 클로저가 함수의 스코프와 변수를 기억하기 때문이다.

  모듈 패턴을 사용하면 전역 네임스페이스를 깔끔하게 유지할 수 있으며, 외부에서 직접 접근할 수 없는 비공개 상태를 만들 수 있다. 이는 코드의 안정성과 유지 보수성을 높이는데 도움이 된다.

  ```javascript
  // 모듈 패턴
  const myModule = (function () {
    // 비공개 변수
    let counter = 0;

    // 비공개 메서드
    function logCounter() {
      console.log('Current counter:', counter);
    }

    // 공개할 객체 리턴
    return {
      // 공개 메서드
      increment: function () {
        counter++;
        logCounter();
      },
      reset: function () {
        counter = 0;
        logCounter();
      },
    };
  })();

  myModule.increment(); // Current counter: 1
  myModule.increment(); // Current counter: 2
  myModule.reset(); // Current counter: 0
  ```

  <br/><br/>

## 클로저 단점

- **메모리 소비**: 클로저는 외부 함수의 변수를 참조하고 있기 때문에, 이 변수들은 메모리에 계속 남아있다.
  <br/><br/>

# 참고

https://www.youtube.com/watch?v=PJjPVfQO61o&t=141s

https://haesoo9410.tistory.com/342
