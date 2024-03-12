---
title: Mar 12
draft: false
tags:
  - Zustand
  - NextJS
---
## 1. Zustand with Next.js
- Zustand의 개발자는 Next.js에서의 몇 가지의 challenge를 정의해두었다.
1) **Per-request store**
   : Next.js 서버는 동시에 다수의 요청을 처리한다. 이것은 request당 store가 생성되어야 하고, request 간에 상태가 공유되지 않아야 함을 의미한다.
2) **SSR friendly**
   : Next.js 앱은 두번 렌더링 된다.(server & client) server와 client가 각각 다른 결과를 return할 경우 "hydration erros."가 발생한다. 에러를 방지하기 위해 store는 서버에서 초기화 되고 클라이언트에서 다시 초기화 되어야한다.([SSR and Hydration](https://docs.pmnd.rs/zustand/guides/ssr-and-hydration))
3) **SPA routing friendly**
   : Next.js는 client side routing을 위해 하이브리드 모델을 지원한다, 이것은 store를 초기화하기 위해서는 컴포넌트 레벨에서 Context를 사용해야 함을 말한다.
4) **Server caching friendly**
   : 최신 버전의 Nexjt.js는 적극적으로 서버 캐싱을 지원한다. store가 module state(?)로 존재하기 위해서는 캐시와 완벽히 호환되어야한다.

- Zustand의 개발자는Zustand를 적절히 사용하기 위해 일반적으로 아래와 같은 것들을 권고한다.
1) **No global stores**
   : 이유는 store의 state가 request간에 공유되면 안되기 때문이다. 전역 store로 정의하지 않는 대신, store는 request당 생성된다.
2) **React Server Components should not read from or write to the store**
   : RSCs는 hook 또는 context를 사용할 수 없다.(statful하지 않기 때문에) RSC에서 전역 store의 state를 읽거나 쓸경우 이것은 Next.js의 아키텍처를 위반하는 일이다.