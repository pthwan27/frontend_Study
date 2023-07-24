# JS에서의 클래스와 객체

## 시작하기 전

대부분 객체 지향 언어에서는 클래스를 통해 객체를 만드는 것이 일반적입니다.

하지만 JS는 다릅니다!
JS는 웹 브라우저에서만 동작하도록 설계되었고, 러닝 커브와 언어의 복잡도를 최소화하는데 초점을 맞췄습니다.
그 결과  **정적 타입, 클래스와 같은 기능들을 포기**하였고 **동적 타입**을 지원하고 **클래스는 없지만 프로토타입 객체를 사용**하며 **함수형 프로그래밍**을 하는 **프로토타입 기반 객체지향 프로그래밍 언어**, JavaScript가 만들어지게 되었습니다. 

## 클래스와 객체(feat : Prototype)

우리가 일반적으로 알고 있는 붕어빵, 붕어빵 틀이 대표적인 예시 입니다!

### 클래스

- template, 틀과 같은 역할을 합니다.
- 단 한 번만 선언합니다.
- 클래스 안에는 데이터가 없습니다.
- 메모리에도 올라가지 않습니다.

### 객체

- instance of class, 클래스의 인스턴스입니다.
- 1개의 클래스로 여러개가 만들어질 수 있습니다.
- 객체 안에는 데이터가 있습니다.
- 메모리에도 올라갑니다.

### JavaScript의 클래스
 **그런데 JS는 class기반의 언어로 된 것은 아닙니다**<br/>
 프로토타입은 유전자라고도 볼 수 있습니다! <br/>
 프로토타입은 객체이고 부모 유전자에 기록이 되어 있으면 자식들도 사용할 수 있습니다. <br/> 또한 프로토타입에 내용을 추가하면 자식요소들이 사용 가능합니다. <br/>
 
 이런 **프로토타입의 객체를 확장하고 객체 지향적인 프로그래밍을 할 수 있는 점을 이용해 클래스를 만드는 것**입니다! 

## **ES5 클래스**

클래스가 등장하기 전 JavaScript는 생성자 함수를 사용해 객체를 생성했습니다.

```jsx
class FunctionCar = (function() {
	function FunctionCar(name){
		this.name = name;
		this.position = 0;
	}
	FunctionCar.prototype.move = function() {
		this.position += 1;
	};

	return FunctionCar;
}());

const funcCarItem = new FunctionCar("꼬마자동차");
console.log(carItem);  // FunctionCar {name : '꼬마자동차', position : 0}

funcCarItem.move();
console.log(carItem);  // FunctionCar {name : '꼬마자동차', position : 1}
```

## **ES6 클래스**

**클래스 문법의 특징** <br/>

1. **es6 부터 class 키워드를 쓸 수 있게 되었습니다.**
2. new가 없으면 에러 <br/>
	-> 생성과 동시에 생성자 함수가 실행되며 새로운 인스턴스가 할당 <br/>
	`const carItem = new Car("꼬마자동차");`
3. extends와 super를 지원 <br/>
	-> 생성된 인스턴스는 클래스 속성과 메소드를 동일하게 가집니다.

4. 항상 use strict 모드
5. 프로토 타입으로 변환됩니다. </br>
`console.log(carItem);  // Car {name : '꼬마자동차', position : 0}`

```jsx
class Car {
	constructor(name){
		this.name = name;
		this.position = 0;
	}
	
	move() {
		this.position += 1;
	}
}
const carItem = new Car("꼬마자동차");
console.log(carItem);  // Car {name : '꼬마자동차', position : 0}

carItem.move();
console.log(carItem);  // Car {name : '꼬마자동차', position : 1}
```
</br>
“4번 프로토 타입으로 변환됩니다”  알아보기 위해 다음과 같이 실행해본다면

```jsx
// 클래스는 함수입니다.
console.log(typeof Car); // function

// 정확하게 말하면 클래스는 생성자 메서드와 동일합니다.
console.log(Car === Car.prototype.constructor); // true

// 클래스 내부에서 정의한 메서드는 Car.prototype에 저장됩니다. 
// 일반적인 메서드는 프로퍼티에 저장됩니다.
console.log(Car.prototype.move); //[Function : move]

// 현재 프로토타입의 메서드를 조회합니다.
console.log(Object.getOwnPropertyNames(Car.prototype)); //['constructor', 'move' ]
```

### **메서드**

