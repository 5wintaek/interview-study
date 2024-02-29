## useEffect란 무엇인가요?

> useEffect는 side effect를 수행하기 위한 Hook이다.

컴포넌트가 맨 처음 화면에 렌더링 될 때, useEffect 함수가 포함된 컴포넌트가 다시 리렌더링 될 때 실행됩니다. 의존성 배열 안에 item 값이 바뀔 때 실행되고, 빈 배열이라면 화면에 첫 렌더링 될 떄 실행이 됩니다.

## (꼬리질문) dependency array란 무엇인가요?

> 이 배열은 특정 값들을 감시하고 해당 값이 변경될 때에만 실행되도록 하는 역할을 합니다.

## (꼬리질문) useEffect의 dependency가 비어있을 경우 어떤 현상이 발생할 수 있나요?

> 상태가 변경될 떄마다 컴포넌트가 리렌더링이 됩니다.

## (꼬리질문) useEffect의 dependency가 빈 배열이 전달된다면 어떻게 되나요 ?

> 빈 배열이 전달된다면 첫 렌더링 될때만 실행이 됩니다.

## (꼬리질문) side Effect란 무엇인가요 ?

> side effect는 함수가 실행되면서 함수 외부에 존재하는 값이나 상태를 변경시키는 등의 행위를 말한다

---

## 설명 자료

[노션자료정리](https://potent-havarti-4b7.notion.site/useEffect-aa186814dd0e4ae199b2f342cbefdb08?pvs=4)

[useEffect 마스터하기 - 별코딩](https://www.youtube.com/watch?v=kyodvzc5GHU)

[useEffect를 사용하여 업데이트 할 작업 설정하기](https://react.vlpt.us/basic/16-useEffect.html)

