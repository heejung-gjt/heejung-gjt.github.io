---
layout: post
title: '프로그래머스 LV0 - 삼각형의 완성조건(2)'
subtitle: '알고리즘'
categories: python
tags: programmers
comments: false
order: 1
---

입출력 예를 보고 2가지의 조건으로 변의 개수가 도출되는것을 보고 for문을 통해 2가지 조건을 각각 만들었다

- 입출력의 예시
```
가장 긴 변이 6인 경우
될 수 있는 나머지 한 변은 4, 5, 6 로 3개입니다.
나머지 한 변이 가장 긴 변인 경우
될 수 있는 한 변은 7, 8 로 2개입니다.
따라서 3 + 2 = 5를 return합니다.
```

- 시간 복잡도: O(n)
```python
def solution(sides):
    answer = []
    max_num = max(sides)
    min_num = min(sides)

    for i in range(1, max_num + 1):
        if max_num < min_num + i:
            answer.append(i)

    for j in range(max_num + 1, sum(sides)):
        if sum(sides) > j:
            answer.append(j)

    return len(set(answer))
```

<br>

### 그 외 풀이


```python
def solution(sides):
    return 2 * min(sides) - 1
```
......🫠🫠🫠🫠 수학적 공식을 이용한 느낌.. 이렇게 풀었을때 얻기 힘든것은 조건에 해당하는 모든 값들을 나열해야 되는 경우에는 사용할 수 없는 풀이 방법이 되긴 한다   
자.. 한번 이해해보도록 노력해보자

- 풀이    

    ```text
    i) c가 a보다 크면 대소관계는 c > a > b가 된다
    a + b > c 이므로 a + b > c > a가 된다

    즉 이때 가능한 c의 갯수는 b - 1개

    ii) c가 a보다 작으면 대소관계는 a > b, a > c 가 된다
    b + c > a 이므로 b + c > a > c가 된다

    즉 이때 가능한 c의 갯수는 b - 1개

    iii) c와 a가 같으면 c = a 한 가지 밖에 존재하지 않는다
    다 합하면 (b - 1) + (b - 1) + 1 = 2 * b - 1 개가 된다

    이때 초기 조건에 따라 b는 a보다 작다
    ```

[해설 참고 블로그](https://velog.io/@cindy0817-web/%EC%BD%94%EB%94%A9%ED%85%8C%EC%8A%A4%ED%8A%B8-lv0-%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%82%BC%EA%B0%81%ED%98%95%EC%9D%98-%EC%99%84%EC%84%B1%EC%A1%B0%EA%B1%B42)

<br><br>

[문제 출처](https://school.programmers.co.kr/learn/courses/30/lessons/120868)

<br><br>