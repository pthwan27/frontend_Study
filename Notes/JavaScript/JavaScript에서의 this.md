# **this**

## **this ?**

객체는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드로 이루어져 있습니다.

![this설명](../../images/JavaScript/JavaScript%EC%97%90%EC%84%9C%EC%9D%98%20this/this%20%EC%84%A4%EB%AA%85.png)

만약 메서드가 자신이 속한 객체의 Property를 참조하려면, <br/> 자신이 속한 객체(Object)를 가리키는 식별자를 참조할 수 있어야한다!

이럴 때 다음과 같이 직접적인 객체 이름으로 명시할 수 도 있지만

```jsx
const someone = {
	name: 'mario',
	getName() {
		return someone.name;
	},
} 
```

대신 **this**를 사용해 자신의 속한 객체나 자신이 생성할 인스턴스를 참조할 수 있습니다.

```jsx
const someone = {
	name: 'mario',
	getName() {
		return this.name;
	},
} 
```

**this**는 기본적으로 함수를 호출할 때 결정됩니다.

![this의 실행 컨텍스트](../../images/JavaScript/JavaScript%EC%97%90%EC%84%9C%EC%9D%98%20this/this%EC%9D%98%20%EC%8B%A4%ED%96%89%20%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8.png)

여기서 printThis() 함수를 호출하면 실행컨텍스트가 생성되고<br/> **this**는 실행 컨텍스트가 생성될 때 함께 결정되기 때문에 **this**는 함수를 호출할 때 결정될 수 있다고 할 수 있습니다.


<br/>

## **this 바인딩 규칙**

**this**와 **this**가 가리킬 객체를 **this** 연결하는 것을 **this** 바인딩이라고 하는데 이는 함수 호출 방식에 의해 결정됩니다.

함수호출 방식은 

1. 일반 함수 호출 - 기본 바인딩
2. 메서드 호출 - 암시적 바인딩
3. 생성자 함수 호출 - new 바인딩
4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출 - 명시적 바인딩

총 4가지가 있습니다.

<br/>

### **1. 일반 함수 호출 - 기본 바인딩**

```jsx
function printThis() { 
	console.log(this);
}
printThis();
```

일반함수 호출 시 기본적으로 this는 전역객체가 바인딩 됩니다. 
Browse → window , Node → global

```jsx
function printThis() {
	`use strict` 
	console.log(this);
}
printThis(); -> undefined
```

하지만 엄격모드(**strict mode**)에서는 undefined가 바인딩됩니다.

<br/>


```jsx
1번 -> 중첩함수
const obj = {
	foo() { 
		function boo() {
			console.log(this);
		}
		boo();
	},
};

obj.foo();

2번 -> 콜백함수
const obj = {
	foo() { 
		setTimeout(function () {
			console.log(this);
		},0);
	},
};

obj.foo();

```

**중첩함수(함수 내에서 또 다른 함수를 선언하여 사용하는 함수)** 와 <br/> **콜백함수(setTimeout 함수)** 의 공통점은 **this**는 전역객체를 바인딩한다는 것입니다.

중첩함수, 콜백함수를 사용하는 것은 내부의 일부로직을 대신하는 경우가 많은데 이렇게 된다면 this를 사용하여 원하는 메소드의 접근을 수월하게 할 수 없게 됩니다.

```jsx
const me = {
	name: 'mario',
	printName() {
		const self = this;
		setTimeout(function() {
			console.log(self.name);
		},0);	
	}
}
me.printName();
```

이를 해결하기 위해 위와 같이 this로 사용하고자 하는 객체를 변수에 할당하여 사용할 수 있는데 위와 같은 경우에 mario가 출력됩니다.

하지만 **권장하는 방법은 아닙니다.**
<br/>

### **2. 메서드 호출 - 암시적 바인딩**
```jsx
const outer = {
	inner : {
		printThis() {
			console.log(this);
		},
	},
};

outer.inner.printThis();
```

위 예시에서 printThis()의 this는 inner가 되는데 **함수를 객체의 메소드로 호출할 경우 this는 메서드를 호출한 객체**입니다!

