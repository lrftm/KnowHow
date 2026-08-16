# Workflow DSL 说明

## 什么是 DSL 文件？

DSL（Domain Specific Language）文件是 Dify 工作流的导出格式（YAML），包含了完整的节点配置、连接关系和提示词。

## 如何导入？

1. 登录 [Dify](https://dify.ai)
2. 「工作室」→「创建应用」→ 右上角选择「导入 DSL」
3. 上传本目录下的 `知其然.yml` 文件
4. 导入后需要重新配置：
   - 模型提供商和 API Key
   - 知识库（重新上传 `knowledge_base/` 下的文档）
   - 会话变量 `current_teacher`（String 类型，在会话变量面板添加）

## 节点说明

| 节点名 | 类型 | 作用 |
|--------|------|------|
| 用户输入 | Start | 接收用户输入，含 current_teacher 会话变量 |
| 问题分类器 | Classifier | 三分类：选师 / 学习 / 复习 |
| SERACH_TEACHER | LLM | 识别用户想选择的老师 |
| 代码执行 | Code | Python 关键词提取老师名字 |
| 变量赋值 | Assign | 写入 current_teacher 会话变量 |
| 条件分支 | If/Else | 判断是否已选老师 |
| 知识检索 | Retrieval | RAG 知识库检索 |
| LLM（教学） | LLM | 苏格拉底式教学主 Agent |
| 知识温习 | LLM | 复习模式 Agent |

## 注意事项

- 导入后 DSL 中的模型配置可能需要重新选择（取决于你的 Dify 账号已配置的模型）
- 知识库需要重新上传并在知识检索节点中重新绑定
