# Conditional rendering

Your components will often need to display different things depending on different conditions. In React, you can conditionally render JSX using JavaScript syntax like `if` statements, `&&`, and `? :` operators.

당신의 컴포넌트는 종종 다른 조건에 따라 다른 것들을 보여줄 필요가 있을 것이다. 리액트에서는 당신은 조건에 따라 자바스크립트 문법의 if문, &&, ? : 연산자와 같은 것들을 사용하면서 JSX를 렌더링 할 수 있다.

## **You will learn**

- How to return different JSX depending on a condition
- How to conditionally include or exclude a piece of JSX
- Common conditional syntax shortcuts you’ll encounter in React codebases

이번 챕터에서 배울 것은…

- 어떻게 다른 JSX를 조건에 따라 return 할 수 있는지
- 조건에 따라 JSX 조각을 포함하고 배제하는 방법
- 당신이 리액트 코드베이스에서 마주하게될 일반 조건부 문법에 대한 단축키

## **Conditionally returning JSX**

Let’s say you have a `PackingList` component rendering several `Item`s, which can be marked as packed or not:

## 조건에 따라 JSX return하기

포장되었는지 아닌지를 표시하고 되어지고 있는 여러 아이템들을 렌더링하고 있는 `PackingList` 라는 컴포넌트를 당신이 갖고 있다고 해보자.

