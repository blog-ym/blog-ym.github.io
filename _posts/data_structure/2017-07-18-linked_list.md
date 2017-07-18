---
title: "linked list"
layout: post
date: 2017-07-19
image: /assets/images/docker_profile.png
headerImage: true
tag:
- data_structure
star: true
category: blog
author: YM
description: single linked list
---

# linked list

**ADT**(Abstract Data Type) : 추상 자료형. 컴퓨터 과학에서 자료들과 그 자료들에 대한 연산들을 명시. 구현 방법을 명시하지 않고 있다는 점에서 자료 구조와 다르다. - 출처:[wikipedia](https://ko.wikipedia.org/wiki/%EC%B6%94%EC%83%81_%EC%9E%90%EB%A3%8C%ED%98%95)

> 추상화 : 구현부와 인터페이스 분산시켜 놓는 것

list : 자료를 순차적으로 정리해 놓은 것.(순서가 있다)

이러한 list 가 linked. 즉, 메모리 상에서 연결되어 있다는 것이다.(데이터와 포인터를 가지고 한줄로 연결)

배열 리스트는 크기를 미리 정해놓고 해당 범위 내에서 관리한다. 결국 해당 범위보다 크게 사용할 수 없고 적은  크기의 자료를 다룰때는 낭비가 심하다.

반면 linked list 경우 자료가 입력될 때 동적으로 새로운 메모리 주소에 값을 할당하고 이전 자료와 연결을 통해 메모리 관리를 효율적으로 한다.

## single linked list

하나의 노드에 하나의 자료 공간(data)과 하나의 포인터 공간(reference)이 존재한다. 이 포인터는 다음 노드를 가리킨다.

### Linked List ADT

1. append(data) --> None
2. empty(  ) --> bool
	* 비어있다면 True
	* 비어있지 않다면 False
3. size(  ) --> integer
4. traverse(mode = 'next') --> **data**
	* 'first' : 첫번째에 해당할 때만
	* 'next' : 순회 기능
5. remove(  ) --> **data**

	cf) 자료구조  
		1. insert -- append  
		2. search -- traverse  
		3. delete -- remove 


### Insert(append)

> tail 은 항상 마지막을 가리킨다.

![linked_list](https://{{site.url}}/assets/images/post/linked_list.png)

새롭게 node 가 생기면 이 값을 연결할 수 있게 참조해줘야 한다. 단순하게 생각해보면 tail 은 마지막 값을 지정해야하기 때문에 새로 들어온 node를 가리켜야 한다.


<img src="/Users/youngminkim/Desktop/insert.png " width="1400">

```
self.tail.next = new_node
self.tail = new_node
```

일반적인 상황에서는 위와 같이 나타내지만 비어있는 data 일때는 새로 들어오는 값이 곧 head 이며 tail이다.

```
new_node = Node(data)

if self.empty():
    self.head = new_node
    self.tail = new_node
else:
    self.tail.next = new_node
    self.tail = new_node
```
append 의 전체 코드를 살펴보자

```
def append(self, data):
    new_node = Node(data)

    if self.empty():
        self.head = new_node
        self.tail = new_node
    else:
        self.tail.next = new_node
        self.tail = new_node

    self.num_data += 1
```


### Search

> 순회 전에 before & current 이 None 값을 갖는다.

<img src="/Users/youngminkim/Desktop/search.png " width="1400">

순차적으로 돌면서 찾기 위해 전제조건이 필요하다. mode = first 가 이미 선언이 되어있으며 비어있는 node 가 아니다.

```
 if mode == 'first':
    if self.empty():
        return None

    self.before = self.head
    self.current = self.head
```

순회할 때 current 가 다음 node 로 이동하고 before 가 이를 따라간다. 이 과정을 None 값을 찾을때까지 반복한다.

```
else:  
    if self.current.next is None:
        return None
    self.before = self.current
    self.current = self.current.next

return self.current.data
```

search 의 전체 코드를 살펴보자

```
    def travers(self, mode='next'):
        if mode == 'first':
            if self.empty():
                return None

            self.before = self.head
            self.current = self.head
        else:  # mode = 'next'
            if self.current.next is None:
                return None
            self.before = self.current
            self.current = self.current.next

        return self.current.data
```


### remove
> current 가 가리키는 위치 삭제


<img src="/Users/youngminkim/Desktop/remove.png " width=1400>

일반적으로 중간값을 지우고자 할때 before의 next 가 current의 next 를 가리키고 current는 before 로 이동한다.

```
self.before.next = self.current.next
self.current = self.before
```

그렇다면 **예외적인 경우**를 살펴보자.

첫번째로, 크기가 1인 경우이다. 이때 head, before, current, tail 이 모두 같은 값을 node를 갖는다.

```
if node.size() == 1:
	self.head = None
	self.before = None
	self.current = None
	self.tail = None
```

두번째, 첫번째 node 를 삭제하기 위해 head, before, corrent 가 같은 node를 갖을 때이다.

```
if self.current is self.head:
	self.head = self.head.next
	self.before = self.before.next
	self.current = self.current.next
```

세번째, 마지막 node 를 삭제하기 위해 current 가 tail 과 같은 node 를 갖을 때이다.

```
if self.current is self.tail:
	self.before = self.tail
```

즉 모든 예외처리를 한 remove 전체 코드를 살펴보자.

```
def remove(self):
    ret_data = self.current.data

    if self.size() == 1:
        self.head = None
        self.before = None
        self.current = None
        self.tail = None

    elif self.current is self.head:
        self.head = self.head.next
        self.current = self.current.next
        self.before = self.before.next
    else:
        if self.current is self.tail:
            self.tail = self.before
        self.before.next = self.current.next
        self.current = self.before
    self.num_data -= 1
    return ret_data
```