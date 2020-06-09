# useReducer Hook  
- `useState`에서는 설정하고 싶은 다음 상태를 직접 지정해주는 방식으로 상태를 업데이트  
- `useReducer`는 `action`이라는 객체를 기반으로 상태를 업데이트  
`action`객체는 업데이트 할 때 참조하는 객체  
`type`이라는 값을 사용해서 어떤 업데이트를 진행할 것인지 명시할 수 있음  
업데이트 할 때 필요한 참조하고싶은 다른 값이 있다면 이 객체 안에 넣을 수 있음  

`useReducer`를 이용하면 컴포넌트의 상태 업데이트 로직을 컴포넌트에서 분리시킬 수 있다.  
다른 파일에 작성 후 불러와서 사용도 가능  

## reducer  
`reducer`: 상태를 업데이트하는 함수  
```javascript
function reducer(state, action) {
  switch(action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
}
```
현재 상태와 `action` 객체를 parameter로 받아와서 새로운(업데이트 될) 상태를 반환해주는 형태를 갖추고 있어야 함  

## useReducer  
```javascript
const [number, dispatch] = useReducer(reducer, 0);
```
- 첫번째 parameter는 `reduce` 함수  
- 두번째는 기본값(숫자, 문자열, 객체, 배열 등)  
- `number`: 현재 상태  
- `dispatch`: `action`을 발생시키는 함수  

#### index.js  
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import Counter from './Counter'
import * as serviceWorker from './serviceWorker';

ReactDOM.render(
  <React.StrictMode>
    <Counter />
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();
```
#### Counter.js  
```javascript
import React, { useReducer } from 'react';

function reducer(state, action) {
  switch(action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      throw new Error('Unhandled action');
  }
}

function Counter() {
  const [number, dispatch] = useReducer(reducer, 0);

  const onIncrease = () => {
    dispatch({
      type: 'INCREMENT'
    })
  }
  const onDecrease = () => {
    dispatch({
      type: 'DECREMENT'
    })
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
상태 업데이트 로직이 `Counter` 밖에 있음  

## useState -> useReducer  
1. 초기상태를 컴포넌트 바깥에 선언해준다. (`useState`에서 사용하던 기본값)  
```javascript
import React, { useRef, useState, useMemo, useCallback } from 'react';
import CreateUser from './CreateUser';
import UserList from './UserList';

function countActiveUsers(users) {
  console.log('활성 사용자 수를 세는중...');
  return users.filter(user => user.active).length;
}

const initialState = {
  inputs: {
    username: '',
    email: '',
  },
  users: [
    {
      id: 1,
      username: 'juyeong',
      email: '2jy22@naver.com',
      active: true,
    },
    {
      id: 2,
      username: 'tester',
      email: 'tester@example.com',
      active: false,
    },
    {
      id: 3,
      username: 'liz',
      email: 'liz@example.com',
      active: false,
    }
  ]
}

function App() {
  return (
    <>
      <CreateUser />
      <UserList users={[]} />
      <div>활성 사용자 수: 0</div>
    </>
  );
}

export default App;
```
2. `useReducer` 불러오고 `reducer`함수의 틀 만들어주기  
```javascript
import React, { useRef, useReducer, useMemo, useCallback } from 'react';
```
```javascript
function reducer(state, action) {
  return state;
}

function App() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      <CreateUser />
      <UserList users={[]} />
      <div>활성 사용자 수: 0</div>
    </>
  );
}
```
3. `inputs`, `users`를 비구조할당을 통해 추출해 준 다음 `component`에게 `props`로 전달해준다.  
```javascript
function App() {
  const [state, dispatch] = useReducer(reducer, initialState);
  const { users } = state;
  const { username, email } = state.inputs;

  return (
    <>
      <CreateUser username={username} email={email} />
      <UserList users={users} />
      <div>활성 사용자 수: 0</div>
    </>
  );
}
```
4. `onChange`, `onCreate`, `onToggle`, `onRemove` 구현  
```javascript
import React, { useRef, useReducer, useMemo, useCallback } from 'react';
import CreateUser from './CreateUser';
import UserList from './UserList';

function countActiveUsers(users) {
  console.log('활성 사용자 수를 세는중...');
  return users.filter(user => user.active).length;
}

const initialState = {
  inputs: {
    username: '',
    email: '',
  },
  users: [
    {
      id: 1,
      username: 'juyeong',
      email: '2jy22@naver.com',
      active: true,
    },
    {
      id: 2,
      username: 'tester',
      email: 'tester@example.com',
      active: false,
    },
    {
      id: 3,
      username: 'liz',
      email: 'liz@example.com',
      active: false,
    }
  ]
}

function reducer(state, action) {
  switch(action.type) {
    case 'CHANGE_INPUT':
      return {
        ...state,
        inputs: {
          ...state.inputs,
          [action.name]: action.value
        }
      };
    case 'CREATE_USER':
      return {
        inputs: initialState.inputs,
        users: state.users.concat(action.user)
      };
    case 'TOGGLE_USER':
      return {
        ...state,
        users: state.users.map(user =>
          user.id === action.id
            ? { ...user, active: !user.active }
            : user
        )
      };
    case 'REMOVE_USER':
      return {
        ...state,
        users: state.users.filter(user => user.id !== action.id)
      }
    default:
      throw new Error('Unhandled action');
  }
}

function App() {
  const [state, dispatch] = useReducer(reducer, initialState);
  const nextID = useRef(4);
  const { users } = state;
  const { username, email } = state.inputs;

  const onChange = useCallback(e => {
    const { name, value } = e.target;
    dispatch({
      type: 'CHANGE_INPUT',
      name,
      value
    })
  }, []);

  const onCreate = useCallback(() => {
    dispatch({
      type: 'CREATE_USER',
      user: {
        id: nextID.current,
        username,
        email,
      }
    });
    nextID.current += 1;
  }, [username, email]);

  const onToggle = useCallback(id => {
    dispatch({
      type: 'TOGGLE_USER',
      id
    });
  }, []);

  const onRemove = useCallback(id => {
    dispatch({
      type: 'REMOVE_USER',
      id
    });
  }, []);

  const count = useMemo(() => countActiveUsers(users), [users]);

  return (
    <>
      <CreateUser
        username={username}
        email={email}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList
        users={users}
        onToggle={onToggle}
        onRemove={onRemove}
      />
      <div>활성 사용자 수: {count}</div>
    </>
  );
}

export default App;
```

## useState vs. useReducer
간단한것: `useState` 사용  
복잡하다: `useReducer` 사용  
