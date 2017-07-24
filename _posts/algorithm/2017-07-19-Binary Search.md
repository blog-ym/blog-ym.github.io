---
title: "Binary Search"
layout: post
date: 2017-07-19
image: /assets/images/docker_profile.png
headerImage: true
tag:
- Algorithm
star: true
category: blog
author: YM
description: algorithm Binary Search summary
---

# Binary Search

> 이진탐색 : O(log n) 의 시간 복잡도를 갖는다.

이미 정렬된 데이터를 검색하는 알고리즘이다. 시작과 끝 값의 중간 값(mid)을 검색값(target) 과 일치 여부를 판단한다. 값을 찾을 때(더 이상의 중간값이 존재하지 않을 때)까지 반복적인 수행을 한다.

이진탐색은 O(log n) 의 시간 복잡도를 갖기 때문에 많은 데이터를 처리할 때 효율이 좋다. 이는 한번에 찾지 못하더라도 해당 수행으로 다음 수행에 이용하는 데이터 양이 반으로 줄기 때문이다. 

### Best Cas

탐색 한번에 찾는 경우. mid 값과 target의 값이 일치할 때이다. 

### Worst Case

탐색을 가장 많이 해야 하는 경우. **성능 평가**의 기준을 최악의 경우로 나타낸다.

> 예외적인 사항으로 Quick Sort 의 경우 평균으로 나타낸다. 

```
def binary_search(data, target):
# binary_search 의 특성상 정렬된 data를 받는다.
	start = 0
	end = len(data) - 1
	
	while start <= end:
		mid = (start+end)//2
		if target == mid:
			return mid
		elif target < data[mid]:
			end = mid - 1
		else:
			start = mid + 1
```

### Recursion(재귀)

함수를 정의할 때 내부에서 자신의 함수를 재참조하는 방식. binary search 를 나타낼 때 재귀함수로 표현해보자.

```
def binary_search_recursion(data, start, end, target):
	while start <= end:
		mid = (start+end) // 2
		if target == data[mid]:
			return mid
		elif target < data[mid]:
			end = mid-1
			binary_search_resursion(data, start, end, target)
			# 자신의 정의 내에서 자신을 재참조했다.
		else:
			start = mid + 1
			binary_search_resursion(data, start, end, target)
	return None
```



## Big - O 

1. O(1)
2. O(n)
3. O(log n)
4. O(n*log n)
5. O(n^2)

