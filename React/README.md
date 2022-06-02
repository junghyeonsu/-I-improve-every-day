# React

## 🤔 리액트에서 사용하는 JSX 문법은 순수 자바스크립트가 아닌데 브라우저에서 어떻게 동작할 수 있는 걸까요?

> 우선 JSX는 필수일까요?

React에서 JSX는 필수가 아닙니다. 빌드 환경에서 컴파일 설정 (웹팩, 바벨)이 귀찮거나 하기 싫을 때는 JSX 없이 그냥 React로 코딩을 할 수 있습니다.

```js
// JSX를 사용하지 않는 예시입니다.
class Hello extends React.Component {
  render() {
    return React.createElement('div', null, `Hello ${this.props.toWhat}`);
  }
}

ReactDOM.render(
  React.createElement(Hello, {toWhat: 'World'}, null),
  document.getElementById('root')
);
```

`JSX` 문법은 우리가 자바스크립트안에 마크업 코드를 넣을 수 있게 해줍니다. 하지만 해당 코드는 브라우저에서 그대로 동작할 수 없기 때문에 브라우저가 이해할 수 있는 코드로 변경 시켜주어야 합니다.

`babel`은 최신 문법을 모든 브라우저에서 동작할 수 있는 예전 코드로 되돌리거나, 표준에서 벗어난 코드를 브라우저가 이해할 수 있는 코드로 변환해주는 트랜스파일러입니다.

`JSX` 문법은 `babel`을 통해서 브라우저가 이해할 수 있는 코드로 변경됩니다. React의 `createElement` 메서드를 통해서 DOM element를 만들 수 있습니다.

> 참고

- [JSX 없이 사용하는 React](https://ko.reactjs.org/docs/react-without-jsx.html)