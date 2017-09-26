---
layout: post
title: Tensor flow CNN code review
subtitle: conv2d, filter, padding, stride, max_pool
date: 2017-09-19
categories: Machine-learning
cover: https://user-images.githubusercontent.com/6357456/30470074-9b04e2fc-99f3-11e7-9c89-869dc06cc8f3.png
tag:    [AI, machine-learning, deep-learning, paper, trend]
---

기본 CNN 튜토리얼 코드를 보면서 Tensorflow의 CNN 코드를 이해해봅니다.

# 구조
기본 구조는 다음과 같습니다.

## L1 (Conv)
input: 28, 28, 1  
filter: 3, 3, 1, 32  
stride: 1, 1  
padding: 1, 1  
activation: ReLU  
max_pool: 2, 2  

## L2 (Conv)
input: 14, 14, 32  
filter: 3, 3, 32, 64  
stride: 1, 1  
padding: 1, 1
activation: ReLU
max_pool: 2, 2

## L3 (FC)
input: 7, 7, 128
weight: 7 * 7 * 64, 1024
activation: ReLU

## L4 (FC)
input: -1, 1024
weight: 1024, 10

# Code
## import and data_set load
```python
import tensorflow as tf
from tensorflow.examples.tutorials.mnist import input_data

mnist = input_data.read_data_sets("./mnist/data/", one_hot=True)
```
mnist 데이터를 로딩합니다.

## placeholder
```python
# placeholder
X = tf.placeholder(tf.float32, [None, 28, 28, 1])
Y = tf.placeholder(tf.float32, [None, 10])
keep_prob = tf.placeholder(tf.float32)
```
`placeholder` 를 설정합니다.

`X` 는 이미지를 `[None, 28, 28, 1]`로 변환합니다.
input 형식은 아래쪽 `tf.nn.conv2d` 에서 설명합니다.
`Y` 는 10개중 하나만 1로 표시된 one_hot 인코딩된 label 데이터를 받습니다.
`keep_prob` 는 `FC layer` 에서 사용될 `dropout` parameter 입니다.

## Model
### Convolution Layer 1 (input layer)
```python
# model
# L1 (Conv)
W1 = tf.Variable(tf.random_normal([3, 3, 1, 32]))
L1 = tf.nn.conv2d(X, W1, strides=[1, 1, 1, 1], padding="SAME")
L1 = tf.nn.relu(L1)
L1 = tf.nn.max_pool(L1, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1], padding="SAME")
```
첫번째 Conv layer 를 구성합니다.

#### Weight
CNN에서는 weight 를 `filter` 로 사용합니다.
필터는 `[filter_height, filter_width, input_channel, output_channel]` 형태로 아래
자세히 설명합니다.
여기서는 3 * 3 크기의 1개의 채널을 입력받아 32개로 만드는 필터를 생성합니다.

#### tf.nn.conv2d
먼저 `tf.nn.conv2d` 를 살펴보면 다음과 같습니다.
```
tf.nn.conv2d(input, filter, strides, padding)
```

conv2d 의 입력값입니다. 이미지를 분석한다면 처음에는 raw 이미지가 입력되겠죠?
`input: [batch, input_height, input_width, input_channel]` 형태입니다.
```
batch: batch 크기
input_height: 높이
input_width: 너비
input_channel: channel(filter 갯수)
```

filter 의 크기와 입출력 채널의 갯수를 정의 합니다.
`filter: [filter_height, filter_width, input_channel, output_channel]` 형태입니다.
```
filter_height: 필터 높이
filter_width: 필터 너비
input_channel: 입력 채널 갯수
output_channel: 출력 채널 갯수
```

`stride`는 `[1, h , w, 1]`의 형태의 tensor 로 필터의 이동 크기를 설정합니다.
높이로 h 만큼, 너비로 w 만큼 이동하며, 보통 정사각형태로 컨볼루션을 하기 때문에
h 와 w 는 같을 경우가 많습니다.

