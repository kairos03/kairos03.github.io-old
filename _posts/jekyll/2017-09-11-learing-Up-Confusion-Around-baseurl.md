---
layout: post
title: "baseurl과 관련된 헷갈리는 것들 정리"
subtitle: "Clearing Up Confusion Around baseurl -- Again : 번역"
date: 2017-09-11
categories: jekyll
cover:
tag:    [jekyll, baseurl, url, github]
---

jekyll 블로그를 github-page를 이용하여 구성할 때 baseurl과 관련된 문제를 격게 되어서
baseurl에 대해 깔끔하게 정리된 
[글](https://byparker.com/blog/2014/clearing-up-confusion-around-baseurl/)을 번역해 보았습니다.

**요약: 도메인의 최상위(root)에 있지 않은 사이트를 만드려면 baseurl을 사용하세요.**

Jekyll의 설정 중 하나인 `baseurl`은 몇가지 헷갈리는 부분이 있습니다. 오픈소스의 아름다움 중 하나인
또한 Jekyll에도 해당 되는 그것은 많은 유연성이 있다는 것입니다. 
하지만 불행히도 `baseurl`은 이러한 유연성과는 거리가 멉니다. 
여기에 그 사용의도를 빠르게 파악하고 사용하는 방법을 설명합니다.

# GitHub Pages 모방하기
`baseurl`은 [2010년에 최초로 추가](https://github.com/jekyll/jekyll/commit/4a8fc1fa6e3fa5dc05c81ac5ac4ffed0b0818ac4)되었는데
그 의도는 다음과 같습니다. 
"내부 웹서버에서 테스트하는 유저가 프로덕션에 배포될 때와 같은 baseurl을 가지고 테스트 할 수 있도록 하기 위해"[^1]

# 예제
![baseurl예제](https://user-images.githubusercontent.com/6357456/30297748-22b5eb18-9749-11e7-9a62-db0778d709d6.png)

자 이제 아주 멋진 새로운 프로젝트를 시작한다고 가정해 봅시다. 나는 그 프로젝트의 
문서를 "example"이라는 곳에 만들고 싶고, "@jekyll"이라는 이름으로 
GitHub-Page에 배포 하고 싶습니다. 이 문서는 다음 URL로 접근이 가능할 것입니다.
`http://jekyll.github.io/example`  
  
이 예제에서 "base URL"은 `/example`을 뜻합니다. 또한 그것은 `_config.yml`
파일에 다음과 같이 정의되어 있습니다.
```yml
baseurl: /example
```

제가 웹사이트를 개발하려 할 때, 보통 `jekyll serve`를 실행시킵니다. 그러나 이번에는 
`http://localhost:4000/example/`로 들어갑니다.
`baseurl`이 하는 일은 도메인과 함께 베이스 패스를 특정하는 것입니다. 
만약 단지 `http://localhost:4000/`으로 접근을 시도했다면 
당신은 곧 에러메시지를 볼 수 있을 것입니다.
모든 링크를 올바르게 연결했다면 경로의 시작 부분에 `/example`과 같은 URL이 표시되지 않습니다.
  
다음과 같이 설정할 수 있을 것입니다.
```yml
{% raw %}{{ page.path | prepend: site.baseurl }}{% endraw %}
```

# 사이트를 올바르게 설정하기
1. `_config.yml`파일에 `baseurl`을 설정합니다. 
단, host는 제외해야 합니다. (e.g. `/example`이 맞고, `http://jekyll.github.io/example`은 틀린설정입니다.)
2. `jekyll serve`를 실행하고, `http://localhost:4000/your_baseurl/`에서 `your_baseurl`을 
`_config.yml`에서 설정한 `baseurl`로 바꾼주소로 들어가세요. 주소 맨마지막에 `/`를 써 넣는것을 잊지 마세요.
3. 모든것이 작동하는지 확인하세요. 언제든지 url에 `site.baseurl`을 prepend하세요.
4. host에 push하고 그 곳에서도 모든것이 작동하는지 확인하세요!

# 그럼 도데체 site.url은 무엇일까?
site.url은 site.baseurl과 함께 전체 URL이 포함 된 링크를 원할 때 사용됩니다. 
일반적인 패러다임 중의 하나 입니다.

# 성공!
해냈습니다! 성공적으로 `baseurl`을 쓸 수 있게되었고 세상은 아름답습니다.

---

[^1]: <https://github.com/jekyll/jekyll/pull/235>