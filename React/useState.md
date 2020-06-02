# useState
```javascript
import React, { useState } from 'react';

function Counter() {
  const [number, setNumber] = useState(0);

  const onIncrease = () => {
    setNumber(number + 1);
  }
  const onDecrease = () => {
    setNumber(number - 1);
  }
  return (
    <div>
      <h1>{number}</h1>
      <button onClick={onIncrease}>+1</button>
      <button onClick={onDecrease}>-1</button>
    </div>
  )
}

export default Counter;
```
`number`라는 상태를 만들건데 이 상태의 기본값은 `0`, `setNumber`는 이 상태를 바꿔주는 함수  

```javascript
import React from 'react';
import Counter from './Counter';

function App() {
  return (
    <Counter />
  )
}

export default App;
```

## 업데이트 함수  
컴포넌트 최적화를 위해 필요하다.  
```javascript
import React, { useState } from 'react';

function Counter() {
  const [number, setNumber] = useState(0);

  const onIncrease = () => {
    setNumber(prevNumber => prevNumber + 1);
  }
  const onDecrease = () => {
    setNumber(prevNumber => prevNumber - 1);
  }
  return (
    <div>
      <h1>{number}</h1>
      <button onClick={onIncrease}>+1</button>
      <button onClick={onDecrease}>-1</button>
    </div>
  )
}

export default Counter;
```
