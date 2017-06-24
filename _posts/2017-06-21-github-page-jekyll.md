---
layout: post
title: "Jekyll로 Github Page 블로그 만들기"
date: 2017-06-24 09:53:12 +0900
categories: jekyll update
---
## Jekyll이란?

`심플함, 정적, 블로그 지향적`  
간단히 말해 개인 블로거에게 아주 알맞는 프레임워크!

필자도 간단한 본인의 블로그가 필요해서 사용해보기로 했다.  
깃허브에서 호스팅도 공짜니 편리함과 함께 1석2조!

자세한 설명은 [링크](https://jekyllrb.com/) 참조!

## Github page란?
github에서 제공하는 무료 홈페이지 호스팅으로 자신의 이름, 단체명, 프로젝트 명으로 생성할수 있다.

간단하게 `[username].github.io` 라는 repo를 만들면
몇분뒤 호스팅이 자동으로 완료 된다!

자세한 설명은 [링크](https://pages.github.com/) 참조!

## How To Work?

### github repo 만들기
1. github에 `[username].github.io` repo 만들기     
![make_repo](https://user-images.githubusercontent.com/6357456/27508224-799bfbb4-591b-11e7-8099-f5cf172f32d7.png)
필자는 이미 만들어서 에러가 뜬다;; 원래는 잘되니 넘어가주세요~  

2. repo clone  
repo를 클론 할 폴더로 이동 후 clone, 샘플 페이지를 만든다.  

        $ git clone [username].github.io
        $ cd username.github,io
        $ echo "Hello Git page!" > index.html
3. github에 push!
        $ git add .
        $ git commit -m "init commit"
        $ git push -u origin remote
이렇게 하면 repo가 만들어지고 몇 시간 후에 자신의 이름으로 된 페이지가 호스팅된다.  
그럼 페이지가 호스팅 되기전 까지 Jekyll을 세팅해보자!

### Jekyll setting

# 추가예정
