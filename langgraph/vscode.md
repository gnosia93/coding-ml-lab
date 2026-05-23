

## uv 환경 디버깅 설정 ##
최근 파이썬 생태계에서 가장 핫한 패키지 매니저인 **uv**를 사용하여 실행하는 애플리케이션을 VS Code에서 디버깅하는 것은 아주 간단하면서도 강력합니다.
uv는 내부적으로 표준 가상환경(.venv)을 생성하여 관리하기 때문에, VS Code가 이 가상환경을 바라보게 설정하거나 launch.json 설정을 조금만 만져주면 중단점(Breakpoint)을 찍고 완벽하게 디버깅할 수 있습니다.

### 방법 1: VS Code 내장 디버거 인터프리터 지정 ###

uv run은 프로젝트 루트 폴더에 .venv라는 이름의 가상환경을 자동으로 만듭니다. VS Code의 파이썬 인터프리터를 이 가상환경으로 지정하면, 일반 스크립트를 디버깅하듯 F5 키만 눌러서 바로 디버깅할 수 있습니다.
	
* 인터프리터 선택 창 열기: VS Code에서 Cmd + Shift + P (윈도우는 Ctrl + Shift + P)를 눌러 명령 팔레트를 엽니다.
* 검색 및 선택: Python: Select Interpreter를 입력하고 선택합니다.
* uv 가상환경 지정: 목록에서 ./venv/bin/python (또는 프로젝트 이름이 적힌 uv 환경)을 선택합니다.
* 디버깅 시작: 디버깅하고 싶은 파일(예: tools.py)을 열고 코드 왼쪽에 빨간색 중단점(Breakpoint)을 찍은 뒤, **F5**를 누르고 **"Python File"**을 선택하면 즉시 디버깅이 시작됩니다.


### 방법 2: launch.json 파일에 uv run 등록하기 (공식적인 방법) ###
디버깅할 때 무조건 uv run 명령어를 거쳐서 실행되도록 명시적인 설정을 해두고 싶다면 VS Code의 디버그 설정 파일을 만드는 것이 가장 좋습니다.
* VS Code 왼쪽의 실행 및 디버그(Run and Debug) 탭(벌레 모양 아이콘)을 클릭합니다.
* "create a launch.json file" 링크를 클릭한 후, 환경으로 Python ➡️ Python File을 선택합니다.
* 생성된 .vscode/launch.json 파일의 내용을 아래와 같이 수정합니다.
```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: UV Run Debug",
            "type": "debugpy",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "python": "${workspaceFolder}/.venv/bin/python" 
        }
    ]
}
```

python 항목에 현재 프로젝트 내부의 uv 가상환경 경로(${workspaceFolder}/.venv/bin/python)를 지정해 주는 것입니다. 이렇게 하면 VS Code 디버거가 uv run과 완벽히 동일한 환경의 파이썬 엔진을 실행하여 디버깅을 시작합니다.
이 설정을 마친 후, 디버깅하고 싶은 코드에 마우스를 올려 올려 빨간 점을 찍고 F5를 누르면 uv 환경 내에서 코드가 멈추며 변수 추적, 콜 스택 확인 등을 자유롭게 하실 수 있습니다!
