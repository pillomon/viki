---
title: Mar 05
draft: false
tags:
  - Docker
  - Shell
---
## 1. Docker 명령어
```bash
docker build --no-cache --progress=plain -t test-name .

docker run -p 5173:5173 test-name
```

Dockerfile을 사용해 이미지를 만들 때 주로 위 두개의 명령어를 사용했다. 

1) --no-cahce: Dockerfile이 변경되었을 때 cache 된 내용을 사용할까봐 추가하였고, 
2) --progress=plain: docker 이미지를 생성하는 과정을 상세하게 보고싶어 추가하였다.
