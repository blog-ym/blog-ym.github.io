---
title: "Bubble Sort"
layout: post
date: 2017-07-17
image: /assets/images/docker_profile.png
headerImage: true
tag:
- Algorithm
star: true
category: blog
author: YM
description: algorithm summary
---

# Bubble_sort

정렬방식을 배울 때 빠지지 않는 정렬이다.

원소의 이동이 거품이 수면으로 올라오는 듯한 모습을 보이기 때문에 지어진 이름이다. \- 출처 : [wikipedia](https://ko.wikipedia.org/wiki/%EA%B1%B0%ED%92%88_%EC%A0%95%EB%A0%AC)

```
[9,4,7,6]
```
위와 같은 리스트가 존재할 때 버블 정렬을 적용해보면

```
[9,4,7,6]
9과 4를 비교하여 자리를 교환하고 [4,9,7,6]

9과 7를 비교하여 자리를 교환하고 [4,7,9,6]

9와 6을 비교하여 자리를 교환한다. [4,7,6,9] 
```

여기까지가 첫 인수 9에 대한 자리교환이다.

이제 첫번째 자리에 있는 4에 대해서 같은 과정을 거치도록 한다.

비교한 결과 4가 최소값이므로 변화가 없다.  

옆의 값과 비교하여 클 경우 자리변환을 한다. 이 과정을 할 수 없을때까지 반복한다. 

```
def bubble_sort(ori_list):
    ori_len = len(ori_list) - 1
    for b in range(ori_len):
        if ori_list[b] > ori_list[b + 1]:
            print('{}번쨰 loop2'.format(b))
            ori_list[b], ori_list[b + 1] = ori_list[b + 1], ori_list[b]
            print('{}와 {} 변화'.format(ori_list[b + 1], ori_list[b]))
            print(ori_list)
    print(ori_list)
```


내부의 첫번째 반복문을 통해 가장 큰 수를 리스트의 마지막 값으로 정렬하고 해당 값을 제외한 나머지 값들을  가지고 앞선 과정을 반복한다. 결국 남은 수 중 가장 큰 값을 뒤에서 두번째에 정렬하게 된다.

> for 문을 한번만 사용하게 될 경우 정렬을 하다가 중간에 끝이 난다.

이때, bubble sort의 시간 복잡도를 살펴보자. 두개의 for 문에 의해 시간복잡도는 n^2 를 나타낸다.


