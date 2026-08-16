<div align="center">

# 知其然 · KnowHow

### AI 苏格拉底式自适应学习助手

*"知其然，知其所以然" —— 不给你答案，引导你自己找到答案。*

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-Dify-6366f1.svg)](https://dify.ai)
[![Model](https://img.shields.io/badge/model-DeepSeek--V4-green.svg)](https://deepseek.com)
[![RAG](https://img.shields.io/badge/RAG-Hybrid_Search-orange.svg)]()

</div>

---

## 项目简介

**知其然**是一个基于大语言模型的苏格拉底式 AI 学习助手，专为自学者设计。

与传统 AI 问答（"问 → 答"）不同，本项目核心理念是：**引导用户通过自主思考得出结论，而不是直接给出答案**。系统通过持续追问，帮助用户建立真正的概念理解，而不是表面的知识记忆。

## 核心功能

| 功能 | 描述 |
|------|------|
| 🎓 苏格拉底式教学 | 每次只问一个引导性问题，禁止直接给出答案 |
| 👨‍🏫 多教师热插拔 | 支持运行中随时切换教师风格（吴恩达 / 李沐） |
| 📚 RAG 知识检索 | 基于上传的教材知识库进行有依据的引导 |
| 🔄 复习模式 | 以考查性提问检验用户是否真正掌握 |
| 🧠 对话记忆 | 保持 10 轮对话上下文，支持连续深入引导 |

## 系统架构

```
用户输入
    │
    ▼
问题分类器（deepseek-v4-flash）
    ├── 选择/切换老师 ──→ 教师管理 Agent ──→ 变量赋值（current_teacher）
    │
    ├── 正常学习提问
    │       ├── [已选老师] ──→ 知识检索（RAG）──→ 教学 LLM ──→ 回复
    │       └── [未选老师] ──→ 提示选择老师
    │
    └── 复习已学内容
            ├── [已选老师] ──→ 知识检索（RAG）──→ 复习 LLM ──→ 回复
            └── [未选老师] ──→ 提示选择老师
```

## 当前支持的教师

| 教师 | 风格 | 专业方向 |
|------|------|---------|
| 吴恩达（Andrew Ng） | 温和鼓励，从直觉和类比出发 | 机器学习、RAG、AI基础原理 |
| 李沐 | 直接务实，强调动手实践 | 深度学习、神经网络、PyTorch |

## 知识库内容

本项目使用以下教材构建 RAG 知识库（混合检索 + Rerank）：

- 吴恩达机器学习个人笔记
- RAG 原理与应用详解
- Agent 架构与设计

## 技术栈

- **平台**：Dify Chatflow
- **主要模型**：deepseek-v4-flash（教学推理）、deepseek-chat（教师识别）
- **Embedding**：text-embedding-v3
- **Rerank**：qwen3-rerank
- **检索策略**：混合检索（语义 + 关键词）+ Rerank 精排
- **会话状态**：Dify 会话变量（current_teacher）

## 快速部署

### 方式一：导入 Dify DSL（推荐）

1. 登录你的 [Dify](https://dify.ai) 账号
2. 进入「工作室」→「创建应用」→「导入 DSL 文件」
3. 上传 `workflow/知其然.yml`
4. 在「知识库」中上传 `knowledge_base/` 目录下的文档
5. 配置模型（deepseek-v4-flash / deepseek-chat）
6. 发布并使用

### 方式二：参考架构自行搭建

参考 `docs/架构设计.md` 了解各节点详细配置。

## 项目演示

> 演示视频 / 截图见 `docs/demo/` 目录

**示例对话（吴恩达老师模式）：**

```
用户：我想学 RAG
AI：好问题！在我们开始之前，我想先问你——你觉得，
    一个已经学过大量知识的 AI，为什么还需要"查资料"？

用户：因为它知道的都是公开的网络数据，不知道私人的信息
AI：很准！那我追问你：如果 AI 不知道这些私有数据，
    当用户问到相关问题时，它通常会怎么做？
```

## 已知局限与复赛规划

| 限制 | 当前状态 | 未来规划 |
|------|---------|---------|
| 教师风格差异度 | 依赖 Prompt，差异有限 | Few-shot 示例注入 / LoRA 微调 |
| 学习记录持久化 | 仅限单次会话 | 引入外部数据库存储学习轨迹 |
| 教师动态扩展 | 需手动修改提示词 | 教师档案知识库 + 动态变量注入 |
| 技术实现层 | Dify 低代码平台 | LangChain / LangGraph 代码重写 |


## License

MIT License — 欢迎 Fork 和二次开发