`padding`은 `'SAME'` or `'VALID'`의 문자열로 `'SAME'`은 기존 이미지사이즈를
유지하게 추가되고 `'VALID'`는 패딩 없이 conv 연산이 수행됩니다.

input에는 로드한
여기서는 weight 를 `filter` 로 사용합니다.

예제에서는 input을 X,  weight 를 `filter` 로 사용하고 strides 와 padding 은
각각 `[1,1,1,1]`, `'SAME'`으로 3 * 3 짜리 필터를 거처서 32개의 체널을 생성합니다.

#### tf.nn.max_pool
tf.nn.max_pool은 다음과 같습니다.
```
tf.nn.max_pool(input, ksize, strides, padding)
```

`input`, `strides`, `padding`은 conv2d 와 같습니다.
`ksize`는 하나의 값으로 대체할 매트릭스의 크기를 나타냅니다.
모양은 `strides`와 동일하게 `[1, h, w, 1]`입니다.

여기서는 2 * 2 칸을 하나의 최대값으로 pool 합니다.

### Convolution Layer 2
```python
# L2
# L2 (Conv)
W2 = tf.Variable(tf.random_normal([3, 3, 32, 64]))
L2 = tf.nn.conv2d(L1, W2, strides=[1, 1, 1, 1], padding="SAME")
L2 = tf.nn.relu(L2)
L2 = tf.nn.max_pool(L2, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1], padding="SAME")
```

L1과 동일하나 채널크기가 32 에서 64로 늘어납니다.

### Full Connected Layer 1
```python
# L3 (FC)
W3 = tf.Variable(tf.random_normal([7 * 7 * 64, 1024]))
L3 = tf.reshape(L2, [-1, 7 * 7 * 64])
L3 = tf.matmul(L3, W3)
L3 = tf.nn.relu(L3)
L3 = tf.nn.dropout(L3, keep_prob=keep_prob)
```

기존의 DNN과 같은 모양의 FC Layer 입니다.
봐야할 부분은 Conv Layer에서 넘어온 `[?, 7, 7, 64]`의 Tensor 를
벡터 형태의 일자 모양으로 `reshape` 하는 부분입니다.
여기서는 `7 * 7 * 64`를 input 으로 받고 hidden layer 로 1024의 노드를 만듦니다.
마지막으로 Dropout 을 시켜줍니다.

### Full Connected Layer 2 (output layer)
```python
# L4 (FC)
W4 = tf.Variable(tf.random_normal([1024, 10]))
model = tf.matmul(L3, W4)
```

FC1과 같으나 데이터를 10개로 분류하기 위해 1024노드를 10개로 줄였습니다.
이 10개 노드가 출력 층이 됩니다.

### cost function and optimizer
```python
# cost and optimizer
cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(labels=Y, logits=model))
optimizer = tf.train.AdadeltaOptimizer(0.01).minimize(cost)
```

cost 함수는 `softmax_cross_entropy_with_logits`를 사용하여 구현합니다.
optimizer 는 `Adadelta` 를 사용하며 learning_rate: 0.01, cost를 최소화합니다.

## Train
### initiation
```python
# train
# init
init = tf.initialize_all_variables()
sess = tf.Session()
sess.run(init)

# batch
batch_size = 100
total_batch_size = int(mnist.train.num_examples / batch_size)
```

세션을 생성하고 변수를 초기화 해줍니다. batch 크기를 지정합니다.

### training
```python
# training
print("Start!")
for epoch in range(1000):
    total_cost = 0
    for i in range(total_batch_size):

        batch_xs, batch_ys = mnist.train.next_batch(batch_size)
        batch_xs = batch_xs.reshape(-1, 28, 28, 1)

        _, cost_val = sess.run([optimizer, cost],
                               feed_dict={X: batch_xs,
                                          Y: batch_ys,
                                          keep_prob: 0.7})
        total_cost += cost_val

    if epoch % 10 == 0:
        print('epoch: {:05d}, avg_cost: {:.6f}'.format(epoch, (total_cost/total_batch_size)))

    # model save
    tf.train.Saver().save(sess, 'cnn_model')

print("Complete!")
```

