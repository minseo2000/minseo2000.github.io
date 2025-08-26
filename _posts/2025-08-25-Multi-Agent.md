---
title: Multi Agent System
author: 박민서
date: 2025-08-25
category: AI
layout: post
---

# Multi Agent System

- 멀티 에이전트 시스템을 직접 만들어보려고 한다. 주제는 다음과 같다.
<br>

"블로그 글 자동 등록기"

<br> 
이를 위해 만들어야 할 에이전트 리스트를 ChatGPT에게 질문해보았다.

| 하위 에이전트                    | 주요 역할                    | 구현 아이디어                                  | 산출물                               |
| -------------------------- | ------------------------ | ---------------------------------------- | --------------------------------- |
| **주제/키워드 조사 에이전트**         | 인기 키워드와 트렌드 조사, 연관 주제 발굴 | Google Trends, 키워드 툴 API, 경쟁 블로그 분석      | “유럽 환율 전망”, “독일 유학생 보험” 같은 주제 리스트 |
| **정보 수집 & 리서치 에이전트**       | 웹/뉴스/위키 등에서 자료 수집 및 요약   | WebScraper + Summarizer, 뉴스/위키 API       | 참고 기사 요약, 최신 데이터 정리               |
| **구조 설계(아웃라인) 에이전트**       | 글의 흐름과 목차 설계             | “서론–본론–결론” 템플릿 자동 생성                     | 글 스켈레톤(소제목, 목차)                   |
| **콘텐츠 작성 에이전트**            | 아웃라인 기반으로 본문 작성          | LLM 기반 작성, 스타일(설명체/스토리텔링) 선택             | 초안 블로그 글                          |
| **스타일 & SEO 최적화 에이전트**     | 톤 조정, 키워드 삽입, SEO 구조화    | NLP로 readability 개선, Yoast SEO 분석        | 검색 최적화된 글                         |
| **이미지/멀티미디어 보조 에이전트**      | 글과 어울리는 이미지/자료 자동 추가     | Unsplash API, DALL·E/Stable Diffusion 호출 | 삽입용 이미지, 썸네일                      |
| **교정/품질 보증 에이전트**          | 맞춤법, 문법, 중복 문장 교정        | Grammarly API, 맞춤법 교정기                   | 교정 완료된 텍스트                        |
| **배포/퍼블리싱 에이전트**           | 블로그/플랫폼 업로드, SNS 공유      | WordPress API, Velog/Tistory 연동, SNS API | 실제 게시된 포스트, 공유 로그                 |
| **Fact-checker 에이전트 (선택)** | 잘못된 정보 필터링               | 신뢰도 높은 소스와 대조                            | 검증된 정보만 반영된 글                     |
| **A/B 테스트 에이전트 (선택)**      | 제목·썸네일 성능 비교             | 유저 반응 데이터 분석                             | 클릭률/조회수 높은 버전 선택                  |

다양한 에이전트가 있고, 하나하나씩 구현해보겠다.

## 에이전트 템플릿, Agent Template

<hr>

- 먼저 만들어줄 에이전트에 대한 템플릿을 만들어주었다. 해당 구조를 기반으로 에이전트부터 구성후에, 랭그래프를 통해 이들을 서로 연결해주려고 한다.
- 사용할 에이전트의 종류는 ReAct를 기반으로 동작하는 에이전트를 사용한다.

### React란?
ReAct는 추론과 행동을 대규모언어모델과 결합하는 것으로, 작업을 위해 언어 추론 추적과 행동을 생성하도록 유도하는 방식을 말합니다.<br>
LLM은 다음과 같은 방식으로 동작하게 됩니다.<br>
```text
Thought: I need to search Apple Remote and find the program it was orignally designed to interacct with.
Act: Search[Apple Remote]
Obs: The Apple Remote is a remote control introduced in October 2005 by Apple ... 
```
-> 위 처럼 생각, 행동, 관찰 3단계를 거쳐 동작하는 에이전트 방식을 말합니다.


