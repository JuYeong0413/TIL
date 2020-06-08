# 배열 렌더링하기
```javascript
import React from 'react';
import UserList from './UserList';

function App() {
  return (
    <UserList />
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

function UserList() {
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
            user => (<User user={user} key={user.id} />)
          )
        }
      </div>
    </>
  );
}

export default UserList;
```
`key`값을 가지고 있어야 element가 정확히 어떤 data를 가리키고 있는지 알고 있기 때문에 항목 추가/제거 시 효율적으로 업데이트 가능하다.  
:point_right: `key`를 설정해야 효율적으로 렌더링 할 수 있다.  

고유값이 없는 경우에는 `key` 자리에 `index`값을 넣을 수 있긴 하지만 비효율적이다.  
```javascript
<div>
  {
    users.map(
      (user, index) => (<User user={user} key={index} />)
    )
  }
</div>
```
다만, 데이터가 몇 개 없을 경우나 자주 업데이트 되지 않는 경우는 `index`를 사용한다고 해서 문제가 되진 않는다.  
