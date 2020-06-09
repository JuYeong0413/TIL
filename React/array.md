# 배열에 항목 추가하기  
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
      email: '2jy22@naver.com'
    },
    {
      id: 2,
      username: 'tester',
      email: 'tester@example.com'
    },
    {
      id: 3,
      username: 'liz',
      email: 'liz@example.com'
    }
  ]);

  const nextId = useRef(4);

  const onCreate = () => {
    const user = {
      id: nextId.current,
      username,
      email,
    };
    setUsers([...users, user]);
    setInputs({
      username: '',
      email: ''
    });
    console.log(nextId.current); // 4
    nextId.current += 1;
  }

  return (
    <>
      <CreateUser
        username={username}
        email={email}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList users={users} />
    </>
  )
}

export default App;
```
배열을 컴포넌트의 상태로써 관리하기 위해서는 `useState`로 감싸주면 된다.  

기존의 배열을 spread 연산자를 이용해 복사해서 넣은 다음에 새로운 배열을 만들고 그 뒤에 새로운 `user` 항목 추가  
```javascript
setUsers(users.concat(user));
```
`concat()`을 이용해도 된다.  

# 배열에 항목 제거하기  
배열에서 특정 아이템을 삭제할 때 불변성을 지켜가면서 업데이트 해 줘야 함. 이 때 `filter()` 함수를 사용하면 편하다.  
배열에서 특정 조건이 만족하는 원소들만 추출해서 새로운 배열을 만들어 준다.  
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
      email: '2jy22@naver.com'
    },
    {
      id: 2,
      username: 'tester',
      email: 'tester@example.com'
    },
    {
      id: 3,
      username: 'liz',
      email: 'liz@example.com'
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

  return (
    <>
      <CreateUser
        username={username}
        email={email}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList users={users} onRemove={onRemove} />
    </>
  )
}

export default App;
```
예를 들어 `id`가 3인 항목을 삭제하려고 하면 `id`가 1, 2인 항목은 `parameter`랑 받아온 `id`와 다르기 때문에 새로운 배열에 추가됨  
3은 만족하지 않으니 추가되지 않음. (값이 `true`인 것만 추가되는 것)  
배열이 `setUsers()`에 들어가서 업데이트 되는 것이다.  
```javascript
import React from 'react';

function User({ user, onRemove }) {
  const { username, email, id } = user;
  return (
    <div>
      <b>{username} <span>({email})</span></b>
      <button onClick={() => onRemove(id)}>삭제</button>
    </div>
  );
}

function UserList({ users, onRemove }) {
  return (
    <div>
      {
        users.map(
          (user, index) => (
            <User
              user={user}
              key={user.id}
              onRemove={onRemove}
            />
          )
        )
      }
    </div>
  );
}

export default UserList;
```
`onClick`에는 함수가 들어가야 한다. 함수호출이 아님  
함수를 호출하면 렌더링되면서 바로 호출됨  

# 배열의 항목 수정하기  
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
`map()` 함수로 불변성 유지 

```javascript
import React from 'react';

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