### Template Code
다음은 ReAct 에이전트를 만들 템플릿 코드입니다.
```python
import os
from dotenv import load_dotenv
from typing import Optional
from pydantic import BaseModel, Field

from langchain.agents import create_react_agent, AgentExecutor, create_structured_chat_agent
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain_core.chat_history import InMemoryChatMessageHistory
from langchain_core.runnables import RunnableWithMessageHistory
from langchain import hub
from langchain_core.tools import tool
from langchain.agents.output_parsers import ReActJsonSingleInputOutputParser
from langchain.schema import AgentAction, AgentFinish
import json

import os
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI

load_dotenv()
BASE_URL = os.getenv("BASE_URL", "")
API_KEY  = os.getenv("API_KEY", "EMPTY")
MODEL_ID = os.getenv("MODEL_ID", "Qwen/Qwen2.5-VL-32B-Instruct-AWQ")

def make_llm(temperature: float = 0.7) -> ChatOpenAI:
    """
    vLLM(OpenAI 호환) 서버에 붙는 ChatOpenAI.
    """
    return ChatOpenAI(
        base_url=BASE_URL,
        api_key=API_KEY,
        model=MODEL_ID,
        streaming=True,          # 스트리밍 사용
        temperature=temperature,
    )

class StrictJsonOutputParser(ReActJsonSingleInputOutputParser):
    def parse(self, text: str):
        try:
            return super().parse(text)
        except Exception:
            if "Action:" in text and "Action Input:" in text:
                try:
                    # 툴 이름 추출
                    tool_line = text.split("Action:", 1)[1].split("\n", 1)[0].strip()
                    tool_name = tool_line.strip()

                    # 입력값 추출
                    after = text.split("Action Input:", 1)[1].strip()
                    if after.startswith("'") or after.startswith('"'):
                        after = after.strip("'\"")

                    action_input = json.loads(after)
                    return AgentAction(tool=tool_name, tool_input=action_input, log=text)

                except Exception as e:
                    raise ValueError(f"❌ JSON 파싱 실패: {e}\n{text}")

            if "Final Answer:" in text:
                answer = text.split("Final Answer:", 1)[1].strip()
                return AgentFinish(return_values={"output": answer}, log=text)

            raise



# -----------------------
# 1. 환경변수 로드
# -----------------------
load_dotenv()
MODEL_ID = os.getenv("MODEL_ID", "Qwen/Qwen2.5-VL-32B-Instruct-AWQ")

import os
from langchain_core.tools import tool, StructuredTool

# -----------------------
# 1. 파일 읽기
# -----------------------
def read_file(path: str) -> str:
    """지정된 파일을 읽습니다.

    Args:
        path: 읽을 파일의 경로
    """
    if not os.path.exists(path):
        return f"❌ File not found: {path}"
    with open(path, "r", encoding="utf-8") as f:
        return f.read()

read_file_tool = StructuredTool.from_function(
    func=read_file,
    name="read_file",
    description="지정된 파일의 내용을 읽습니다."
)


# -----------------------
# 2. 파일 쓰기
# -----------------------
def write_file(path: str, content: str) -> str:
    """지정된 파일에 내용을 씁니다.

    Args:
        path: 작성할 파일 경로
        content: 파일에 쓸 텍스트 내용
    """
    with open(path, "w", encoding="utf-8") as f:
        f.write(content)
    return f"✅ File written: {path}"

write_file_tool = StructuredTool.from_function(
    func=write_file,
    name="write_file",
    description="지정된 파일에 텍스트를 씁니다. (path, content 필요)"
)


# -----------------------
# 3. 파일 삭제
# -----------------------
def delete_file(path: str) -> str:
    """지정된 파일을 삭제합니다.

    Args:
        path: 삭제할 파일 경로
    """
    if not os.path.exists(path):
        return f"❌ File not found: {path}"
    os.remove(path)
    return f"🗑️ File deleted: {path}"

delete_file_tool = StructuredTool.from_function(
    func=delete_file,
    name="delete_file",
    description="지정된 파일을 삭제합니다."
)


# -----------------------
# 4. 툴 리스트
# -----------------------
FILE_TOOLS = [read_file_tool, write_file_tool, delete_file_tool]


# -----------------------
# 5. Agent 클래스 정의
# -----------------------
class UpLoaderAgent:
    def __init__(self, session_id: str = "default_session"):
        self.session_id = session_id
        self.store = {}

        def get_session_history(session_id: str):
            if session_id not in self.store:
                self.store[session_id] = InMemoryChatMessageHistory()
            return self.store[session_id]

        # ✅ 프롬프트 (LangChain ReAct 권장 구조)
        prompt = ChatPromptTemplate.from_messages([
            ("system",
             "You are a helpful AI assistant. You can manage files using these tools:\n\n{tools}\n\n"
             "Follow this process strictly:\n\n"
             "Question: {input}\n"
             "Thought: think step by step\n"
             "Action: one of [{tool_names}]\n"
             "Action Input: MUST be a JSON object matching the tool schema. "
             "For example, to call read_file, use:\n"
             "Action: read_file\n"
             "Action Input: {{\"path\": \"test.txt\"}}\n\n"
             "Do NOT return a string or tuple. Always use JSON.\n"
             "Observation: result of the action\n"
             "Final Answer: the final answer to the user.\n"),
            MessagesPlaceholder("history"),
            ("human", "{input}"),
            ("assistant", "{agent_scratchpad}"),
        ])

        # ✅ ReAct Agent 생성
        react_agent = create_react_agent(
            llm=make_llm(),
            tools=FILE_TOOLS,
            prompt=prompt,
            output_parser=StrictJsonOutputParser()  # 👈 여기!
        )

        # ✅ Executor
        executor = AgentExecutor.from_agent_and_tools(
            agent=react_agent,
            tools=FILE_TOOLS,
            handle_parsing_errors=True,
            verbose=True
        )

        # ✅ 메모리 래핑
        self.agent = RunnableWithMessageHistory(
            executor,
            get_session_history,
            input_messages_key="input",
            history_messages_key="history",
        )

    def run(self, user_input: str):
        return self.agent.invoke(
            {"input": user_input},
            config={"configurable": {"session_id": self.session_id}}
        )


# -----------------------
# 6. 테스트
# -----------------------
if __name__ == "__main__":
    test_agent = UpLoaderAgent(session_id="test_user_1")

    # 파일 쓰기
    resp1 = test_agent.run("test.txt 파일에 플러터 예제 코드 만들어서 써줘")
    print(resp1["output"])

    # 파일 읽기
    resp2 = test_agent.run("test.txt 파일 내용을 읽어줘")
    print(resp2["output"])

    # 파일 삭제
    #resp3 = test_agent.run("test.txt 파일을 삭제해줘")
   # print(resp3["output"])

```

