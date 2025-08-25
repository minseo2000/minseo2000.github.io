---
title: LangChain
author: 박민서
date: 2025-08-22
category: Langchain
layout: post
mermaid: true
---

# 랭체인, LangChain (Language Chain)
<hr>
Langchain은 애플리케이션에 LLM을 쉽게 추가할 수 있도록하는 프레임워크입니다.

<a href="https://www.langchain.com/">랭체인 공식 홈페이지 바로가기</a><br>
<a href="https://github.com/langchain-ai/langchain">랭체인 공식 깃허브 페이지</a>


## LangChain 기본 사용 방법

<hr>

### ChatOpenAI 클래스를 이용해, 로컬 LLM을 이용해, LLM과 질의응답하기

VLLM으로 내 PC에서 서빙중인 로컬 LLM 모델을 이용해 OpenAi Compatible 한 API를 끌어다가 사용할 수 있다.
나는 Image, Text -> to -> Text 모델인 Qwen2.5-VL 32GB 모델을 이용해 테스트 해보았다.

<br>
* VLLM은 Ubuntu(Linux) 환경에서 가장 호환이 높기 때문에 되도록 우분투 환경에서 테스트하는 것을 추천한다.

<a href="https://docs.vllm.ai/en/latest/">VLLM 공식 홈페이지 바로가기</a> <br>
- VLLM에 대한 자세한 내용은 공식 홈페이지 Docs를 참고하면 자세히 나와있다.

```python
# 로컬 실행
import subprocess

base_cmd = [
            "vllm", "serve", "Qwen/Qwen2.5-VL-32B-Instruct-AWQ",
            "--host", "0.0.0.0",
            "--port", "50001",
            "--tensor-parallel-size", "2",
            "--max-model-len", "50000",
            "--enable-auto-tool-choice",
            "--tool-call-parser", "hermes",
        ]
subprocess.run(base_cmd, check=True)
```
<br>
위 코드를 이용해, 직접 VLLM을 통해서 서빙을 해주었다. 서빙을 해주었다면, 다음 주소를 통해 Swagger API 웹 페이지를 확인할 수 있다. 나는 port=50001로 사용해주었기 때문에 포트번호가 50001이다.

<a href="http://localhost:50001/docs">http://localhost:50001/docs </a>

<br>
확인을 해보았다면, 다음과 같이 ChatOpenAI 클래스를 이용해서, 랭체인에 내 로컬 LLM을 붙여줄 수 있다.
<br>

```python
from langchain_openai import ChatOpenAI
from langchain.prompts import ChatPromptTemplate

llm = ChatOpenAI(
        base_url="http://localhost:50001/v1",
        api_key="설정한 경우에만 입력",
        model="모델은 내가 서빙한 모델과 이름이 일치해야한다. 내 경우는 : Qwen/Qwen2.5-VL-32B-Instruct-AWQ",
        streaming=True,          # 스트리밍 사용
        temperature="모델의 창의성을 설정하는 값",
    )


chat_template =  ChatPromptTemplate(
    [
        ("system", "You are a helpful assistant. Your name is {name}"),
        ("human", "Hello how are you?"),
        ("ai", "I'm doing well, Thanks!"),
        ("human", "{user_input}")
    ]
)

messages = (chat_template.format_messages(name="Minseo", user_input="What is your name?"))

    print(messages)

    print(llm.invoke(messages))
```

- 랭체인은 프롬프트 관리를 쉽게 해주기 위해서 ChatPromptTemplate과 같은 클래스를 제공한다. 이외에도 여러 종류의 프롬프트 클래스가 존재한다.

<br> 
여기까지가 기본적으로 vllm을 통해 로컬 llm 구동 후, langchain을 통해 쉽게 llm과 질의응답을 할 수 있는 환경을 만드는 방법이다.


