# useCallback Hook  
이전에 만들었던 함수를 새로 만들지 않고 재사용하는 방법  
`useMemo`랑 비슷한데 함수를 위한 `Hook`이다.  

컴포넌트들이 `props`가 바뀌지 않았다면 virtual DOM에 한해 리렌더링조차 안 하게끔 만들어 줄 수 있다.  
이전에 만들어놓았던 결과물을 재사용할 수 있게 구현을 하기 위함  
#### UserList.js
```javascript
import React, { useEffect } from 'react';

function User({ user, onRemove, onToggle }) {
  const { username, email, id, active } = user;

  return (
    <div>
      <b
        style={{
          color: active ? 'green' : 'black',
          cursor: 'pointer'
        }}
        onClick={() => onToggle(id)}
      >
        {username}
      </b>
      &nbsp;
      <span>({email})</span>
      <button onClick={() => onRemove(id)}>삭제</button>
    </div>
  );
}

function UserList({ users, onRemove, onToggle }) {
  return (
    <div>
      {
        users.map(
          (user) => (
            <User
              user={user}
              key={user.id}
              onRemove={onRemove}
              onToggle={onToggle}
            />
          )
        )
      }
    </div>
  );
}

export default UserList;
```
#### CreateUser.js
```javascript
import React from 'react';

function CreateUser({ username, email, onChange, onCreate }) {
  return (
    <div>
      <input
        name="username"
        placeholder="계정명"
        onChange={onChange}
        value={username}
      />
      <input
        name="email"
        placeholder="이메일"
        onChange={onChange}
        value={email}
      />
      <button onClick={onCreate}>등록</button> 
    </div>
  );
}

export default CreateUser;
```
#### App.js
```javascript
import React, { useRef, useState, useMemo, useCallback } from 'react';
import CreateUser from './CreateUser';
import UserList from './UserList';

function countActiveUsers(users) {
  console.log('활성 사용자 수를 세는중...');
  return users.filter(user => user.active).length;
}

function App() {
  const [inputs, setInputs] = useState({
    username: '',
    email: '',
  });
  const { username, email } = inputs;
  const onChange = useCallback(e => {
    const { name, value } = e.target;
    setInputs({
      ...inputs,
      [name]: value
    });
  }, [inputs]);

  const [users, setUsers] = useState([
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
  ]);

  const nextId = useRef(4);

  const onCreate = useCallback(() => {
    const user = {
      id: nextId.current,
      username,
      email,
    };
    setUsers(users.concat(user));
    setInputs({
      username: '',
      email: ''
    });
    console.log(nextId.current); // 4
    nextId.current += 1;
  }, [username, email, users]);

  const onRemove = useCallback(id => {
    setUsers(users.filter(user => user.id !== id));
  }, [users]);

  const onToggle = useCallback(id => {
    setUsers(users.map(
      user => user.id === id
      ? { ...user, active: !user.active }
      : user
    ))
  }, [users]);

  const count = useMemo(() => countActiveUsers(users), [users]);

  return (
    <>
      <CreateUser
        username={username}
        email={email}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList users={users} onRemove={onRemove} onToggle={onToggle} />
      <div>활성 사용자 수: {count}</div>
    </>
  )
}

export default App;
```
##### onChange  
```javascript
const onChange = useCallback(e => {
  const { name, value } = e.target;
  setInputs({
    ...inputs,
    [name]: value
  });
}, [inputs]);
```
내부에서 의존하는 값을 두번째 `parameter`인 `deps` 배열에 넣어준다.  
`onChange`는 `inputs`가 바뀔 때만 새로 함수가 만들어지고, 그렇지 않다면 기존에 만들어진 함수를 재사용함  
##### onCreate  
```javascript
const onCreate = useCallback(() => {
  const user = {
    id: nextId.current,
    username,
    email,
  };
  setUsers(users.concat(user));
  setInputs({
    username: '',
    email: ''
  });
  console.log(nextId.current); // 4
  nextId.current += 1;
}, [username, email, users]);
  ```
`username`, `email` 같은 경우에 `inputs`에서 바깥으로 빼내준 값인데 이런 값도 결국에는 상태이기 때문에 `deps` 배열에 넣어야 함  
만약에 빼먹으면 최신 상태가 아닌 옛날 상태를 참조하게 된다.  
