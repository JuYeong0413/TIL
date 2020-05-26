## JSX 규칙  
- 태그는 꼭 닫혀있어야 한다.  
태그와 태그 사이에 별도의 내용이 없다면 `<input />` 또는 `<br />`과 같이 self-closing tag를 사용할 수 있다.  

- 2개 이상의 태그는 꼭 하나의 태그로 감싸주어야 한다.  
```javascript
// App.js

function App() {
  return (
    <>
      <Hello />
      <div>안녕하세요.</div>
    </>
  )
}
```
`fragment`(비어있는 이름을 가지고 있는 태그) 사용하면 된다.  

- JSX에서 괄호(`()`)를 감싸는 건 부가적인 것이다. 가독성을 위해 사용한다.  

## JSX 내에서 JavaScript 값 사용하기  
중괄호(`{}`)로 감싼다.  
```javascript
// App.js

function App() {
  const name = 'React';
  return (
    <>
      <Hello />
      <div>{name}</div>
    </>
  )
}
```

## style 설정하기
`HTML`에서 `inline`으로 설정한 것과 다르게 객체를 만들어줘야 한다.  
`background-color` 처럼 dash(`-`)로 구분되어 있는 경우 `camel case`(`backgroundColor`)로 naming을 해줘야 한다.  
숫자의 기본 단위는 `px`, 다른 단위를 사용하고 싶다면 문자열(`'1rem'`)로 사용한다.  
```javascript
// App.js

function App() {
  const name = 'React';
  const style = {
    backgroundColor = 'black',
    color: 'aqua',
    fontSize = 24,
    padding: '1rem'
  };
  return (
    <>
      <Hello />
      <div style={style}>{name}</div>
    </>
  )
}
```

## class name 설정하기
`HTML`에서 `class`로 설정한 것과 다르게 JSX에서는 `className`을 사용한다.  
```css
/* App.css */

.gray-box {
  background-color: gray;
  width: 64px;
  height: 64px;
}
```
```javascript
// App.js

import './App.css';

function App() {
  const name = 'React';
  const style = {
    backgroundColor = 'black',
    color: 'aqua',
    fontSize = 24,
    padding: '1rem'
  };
  return (
    <>
      <Hello />
      <div style={style}>{name}</div>
      <div className="gray-box"></div>
    </>
  )
}
```

## JSX에서 주석 사용하기  
```javascript
// App.js

function App() {
  return (
    <>
      <Hello />
      {/* 어쩌고 저쩌고 */}
      <div>안녕하세요.</div>
    </>
  )
}
```
태그를 여는 부분이나 self-closing tag에서도 주석 작성이 가능하다.  
```javascript
// App.js

function App() {
  const style = {
    backgroundColor = 'black',
    color: 'aqua',
    fontSize = 24,
    padding: '1rem'
  };
  
  return (
    <>
      <Hello 
        // 이런식으로 작성하는 주석은
      />
      <div
        // 화면에 나타나지 않는다.
      style={style}>안녕하세요.</div>
    </>
  )
}
```

