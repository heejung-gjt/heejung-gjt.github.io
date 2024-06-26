---
layout: post
title: 'pgcrypto를 이용한 암호화'
subtitle: 'django에서 pgcrypto를 import해서 디비 데이터 암호화'
categories: db
comments: false
order: 1
---

현재 내가 맡고 있는 프로젝트에서는 django-pgcrypto를 이용해 디비에 저장되는 민감한 데이터를 암호화하고 있다.
```shell
pip install django-pgcrypto
```
🔺 [Pypi](https://pypi.org/project/django-pgcrypto/)

특정 필드의 값을 암호화하고 싶을땐 아래와 같은 장고 필드를 이용하면 된다
- pgcrypto.EncryptedTextField()
- pgcrypto.EncryptedDecimalField()
- pgcrypto.EncryptedDateField()   


<br>

장고 패키지를 이용해 데이터를 암호화하는 경우에는 아래와 같이 데이터가 저장된다

- ex) test0303

```text
-----BEGIN PGP MESSAGE-----ZftE/7eXXkWCBbxedCGHfQ===lq5n-----END PGP MESSAGE-----
```

<br>

postgres에서 쿼리로는 아래와 같이 암/복호화를 진행할 수 있다.

- 데이터 암호화

```sql
select armor(encrypt(convert_to('0102293029023', 'utf8'), 'key값','aes'))
```

- 데이터 복호화

```sql
select convert_from(decrypt(dearmor('-----BEGIN PGP MESSAGE-----ZftE/7eXXkWCBbxedCGHfQ===lq5n-----END PGP MESSAGE-----'), 'key값', 'aes'), 'utf-8')
```

<br>

이때 pgcrypto사용에 주의할 점이 있다.
쿼리를 이용해 데이터를 위에처럼 암호화하는 경우 암호문자열 맨끝에 새로운 라인이 생기게 된다
```text
-----BEGIN PGP MESSAGE-----ZftE/7eXXkWCBbxedCGHfQ===lq5n-----END PGP MESSAGE-----

```

ORM의 경우는 새로운 라인 없이 암호화된 문자열만 저장된다. 이렇게 되면 값을 비교할때 문제가 된다. 각각 ORM과 query로 데이터를 암호하하여 insrt한 뒤 아래처럼 값을 select하는 경우 결과 값이 달라지게 된다
```sql
select count(name) from test_table group by name  -- 같은 값이지만 다른 값으로 인식 (줄끝에 새로운 라인이 원인이다)

select count(name) from test_table group by convert_from(decrypt(dearmor(name), 'key값', 'aes'), 'utf-8') -- 실제 복호화하는 경우 똑같은 값이 나오기 때문에 하나의 값으로 인식
```

실제로 위의 이슈 때문에 문제가 있었다 
- unique 걸어놓은 컬럼값에 다른 프로젝트에서 ORM, query로 각각 insert진행하는 경우가 존재했다. 암호화된 문자열의 라인차이 때문에 postgres는 다른 문자열로 인식했고 그대로 2개의 값이 모두 insert되버렸다. 그 결과 get_or_create하는 과정에서 object가 1개 반환되야 하지만 1개 이상 반환되는 이슈로 에러가 발생했다    

해결 방법으로는 query로 문자를 암호화 할 때 라인을 제거해주는 정규 표현식을 사용했다

```sql
regexp_replace(armor(encrypt(convert_to('moneycoon', 'utf8'), 'key값','aes')),E'([\n\r]+$)', '', 'g' )
```

<br>

이런 이슈는 정말 찾기 힘든 부분 중 하나이다. 💦    

pgcrypto를 쓸때는 몇가지 생각해야 한다.

> (1). 장고 ORM 문법 중 인식이 안되는 경우가 존재한다.

 대표적으로 ```contains``` 라는 문법이다. 해당 문법은 필드값에 원하는 값이 들어가 있는지를 필터링 해주는 문법이지만 pgcrypto로 암호화된 데이터는 contains가 먹지 않는다. 그래서 검색 기능과 같이 필터링 해줘야 할때는 직접 로우쿼리를 작성해줬다. (decrypt후 like를 사용해 필터링해줬다) 

<br>

> (2). decrypt보다는 encrypt로 값을 비교하자.

decrypt로 데이터를 복호화 한 후 데이터와 비교하게 되면 복호화하는 시간이 걸리기 때문에 (인덱스도 탈 수 없다..) encrypt로 암호화된 데이터를 비교하는 방법이 실행속도를 줄일수 있는 방법이다 