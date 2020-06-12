# componentDidCatch 메서드  
클래스형 컴포넌트에서만 할 수 있음  

#### App.js  
```javascript
import React from 'react';
import User from './User';

function App() {
  return (
    <User />
  );
}

export default App;
```
#### User.js  
```javascript
import React from 'react';

function User({ user }) {
  if (!user) return null;
  
  return (
    <div>
      <div>
        <b>ID</b>: {user.id}
      </div>
      <div>
        <b>Username</b>: {user.username}
      </div>
    </div>
  );
}

export default User;
```
`props`가 유효한지 확인하고 `null` 처리하는 것은 단순히 에러를 발생하지 않게 하기 위한 작업  

실수로 놓쳤을 경우에 `componentDidCatch`로 사용자에게 에러가 발생했음을 알리고 모니터링도 할 수 있음  
#### App.js  
```javascript
import React from 'react';
import User from './User';
import ErrorBoundary from './ErrorBoundary';

function App() {
  const user = {
    id: 1,
    username: 'juyeong'
  }

  return (
    <ErrorBoundary>
      <User />
    </ErrorBoundary>
  );
}

export default App;
```

#### User.js  
```javascript
import React from 'react';

function User({ user }) {
  // if (!user) return null;

  return (
    <div>
      <div>
        <b>ID</b>: {user.id}
      </div>
      <div>
        <b>Username</b>: {user.username}
      </div>
    </div>
  );
}

export default User;
```

#### ErrorBoundary.js  
```javascript
import React, { Component } from 'react';

class ErrorBoundary extends Component {
  state = {
    error: false,
  };

  componentDidCatch(error, info) {
    console.log('에러가 발생했습니다');
    console.log({
      error,
      info
    });
    this.setState({
      error: true,
    })
  }

  render() {
    if (this.state.error) {
      return <h1>에러 발생!</h1>
    }
    return this.props.children;
  }
}

export default ErrorBoundary;
```
- `error`: 에러에 대한 정보  
- `info`: 에러가 어디서 발생했는지에 대한 정보  

`componentDidCatch`의 역할은 아직 발견하지 못한 에러가 있을 때 사용자에게 에러가 발생했음을 알려주는 작업을 할 수 있다.  
`error`나 `info`를 에러 관리 서버에다가 요청을 넣어주게 되면 실시간으로 확인을 할 수 있다.  
