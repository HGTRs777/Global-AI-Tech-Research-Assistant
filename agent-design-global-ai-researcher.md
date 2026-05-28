# 🧠 全球AI技术研究助手 (Global AI Tech Research Assistant)

> **Nexent Discussion · Show and Tell**
>
> 基于 Nexent + SiliconFlow + ModelScope MCP + Exa.ai 四引擎打造的
> 中英双语全栈AI技术研究智能体

---

## 📌 一句话简介

> 输入一个技术问题或研究方向，自动对中英文技术生态进行全景搜索、深度分析，输出结构化研究报告。

---

## 🎯 为什么要做这个智能体？

当前 AI 技术发展日新月异，但研究者面临三大痛点：

| 痛点 | 描述 |
|------|------|
| **信息孤岛** | 中文技术社区（CSDN/知乎/公众号）与国际社区（arXiv/GitHub/Hacker News）信息割裂 |
| **工具碎片** | 需要同时打开多个搜索引擎、代码仓库、论文网站，切换成本高 |
| **深度不足** | 传统搜索只能返回链接列表，无法进行跨源综合分析和结构化输出 |

这个智能体通过 **Exa.ai（国际）+ ModelScope MCP（国内）+ SiliconFlow（推理）+ Claude Code（工程化）** 四引擎协同，一步到位解决上述问题。

---

## 🏗️ 技术架构

```
┌──────────────────────────────────────────────────────────┐
│                  Nexent Agent Runtime                     │
│                                                          │
│  ┌────────────────────────────────────────────────────┐  │
│  │              用户输入（自然语言）                      │  │
│  │   "帮我调研一下Flex:AI和vLLM在GPU虚拟化上的差异"       │  │
│  └─────────────────────┬──────────────────────────────┘  │
│                        ▼                                  │
│  ┌────────────────────────────────────────────────────┐  │
│  │              Planner（任务规划）                      │  │
│  │   1. 拆解关键词 → 中/英文分别搜索                     │  │
│  │   2. 收集论文、代码仓库、技术博客                      │  │
│  │   3. 交叉验证信息                                     │  │
│  │   4. 生成结构化对比报告                               │  │
│  └─────────────────────┬──────────────────────────────┘  │
│           ┌────────────┼────────────┐                    │
│           ▼            ▼            ▼                    │
│  ┌──────────┐ ┌──────────┐ ┌──────────────┐            │
│  │  Exa.ai  │ │ModelScope│ │ SiliconFlow  │            │
│  │   MCP    │ │   MCP     │ │    Models    │            │
│  │          │ │           │ │              │            │
│  │· search  │ │· 秘塔搜索  │ │· DeepSeekV3 │            │
│  │· answer  │ │· Fetch    │ │· Qwen-VL    │            │
│  │· deep    │ │· Trends   │ │· BGE-M3     │            │
│  │· code    │ │· Markdown │ │              │            │
│  └────┬─────┘ └─────┬─────┘ └──────┬───────┘           │
│       │              │              │                    │
│       ▼              ▼              ▼                    │
│  ┌────────────────────────────────────────────────────┐  │
│  │            Critic（质量审查）                         │  │
│  │   · 信息溯源验证  · 时效性检查  · 结构化输出          │  │
│  └─────────────────────┬──────────────────────────────┘  │
│                        ▼                                  │
│  ┌────────────────────────────────────────────────────┐  │
│  │            结构化研究报告                             │  │
│  │   · 摘要  · 技术对比表  · 代码示例  · 参考文献       │  │
│  └────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────┘
```

---

## ⚙️ 模型配置

在 Nexent 中按以下方式配置三种模型（均通过 SiliconFlow API）：

### LLM（大语言模型 — 决策大脑）

| 配置项 | 值 |
|--------|-----|
| **模型** | `deepseek-ai/DeepSeek-V3` |
| **提供商** | SiliconFlow（硅基流动） |
| **API Endpoint** | `https://api.siliconflow.cn/v1/chat/completions` |
| **用途** | 复杂推理、技术分析、报告生成 |

> 备选：`Qwen/Qwen3-235B-A22B` — 适合需要更长上下文的深度文档分析

### Embedding（向量模型 — 知识检索）

| 配置项 | 值 |
|--------|-----|
| **模型** | `BAAI/bge-m3` |
| **提供商** | SiliconFlow |
| **用途** | 知识库文档向量化、语义检索 |

> BGE-M3 支持中英双语，且支持 8192 token 长文本嵌入，非常适合技术文档场景

### VLM（视觉模型 — 图表理解）

| 配置项 | 值 |
|--------|-----|
| **模型** | `Qwen/Qwen2.5-VL-72B-Instruct` |
| **提供商** | SiliconFlow |
| **用途** | 解析论文中的架构图、表格、Benchmark 截图 |

---

## 🔌 MCP 工具配置

### 工具组 A：Exa.ai MCP（国际技术搜索）

