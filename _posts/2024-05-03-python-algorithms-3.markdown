---
layout: post
title: '알고리즘 - 연결리스트(Linked List)'
subtitle: '알고리즘'
categories: python
tags: Algorithms
comments: false
order: 1
---

### 연결 리스트란
연결 리스트는 데이터를 연속된 메모리 공간에 저장하지 않고 각 데이터가 다음 데이터의 위치를 가리키는 형태로 구성된 선형 자료 구조이다.    
- 각 데이터 단위: 노드   
- 시간 복잡도
    - 데이터 추가와 삭제: 상황에 따라 다름
        - 맨 앞 삽입: O(1)
        - tail을 알 경우에 맨 뒤 삽입: O(1)
        - tail을 알 수 없는 경우에 맨 뒤 삽입: O(N)
    - 특정 위치의 데이터 검색: O(n)
- 각 노드가 데이터와 포인터를 가지고 데이터를 저장한다

<br>

### 연결 리스트 종류

- 단일 연결 리스트
    각 노드에 자료 공간과 한개의 포인터 공간이 있고 각 노드의 포인터는 다음 노드를 가리킴
    <img width="382" alt="스크린샷 2024-05-05 오후 7 16 18" src="https://github.com/heejung-gjt/heejung-gjt.github.io/assets/64240637/dd986580-f1d8-4c3b-9d03-30bde3d44627">

- 이중 연결 리스트
    포인터 공간이 두개가 있고 각각의 포인터는 앞의 노드와 뒤의 노드를 가리킴
    <img width="389" alt="스크린샷 2024-05-05 오후 7 16 35" src="https://github.com/heejung-gjt/heejung-gjt.github.io/assets/64240637/c281b83c-7fe0-4ccd-bd51-7377c6508fca">
- 원형 연결 리스트
    연결 리스트에 마지막 노드와 처음 노드를 연결시켜 원형을 만든 구조임
    <img width="371" alt="스크린샷 2024-05-05 오후 7 16 49" src="https://github.com/heejung-gjt/heejung-gjt.github.io/assets/64240637/3b5371b0-5f83-4808-bf59-ad445e800b01">


<br>

### 단일 연결리스트 구현
```python
    class Node:
        def __init__(self, item):
            self.data = item
            self.next = None


    class LinkedList:
        def __init__(self):
            self.nodeCount = 0
            self.head = Node(None)
            self.tail = None
            self.head.next = self.tail

        def traverse(self):  # 모든 데이터 조회
            result = []
            curr = self.head
            while curr.next:
                curr = curr.next
                result.append(curr.data)
            return result

        def getAt(self, pos):  # 특정 데이터 조회
            if pos < 0 or pos > self.nodeCount:
                return None

            i = 0
            curr = self.head
            while i < pos:
                curr = curr.next
                i += 1
            return curr

        def insertAfter(self, prev, newNode):  # prev 다음에 데이터 삽입
            newNode.next = prev.next
            if prev.next is None:
                self.tail = newNode

            prev.next = newNode
            self.nodeCount += 1
            return True

        def insertAt(self, pos, newNode):  # 현재 pos에 데이터 삽입
            if pos < 1 or pos > self.nodeCount + 1:
                return False

            if pos != 1 and pos == self.nodeCount + 1:
                prev = self.tail
            else:
                prev = self.getAt(pos - 1)
            return self.insertAfter(prev, newNode)

        def popAfter(self, prev):  # prev 다음 데이터 pop
            if prev.next is None:
                return None

            curr = prev.next
            if curr.next is None:
                self.tail = prev
                prev.next = None

            prev.next = curr.next
            self.nodeCount -=1
            return curr.data

        def popAt(self, pos):  # 현재 pos 데이터 pop
            if pos < 1 or pos > self.nodeCount:
                raise IndexError

            prev = self.getAt(pos - 1)
            return self.popAfter(prev)


    def solution(x):
        return 0
```

<br>

### 양방향 연결리스트 구현 (작성중..)
아래 이미지처럼 더미 head, tail 데이터가 있는 구조로 구현됨    
<img width="920" alt="스크린샷 2024-05-05 오후 7 50 50" src="https://github.com/heejung-gjt/heejung-gjt.github.io/assets/64240637/28033d15-3286-41be-af11-c8641121ebbd">    


```python
class Node:

    def __init__(self, item):
        self.data = item
        self.prev = None
        self.next = None


class DoublyLinkedList:

    def __init__(self):
        self.nodeCount = 0
        self.head = Node(None)
        self.tail = Node(None)
        self.head.prev = None
        self.head.next = self.tail
        self.tail.prev = self.head
        self.tail.next = None


    def reverse(self):
        cnt = 0
        res = []
        curr = self.tail
        while curr.prev.prev:
            curr = curr.prev
            res.append(curr.data)
        return res


    def getAt(self, pos):
        if pos < 0 or pos > self.nodeCount:
            return None

        if pos > self.nodeCount // 2:
            i = 0
            curr = self.tail
            while i < self.nodeCount - pos + 1:  # TODO. 왜 범위가 이와 같은지 정확한 이유 이해하기
                curr = curr.prev
                i += 1
        else:
            i = 0
            curr = self.head
            while i < pos:
                curr = curr.next
                i += 1

        return curr


    def insertAfter(self, prev, newNode):
        next = prev.next
        newNode.prev = prev
        newNode.next = next
        prev.next = newNode
        next.prev = newNode
        self.nodeCount += 1
        return True

    def insertBefore(self, next, newNode):
        prev = next.prev
        newNode.prev = prev
        prev.next = newNode
        next.prev = newNode
        newNode.next = next
        self.nodeCount += 1
        return True

    def insertAt(self, pos, newNode):
        if pos < 1 or pos > self.nodeCount + 1:  # TODO. 왜 범위가 이와 같은지 정확한 이유 이해하기
            return False

        prev = self.getAt(pos - 1)
        return self.insertAfter(prev, newNode)

    def popAfter(self, prev):
        curr = prev.next
        next_node = curr.next
        next_node.prev = prev
        prev.next = next_node
        self.nodeCount -= 1
        return curr.data


    def popBefore(self, next):
        curr = next.prev
        prev_node = curr.prev
        prev_node.next = next
        next.prev = prev_node
        self.nodeCount -= 1
        return curr.data


    def popAt(self, pos):
        if pos < 1 or pos > self.nodeCount:  # TODO. 왜 범위가 이와 같은지 정확한 이유 이해하기
            raise IndexError

        prev = self.getAt(pos - 1)
        return self.popAfter(prev)

    def concat(self, L):
        link1 = self.tail.prev
        link2 = L.head.next
        link1.next = link2
        link2.prev = link1
        self.tail = L.tail
        self.nodeCount += L.nodeCount
        return True


def solution(x):
    return 0
```