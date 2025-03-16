# AI Agent
- [AI Agent](#ai-agent)
  - [Concept](#concept)
  - [Tech Framework](#tech-framework)
    - [XAgent](#xagent)
      - [1. 概况](#1-概况)
      - [2. 团队](#2-团队)
  - [Research](#research)
  - [other](#other)
  - [langchain](#langchain)
  - [RAG](#rag)

## Concept
- llm based agent 智能体/代理

> LLM（Large Language Model）的Agent是指利用大型语言模型构建的智能体。这类智能体主要依赖于LLM强大的语
言理解和生成能力，以实现对用户输入的理解、思考和回答。LLM Agent通常包含以下几个部分：
> 1. PromptTemplate：用于指导LLM进行任务处理的模板，包括任务的输入、中间步骤、可用工具等信息。
> 2. LLM：接收PromptTemplate和用户输入，进行理解和生成反应。
> 3. OutputParser：解析LLM生成的输出，提取有用的信息。
> 4. AgentExecutor：负责执行LLM生成的行动，并获取新的观察结果。
> LLM Agent通过接收用户输入和之前的步骤信息，利用LLM的理解和生成能力进行思考和决策，然后执行行动并解决问题。这个过程会循环进行，直到LLM返回AgentFinish，表示任务完成。
> 在实际应用中，LLM Agent可以用于构建各种智能对话系统、智能助手等，以实现对用户问题的理解和回答。

## Tech Framework
### [XAgent](https://github.com/OpenBMB/XAgent/blob/main/README_ZH.md)
#### 1. 概况
XAgent是一个开源的基于大型语言模型（LLM）的自主智能体，可以自动解决各种任务。 它被设计为一个通用的智能体，可以应用于各种任务。目前，XAgent仍处于早期阶段，我们正在努力改进它。 🏆 我们的目标是创建一个可以解决任何给定任务的超级智能智能体！
#### 2. 团队
openBMB/tsinghua

- XAgent: 一个开源的基于大型语言模型（LLM）的自主智能体，可以自动解决各种任务。 它被设计为一个通用的智能体，可以应用于各种任务。目前，XAgent仍处于早期阶段，我们正在努力改进它。 🏆 我们的目标是创建一个可以解决任何给定任务的超级智能智能体！

- Langchain：它提供了一系列抽象，包括agent、memory、vector store和tools，利用这些抽象来创建AI Agent。其核心算法包括思考（Thought）、行动输入（Action/Action Input）、观察（Observation）等步骤，直至AI Agent认为任务完成。Langchain特别适用于短期任务。

- AutoGPT：这个框架的亮点在于处理长期任务。它的方法是将中间结果存储在向量数据库中，允许AI Agent在长时间内进行复杂的操作。Langchain的长期任务版本也在实验中引入了类似的机制。

- BabyAGI：这个框架同样采用向量数据库存储中间步骤，并将计划分解成一系列步骤，然后一次性生成并执行这些步骤，以此来提高处理复杂问题的能力。

- Generative Agents：这类框架通常关注于使用生成模型来模拟AI Agent的智能行为，它们可以创造性地生成解决方案，适用于需要创新和想象力的任务。

- ModelScope-Agent：ModelScope-Agent 是一个适配开源大语言模型的 AI Agent 开发框架。借助 ModelScope-Agent，所有开发者都可以基于开源 LLM 搭建属于自己的智能体应用，最大限度地释放想象力和创造力。ModelScope-Agent 适用于各种复杂任务，如接入视频生成模型、接入外部软件等。

- chatDev: https://github.com/OpenBMB/ChatDev

## Research
- [The Rise and Potential of Large Language Model
Based Agents: A Survey](https://arxiv.org/pdf/2309.07864.pdf) from Fudan NLP Group
[Paper List](https://github.com/WooooDyy/LLM-Agent-Paper-List)
- [速读版](https://mp.weixin.qq.com/s/eWlms4IxTd4wzka4V7Gu6w)
- [ChatEval: Towards Better LLM-based Evaluators through Multi-Agent Debate](https://arxiv.org/abs/2308.07201)


## other
- [openBMB大模型公开课](https://www.openbmb.cn/community/course)
- [全面超越AutoGPT，面壁智能联合清华 NLP实验室打造大模型“超级英雄”—— XAgent](https://github.com/OpenBMB/XAgent/blob/main/README_ZH.md)
- [【观点】Agent 将是 AI 最大的赛道！](https://mp.weixin.qq.com/s/yoS3qu4ocIZHwaKEAs6-NQ)
- docker vs. docker compose

## langchain

## RAG
[RAG技术入门与实战：基于混元大模型做一个ChatPDF应用](https://km.woa.com/articles/show/592124?from=iSearch）
[探索知识驱动LLM生成：深入解析RAG技术全流程](https://km.woa.com/articles/show/591007?from=iSearch)