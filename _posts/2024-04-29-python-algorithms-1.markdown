---
layout: post
title: '알고리즘 - 이진탐색 (Binary Search)'
subtitle: '240425'
categories: python
tags: Algorithms
comments: false
order: 1
---

### 이진탐색이란
데이터가 오름차순으로 __정렬되어 있는 배열__ 에서 특정한 값을 찾아내는 알고리즘
- 크기순으로 정렬되어 있는 성질을 이용
- 탐색할 범위가 너무 많은 경우 (천만건 이상의 데이터 탐색해야 하는 경우)
- 시간 복잡도: O(log n)

- 방식      
    1. 배열의 중간값을 선택한다 (middle = 7)
    2. 찾으려는 값 n값과 비교한다 (n = 30)
    3. 중간값 > n의 경우 중간값 기준 왼쪽의 데이터들과 비교한다 (1 ~ 24)
    4. 중간값 < n의 경우 중간값 기준 오른쪽의 데이터들과 비교한다 (32 ~ 62)

        <img width="800" alt="스크린샷 2024-04-29 오후 4 03 42" src="https://github.com/Odreystella/Odreystella.github.io/assets/64240637/a653b3ae-5ec3-48a3-9f4d-c8979c640104">


        <img width="787" alt="스크린샷 2024-04-29 오후 4 04 26" src="https://github.com/Odreystella/Odreystella.github.io/assets/64240637/1ca452d4-1817-455b-ac21-e278404c5234">


<br>

- 구현
    - 반복문을 이용
    ```python
    def binary_search(L, x):
        start = 0
        end = len(L) - 1

        while start <= end:
            mid = (start + end) // 2  # 중간값
            if L[mid] == x:
                return mid

            elif L[mid] > x:  # 중간값이 더 큰 경우, end값 -1처리
                end = mid - 1

            else:
                start = mid + 1  # 중간값이 더 작은 경우, start값 +1 처리

        return -1
    ```

    - bisect라이브러리를 이용
    ```python
    from bisect import bisect_left, bisect_right
    def solution(L, x):
        index = bisect_left(L, x)
        if index < len(L) and L[index] == x:
            return index
        else:
            return -1
    ```

