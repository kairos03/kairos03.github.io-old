---
layout: post
title:  Jekyll 머리말
subtitle: 지킬 포스트의 시작을 알리는 머리말의 속성들
date: 2017-09-07
categories: jekyll
cover:
tag:    [jekyll, 머리말, pre-defined, user-defined]
author-name: kairos
---

# Jekyll 머리말
Jekyll의 멋있는 기능 중 가장 첫번째는 머리말 기능입니다.  
머리말은 YAML 타입으로 처리되며 다양한 옵션을 가지고 있으며 다음과 같은 형태로  
포스트의 최상단부분에 위치하게 됩니다.  

```yaml
---
layout: post
title: jekyll Frontmatter
---
```
  
# pre-defined global variables (사전-정의 전역 변수)
jekyll의 header에는 페이지 또는 머릿말에 사용할 수 있는 다양한
사전-정의 전역 변수들이 존재합니다.

## layout
사용할 레이아웃 파일을 지정합니다. 
레이아웃 파일명에서 확장자를 제외한 나머지 부분만 입력합니다. 
레이아웃 파일은 **반드시 _layouts** 디렉토리에 존재해야 합니다:  

```yaml
---
layout: post
---
```

## permalink
생성된 블로그 포스트 URL을 사이트 전역 스타일 
(디폴트 설정: `/year/month/day/title.html`)이 아닌 다른 스타일로 만드려면, 
이 변수를 사용하여 최종 URL 을 설정하면 됩니다. 

다음 [사이트](http://jekyllrb-ko.github.io/docs/permalinks/)에서 고유주소 변수와 예시가 설명되어 있습니다:

```yaml
---
permalink: /:short_year-:month-:day/:title.html     #/17-09-07/title.html
permalink: /:categories/:year-:month-:day/:title    #/jekyll/2017-09-07/title
---
```

## published
사이트가 생성되었을 때 특정 포스트가 나타나지 않게 하려면 false 로 설정하면 됩니다.
default 값은 true 입니다:
 
```yaml
---
published: false
---
```

## category / categories
포스트를 특정 폴더에 넣지 않고, 포스트가 속해야 하는 카테고리를 하나 또는 그 이상 지정할 수 있습니다. 
사이트가 생성될 때, 포스트는 그냥 평범하게 이 카테고리들에 속한 것처럼 생성됩니다. 
두 개 이상의 카테고리들을 지정할 때에는 YAML 리스트 또는 쉼표로 구분된 문자열을 사용합니다:

```yaml
---
category: jekyll
categories: [jekyll, dev, kiros]
---
``` 

## tag
카테고리와 유사하게, 하나 이상의 태그를 포스트에 추가할 수 있습니다. 
또 카테고리와 동일하게, YAML 리스트 또는 쉼표로 구분된 문자열로 여러개를 지정할 수도 있습니다:

```yaml
---
tag: jekyll
tag: [jekyll, kiros, tag, exmple]
---
```

# user-defined variable (사용자-정의 변수)
변환 작업 중, 사전-정의된 변수가 아닌 모든 머리말 변수들은 데이터로 정리되어 Liquid 템플릿 엔진에 전달됩니다. 
예를 들어, title 변수를 설정하면 레이아웃에서 페이지 제목을 입력할 때 사용할 수 있습니다:

```html
<!DOCTYPE HTML>
<html>
  <head>
    <title> {% raw %}{{ page.title }}{% endraw %} </title>
  </head>
  <body>
    ...
```

다음과 같이 헤더를 정의 한다면,
```yaml
---
author-name: kairos
---
```
본문에 다음과 같이 사용할수 있습니다.
```html
...
<p> 
    {% raw %}작성자: {{page.author-name}}{% endraw %}      # 작성자: kairos
</p>
...
```

# Post's pre-defined variable (포스트의 사전-정의 변수)
포스트에서 사용할 수 있는 특별한 머릿말 변수입니다.

## date
여기에 지정한 날짜가 포스트 파일 이름에 있는 날짜보다 더 우선순위가 높습니다. 
포스트를 올바르게 정렬하기 위해 사용할 수 있는 기능입니다. 
날짜 형식은 `YYYY-MM-DD HH:MM:SS +/-TTTT` 입니다.
시간, 분, 초와 타임존 오프셋(`HH:MM:SS +/-TTTT`)은 선택사항입니다.

```yaml
---
date: 2017-09-07 17:32:09 +0100
date: 2017-09-07
---
```

<br>
> 자주 사용하는 머리말 변수를 계속 반복해서 입력하고 싶지 않으면, 해당 변수에 대한 
[기본값](http://jekyllrb-ko.github.io/docs/configuration/#front-matter-defaults)을 정의하고, 
필요한 곳에서만 다른 값으로 덮어쓰세요. 사전-정의 변수와 사용자-정의 변수 
모두 이 방법을 적용할 수 있습니다. 

---
위 포스트는 지킬의 공식 
[포스트](http://jekyllrb-ko.github.io/docs/frontmatter/)를 참고하여 작성하였습니다.
