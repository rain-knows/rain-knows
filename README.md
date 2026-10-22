### Yunpeng Zhang

上海，信息系统专业本科在读。做的项目基本是同一类：LLM 理解意图并组织解释，金额、权限、状态与数据范围交给服务端确定性代码和数据库约束。

[rainknows.dev](https://rainknows.dev) · [Repositories](https://github.com/rain-knows?tab=repositories)

---

## 项目

**[CostGraph](https://github.com/rain-knows/costgraph)** — 制造业成本分析。沿真实采购、工艺事件与领用关系做批次级成本卷积，配一个可恢复的 LangGraph 运行时：队列、租约、Checkpoint、SSE 续传。
`Python` `FastAPI` `LangGraph` `PostgreSQL`

**[pi-orb](https://github.com/rain-knows/pi-orb)** — 桌面 AI 助手。Electron 悬浮球接入聊天、截图、桌面操作与后台任务，带完整权限生命周期。
`TypeScript` `Electron` `Pi SDK`

**[RAG Notebook](https://github.com/rain-knows/RAG-notebook)** — 带来源引用的个人知识库。笔记、检索问答、间隔重复与写作辅助。
`Python` `FastAPI` `LangChain` `ChromaDB`

**[Get My Page](https://github.com/rain-knows/get_my_page)** — 全栈博客平台。SSR/SSG、Meilisearch 全文检索、Caffeine→Redis→MySQL 三级缓存、全容器化。
`Next.js` `Spring Boot` `Redis` `Meilisearch`

**[pi-dotfiles](https://github.com/rain-knows/pi-dotfiles)** — pi coding agent 配置与 `prompt-architect` 扩展，按能力门控的系统提示词分层。
`JavaScript` `PowerShell`

---

## 一条链路怎么切分

以 CostGraph 为例。模型只占一个格子，其余都是确定性代码：

```mermaid
flowchart LR
    UI["React / Vite"] --> API["FastAPI<br/>HTTP · SSE"]
    API --> Gate["主体 · 数据范围"]
    Gate --> Q[("Run 队列")]
    Q --> W["Worker"]
    W --> G["LLM<br/>意图 · 槽位 · 澄清"]
    W --> D["确定性服务<br/>校验 · 卷积 · 舍入"]
    D --> Snap[("已发布快照")]
    Snap --> API
    classDef llm fill:#f6f8fa,stroke:#57606a,stroke-width:1px,color:#57606a
    classDef det fill:#ffffff,stroke:#1f2328,stroke-width:2px
    class G llm
    class D det
```

金额、权限、批次关系、状态机与舍入由服务端代码和数据库约束负责，模型的输出不是事实来源。产出始终能追溯回源数据、工具调用和显式假设。
