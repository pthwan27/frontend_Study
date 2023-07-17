# 프로토타입 객체

JavaScript는 클래스 기반 객체지향 프로그래밍 언어와 달리 **기존의 객체를 참조하여 새로운 객체를 생성**한다. 이를 프로토타입 기반 객체지향 프로그래밍 언어라 한다.

여기서 참조라는 말은 **프로토타입 링크를 통해 해당 객체를 참조하여 만드는 것**을 말한다.<br/><br/><br/>

# `[[Prototype]]` 내부 속성

JavaScript의 모든 객체는 `[[Prototype]]`이라는 내부 속성을 가지며, 이는 객체의 프로토타입을 가리킨다.
이 `[[Prototype]]` 속성을 통해 프로토타입 링크가 형성되는데, 이 링크를 따라 객체의 속성이나 메소드를 탐색하게 된다.

# 프로토타입 링크

원시타입(Number, String, Boolean, Symbol, Undefined, Null)을 제외한 모든 참조타입은 다 객체이다. 그래서 배열이나 함수도 자바스크립트에서는 모두 객체라고 말하고 있다. **자바스크립트 객체가 원형을 참조해서 새로운 객체를 만들게 되는데 이 때 프로토타입 링크(`__proto__`)가 만들어진다.**

이 객체는 프로토타입 링크를 가지기 때문 다른 원형을 참조해서 만들어지게 된다. 링크를 타고 타고 가면 자바스크립트의 최종 원형 객체인 Object.prototype이란 것에 도달하게 된다.

<img src="../../../images/Language/JavaScript/prototype/prototype-link.png"><br/><br/>

## 예시

프로토타입 링크에는 표준 메소드인 `Object.getPrototypeOf()`를 사용하거나 `instance.constructor.prototype`을 통해 접근할 수 있다. `__proto__`로 접근하는 것은 비표준 방식이므로 권장되지 않는다.(설명은 `__proto__`로 하겠습니다..!)

```jsx
function Animal(name) {
  this.name = name;
}

Animal.prototype.speak = function () {
  console.log(this.name + ' makes a noise.');
};

let animal = new Animal('Animal');

console.log(Object.getPrototypeOf(animal) === Animal.prototype); // true
console.log(animal.__proto__ === Animal.prototype); // true
```

<br/><br/><br/>

# 프로토타입 체인

프로토타입 체인은 프로토타입 링크가 연속적으로 이어진 구조를 말한다.

객체의 속성을 탐색할 때, JavaScript는 해당 객체부터 시작해 프로토타입 체인을 따라가며 해당 속성을 찾는다. 만약 최종적인 원형 객체은 Object.prototype까지 탐색했는데도 찾지 못하면 undefined를 반환한다.
bar란 객체 속성된 것만 탐색을 하는 게 아니라 Object.prototype이라는 자바스크립트의 최종 원형 객체까지 탐색을 한 후 해당 값을 반환한다.<br/><br/>

## 예시

```jsx
// 1. Animal 생성자 함수 생성
function Animal(name) {
  this.name = name;
}

// 2. 프로토타입에 speak 메소드 추가
Animal.prototype.speak = function () {
  console.log(this.name + ' makes a noise.');
};

// 3. Cat, Dog 생성자 함수 정의: call 메소드를 이용하여 Animal 속성 상속
function Cat(name) {
  Animal.call(this, name);
}

function Dog(name) {
  Animal.call(this, name);
}

// 4. Animal.prototype 상속 받도록 설정
Cat.prototype = Object.create(Animal.prototype);

Dog.prototype = Object.create(Animal.prototype);

// 5. Dog 생성자 함수에 speak 메소드 오버라이딩
Dog.prototype.speak = function () {
  console.log(this.name + ' barks.');
};

let cat = new Cat('Chaenny');
cat.speak(); // Chaenny makes a noise.

let dog = new Dog('Gunny');
dog.speak(); // Gunny barks.
```

`Cat` 인스턴스가 `speak` 메소드를 호출하면 `Cat.prototype`에는 `speak` 메소드가 없으므로 **프로토타입 체인을 통해 `Animal.prototype.speak` 메소드가 호출된다.**

반면에, `Dog` 인스턴스는 `Dog` 생성자 함수에서 `speak` 메소드를 오버라이드하였기 때문에 `speak`를 호출할 때 `Animal.prototype.speak` 메소드를 사용하지 않고 `Dog.prototype.speak` 메소드를 사용하게 된다.

이런 방식으로, 프로토타입 체인은 상속과 메소드 공유를 가능하게 하며, 메소드 오버라이딩은 특정 인스턴스에서의 메소드의 동작을 변경할 수 있다.<br/><br/><br/>

# 함수의 prototype

위의 Animal 예시로 함수의 prototype까지 알아보자. 자바스크립트에서 함수는 특별한 종류의 객체이다. `Function`이라는 내장 생성자 함수에 의해 생성되며, 그 결과로 반환된 함수 객체는 `Function.prototype`를 프로토타입으로 가지게 된다.

