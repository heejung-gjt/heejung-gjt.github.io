---
layout: post
title: '절대경로와 상대경로 (w. debugging 임포트 에러)'
subtitle: '경로 설명'
categories: python
tags: know
---


파이썬을 이용해 파일을 불러오기 위해서는 import를 진행한다. 이때 파일의 위치 경로를 입력해줘야 하는데 절대 경로와 상대 경로를 이용할 수 있다.   

---

- 절대경로: 모든 경로를 입력해준다
  - ex) /Users/sonheejeong/Desktop/src/project_name   

- 상대경로: 특정한 경로를 기준으로 다른 경로를 표시한다   
  - ex) ./sub_directory/ex.py
    - .(dot)은 현재 파일이 위치한 디렉토리 경로를 말한다
    - ..의 경우에는 현재 디렉토리의 상위 디렉토리를 말한다   

상대경로를 사용하는 이유는 최초 디렉토리가 다른 os에서 모두 작동해야 하는 경우 os별로 디렉토리를 작성해야 하는 불편함이 있을때 사용한다.

### 임포트 에러 발생
<img width="222" style="margin-left:0" alt="스크린샷 2023-05-08 오후 11 05 52" src="https://user-images.githubusercontent.com/64240637/236845476-d28cf7f3-6b28-4603-942b-6e214df86f30.png">

디버깅 모드로 실행하면 ModuleNotFound, ImportError와 관련된 에러가 발생했다.
이를 해결 할 수 있는 방법이 2가지 있다.   

- from api.proj import get_method
    - 임포트 에러 발생
- from scheduler.api.proj import get_method
    - 임포트 에러 발생

> 디버깅 launch.json파일에 env 파일 위치 지정해주는 경우

```python
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File",
            "type": "python",
            "request": "launch",
            "program": "${workspaceFolder}/scheduler/main.py",
            "console": "integratedTerminal",
            "justMyCode": true,
            "env": { "PYTHONPATH": "${workspaceRoot}"},
        }
    ]
}
```
- from api.proj import get_method
    - 임포트 에러 발생
- from scheduler.api.proj import get_method
    - 정상적 임포트

---

> path경로에 root경로 넣어준 경우    

```shell
$ Users/sonheejeong/Desktop/src/project_folder
```

launch.json에 env를 설정한 뒤 디버깅 모드로 실행시키는 경우
ImportError가 발생하는 경우가 있다. 이때 path에 root경로를 넣어주었다

- from api.proj import get_method
    - 정상적 임포트
- from scheduler.api.proj import get_method
    - 임포트 에러 발생

env도 설정하고, path에 root경로도 넣어주면 디버깅 모드일때 임포트 에러가 발생하지 않는다

---


#### 경로 넣어주는 2가지 방법

~~~shell
- $ PATH=$PATH:/Users/sonheejeong/Desktop/src/project_folder
- $ sudo vi /etc/paths # 접속후 경로 추가 (터미널 껐다가 켜야 적용된다)
~~~

---

#### 디버깅 설정 파일 속성
[https://code.visualstudio.com/docs/editor/debugging](https://code.visualstudio.com/docs/editor/debugging)

```
type - 실행구성에 사용할 디버거 유형(python..)
request - 실행구성의 요청유형 현재는 'launch', 'attach` 입니다.
name - 디버그 런치 이름
program - 디버거를 시작할 때 실행할 실행 파일 또는 파일
args - 디버깅을 위해 프로그램에 전달된 인수
env - 환경 변수
cwd - 종속성 및 기타 파일을 찾기 위한 현재 작업 디렉토리
port - 실행 중인 프로세스에 연결할 때의 포트
stopOnEntry - 프로그램이 실행되면 즉시 중단
console - 사용할 콘솔의 종류(예: internalConsole, integratedTerminal또는)externalTerminal
```