---
layout: post
title: 'python의 math 모듈 정리'
subtitle: 'python'
categories: python
comments: false
order: 4
---

### math.prod()
math.prod()메서드는 주어진 iterable에서 요소의 곱을 반환한다   

> math.prod(iterable, start)   

```python
import math

array = [2, 3, 4]

# array의 모든 데이터 곱한다
product = math.prod(array)

print(product)  # 24
```

```python
import math

array = [2, 3, 4]
n = 2
math.prod(map(lambda x: x//n, array))  # 2
```

<br>
### math.ceil() 과 math.floor

- math.ceil(x): 소수를 올림하여 정수로 반환하는 함수
- math.floor(x): 소수를 내림하여 정수로 반환하는 함수

> math.prod(iterable, start)   

```python
import math

array = [2, 3, 4]

# array의 모든 데이터 곱한다
product = math.prod(array)

print(product)  # 24
```

```python
import math

array = [2, 3, 4]
n = 2
math.prod(map(lambda x: x//n, array))  # 2
```
