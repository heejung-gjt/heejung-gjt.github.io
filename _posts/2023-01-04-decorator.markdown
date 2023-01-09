---
layout: post
title: '데코레이터 (Decorator)의 사용'
subtitle: '230104'
categories: python
tags: know
comments: false
---

데코레이터는 함수에 추가적인 기능을 붙이는 역할을 한다고 생각하면 된다. 데코레이터를 사용한다고 성능이 좋아지는건 아니지만, 
적절히 데코레이터를 사용하는 경우 중요 코드를 관리하기 쉬워지고 가독성이 좋아지는 장점이 있다. 

아래 예시는 test라는 함수에 @decorator를 선언하였다. 여기서 decorator는 test함수에서 반환된 값에 1000을 더해주는 역할을 한다.

```python

def decorator(func):
    def wrapper(*args, **kwargs):
        res = func(*args, **kwargs)
        res += 1000

        return res

    return wrapper


@decorator
def test(a, b):
    return a + b


res = test(1, 6)
print(res)
```

<br>

실제로 프로젝트에서 데코레이터가 어떻게 사용되었는지 보자   
간단히 상황설명을 하자면, 파이썬으로 만들어진 스케줄러가 있다. 해당 스케줄러는 어떠한 오류가 발생해도 종료되면 안되고 계속 실행되어야 했다. 가끔 디비 커넥션 하는쪽에 에러가 발생하면서 스케줄러가 종료되는 이슈가 있었다. 이때 아래처럼 진행되도록 로직을 수정하였다  

- ```디비 커넥션 오류 발생 시 5초뒤 재 커넥션```


```python
def conn_decorator(func):
    def wrapper(*args, **kwargs):

        """
        디비 커넥션이 정상적으로 이루어지지 않을 경우 커넥션 재시도 진행하는 데코레이터
        """
        conn_res = None

        while True:  # 디비 커넥션이 정상적으로 진행될 때까지 connection 시도
            if conn_res:
                break

            try:
                conn_res = func(*args, **kwargs)  # connect_db() 실행

            except Exception as e:
                _LOGGER.error(
                    f"Failed DB connection. Try Again...."
                )
                time.sleep(5)  # 커넥션 실패 시 5초동안 sleep 상태 유지

        return conn_res

    return wrapper

@conn_decorator
def connect_db():
    client = psycopg2.connect(
        host=self.host,
        port=self.port,
        dbname=self.db_name,
        user=self.db_user_name,
        password=self.db_user_passwd,
        connect_timeout=self.connect_timeout,
    )
    self.client.autocommit = True

    return self.client
```

이렇게 커넥션을 재시도 하는 기능을 데코레이터로 빼고 나니까 커넥션 하는 주요 로직만 볼 수 있었다.  