**`prototype` 프로퍼티는 해당 함수가 생성자로 사용될 때, 그 생성자에 의해 생성된 객체의 프로토타입을 가리킨다.** 예시에서 `Animal.prototype.speak` 처럼 생성자 함수의 `prototype` 프로퍼티에 새로운 메소드나 프로퍼티를 추가하면, 그 생성자를 사용하여 생성된 모든 객체가 해당 메소드와 프로퍼티에 접근할 수 있게 된다.

## `__proto__`와 `prototype`

### `__proto__`

`__proto__`는 모든 객체가 가지고 있는 속성이다. 위에서 언급했다싶이 객체를 생성하면 해당 객체의 원형(`prototype`)을 가리키는 proto 링크가 자동으로 생성된다.

```javascript
console.log(cat.__proto__); // Animal.prototype
```

### `prototype`

반면에, `prototype`은 함수만이 가지는 속성이다. 이 속성은 해당 함수를 생성자로 사용하여 새 객체를 생성할 때, 생성된 객체의 원형(`prototype`)을 가리킨다.

```javascript
console.log(Cat.prototype); // Cat 객체의 원형(`prototype`)
```

위 코드에서, Cat 함수는 prototype 속성을 가지고 있다. Cat으로 새 객체를 생성하면, 생성된 객체의 `__proto__`는 `Person.prototype`을 가리키게 된다.

```javascript
console.log(cat.__proto__ === Cat.prototype); // true
```

### 결론: `__proto__`와 `prototype`

결론적으로, `__proto__`는 인스턴스가 자신의 프로토타입을 가리키는 반면, `prototype`은 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.

또한, new 연산자를 사용하여 함수를 호출하면, JavaScript는 먼저 새 객체를 생성하고 이 객체의 `__proto__`를 상생자 함수의 `prototype` 객체로 설정한다.

이를 통해, **JavaScript의 객체는 원형(`prototype`)을 참조하여 새 객체를 생성하고, 생성된 객체는 proto 링크(`__proto__`)를 통해 원형을 참조한다.** 이 프로토타입 체인을 통해, 객체는 원형 속성 및 메소드를 상속받을 수 있다.<br/><br/><br/>

# this 메소드 생성 vs 프로토타입 메소드 생성

this를 통한 메소드 생성, 프로토타입을 통한 메소드 생성, 뭐가 다른 걸까? 생성자 함수의 **`this`** 를 사용하여 메소드를 생성하는 방법과 프로토타입에 메소드를 생성하는 방법은 사실상 동일한 기능을 수행하지만, **메모리 사용과 관련하여 차이가 있다.**

## 생성자 함수의 this를 사용하여 메소드 생성

이 방법은 **각 객체마다 독립적인 메소드를 생성**한다. 따라서 같은 기능을 가진 메소드를 각각의 객체마다 따로 가지게 되어 메모리를 더 많이 사용하게 된다.

```jsx
function Person(name) {
    this.name = name;
    this.sayHello = function() {
        return this.name + " says hello!";
    }
}

let person1 = new Person("Chaeeun");
let person2 = new Person("Taehwan");

**console.log(person1.sayHello === person2.sayHello); // false**
```

위의 코드에서 **`person1`** 과 **`person2`** 는 각각 자신만의 **`sayHello`** 메소드를 가지고 있다. 이들 메소드는 기능적으로 동일하지만, 실제로는 서로 다른 메모리 주소에 할당된 별개의 함수이다.<br/><br/>

## 프로토타입에 메소드 생성

모든 인스턴스가 하나의 메소드를 공유하게 만든다. 이는 메모리 효율성을 향상시키는 방법이며, 모든 인스턴스가 동일한 동작을 공유하게 된다.

```jsx
function Person(name) {
    this.name = name;
}

Person.prototype.sayHello = function() {
    return this.name + " says hello!";
}

let person1 = new Person("Chaeeun");
let person2 = new Person("Taehwan");

**console.log(person1.sayHello === person2.sayHello); // true
```

위의 코드에서 **`person1`** 과 **`person2`** 는 동일한 **`sayHello`** 메소드를 공유한다. 이 메소드는 **`Person.prototype`** 에 저장되어 있으며, 모든 **`Person`** 인스턴스에서 접근할 수 있다.<br/><br/>

# 정리

1. 자바스크립트는 프로토타입 기반 객체 지향 프로그래밍 언어이다.
2. 객체는 원형을 참조하여 새로운 객체를 생성하며, 생성된 객체는 프로토타입 링크를 통해 원형을 참조한다.
3. 프로토타입 체인을 통해 객체는 원형 속성 및 메소드를 상속받는다.
4. 함수의 `prototype`프로퍼티는 해당 함수를 생성자로 사용할 때 생성된 객체의 프로토타입을 가리킨다.
5. 메소드 생성 방법에 따라 메모리 사용량이 달라진다. 생성자 함수의 `this`를 이용한 방법은 더 많은 메모리를 사용하지만, 프로토타입을 이용한 방법은 메모리를 절약한다.

# 참고

[https://www.youtube.com/watch?v=7UmApWKRGRw&t=321s](https://www.youtube.com/watch?v=7UmApWKRGRw&t=321s)
