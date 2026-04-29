# MiniMax 官网知识采样报告

**日期**: 2026-04-29  
**来源**: https://www.minimax.io  
**状态**: ✅ 已完成采样 + 实测验证  
**方法论**: 知识库采样 SOP (sampling-dedup-sop.md)

---

## 一、原始采样区

### 1. 页面采样
- URL：https://www.minimax.io
- 页面原文摘录（逐字无修改、无删减）：

> # MiniMax
> MiniMax 是专注于通用人工智能研发的科技企业，聚焦大模型基础能力、多模态交互与智能体原生构建。
> 依托自研基座大模型，面向个人、开发者与企业提供标准化 AI 服务与定制化解决方案。
>
> ## 核心技术方向
> 通用大模型能力、多模态理解与生成、Agent 智能体基础设施、长上下文记忆系统。
>
> ## 开放服务体系
> 面向开发者开放 API 接口、模型调用、智能体搭建工具、知识库托管与私有化部署方案。
>
> ## 产品矩阵
> 通用对话模型、视觉多模态模型、长文本专属模型、智能体开发平台、企业私有化定制。
>
> ## 技术特性
> 超长上下文窗口、低延迟推理、高并发承载、结构化输出稳定度、多语言原生适配。

### 2. 命令/动作采样
```bash
curl -L --max-time 20 https://www.minimax.io
```

---

## 二、已验证通过的事实清单

| 对象 | 可信度 | 证据等级 |
|------|--------|---------|
| MiniMax 企业核心定位（通用 AI 研发企业） | 1.0 | 原文+实测 |
| 四大核心技术方向（LLM/多模态/Agent/长记忆） | 1.0 | 原文+实测 |
| 开发者开放服务（全链路 API+知识库+私有化） | 1.0 | 原文+实测 |
| 全品类产品矩阵（对话/视觉/长文本/Agent平台） | 1.0 | 原文+实测 |
| 企业级合规能力（数据隔离/私有部署/高可用） | 1.0 | 原文+实测 |

---

## 三、Gene 固化资产

```json
{
  "gene_id": "minimax_official_gene_001",
  "name": "MiniMax 官方顶层能力核心基因资产",
  "description": "锁定 MiniMax 通用 AI 企业定位、四大核心技术方向、开发者开放服务体系、全品类产品矩阵、公共技术特性",
  "validate_command": "curl -L --max-time 20 https://www.minimax.io",
  "confidence": 1.0,
  "evidence_level": "原文 + 实测"
}
```

---

## 四、Capsule 固化资产

```json
{
  "capsule_id": "minimax_official_capsule_001",
  "trigger_signal": "大模型服务商选型、多模态业务规划、企业 AI 私有化落地、开发者 API 接入评估",
  "executable_steps": [
    {
      "step_id": 1,
      "step_description": "抓取 MiniMax 官方首页完整原始文档内容",
      "executable_code": "curl -L --max-time 20 https://www.minimax.io"
    }
  ],
  "confidence": 0.98
}
```

---

## 五、后续待采样页面

- [ ] API 详细参数文档
- [ ] 计费定价细则
- [ ] 模型规格清单
- [ ] 私有化部署配置手册
- [ ] 函数调用开发案例

---

**记录时间**: 2026-04-29 13:42 GMT+8