### **3. 생성자 함수 호출 - new 바인딩**

```jsx
function Someone(name) {
	this.name = name;
}
const me = new Someone('mario');
```

생성자 함수는 이름 그대로 **객체, 인스턴스를 생성하는 함수**입니다.

생성자 함수 내부 this에는 함수를 사용하여 생성할 인스턴스가 바인딩됩니다.

위에서는 me 가 바인딩 됩니다.

### **4. apply/call/bind 메서드에 의한 간접 호출 - 명시적 바인딩**

3가지의 메소드는 Function.prototype 의 메서드로써 <br/> 모든 함수가 상속받아 사용할 수 있습니다.

이중 apply, call은 특징이 비슷합니다.

```jsx
Function.prototype.apply(thisArg, argsArray)
Function.prototype.call(thisArg, arg1, arg2, …)
```

호출할 함수의 인수를 전달하는 방식만 다르고 
this로 사용할 객체 전달하며 알아서 함수를 호출하는 점에서 동일하게 동작합니다.

```jsx
Function.prototype.bind(thisArg, arg1, arg2, …)
```

bind도 apply, call 처럼 this로 사용할 객체와 인수를 전달받지만 함수를 호출해주지 않습니다.

<br/>

## **this 바인딩 규칙 우선순위**
### **암시적 바인딩 vs 명시적 바인딩**

```jsx
const someone = {
	name : 'mario',
	printName() {
		console.log(this.name);	
	}
};

someone.printName.bind({ name : 'luigi' })();
```

`someone.printName`
printName을 someone의 메소드를 호출하면서 <br/> this를 someone으로 암시적 바인딩
`this.name = ‘mario’`

`bind({ name : 'luigi' })`
bind 메서드로 this를 ‘luigi’라는 이름을 가진 객체로 <br/> 명시적 바인딩
`this.name = ‘luigi’`

이 경우 명시적 바인딩이 된 luigi가 나오는데 <br/> 명시적 바인딩이 암시적 바인딩보다 우선순위가 높기 때문입니다.

<br/>


### **명시적 바인딩 vs new 바인딩**

```jsx
function setName(name){
	this.name = name;
}
let someone = {
	name : 'mario',
};

const setNameBindSomeone = setName.bind(someone);

someone = new setNameBindSomeone('luigi');
```

`setName.bind(someone);`
setName 함수에 someone객체를 <br/> 명시적 바인딩하는 함수
`this.name = ‘mario’`

`new setNameBindSomeone('luigi');`
<br/> new 바인딩 
`this.name = ‘luigi’`

위의 경우 `luigi`가 나옵니다.

new 바인딩이 명시적 바인딩보다 우선순위가 높기 때문입니다.

new > 명시적 > 암시적 > 기본

<br/>


## **화살표 함수의 this**

```jsx
const arrowFn = () ⇒ {
	console.log('화살표');
}
```

ES6에서 도입된 화살표 함수

일반함수에서 this가 전역객체를 바라보는 문제를 해결하고자 등장

**this**는 실행 컨텍스트가 생성될 때 함께 결정되지만 화살표 함수는 실행 컨텍스트를 생성할 때 this binding과정 자체가 빠지게 되어 함수 내에 this가 존재하지 않습니다!

따라서 화살표 함수 내에서 this를 호출하면 상위 스코프의 this에 접근합니다.

```jsx
const someone = {
	name : 'mario',
	printName: () => {
		console.log(this.name);
	},
};

someone.printName();
```

`someone.printName();` 암시적 바인딩에 의해 this가 someone으로 바인딩 되었다고 생각할 수도 있지만! <br/>
실행해본다면 브라우저에서는 빈 문자열, 노드에서는 undefined가 나옵니다.

이는 스코프 체인에 의해 따라간 가장 가까운 this가 전역객체이기 때문입니다.

![this 화살표 함수 스코프체이닝 과정](../../images/JavaScript/JavaScript%EC%97%90%EC%84%9C%EC%9D%98%20this/%ED%99%94%EC%82%B4%ED%91%9C%ED%95%A8%EC%88%98%EC%97%90%EC%84%9C%EC%9D%98%20this%20%EC%8A%A4%EC%BD%94%ED%94%84%20%EC%B2%B4%EC%9D%B8.png)

