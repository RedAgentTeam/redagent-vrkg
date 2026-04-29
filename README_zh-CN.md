# redagent-vrkg

**Verifiable Retrieval Knowledge Graph** — 面向 AI Agent 的人类自治理、可验证锚定知识资产系统。

> 当 AI Agent 能够验证、复用、更新自身知识库而不产生重复 LLM 调用时，智能成为一种**公共资源**——而非消耗品。

---

## 问题背景

大语言模型会幻觉。RAG（检索增强生成）有所帮助，但存在根本性缺陷：

| 问题 | 传统 RAG | redagent-vrkg |
|------|----------|---------------|
| **知识所有权** | 模型掌握它知道的知识 | 人类掌握知识源头 |
| **可验证性** | 无法重新核查 | 每个知识单元自带验证方法 |
| **知识过时** | 静默退化 | 显式过期信号 |
| **成本** | 每次查询都消耗 token | 检索零 token 消耗 |
| **意图保留** | 被摘要化，原意丢失 | 原文存储，意义不走形 |

核心问题：**RAG 优化的是检索速度；我们优化的是知识可信度。**

---

## 系统架构

```
原始采样 → 证据锁定 → Gene/Capsule 固化 → 查询检索
     ↓
  通过可执行命令验证
```

### 核心概念

#### Gene（知识基因）

原子单元。一个 Gene = **知识事实 + 验证方法**，打包存储。

```json
{
  "gene_id": "example_001",
  "name": "MiniMax 官方能力定义",
  "description": "锁定 MiniMax 官方定位：AGI 研发、大模型/多模态/Agent/长上下文",
  "validate_command": "curl -L --max-time 20 https://www.minimax.io",
  "validate_output": "HTML 返回完整首页，品牌定位、核心技术、产品矩阵内容无缺失",
  "confidence": 1.0,
  "evidence_level": "原文 + 实测"
}
```

#### Capsule（可执行知识胶囊）

触发信号 + 可执行步骤 + 用途 的打包。Capsule 告诉系统**何时使用**以及**如何使用**。

```json
{
  "capsule_id": "minimax_official_capsule_001",
  "trigger_signal": "大模型服务商选型、多模态业务规划、企业 AI 评估",
  "executable_steps": [
    {
      "step_id": 1,
      "step_description": "通过 curl 获取官方首页原始内容",
      "executable_code": "curl -L --max-time 20 https://www.minimax.io"
    }
  ],
  "purpose": "AI 选型 SOP、智能体架构参考"
}
```

#### 可信度评分

每个知识单元都带有明确的置信度分数：

| 分数 | 含义 | 使用建议 |
|------|------|---------|
| 1.0 | 原文 + 实测验证 | 作为主要知识来源 |
| 0.8–0.9 | 原文，未实测，来源权威 | 作为辅助参考 |
| 0.5–0.7 | 第三方来源 | 需要验证 |
| < 0.5 | 大量加工处理 | 不适合写入 Gene/Capsule |

---

## 与现有方案对比

| 特性 | Naive RAG | Vector RAG | redagent-vrkg |
|------|-----------|-----------|---------------|
| 每次查询 token 消耗 | ~500–2000 | ~500–2000 | **~50–100** |
| 幻觉风险 | 高 | 中 | **低** |
| 知识过时处理 | 静默 | 静默 | **显式信号** |
| 人类参与 | 无 | 无 | **全流程 curation** |
| 可验证性 | ❌ | ❌ | **✅ 可执行验证** |
| 原始意图保留 | ❌ | ❌ | **✅ 原文存储** |

---

## 核心设计原则

1. **知识即公共资源**：知识经过一次整理，无限复用，不产生重复 token 消耗。
2. **信任前先验证**：每个知识单元自带验证方法。
3. **人类参与 curation**：无自动摄入，每条知识都经过审核和签字确认。
4. **置信度感知检索**：查询引擎遵循置信度评分，高风险答案需要高置信度来源。
5. **意图保留**：存储原文而非摘要，意义不随时间降解。

---

## 文件结构

```
redagent-vrkg/
├── README.md              ← 本文件
├── SOP.md                 ← 知识采样与整理标准操作流程
├── GENE-FORMAT.md         ← Gene 格式规范
├── CAPSULE-FORMAT.md      ← Capsule 格式规范
├── CONCEPT-DICTIONARY.md  ← 核心概念定义
├── REPORTS/               ← 已发布的采样报告与验证记录
└── ASSETS/               ← 图表与截图
```

---

## 动机

这套系统的诞生源于需求，而非理论。

> "我们一开始不是要做学术框架。我们只是需要 AI 不要在我们的私有知识上产生幻觉。所以我们建了一套系统，让 AI 能记住它知道什么、怎么知道的、以及是否还能相信它。"

本系统目前由 **Red Agent Team** 在生产环境中实际使用，管理 3800+ 知识单元，覆盖 AI 运维、工程决策和项目历史。

---

## 出版物

详见 `REPORTS/` 目录中的已发布采样报告与验证记录。

---

## 引用格式

```bibtex
@misc{redagentvrkg2026,
  title = {redagent-vrkg: A Verification-Anchored Knowledge Asset System for AI Agents},
  author = {Red Agent Team},
  year = {2026},
  url = {https://github.com/yesimagine-oss/redagent-vrkg}
}
```

---

## 许可证

MIT License — 自由使用，开放贡献。

---

**维护者**：Red Agent Team  
**联系**：yesimagineoss (at) gmail.com  
**仓库**：https://github.com/yesimagine-oss/redagent-vrkg
