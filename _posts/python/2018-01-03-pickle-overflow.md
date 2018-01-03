---
layout: post
title: "Pickle Overflow"
subtitle: Overflow 해결하기
date: 2018-01-03
categories: python
cover: https://user-images.githubusercontent.com/6357456/34518014-a9a1dcc6-f07d-11e7-8055-7f0ec0d0e3ab.png
tag:    [python, pickle]
---

Pickle로 4GB 이상의 대용량 파일을 저장하려고 하면 다음과 같은 에러가 발생한다.

> OverflowError: cannot serialize a bytes object larger than 4 GiB

다음과 같이 `protocol` 옵션을 사용하여 해결 할 수 있다.

```python
pickle.dump(d, open("file", 'w'), protocol=4)
```

참고자료
[[https://docs.python.org/3/library/pickle.html]]