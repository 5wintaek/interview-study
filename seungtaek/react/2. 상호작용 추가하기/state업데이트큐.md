### state 업데이트 큐

state 변수를 설정하면 다음 렌더링이 큐에 들어갑니다. 그러나 때에 따라 다음 렌더링을 큐에 넣기 전에, 값에 대해 여러 작업을 수행하고 싶을 때도 있습니다. 이를 위해서는 React가 state 업데이트를 어떻게 배치하면 좋을지 이해하는 것이 도움이 됩니다.

### React state batches 업데이트

```jsx
import { useState } from "react";

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button
        onClick={() => {
          setNumber(number + 1);
          setNumber(number + 1);
          setNumber(number + 1);
        }}
      >
        +3
      </button>
    </>
  );
}
```

setNumber(number + 1) 를 세 번 호출하므로 “+3” 버튼을 클릭하면 세 번 증가할 것으로 예상할 수 있습니다.

하지만 각 렌더링의 state값은 고정되어 있으므로(state스냅샷), 첫 번쨰 렌더링의 이벤트 핸들러의 number 값은 setNumber(1)을 몇번 호출하든 **항상 0** 입니다.

```jsx
setNumber(0 + 1);
setNumber(0 + 1);
setNumber(0 + 1);
```

하지만 여기에는 한가지 요인이 더 있습니다. React는 state 업데이트를 하기 전에**이벤트 핸들러의 모든 코드가 실행될 때까지 기다립니다.** 이 때문에 리렌더링은 모든 setNumber() 호출이 완료된 이후에만 일어납니다.

이렇게 하면 너무 많은 리렌더링을 촉발하지 않고도 여러 컴포넌트에서 나온 다수의 state 변수를 업데이트할 수 있습니다. 하지만 이는 이벤트 핸들러와 그 안에 있는 코드가 완료될 때까지 UI가 업데이트되지 않는다는 의미이기도 합니다. 일괄처리(배칭, batching)라고도 하는 이 동작은 React 앱을 훨씬 빠르게 실행할 수 있게 해줍니다. 또한 일부 변수만 업데이트된 “반쯤 완성된” 혼란스러운 렌더링을 처리하지 않아도 됩니다.

### 다음 렌더링 전에 동일한 state 변수를 여러 번 업데이트하기

흔한 사례는 아니지만, 다음 렌더링 전에 동일한 state 변수를 여러 번 업데이트 하고 싶다면 setNumber(number + 1) 와 같은 다음 state 값을 전달하는 대신, setNumber(n => n + 1) 와 같이 이전 큐의 state를 기반으로 다음 state를 계산하는 함수를 전달할 수 있습니다. 이는 단순히 state 값을 대체하는 것이 아니라 React에 “state 값으로 무언가를 하라”고 지시하는 방법입니다.

```jsx
import { useState } from "react";

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button
        onClick={() => {
          setNumber((n) => n + 1);
          setNumber((n) => n + 1);
          setNumber((n) => n + 1);
        }}
      >
        +3
      </button>
    </>
  );
}
```

+3씩 증가하는걸 볼 수 있습니다. 여기서 n => n + 1 은 **업데이터 함수(updater function)**라고 부릅니다. 이를 state 설정자 함수에 전달 할 때,

1. React는 이벤트 핸들러의 다른 코드가 모두 실행된 후에 이 함수가 처리되도록 큐에 넣습니다.
2. 다음 렌더링 중에 React는 큐를 순회하여 최종 업데이트된 state를 제공합니다.

```jsx
setNumber((n) => n + 1);
setNumber((n) => n + 1);
setNumber((n) => n + 1);
```

React가 이벤트 핸들러를 수행하는 동안 여러 코드를 통해 작동하는 방식은 다음과 같습니다.

1. setNumber(n => n + 1): n => n + 1 함수를 큐에 추가합니다.
2. setNumber(n => n + 1): n => n + 1 함수를 큐에 추가합니다.
3. setNumber(n => n + 1): n => n + 1 함수를 큐에 추가합니다.

다음 렌더링 중에 useState 를 호출하면 React는 큐를 순회합니다. 이전 number state는 0이었으므로 React는 이를 첫 번째 업데이터 함수에 n 인수로 전달합니다. 그런 다음 React는 이전 업데이터 함수의 반환 값을 가져와서 다음 업데이터 함수에 n으로 전달하는 식으로 반복합니다.

![alt text](./images/image-1.png)

React는 3을 최종 결과로 저장하고 useState에서 반환합니다.

```jsx
import { useState } from "react";

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button
        onClick={() => {
          setNumber((n) => n + 1);
          console.log(number);
          setNumber((n) => n + 1);
          console.log(number);
          // setNumber(n => n + 1);
        }}
      >
        +2
      </button>
    </>
  );
}
```

