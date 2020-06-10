# immer를 사용한 불변성 지키기  
`immer`를 사용하면 불변성을 해치는 코드를 작성해도 대신 불변성 유지를 해준다.  

## immer 설치  
```bash 
$ yarn add immer
```

## immer 불러오기  
```javascript
import produce from 'immer';
```
Chrome 개발자 도구에서 `produce`를 사용하려면  
```javascript
window.produce = produce;
```
를 넣어줘야 한다.  

## produce 함수
```javascript
const state = {
  number: 1,
  dontChangeMe: 2
};

const nextState = produce(state, draft => {
  draft.number += 1;
});
```
- 첫번째 parameter는 우리가 바꿔주고 싶은 객체 또는 배열을 넣는다.  
- 두번째 parameter는 어떻게 바꿀지 알려주는 함수를 넣어주면 된다. `draft`라는 값을 parameter로 받아와서 내부에서 하고싶은 작업을 하면 됨  
`draft`가 가지고 있는 값은 `state`와 똑같다. 근데 여기서 변화를 주게 되면 불변성을 지키면서 새로운 객체를 만들어 줌  

```javascript
const array = [
  { id: 1, text: 'hello' },
  { id: 2, text: 'bye' },
  { id: 3, text: 'lalalala' }
]

const nextArray = produce(array, draft => {
  draft.push({ id: 4, text: 'blabla' });
  draft[0].text = draft[0].text + ' world';
});
```
`produce`가 알아서 해준다!  
