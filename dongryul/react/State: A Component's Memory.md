# State: A Component's Memory

## When a regular variable isn’t enough 
로컬변수 사용의 문제
1. 렌더 사이에 로컬 변수들은 지속되지 않는다.
2. 로컬 변수의 변경은 렌더링을 야기하지 않는다.

이러한 이유로, 새 데이터를 업데이트하기 위해 2가지 필요하다.
1. 렌더 사이에 데이터를 얻는 것
2. 새 데이터를 가진 컴포넌트를 렌더링하도록 리액트를 유발시키는 것

`useState` 훅은 2가지를 제공한다.
1. 렌더 사이에 데이터를 얻을 수 있는 상태 변수
2. 변수를 업데이트하고 리액트가 다시 그 컴포넌트를 렌더링시키게끔 하는 상태 설정 함수

## Adding a state variable 
const [state, setState] = useState(initialState); 

### Meet your first Hook 
Hook은 리액트가 렌더링하고 있을 때 사용할 수 있는 특별한 함수다. 

> 유의! <br/>
> 훅은 탑레벨에서만 호출될 수 있다. 조건문, 반복문, 중첩함수 내에서 호출될 수 없다.


### Anatomy of useState 

> 노트
> useState 컨벤션 : 맨앞은 상태이름, 두번째에는 set+상태이름을 카멜케이스로 만들어준다.

useState의 인자는 상태변수의 초기값이다. 
setState는 상태변수를 업데이트하고, 리액트가 그 컴포넌트를 다시 렌더링하게끔 유발시킨다.

## Giving a component multiple state variables 


> DEEP DIVE <br/>
> How does React know which state to return? <br/>
> 어떻게 리액트는 어떤 상태를 반환해야 하는지 알까? <br/>
> 훅은 탑레벨에서만 호출되고 그렇기 때문에 항상 같은 순서로 안정적인 호출을 하기 때문에 잘 작동한다. <br/>

## State is isolated and private
State는 각 컴포넌트에 지역적이고 분리되어있다. 같은 컴포넌트를 2번 렌더링했더라도, 각 컴포넌트는 완전히 분리된 상태를 가진다.
props와 다르게, 그것을 선언한 컴포넌트에 비공개적이다. 부모 컴포넌트는 그것을 바꿀 수 없다. 이말인 즉슨 당신은 나머지 컴포넌튿르에 영향을 주지 않고 컴포넌트에 상태를 추가하거나 제거할 수 있다는 것이다. 


---

## Challenges 1
```jsx
import { useState } from 'react';
import { sculptureList } from './data.js';

export default function Gallery() {
  const [index, setIndex] = useState(0);
  const [showMore, setShowMore] = useState(false);

  function handleNextClick() {
    setIndex(index + 1);
  }

  function handlePrevClick() {
    setIndex(index - 1);
  }

  function handleMoreClick() {
    setShowMore(!showMore);
  }

  let sculpture = sculptureList[index];
  return (
    <>
      <nav>
        <button onClick={handlePrevClick} disabled={(index === 0)}>
          Prev
        </button>
        <button onClick={handleNextClick} disabled={(index === 11)}>
          Next
        </button>
      </nav>
      <h2>
        <i>{sculpture.name} </i> 
        by {sculpture.artist}
      </h2>
      <h3>  
        ({index + 1} of {sculptureList.length})
      </h3>
      <button onClick={handleMoreClick}>
        {showMore ? 'Hide' : 'Show'} details
      </button>
      {showMore && <p>{sculpture.description}</p>}
      <img 
        src={sculpture.url} 
        alt={sculpture.alt}
      />
    </>
  );
}

```

## Challenges 2
```jsx
import { useState } from 'react';

export default function Form() {
  const [firstName, setFirstName] = useState("");
  const [lastName, setLastName] = useState("");

  function handleFirstNameChange(e) {
    setFirstName(e.target.value);
  }

  function handleLastNameChange(e) {
    setLastName(e.target.value);
  }

  function handleReset() {
    setFirstName = '';
    setLastName = '';
  }

  return (
    <form onSubmit={e => e.preventDefault()}>
      <input
        placeholder="First name"
        value={firstName}
        onChange={handleFirstNameChange}
      />
      <input
        placeholder="Last name"
        value={lastName}
        onChange={handleLastNameChange}
      />
      <h1>Hi, {firstName} {lastName}</h1>
      <button onClick={handleReset}>Reset</button>
    </form>
  );
}

```

## Challenges 3
```jsx
import { useState } from 'react';

export default function FeedbackForm() {
  const [isSent, setIsSent] = useState(false);
  const [message, setMessage] = useState('');
  if (isSent) {
    return <h1>Thank you!</h1>;
  }
  // eslint-disable-next-line
    return (
      <form onSubmit={e => {
        e.preventDefault();
        alert(`Sending: "${message}"`);
        setIsSent(true);
      }}>
        <textarea
          placeholder="Message"
          value={message}
          onChange={e => setMessage(e.target.value)}
        />
        <br />
        <button type="submit">Send</button>
      </form>
    );
}

```

## Challenges 4
```jsx
import { useState } from 'react';

export default function FeedbackForm() {
  let name;
  
  function handleClick() {
    name = prompt('What is your name?');
    alert(`Hello, ${name}!`);
  }

  return (
    <button onClick={handleClick}>
      Greet
    </button>
  );
}
```