# 스코프(scope)

## 스코프란

스코프(scope)는 변수, 함수, 객체 등의 식별자(identifier)가 코드의 어떤 부분에서 액세스 가능한지 정의하는 범위이다.
<br/><br/>

## 전역 스코프(global scope)

코드의 어느 곳에서나 액세스할 수 있는 식별자를 의미한다.

전역 스코프에서 선언된 변수는 전역 변수라고 불린다. 전역에 선언되어있어 어느 곳에서든지 해당 변수에 접근할 수 있다.
<br/><br/>

## 지역 스코프(local scope)

특정한 코드 블록 내부에서만 액세스할 수 있는 식별자를 의미한다.
지역스코프에는 함수 스코프와 블록 스코프가 있다.

### 함수 스코프

함수 스코프는 함수에서 선언한 변수는 해당 함수 내에서만 접근 가능하다는 걸 의미한다.

`var` 키워드를 사용하여 선언된 변수는 함수 스코프를 가진다.

- 함수 스코프와 var

```jsx
function abc() {
  var a = '12'; // 함수 내부에서 선언
}
console.log(a); // Uncaught ReferenceError: aa is not defined
```

함수 외부에서 `a` 를 호출하면 `undefined` 에러가 뜬다.

```jsx
if (true) {
  var abc = '123'; // var로 선언하면 블록에 의한 범위 제한이 없음
}
console.log(abc); // 123
```

`var`은 함수 내에서만 지역 변수로 유지되기 때문에, 아래 코드에서는 전역 변수로 취급된다.

```jsx
if (true) {
  const abc = '123'; // const, let은 블록 스코프를 따름
}
console.log(abc); // ReferenceError: abc is not defined
```

<br/>

### 블록 스코프

블록 스코프는 블록내부(`{}`)에서 선언된 변수는 해당 블록에서만 접근 가능한 걸 의미한다.

let, const로 선언된 변수가 블록 스코프 방식을 따른다.
<br/><br/>

### `var` vs `let`, `const` 함수스코프, 블록스코프 뭐가 다른거지?

둘 다 결국에 내부에서 외부변수를 접근할 수 있고, 외부에서는 내부 변수를 접근할 수 없다는 점에서 같은거 아닌가 생각했다. 하지만 둘은 적용되는 영역이 다르다! 이름 그대로 함수 스코프는 함수 내부에만 적용되었던 것...

```jsx
function hello() {
  for (var i = 0; i < 12; i++) {
    ...
  }
  console.log(i)  // 접근가능
}

hello();  // 12
```

- 하지만, `let`, `const`는 블록 스코프를 따르므로 블록 바깥에서는 변수 접근이 불가하다.

```jsx
function hello() {
  for (let i = 0; i < 12; i++) {
    ...
  }
  console.log(i)
}
hello();  // ReferenceError: i is not defined
```

> ES6이전에는 변수 선언시 var을 사용했는데, var은 함수 내에서만 지역변수로 유지된다. 하지만 ES6에 추가된 let, const의 경우 함수가 아닌 블록 단위에서 지역변수로 선언됨을 알 수 있다.

<br/><br/>

## 스코프 예제

```jsx
{
  const x = 'Hello';
  let y = 'world!';
  console.log(x, y);
}

console.log(x);
console.log(y);
/*
Hello world!
Uncaught ReferenceError: x is not defined
*/
```

블록 안에 선언된 변수와 상수를 밖에서 사용 불가<br/><br/>

```jsx
let x = 1;

{
  let y = 2;
  console.log(x, y);
}
console.log(x);
console.log(y);
/*
1 2
1
Uncaught ReferenceError: y is not defined
*/
```

블록 안쪽에서는 바깥의 것 사용 가능<br/><br/>

```jsx
const xx = 0;
let yy = 'Hello!';
console.log(xx, yy);

{
  const xx = 1; // 💡 블록 안에서는 바깥의 const 재선언 가능
  let yy = '안녕하세요~';

  console.log(xx, yy);
  // ⚠️ const, let을 빼먹으면 재선언이 아니라 바깥것의 값을(변수면) 바꿈!
}

console.log(xx, yy);
/*
0 'Hello!'
1 '안녕하세요~'
0 'Hello!'
*/
```

블록 안쪽에 변수나 상수가 새로 선언되면, 그 변수는 블록 안에서만 유효하고, 이 블록 내에서는 외부의 같은 이름의 변수를 가리게 된다. 이런 현상을 **변수 섀도잉(variable shadowing)**이라고 한다.
<br/><br/><br/>

## 스코프 체인

식별자를 결정할 때 활용하는 스코프들의 연결리스트를 말한다.

## 스코프 체이닝

현재 스코프에서 식별자를 검색할 때 상위 스코프를 연쇄적으로 찾아나가는 방식을 말한다.

```jsx
let a = 0;
let b = 1;
let c = 2;
console.log('시점 1:', a, b, c);

{
  let a = 'A';
  let b = 'B';
  console.log('시점 2:', a, b, c);

  {
    let a = '가';
    console.log('시점 3:', a, b, c);
  }

  console.log('시점 4:', a, b, c);
}

console.log('시점 5:', a, b, c);

/*
시점 1: 0 1 2
시점 2: A B 2
시점 3: 가 B 2
시점 4: A B 2
시점 5: 0 1 2
*/
```

블럭 안에 해당 변수/상수가 없으면 바깥쪽으로 찾아나간다.

블럭 밖에서는 내부 변수를 접근할 수 없다.
<br/><br/><br/>

## 렉시컬 스코프(정적 스코프)

상위 스코프를 결정하는 방법엔 동적 스코프, 렉시컬 스코프 두가지가 있다.
렉시컬 스코프는 함수를 어디서 선언하였는지에 따라 상위 스코프를 결정하는 방법을 의미한다. 즉, 코드를 작성하는 시점에 변수가 어디서와 어떻게 참조될 수 있는지가 결정된다. JavaScript 및 대부분의 프로그래밍 언어에서 사용하는 방법이다.

참고로 동적 스코프는 함수를 어디서 호출하였는지에 따라 상위 스코프를 결정하는 방법이다.

<br/><br/><br/>

## 식별자 결정(identifier resolution)

식별자 결정이란 코드에서 변수나 함수의 값을 결정하는 것을 말한다.
그렇다면 자바스크립트에서 식별자 결정은 무엇일까? 바로 자바스크립트 엔진이 스코프 체인을 통해 참조할 변수를 검색하는 것이라고 말할 수 있겠다.

# 참고

[https://www.youtube.com/watch?v=PVYjfrgZhtU](https://www.youtube.com/watch?v=PVYjfrgZhtU)

[https://mong-blog.tistory.com/entry/블록-스코프-함수-스코프의-차이-javascript](https://mong-blog.tistory.com/entry/%EB%B8%94%EB%A1%9D-%EC%8A%A4%EC%BD%94%ED%94%84-%ED%95%A8%EC%88%98-%EC%8A%A4%EC%BD%94%ED%94%84%EC%9D%98-%EC%B0%A8%EC%9D%B4-javascript)
[https://www.yalco.kr/@javascript/3-1/](https://www.yalco.kr/@javascript/3-1/)
