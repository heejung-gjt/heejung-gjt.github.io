---
layout: post
title: 'Call By Assignment (python) '
subtitle: 'call by reference vs call by value in python'
categories: python
---

python에는 Call By Reference 또는 Call By Value라는 개념이 존재하지 않는다. python은 함수에 인수를 전달할때 call by assignment 방식으로 전달되며, 함수의 파라미터로 전달받는 객체의 유형에 따라 참조방식이 결정된다    

<br>

### mutable과 immutable
python에서는 모든 것이 객체이며 크게 2가지의 종류가 있다   
- mutable: 값이 변경 가능한 객체
    - string, int, float, tuple
- immutable: 값 변경이 불가능한 객체
    - list, dictionary, set   

<br>


### mutable객체와 Call By Reference
mutable객체가 함수의 인자로 전달이 되면 값의 원본 주소가 전달되는 Call By Reference처럼 동작된다. 즉 원본값에 영향을 준다
     

```python
b = [1]

def exam(b):
    b.append(2)
    print(b)

exam(b)
>>> [1, 2]
b
>>> [1, 2]
```

- Call By Reference란 ? 
    - 받은 변수의 주소 값을 전달하는 방식          
    불가변 타입의 객체를 전역에 할당하고 함수의 인자로 받았을때 전달받는 b는 원본, 즉 주소값이다.    
    변수의 주소 값으로 타고 들어가서 실제 값 자체를 바꾸기 때문에 원본값이 변경된다.    
    - ex) c++ (Call By Address)
    - 장점: 복사가 아닌 직접 참조이기 때문에 빠르다
    - 단점: 원본값이 변경된다   

<br>


### immutable객체와 Call By Value
immutable객체가 함수의 인자로 전달이 되면 값이 복사되어 전달되는 Call By Value처럼 동작된다. 즉 원본값에는 영향을 주지 않는다

```python
a = 1

def exam(a):
a += 1
print(a)

exam(a)
>>> 2
a
>>> 1
```

- Call By Value란 ?
    - 변수를 복사한 값을 전달하는 방식
    가변 타입의 객체를 전역에 할당하고 함수의 인자로 받았을때 전달받는 a는 주소값이 아닌 복사된 값이기 때문에 실제     
    전역에 있는 a의 값에는 아무런 영향을 주지 않는다. 즉 원본값에는 아무런 영향을 줄 수 없다.        

    - ex) 자바는 모든 기본 타입 변수가 Call By Value방식으로 처리됨    

    - 장점: 복사하여 처리하기 때문에 원본값이 보존    
    - 단점: 메모리 사용량이 늘어남   

<br>


파이썬은 ```call by assignment``` 이다. 어떤 값을 파라미터로 전달하냐에 따라 달라진다.

