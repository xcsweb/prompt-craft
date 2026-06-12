# GitHub 搜索策略指南

当内置提示词库无法满足用户需求时，使用以下策略搜索 GitHub 上的优秀 prompt 实践。

## 搜索模板

```
site:github.com "prompt" "{领域关键词}" "{技术栈}" stars:>100
```

## 按领域搜索关键词

| 领域 | 搜索关键词 |
|------|-----------|
| 前端大屏 | dashboard prompt engineering, data visualization prompt, big screen template |
| 企业官网 | landing page prompt, corporate website prompt, next.js landing page |
| 后台管理 | admin dashboard prompt, crud prompt, react admin template |
| REST API | api development prompt, restful api prompt, backend prompt template |
| 微服务 | microservices prompt, distributed system prompt, grpc prompt |
| 数据库 | database schema prompt, sql prompt engineering, prisma prompt |
| 全栈应用 | fullstack prompt, saas development prompt, next.js fullstack |
| 小程序 | wechat mini program prompt, uni-app prompt, taro prompt |
| 数据可视化 | echarts prompt, d3.js prompt, real-time dashboard prompt |

## 评估标准

搜索到项目后，按以下标准评估是否采用：
1. Stars > 500（或近期活跃度高）
2. 包含 `.prompt.md` / `prompts/` / `copilot-instructions.md` 等 prompt 文件
3. README 中有详细的使用说明
4. 最近 6 个月有更新

## 提取方法

1. 读取项目的 prompt 文件内容
2. 分析其结构特点（角色定义、约束条件、输出格式等）
3. 结合用户需求，融合到内置提示词模板中
4. 标注来源，确保可追溯
