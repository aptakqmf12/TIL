# 클로저

리액트의 함수형 컴포넌트에서의 핵심은 클로저를 이해하는것이다.

아래의 경우에 모두 클로저의 개념에 의존한다.

- 함수컴포넌트의 구조와 작동방식
- 훅의 원리
- 의존성 배열
- 기타등등

> 클로저란? 함수와 함수가 선언된 어휘적 환경의 조합

> 렉시컬 스코프란? 함수가 어디서 선언하였는지에 따라 결정되는 스코프

여기서 어휘적 환경에 대한 예제를 보자.

```js
function outer() {
  const a = 10;

  function inner() {
    const b = 20;
    console.log(a + b);
  }

  inner(); // 30
}

outer(); // 30
```

inner함수가 선언된 시점에 변수 a가 존재하고, 변수 a의 유효범위는 outer전체이다.
따라서 outer() 실행시 inner의 선언시점에 존재하는 변수 a에 접근이 가능하다.

## 변수 유효범위 스코프

### 전역스코프

window or global 객체의 범위.

### 함수 스코프

- 자바스크립트는 기본적으로 함수레벨의 스코프를 따른다.
- 즉 {} 블록이 스코프의 범위를 정하지않음.

```js
if (true) {
  var global = "global scope"; // global은 if문의 {}안에 있지만
}

// 여기서 접근 가능하다.
console.log(global); // global scope
```

- 하지만 함수 블록 내부에서는 {}가 예상대로 동작함

```js
function hello() {
  var local = "v";
  conosole.log(local); // v
}

hello();

console.log(local); // Uncaught ReferenceError!
```

- 스코프가 중첩되어있다면 가장 가까운 스코프에 변수가 존재하는지 확인.

```js
var x = 1;

function foo() {
  var x = 100;
  console.log(x); // 100

  function bar() {
    var x = 999;
    console.log(x); // 999
  }
}

console.log(x); // 1
```

## 클로저의 활용

### 클로저 활용의 이점

- 캡슐화 : 직접적으로 변수를 노출하지않음
- 로그를 남기는등 부가적인 수행 가능
- 변수가 readOnly이므로 업데이트를 특정 함수로 제한 가능.

### 리액트에서의 클로저

`useState` 는 내부적으로 클로저를 활용한다.

위 예제처럼 생각해보면 아래와 같다고 볼수있다.

- outer함수는 `useState`
- inner함수는 `setState`
- a변수는 `state`

### 주의점

- `var` 는 함수 레벨 스코프를 따르므로, 해당 i 구문이 선언된 스코프를 바라보기 때문에 전역 스코프애 i가 등록된다. 따라서 setTimeOut이 실행될 시점엔 i는 5로 업데이트 되어있음.
- `5 5 5 5 5`가 찍힌다.

```js
for (var i = 0; i < 5; i++) {
  setTimeOut(function () {
    console.log(i);
  }, i * 1000);
}
```

- 반면 `let`은 블록 레벨 스코프를 따르므로 기대처럼 동작함
- `1 2 3 4 5`가 찍힌다

```js
for (let i = 0; i < 5; i++) {
  setTimeOut(function () {
    console.log(i);
  }, i * 1000);
}
```

- 아래처럼 IIFE로 for문 내부를 선언하면 `sec`에 i를 저장해두었다가 setTimeOut의 콜백으로 넘긴다
- setTimeOut의 콜백이 바라보는 클로저 환경은 IIFE가 되고, 이는 for문 마다 생성과 실행이 반복되어 각각의 스코프는 고유하고 고유한 sec를 가지게된다

```js
for (let i = 0; i < 5; i++) {
  setTimeOut(
    (function (sec) {
      return function () {
        console.log(sec);
      };
    },
    i + 1000)(i)
  );
}
```
