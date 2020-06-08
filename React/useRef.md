# useRef로 특정 DOM 선택하기  
```javascript
import React, { useState, useRef } from 'react';

function InputSample() {
  const [inputs, setInputs] = useState({
    name: '',
    nickname: '',
  });
  const nameInput = useRef();
  const { name, nickname } = inputs;

  const onChange = (e) => {
    const { name, value } = e.target;

    setInputs({
      ...inputs,
      [name]: value,
    });
  };

  const onReset = () => {
    setInputs({
      name: '',
      nickname: '',
    });
    nameInput.current.focus();
  };

  return (
    <div>
      <input
        name="name"
        placeholder="이름"
        onChange={onChange}
        value={name}
        ref={nameInput}
      />
      <input
        name="nickname"
        placeholder="닉네임"
        onChange={onChange}
        value={nickname}
      />
      <button onClick={onReset}>초기화</button>
      <div>
        <b>값: </b>
        {name} ({nickname})
      </div>
    </div>
  )
}

export default InputSample;
```
선택하고 싶은 DOM에 `ref` 설정하면 DOM에 직접 접근이 가능함  
`ref객체.current`가 현재 선택하고 싶은 DOM을 가리키는 것이다.  

# useRef로 컴포넌트 안의 변수 만들기  
```javascript
import React, { useRef } from 'react';
import UserList from './UserList';

function App() {
  const users = [
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
  ];

  const nextId = useRef(4);

  const onCreate = () => {

    console.log(nextId.current); // 4
    nextId.current += 1;
  }

  return (
    <UserList users={users} />
  )
}

export default App;
```

```javascript
import React from 'react';

function User({ user }) {
  return (
    <div>
      <b>{user.username} <span>({user.email})</span></b>
    </div>
  );
}

function UserList({ users }) {
  return (
    <>
      <div>
        <div>
          <b>{users[0].username} <span>({users[0].email})</span></b>
        </div>
        <div>
          <b>{users[1].username} <span>({users[1].email})</span></b>
        </div>
        <div>
          <b>{users[2].username} <span>({users[2].email})</span></b>
        </div>
      </div>

      <div>
        <User user={users[0]} />
        <User user={users[1]} />
        <User user={users[2]} />
      </div>

      <div>
        {
          users.map(
            (user, index) => (<User user={user} key={user.id} />)
          )
        }
      </div>
    </>
  );
}

export default UserList;
```
`useRef`로 관리해준 이유는 값이 바뀐다고 해서 컴포넌트가 리렌더링될 필요가 없기 때문  
`useState`로 관리해도 상관없지만 리렌더링되는 값은 아니기 때문에 변수로 관리하는 것도 괜찮다.  
