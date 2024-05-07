---
layout: post
title: '알고리즘 - 스택(Stack) & 큐(Queue)'
subtitle: '알고리즘'
categories: python
tags: Algorithms
comments: false
order: 1
---

### 스택이란
자료를 보관할 수 있는 (선형) 구조이다


- 넣을때는 한 쪽 끝에서 넣어야 하고 (push 연산), 꺼낼때는 같은 쪽에서 뽑아 꺼내야 하는 (pop 연산) 제약이 있다.  
- 후입선출(LIFO) 특성을 가진다
- 구현
    - 배열 이용   
    ```python
    class ArrayStack:
        def __init__(self):
            self.data = []

        def size(self):
            return len(self.data)

        def is_empty(self):
            return self.size() == 0

        def push(self, item):
            self.data.append(item)

        def pop(self):
            return self.data.pop()

        def peek(self):
            return self.data[-1]
    ```
    - 연결 리스트 이용 -> [연결리스트 구현 코드 확인](https://heejung-gjt.github.io/python/2024/05/03/python-algorithms-3/)

<br><br>

### 큐란
자료를 보관할 수 있는 (선형) 구조이다  


- 넣을 때는 한 쪽 끝에서 밀어넣어야 하고 (enqueue 연산), 꺼낼 때에는 반대쪽에서 뽑아 꺼내야 하는 제약이 있다 (dequeue 연산)   
- 선입 선출 (FIFO) 특성을 가진다
- 구현
    - 배열 이용
        - size(): O(1)
        - is_empty: O(1)
        - enqueue: O(1)
        - dequeue: O(n)
        - peek(): O(1)    

    ```python
    def __init__(self):
        self.data = []

    def size(self):
        return len(self.data)

    def is_empty(self):
        return self.size() == 0

    def enqueue(self, item):
        self.data.append(item)

    def dequeue(self):
        return self.data.pop(0)

    def peek(self):
        return self.data[0]
    ```

    - 연결 리스트 이용
        - 시간 복잡도는 dequeue또한 O(1)으로 빠르다    

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


        def __repr__(self):
            if self.nodeCount == 0:
                return 'LinkedList: empty'

            s = ''
            curr = self.head
            while curr.next.next:
                curr = curr.next
                s += repr(curr.data)
                if curr.next.next is not None:
                    s += ' -> '
            return s


        def getLength(self):
            return self.nodeCount


        def traverse(self):
            result = []
            curr = self.head
            while curr.next.next:
                curr = curr.next
                result.append(curr.data)
            return result


        def reverse(self):
            result = []
            curr = self.tail
            while curr.prev.prev:
                curr = curr.prev
                result.append(curr.data)
            return result


        def getAt(self, pos):
            if pos < 0 or pos > self.nodeCount:
                return None

            if pos > self.nodeCount // 2:
                i = 0
                curr = self.tail
                while i < self.nodeCount - pos + 1:
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


        def insertAt(self, pos, newNode):
            if pos < 1 or pos > self.nodeCount + 1:
                return False

            prev = self.getAt(pos - 1)
            return self.insertAfter(prev, newNode)


        def popAfter(self, prev):
            curr = prev.next
            next = curr.next
            prev.next = next
            next.prev = prev
            self.nodeCount -= 1
            return curr.data


        def popAt(self, pos):
            if pos < 1 or pos > self.nodeCount:
                raise IndexError('Index out of range')

            prev = self.getAt(pos - 1)
            return self.popAfter(prev)


        def concat(self, L):
            self.tail.prev.next = L.head.next
            L.head.next.prev = self.tail.prev
            self.tail = L.tail

            self.nodeCount += L.nodeCount


    class LinkedListQueue:

        def __init__(self):
            self.data = DoublyLinkedList()

        def size(self):
            return self.data.getLength()

        def isEmpty(self):
            return self.size() == 0

        def enqueue(self, item):
            node = Node(item)
            self.data.insertAt(self.size()+1, node)

        def dequeue(self):
            return self.data.popAt(1)

        def peek(self):
            return self.data.getAt(1).data


    def solution(x):
        return 0
    ```
