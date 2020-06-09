# React.memo
컴포넌트에서 리렌더링이 불필요할 때는 이전에 렌더링했던 결과 재사용할 수 있게 하는 방법  
컴포넌트의 리렌더링 성능을 최적화할 수 있다.  

```javascript
export default React.memo(CreateUser);
```
#### UserList.js
```javascript
import React, { useEffect } from 'react';

const User = React.memo(function User({ user, onRemove, onToggle }) {
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
});

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

export default React.memo(UserList);
```
컴포넌트를 내보낼 때 `React.memo()`로 감싸주면 된다.  
`React.memo()`를 사용하면 `props`가 바뀌었을 때만 리렌더링해준다.  

:bulb: 함수형 업데이트를 하면 `deps` 쪽에 `users` 안 넣어도 됨
```javascript
const onCreate = useCallback(() => {
  const user = {
    id: nextId.current,
    username,
    email,
  };
  setUsers(users => users.concat(user));
  setInputs({
    username: '',
    email: ''
  });
  console.log(nextId.current); // 4
  nextId.current += 1;
}, [username, email]);
```
`setUsers()`에 등록하는 콜백함수의 `parameter`에서 최신 `users`를 조회하기 때문에 `deps`에 `users`가 안 들어가도 된다.  
결국 `username`이랑 `email`이 바뀔 때에만 새로 만들어지는 것  
```javascript
const onRemove = useCallback(id => {
  setUsers(users => users.filter(user => user.id !== id));
}, []);

const onToggle = useCallback(id => {
  setUsers(users => users.map(
    user => user.id === id
    ? { ...user, active: !user.active }
    : user
  ))
}, []);
```
컴포넌트가 처음 렌더링됐을 때 딱 한번 만들어지고 그 다음부터는 재사용된다.  

## propsAreEqual  
`React.memo()`의 두번째 `parameter`로 `propsAreEqual` 함수를 넣어줄 수 있다.  
`prevProps`, `nextProps`(전, 후 `props`)를 가져와서 비교해준다.  
`true`를 반환하면 리렌더링 방지, `false`를 반환하면 리렌더링하게 하는 것  
```javascript
export default React.memo(
  UserList,
  (prevProps, nextProps) => nextProps.users === prevProps.users
);
```
나머지 `props`가 정말 고정적이어서 비교를 할 필요가 없는지 꼭 확인해줘야 함  

