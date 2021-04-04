---
layout: post
title:  "Dynamic Time Warping (동적 시간 워핑)"
date:   2021-04-05T14:25:52-05:00
categories: For-You
---

<h3> 시계열 데이터의 유사성 </h3>

![alt text]({{ site.baseurl }}/assets/dtw-origin.png "Profile Picture"){:.profile}

- A = **[1, 3, 5, 7, 6, 8, 9, 10, 8, 7] ** <br/>
- B = **[1, 2, 6, 5, 7, 8]** <br/>
위 두 시계열 데이터 (벡터)를 보자. 데이터 A는 데이터 B 보다 길이가 길다. 이 처럼 길이가 다른 두 시계열 데이터의 유사성 (distance, similarity)을 어떻게 계산할까?

![alt text]({{ site.baseurl }}/assets/dtw-origin2.png "Profile Picture"){:.profile}
가장 간단하게는 위와 같이 데이터 A 전체를 데이터 B와 비교하는 것이 아니라 B의 길이 만큼만 비교하는 간단한 방법이 있다.   
하지만, 이는 데이터 A 일부를 무시하므로 부적절한 방법이라 할 수 있다. (distance: 2.645751).

```python
from scipy.spatial.distance import euclidean
A = [1, 3, 5, 7, 6, 8, 9, 10, 8, 7]
B = [1, 2, 6, 5, 7, 8]
print("distance: ", euclidean(A[:len(B)], B))
```

<h3> Dynamic time warping이란? </h3>

DTW란 시계열 데이터 간 비교를 위한 최적의 index 매칭을 추정하는 알고리즘이다. 알고리즘 이름의 warping (휘다; 휘게 만들다)은 길이가 짧은 데이터를 휘게 하는 것이라 이해하면 더 쉽다. 아래의 알고리즘과 제일 하단의 그림으로 쉽게 이해할 수 있다.
알고리즘은 간단하다.


Step 1. Distance matrix를 구성하여 각 index의 값 간의 유사도 (distance)를 계산한다.

![alt text]({{ site.baseurl }}/assets/dtw-similarity.png "Profile Picture"){:.profile}

Step 2. 좌 상단 (첫 index)부터 우 하단 (마지막 index)까지 최소 cost (distance)로 이동할 수 있는 최단 경로를 찾는다.

![alt text]({{ site.baseurl }}/assets/dtw-similarity2.png "Profile Picture"){:.profile}

Step 3. 경로에 포함된 노란색 셀이 최적의 index 매칭 및 유사도 (distance)이다.
DTW 알고리즘을 실제로 구현을 한다면 Dynamic programming으로 쉽게 가능하다. 가장 중요한 제약 조건은 음의 방향으로는 이동하지 못한다는 것이다.


python에 fastdtw라는 library가 있어 간단하게 실험을 수행하고 결과값을 볼 수 있다. 
fastdtw 함수의 출력 값은 두 데이터의 Manhattan distance ([0])와 최적의 path이다 ([1]).


path: [(0, 0), (1, 1), (2, 2), (2, 3), (3, 4), (4, 4), (5, 5), (6, 5), (7, 5), (8, 5), (9, 5)].


최적의 path를 통해 길이가 짧은 데이터 B를 warping한 데이터 그래프는 아래와 같다.

![alt text]({{ site.baseurl }}/assets/dtw-result.png "Profile Picture"){:.profile}

실제로 길이가 짧았던 데이터 B는 warping 되었고, 두 데이터 간의 distance는 3.32로써 기존의 방법의 결과 값인 2.64와는 차이를 보이는 것을 확인했다.

```python
from fastdtw import fastdtw
path = fastdtw(A, B)[1]
B_ = []
for i in range(len(A)):
    B_.append(A[path[i][1]])
print("distance: ", euclidean(A, B_))
```
