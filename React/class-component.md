# class형 컴포넌트  
#### Hello.js  
```javascript
import React, { Component } from 'react';

class Hello extends Component {
  render() {
    return (
      <div>
        {this.props.isSpecial && <b>*</b>}
        안녕하세요 {this.props.name}
      </div>
    )
  }
}

Hello.defaultProps = {
  name: '이름없음'
}

export default Hello;

- `class`로 시작함  
- `render`라는 메서드가 있어야 하고 이 내부에서는 `JSX`를 반한해줘야 함  
- `props`를 읽으려면 `this.props.[props이름]`  

비구조할당을 통해서 값을 따로 선언할 수도 있음  
```javascript
class Hello extends Component {
  render() {
    const { color, isSpecial, name } = this.props;
    return (
      <div style={{ color }}>
        {isSpecial && <b>*</b>}
        안녕하세요 {name}
      </div>
    )
  }
}
```
`this.props` 안에 있는 `color`, `isSpecial`, `name`을 따로 추출해서 선언하겠다는 의미  

## defaultProps를 설정하는 방법  
1.  
```javascript
Hello.defaultProps = {
  name: '이름없음'
}
```
2.  
```javascript
class Hello extends Component {
  static defaultProps = {
    name: '이름없음',
  };

  render() {
    const { color, isSpecial, name } = this.props;
    return (
      <div style={{ color }}>
        {isSpecial && <b>*</b>}
        안녕하세요 {name}
      </div>
    )
  }
}
```

## class형 컴포넌트의 state와 setState  
#### Counter.js  
```javascript
import React, { Component } from 'react';

class Counter extends Component {

  handleIncrease() {
    console.log('increase');
  }

  handleDecrease() {
    console.log('decrease');
  }

  render() {
    return (
      <div>
        <h1>0</h1>
        <button onClick={this.handleIncrease}>+1</button>
        <button onClick={this.handleDecrease}>-1</button>
      </div>
    );
  }
}

export default Counter;
```
- custom method: 클래스 내부에 함수를 선언하는 것 (`render`는 컴포넌트가 자체적으로 지니고 있는 메서드)  

상태를 업데이트하기 위해서는 `this.setState`를 사용해야 함  
문제는 함수를 `onClick`에 연결해주었는데 그렇게되면 함수에서 `console.log(this);` 했을 때 컴포넌트 인스턴스 자기 자신을 가리켜야 하는데 이 함수를 특정 이벤트에다가 연결시켜주는 순간 함수와 `this`와의 연결이 사라져버려서 함수가 실행되는 시점에는 `this`가 무엇인지 모르게 된다. (`undefined`라고 나옴) :point_right: 메서드를 버튼 이벤트로 등록하는 과정에서 각 메서드와 컴포넌트 인스턴스와의 관계가 끊겨버리기 때문  

#### 해결방법  
1. 컴포넌트의 생성자 함수인 `constructor`에서 함수를 미리 `bind`해주는 것  
```javascript
constructor(props) {
  super(props);
  this.handleIncrease = this.handleIncrease.bind(this);
  this.handleDecrease = this.handleDecrease.bind(this);
}
```
- `constructor`는 `props`라는 parameter를 가져야 함  
- `bind`라는 작업을 하게 되면 이 함수에서 `this`를 가리키게 된다면 `constructor`에서 사용하는 `this`를 가리키게 하라는 것  

2. custom method를 작성할 때 화살표 함수 사용  
```javascript
handleIncrease = () => {
  console.log(this);
  console.log('increase');
}

handleDecrease = () => {
  console.log('decrease');
}
```

## 상태관리  
```javascript
import React, { Component } from 'react';

class Counter extends Component {

  constructor(props) {
    super(props);
    this.state = {
      counter: 0
    };
  }

  handleIncrease = () => {
    this.setState({
      counter: this.state.counter + 1
    });
  }

  handleDecrease = () => {
    this.setState({
      counter: this.state.counter - 1
    });
  }

  render() {
    return (
      <div>
        <h1>{this.state.counter}</h1>
        <button onClick={this.handleIncrease}>+1</button>
        <button onClick={this.handleDecrease}>-1</button>
      </div>
    );
  }
}

export default Counter;
```
- `constructor`에서 `this.state`에다가 해당 컴포넌트에서 사용할 초기값을 넣어주면 된다.  
:bulb: `state`는 무조건 객체 형태여야 한다.  
- `constructor` 대신에 다른 방법은  
```javascript
state = {
  counter: 0
}
```
처럼 바로 값을 지정해주는 것  
정식 `JavaScript` 문법은 아니고 Babel 플러그인을 통해서 사용할 수 있는 class properties 문법  

#### 다뤄야 하는 상태가 객체라면  
```javascript
state = {
  counter: 0,
  fixed: 1,
  updateMe: {
    toggleMe: false,
    dontChangeMe: 1,
  }
}
```
상태 안에 들어있는 게 객체라면 객체에 대해서는 불변성을 유지해줘야 함  
```javascript
handleToggle = () => {
  this.setState({
    updateMe: {
      ...this.state.updateMe,
      toggleMe: !this.state.updateMe.toggleMe,
    }
  });
}
```
일반적으로는 객체를 통째로 집어넣고나서 원하는 값을 덮어씌워주는 형태로 작성함  
```javascript
dontChangeMe: this.state.updateMe.dontChangeMe
```
이렇게 해도 됨  

#### setState의 함수형 업데이트  
```javascript
handleIncrease = () => {
  this.setState(state => ({
    counter: state.counter + 1
  }));
}
```
`state`를 parameter로 가져와서 원하는 변화를 준 객체를 반환  
