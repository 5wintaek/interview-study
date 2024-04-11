# Queueing a Series of State Updates

## React batches state updates (리액트는 state 업데이트를 묶는다.)
React는 state 업데이트를 하기전에 이벤트 핸들러의 모든 코드가 실행될 때까지 기다린다.
이벤트 핸들러와 그 안의 코드가 완료될 때까지 ui 업데이트를 하지 않는다. 이를 batching이라고 한다.
클릭과 같은 여러 의도적 이벤트들을 넘어 배치처리하진 않는다. 각 클릭은 분리되어 핸들링된다. 리액트는 오로지 그것이 안전하게 해도될때 배치처리를 한다.

## Updating the same state multiple times before the next render (다음 렌더링 전에 같은 state를 여러번 업데이트 하는 것)
다음 렌더링 전에 같은 state 전에 여러번 업데이트 하는 경우에는 `setState(prev => prev + 1)`과 같이 리액트에게 state 값에 무언가 한다 라는 것을 전달해주면 된다.
위와 같은 함수는 updater function이라고 불린다. 
1. 리액트는 이벤트 핸들러 안에 있는 다른 모든 코드가 실행된 후에, 진행되어야하는 이 함수를 큐에 넣는다.
2. 다음 렌더링 동안, 리액트는 그 큐를 훑고 당신에게 마지막 업데이트된 상태를 제공한다.

### What happens if you update state after replacing it (만일 state를 교체한 후에 업데이트 한다면 무슨 일이 일어날까?)
교체 후 업데이트를 하면 교체된 state를 업데이트하므로 기대한 대로 동작할 수 있다. 

### What happens if you replace state after updating it (만일 state를 업데이트한 후에 교체한다면 무슨 일이 일어날까?)
업데이트 후 교체하게되면 최종 교체한 state만 남게된다. 
업데이터 함수는 순수해야한다. 결과만 반환해야한다. 업데이터 함수 내에서 state를 변경하거나 다른 사이드 이펙트를 실행하면 안된다. 

### Naming convention
setState의 이름은 state 변수의 첫글자로 지정하는 것이 일반적이다. 혹은 prev와 같은 접두사를 사용할 수 있다.

## Challenge

### 1
  
```jsx
import { useState } from 'react';

export default function RequestTracker() {
  const [pending, setPending] = useState(0);
  const [completed, setCompleted] = useState(0);

  async function handleClick() {
    setPending(pending => pending + 1);
    await delay(3000);
    setPending(pending => pending - 1);
    setCompleted(completed => completed + 1);
  }

  return (
    <>
      <h3>
        Pending: {pending}
      </h3>
      <h3>
        Completed: {completed}
      </h3>
      <button onClick={handleClick}>
        Buy     
      </button>
    </>
  );
}

function delay(ms) {
  return new Promise(resolve => {
    setTimeout(resolve, ms);
  });
}

```

### 2

```jsx
export function getFinalState(baseState, queue) {
  let finalState = baseState;
  queue.forEach(el => {
    if (typeof el === 'function') {
      finalState = el(finalState)
    } else finalState = el;
  })

  return finalState;
}

```