1000 epoch를 돌리며 학습시킵니다.
각 batch 마다 input 값을 모델의 값에 맞게 `reshape` 해줍니다.
Dropout parameter인 `keep_prob`값은 0.7로 해줍니다.
출력은 10 epoch 마다 하도록 하며 한 epoch 가 끝나면 모델을 저장합니다.

### Validate
```python
# validate
is_correct = tf.equal(tf.argmax(model, 1), tf.argmax(Y, 1))
accuracy = tf.reduce_mean(tf.cast(is_correct, tf.float32))
print("accuracy: ", sess.run([accuracy],
                             feed_dict={X: mnist.test.images.reshape(-1, 28, 28, 1),
                                        Y: mnist.test.labels,
                                        keep_prob: 1}))
```
모델이 예측한 예측값과 실제 label 을 비교합니다.
validate 때는 dropout 을 시키지 않습니다.

# 전체 코드
```python
import tensorflow as tf
from tensorflow.examples.tutorials.mnist import input_data

mnist = input_data.read_data_sets("./mnist/data/", one_hot=True)

# placeholder
X = tf.placeholder(tf.float32, [None, 28, 28, 1])
Y = tf.placeholder(tf.float32, [None, 10])
keep_prob = tf.placeholder(tf.float32)

# model
# L1 (Conv)
W1 = tf.Variable(tf.random_normal([3, 3, 1, 32]))
L1 = tf.nn.conv2d(X, W1, strides=[1, 1, 1, 1], padding="SAME")
L1 = tf.nn.relu(L1)
L1 = tf.nn.max_pool(L1, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1], padding="SAME")

# L2 (Conv)
W2 = tf.Variable(tf.random_normal([3, 3, 32, 64]))
L2 = tf.nn.conv2d(L1, W2, strides=[1, 1, 1, 1], padding="SAME")
L2 = tf.nn.relu(L2)
L2 = tf.nn.max_pool(L2, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1], padding="SAME")

# L3 (FC)
W3 = tf.Variable(tf.random_normal([7 * 7 * 64, 1024]))
L3 = tf.reshape(L2, [-1, 7 * 7 * 64])
L3 = tf.matmul(L3, W3)
L3 = tf.nn.relu(L3)
L3 = tf.nn.dropout(L3, keep_prob=keep_prob)

# L4 (FC)
W4 = tf.Variable(tf.random_normal([1024, 10]))
model = tf.matmul(L3, W4)

# cost and optimizer
cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(labels=Y, logits=model))
optimizer = tf.train.AdadeltaOptimizer(0.01).minimize(cost)

# train
# init
init = tf.initialize_all_variables()
sess = tf.Session()
sess.run(init)

# batch
batch_size = 100
total_batch_size = int(mnist.train.num_examples / batch_size)

# training
print("Start!")
for epoch in range(1000):
    total_cost = 0
    for i in range(total_batch_size):

        batch_xs, batch_ys = mnist.train.next_batch(batch_size)
        batch_xs = batch_xs.reshape(-1, 28, 28, 1)

        _, cost_val = sess.run([optimizer, cost],
                               feed_dict={X: batch_xs,
                                          Y: batch_ys,
                                          keep_prob: 0.7})
        total_cost += cost_val

    if epoch % 10 == 0:
        print('epoch: {:05d}, avg_cost: {:.6f}'.format(epoch, (total_cost/total_batch_size)))

    # model save
    tf.train.Saver().save(sess, 'cnn_model')

print("Complete!")

# validate
is_correct = tf.equal(tf.argmax(model, 1), tf.argmax(Y, 1))
accuracy = tf.reduce_mean(tf.cast(is_correct, tf.float32))
print("accuracy: ", sess.run([accuracy],
                             feed_dict={X: mnist.test.images.reshape(-1, 28, 28, 1),
                                        Y: mnist.test.labels,
                                        keep_prob: 1}))

```

> 설명이 틀린 부분이나 오탈자 제보는 언제든 환영입니다 ^^