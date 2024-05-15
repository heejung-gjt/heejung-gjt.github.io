---
layout: post
title: '프로그래머스 LV0 - 외계어 사전'
subtitle: '알고리즘'
categories: python
tags: programmers
comments: false
order: 1
---


문제의 조건을 잘 읽으면 어떤 함수? 어떤 알고리즘? 을 사용해야 하는지 알 수 있게 된다. 하지만 나는 이문제를 풀때 "중복된 원소"에는 크게 신경 쓰지 않고 itertools의 순열이나 조합을 사용하면 되겠다라고 가장 먼저 떠올렸다.   
문제는.. combinations나 permutations냐 헷갈렸다는거.. ^^   
- combinations: iterable에서 원소 개수가 r개인 조합 뽑기
    - 중복은 따로 나오지 않는다    

    ```python
    from itertools import combinations

    a = ['p', 'o', 's']
    list(combinations(a, 3))  # [('p', 'o', 's')]
    ```

- permutations: iterable에서 원소 개수가 r개인 순열 뽑기
    - 위치가 다른 중복값이 나온다    

    ```python
    from itertools import permutations

    a = ['p', 'o', 's']
    list(permutations(a, 3))  # [('p', 'o', 's'), ('p', 's', 'o'), ('o', 'p', 's'), ('o', 's', 'p'), ('s', 'p', 'o'), ('s', 'o', 'p')]
    ```

<br>

### 문제 풀이
```python
import itertools

def solution(spell, dic):
    answer = []
    permutations = list(itertools.permutations(spell, len(spell)))
    transformed_permutations = list(map(lambda x: ''.join(x), permutations))

    for i in dic:
        if i in transformed_permutations:
            return 1
        
    return 2
```


<br>

### 그 외 풀이

set을 사용한 풀이다. 간단하기는 하다   
len(spell) == len(s)을 해주는 이유      
    - ex) spell = ['m', 'o', 's']이고 dic에 ['moos']값 있는 경우   

```python
def solution(spell, dic):
    for s in dic:
        if (len(spell) == len(s)) and (not set(spell) - set(s)):
            return 1
    return 2
```