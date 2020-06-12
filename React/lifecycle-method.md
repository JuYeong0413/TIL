# LifeCycle 메서드  
- 컴포넌트가 브라우저 상에 나타나고, 업데이트되고, 사라지게 될 때 호출되는 메서드  
- 컴포넌트에서 에러가 났을 때 호출되는 메서드도 LifeCycle 메서드의 일부  
- 클래스형 컴포넌트에서만 사용이 가능함  

#### [예제 프로젝트](https://codesandbox.io/s/currying-bash-mrkjb?fontsize=14)  
##### 기능  
1. 토글  
아래쪽 컴포넌트가 사라졌다가 나타났다가 함  
2. 랜덤 색상  
숫자의 색상이 변경됨  
3. 더하기  
숫자를 1씩 더해줌  

#### [React lifecycle methods diagram](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)  
- `Mounting`: 생성될 때  
생성될 때는 컴포넌트가 화면에 안 보여지고 있다가 보여지게 될 때를 의미함  
  - `constructor`: 컴포넌트가 생성될 때는 `constructor`(생성자 함수)가 먼저 실행된다. 이 함수에서 호출하는 것들이 가장 먼저 호출되는 내용이다.  

    ```javascript
    constructor(props) {
      super(props);
      console.log("constructor");
    }
    ```
    컴포넌트가 가장 처음 만들어질 때 호출되는 함수  
    
    `super(props);`를 하는 이유: 원래 리액트 컴포넌트 클래스가 지니고 있는 `constructor`를 먼저 호출하고 나서 그 다음에 원하고자 하는 작업을 하는 것  
  - `getDeriedStateFromProps`: `props`로 받아온 것을 `state`로 넣어야 할 때 이 메서드를 사용함  
  
    ```javascript
      static getDerivedStateFromProps(nextProps, prevState) {
      console.log("getDerivedStateFromProps");
      if (nextProps.color !== prevState.color) {
        return { color: nextProps.color };
      }
      return null;
    }
    ```
    - 첫번째 parameter: 다음 받아올 `props`  
    - 두번째 parameter: 현재 컴포넌트가 지니고 있는 상태  
    
    현재 지니고 있는 `props`랑 `state`안에 있는 어떤 값이랑 다르면 해당 값을 현재 상태에 반영  
    :point_right: `props`로 받아온 어떠한 값을 `state`에다가 동기화시켜주는 역할  
  - `render`: 클래스 형태로 만든 컴포넌트에서 `render`쪽에서 우리가 무엇을 보여주고 싶은지 `JSX` 형태로 정의해서 반환해 줘야 함  
  - React DOM 및 refs 업데이트: 실제 브라우저에서 변화 발생  
  - `componentDidMount`: `componentDidMount`가 발생하는 시점에서는 브라우저에 우리가 원하는 컴포넌트가 보여져 있는 상태이기 때문에 DOM에 직접 접근하거나 외부 라이브러리 사용 가능  
    ```javascript
      componentDidMount() {
      console.log("componentDidMount");
    }
    ```
    외부 API 호출 가능, 특정 DOM에 이벤트를 걸어줄 수도 있음  
    이 메서드가 호출되는 시점에서는 컴포넌트에 있는 element들이 브라우저에 나타나 있는 상태이므로 브라우저에 나타난 DOM에 직접 접근 가능  
- `Updating`: 업데이트 할 때  
업데이트한다는 것은 컴포넌트가 리렌더링 된다는 것을 의미함  
컴포넌트가 리렌더링 되는 경우는 부모 컴포넌트가 리렌더링 되었을 때, 자신의 `props`가 바뀌었을 때, 자신의 상태가 바뀌었을 때, 클래스형 컴포넌트에서 `force update`를 호출하게 되면 강제로 렌더링 되기도 함  
  - `getDeriedStateFromProps`: `props`로 받아온 어떤 값을 `state`안에다가 넣어주고 싶을 때 사용  
  - `shouldComponentUpdate`: 컴포넌트를 최적화해야하는 단계에서 사용함, 컴포넌트에서 리렌더링이 불필요한 시점에서 리렌더링을 막을 수 있다.  
    ```javascript
      shouldComponentUpdate(nextProps, nextState) {
      console.log("shouldComponentUpdate", nextProps, nextState);
      // 숫자의 마지막 자리가 4면 리렌더링하지 않습니다
      return nextState.number % 10 !== 4;
    }
    ```
    `false`를 반환하면 리렌더링 하지 않음, `true`를 반환하면 리렌더링함  
    이 메서드를 따로 구현하지 않으면 컴포넌트는 언제나 리렌더링하게 된다.  
  - `getSnapshotBeforeUpdate`: 브라우저에 변화를 일으키기 바로 직전에 호출되는 메서드  
    ```javascript
      getSnapshotBeforeUpdate(prevProps, prevState) {
      console.log("getSnapshotBeforeUpdate");
      if (prevProps.color !== this.props.color) {
        return this.myRef.style.color;
      }
      return null;
    }
    ```
    컴포넌트가 리렌더링되고나서 브라우저에 변화를 반영시키기 바로 직전에 DOM에 접근할 수 있다.  
    어떤 값을 리턴하게 되면 `componentDidUpdate`의 세번째 parameter에서 조회할 수 있다.  
  - `componentDidUpdate`: 브라우저에 변화가 반영되고 나서 호출됨  
    ```javascript
      componentDidUpdate(prevProps, prevState, snapshot) {
      console.log("componentDidUpdate", prevProps, prevState);
      if (snapshot) {
        console.log("업데이트 되기 직전 색상: ", snapshot);
      }
    }
    ```
      - 첫번째 parameter: 이전에 자신이 들고있던 `props`의 정보  
      - 두번째 parameter: 이전에 지니고 있던 상태  
      - 세번째 parameter: `snapshot`인 경우에 `getSnapshotBeforeUpdate`에서 반환한 값이 조회됨  
- `Unmounting`: 제거할 때  
  - `componentWillUnmount`: `componentDidMount`에서 등록한 이벤트를 지워주는 작업을 함, `setTimeout`으로 예약한 걸 없애주는 작업을 하기도 함  
    ```javascript
      componentWillUnmount() {
      console.log("componentWillUnmount");
    }
    ```
    컴포넌트가 사라지기 바로 직전에 호출되는 메서드, 등록된 이벤트를 제거하거나 `setTimeout`을 취소하는 작업을 함    

- `Render` 단계: 순수하고 부작용이 없습니다. React에 의해 일시중지, 중단 또는 재시작 될 수 있습니다.  
- `Pre-commit` 단계: DOM을 읽을 수 있습니다.  
- `Commit` 단계: DOM을 사용하여 부작용을 실행하고 업데이트를 예약할 수 있습니다.  
