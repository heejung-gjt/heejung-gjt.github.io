---
layout: post
title: '알고리즘 - 재귀함수(recursive functions)'
subtitle: '알고리즘'
categories: python
tags: Algorithms
comments: false
order: 1
---

### 재귀 알고리즘이란
하나의 함수에서 자신을 다시 호출하여 작업을 수행하는 알고리즘
- 재귀 호출에는 항상 종료 조건이 필요하다
- 시간복잡도: O(n^2)
- 재귀 함수의 단점: 메모리 사용량이 증가하고, 재귀 호출이 지나치게 많이 일어날 경우 스택 오버플로우가 발생할 수 있음, 하지만 효율적인 측면에서 재귀함수를 썼을때 더 좋은 경우가 있기 때문에 적절히 사용해줘야 함
<br>

- 구현예제
    - 1 ~ n까지의 모든 합 구하기
    ```
    def sum_func(n):
        if n >= 1:
            return n
        else:
            return n + (n - 1)
    ```

    - 팩토리얼(n! -> n * n-1 * n-2...) 구하기
    ```python
    def factorial(n):
        if n == 1:
            return 1

        else:
            return n * factorial(n - 1)
    ```

    - 피보나치 구하기
    ```python
    def fibonacci:
        if n == 0 or n == 1:
            return 1
        else:
            return n + fibonacci(n - 1) + fibonacci(n - 2)
    ```