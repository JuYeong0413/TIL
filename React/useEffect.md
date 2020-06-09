# useEffect Hook  
리액트 컴포넌트가 처음 화면에 나타나게 될 때, 화면에 사라지게 될 때 특정 작업을 할 수 있다.  
컴포넌트의 `props`나 상태가 바뀌어서 업데이트 되기 전이나 업데이트 될 때 어떤 작업을 할 수 있다.  

## 컴포넌트가 `mount`(나타날 때), `unmount`(삭제될 때)될 때 특정 작업 처리  
### 컴포넌트가 나타날 때 특정 작업을 하는 방법  
컴포넌트가 `mount`될 때 주로 추가하는 작업으로는  
- `props`로 받은 값을 컴포넌트의 `state`로 설정하는 것  
- 컴포넌트가 나타나면 특정 외부 API 요청을 할 때 작업 처리  
- 라이브러리 사용할 때 처리  
- `setInterval`, `setTimeout` 작업 처리  

등이 있다.  
#### App.js
```javascript
import React, { useRef, useState } from 'react';
import CreateUser from './CreateUser';
import UserList from './UserList';

function App() {
  const [inputs, setInputs] = useState({
    username: '',
    email: '',
  });
  const { username, email } = inputs;
  const onChange = e => {
    const { name, value } = e.target;
    setInputs({
      ...inputs,
      [name]: value
    });
  };

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

  const onCreate = () => {
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
  };

  const onRemove = id => {
    setUsers(users.filter(user => user.id !== id));
  };

  const onToggle = id => {
    setUsers(users.map(
      user => user.id === id
      ? { ...user, active: !user.active }
      : user
    ))
  };

  return (
    <>
      <CreateUser
        username={username}
        email={email}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList users={users} onRemove={onRemove} onToggle={onToggle} />
    </>
  )
}

export default App;
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
#### UserList.js
```javascript
import React, { useEffect } from 'react';

function User({ user, onRemove, onToggle }) {
  const { username, email, id, active } = user;
  useEffect(() => {
    console.log('컴포넌트가 화면에 나타남');
  }, []);

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
- 첫번째 `parameter`는 실행하고 싶은 함수  
- 두번째 `parameter`는 비어있는 배열, `deps`(dependency)라고 부른다. 의존되는 값들을 배열 안에다가 넣어주면 됨  
만약에 의존되는 값들이 비어있다면 컴포넌트가 화면에 처음 나타날 때만 실행됨  

`useEffect()`에서 함수가 호출되는 시점에서는 UI가 화면에 나타난 상태 이후이기 때문에 DOM에 바로 접근을 해도 된다.  

### 컴포넌트가 사라질 때 특정 작업을 하는 방법 
컴포넌트가 `unmount`될 때 주로 하는 작업으로는  
- `clearInterval`, `clearTimeout`(`setInterval`, `setTimeout`을 사용해서 등록했던 작업을 제거)  
- 라이브러리 인스턴스 만들었으면 제거하는 작업  

```javascript
useEffect(() => {
  console.log('컴포넌트가 화면에 나타남');
  return () => {
    console.log('컴포넌트가 화면에서 사라짐');
  }
}, []);
```
클리너 함수를 반환하면 됨  
클리너 함수는 일종의 뒷정리 함수라고 이해하면 된다. 업데이트 되기 바로 직전에 호출되는 것  

### `deps` 배열  
```javascript
useEffect(() => {
  console.log('user 값이 설정됨');
  console.log(user);
  return () => {
    console.log('user 값이 바뀌기 전');
    console.log(user);
  }
}, [user]);
```
`deps` 배열에 어떤 값(`user`)을 넣어주면 `useEffect()`에 등록한 함수는 해당 값(`user`)이 설정되거나 바뀔 때마다 호출된다.  
해당 값(`user`)이 바뀌기 직전, 사라지기 직전에는 클리너 함수가 호출된다.  

만약에 `useEffect()`에 등록한 함수에서 `props`로 받아온 값을 참조하거나 `useState`로 관리하고 있는 값을 참조하고 있는 경우에는 `deps` 배열에 꼭 넣어줘야 한다. 그래야만 최신의 값을 가리키고 있게 된다.  

```javascript
useEffect(() => {
  console.log(user);
});
```
`deps` 배열을 아예 생략하면 모든 컴포넌트에서 `useEffect()`가 실행됨 
리액트 컴포넌트에서는 부모 컴포넌트가 리렌더링되면 자식 컴포넌트도 리렌더링된다. `User` 컴포넌트의 부모 컴포넌트는 `UserList`. `UserList`에서 `users` 배열이 바뀌면 컴포넌트가 리렌더링되는거고, 모든 `User` 컴포넌트가 리렌더링되고있다는 것  