## WorkFLow 만들기 with LangGraph

<hr>

LangGraph로 만든 에이전트를 노드로 만들어, 노드간 엣지를 이어 에이전트끼리 통신 가능하게 만들 수 있다.

```python
from typing import Any, Dict, List
from langgraph.graph import StateGraph, START, END
from langchain_openai import ChatOpenAI
from pydantic import BaseModel, Field
from dotenv import load_dotenv
import os
from agents.file_agent import UpLoaderAgent

load_dotenv()
BASE_URL = os.getenv("BASE_URL", "")
API_KEY = os.getenv("API_KEY", "EMPTY")
MODEL_ID = os.getenv("MODEL_ID", "Qwen/Qwen2.5-VL-32B-Instruct-AWQ")

file_agent = UpLoaderAgent(session_id="test_user_1")

def make_llm(temperature: float = 0.7) -> ChatOpenAI:
    return ChatOpenAI(
        base_url=BASE_URL,
        api_key=API_KEY,
        model=MODEL_ID,
        streaming=True,
        temperature=temperature,
    )

class AgentState(BaseModel):
    user_message: str = Field(default="", description="사용자 입력 원문")
    intent: str = Field(default="", description="사용자의 요청이 분류된 작업 종류 (예: file_agent, writer_agent)")
    task_details: str = Field(default="", description="추가 정보")
    retrieved_docs: List[str] = Field(default_factory=list, description="검색 결과")
    tool_result: Any = Field(default=None, description="도구 실행 결과")
    response: str = Field(default="", description="최종 응답 결과")
    metadata: Dict[str, Any] = Field(default_factory=dict, description="메타데이터")
    approved: bool = False  # ✅ 사용자 승인 여부

"""
Node Definitions
"""

# ------------------------
# intent 분류 노드 (state 업데이트)
# ------------------------
def classify_intent_node(state: AgentState) -> dict:
    llm = make_llm(temperature=0.0)

    prompt = f"""
    사용자의 요청을 보고 어떤 에이전트를 사용할지 결정하세요.
    가능한 값: "file_agent", "writer_agent"

    사용자 입력: {state.user_message}

    반드시 위 값 중 하나만 출력하세요.
    """
    response = llm.invoke(prompt).content.strip()
    print("LLM intent 분류 결과:", response)

    if "file" in response:
        intent = "file_agent"
    elif "writer" in response:
        intent = "writer_agent"
    else:
        intent = "writer_agent"

    # ✅ dict 반환 → state에 병합됨
    return {"intent": intent}

# ------------------------
# 사용자 승인 요청 노드
# ------------------------
def request_user_confirmation(state: AgentState) -> dict:
    print(f"\n⚠️ 에이전트가 제안: '{state.intent}' 작업을 수행하려 합니다.")
    answer = input("이 작업을 진행할까요? (y/n): ")
    if answer.lower().startswith("y"):
        return {"approved": True}
    else:
        return {"approved": False, "response": "❌ 사용자가 작업을 거부했습니다."}

# ------------------------
# 에이전트 실행 노드
# ------------------------
def file_agent_node(state: AgentState):
    if not state.approved:
        return {"response": "파일 작업이 취소되었습니다."}
    response = file_agent.run(state.user_message)
    return {"response": response}

def writer_agent_node(state: AgentState):
    if not state.approved:
        return {"response": "글쓰기 작업이 취소되었습니다."}
    return {"response": "✍️ 글쓰기 에이전트 실행 완료"}

# ------------------------
# 그래프 정의
# ------------------------
def create_graph():
    workflow = StateGraph(AgentState)

    workflow.add_node("classify", classify_intent_node)
    workflow.add_node("ask_approval", request_user_confirmation)
    workflow.add_node("file_agent", file_agent_node)
    workflow.add_node("writer_agent", writer_agent_node)

    # 시작 → intent 분류
    workflow.add_edge(START, "classify")

    # intent 결과에 따라 approval 단계로 이동
    workflow.add_conditional_edges(
        "classify",
        lambda s: s.intent,
        {"file_agent": "ask_approval", "writer_agent": "ask_approval"}
    )

    # 승인 → 에이전트 실행
    workflow.add_conditional_edges(
        "ask_approval",
        lambda s: s.intent,
        {"file_agent": "file_agent", "writer_agent": "writer_agent"}
    )

    workflow.add_edge("file_agent", END)
    workflow.add_edge("writer_agent", END)

    return workflow.compile()

# ------------------------
# 실행
# ------------------------
def main():
    print("-======LangGraph Human In the loop 예제=======")
    app = create_graph()
    result = app.get_graph().draw_mermaid_png()
    with open("./graph.png", "wb") as f:
        f.write(result)
    state = AgentState(user_message="make.md 파일 하나 만들어줘")
    final = app.invoke(state)
    print("\n최종 응답:", final)

    print("\n======워크 플로 종료 =======")

if __name__ == "__main__":
    main()
```
- 테스트를 위해서, 의도 분류를 통해 사용자 요청에 대해 어떤 에이전트를 실행시켜야하는지 만들었다.<br>

다음으로는 에이전트 간 통신을 가능하게 만드는 것!


## 블로그 글 Reviewer Agent 만들기

<hr>



