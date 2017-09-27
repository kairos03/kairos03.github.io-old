---
layout: post
title:  jCenter에 Android 라이브러리 쉽고 빠르게 배포하기
subtitle: Android library deploy to jCenter
date:   2017-09-27
categories: android
cover:  https://user-images.githubusercontent.com/6357456/30913297-002b7bc8-a390-11e7-9421-7c60bb340bf6.png
tag:    [andorid, jcenter, maven, kotlin, Gradle]
---

Gradle 과 bintray-release 를 사용하여 쉽고빠르게 안드로이드 라이브러리를 배포해 보자!

# jCenter에 Library 배포
1. jCenter를 운영하는 [bintray](https://bintray.com/)에 회원가입을 합니다.
깃허브 계정으로 빠르게 가입할수 있습니다.

2. 개인 페이에서 할수도 있고 단체를 만들수도 있습니다. (option)
저는 그룹 프로젝트라 그룹을 만들어서 진행했습니다.<br>
![Group page](https://user-images.githubusercontent.com/6357456/30902691-79713db6-a36c-11e7-9d7a-2d6eb961724c.png)

3. 다양한 리포지토리로 배포 할수 있는데 여기서는 Maven 을 사용할 것입니다.
왼쪽아래 Repository 추가를 눌러 Maven repo를 생성합니다.<br>
![repo create](https://user-images.githubusercontent.com/6357456/30902794-dc591124-a36c-11e7-8a35-199d4631a76d.png)
사이트 설정은 끝났습니다. 안드로이드 Gradle로 넘어옵니다.

4. [novoda:bintray-release](https://github.com/novoda/bintray-release) 를 사용하여 빌드후 자동 배포되도록 설정하겠습니다.
5. 배포하고자 하는 라이브러리 `project`의 `build.gradle`의 dependencies, classpath에 다음을 추가합니다
    ```
    buildscript {
        ext.kotlin_version = '1.1.4'

        repositories {
            jcenter()
        }
        dependencies {
            classpath 'com.android.tools.build:gradle:2.3.3'
            classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
            classpath 'com.novoda:bintray-release:0.3.4'    // 최신버전을 확인하세요!

            // NOTE: Do not place your application dependencies here; they belong
            // in the individual module build.gradle files
        }
    }
    ```
    ![image](https://user-images.githubusercontent.com/6357456/30903128-fa9478da-a36d-11e7-95ae-c92b8119f197.png)
    > project 와 Module 의 build.gradle 를 잘보고 추가하시기 바랍니다.
6. 라이브러리의 모듈의 build.gradle 에 사용자 정보와 라이브러리 설명을 추가합니다. <br>
    ```
    apply plugin: 'com.android.library'
    apply plugin: 'kotlin-android'
    apply plugin: 'kotlin-android-extensions'
    apply plugin: 'com.novoda.bintray-release'

    publish {
        userOrg = 'dcom'                // repo user/organization
        groupId = 'com.d.campaign'      // group ID
        artifactId = 'campaign-adviser' // artifact ID, don't use space
        publishVersion = '0.3'          // version
        desc = 'Campaign Adviser'       // description
        website = ''                    // website
        issueTracker = "https://github.com/GamePlatform/Campaign-SDK-Android/issues"    // git issue tracker
        repository = "https://github.com/GamePlatform/Campaign-SDK-Android.git"         // git repo
    }
    ```
    ![image](https://user-images.githubusercontent.com/6357456/30903611-a3c49f4c-a36f-11e7-99c1-060598785118.png)
    > GroupId, artifactId의 컨벤션을 꼭 지키시기 바랍니다.

7. 터미널에서 아래 명령으로 빌드합니다.
    ```bash
    $ ./gradlew clean build bintrayUpload -PbintrayUser=BINTRAY_USERNAME -PbintrayKey=BINTRAY_KEY -PdryRun=false
    ```
    BINTRAY_USERNAME은 개인 사용자 아이디입니다. (그룹이나 단체명이 아닙니다)<br>
    BINTRAY_KEY는 개인 사용자의 API KEY입니다. Profile > Edit > API Key에서 확인 할수 있습니다.

    [kotlin javadoc 빌드 에러 해결하기](#kotlin javadoc 빌드 에러 해결하기)

8. 빌드가 완료되면 자동으로 bin-tray 에 업로드 됩니다.<br>
maven repo에서 확인 가능합니다.<br>
![image](https://user-images.githubusercontent.com/6357456/30904088-24f4f9bc-a371-11e7-9d72-b335598734c3.png)

9. jCenter에 올리기 위해서는 프로젝트 내부의오른쪽 아래에 있는 Link to를 누르면 간단하게 가능하다.<br>
![image](https://user-images.githubusercontent.com/6357456/30904455-53526c12-a372-11e7-8091-a0c2567a8017.png)

# kotlin javadoc 빌드 에러 해결하기
kotlin 프로젝트 빌드시 javadoc 에러가 나는데 다음 코드를 모듈 build.gradle에 추가하면 해결이 가능하다.<br>
```gradle

tasks.withType(Javadoc).all {
    enabled = false
}
```

-----------
이 포스트는 [다음 글](http://www.kmshack.kr/2016/06/jcenter%EB%A1%9C-%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0/)을 참고 하여 작성하였습니다.