---
title: Dec 08
draft: false
tags:
  - React
---
# React 공식문서에서 제공하는 9가지 권장사항

1. JSX 내의 반복문에서 key 내에 array index 사용 지양하기.
2. 컴포넌트를 정의할 때 다른 컴포넌트 or 함수안에 중첩하지 않고 파일/모듈 최상위 레벨에 정의하기.
3. 상태에 무엇을 저장할지 결정할 때는 필요한 것을 계산하는데 사용할 수 있는 최소한의 정보를 저장하기
```tsx
// 🛑 잘못된 코드
const [allItems, setAllItems] = useState([]);
const [urgentItems, setUrgentItems] = useState([]);

function handleSomeEvent(newItems) {
  setAllItems(newItems);
  setUrgentItems(newItems.filter((item) => item.priority === "urgent"));
}

// 🟢 올바른 코드
const [allItems, setAllItems] = useState([]);
const urgentItems = allItems.filter((item) => item.priority === "urgent");

function handleSomeEvent(newItems) {
  setAllItems(newItems);
}
```
4. useMemo, useCallback, React.memo를 사용하여 캐싱 적용 여부를 고려한다면 성능 문제가 발견될때까지 캐싱을 미루기.
5. 공통된 코드를 함수로 추출할 때, 다른 hook을 호출하는 경우에만 hook으로 지정하기.
6. props 변경에 따라 상태를 조정해야 하는 경우 useEffect(렌더링 이후)가 아닌 컴포넌트 함수에(렌더링 중에) 직접 상태를 설정하자.
7. data를 fetching 할 때, useEffect보다 라이브러리를 사용하기.
8. 이벤트 발생에 대한 응답으로 어떠한 액션을 처리해야 할 때, useEffect가 아닌 이벤트 핸들러 내에 코드를 작성하자.
9. When a useEffect dependency is causing re-renders you don’t want (including infinite loops), don’t just remove the dependency from the array: remove the dependency from the effect function too.

## References
[The nine best recommendations in the new React docs](https://blog.testdouble.com/posts/2023-10-16-react-docs-recommendations/#9-when-a-useeffect-dependency-is-causing-rerenders-you-dont-want-including-infinite-loops-dont-just-remove-the-dependency-from-the-array-remove-the-dependency-from-the-effect-function-too)