첫번째 클릭 후 console.log(0) 이 찍히는 모습
![alt text](./images/image-2.png)

두번째 클릭 후 console.log(2) 가 찍히는 모습

![alt text](./images/image-3.png)

### state를 교체한 후 업데이트하면 어떻게 되나요?

이 이벤트 핸들러는 어떨까요? 다음 렌더링에서 number가 어떻게 될까요?

```jsx
<button onClick={() => {
  setNumber(number + 5);
  setNumber(n => n + 1);
}}>
```

이 이벤트 핸들러가 React에 지시하는 작업은 다음과 같습니다.

1. setNumber(number + 5) : number는 0이므로 setNumber(0 + 5)입니다. React는 큐에 “5로 바꾸기” 를 추가합니다.
2. setNumber(n => n + 1) : n => n + 1는 업데이터 함수입니다. React는 해당 함수를 큐에 추가합니다. n은 5를 머금고 있으니 5+1 = 6 입니다.

![alt text](./images/image-4.png)

React는 6을 최종 결과로 저장하고 useState에서 반환합니다.

**중요합니다 !**
setState(5)가 실제로는 setState(n => 5) 처럼 동작하지만 n이 사용되지 않는다는 것을 눈치채셨을 것입니다!

### 업데이트 후 state를 바꾸면 어떻게 되나요?

한 가지 예를 더 들어보겠습니다. 다음 렌더링에서 number가 어떻게 될까요?

**나의 생각**

1. number(0) + 5 => 5
2. setNumber(n => n + 1) => n값은 5로 저장 5+1 = 6
3. setNumber(42) => 42로 교체했으니 최종적으로는 42값

```jsx
<button onClick={() => {
  setNumber(number + 5);
  setNumber(n => n + 1);
  setNumber(42);
}}>
```

![alt text](./images/image-5.png)

이 이벤트 핸들러를 실행하는 동안 React가 이 코드를 통해 작동하는 방식은 다음과 같습니다.

1. setNumber(number + 5): number 는 0 이므로 setNumber(0 + 5)입니다. React는 “5로 바꾸기” 를 큐에 추가합니다.
2. setNumber(n => n + 1): n => n + 1 는 업데이터 함수입니다. React는 이 함수를 큐에 추가합니다.
3. setNumber(42): React는 “42로 바꾸기” 를 큐에 추가합니다.

![alt text](./images/image-6.png)

그런 다음 React는 42를 최종 결과로 저장하고 useState에서 반환합니다.

요약하자면, setNumber state 설정자 함수에 전달할 내용은 다음과 같이 생각할 수 있습니다:

- 업데이터 함수 (예. n => n + 1) 가 큐에 추가됩니다.
- 다른 값 (예. 숫자 5) 은 큐에 “5로 바꾸기”를 추가하며, 이미 큐에 대기 중인 항목은 무시합니다.

이벤트 핸들러가 완료되면 React는 리렌더링을 실행합니다. 리렌더링하는 동안 React는 큐를 처리합니다. **업데이터 함수는 렌더링 중에 실행되므로, 업데이터 함수는 순수해야 하며 결과만 반환 해야 합니다**. 업데이터 함수 내부에서 state를 변경하거나 다른 사이드 이팩트를 실행하려고 하지 마세요. Strict 모드에서 React는 각 업데이터 함수를 두 번 실행(두 번째 결과는 버림)하여 실수를 찾을 수 있도록 도와줍니다.

### Recap요약

- state를 설정하더라도 기존 렌더링의 변수는 변경되지 않으며, 대신 새로운 렌더링을 요청합니다.
- React는 이벤트 핸들러가 실행을 마친 후 state 업데이트를 처리합니다. 이를 batching 이라고 합니다.
- 하나의 이벤트에서 일부 state를 여러 번 업데이트하려면 setNumber(n => n + 1) 업데이터 함수를 사용할 수 있습니다.

### 첼린지

```jsx
const [pending, setPending] = useState(0);
const [completed, setCompleted] = useState(0);

async function handleClick() {
  setPending((p) => p + 1);
  // await delay(3000);
  console.log(pending);
  setPending(pending - 1);
  // setCompleted(completed + 1);
}
```

풀이과정 -> [코드샌드박스](https://codesandbox.io/p/sandbox/react-dev-forked-lpq9mq?file=%2Fsrc%2FApp.js%3A13%2C28)

풀이과정 -> [코드샌드박스](https://codesandbox.io/p/sandbox/react-dev-forked-r3sd6n?file=%2Fsrc%2FprocessQueue.js%3A5%2C40)
