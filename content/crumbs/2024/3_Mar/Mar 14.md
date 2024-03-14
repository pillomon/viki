---
title: Mar 14
draft: false
tags:
---
## 1. Conditional props
```tsx
enum Action {
	CLICK,
	MOUSE_ENTER	
}

interface CommonProps {
	children: React.ReactNode
}

type ConditionalProps =
	| {
		action: Action.CLICK;
		status?: never;
	}
	| {
		action: Action.MOUSE_ENTER;
		status: number;
    };
type Props = CommonProps & ConditionalProps  

function Comp = ({children, action, status}: Props) => {
	return (
		<div>
			{icon && icon}
			{action === Action.MOUSE_ENTER ? status : (<></>)}
			{children}
	    </div>
    );
};

export default Comp
```
- 조건부로 Props를 받아야할 때 위와같이 구현하여 해결이 가능하다. 위는 action이 MOUSE_ENTER일 경우에만 status를 받는다. 사용부에서는 props에 action 어떤 action이 들어가냐에 따라 에러가 발생한다.