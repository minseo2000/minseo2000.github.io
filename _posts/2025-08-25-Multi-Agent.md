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
from langgraph.prebuilt import create_react_agent
from dotenv import load_dotenv
from get_llm import make_llm
import os

load_dotenv()
MODEL_ID = os.getenv("MODEL_ID", "Qwen/Qwen2.5-VL-32B-Instruct-AWQ")


class AgentTemplate:

    def __init__(self):
        # 도구 만들기
        # MCP 서버로 만들지 결정하기
        # 메모리 탑재 해주기

        # 여기에 에이전트 생성하기
        self.agent = create_react_agent(
            llm=make_llm(),
            model=MODEL_ID,
        )


if __name__ == "__main__":
    
    test_agent = AgentTemplate()
    print(test_agent.agent.invoke("테스트입니다"))



```

## 배포/퍼블리싱 에이전트 구현

<hr>


