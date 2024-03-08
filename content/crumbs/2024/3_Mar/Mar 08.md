---
title: Mar 08
draft: false
tags:
---
## 1. Next.js 에서의 확장된 fetch?
- Next.js는 JS native fetch Web API를 확장하여, 사용자에게 서버에서의 각 요청에 대해 캐싱, 재검증이 가능하게하였다. React는 컴포넌트 트리를 렌더링하는 동안 자동으로 memoize fetch 요청으로 확장한다.
- 아래는 코드 예시
```typescript
const getData = async () => {
	const res = await fetch("https://jsonplaceholder.typicode.com/todos/1");
	
	if (!res.ok) {
		throw new Error("Failed to fetch data");
	}
	
	return res.json();
};

export default function Home() {
	const data = getData();
	return (<main></main>);
}
  
```
## 2. 캐시는 어떻게 사용하는가?
- fetch의 cache 기본값이 'force-cache' 설정으로 적용되어 있어, Next.js는 서버의 응답에 대해 캐싱을 진행한다.([Data Cache](https://nextjs.org/docs/app/building-your-application/caching#data-cache))
- 두가지 사항에 대해서는 cache를 진행하지 않는다.
  1) Server Action 내부에서 사용할 때.
  2) Route Handler 내부에서 POST methd로 통신할 때.
```typescript
// 기본 값
fetch('https://...', { cache: 'force-cache' })

// 예외 사항일 경우
fetch('https://...', { cache: 'no-store' })
```
## 3. Revalidation 활용방안
- 공식문서에서는 Time-based / On-demand Revalidation 두가지 방법을 제공한다.
  1) Time-based: 특정 시간 값을 부여해 해당 시간이 지나면 revalidate 하는 방법이다. 서버 측 데이터가 드물게 변경되거나 fresh하지 않은 데이터 사용에 문제 없을때 유용하다.
  2) On-Demand: event에 기반해 revalidate 하는 방법이다. tag-based, path-based로 revalidate된 데이터 그룹에 접근이 가능하다. 이 방법은 최신 데이터가 가능한 빨리 보여지게 하는 것을 보장해야할 때 유용하다. 
  ![On-Demand_Revalidation](https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fon-demand-revalidation.png&w=1920&q=75&dpl=dpl_AckYwQCkMMSRwCPNz5E7uA4TxZj7)
-   data를 revalidating하는 과정에서 에러가 발생하면, 마지막으로 성공한 데이터가 캐시로부터 제공된다. 그 다음 요청부터 revalidating을 다시 시도한다.
## 4. 정리
- 확장 fetch가 좋아져, 현재 진행중인 프로젝트를 app router로 이전하면 axios와 tanstack-query를 둘 다 사용하지 않는 방향으로도 생각해봐야 할 것 같다. 특히 caching을 해주는 기능은 정말 좋음.