# State as a snapshot
state는 스냅샷처럼 동작한다.

## Setting state triggers renders
state를 설정하는 것은 렌더링을 유발한다.

## Rendering takes a snapshot in time 
랜더링이란, 컴포넌트라는 함수를 호출한다는 뜻이다. 해당 함수에서 반환하는 jsx는 그 당시의 state를 사용해 계산된다.
react가 리렌더링될때, react가 함수를 다시 호출하고, 함수가 새로운 스냅샷을 반환하며, react가 반환한 스냅샷과 일치하도록 화면을 업데이트한다.
state를 설정하면, 다음 렌더링에 대해서만 변경된다. 한번의 이벤트에 여러번 state를 설정하면, 마지막 설정만 반영된다.

## State over time (시간에 따른 State)
비동기라 할지라도, state 변수의 값은 렌더링 내에서 달라지지 않는다.

## Challenge

### 1

```jsx
import { useState } from 'react';

export default function TrafficLight() {
  const [walk, setWalk] = useState(true);

  function handleClick() {
    setWalk(!walk);
    alert(walk ? "Stop is next" : "Walk is next")
  }

  return (
    <>
      <button onClick={handleClick}>
        Change to {walk ? 'Stop' : 'Walk'}
      </button>
      <h1 style={{
        color: walk ? 'darkgreen' : 'darkred'
      }}>
        {walk ? 'Walk' : 'Stop'}
      </h1>
    </>
  );
}

```

### 2

```jsx

```