```json
{
  "mcpServers": {
    "exa-search": {
      "command": "node",
      "args": ["path/to/exa-mcp-server/build/index.js"],
      "env": {
        "EXA_API_KEY": "<your-exa-api-key>"
      }
    }
  }
}
```

**注册的工具：**
| 工具名 | 功能 | 使用场景 |
|--------|------|----------|
| `exa_search` | AI 语义搜索 | 搜索前沿论文、技术博客、开源项目 |
| `exa_answer` | 直接生成答案 | 快速获取技术定义的精确回答 |
| `exa_deep_search` | 深度研究 | 大型技术主题的全面调研 |
| `exa_code_search` | 代码搜索 | 查找 GitHub 上的实现代码 |

### 工具组 B：ModelScope MCP（中文技术生态）

**MCP Server 1 — 秘塔搜索（Metaso Search）**

| 配置项 | 值 |
|--------|-----|
| **MCP URL** | `https://api.modelscope.cn/mcp/metaso-search/sse` |
| **工具** | `metaso_search`（支持简单/深度/研究三种模式） |
| **用途** | 搜索中文技术文章、知乎讨论、公众号内容 |

**MCP Server 2 — Trends Hub（热点聚合）**

| 配置项 | 值 |
|--------|-----|
| **MCP URL** | `https://api.modelscope.cn/mcp/trends-hub/sse` |
| **工具** | `get_trends` |
| **用途** | 获取 GitHub Trending、知乎热榜、36氪等 20+ 平台技术热点 |

**MCP Server 3 — Markdownify（文档转换）**

| 配置项 | 值 |
|--------|-----|
| **MCP URL** | `https://api.modelscope.cn/mcp/markdownify/sse` |
| **工具** | `convert_to_markdown` |
| **用途** | 将搜索到的 PDF 论文、网页转 Markdown 供 LLM 分析 |

**MCP Server 4 — Fetch MCP（网页抓取）**

| 配置项 | 值 |
|--------|-----|
| **MCP URL** | `https://api.modelscope.cn/mcp/fetch/sse` |
| **工具** | `fetch_url` |
| **用途** | 抓取特定技术文章全文进行深度分析 |

---

## 🗃️ 知识库配置

| 知识库 | 内容 | 文件格式 |
|--------|------|----------|
| **AI基础设施** | Flex:AI、Nexent、DataMate 技术文档 | MD, PDF |
| **模型生态** | DeepSeek、Qwen、NVIDIA 等官方文档 | MD, PDF |
| **学术论文** | 经典 AI Infra 论文（vLLM, SGLang 等） | PDF |

---

## 📝 System Prompt 设计

以下 Prompt 可通过 Nexent 的 NL2Agent 自动生成，也可手动精调：

````markdown
## 角色定义
你是「全球AI技术研究助手」，一个专注于 AI 基础设施
（算力虚拟化、智能体平台、数据处理）领域的技术研究智能体。
你能够同时检索中英文技术生态，进行深度分析并输出结构化研究报告。

## 核心能力
1. **双语搜索**：通过 Exa.ai 搜索国际前沿论文、开源项目、
   技术博客；通过秘塔搜索中文技术社区内容
2. **代码理解**：通过 Exa Code Search 查找相关实现代码，
   并进行解读
3. **文档解析**：将 PDF/网页转换为可读文本，提取关键技术细节
4. **综合分析**：交叉验证中英文信息源，生成对比分析
5. **结构化输出**：以研究报告格式输出，包含摘要、技术对比表、
   代码示例、参考文献

## 工作流程
1. 收到用户问题后，先拆解核心关键词
2. 同时发起 Exa.ai（英文）和 秘塔搜索（中文）查询
3. 对高价值链接使用 Fetch 抓取全文
4. 用 Markdownify 转换非文本格式文档
5. 综合所有信息，使用 DeepSeek-V3 进行深度分析
6. 按结构化模板输出研究报告
7. 附上所有参考来源（标题 + URL）

## 输出模板
### 一、研究摘要
（200字以内概括核心发现）

### 二、技术背景
（相关技术的演进脉络和关键概念）

### 三、核心对比分析
| 维度 | 方案A | 方案B |
|------|-------|-------|
| xxx  | xxx   | xxx   |

### 四、代码示例
（关键实现的代码片段及解读）

### 五、社区生态对比
（GitHub Star、贡献者、社区活跃度）

### 六、结论与建议

### 七、参考文献
- [标题](URL) — 来源

## 约束规则
- 必须标注每条信息的来源链接
- 对时效性敏感的话题，优先返回最近 6 个月的内容
- 如果中英文信息存在矛盾，明确指出并分析可能原因
- 中文回复，但技术术语保留英文原名并附中文解释
- 代码块必须标注语言类型
````

---

## 🎬 使用示例

### 示例 1：技术对比研究

**用户输入：**
> "帮我对比 Flex:AI 和 NVIDIA MIG 在 GPU 虚拟化技术上的差异和优劣"

