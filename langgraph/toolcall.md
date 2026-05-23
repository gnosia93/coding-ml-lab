1. ToolCall 객체의 표준 타입 스펙 (파이썬)
langchain_core.messages.tool 모듈에 정의된 ToolCall 구조는 다음과 같습니다. 딱 4가지 필드로만 구성된 매우 심플한 명세서입니다.
from typing import TypedDict, Any, Dict, Optional

class ToolCall(TypedDict):
    name: str               # 1. 실행할 도구(함수)의 이름
    args: Dict[str, Any]    # 2. 함수에 넘겨줄 인자(파라미터) 딕셔너리
    id: Optional[str]       # 3. LLM이 발급한 고유 요청 ID (비동기/병렬 매칭용)
    type: str               # 4. 객체의 타입 정보 (항상 "tool_call" 고정)