때문에 가장 가까운 상위 scope의 this가 전역객체일 때에는 추상메서드를 쓰는 것이 좋습니다.

```jsx
	printName() {
		console.log(this.name);
	},
```
<br/>

## **클래스에서의 this**

앞에 설명에서 **엄격모드(strict mode)**일 때 기본 바인딩이 전역객체가 아닌 undefined가 된다고 설명했습니다. 

클래스에서는 **strict mode**가 기본적으로 적용됩니다.

일반적으로 클래스 내부에서 this 호출 시 class를 통해 생성한 인스턴스 객체로 바인딩 됩니다.

만약 메서드 내부에 **중첩 함수, 콜백 함수**를 일반 함수로 선언, 호출하였을 때 클래스 내부는 엄격모드이기 때문에 this가 undefined로 됩니다. 

동일하게 this를 사용하고 싶다면 **명시적 바인딩**, **화살표 함수**등을 사용하면 됩니다.

다음은 예시입니다.

```jsx
var word = "good morning";
class helloWorld {
    word = "good evening";
    greetings(){
        console.log(this.word);
    };
}

let obj = new helloWorld();
obj.greetings(); 
```

이렇게 한다면 `good evening`이 출력됩니다

```jsx
class helloWorld {
    greetings() {
        function test() {
            console.log(this.word);
        }        
        test();
    }
};

let obj = new helloWorld();
obj.greetings(); 
```

하지만 이런식으로 일반함수를 선언하고 출력한다면 undefined가 나오는데요.

`this.word`에서 `this`의 값이 예상과 다른 객체를 참조하기 때문에 undefined가 출력됩니다!

이를 해결하기 위해서는 

화살표 함수를 사용하여 자신의 `this`를 갖지 않고, 외부 스코프의 `this`를 그대로 사용하도록 해주거나  

생성자 함수를 사용하여 `greetings()` 메서드가 생성된 객체의 속성에 접근하여 값을 출력할 수 있도록 해줘야 합니다.

```jsx
class HelloWorld {
    constructor() {
        this.word = "good night";
    }

    greetings() {
        const test = () => {
            console.log(this.word);
        }
        
        test();
    }
}

let obj = new HelloWorld();
obj.greetings();
```

## **정리**

어렵다 어렵다 어려워

### **this binding rule**
**우선순위는 new 바인딩, 명시적 바인딩, 암시적 바인딩, 기본 바인딩 순서입니다.|**

`생성자 함수 호출(new 바인딩)` 시<br/> 
인스턴스 객체로 바인딩 됩니다.

`apply, call, bind에 의한 간접 호출(명시적 바인딩)`시 <br/>첫 번째 인자 객체로 주어지는 객체로 바인딩 됩니다.

`메서드 호출(암시적 바인딩)`에선 <br/> 
호출 객체로 바인딩 됩니다

`일반 함수 호출 (기본 바인딩)` 에서는<br/>
비엄격모드 → 전역객체
엄격모드 undefined

### **화살표 함수**

this를 바인딩 하지 않기 때문에 스코프 체인에 따라 상위 스코프 this에 접근합니다.

### **class**

클래스 내부는 엄격모드가 적용되고 일반적으로  this는 인스턴스 객체가 바인딩되고 중첩함수, <br/> 콜백함수로 일반 함수 선언, 호출 시 this에 undefined가 바인딩 됩니다. 

이를 해결하기 위해서는 constructor 또는 화살표함수를 사용합니다. 



<br/><br/><br/><br/>
출처 : 

[https://www.youtube.com/watch?v=DTX52Pv7PZM&list=PLgXGHBqgT2TvpJ_p9L_yZKPifgdBOzdVH&index=148](https://www.youtube.com/watch?v=DTX52Pv7PZM&list=PLgXGHBqgT2TvpJ_p9L_yZKPifgdBOzdVH&index=148)

[https://navydoc.tistory.com/43](https://navydoc.tistory.com/43)
