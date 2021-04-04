---
layout: post
title:  "[dl] Max pooling, Mean pooling, Min pooling, 어느 것을 사용해야할까?"
date:   2021-04-05T14:25:52-05:00
tags: Max-pooling Mean-pooling Min-pooling CNN Deeplearning
categories: For-You
---

## Image data의 pooling layer 
CNN 모델을 구성할 때 pooling layer는 핵심적인 역할을 한다. 
먼저, convolution 연산을 통해 추출된 원본 이미지의 feature map 크기를 줄임으로서 이후 convolution 연산의 복잡성을 낮출 수 있다. 
그리고 feature map의 불필요한 feature를 제거함으로서 예측/분류 정확도를 개선한다. 이러한 역할을 하는 pooling layer를 동작에 따라 3가지로 세분화할 수 있다.

- Max pooling: 정의된 사이즈 내의 값 중 최대값 추출
- Mean pooling: 정의된 사이즈 내의 값들의 평균값 추출
- Min pooling: 정의된 사이즈 내의 값 중 최소값 추출

아래의 그림은 convolution layer를 통과한 후의 16 X 16 feature map에 2 X 2 pooling layer를 적용한 예시이다. 
Grey color 농도에 따라 1부터 5까지의 숫자를 지정하였다. (RGB 값이 흰색 >> 검은색임을 고려한 것이다.)

![alt text]({{ site.baseurl }}/assets/pooling.png "Profile Picture"){:.profile}

그림으로 이해되듯이 min pooling은 어두운 색상을 더 강조하고, max pooling은 밝은 색상을 더 강조한다. 
Yolo와 같이 CNN 기반의 유명 논문에서는 mean pooling이 사용되고, 일반적으로도 mean pooling이 통용된다. 이는 평균치로 잡으면 중간은 간다라는 국제적으로 통하는 인식 때문이 아닐까?
그렇다고 mean pooling이 지배적으로 좋은 선택임을 이야기하는 것은 아니고, 위의 그림에서 보다시피 어떤 pooling을 적용하느냐에 따라 결과가 확연히 다르기 때문에 `case-by-case` 선택이 필요하다.
(`케바케`도 국제적으로 통하지 까..?)
