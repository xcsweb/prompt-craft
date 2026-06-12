# Prompt Craft v2.1 架构说明

> 基于 GitHub 生态调研（30k+ stars Prompt-Engineering-Guide / 139k stars system-prompts-and-models-of-ai-tools / 66.9k stars microsoft/ai-agents-for-beginners）提炼。
> 目标：让用户**只说"做一个大屏"或"帮我写一个登录页"**，Skill 自动注入专家级 System Prompt + 项目级规则 + 任务级约束。

---

## 核心设计理念：三层 Prompt 架构

```
┌──────────────────────────────────────────────────────────────────┐
│  T1 — 项目级规则（永久生效，写入 .cursor/rules/ 或 copilot-instructions.md）│
│  每项目一份，定义全局不变量：技术栈、架构风格、安全要求、编码约定      │
├──────────────────────────────────────────────────────────────────┤
│  T2 — 模板/场景级（本 Skill 核心资产）                              │
│  选择场景（大屏/后台/API/Chatbot）后自动注入的 Expert Prompt       │
│  包含：角色定义、技术栈约束、功能清单、代码风格、反模式清单           │
├──────────────────────────────────────────────────────────────────┤
│  T3 — 请求级（用户描述的具体任务）                                   │
│  CO-STAR 六要素：Context / Objective / Style / Tone /              │
│            Audience / Response Format                              │
└──────────────────────────────────────────────────────────────────┘
```

## 使用流程（用户视角）

```
用户："帮我做一个数据可视化大屏，柱状图+折线图+饼图，支持响应式"
          │
          ▼
  Step 1. 场景识别 → 激活"前端-数据可视化大屏"模板
  Step 2. 档位询问 → "🚀 快速原型 还是 🏭 生产级？"（默认快速原型）
  Step 3. 技术栈确认 → "用 Vue + ECharts / React + Recharts？"（如有偏好可选）
  Step 4. 注入完整 Prompt → 【T1 项目级规则】+【T2 场景专家 Prompt】+【T3 CO-STAR 任务描述】
  Step 5. 生成代码 → 按输出协议（文件结构 + 代码 + 注释 + 摘要）交付
          │
          ▼
  用户可继续迭代（"加一个地图", "换深色主题", "导出功能"）
```

## 本次 v2.1 更新包含的文件

```
/workspace/prompt-craft/
├── references/
│   └── T1_PROJECT_RULES.md         # 项目级规则模板（可复制到 .cursor/rules/）
├── project-rules/                  # T1 可复用规则库（用户直接拷贝使用）
│   ├── nextjs-shadcn.md            # Next.js + shadcn/ui 项目规则
│   ├── vite-vue3-typescript.md     # Vite + Vue 3 + TS 项目规则
│   ├── node-express-prisma.md      # Node.js + Express + Prisma 后端规则
│   └── shared-security.md          # 共享安全规则（SHARED_SECURITY_RULES）
├── task-templates/                 # T3 任务级 CO-STAR 风格模板
│   ├── build-dashboard.md          # "生成一个大屏" 任务模板
│   ├── build-login-form.md         # "生成一个登录页" 任务模板
│   ├── build-crud-api.md           # "生成一个 CRUD API" 任务模板
│   ├── debug-error.md              # "帮我调试" 任务模板
│   ├── code-review.md              # "帮我代码审查" 任务模板
│   └── write-tests.md              # "为某代码写测试" 任务模板
└── expert-personas/                # T2 专家角色定义（System Prompt 骨架）
    ├── senior-frontend-engineer.md
    ├── senior-backend-engineer.md
    ├── senior-fullstack-engineer.md
    └── senior-ai-agent-developer.md
```

## 为什么选择 CO-STAR + XML 作为核心语法

| 框架 | 优势 | 在本 Skill 中的使用 |
|------|------|-------------------|
| **CO-STAR 六要素** | 业界验证有效的 prompt 骨架，确保信息不遗漏 | T3 任务级模板的主骨架 |
| **Claude XML 标签** | 已证明在 Claude / GPT-4o 系列上降低幻觉率 30%+ | 所有 T2 专家角色的 system prompt 默认使用 XML 语法 |
| **Stripe 五段式** | 将 System / Context / Task / Output / Constraints 明确分隔 | 所有代码生成请求遵循五段式 |

## Prompt 效果评测方法

每次使用 Skill 后，可自问以下 5 个问题评估输出质量：

1. **完整性**：是否生成了可独立运行的完整代码（无缺失 import、无 placeholder）？
2. **一致性**：变量命名、组件结构、错误处理是否遵循了模板中的约定？
3. **可维护性**：代码是否易于二次修改（注释清晰、拆分合理、单一职责）？
4. **安全性**：是否包含基本安全防护（输入验证、XSS 防护、无硬编码密钥）？
5. **可验证性**：是否输出了"如何验证这段代码工作正常"的说明？

---

## 参考资源（调研来源）

1. dair-ai/Prompt-Engineering-Guide (30k+ ⭐) — Prompt 工程术语体系
2. x1xhlol/system-prompts-and-models-of-ai-tools (139k ⭐) — Cursor/v0/Claude Code 真实 System Prompt 参考
3. microsoft/ai-agents-for-beginners (66.9k ⭐) — Agent 设计模式
4. CO-STAR Framework — 六要素 Prompt 骨架
5. ZapDev 分层 Prompt 架构 — SHARED_RULES + 框架级规则
6. code2prompt — 仓库级上下文打包思路
