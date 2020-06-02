# input 상태 관리하기  
```javascript
import React from 'react';
import InputSample from './InputSample';

function App() {
  return (
    <InputSample />
  )
}

export default App;
```
```javascript
import React, { useState } from 'react';

function InputSample() {
  const [text, setText] = useState('');

  const onChange = (e) => {
    console.log(e.target);
    console.log(e.target.value);
    setText(e.target.value);
  }

  const onReset = () => {
    setText('');
  }
  return (
    <div>
      <input onChange={onChange} value={text} />
      <button onClick={onReset}>초기화</button>
      <div>
        <b>값: </b>
        {text}
      </div>
    </div>
  )
}

export default InputSample;
```
수정이벤트가 발생했을 때 그 이벤트에 대한 내용이 이벤트 객체 `e` parameter에 받아와서 사용할 수 있게 되는 것  
`e.target` 값은 현재 이벤트가 발생한 DOM(input)에 대한 정보를 가지고 있다.  
값을 가지고 오려면 `e.target.value`를 사용해야 한다.  
