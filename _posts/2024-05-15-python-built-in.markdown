---
layout: post
title: 'python 내장 함수'
subtitle: 'python'
categories: python
# tags: asynchronous
comments: false
order: 4
---


python의 내장함수(built-in functions)는 자주 사용되는 함수를 말한다   


### eval()
eval 함수는 매개변수로 받은 식을 문자열로 받아서 실행하는 함수이다.    
즉, 매개변수로 받은 식은 실행 가능한 문자열로 들어와야 한다   

```python
a = '1 + 3 + 4 + 5'
eval(a)  # 13

a = '1 + 3 + '
eval(a)  # 
```

<br>

### 진수 변환 함수

- oct()
    - 10진수 -> 8진수 문자열
    ```python
    a = '10'
    oct(int(a))  # '0o12'
    ```

    - 8진수 -> 10진수
    ```python
    a = '10'
    int(a, 8)
    ```

- hex()
    - 10진수 -> 16진수 문자열
    ```python
    a = '10'
    hex(int(a))  # '0xa'
    ```

    - 16진수 -> 10진수
    ```python
    a = '10'
    int(a, 16)
    ```

- bin()
    - 10진수 -> 2진수 문자열
    ```python
    a = '10'
    bin(int(a))  # '0b1010'
    ```

    - 2진수 -> 10진수
    ```python
    a = '10'
    int(a, 2)
    ```

<br>

### sort()와 sorted()

- sort() 함수는 리스트 객체 자체를 정렬해주는 함수이며 리스트에서만 사용이 가능하다
- reverse옵션 : reverse옵션으로 오름차순, 내림차순 정렬을 해준다
(기본이 오름차순이다)   
- key옵션: 무엇을 기준으로 정렬하는지를 나타내는 키값이다
```python
li = [-1, 1 , -4, 22, -22]
li.sort(reverse=True)  # 내림차순 정렬
li.sort(lambda x: (abs(x-5), x-5)  # x-5를 한 값이 작은 순으로 정렬하고, 같은 경우에는 x-5의 값이 작은 순으로 정렬
```

- ex) ['api', 'app', 'aaa', 'demon', 'carrier'] 를 길이순으로 정렬하고, 사전순(오름차순)으로 정렬하기

```python
li = ['api', 'carrier', 'app', 'aaa', 'demon']
li.sort(key=lambda x: (len(x), x))
```


- sorted(): 정렬된 리스트를 새롭게 반환한다   

<br>

### dict.fromkeys()
딕셔너리 생성할 때 사용하는 메소드이며 seq, value 셋으로 사전을 생성한다   
- seq: 생성하려는 딕셔너리 키의 목록
- value: 생성하려는 딕셔너리 값

```python
seq = 'test'
dic1 = dict.fromkeys(seq)
print(dic1)  # {'t':None, 'e':None, 's':None, 't':None}

dic2 = dict.fromkeys(seq, 0)
print(dic1)  # {'t':0, 'e':0, 's':0, 't':0}
```

<br>

### copy()와 deepcopy()
값을 복사하는 기능으로 얕은 복사와 깊은 복사 방식이 있다. 해당 함수를 사용할때는 mutable한 객체를 복사할 때 유의하면 된다   
- copy(): 얕은 복사   
    - 객체를 복사할때 해당 객체만 복사하여 새 객체를 생성한다. 복사된 객체의 인스턴스 변수는 __원본 객체의 인스턴스 변수와 같은 메모리 주소를 참조__ 하기 때문에 해당 메모리
    주소의 값이 변경되면 원본 객체 및 복사 객체의 변인스턴스 수값은 같이 변경된다   

    ```python
    a = [[1, 2], 3]
    b = a  ## b = a.copy

    id(a)  # 4310711488
    id(b)  # 4310743808

    id(a[0])  # 4310761536
    id(b[0])  # 4310761536
    ```

- deepcopy(): 깊은 복사   
    - 객체를 복사할때 해당 객체와 인스턴스 변수까지 복사하는 방식이기 때문에 참조를 공유하지 않는다    

    ```python
    import copy
    a = [[1, 2], 3]
    b = copy.deepcopy(a)  ## b = a.copy

    id(a)  # 4307747120
    id(b)  # 4307747152

    id(a[0])  # 4307747056
    id(b[0])  # 4307747088
    ```

<br><br>

[내장 함수 참고 docs](https://docs.python.org/ko/3/library/functions.html)


