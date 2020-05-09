> 수정 날짜 (없음) / 작성 날짜 : (2020-05-9일)

# Redux 리덕스 사용법 및 정리

### ⚛️ 리덕스란?
> Redux는 애플리케이션 상태를 관리하기위한 오픈 소스 JavaScript 라이브러리입니다.   
사용자 인터페이스 구축을 위해 React 또는 Angular와 같은 라이브러리와 함께 가장 일반적으로 사용됩니다.

즉 컴포넌트들 끼리 통신할때 복잡해지고 상태 관리를 효율적으로 관리하기 위해 나온 모듈이다.

### 🔑 키워드
**Action** : 상태 변화가 필요할 때 액션 발생시킴 / 객체로 표현  
+ 액션 생성 함수에서 객체로 반환  

**리듀서** : 변화를 일으키는 함수  
**스토어** : 현재 앱 상태, 리듀서, 내장 함수  
**dispatch** : 스토어의 내장함수 중 하나 / 액션 발생

### 🤞 규칙
1. 앱 한개에는 한개의 스토어만 
- 가능은 하나 권장하지는 않음 + 개발 도구 이용불가
2. 상태는 읽기전용
- 수정말고 교체하는 방식으로 불변성 유지를 위해 `Object.assign` 또는 `... 연산자` 사용 

### 📁 리덕스 모듈 만들기

```js
// modules/counter.js

// 액션 타입

// 앞에는 중복 방지를 위해 네임스페이스를 붙여준다.
// 변수로 선언하는 이유는 자동완성 및 오타 방지를 위함이다.
const INCREASE = 'count/INCREASE';
const DECREASE = 'count/DECREASE';

//액션 생성 함수

export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });

const initialState = {
  number: 0,
};

export default function counter(state = initialState, action) {
  switch(action.type) {
    case INCREASE: 
      return {
        ...state,
        number: state.number + 1,
      };
    case DECREASE:
      return {
        ...state,
        number: state.number + 1,
      };
    default:
      return state; // 중요 : default 에서 return gownrl
  }
}
```

#### 루트 리듀서
여러 개의 리듀서가 있을 때 합치 것을 루트 리듀서라고 한다.
```js
// modules/index.js

import { combineReducers } from 'redux';
import counter from './counter';

const rootReducer = combineReducers({
  counter,
});

export default rootReducer;
```

그리고 메인에는 스토어를 만들어야한다.

```js
// index.js
// ...
import { createStore } from 'redux';
import rootReducer from './modules';

const store = createStore(rootReducer);

ReactDOM.render(<App />, document.getElementById('root'));
// ...
```

#### 리액트에서 리덕스 사용하기

```bash
$: yarn add react-redux
```


`Provider` 라는 컴포넌트 감싸준다. 그러면 컴포넌트에서 리덕스 스토어에 접근이 가능하다.
```js
// index.js

// ...
import { createStore } from 'redux';
import { Provider } from 'react-redux';
import rootReducer from './modules';

const store = createStore(rootReducer); // 스토어를 만듭니다.

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
// ...
```

### 👪 컴포넌트에서 사용

```hs
container
  - CounterContainer.js
component
  - Counter.js
```

컴포넌트는 리덕스 스토어에는 직접적으로 접근하지 않는다. 필요한 값, 함수를 props로 가져온다.
```js
// Counter.js
import React from 'react';

function Counter({ number, onIncrease, onDecrease }) {
  const onChange = e => onSetDiff(parseInt(e.target.value, 10));
  
  return (
    <div>
      <h1>{number}</h1>
      <div>
        <button onClick={onIncrease}>+</button>
        <button onClick={onDecrease}>-</button>
      </div>
    </div>
  );
}

export default Counter;
```

```js
// CounterContainer.js
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';

import Counter from '../components/Counter';
import { increase, decrease } from '../modules/counter';

function CounterContainer() {
  // useSelector: 리덕스 스토어의 상태를 조회하는 Hook
  // state === store.getState()
  const { number } = useSelector(state => ({
    number: state.counter.number,
  }));

  // useDispatch: 리덕스 스토어의 dispatch 를 함수에서 사용하게 해주는 hook
  const dispatch = useDispatch();

  const onIncrease = () => dispatch(increase());
  const onDecrease = () => dispatch(decrease());

  return (
    <Counter
      number={number}
      onIncrease={onIncrease}
      onDecrease={onDecrease}
    />
  );
}

export default CounterContainer;
```

위의 패턴은 리덕스의 창시자 Dan Abramo가 소개함  
https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0