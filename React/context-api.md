# Context API를 사용한 전역 값 관리  
#### ContextSample.js  
```javascript
import React, { createContext, useContext } from 'react';

function Child({ text }) {
  return <div>안녕하세요? {text}</div>
}

function Parent({ text }) {
  return <Child text={text} />
}

function GrandParent({ text }) {
  return <Parent text={text} />
}

function ContextSample() {
  return <GrandParent text="GOOD" />
}

export default ContextSample;
```

```javascript
import React, { createContext, useContext } from 'react';

const MyContext = createContext('defaultValue');

function Child() {
  const text = useContext(MyContext);
  return <div>안녕하세요? {text}</div>
}

function Parent() {
  return <Child />
}

function GrandParent() {
  return <Parent />
}

function ContextSample() {
  return (
    <MyContext.Provider value="GOOD">
      <GrandParent />
    </MyContext.Provider>
  )
}

export default ContextSample;
```
- `createContext()`에는 context에서 사용할 기본값을 넣어줄 수 있다. (`Provider`를 사용하지 않았을 때의 기본값)  
- `Child`에서 `MyContext`값을 불러와서 쓸 수 있다.  
- `useContext()`는 context에 있는 값을 읽어와서 사용할 수 있게 해주는 리액트 내장 hook  
- `MyContext`의 값을 지정해주고 싶으면 context를 사용하는 최상위 컴포넌트에서 `Provider`라는 컴포넌트를 사용해야 함  
`MyContext.Provider`를 통해서 `value`값을 설정해줘서 context의 값이 설정이 되는 것  
`Child` 컴포넌트에서 `useContext` 사용해서 `MyContext`에 있는 값을 그대로 불러와서 `text`값을 사용할 수 있게 되는 것  
- context를 다른 파일에서 작성할 수도 있다. 내보낸 것을 불러와서 어디서든 사용할 수 있다는 장점이 있음  

#### 유동적으로 변하는 context  
```javascript
import React, { createContext, useContext, useState } from 'react';

const MyContext = createContext('defaultValue');

function Child() {
  const text = useContext(MyContext);
  return <div>안녕하세요? {text}</div>
}

function Parent() {
  return <Child />
}

function GrandParent() {
  return <Parent />
}

function ContextSample() {
  const [value, setValue] = useState(true);
  return (
    <MyContext.Provider value={value ? 'GOOD' : 'BAD'}>
      <GrandParent />
      <button onClick={() => setValue(!value)}>CLICK ME</button>
    </MyContext.Provider>
  )
}

export default ContextSample;
```

#### App.js  
```javascript
import React, { useRef, useReducer, useMemo, useCallback, createContext } from 'react';
import CreateUser from './CreateUser';
import UserList from './UserList';
import useInputs from './useInputs';

function countActiveUsers(users) {
  console.log('활성 사용자 수를 세는중...');
  return users.filter(user => user.active).length;
}

const initialState = {
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

export const UserDispatch = createContext(null);

function App() {
  const [state, dispatch] = useReducer(reducer, initialState);
  const [form, onChange, reset] = useInputs({
    username: '',
    email: '',
  });
  const { username, email } = form;
  const nextID = useRef(4);
  const { users } = state;

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
    reset();
  }, [username, email, reset]);

  const count = useMemo(() => countActiveUsers(users), [users]);

  return (
    <UserDispatch.Provider value={dispatch}>
      <CreateUser
        username={username}
        email={email}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList users={users} />
      <div>활성 사용자 수: {count}</div>
    </UserDispatch.Provider>
  );
}

export default App;
```

#### UserList.js  
```javascript
import React, { useContext } from 'react';
import { UserDispatch } from './App';

const User = React.memo(function User({ user }) {
  const { username, email, id, active } = user;
  const dispatch = useContext(UserDispatch);

  return (
    <div>
      <b
        style={{
          color: active ? 'green' : 'black',
          cursor: 'pointer'
        }}
        onClick={() => dispatch({
          type: 'TOGGLE_USER',
          id
        })}
      >
        {username}
      </b>
      &nbsp;
      <span>({email})</span>
      <button onClick={() => dispatch({
        type: 'REMOVE_USER',
        id
      })}>삭제</button>
    </div>
  );
});

function UserList({ users }) {
  return (
    <div>
      {
        users.map(
          (user) => (
            <User
              user={user}
              key={user.id}
            />
          )
        )
      }
    </div>
  );
}

export default React.memo(
  UserList,
  (prevProps, nextProps) => nextProps.users === prevProps.users
);
```
