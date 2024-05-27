# 정리

- 사용가능한 JSX 형태

```js
// 사용가능
<$></$>
<_></_>
<foo:bar></foo:bar>
<foo.bar></foo.bar>
<foo.bar foo:bar="baz"></foo.bar>
<foo.bar.baz/></foo.bar.baz>

// 사용불가
<foo:bar:baz></foo:bar:baz>
```

- JSX에서 `\`는 이스케이프 처리하지 않는다.

```js
// 사용가능

<button>\</button>
```

- JSX는 `@babel/plugin-transform-react-jsx`로 트랜스파일링 된다
- JSX 반환값은 결국 `React.createElement`로 귀결된다.
- React.createElement는 (tagName, props, children)순서를 가지는 자바스크립트 객체형태다

```js
// 트랜스파일 전
const Component = <ComponentA required={true}>hellow</ComponentA>;

// 트랜스파일 후
var Component = React.createElement("ComponentA", { required: true }, "hellow");
```
