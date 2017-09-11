---
layout: post
title: Jekyll로 Github Page 블로그 만들기
subtitle: 지킬과 깃허브 페이지를 이용한 빠른 시작 설명서
date: 2017-06-24
categories: jekyll
cover: https://user-images.githubusercontent.com/6357456/30242776-f4414866-959c-11e7-9bc1-d4d118863b1c.png
tag: [jekyll, github-pages, blog]
---    

# Jekyll이란?
`심플함, 정적, 블로그 지향적`  
간단히 말해 개인 블로거에게 아주 알맞는 프레임워크!

저도 간단한 블로그가 필요해서 사용해보기로 했습니다.  
깃허브에서 호스팅도 공짜니 편리함과 함께 1석2조!

자세한 설명은 [링크](https://jekyllrb.com/) 참조!

# Github page란?
github에서 제공하는 무료 홈페이지 호스팅으로 자신의 이름, 단체명, 프로젝트 명으로 생성할수 있습니다.

간단하게 `[username].github.io` 라는 repo를 만들면
몇분뒤 호스팅이 자동으로 완료 됩니다!

자세한 설명은 [링크](https://pages.github.com/) 참조!

# How To Work?

## github repo 만들기
1. github에 `[username].github.io` repo 만들기     
    ![make_repo](https://user-images.githubusercontent.com/6357456/27508224-799bfbb4-591b-11e7-8099-f5cf172f32d7.png)  
2. repo clone  
repo를 clone 할 폴더로 이동 후 clone, 샘플 페이지를 만듭니다.  
```bash
$ git clone [username].github.io
$ cd username.github,io
$ echo "Hello Git page!" > index.html
```
3. github에 push!
```bash
$ git add .
$ git commit -m "init commit"
$ git push -u origin remote
```
이렇게 하면 repo가 만들어지고 몇 시간 후에 자신의 이름으로 된 페이지가 호스팅됩니다.  
그럼 페이지가 호스팅 되기전 까지 Jekyll을 세팅해봅시다!

## Jekyll Install
Linux와 Windows 두가지 플랫폼에서 설치를 설명합니다.
각각 설치법은 아래 공식페이지를 참조했습니다. 
1. [Jekyll Linux](https://jekyllrb.com/docs/installation/)
2. [Jekyll Windows](https://jekyllrb.com/docs/windows/) 

### Linux
> Ubuntu 16.04.3을 기준으로 설명합니다.

추가예정

#### Window
> Windows 10을 기준으로 설명합니다.

1. Bash on Ubuntu on Windows를 활성화 시킵니다.  
다음 ms의 공식 [Bash on Ubuntu on Windows](https://msdn.microsoft.com/ko-kr/commandline/wsl/about)을 참고합니다.  
다음 명령어를 통해 bash 쉘을 실행시킬 수 있습니다.
```bash
bash    # 시작 메뉴에서 검색가능 
```
또는 다음과 같은 아이콘을 클릭해 실행 할 수 있습니다.  
![bash on ubuntu on windows](https://user-images.githubusercontent.com/6357456/27509887-7d822f48-5941-11e7-988a-4e25138992aa.png)

2. 기존의 repo list와 패키지를 업데이트 및 업그래이드 합니다.
```bash
$ sudo apt update -y && sudo apt upgrade -y
```
3. 이제 ruby를 설치합니다. 설치를 위해 [BrightBox](https://www.brightbox.com/docs/ruby/ubuntu/) repo를 추가합니다.
```bash
$ sudo apt-add-repository ppa:brightbox/ruby-ng
$ sudo apt-get update
$ sudo apt-get install ruby2.3 ruby2.3-dev build-essential
```
4. ruby gem을 업데이트 합니다.
```bash
$ sudo gem update
```
5. 마지막으로 Jekyll을 설치합니다.
```bash
$ sudo gem install jekyll bundler
```
6. 버전 체크
```bash
$ jekyll -v
```
설치 완료!

# 테스트 블로그 만들고 로컬에서 돌려보기
실제 github page에 올리기 전에 간단히 구조를 살펴보기위해 테스트 블로그를 만들어보겠습니다!

새 블로그 이름을 `my_blog`로 만듭니다.
```bash
$ jekyll new my_blog
```
`_posts`폴더안에 파일 이름을 확인해 현재 시간이 잘 적용됬는지 확인합니다.
> ***NOTE.*** Bash on Ubuntu on Windows는 현재 개발중임으로 실행중에 몇몇 이슈들이 발생 할 수 있습니다.

로컬 Jekyll 서버를 만듭니다.
```bash
$ jekyll serve
```
`http://localhost:4000/`를 브라우저 주소창에입력 하여 자신의 싸이트를 확인 해 볼 수 있습니다.  
> ***NOTE.*** 2.4 버전 부터 자동으로 변경사항을 감지하여 빌드를 다시 해 줍니다.  
> ***NOTE.*** 더 자세한 명령은 [다음](http://jekyllrb-ko.github.io/docs/usage/)을 참고

## Github Page에 올리기!

[github repo 만들기](#github repo 만들기), [Jekyll Install](#Jekyll Install)에서 만들었던 폴더들은 지운다.

테스트 블로그를 만들 었을 때 처럼 하되, 프로젝트 이름은 [username].github.io로 만듭니다.
```bash
$ jekyll new [username].github.io
```
기본 템플릿은 만들어 졌으니 github에 올립니다.  
[username].github.io 폴더에서 진행합니다.  
```bash
$ git init
$ git remote add origin [github repo url]
$ git add .
$ git commit -m "first jekyll commit"
$ git push
```
remote repo url은 git@github.com:[username]/[username].github.io.git 이렇게 되어 있습니다.

이제 `http://[username].github.io/`에 들어가보면!
![init_blog](https://user-images.githubusercontent.com/6357456/27743476-1a153f3a-5df7-11e7-8df7-2c46c5a3475d.png)  
이렇게 잘 호스팅이 되어있습니다. (저는 조금 고쳐서 제목이나 footer부분이 다르게 보입니다. ^^)

## 포스팅하기
`jekyll new`를 하면 폴더에 `_posts` 폴더가 생성됩니다.
`_posts`폴더 안에 `[yyyy-mm-dd]-title.md`로 파일을 생성하면 자동으로 포스팅이 됩니다!

---
위 포스트는 지킬의 공식 
[포스트](http://http://jekyllrb-ko.github.io/docs/home/)를 참고하여 작성하였습니다.