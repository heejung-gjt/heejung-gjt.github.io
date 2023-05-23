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

디버깅 모드로 실행하면 ModuleNotFound, ImportError와 관련된 에러가 발생할때가 있다. 이럴땐 디버깅 json 파일을 들여다 보자

- 기본 launch.json 파일
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File",
            "type": "python",
            "request": "launch",
            "subProcess": true,
            "program": "${file}",
            "console": "integratedTerminal"
        },
    ]
}
```
디버깅을 처음 실행하면 launch.json 파일이 위와 같이 만들어진다. 
이때 봐야하는건 program 키값이다. program은 디버깅 대상 프로그램을 지정한다. 현재 편집 중인 파일의 경로로 대체되며, 디버거는 해당 파일을 실행하고 디버깅 작업을 수행한다. 즉 현재 내가 켜놓고 있는 파일이 실행된다고 생각하면 된다.   
현재 파일의 경로를 동적으로 설정하여 디버깅 하기 편리하지만 현재 작업 디렉토리나 PYTHONPATH에 없는 모듈을 import하려면 문제가 발생한다
그리고 시작점으로 가서 항상 디버깅 모드를 켜야 하는 불편함이 있다.

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File",
            "type": "python",
            "request": "launch",
            "subProcess": true,
            "program": "${workspaceFolder}/scheduler/main.py",
            "console": "integratedTerminal"
        },
    ]
}
```

${workspaceFolder} 는 실제 작업 공간 폴더의 경로로 치환된다 그렇기 때문에 ```${workspaceFolder}/scheduler/main.py```은 작업 공간 폴더 내의 litestagram_app/main.py 파일을 프로그램으로 지정하는 것을 의미한다. 즉 어떤 파일이 켜져 있던 main.py에서 디버깅이 시작된다.

디버깅 모드를 돌리려면 위와 같이 시작되는 main 지점을 써주자

--------------

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