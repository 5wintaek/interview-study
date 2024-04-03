# Responding to Events

## Adding Event Handlers
Event handler functions:
- Are usually defined inside your components.
- Have names that start with handle, followed by the name of the event.

> 유의! <br/>
> 이벤트 핸들러는 전달되기만 해야하고, 호출되면 안된다.

## Reading props in event handlers 
이벤트 핸들러에서 props에 접근할 수 있다. 

## Passing event handlers as props
이벤트 핸들러를 prop으로 전달할 수 있다.
- 디자인시스템을 사용한다면 자주 사용하는 패턴이다. 

## Naming Event Handler Props
이벤트 핸들러 prop의 이름을 커스텀할 수 있다.

## Event propagation 
리액트에서 이벤트는 부모컴포넌트로 전파된다. 

> 유의! <br/>
> onScroll 이벤트는 전파에서 예외이다.

## Stopping propagation
`e.stopPropagation()` 을 통해 부모 컴포넌트로의 전파를 막을 수 있다.

> DEEP DIVE<br/>
> onClickCapture와 같이 이벤트 이름에 Capture를 붙이면 전파는 멈추고 자식으로의 모든 이벤트를 캐치할 수 있다.

## Passing handlers as alternative to propagation
전파의 대안으로 핸들러 자체를 전달하는 패턴이 있다. 이러한 패턴은 이벤트의 결과로서 실행되는 코드 체인을 팔로우하는데 도움이 된다. 

## Preventing default behavior
`e.preventDefault()` 을 통해 브라우저 기본동작을 막을 수 있다.

## Can event handlers have side effects?
이벤트 핸들러는 사이드 이펙트를 일으키기 위한 최적의 장소이다. 컴포넌트와 달리 이벤트 핸들러는 pure할 필요없으며, 무언가를 변경해도 괜찮다.

----

## Challenge 1
```jsx
export default function LightSwitch() {
  function handleClick() {
    let bodyStyle = document.body.style;
    if (bodyStyle.backgroundColor === 'black') {
      bodyStyle.backgroundColor = 'white';
    } else {
      bodyStyle.backgroundColor = 'black';
    }
  }

  return (
    <button onClick={() => handleClick()}>
      Toggle the lights
    </button>
  );
}

```

## Challenge 2
```jsx
export default function ColorSwitch({
  onChangeColor
}) {
  return (
    <button onClick={function (e){
      e.stopPropagation();
      onChangeColor();
    }}>
      Change color
    </button>
  );
}

```