클래스 내부에는 **생성자 메서드, 프로토타입 메서드, 정적 메서드** 3가지가 올 수 있습니다.

1. **생성자 메서드**는 `constructor()`로 1개 이하만 만들 수 있고 `return`을 사용할 수 없습니다.
2. **프로토타입 메서드**는 메서드에 특별한 명시가 없다면 `prototype`에 저장됩니다. (ex : move 메서드)
3. 메서드 앞에 static을 붙히면 정적 메서드가 만들어집니다. </br>
**정적 메서드**는 프로토타입이 아닌 클래수 함수 자체에 설정되며, 인스턴스를 생성하지 않고도 호출할 수 있습니다.

```jsx
class Car {
	constructor(name){
		this.name = name;
		this.position = 0;
	}
	
	move() {
		this.position += 1;
	}

	static staticMethod() {
		console.log(this === Car);
	}
}
```

### **정적 메서드 활용**

```jsx
class Car {
	constructor(name, date){
		this.name = name;
		this.date = date;
	}
	static createCar() {
		return new this('새로운 차', new Date());
	}
}

const car = Car.createCar();
console.log(car); // Car { name: '새로운 차' ,date:'현재날짜' }
```

클래스를 new 키워드(생성자) 없이 정적 처리하기 위해 사용합니다.

### getter / setter

접근자 프로퍼티로 get, set키워드를 지원합니다!

```jsx
class Car {
	constructor(name){
		this.name = name;
		this.position = 0;
	}
	get name() {
		return this._name;
	}

	set name(newName){
		this._name = newName;
	}
}

const car = new Car("붕붕이");
console.log(car); // Car { _name: '붕붕이', position : 0 }
console.log(car.name); // 붕붕이
car.name = '새차';
console.log(car.name); // 새
```

여기서 `car`를 리턴할 때 그냥 name이 아니라 _name 을 리턴하는 이유, </br>
get , set 에서 name이 아닌 _name 을 사용하는 이유는 뭘까요?!

![underscope 오류](../../../images/Language/JavaScript/JS에서의%20클래스와%20객체/underscope%20error.png	)

constructor안에 있는 `this.name  = name;` 코드는 set name()을 호출합니다
여기서 name을 바로 할당해 업데이트하는 것이 아니라 setter를 호출하는데요

호출된 setter 내부에서 `this.name = name;`코드가 똑같이 존재하므로 또 다시 `set name()`을 호출해 무한 재귀에 빠지게 됩니다~!@!~@!@!

즉!! 

실제 내부에는 `_name`이라고 저장되어 있고 `name`은 저장된 `_name`에 접근하기 위한 키 역할을 하는 것 입 니 다!

### private를 활용한 getter/setter

setter, getter를 private를 활용해 사용할 수도 있습니다!

```jsx
class Car {
	#name = '';

	constructor(name){
		this.name = name;
	}

	get name() {
		return this._name;
	}

	set name(newName){
		this._name = newName;
	}
}

const car = new Car("부릉부릉");
console.log(car); // Car { }
console.log(car.name); // 부릉부릉
```

이처럼 멤버 변수에 #을 통해 은닉화를 할 수 있습니다.
우리가 getter / setter를 쓰는 이유는 은닉화이기 때문에! 이렇게 사용하는 것도 조아요~

### 클래스 상속

```jsx
class Phone {
    constructor() {}

    getCall() {
        return '전화 받기';
    }
}

class SmartPhone extends Phone{
  constructor() {}
}

let smartPhone = new SmartPhone();

console.log(smartPhone.getCall()); //  전화받

console.log(smartPhone instanceof SmartPhone);
console.log(smartPhone instanceof Phone);
```

`ex) class 자식클래스 extends 부모클래스`

SmartPhone 클래스는 Phone에 있는 메서드를 끌어다가 쓸 수 있습니다!

`smartPhone`인스턴스는 `SmartPhone`의 인스턴스이자 `Phone`의 인스턴스입니다!

### super (생성자 호출)

```jsx
class Base {
  constructor(a, b) {
    this.a = a;
    this.b = b;
  }
}

class Derived extends Base {
  constructor(a, b, c) {
    super(a, b);
    this.c = c;
  }
}

const derived = new Derived(1, 2, 3);

console.log(derived); // Derived { a : 1, b : 2, c : 3}
```

생성자 메서드에서 super 키워드를 호출하면 슈퍼클래스에 있는 생성자를 호출합니다!

여기서는 a,b는 부모클래스 Base의 생성자를 사용하게 됩니다!