# 조건부 렌더링  
## 삼항연산자  
```javascript
import React from 'react';
import Hello from './Hello';
import Wrapper from './Wrapper';

function App() {
  return (
    <Wrapper>
      <Hello name="react" color="red" isSpecial={true} />
      <Hello color="pink" />
    </Wrapper>
  )
}

export default App;
```
`true` 또한 JavaScript 값이기 때문에 중괄호로 감싸주어야 함  
그냥 `isSpecial` 만 적어도 `true`의 값을 가짐  
```javascript
import React from 'react';

function Hello({ color, name, isSpecial }) {
  return (
    <div style={{
      color
    }}>
      {isSpecial ? <b>*</b> : null}
      안녕하세요 {name}
    </div>
  );
}


Hello.defaultProps = {
  name: '이름없음'
}

export default Hello;
```
`JSX`에서 `{null}`, `{false}`, `{undefined}`는 아무것도 렌더링하지 않는다.  
대신 `{0}`은 그대로 나타나게 된다.  
삼항연산자는 내용이 달라질 때 주로 사용함  

## And(&&) 연산자  
위와 같은 상황에서는 `&&` 연산자를 사용해서 처리하는 것이 더 간편하다.  
```javascript
import React from 'react';

function Hello({ color, name, isSpecial }) {
  return (
    <div style={{
      color
    }}>
      {isSpecial && <b>*</b>}
      안녕하세요 {name}
    </div>
  );
}


Hello.defaultProps = {
  name: '이름없음'
}

export default Hello;
```
