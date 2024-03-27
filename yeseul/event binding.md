# 이벤트 바인딩이란?

- 이벤트 바인딩이란, 발생하는 이벤트와 그 후에 어떤 일이 벌어질지 알려주는 함수(콜백함수)와 묶어서 연결해 준다는 뜻이다.

- 대표적인 이벤트 바인딩 3가지 방식
  1. 어트리뷰트 방식
  2. 프로퍼티 방식
  3. Event Listener 방식

## 1. 어트리뷰트 방식(HTML 이벤트 핸들러)
- HTML 요소의 이벤트 Attribute에 이벤트 핸들러를 대응시키는 방법이다.

```js
<button onclick="clickBtn()">Click me</button>

function clickBtn() {
  alert('Button clicked!');
}
```
- 어트리뷰트 방식 내부의 this
  - HTML 이벤트 핸들러 방식의 경우, 이벤트 핸들러 내부의 this는 window를 가리킨다.
```js
<button onclick="clickBtn()">Click me</button>

function clickBtn() {
  alert('Button clicked!');
  console.log(this); // window
  console.log(event.currentTarget); // <button onclick="clickBtn()">Click me</button>
}
```

## 2. 프로퍼티 방식(전통적인 DOM 이벤트 핸들러)
- HTML과 Javascript가 혼용되는 문제는 해결
  1. 이벤트 핸들러에 하나의 함수만을 바인딩할 수 있다.
  2. 함수에 인수를 전달할 수 없다.
  3. 바인딩된 이벤트 핸들러가 2개 이상일 경우, 제일 마지막에 추가된 코드의 바인딩된 이벤트 핸들러만 실행된다.

```js
<button id="myBtn">Click me</button>

var myBtn = document.getElementById('myBtn');

// 첫번째 바인딩된 이벤트 핸들러 => 실행되지 않는다.
myBtn.onclick = function () {
  alert('Button clicked 1');
};

// 두번째 바인딩된 이벤트 핸들러
myBtn.onclick = function () {
  alert('Button clicked 2');
};
```

- 프로퍼티 방식 내부의 this
  - 전통적인 DOM 이벤트 핸들러 방식에서 이벤트 핸들러 내부의 this는 이벤트에 바인딩된 요소를 가리킨다. 이것은 이벤트 객체의 currentTarget 프로퍼티와 같다.

```js
<button id="myButton">Click me!!!</button>

var myBtn = document.getElementById('myButton');
myBtn.onclick = function() {
  console.log(this); // <button id="myButton">Click me!!!</button>
  console.log(event.currentTarget); // <button id="myButton">Click me!!!</button>
  console.log(this === event.currentTarget); // true
};
```

## 3. addEventListner 방식
- addEventListener 함수를 이용하여 대상 요소(Event Target)에 이벤트를 바인딩하고, 해당 이벤트가 발생했을 때 실행될 콜백 함수를 지정한다.

```js
target.addEventListener(type, listener[, useCapture]);

var el = document.getElementById("outside");
el.addEventListener("click", function(){modifyText("four")}, false);

```
- addEventListener 함수의 인수
  - type: 이벤트 타입
  - listener: 이벤트 핸들러, 즉 이벤트가 발생했을 때, 실행될 콜백함수
  - useCapture: true면 Capturing, false면 Bubbling(Default: false)

- addEventListner 방식의 장점
  - 하나의 이벤트에 대해 하나 이상의 핸들러를 추가할 수 있다.
  - 캡처링(Capturing)과 버블링(Bubbling)을 지원한다.
  - HTML 요소뿐만아니라 모든 DOM 요소에 대해 동작한다.

- addEventListner 방식 내부의 this
  - addEventListener 함수에서 지정한 이벤트 핸들러 내부의 this는 Event Listener에 바인딩된 요소(currentTarget)를 가리킨다. 이것은 이벤트 객체의 currentTarget 프로퍼티와 같다.

```js
<button id="myBtn">Click me!!!</button>

var myBtn = document.getElementById('myBtn');
myBtn.addEventListener('click', function (event) {
  console.log(this); // <button id="myBtn">Click me!!!</button>
  console.log(event.currentTarget); // <button id="myBtn">Click me!!!</button>
  console.log(this === event.currentTarget); // true
});
```

## 면접용 답변
1. 이벤트 바인딩이란 무엇인가요?
  - 이벤트 바인딩이란, 발생하는 이벤트와 그 후에 어떤 일이 벌어질지 알려주는 함수(콜백함수)와 묶어서 연결해 준다는 뜻입니다.

2. 이벤트 핸들러를 등록하는 방식에는 어떤 것이 있나요?
  - 어트리뷰트 방식, 프로퍼티 방식, addEventListner 방식이 있습니다.

출처: 
https://cheonmro.github.io/2018/09/03/event-binding-and-event-handler/
