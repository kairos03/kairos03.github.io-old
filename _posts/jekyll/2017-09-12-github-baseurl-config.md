---
layout: post
title: "GitHub에서 baseurl 설정 오류" 
subtitle: "baseurl: / 설정 오류"
date: 2017-09-11
categories: jekyll
cover:
tag:    [jekyll, baseurl, url, github, github-pages]
---

H2O테마를 적용하는데 resource를 불러오는데 문제가 생겨서 여러번의 
삽질끝에 알아낸 해결법을 알려드립니다.

# GitHub baseurl bug(?) 
만약 `_config.yml`파일에 `baseurl: /`이렇게 지정을 하셨다면
Jekyll 3.5.2 (현제)에서는 `/`를 리턴하여 올바르게 작동을 하지만
깃허브에서는 아무것도 리턴하지 않습니다. 

깃허브에서 `baseurl: /`일때는 `baseurl: `으로 `_config.yml`을 덮어쓰는것으로 보입니다.[^1]
 

# 해결법
`baseurl: /`로 설정하고 다음과 같이 리소스에 접근하신다면 
```yml
{%raw%}{{ res/img/jekyll.png | prepend: site.baseurl }}{%endraw%}
```
로컬에서는 잘 동작하지만 GitHub-Pages에서는 동작하지 않는것을 보실수 있습니다.

따라서 다음과 같이 `baseurl: `로 설정하고 리소스 접근은
```yml
{%raw%}{{ /res/img/jekyll.png | prepend: site.baseurl }}{%endraw%}
```
로 하시면 정상으로 작동합니다.

---
[^1]: <https://github.com/github/pages-gem/issues/350> 