**智能体执行过程：**

```
🤔 Thought: 需要从三个维度搜索 —
   1. Flex:AI XPU虚拟化技术细节 (中文+英文)
   2. NVIDIA MIG 技术文档 (英文为主)
   3. 第三方对比评测

🔍 并行搜索中...
   [Exa.ai] "Flex:AI GPU virtualization vs NVIDIA MIG comparison"
   [Exa.ai] "NVIDIA MIG technical deep dive architecture"
   [秘塔搜索] "Flex:AI XPU虚拟化 技术原理 切分粒度"
   [秘塔搜索] "Flex:AI vs NVIDIA MIG GPU虚拟化对比"

📄 抓取关键文档...
   [Fetch] https://github.com/ModelEngine-Group/flexai
   [Fetch] https://docs.nvidia.com/datacenter/tesla/mig-user-guide/

✍️ 综合分析中...

📊 输出结构化研究报告
```

**智能体输出（摘要）：**

```markdown
## 一、研究摘要
Flex:AI 和 NVIDIA MIG 均为 GPU 算力细粒度切分方案，但设计理念不同：
MIG 是硬件级隔离（仅支持 A100/H100 等高端卡），切分粒度为实例级；
Flex:AI 是软件定义虚拟化（兼容 NVIDIA + 昇腾），支持 10% 精度切分
和跨节点拉远。对于异构算力混合场景，Flex:AI 的灵活性优于 MIG。
...
```

### 示例 2：开源项目调研

**用户输入：**
> "最近有什么值得关注的 AI Agent 开源框架？对比一下 Dify、Nexent、LangGraph"

### 示例 3：论文解读

**用户输入：**
> "帮我找一下关于 GPU 显存优化的最新论文，并做一个 5 分钟的快速解读"

---

## 🚀 部署方式

### 前提条件

| 资源 | 获取方式 |
|------|----------|
| **SiliconFlow API Key** | https://cloud.siliconflow.cn → 注册即送额度 |
| **Exa.ai API Key** | https://exa.ai → 注册获取 |
| **ModelScope MCP** | https://modelscope.cn/mcp → 免费使用 |
| **Nexent 平台** | `git clone https://github.com/ModelEngine-Group/nexent` |

### 部署步骤

```bash
# 1. 部署 Nexent（Docker 方式，2C6G 即可）
git clone https://github.com/ModelEngine-Group/nexent.git
cd nexent/docker
cp .env.example .env
# 编辑 .env 填入 SiliconFlow API Key
bash deploy.sh

# 2. 访问 Nexent 控制台
# http://localhost:3000

# 3. 在 Nexent 中配置（按本文档依次操作）
#    模型配置 → MCP 工具配置 → 知识库上传 → 生成智能体
```

### 在 Nexent 中的操作顺序

```
1. 模型接入 → 填入 SiliconFlow API Key → 测试连通性
2. MCP 工具 → 依次添加 Exa.ai + 秘塔搜索 + Fetch + Markdownify
3. 知识库   → 上传 Flex:AI/Nexent/DataMate 文档
4. 生成智能体 → 粘贴 System Prompt → 点击「生成智能体」
5. 调试     → 发送测试问题 → 观察工具调用链
6. 发布     → 发布到智能体市场供同学使用
```

---

## 🔮 扩展方向

| 方向 | 描述 | 所需 MCP |
|------|------|----------|
| **论文翻译** | 自动翻译英文论文摘要为中文 | SiliconFlow 翻译能力 |
| **代码复现** | 自动 clone 仓库 → 运行 Demo → 输出结果 | Playwright MCP + Terminal MCP |
| **日报生成** | 每日自动扫描技术热点 → 推送日报 | Trends Hub + 定时任务 |
| **视频解读** | 解析技术演讲视频 → 提取要点 | YouTube MCP + Whisper |
| **SQL 分析** | 对接数据库进行量化分析 | XiYan MCP (Text-to-SQL) |

---

## 📎 相关资源

| 资源 | 链接 |
|------|------|
| Nexent GitHub | https://github.com/ModelEngine-Group/nexent |
| Nexent AtomGit | https://atomgit.com/ModelEngine-Group/nexent |
| SiliconFlow 模型广场 | https://cloud.siliconflow.cn/me/models |
| ModelScope MCP 广场 | https://modelscope.cn/mcp |
| Exa.ai | https://exa.ai |
| Flex:AI 项目 | https://gitcode.com/ModelEngine/flexai |
| DataMate 项目 | https://github.com/ModelEngine-Group/DataMate |

---

> 💡 **设计者注**：这个智能体的设计初衷是将"中英文技术生态的信息差"抹平 —
> Exa.ai 覆盖国际前沿，ModelScope MCP 扎根中国技术土壤，
> SiliconFlow 提供认知引擎，Nexent 承载工程落地。
> 四个工具各司其职，形成完整的技术研究闭环。
>
> 欢迎 Star 我们的项目！⭐
