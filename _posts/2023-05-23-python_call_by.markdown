---
layout: post
title: 'call by reference vs call by value'
subtitle: 'call by reference vs call by value in python'
categories: python
tags: know
---


### call by value
변수를 복사한 값을 전달하는 방식          
가변 타입의 객체를 전역에 할당하고 함수의 인자로 받았을때 전달받는 a는 주소값이 아닌 복사된 값이기 때문에 실제
전역에 있는 a의 값에는 아무런 영향을 주지 않는다. 즉 원본값에는 아무런 영향을 줄 수 없다.
```
a = 1

def exam(a):
    a += 1
    print(a)

exam(a)
>>> 2
a
>>> 1
```

### call by reference
받은 변수의 주소 값을 전달하는 방식          
불가변 타입의 객체를 전역에 할당하고 함수의 인자로 받았을때 전달받는 b는 원본, 즉 주소값이다.
변수의 주소 값으로 타고 들어가서 실제 값 자체를 바꾸기 때문에 원본값이 변경된다.
```
b = [1]

def exam(b):
    b.append(2)
    print(b)

exam(b)
>>> [1, 2]
b
>>> [1, 2]
```



파이썬은 ```call by assignment``` 이다. 어떤 값을 전달하냐에 따라 달라진다.
- immutable 타입의 객체: call by value (ex- int, str)
- mutable 타입의 객체: call by reference (ex- list, dictionary)
