---
layout: post
title:  TeamCity로 Android Kotlin Project 관리하기
subtitle: Android CI/CD
date:   2017-09-12
categories: TeamCity
cover: https://user-images.githubusercontent.com/6357456/30398021-2063882a-98cf-11e7-89d4-88d5e65148e0.png
tag:    [TeamCity, CI, CD]
---

> 작성중인 글입니다.

# Teamcity?
Jetbrains 사에서 개발한 CI/CD 툴 이며 Free버전에서는 
최대 3개의 에이젼트를 갖을 수 있습니다.
다양한 언어로 개발된 프로젝트에 모두 사용할수 있으며
이 글에서는 Android 빌드 부분에 대해 자세히 다룰것 입니다. 

# JAVA 8설치
Oracle에서 직접 다운받거나
패키지 관리자에서 설치

# PATH 설정
JAVA_HOME
JRE_HOME 

# Team city 설치

** SDK  설치
** SDK_HOME

wget https://dl.google.com/android/android-sdk_r24.0.2-linux.tgz
export ANDROID_HOME=/opt/android-sdk-linux

sdk license agreement
android update sdk --no-ui --all --filter platform-tools,android-25,extra-android-m2repository
https://stackoverflow.com/questions/39760172/you-have-not-accepted-the-license-agreements-of-the-following-sdk-components

출처: http://slg1119.tistory.com/41 [Hello World!]


# ssh tunneling http

sudo ssh root@211.253.128.132 -p 22 -N -L 80:localhost:80