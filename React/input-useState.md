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

# 여러개의 input 상태 관리하기  
`input`에 `name`값을 설정하고 이벤트가 발생했을 때 이 값을 참조하는 것이 좋은 방법  
`useState`에서는 여러 개의 문자열을 가지고 있는 객체 형태의 상태를 관리해줘야 함  
```javascript
import React, { useState } from 'react';

function InputSample() {
  const [inputs, setInputs] = useState({
    name: '',
    nickname: '',
  });
  const { name, nickname } = inputs;

  const onChange = (e) => {
    const { name, value } = e.target;

    const nextInputs = {
      ...inputs,
      [name]: value,
    };

    setInputs(nextInputs);
  };

  const onReset = () => {
    setInputs({
      name: '',
      nickname: '',
    });
  };
  
  return (
    <div>
      <input name="name" placeholder="이름" onChange={onChange} />
      <input name="nickname" placeholder="닉네임" onChange={onChange} />
      <button onClick={onReset}>초기화</button>
      <div>
        <b>값: </b>
        이름 (닉네임)
      </div>
    </div>
  )
}

export default InputSample;
```
💡 객체 상태를 업데이트 할 때는 꼭 기존의 상태를 복사하고나서 거기에 특정 값을 덮어씌우고 그걸 새로운 상태로 설정해주어야 한다.(불변성을 지켜준다.)  
👉 불변성을 지켜야 리액트 컴포넌트에서 상태가 업데이트 되었음을 감지할 수 있고, 필요한 렌더링이 발생함  
```javascript
const nextInputs = {
  ...inputs,
  [name]: value,
};

setInputs(nextInputs);
```
이 부분을  
```javascript
setInputs({
  ...inputs,
  [name]: value,
});
```
이렇게 작성할 수 있다.  
