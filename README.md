# 🎬 电影推荐智能体 (Movie-Recommendation-Agent)

## 📖 项目背景 (Project Background)

在当今的流媒体时代，电影推荐系统在提升用户体验方面扮演着至关重要的角色。传统推荐系统（如协同过滤）往往缺乏对用户自然语言复杂意图的理解，且容易陷入“信息茧房”。本项目旨在解决海量内容下用户难以发现心仪影视作品的痛点，通过引入**大语言模型 (LLM)** 与 **Agentic Workflow（智能体工作流）**，打造一个真正能“听懂人话”、具备独立思考能力的赛博影评人。

* **海量内容的发现痛点 (The Content Discovery Pain-point)**
  现代流媒体平台拥有庞大的资源库，但用户常常面临“不知道看什么”的窘境。传统的标签化搜索无法处理类似 *“我想看2010年以后、关于火星移民且评分极高的浪漫喜剧”* 这种复杂长尾需求。
  
* **从“推荐算法”到“推荐智能体”的进化 (Evolution to Agentic Recommender)**
  本项目摒弃了传统的死板推荐逻辑，赋予 AI 主动调用工具、分析数据并进行多轮对话的能力。它不仅能精准推荐，还能为你提供情绪价值和深度的电影赏析。

---

## 🧠 核心智能体架构 (Core Agent Architecture)

本项目不再是单一的推荐算法，而是基于 **ReAct (Reasoning and Acting)** 范式构建的智能体系统。核心工作流如下：

1. **大语言模型“大脑” (LLM-Driven Brain)**
   依托 Qwen/GPT 等强大的大语言模型，利用 Function Calling（函数调用）机制，精准提取用户的多维度观影意图（年份、流派、核心剧情、评分门槛）。
   
2. **混合检索增强生成 (Hybrid RAG Engine)**
   为了彻底消除大模型的“幻觉 (Hallucination)”，底层挂载了双路混合检索器：
   * **语义检索 (Semantic Search):** 使用 `SentenceTransformer` 将电影简介转化为高维向量，计算余弦相似度，精准捕捉用户的“抽象剧情需求”。
   * **关键词检索 (Keyword Search):** 引入 `BM25` 统计学算法进行查漏补缺，确保导演名、特定人名等专有名词被绝对命中。
   * 结合两者分数进行归一化重排 (Hybrid Reranking)，输出 Top 5 真实电影事实。

---

## 🧰 多工具编排与未来规划 (Multi-Tool Orchestration & Roadmap)

为了让 Agent 具备真正的“规划 (Planning)”与“纠错”能力，系统正在集成和扩展以下多工具网络：

### ✅ 已实现功能 (Currently Implemented)
* **`search_movies_tool` (本地事实基座):** 优先在本地清洗过的高质量电影数据库 (DataFrame) 中进行混合检索，保证响应速度与 Token 成本的极度优化。
* **`search_external_api_tool` (全网 Fallback 兜底):** 当用户的条件极其苛刻导致本地无数据时，Agent 会**自主决策**连线 TMDB (The Movie Database) 全网数据库进行实时 API 检索，突破本地数据边界。

### 🚧 规划中的进阶模块 (Upcoming Features)
* **动态长期记忆网络 (Stateful Memory Management)**
  引入 `update_user_profile_tool`。Agent 能在多轮对话中自动捕捉并记录用户的长线偏好（如“讨厌血腥片”、“钟爱诺兰导演”），形成个性化 User Profile，实现跨会话的精准推荐。
* **电影大师课模式 (The Masterclass Mode)**
  * **影史脉络定位:** 调用专用的历史分析 Prompt 链路，梳理电影在电影史中的地位及同类型佳作对照。
  * **视听语言赏析:** 计划接入视频平台 API，向用户主动推荐关于该电影的专业拉片解析和视听语言分析视频，将单纯的“推荐”升华为“电影艺术学习”。

---

## 🛠️ 安装与运行指南 (Installation & Usage)

### 环境依赖 (Dependencies)
- Python 3.11
- Pandas, NumPy, Scikit-learn (数据处理与向量计算)
- OpenAI Python SDK (用于调用大模型 API 及 Function Calling)
- Sentence-Transformers, rank_bm25 (用于构建双路混合检索索引)
- Requests (用于请求 TMDB 等外部 API)

### 快速启动 (Quick Start)
1. 配置好相关的 API Key (`SiliconFlow / OpenAI`, `TMDB`)。
2. 运行 `data_prep.py`生成 `movies_cleaned.csv` 与 `movie_embeddings.pkl`（也可直接获取，本repo母目录中即有提供）。
3. 实例化 `MovieAgent` 类并开始对话：
   ```python
   agent = MovieAgent()
   agent.chat("我想看一部视觉特效震撼的赛博朋克电影！")
