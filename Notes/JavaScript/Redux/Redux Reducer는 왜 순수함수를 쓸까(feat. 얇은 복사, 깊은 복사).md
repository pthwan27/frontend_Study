# Redux Reducer는 왜 순수함수를 쓸까?

## **순수 함수**

**순수함수**란 동일한 인자가 주어졌을 때 항상 동일한 결과를 반환해야 하며 **외부의 상태를 변경하지 않는 함수**이다. 즉 함수 내 변수 외에 외부의 값을 참조, 의존하거나 변경하지 않아야한다.

```jsx
const num_arr = [1, 2, 3, 4, 5];

// 매개변수의 값을 직접 변경하는 불순함수
const addSixImpure = (arr) => {
  // 매개변수에 직접 6 추가
  arr.push(6);
  return arr;
};

// 매개변수를 복사한 값을 변경하는 순수함수
const addSixPure = (arr) => {
  // 펼침 연산자로 새로운 배열에 6 추가
  newArr = [...arr, 6];
  return newArr;
};
```

## Redux Reducer가 순수함수를 쓰는 이유

1. Reducer 함수는 이전 상태와 액션 객체를 파라미터로 받는다!
2. 여기서 이전의 전달받은 매개변수(state, actions)는 절대로 건드리지 않고, **변화를 일으킨 새로운 상태 객체를 만들어서 반환**한다!
3. 똑같은 파라미터로 호출된 리듀서 함수는 언제나 똑같은 결과값을 반환해야 한다!

## **새로운 상태 객체를 만들어서 반환해야 하는 이유가 뭘까 ?**

이를 알기 위해서는
**얕은 복사(Shallow Copy)와 깊은 복사(Deep Copy)** 를 살짝 공부하고 넘어가야한다!

### 얕은 복사(Shallow Copy)

얕은 복사는 참조(주소)값의 복사를 의미한다.

```jsx
const obj = { value: 10 };
const newObj = obj;

newObj.value = 5;

console.log(obj.value); // 2
console.log(newObj.value); // 2
```

복사 후 newObj 객체의 value를 변경하니 기존 obj.value값도 함께 변경되어있다

→ 이는 데이터가 새로 생긴것이 아닌 해당 데이터의 참조 값을 전달하여 결국 한 데이터를 가지고 공유한것!

### 깊은 복사(Deep Copy)

깊은 복사는 값 자체의 복사를 나타내 다른 주소값을 지닌다.

```jsx
const obj = { value: 10 };
const newObj = { ...obj };

newObj.value = 5;

console.log(obj.value); // 10
console.log(newObj.value); // 5
```

---

### **새로운 상태 객체를 만들어서 반환해야 하는 이유?**

우리가 Reducer를 통해 state를 업데이트 할 때

```jsx
reducers: {
    saveOrderState: (state, action) => {
      return {
        ...state,
        orderState: action.payload,
      };
    },
}
```

이런식으로 … ⇒ spread연산자를 이용해 복사한 후 수정한다.
<br/>

### **Redux의 변경감지 알고리즘**

이렇게 하는 이유는 **Redux의 변경감지 알고리즘** 때문인데
redux는 reducer를 거친 state가 변경됐는지 검사하기 위해 **state 객체의 주소**를 비교한다.

그런데 얇은 복사를 한다? 주소가 같다라는 말이 되어 변경감지가 되지 않는다라고 할 수 있다.

따라서 spread 연산자(…) , Object.assign() 메소드를 통해 state의 깊은 복사를 통해 복제본을 만들어야 state가 변경되었다고 판단된다!

## **정리**

결국 우리가 순수함수를 써야하는 이유는 Side Effect를 막고
**이전 State 주소값과 리턴되는 state의 주소값을 비교해서 변경을 감지해야 하기 때문이다!**

### **Side Effect(부수 효과)**

**부수 효과(side effects)** 란 **함수가 만들어진 목적과는 다른 효과 또는 부작용!!**
더 쉽게 말하면 함수에 **예상할 수 없는 일이 생길 가능성**이 존재한다면 이 함수는 부수 효과를 가질 수 있는 함수다~!!

부수효과가 없는 함수는 **순수함수(pure function)**, 부수효과가 있는 함수는 불순함수(impure function)라고 한다!
