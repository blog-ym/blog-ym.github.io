17-07-16 (1)
--- 

## Data structure

1. 자료구조 : 어떻게 해야 가장 효율적인가
	* insert : 어떻게 저장할 것인가.
	* search : 어떻게 탐색할 것인가.
	* delete : 필요 없는 데이터를 어떻게 지울 것인가.
2. 알고리즘 : 문제 해결하는 방법론

1. 자료구조의 알고리즘 : 어떻게
2. 자료구조를 이용한 알고리즘 : 자료구조를 이용한 문제 해결


### 배열 
동일한 자료형을 가진 변수의 모음, 고정된 배열(전체 크기가 정해짐)
> 접근이 빈번하며 수정이 거의 없을 때

1. 새로운 공간 할당
2. 기존의 데이터 복사
3. 기존의 데이터 삭제

> 메모리 몹시 차지

index 한방에 가능


### linked list : 뒤에 붙이기만하면 끝.(지우거나 복사 필요 없다.)

> 접근 방법이 앞에서부터 순차적으로 

> 접근은 빈도가 낮고 추가 삭제가 빈번


* 배열을 사용할 수 있을 때는 linked list 보다 우선(?)


17-07-16 (2)
--- 


## 재귀함수

함수 정의 내에서 다시 재사용

# def function(num, start, end):
#     num_list = range(start, end + 1)
#     mid = end % 2
#     while start < end:
#         if num < mid:
#             function(num, start, mid)
#         elif num > mid:
#             function(num, mid, end)
#         elif num == mid:
#             print(num_list.index(mid))
#             break























#### ps
compiler lang(?)

stackoverflow : 메모리를 잡아 내려가다가 펑!