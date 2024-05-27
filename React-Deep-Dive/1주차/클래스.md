# 클래스란

특정한 객체를 만들기위한 템플릿.

### constructor

- 최초 생성시 어떤 인수를 받을지 결정할수있고, 객체 초기화 용도로도 사용됨
- 단 하나만 존재 가능하며, 생략 가능하다

### 프로퍼티

- 인스턴스 생성할때 내부에 정의 할 수 있는 속성값
- #을 붙여서 private선언 가능 (ES2019)
- typescript를 사용하면 private, protected, public 처럼 사용가능
- \_를 붙여서 private처럼 사용하는 컨벤션도 있었음.

### getter, setter

- 클래스에서 무언가를 가져올때 get 키워드를 붙여주고
- 무언가를 할당할때는 set 키워드를 붙여준다.

```js
// class 내부

get firstCharacter(){
    return this.name[0]
}

set firstCharacter(c){
    this.name  = [c, ...this.name.slice(1)].join("")
}
```

### 인스턴스 메서드 (프로토타입 메서드)

- class내부에 메서드를 선언하면, prototype에 정의됨.

```js
class Car {
  constructor(name) {
    this.name = name;
  }

  hellow() {
    console.log("안녕하세요", this.name);
  }
}

const myCar = new Car("자동차");

Object.getPrototypeOf(myCar); // {constuctor:f, hellow:f}

Object.getPrototypeOf(myCar) === Car.prototype; // true

Object.getPrototypeOf(myCar) === myCar.__proto__; // true
```

인스턴스의 prototype을 찾을때 `__proto__`를 통해 프로토 링크를 따라 찾을 수 있는데, 이는 호환성을 지키기위해 존재하는 기능이기 때문에 `getPrototypeOf`를 사용하길 권장함.

### 정적 메서드

- 클래스 자신을 가르키기때문에 this를 사용 불가
- 인스턴스를 생성하지않아도 사용가능함. 따라서 여러곳에서 재사용 가능.
- utils함수를 정의할때 활용

```js
class Car {
  static hello() {
    console.log("안녕하세요");
  }
}

const myCar = new Car();

myCar.hello(); // Uncaught TypeError: .....
Car.hello(); // 안녕하세요
```

### 상속

`extends` 키워드를 통해 클래스를 확장할 수 있음.

```js
class Car {
  honk() {
    console.log("경적을 울립니다");
  }
}

class Truck extends Car {
  constructor() {
    super();
  }
}

const myTruck = new Truck();

myTruck.honk(); // 경적을 울립니다
```

### 클래스와 함수의 관계

자바스크립트의 class 문법은 실제 객체지향 언어로 동작하는것이 아닌, class형태로 구현된 일종의 폴리필 함수이다.

# 정리

- 리액트 16.8 버전 이전까지는 모든 컴포넌트들이 class로 만들어졌다.
- 따라서 앞으로의 리액트는 functional로 작성이 되겠지만, legacy 프로젝트를 다루거나, migration을 하게 될 경우에 class형 컴포넌트를 이해 할 수 있어야한다.
