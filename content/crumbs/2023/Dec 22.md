---
title: Dec 22
draft: false
tags:
  - useTransition
  - React
---
# useTransition 이란?

*useTransitiondms UI를 차단하지 않고 상태를 업데이트할 수 있는 React Hook*

## 내가 사용하게된 이유?
- tanstack-query의 useSuspenseQuery를 사용해서 작업을 Suspense, ErrorBoundary 작업을 하고 있었음.
- 디자이너의 요청에 의해 초기 데이터 fetching만 Suspense의 Fallback UI를 노출시켜주고 이후에는 이전데이터를 유지하면서 Fallback UI 노출은 하지 말아 달라는 요청이 있었음.
- Table UI였는데 Select UI 및 Pagination UI에 따라서 Table body의 내용이 변경됨.
- Pagination, Select의 handler를 useTransition의 startTransition의 wrapping하여 사용하였음.
- 그렇게 되면 우선순위가 낮은 상태 업데이트가되어 UI를 Blocking하지 않고 자연스럽게 동작이 가능하다.

```tsx
const [isPending, startTransition] = useTransition();

const handleSelectTransition = (  
  type: 'tableRowCnt' | 'mediaGroupStatus',  
  opt: SelectOption,  
) => {  
  switch (type) {  
    case 'tableRowCnt': {  
      startTransition(() => {  
	    // 원본 handler
        handleTableRowCntChange(opt);  
      });  
      break;  
    }  
    case 'mediaGroupStatus': {  
      startTransition(() => {
	    // 원본 handler
        handleMediaGroupStatusChange(opt);  
      });  
      break;  
    }  
    default: {  
      break;  
    }  
  }  
};

...

<Select options={options} onChange={(opt) => handleTableRowCntChange('tableRowCnt', opt)} />
<Select options={options} onChange={(opt) => handleTableRowCntChange('mediaGroupStatus', opt)} />

```

- React API로 startTransition을 제공하는 것 같다. isPending이 필요없으면 API를 사용하는 것이 좋아보인다.