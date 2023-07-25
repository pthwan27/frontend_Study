# 불변성(Immutability)이란?
불변성은 데이터가 한번 생성된 후에는 변경될 수 없음을 의미한다.</br></br>

# 불변성의 중요성
- 예측 가능성: 데이터의 불변성을 유지하면 데이터가 예기치 않게 변하지 않기 때문에 애플리케이션의 상태를 예측하기 쉽다.
- 버그 감소: 데이터의 변화는 복잡성을 가져올 수 있기 때문에 데이터의 변화를 줄이면 버그의 가능성이 줄어든다.
- 성능 최적화: React와 같은 라이브러리/프레임워크에서는 불변성을 통해 컴포넌트 재렌더링 여부를 효율적으로 판단한다.</br></br>


# 원시타입 vs 객체타입
## 원시타입(Primitive Types)
자바스크립트의 원시타입(`Boolean`, `Number`, `String`, `null`, `ndefined`, `Symbol`, `BigInt`)은 기본적으로 불변이다. 즉, 한번 값을 할당하면 그 값을 바꿀 수 없다.

```javascript
let str = "hello";
str[0] = "H";
console.log(str);	// "hello" => 문자열이 변경되지 않음
```

### 잠깐🤚 여기서 궁금한 점이 생겼다!
```javascript
let n = 2;
n = 1;
console.log(n);	// 1
```
"불변인데 왜 바뀌는 거지?" 라는 생각이 들었다.
찾아보니 원시 타입의 값 자체가 불변하다는 것은 **값이 메모리에 할당된 후 그 값을 변경할 수 없다**는 뜻이다. 그러나 변수에 새로운 원시 값을 할당하는 것은 허용이된다. 위의 코드에서 에서 `n`은 원래의 원시 값 자체를 바꾸는 것이 아닌 참조하는 메모리 위치가 변경된 것이다.

즉, 변수에 다른 원시 값을 재할당하는 것은 가능하지만, 해당 원시 값을 변경하는 것은 불가능하다.

## 객체 타입(Object Types)
반면, 객체(Object), 배열(Array), 함수(Function) 등의 참조 타입은 가변적이다.
```javascript
let obj = { name: "Alice" };
obj.name = "Bob";
console.log(obj); // { name: "Bob" }, 객체 내용이 변경됨
```
</br></br>

# 객체의 불변성 유지 방법
객체나 배열을 직접 수정하는 대신 복사본을 만들어서 작업한다.

## 얕은 복사(Shallow Copy)
얕은 복사는 객체의 최상위 프로퍼티만 복사하는 방법이다. 
```javascript
const obj = { a: 1, b: 2 };

// Object.assign() 메서드를 사용한 복사
const copy = Object.assign({}, obj);
copy.a = 3;

console.log(obj);  // { a: 1, b: 2 }
console.log(copy); // { a: 3, b: 2 }
```

만약 복사하려는 객체에 중첩된 객체나 배열이 있다면, 복사된 객체의 해당 프로퍼티는 원본 객체의 참조를 공유하게 된다.

```javascript
const obj = {
    a: 1,
    b: {
        c: 2
    }
};

const shallowCopy = Object.assign({}, obj);

shallowCopy.b.c = 3;
console.log(obj.b.c); // 3, 원본 객체의 값도 변경됨

```

## 깊은 복사(Deep Copy)
깊은 복사는 객체의 모든 레벨의 프로퍼티를 복사하는 방법이다. 복사된 객체는 원본 객체와 완전히 독립적이다. `JSON.parse`와 `JSON.stringify` 또는 라이브러리를 사용할 수 있다.

```javascript
const obj = { a: 1, b: { c: 2 } };
const deepCopy = JSON.parse(JSON.stringify(obj));

deepCopy.b.c = 3;
console.log(obj.b.c); // 2, 원본 객체의 값은 변하지 않음
```

## ES6의 Spread 연산자
Spread 연산자는 배열이나 객체를 개별 요소나 프로퍼티로 확장시키는 데 사용된다.
```javascript
const arr = [1, 2, 3];
const newArr = [...arr, 4]; // [1, 2, 3, 4]

const obj = { a: 1, b: 2 };
const newObj = { ...obj, c: 3 }; // { a: 1, b: 2, c: 3 }
```

spread 연산자는 최상위 레벨의 프로퍼티만 복사하기 때문에 복사하려는 객체 내부의 객체는 불변성을 유지할 수 없다. 만약 내부 객체까지 불변성을 유지해주려면 별도의 변수에 값을 재할당하고 넣어주는 번거로운 과정을 거쳐야 한다.
```javascript
const user = {
  name: 'John',
  address: {
    city: 'Seoul',
    country: 'South Korea'
  }
};

const updatedUser = {
  ...user,
  address: {
    ...user.address,
    city: 'Busan'
  }
};

console.log(updatedUser);
```
## immer
`immer`는 불변성을 유지하면서 객체나 배열을 쉽게 업데이트할 수 있게 해주는 자바스크립트 라이브러리이다.

라이브러리의 주요 함수인 `produce`를 사용하면, 일반적인 객체나 배열의 수정 방법으로 불변성을 유지하면서 업데이트를 수행할 수 있다.


```javascript
import produce from 'immer';

const user = {
  name: 'John',
  address: {
    city: 'Seoul',
    country: 'South Korea'
  }
};

const updatedUser = produce(user, draft => {
  draft.address.city = 'Busan';
});

console.log(updatedUser);
// {
//   name: 'John',
//   address: {
//     city: 'Busan',
//     country: 'South Korea'
//   }
// }

```

`produce` 함수는 두 개의 인자를 받는다. 첫 번째 인자는 업데이트할 원본 객체이고, 두번째 인자는 변화를 적용할 콜백함수이다. 이 콜백 함수에서는 `draft`라는 객체를 수정하게 되는데, 이 `draft`는 원본 객체의 프록시 버전이다. 콜백 함수의 실행이 끝나면 `immer`는 우리가 `draft`에 가한 변화를 바탕으로 새로운 객체를 반환한다.
