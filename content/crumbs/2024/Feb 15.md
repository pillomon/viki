---
title: Feb 15
draft: false
tags:
  - typescript
  - class
---
```typescript
class Human {
  #age = 10;

  getAge() {
    return this.#age; 
  }
}

class Person extends Human {
  #age = 20;

  getFakeAge() {
    return this.#age;
  }
}

const p = new Person();
console.log(p.getAge()); // 10
console.log(p.getFakeAge());  // 20
```

우리가 알고있던 this의 컨텍스트와는 다른 방식으로 동작한다. 각 클래스의 private 필드는 서로 다른 scope를 가진다. 위 예제는 private 필드가 class 기반 상속에서 어떻게 동작하는지 확인할 수 있다.