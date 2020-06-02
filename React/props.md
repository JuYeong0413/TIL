# props
`properties`의 줄임말.  
컴포넌트를 사용하게 될 때 특정 값을 전달해주고 싶을 때 사용하는 것  
```javascript
import React from 'react';
import Hello from './Hello';

function App() {
  return (
    <Hello name="react" />
  )
}

export default App;
```
컴포넌트에서 `name`으로 넣어준 값을 사용하고 싶다면
```javascript
import React from 'react';

function Hello(props) {
  return <div>안녕하세요</div>;
}

export default Hello;
```
함수에서 `props`라는 parameter를 가져온다.  
`props`라는 parameter는 우리가 넣어준 값들이 객체 형태로 들어가 있음  
```javascript
console.log(props);
```
를 해보면 `{name: "react"}`라고 뜬다.  
react라는 값을 컴포넌트 내부에서 보여주고 싶다면 
```javascript
import React from 'react';

function Hello(props) {
  return <div>안녕하세요 {props.name}</div>;
}

export default Hello;
```
  
```javascript
import React from 'react';
import Hello from './Hello';

function App() {
  return (
    <Hello name="react" color="red" />
  )
}

export default App;
```
```javascript
import React from 'react';

function Hello(props) {
  return <div style={{
    color: props.color
  }}>안녕하세요 {props.name}</div>;
}

export default Hello;
```
## 구조분해(비구조할당)  
```javascript
import React from 'react';

function Hello({ color, name }) {
  return <div style={{
    color
  }}>안녕하세요 {name}</div>;
}

export default Hello;
```
## props를 사용하지 않았을 때의 기본값  
```javascript
import React from 'react';

function Hello(props) {
  return <div>안녕하세요 {props.name}</div>
}


Hello.defaultProps = {
  name: '이름없음'
}

export default Hello;
```
```javascript
import React from 'react';
import Hello from './Hello';

function App() {
  return (
    <>
      <Hello name="react" color="red" />
      <Hello color="pink" />
    </>
  )
}

export default App;
```

## children  
태그와 태그 사이의 내용으로, 컴포넌트 안에 넣는 값을 조회하기 위해 사용함  
```javascript
import React from 'react';

function Wrapper({ children }) {
  const style = {
    border: '2px solid black',
    padding: 16
  };
  
  return <div style={style}>{children}</div>
}

export default Wrapper;
```
```javascript
import React from 'react';
import Hello from './Hello';
import Wrapper from './Wrapper';

function App() {
  return (
    <Wrapper>
      <Hello name="react" color="red" />
      <Hello color="pink" />
    </Wrapper>
  )
}

export default App;
```
