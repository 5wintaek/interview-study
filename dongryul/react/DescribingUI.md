> React is a JavaScript library for rendering user interfaces (UI). UI is built from small units like buttons, text, and images. React lets you combine them into reusable, nestable *components.* From web sites to phone apps, everything on the screen can be broken down into components. In this chapter, you’ll learn to create, customize, and conditionally display React components.
> 

리액트는 ui를 렌더링하는 자바스크립트 라이브러리이다. 

ui는 버튼이나 테스트 이미지와 같은 작은 유닛에서부터 만들어진다. 리액트는 당신이 그것들을 재사용가능하고 중첩가능한 컴포넌트들로 결합할 수 있게 한다. 

웹사이트에서 휴대폰 앱까지 스크린의 모든 것은 컴포넌트들로 쪼개어질 수 있다. 이번 챕터에서 당신은 리액트 컴포넌트들을 만들고, 커스터마이징하고, 조건에 따라 보여지게끔 하는 것을 배울 것이다.

> **In this chapter**
> 
> - [How to write your first React component](https://react.dev/learn/your-first-component)
> - [When and how to create multi-component files](https://react.dev/learn/importing-and-exporting-components)
> - [How to add markup to JavaScript with JSX](https://react.dev/learn/writing-markup-with-jsx)
> - [How to use curly braces with JSX to access JavaScript functionality from your components](https://react.dev/learn/javascript-in-jsx-with-curly-braces)
> - [How to configure components with props](https://react.dev/learn/passing-props-to-a-component)
> - [How to conditionally render components](https://react.dev/learn/conditional-rendering)
> - [How to render multiple components at a time](https://react.dev/learn/rendering-lists)
> - [How to avoid confusing bugs by keeping components pure](https://react.dev/learn/keeping-components-pure)
> - [Why understanding your UI as trees is useful](https://react.dev/learn/understanding-your-ui-as-a-tree)

### 이번 챕터에서는…

- 당신의 첫번째 리액트 컴포넌트를 작성하는 방법
- 언제 그리고 어떻게 멀티 컴포넌트 파일을 만드는지
- jsx로 자바스크립트에 마크업을 더하는 방법
- 당신의 컴포넌트로부터 자바스크립트 기능에 접근하기 위해 jsx로 중괄호를 사용하는 방법
- props로 컴포넌트들을 환경설정하는 방법
- 조건에 따라 컴포넌트를 렌더링 하는 방법
- 여러 컴포넌트들을 동시에 렌더링 하는 방법
- 컴포넌트들을 pure하게 유지함으로서 혼란스러운 버그들로부터 피할 수 있는 방법
- 왜 당신의 ui를 트리로서 이해하는 것이 유용한지

### Your first component

> React applications are built from isolated pieces of UI called *components*. A React component is a JavaScript function that you can sprinkle with markup. Components can be as small as a button, or as large as an entire page. Here is a `Gallery` component rendering three `Profile` components:
> 

리액트 어플리케이션은 컴포넌트라 불리우는 ui의 독립된 조각들로부터 만들어진다. 리액트 컴포넌트는 당신이 마크업으로 뿌릴 수 있는 자바스크립트 함수이다. 컴포넌트는 버튼과 같이 작을수도, 전체 페이지와 같이 클 수도 있다. 여기 3개의 프로필 컴포넌트를 렌더링하고 있는 갤러리 컴포넌트가 있다.

---

### **Importing and exporting components**

> You can declare many components in one file, but large files can get difficult to navigate. To solve this, you can *export* a component into its own file, and then *import* that component from another file:
> 

당신은 많은 컴포넌트들을 하나의 파일에 선언할 수 있다, 그렇지만 큰 파일은 탐색하기 어렵게 할 수 있다. 이러한 것을 해결하기 위해 당신은 그 파일 자체를 하나의 컴포넌트로 내보낼 수 있고, 그러고 난 다음 다른 파일에서 그 컴포넌트를 불러올 수 있다. 

---

### Writing markup with JSX

> Each React component is a JavaScript function that may contain some markup that React renders into the browser. React components use a syntax extension called JSX to represent that markup. JSX looks a lot like HTML, but it is a bit stricter and can display dynamic information.
> 

각각의 리액트 컴포넌트는 리액트가 브라우저로 렌더링하는 일부 마크업을 포함하는 자바스크립트 함수이다. 리액트 컴포넌트는 마크업을 표현하기 위해 jsx라는 확장 문법을 사용한다. jsx는 많은 것이 html과 같이 보이지만, HTML보다는 더 엄격한 문법이며, 동적 정보를 보여줄 수 있다.

> If we paste existing HTML markup into a React component, it won’t always work:
> 

우리가 만일 html 마크업을 리액트 컴포넌트에 붙여넣으면 그것은 작동하지 않을 것이다.

> If you have existing HTML like this, you can fix it using a [converter](https://transform.tools/html-to-jsx):
> 

당신이 이것과 같이 존재하는 HTML을 갖고있다면, 그것을 HTML-to-JSX 컨버터를 통해 고칠 수 있을 것이다. 

---

### **JavaScript in JSX with curly braces**

> JSX lets you write HTML-like markup inside a JavaScript file, keeping rendering logic and content in the same place. Sometimes you will want to add a little JavaScript logic or reference a dynamic property inside that markup. In this situation, you can use curly braces in your JSX to “open a window” to JavaScript:
> 

JSX는 당신이 HTML과 같은 마크업을 렌더링 로직과 콘텐츠를 같은 장소 내에 유지하면서,  자바스크립트 파일 안에 작성하게끔 해준다. 때때로 당신은 마크업안에 조금의 자바스크립트 로직이나 동적 프로퍼티에 대한 참조를 추가하길 원할 것이다. 이런 상황에서, 당신은 당신의 jsx안에 “자바스크립트로 통하는 창문”을 열기 위해 중괄호를 사용할 수 있다.

---

### **Passing props to a component**

> React components use *props* to communicate with each other. Every parent component can pass some information to its child components by giving them props. Props might remind you of HTML attributes, but you can pass any JavaScript value through them, including objects, arrays, functions, and even JSX!
> 

리액트 컴포넌트는 props를 사용하여 서로 커뮤니케이션한다. 모든 부모 컴포넌트는 그것의 자식 컴포넌트들에 props를 줌으로써 몇몇 정보들을 넘겨줄 수 있다. Props는 당신이 HTML 어트리뷰트를 떠올리게 한다, 그렇지만 당신은 어떠한 자바스크립트 값을 그것들을 통해 전달할 수 있다, 객체, 배열, 함수 심지어는 JSX까지도 말이다.

---

### **Conditional rendering**

> Your components will often need to display different things depending on different conditions. In React, you can conditionally render JSX using JavaScript syntax like `if` statements, `&&`, and `? :` operators.
> 

당신의 컴포넌트들은 다른 조건에 따라 다른 것들을 보여줄  필요가 있을 것이다. 리액트에서, 당신은 if문, && 그리고 ? : 연산자와 같은 자바스크립트 문법을 사용하여 조건별로 jsx를 렌더링할 수 있다.

> In this example, the JavaScript `&&` operator is used to conditionally render a checkmark:
> 

이 예시에서, 자바스크립트 && 연산자는 조건별로 체크마크를 렌더링하는데 사용된다.

---

### Rendering lists

> You will often want to display multiple similar components from a collection of data. You can use JavaScript’s `filter()` and `map()` with React to filter and transform your array of data into an array of components.
> 

당신은 종종 data 컬렉션으로부터 여러 비슷한 컴포넌트들을 보여주길 원할 것이다. 당신은 당신의 data 배열을 컴포넌트들의 배열로 필터링하고 바꾸기 위해, 리액트와 자바스크립트의 filter와 map을 사용할 수 있다.  

> For each array item, you will need to specify a `key`. Usually, you will want to use an ID from the database as a `key`. Keys let React keep track of each item’s place in the list even if the list changes.
> 

각 배열 아이템에 우리는 키를 특정시킬 필요가 있다. 보통 우리는 데이터베이스의 id를 키로서 사용하길 원할 것이다. key는 리액트가, 리스트가 변하더라도 리스트 내의 각 아이템의 위치를 추적하게끔 하는 역할이다.

---

### Keeping components pure

> Some JavaScript functions are *pure.* A pure function:
- **Minds its own business.** It does not change any objects or variables that existed before it was called.
- **Same inputs, same output.** Given the same inputs, a pure function should always return the same result.

By strictly only writing your components as pure functions, you can avoid an entire class of baffling bugs and unpredictable behavior as your codebase grows. Here is an example of an impure component:
> 

몇몇 자바스크립트 함수는 pure하다. 

pure function이란

- 함수 자체의 업무만을 다룬다. 함수가 호출되기전에 기존에 존재하던 어떠한 객체나 변수를 바꾸지 않는다.
- 같은 input을 넣으면 같은 output이 나온다. 같은 input이 주어지면, pure function은 항상 같은 결과를 return해야만한다.

엄격하게 오로지 당신의 컴포넌트들이 pure function으로서 작성된다면, 당신은 당신의 코드베이스가 커질때 전체 단위의 당황스러운 버그와 예상치못한 현상들을 피할 수 있을 것입니다. 여기 pure하지 않은 컴포넌트에 대한 예시가 있습니다.

> You can make this component pure by passing a prop instead of modifying a preexisting variable:
> 

당신은 이미 존재하는 변수를 수정하는 대신, prop을 전달함으로써, 이 컴포넌트를 pure하게 만들수 있습니다. 

---

### Your UI as a tree

> React uses trees to model the relationships between components and modules. 
A React render tree is a representation of the parent and child relationship between components.
> 

리액트는 컴포넌트와 모듈간의 관계를 모델링하기 위해 트리구조를 사용한다. 리액트 렌더트리는 컴포넌트간의 부모와 자식 관계를 대표한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f7ce23c1-8c76-4d24-9fab-def7d0d68c40/ec621cab-8052-4863-b478-301c1da43004/Untitled.png)

> Components near the top of the tree, near the root component, are considered top-level components. Components with no child components are leaf components. This categorization of components is useful for understanding data flow and rendering performance.

Modelling the relationship between JavaScript modules is another useful way to understand your app. We refer to it as a module dependency tree.
> 

컴포넌트들은 트리의 꼭대기에 가까울수록, 루트 컴포넌트에 가까울 수록, 탑 레벨 컴포넌트로 여겨진다. 자식이 없는 컴포넌트들은 리프 컴포넌트들이다. 이러한 컴포넌트들의 카테고리화는 data flow와 렌더링 퍼포먼스를 이해할때 유용하다.

![Untitled]()

> A dependency tree is often used by build tools to bundle all the relevant JavaScript code for the client to download and render. A large bundle size regresses user experience for React apps. Understanding the module dependency tree is helpful to debug such issues.
> 

의존성 트리는 종종 클라이언트가 다운로드하고 렌더링하기 위해 모든 관계있는 자바스크립트 코드들을 번들링하는 빌드툴들에 의해서 사용된다. 큰 번들 사이즈는 리액트 앱의 사용자 경험을 반감시킨다. 모듈 의존성 트리를 이해하는 것은 그러한 이슈들을 디버그하는데 도움이 된다.