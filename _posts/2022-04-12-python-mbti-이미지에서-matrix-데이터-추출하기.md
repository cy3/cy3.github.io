---
layout: post
title: python MBTI 이미지에서 matrix 데이터 추출하기
date: 2022-04-12 03:12:33 +0000
tags: cs python 
---

다음과 같은 png 파일에서 matrix를 뽑아내보자.

![image](/images/2022-04-12-python-mbti-이미지에서-matrix-데이터-추출하기/matrix.png){: .center-image}

---

```python
import numpy as np
from PIL import Image
from sklearn.manifold import MDS
import matplotlib.pyplot as plt
```

먼저 파일을 불러온다.

```python
im = Image.open('matrix.png')
```

색상 정보를 뽑아오기 위해 점을 찍어본다. 약간의 노가다로 위치를 맞춰주었다.

```python
rgb = im.convert("RGB")
start_width = 160
block_width = 84
start_height = 105
block_height = 32

plt.figure(dpi=300)
plt.imshow(im)

rgbmat = np.zeros((16,16,3), dtype=int)
for i in range(16):
    for j in range(16):
        cur_point = (start_width+block_width*i,start_height+block_height*j)
        rgbmat[i][j] = rgb.getpixel(cur_point)
        plt.scatter(*cur_point, s=5, c='red', marker='o')
plt.show()
rgbmat.shape
```

![image](/images/2022-04-12-python-mbti-이미지에서-matrix-데이터-추출하기/608E9FF2-CA7A-44D6-8487-7C50EFC346B1.png){: .center-image}

RGB값들이 일정하지 않기 때문에 k-means 를 사용해서 색상을 매핑해 줄 것이다.
먼저 RGB값들을 plot해 본다.

```python
from mpl_toolkits.mplot3d import Axes3D
fig = plt.figure(dpi=300)
ax=fig.add_subplot(111, projection='3d')
data = rgbmat.reshape(-1, 3)
ax.scatter(data[:,0],data[:,1], data[:,2])
plt.show()
```


![image](/images/2022-04-12-python-mbti-이미지에서-matrix-데이터-추출하기/45BD60A3-5054-4634-AF38-E9657DD9F758.png){: .center-image}

쉽게 클러스터링 할 수 있을만한 모양이 나왔다. k-means를 돌려서 잘 분류됬는지 확인해 본다.

```python
from sklearn.cluster import KMeans

fig = plt.figure(dpi=300)
kmeans = KMeans(n_clusters=5)
ids = kmeans.fit(data)
ax=fig.add_subplot(111, projection='3d')
ax.scatter(data[:,0],data[:,1], data[:,2], c=ids.labels_)
plt.show()
```

![image](/images/2022-04-12-python-mbti-이미지에서-matrix-데이터-추출하기/3CC830FB-187F-4FF1-91DB-5BDC5B424543.png){: .center-image}

이제 matrix 형태로 바꾼다.

```python
convdict = {}
for i,datum in enumerate(data):
    convdict[tuple(datum)]=ids.labels_[i]
convmat = np.zeros((16,16),dtype=int)
for i in range(16):
    for j in range(16):
        convmat[i,j] = convdict[tuple(data[i*16+j])]
convmat
```

![image](/images/2022-04-12-python-mbti-이미지에서-matrix-데이터-추출하기/7B09BD03-54CD-4649-BA5C-E99D53559FE6.png){: .center-image}

