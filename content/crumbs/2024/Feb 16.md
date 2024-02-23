---
title: Feb 16
draft: false
tags:
  - React
  - Jest
---
# Custom Hook 테스트 코드 작성

- 구현한 useQueryParams Hook은 NextJS의 useRouter를 사용하여 query parameter를 제어하는 로직을 추상화한 Hook이다.
- 예전에는 Custom Hook 테스트에 테스트용 컴포넌트를 별도로 만들어서 render를 통한 테스트를 진행하였던 것 같다. 현재는 @testing-library/react의 reactHook을 사용해서 테스트를 진행한다.
```typescript
	jest.mock('next/router', () => ({  
	  useRouter: jest.fn(),  
	}));
	
	/** 테스트 코드 내부 **/
	it(`getAllQueryParams - Query does exist.`, () => {  
	  const queryObj = { a: 'a', b: 'b' };  
	  const { useRouter } = requireMock('next/router');  
	  useRouter.mockImplementationOnce(() => ({  
	    query: queryObj,  
	  }));  
	  const {  
	    result: {  
	      current: { getAllQueryParams },  
	    },  
	  } = renderHook(useQueryParams);  
	  
	  expect(getAllQueryParams()).toEqual(queryObj);  
	});
	it('deleteQueryParams - Key in object.', () => {  
	  const pushFn = jest.fn();  
	  const queryKey = 'a';  
	  const queryObj = { a: 'a', b: 'b', c: 'c' };  
	  const { useRouter } = requireMock('next/router');  
	  useRouter.mockImplementationOnce(() => ({  
	    query: queryObj,  
	    push: pushFn,  
	  }));  
	  const {  
	    result: {  
	      current: { deleteQueryParams },  
	    },  
	  } = renderHook(useQueryParams);  
	  
	  act(() => deleteQueryParams(queryKey));  
	  expect(pushFn).toHaveBeenCalled();  
	});
```
- mocking은 각 테스트 코드 상단에 위치하거나 전역으로 사용할 경우 jest.setup.js 내부에 작성한다.
- mockImplementation을 초기에 jest.mock 할 때 지정할 수도 있으나 테스트 할 때 알맞는 data를 제공하기 위해서 각 테스트 코드 내에서 mockImplementation을 사용했다. 이렇게 사용하면 유동성있게 테스트코드 작성이 가능하다. 또한 객체 변수일 경우 참조문제로 정확한 테스트가 되지 않는 것으로 보여져 케이스마다 따로 주입을 했다.
- React Dom에서 act()를 제공해주지만 부가적인 코드가 길어질 때가 있다고 공식문서에 나와 있어 공식문서에서도 rtl의 act를 사용하는것을 권장한다. act()는 함수를 인자로 받아 가상의 DOM에 적용하는 역할을 한다. act() 함수가 호출된 뒤, DOM에 반영되었다고 가정하고 그 다음 코드를 사용할 수 있어 React가 브라우저에서 실행될 떄와 최대한 비슷한 환경에서 테스트 가능하다.