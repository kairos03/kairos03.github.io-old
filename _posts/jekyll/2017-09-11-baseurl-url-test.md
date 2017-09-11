---
layout: post
title: site.baseurl and site.url test
subtitle: site.baseurl과 site.url의 github에서의 작동방식 테스트
date: 2017-09-11
categories: jekyll
cover:
tag:    [jekyll, baseurl, url, github]
---

jekyll 블로그를 github-page를 이용하여 구성할 때 baseurl과 관련된 문제를 격게 되어서
site.url, site.baseurl의 차이점을 살펴보고 문제의 해결 방법을 포스팅합니다.

# site.baseurl? site.url?
jekyll에서는 _config.yml 파일에 전역변수를 지정 할 수 있는데 그 중 URL과 관련되어 
가장많이 사용하는 것이 바로 site.url과 site.baseurl입니다. 
그럼 둘의 차이점은 무엇일까요? 

## site.baseurl

# Baseurl

site.baseurl  
{{ site.baseurl }}  

site.url  
{{ site.url }}  

site.base_path  
{{site.base_path}}