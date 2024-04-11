# Updating Objects in State

## What's the mutation? (변경이란 무엇인가)
원시값은 변경할 수 없다. 
객체 자체의 내용은 변경할 수 있다. 이를 mutation이라고 한다.

## Treat state as read-only
state는 읽기전용 데이터처럼 다뤄야한다. 
그래서 state가 객체일 때, 객체가 변경가능함에도 불구하고 읽기전용처럼 다뤄, 수정하기 보다는 교체하는 방식으로 다뤄야한다.

> DEEPDIVE <br/>
> 지역 변경을 괜찮다. 문제가 생기는 경우는 state로 있는 존재하는 객체를 변경할때 생겨난다. 어디도 참조하고 있지 않은 갓 만든 객체를 변경하는 것은 괜찮다. 

## Copying objects with the spread syntax 
스프레드 연산자를 통해, 변경하고 싶은 필드를 제외한 객체의 나머지 모든 필드를 복사하고, 변경하고 싶은 필드만 변경한다.

> DEEP DIVE
> 아래와 같이 [ ] 을 사용해 동적 프로퍼티를 지정할수있다.
```jsx
function handleChange(e) {
    setPerson({
      ...person,
      [e.target.name]: e.target.value
    });
  }
```

## Updating a nested object
중첩된 객체를 변경하려면 번거롭지만 중첩된 객체까지 복사하여 변경해야한다.

> DEEP DIVE : 객체는 사실 정말 중첩된게 아니다
> 중첩되었다기보다는 하위 객체를 가리키고 있다는 표현이 정확할 것이다. 

## Write concise update logic with Immer 
깊히 중첩된 상태라면, 그것을 평탄화하는 것을 고려할 것이다. 그러나 당신의 상태구조를 변화하길 원하지 않다면, immer를 사용해보자. 

1. npm install use-immer
2. import { useImmer } from 'use-immer';

> DEEP DIVE : Why is mutationg state not recommended in React?
> 1. 디버깅
> 2. 최적화
> 3. 새 기능
> 4. 요구사항 변화 : 재설정, 되돌리기와 같은 기능
> 5. 더 단순한 실행

## Challenge

### 1

```jsx
import { useState } from 'react';

export default function Scoreboard() {
  const [player, setPlayer] = useState({
    firstName: 'Ranjani',
    lastName: 'Shettar',
    score: 10,
  });

  function handlePlusClick() {
    const newScore = player.score + 1;
    setPlayer({...player, score: newScore});
  }

  function handleFirstNameChange(e) {
    setPlayer({
      ...player,
      firstName: e.target.value,
    });
  }

  function handleLastNameChange(e) {
    setPlayer({
      ...player,
      lastName: e.target.value
    });
  }

  return (
    <>
      <label>
        Score: <b>{player.score}</b>
        {' '}
        <button onClick={handlePlusClick}>
          +1
        </button>
      </label>
      <label>
        First name:
        <input
          value={player.firstName}
          onChange={handleFirstNameChange}
        />
      </label>
      <label>
        Last name:
        <input
          value={player.lastName}
          onChange={handleLastNameChange}
        />
      </label>
    </>
  );
}
```

### 2

```jsx
import { useState } from 'react';
import Background from './Background.js';
import Box from './Box.js';

const initialPosition = {
  x: 0,
  y: 0
};

export default function Canvas() {
  const [shape, setShape] = useState({
    color: 'orange',
    position: initialPosition
  });

  function handleMove(dx, dy) {
    setShape({...shape,
      position: {
        x: shape.position.x + dx,
        y: shape.position.y + dy,
      },
    })
  }

  function handleColorChange(e) {
    setShape({
      // position: initialPosition,
      ...shape,
      color: e.target.value
    });
  }

  return (
    <>
      <select
        value={shape.color}
        onChange={handleColorChange}
      >
        <option value="orange">orange</option>
        <option value="lightpink">lightpink</option>
        <option value="aliceblue">aliceblue</option>
      </select>
      <Background
        position={initialPosition}
      />
      <Box
        color={shape.color}
        position={shape.position}
        onMove={handleMove}
      >
        Drag me!
      </Box>
    </>
  );
}
```

### 3

```jsx
import { useState } from 'react';
import { useImmer } from 'use-immer';
import Background from './Background.js';
import Box from './Box.js';

const initialPosition = {
  x: 0,
  y: 0
};

export default function Canvas() {
  const [shape, setShape] = useImmer({
    color: 'orange',
    position: initialPosition
  });

  function handleMove(dx, dy) {
    setShape(draft => {
      draft.position.x += dx;
      draft.position.y += dy;
      })
  }

  function handleColorChange(e) {
    setShape(draft => {
      draft.color = e.target.value;
    });
  }

  return (
    <>
      <select
        value={shape.color}
        onChange={handleColorChange}
      >
        <option value="orange">orange</option>
        <option value="lightpink">lightpink</option>
        <option value="aliceblue">aliceblue</option>
      </select>
      <Background
        position={initialPosition}
      />
      <Box
        color={shape.color}
        position={shape.position}
        onMove={handleMove}
      >
        Drag me!
      </Box>
    </>
  );
}

```
