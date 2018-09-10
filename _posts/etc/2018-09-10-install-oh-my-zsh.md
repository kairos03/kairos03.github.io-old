---
layout: post
title: "Ubuntu 18.04에서 oh-my-zsh 설치하기" 
subtitle: "install oh-my-zsh with ubuntu 18.04"
date: 2018-09-10
categories: 
cover: 
tag: []
---

# Oh My Zsh 설치
리눅스에서 기본으로 제공되는 bash도 사용하기 좋지만 저는 git과의 연동이나 대소문자 구분 등의 다양한 편의 기능이 있는 zsh을 주로 사용합니다.  
이 글에서는 ubuntu 18.04에서 oh-my-zsh을 설치하는 방법을 알아봅니다.  
Oh-My-Zsh페이지를 참고하였습니다.  
https://github.com/robbyrussell/oh-my-zsh

# 시스템 기본 프로그램 설치
## zsh
Oh-my-zsh을 설치하기에 앞서 기본이 되는 zsh을 설치합니다.

```sh
$ sudo apt install -y zsh
```

## wget or curl
자동 설치 스크립트를 받기 위해서는 둘 중 하나가 필요합니다.  
18.04에는 wget이 설치 되어있기 때문에 curl을 설치 하지 않아도 됩니다.

```sh
$ sudo apt install -y wget
or
$ sudo apt install -y curl
```

## git
소스코드가 github에서 관리 되기 때문에 설치시에 git이 필요합니다.

```sh
$ sudo apt install -y git
```

## 참고
다음과 같이 한꺼번에 설치 할 수 있습니다.

```sh
$ sudo apt install -y zsh wget curl git
```

# 설치
이제 Oh-My-Zsh을 설치합니다.

#### via curl

둘 중 하나를 선택하여 설치 합니다.

```shell
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

#### via wget

```shell
$ sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

설치가 완료되었습니다!

# 테마 수정
기본 테마도 좋지만 저는 'agnoster'테마를 사용합니다.
다음 `~/.zshrc`파일을 수정하여 테마를 변경합니다.

11번째 줄에 `ZSH_THEME=****`을 다음과 같이 수정합니다.
```
ZSH_THEME="agnoster"
```
다시 터미널을 열면 변경된 테마가 보입니다.

# 폰트 설치
기본적으로 `agnoster`에서 사용하는 폰트가 설치되어 있지 않아 화면에 깨져서 나타납니다. 
`power-line`폰트를 설치 해줍니다.

```
$ sudo apt install fonts-powerline
```
테마적용까지 완료되었습니다.!
