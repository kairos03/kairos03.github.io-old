---
layout: post
title: Hands-On Tensorboard
subtitle: 텐서플로우 개발자 모임 2017 에서 발표된 'Hands-On Tensorboard' 정리
date: 2017-10-17
categories: Machine-learning
cover: https://user-images.githubusercontent.com/6357456/31669820-6c981548-b356-11e7-8f05-028eb0e7feae.png
tag:    [AI, machine-learning, deep-learning, tensorflow, tensorboard]
---

> 텐서플로우 개발자 모임 2017에서 발표된 **Hands-On Tensorboard**([동영상](https://www.youtube.com/watch?v=eBbEDRsCmv4))
를 정리한 글입니다.
> 전체 예제 코드는 다음 [링크](https://github.com/mamcgrath/TensorBoard-TF-Dev-Summit-Tutorial/blob/master/mnist.py)
에서 볼수 있고 강의 슬라이드는 다음 [링크](https://github.com/mamcgrath/TensorBoard-TF-Dev-Summit-Tutorial/blob/master/slides.pdf)
에서 볼 수 있습니다.

# Tensorboard
Tensorboard는 Tensorflow로 작업한 모델을 시각화 해주는 툴입니다.
굉장히 다양한 기능을 갖고 있는데 예제를 이용해 살펴보도록 하겠습니다.

# Basic MNIST CNN Model
예제로 사용되는 모델은 간단한 CNN MNIST 모델입니다.

![image](https://user-images.githubusercontent.com/6357456/31670351-a9d46c58-b357-11e7-8e55-894eb317fcae.png)

2개의 Conv2d Layer와 2개의 FC Layer로 되어 있습니다.

# Graph
Model의 형태를 알기 쉽게 보기 위해 `tf.summary.FileWriter()`를 세션선언 후에 추가해 줍니다.

![image](https://user-images.githubusercontent.com/6357456/31670948-19d1f47a-b359-11e7-8084-7164e22cd9eb.png)

그 후에 텐서보드를 띄우기 위해서 터미널에서 다음 명령을 실행합니다.
```bash
$ tensorboard --logdir /tmp/mnist_demo/1
```
이 후 웹브라우저에서 <http://localhost:6006/>에 접속합니다.

![image](https://user-images.githubusercontent.com/6357456/31673341-47cd2704-b35f-11e7-82c7-0f887979d8fd.png)

GRAPHS 탭으로 가면 이런 매우 복잡한 Graph를 볼수 있습니다.

Graph를 깔끔하게 정리하기 위해 **Node Names**과 **Name Scopes**를 사용합니다.

![image](https://user-images.githubusercontent.com/6357456/31673811-745a6f56-b360-11e7-8f3c-0ac14363315a.png)

다음과 같이 **Node Names**과 **Name Scopes**를 추가 할 수 있습니다.

![image](https://user-images.githubusercontent.com/6357456/31674196-7810bf3c-b361-11e7-816b-7757707a1caa.png)

다음과 같이 좀더 깔끔한 형태의 그래프를 얻을 수 있습니다.

더블 클릭을 통해 각 노드를 세부적으로 볼수 있습니다. 또한 색을 통해 어떤 같은 노드를 구분 할수 있습니다.
왼쪽의 옵션을 이용해서 각 노드의 CPU, GPU배치를 확인 할 수 있고, 입력값을 추적 할 수 있습니다.

# Summary
later