```jsx
function Item({ name, isPacked }) {
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

Notice that some of the `Item` components have their `isPacked` prop set to `true` instead of `false`. You want to add a checkmark (✔) to packed items if `isPacked={true}`.

You can write this as an `[if`/`else` statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) like so:

몇몇 Item 컴포넌트들은 false대신에 true로 셋팅되어있는  isPacked prop을 갖고 있는것을 보라. 당신은 만일`isPacked={true}` 라면 packed items에 checkmark를 더하고 싶다.

당신은 if/else 구문을 통해 이렇게 쓸 수 있다. 

```jsx
function Item({ name, isPacked }) {
  if (isPacked) {
    return <li className="item">{name} ✔</li>;
  }
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}

```

Try editing what gets returned in either case, and see how the result changes!

Notice how you’re creating branching logic with JavaScript’s `if` and `return` statements. In React, control flow (like conditions) is handled by JavaScript.

다른 케이스에선 무엇을 리턴하는지 편집해보는 것을 시도해봐라, 그리고 어떻게 결과가 바뀌는지 보아라!

당신이 자바스크립트 if와 return 구문으로 분기로 로직을 어떻게 만드는지 알아라. 리액트에서, 조건문과 같이 통제되는 flow는 자바스크립트 에 의해 조절된다. 

---

## **Conditionally returning nothing with `null`**

In some situations, you won’t want to render anything at all. For example, say you don’t want to show packed items at all. A component must return something. In this case, you can return `null`:

## 조건에 따라 null로 아무 것도 없음을 리턴하기

몇몇 상황에서 당신은 어떠한 것도 렌더링하고 싶지 않을 것이다. 예를 들어 어떠한  packed item을 전혀 보여주고 싶지 않을 수 있다. 컴포넌트는 항상 무언가를 return해야된다. 이 경우, 당신은 null을 return 할 수 있다.

```jsx
if (isPacked) {
  return null;
}
return <li className="item">{name}</li>;
```

If `isPacked` is true, the component will return nothing, `null`. Otherwise, it will return JSX to render.

만일 `isPacked` 가 true라면, 컴포넌트는 null을 반환하며 아무것도 return하지 않을 것이다. 그렇지않으면 그것은 렌더링할 JSX를 return할 것이다.

```jsx
function Item({ name, isPacked }) {
  if (isPacked) {
    return null;
  }
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}

```

In practice, returning `null` from a component isn’t common because it might surprise a developer trying to render it. More often, you would conditionally include or exclude the component in the parent component’s JSX. Here’s how to do that!

실무에서, 컴포넌트로부터 null을 return하는 것은 일반적이지 않다, 왜냐하면 그것을 렌더링하려는 개발자를 놀라게 할 수 있기 때문이다. 좀 더 자주, 당신은 조건에 따라 부모컴포넌트의 JSX에, 그 컴포넌트를 포함하거나 배제할 수 있을 것이다. 여기 그것을 어떻게 하는지 나와있다!

---

## **Conditionally including JSX**

In the previous example, you controlled which (if any!) JSX tree would be returned by the component. You may already have noticed some duplication in the render output:

앞선 예시에서, 당신은 컴포넌트에 의해 JSX 트리를 return되는 것을 컨트롤 했다.당신은 렌더링 결과물에서 몇몇 중복을 이미 알아차렸을 수도 있다. 

```jsx
<li className="item">{name} ✔</li>
```

is very similar to

이 코드는 매우 비슷하다. (아래코드와)

```jsx
<li className="item">{name}</li>
```

Both of the conditional branches return 

`<li className="item">...</li>`:

두 조건부 분기 모두 

`<li className="item">...</li>` 를 반환한다.

```jsx
if (isPacked) {
  return <li className="item">{name} ✔</li>;
}
return <li className="item">{name}</li>;
```

While this duplication isn’t harmful, it could make your code harder to maintain. What if you want to change the `className`? You’d have to do it in two places in your code! In such a situation, you could conditionally include a little JSX to make your code more [DRY.](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)

이러한 중복이 해가 되지는 않지만, 당신의 코드에서 유지보수하는 것을 더 어렵게 만든다. 만일 className을 바꾸고 싶다면? 당신은 당신의 코드에서 두곳에 그것을 해야한다! 그러한 상황에서 당신은 당신의 코드를 더 DRY(Don’t Repeat Yourself)하게 만들기 위해 조건에 따라 JSX를 일부 포함하게 할 수 있다. 

---

## **Conditional (ternary) operator (`? :`)**

JavaScript has a compact syntax for writing a conditional expression — the [conditional operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) or “ternary operator”.

## 조건부 삼항 연산자

자바스크립트는 조건문을 위한 컴팩트한 문법을 갖고 있다. 바로 조건부 연산자 혹은 ‘삼항 연산자’이다.

Instead of this: 이 코드 대신에:

```jsx
if (isPacked) {
  return <li className="item">{name} ✔</li>;
}
return <li className="item">{name}</li>;
```

You can write this: 이렇게 쓸 수 있다.

```jsx
return (
  <li className="item">
    {isPacked ? name + ' ✔' : name}
  </li>
);
```

You can read it as *“if `isPacked` is true, then (`?`) render `name + ' ✔'`, otherwise (`:`) render `name`”*.

*만일 `isPacked` 가 true라면,(`?`)  `name + ' ✔'` 를 렌더링하고, 그렇지 않으면 (`:`)`name` 을 렌더링한다.*

## DEEP DIVE

<aside>
💡 **Are these two examples fully equivalent?
이 두 예시가 완전히 동일한가요?**

If you’re coming from an object-oriented programming background, you might assume that the two examples above are subtly different because one of them may create two different “instances” of `<li>`. But JSX elements aren’t “instances” because they don’t hold any internal state and aren’t real DOM nodes. They’re lightweight descriptions, like blueprints. So these two examples, in fact, *are* completely equivalent. [Preserving and Resetting State](https://react.dev/learn/preserving-and-resetting-state) goes into detail about how this works.

당신이 만일 객체지향 프로그래밍에 대한 배경지식이 있다면, 당신은 아마 위의 두 예시가 미묘하게 다르다는 것을 아마 짐작할 거다. 왜냐하면 그것들 중 하나는 li의 두개의 다른 인스턴스를 만들기 때문이다. 그러나 JSX 요소들은 인스턴스가 아니다, 왜냐하면 그것들은 어떠한 내부적인 상태를 갖고 있지 않고 real DOM 노드가 아니기 때문이다. 이것은 청사진과 같은 간단한 설명이다. 그래서 이 두 예시는 사실 완전히 동일하다. ‘상태를 보호하고 초기화하기’는 이것들이 어떻게 동작하는지 자세히 알아볼 것이다.

</aside>

Now let’s say you want to wrap the completed item’s text into another HTML tag, like `<del>` to strike it out. You can add even more newlines and parentheses so that it’s easier to nest more JSX in each of the cases:

이제 `<del>` 과 같은 또다른 HTML 태그로 삭제하기 위해 완전한 항목의 텍스트를 넣고 싶은 경우에 대해 얘기해보자. 당신은 심지어 새로운 줄과 괄호를 추가할 수 있고, 그래서 추가적인 JSX를 각각의 경우에 중첩하기 쉬워졌다.

```jsx
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {isPacked ? (
        <del>
          {name + ' ✔'}
        </del>
      ) : (
        name
      )}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

This style works well for simple conditions, but use it in moderation. If your components get messy with too much nested conditional markup, consider extracting child components to clean things up. In React, markup is a part of your code, so you can use tools like variables and functions to tidy up complex expressions.

이러한 스타일은 간단한 조건에서 잘 동작한다, 그러나 신중하게 사용해야한다. 만일 당신의 컴포넌트가 너무 많은 중첩된 조건부 마크업으로 지저분하다면, 그것들을 정리하기 위하여 자식 컴포넌트들을 압축하는 것을 고려하라. 리액트에서는 마크업은 당신의 코드의 한 부분이며, 그래서 당신은 복잡한 표현들을 정리하기 위해서 변수나 함수와 같은 도구들을 사용할 수 있다. 

---

## **Logical AND operator (`&&`)**

Another common shortcut you’ll encounter is the [JavaScript logical AND (`&&`) operator.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND#:~:text=The%20logical%20AND%20(%20%26%26%20)%20operator,it%20returns%20a%20Boolean%20value.) Inside React components, it often comes up when you want to render some JSX when the condition is true, **or render nothing otherwise.** With `&&`, you could conditionally render the checkmark only if `isPacked` is `true`:

### 논리 AND 연산자 (&&)

당신이 마주하게 될 또다른 일반적인 단축키는 자바스크립트 논리 AND 연산자 이다. 리액트 컴포넌트 안에서 그것은 그것은  당신이 일부 JSX를 렌더링하기 원할 때 조건이 true이면, 그렇지 않으면 아무것도 렌더링되지 않는 것을 원할 때 자주 나타납니다. &&와, 당신은 조건에 따라 오로지 isPacked가 true일때만 체크마크를 렌더링할 수 있습니다. 

 

You can read this as *“if `isPacked`, then (`&&`) render the checkmark, otherwise, render nothing”*.

Here it is in action:

당신은 이것을 이렇게 읽을 수 있다. “isPacked라면, 체크마크를 렌더링하고, 그렇지 않으면 아무것도 렌더링하지 않는다”

여기 실제적으로 동작한다.

```jsx
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked && '✔'}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

A [JavaScript && expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND) returns the value of its right side (in our case, the checkmark) if the left side (our condition) is `true`. But if the condition is `false`, the whole expression becomes `false`. React considers `false` as a “hole” in the JSX tree, just like `null` or `undefined`, and doesn’t render anything in its place.

자바스크립트 && 표현식은 왼쪽의 조건이 true이면, 그것의 오른쪽의 값을 (이경우 체크마크) return한다. 그러나 조건이 false라면, 전체 표현식은 false가 된다. 리액트는 false를 null이나 undefined와 같이 jsx 트리에서 구멍으로서 간주한다. 그리고 그것의 장소에서 아무것도 렌더링하지 않는다.

<aside>
💡 위험

**Don’t put numbers on the left side of `&&`.**

To test the condition, JavaScript converts the left side to a boolean automatically. However, if the left side is `0`, then the whole expression gets that value (`0`), and React will happily render `0` rather than nothing.

For example, a common mistake is to write code like `messageCount && <p>New messages</p>`. It’s easy to assume that it renders nothing when `messageCount` is `0`, but it really renders the `0` itself!

To fix it, make the left side a boolean: `messageCount > 0 && <p>New messages</p>`.

**&& 왼쪽편에 숫자를 두지 마시오.**
조건을 테스트하기 위해 자바스크립트는 왼쪽 편을 boolean값으로 자동으로 변환한다. 그러나 왼쪽 편이 0이라면 전체 표현식은 그 값을 갖고, 리액트는 기쁘게도 아무것도 렌더링하는 대신 0을 렌더링한다. 

예를들면, 일반적인 실수는 `messageCount && <p>New messages</p>` 와 같이 코드를 쓰는 것이다. `messageCount` 가 0일때 아무것도 렌더링하지 않을 것이라 가정하는 것이 쉽지만, 실제로는 0 그자체가 렌더링된다. 그것을 수정하기 위해 왼쪽 편을 boolean으로 만들어라. `messageCount > 0 && <p>New messages</p>`

</aside>

---

## **Conditionally assigning JSX to a variable**

## 조건에 따라 JSX를 변수에 할당하기

When the shortcuts *get in the way of writing plain code*, try using an `if` statement and a variable. You can reassign variables defined with `[let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)`, so start by providing the default content you want to display, the name:

단축어가 일반 코드를 작성하는데 방해될 때, if문과 변수를 사용하는 것을 시도하라. 당신은 변수를 let으로 정의된 변수에 재할당할 수 있다, 그래서 당신이 보여지길 원하는 기본 값인, `이름`을 제공하는 것으로 시작하라

```jsx
let itemContent = name;
```

Use an `if` statement to reassign a JSX expression to `itemContent` if `isPacked` is `true`:

`isPacked` 이 true일 때 JSX 표현식을 itemContent에 재할당하기 위해, if 문을 사용하라

```jsx
if (isPacked) {
  itemContent = name + " ✔";
}
```

[Curly braces open the “window into JavaScript”.](https://react.dev/learn/javascript-in-jsx-with-curly-braces#using-curly-braces-a-window-into-the-javascript-world) Embed the variable with curly braces in the returned JSX tree, nesting the previously calculated expression inside of JSX:

중괄호는 자바스크립트로의 창문을 연다. return된 JSX 트리에서 중괄호로 끼워진 변수는 JSX안에서 이전에 계산된 표현식을 중첩하고 있다. 

```jsx
<li className="item">
  {itemContent}
</li>
```

This style is the most *verbose*, but it’s also the most flexible. Here it is in action:

이 스타일은 가장 장황하다, 그렇지만 또한 가장 유연하다. 여기 그것이 동작하고 있습니다.

```jsx
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = name + " ✔";
  }
  return (
    <li className="item">
      {itemContent}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

Like before, this works not only for text, but for arbitrary JSX too:

이전과 같이, 이것은 text뿐만 아니라 임의의 JSX에도 동작한다.

```jsx
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = (
      <del>
        {name + " ✔"}
      </del>
    );
  }
  return (
    <li className="item">
      {itemContent}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}

```

If you’re not familiar with JavaScript, this variety of styles might seem overwhelming at first. However, learning them will help you read and write any JavaScript code — and not just React components! Pick the one you prefer for a start, and then consult this reference again if you forget how the other ones work.

만일 당신이 자바스크립트에 친숙하지 않다면, 이러한 다양한 스타일은 처음엔 압도적으로 보일 수 있다. 그러나 그것들을 배우는 것은 당신이 리액트 컴포넌트뿐만 아니라 어떠한 자바스크립트 코드를 읽고 쓰는 것도 도울 것이다. 당신이 시작하기에 선호하는 것을 하나 골라라 그리고 이 참조를 다시 찾아 만일 당신이 다른 것들이 어떻게 동작하는지를 잊어버렸다면.

---

## 복습

- In React, you control branching logic with JavaScript.
- You can return a JSX expression conditionally with an `if` statement.
- You can conditionally save some JSX to a variable and then include it inside other JSX by using the curly braces.
- In JSX, `{cond ? <A /> : <B />}` means *“if `cond`, render `<A />`, otherwise `<B />`”*.
- In JSX, `{cond && <A />}` means *“if `cond`, render `<A />`, otherwise nothing”*.
- The shortcuts are common, but you don’t have to use them if you prefer plain `if`.

- 리액트에서, 당신은 분기처리 로직을 자바스크립트로 컨트롤한다.
- 당신은 if문과 함께 jsx 표현식을 조건에 따라 return할 수 있다.
- 당신은 조건에 따라 일부 jsx를 변수에 저장할 수 있고, 그리고 나서 중괄호를 사용하여 다른 JSX 안에 그것을 포함시킬 수 있다.
- JSX에서 조건 ? A : B는 조건일 때, A를 렌더링하고 그렇지 않을때 B를 렌더링한다.
- JSX에서 조건 && A는 조건일 때 A를 렌더링하고 그렇지 않을때는 아무것도 렌더링하지 않는다.
- 단축어는 일반적이다, 그러나 일반if를 당신이 선호한다면, 당신은 그것들을 사용할 필요는 없다.