# 호이스팅(Hoisting)

## 호이스팅이란?

- 코드가 실행하기 전 `변수선언/함수선언`이 해당 스코프의 최상단으로 끌어 올려진 것 같은 현상을 말한다.
- 인터프리터가 변수와 함수의 메모리 공간을 선언 전에 미리 할당하는 것을 의미한다.

</br>

## 예시

```javascript
console.log(a); // undefined
var a = 5;
console.log(a); // 5
```

코드를 보면 `a`가 선언되기 전에 이미 참조되고 있다.
자바스크립트 엔진이 호이스팅 시 변수의 선언과 초기화 (undefined으로) 같이 시켜버린다.

```javascript
console.log(hello()); // "Hello, world!"

function hello() {
  return 'Hello, world!';
}
```

함수 선언은 변수 호이스팅과 달리 전체 본문이 호이스팅 되므로, 선언문보다 먼저 함수를 호출할 수 있다.

</br>

## let과 const, class에 대한 호이스팅

ES6에 도입된 `let`과 `const`는 `var`와 다르게 동작하는데, '일시적 사각 지대(Temporal Death Zone: TDZ)'라는 개념 때문이다.
TDZ는 변수가 호이스팅되었지만, 아직 선언되지 않은 상태를 나타낸다.
즉 `let`과 `const` 변수는 선언 전에는 TDZ에 있으며, 이 시간 동안에는 변수에 접근하려고 하면 에러가 발생한다.

```javascript
console.log(a); // ReferenceError: a is not defined
let a = 5;
console.log(a); // 5
```

위 코드의 첫번째 줄에서 `a`는 아직 TDZ에 있기 때문에 참조를 하려고 하면 에러가 발생한다.
`a`의 선언문이 실행되면 TDZ를 빠져나와 접근할 수 있게 된다.

</br>

## 변수 할당과 함수 표현식, 화살표 함수에 대한 호이스팅

- '변수 선언'과 '함수 선언'은 호이스팅이 되지만, '변수 할당'은 호이스팅의 대상이 아니다. 변수에 값을 할당하는 작업은 해당 코드가 실제로 실행되는 위치에서 이루어진다.
- 함수 표현식과 화살표 함수는 호이스팅의 대상이 아니다. 함수 표현식과 화살표 함수가 변수에 할당되는 값이기 때문이다.

```javascript
console.log(hello); // undefined
console.log(hello()); // TypeError: hello is not a function

var hello = function () {
  return 'Hello, world!';
};

console.log(hello()); // "Hello, world!"
```

# 참고

https://developer.mozilla.org/ko/docs/Glossary/Hoisting</br>
https://hanamon.kr/javascript-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85%EC%9D%B4%EB%9E%80-hoisting/
