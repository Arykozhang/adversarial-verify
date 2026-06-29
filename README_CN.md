<div align="center">
  <h1>⚔️ Adversarial Verify · 对抗验证</h1>

  <p><strong>把项目打包，发给对抗AI找到你习以为常但实际上是瓶颈的设计选择。</strong></p>

  <p>
    <a href="https://github.com/Arykozhang/adversarial-verify/stargazers">
      <img src="https://img.shields.io/github/stars/Arykozhang/adversarial-verify?style=for-the-badge&color=FFD700&labelColor=050b1f&logo=github" alt="Stars" />
    </a>
    <a href="LICENSE">
      <img src="https://img.shields.io/github/license/Arykozhang/adversarial-verify?style=for-the-badge&color=blue&labelColor=050b1f" alt="MIT License" />
    </a>
    <img src="https://img.shields.io/badge/Claude%20Code-skill-D97757?style=for-the-badge&labelColor=050b1f" alt="Claude Code Skill" />
    <img src="https://img.shields.io/github/last-commit/Arykozhang/adversarial-verify?style=for-the-badge&color=22c55e&labelColor=050b1f&logo=git&logoColor=white" alt="Last commit" />
  </p>

  <p>
    <a href="README.md">English</a>
  </p>
</div>

---

## 这是什么？

一个 **Claude Code Skill**。当你卡在某个瓶颈——分数提不上去、方案有盲区、不知道从哪里改进——它帮你把整个项目打包成对抗AI能理解的材料包，并生成结构化的攻击任务书。

不是"提几个优化建议"。是让另一个AI用**"我不同意你的做法"**的心态，从4个方向进行系统性诊断，输出 P0/P1/P2 分级执行清单——每个缺陷附具体文件引用和验收标准。

## 和普通AI咨询的区别

| 普通问法 | 对抗验证 |
|---------|---------|
| 描述问题 → AI给建议 | 打包完整项目 → AI做结构化审计 |
| AI不知道你试过什么 | 你告诉它：AST方案已废弃（避免它再建议AST） |
| 收到泛泛的建议 | 收到 P0/P1/P2 分级缺陷 + 文件引用 + 修复方案 |
| 自己判断修没修好 | 每项修复配验收标准，修完就知道 |

## 安装

```bash
git clone https://github.com/Arykozhang/adversarial-verify.git \
  ~/.claude/skills/adversarial-verify
```

Claude Code 中输入：

```
/adversarial-verify
```

触发词：`对抗验证` · `找AI评审` · `四轮验证` · `adversarial verify` · `魔鬼代言人`

## 工作流

```
  ╔══════════════════════════════════════════════════════╗
  ║                  对 抗 验 证 流 水 线                ║
  ╠══════════════════════════════════════════════════════╣
  ║                                                      ║
  ║  Step 1        Step 2        Step 3–4      Step 5    ║
  ║  ┌──────┐     ┌──────┐     ┌──────────┐   ┌──────┐   ║
  ║  │确认  │ ──▶ │组装  │ ──▶ │ 生成任务 │──▶│发送  │   ║
  ║  │范围  │     │材料包│     │ 书+文案  │   │给对抗│   ║
  ║  │对标  │     │≤50份│     │          │   │ AI   │   ║
  ║  └──────┘     └──────┘     └──────────┘   └──────┘   ║
  ║                                      │               ║
  ║                                      ▼               ║
  ║  Step 6                  Step 7          ┌──────┐    ║
  ║  ┌──────────────┐      ┌──────────┐     │对抗AI│    ║
  ║  │P0立即修/P1   │ ◀─── │干扰清理  │ ◀── │回复  │    ║
  ║  │排期/P2记录   │      │清除残留  │     │审计  │    ║
  ║  └──────────────┘      └──────────┘     └──────┘    ║
  ║                                                      ║
  ╚══════════════════════════════════════════════════════╝
```

**Step 1** — 确认项目范围：当前水平、对标基准、已知瓶颈、历史迭代。

**Step 2** — 按 `templates/package-structure.md` 组装 7 类材料包：背景文档、代表样本、核心源码、系统输出、方法论、完整数据集、任务书。扁平目录，不超过 50 份。

**Step 3** — 用 `templates/task-document.md` 生成 4 方向攻击任务书。

**Step 4** — 用 `templates/message-template.md` 生成可直接粘贴给对抗AI的沟通文案。

**Step 5** — 用户上传文件 + 发送文案。

**Step 6** — 用 `templates/execution-command-template.md` 处理对抗AI回复：P0（阻断）→ 立即修 / P1（重要）→ 排期修 / P2（优化）→ 记录。

**Step 7** — **干扰清理**（实战反复验证的关键步骤）：修复部署后清除残留旧文件——旧Agent脚本、过期SystemPrompt、`__pycache__`、废弃CSV——防止旧模块冲突导致 import 报错。

## 实战案例

> 虚构示例，展示完整工作流。所有数据和名称均为虚构。

小张在做「智能Code Review Agent」——自动审查 GitHub PR 并给出修改建议。经过三轮迭代后卡住了：召回率 78%，误报率 35%。有开源项目宣称做到 92% / 12%。

他触发 `/adversarial-verify`，按模板打包了 29 份材料（三轮迭代历史、已废弃的 AST 方案、已知泛型盲区、200 个测试 PR 数据集、当前 Prompt 模板、10 个关键 case 的完整链路），随 4 大攻击方向任务书发给对抗AI。

对抗AI回复了 17 个缺陷。**P0-1**：规则引擎的正则 `<T extends \w+>` 只匹配单层泛型，漏掉了 `<T extends Comparable<T>>` 嵌套泛型——导致约 6% 的漏报。**P0-3**：Prompt 里要求 LLM 检查空指针，但从来没把变量类型信息喂进去——LLM 在瞎猜。

修复后执行 Step 7 清理（删掉旧 `ast_reviewer.py`、清空 `__pycache__`）：召回率从 78% 升到 87%。虽然还没到 92%，但找到了最大的两个盲区。

## 7 类材料包

| 类别 | 内容 | 为什么重要 |
|------|------|-----------|
| 背景 | 项目描述 + 评分标准 + 迭代历史 | 对抗AI没有记忆，必须完整交代 |
| 代表样本 | 3-5 份原始文档 + 提取文本 | 选最有代表性、最难的 |
| 源代码 | 核心逻辑的源文件 | 不是全部代码，是核心路径 |
| 输出样例 | 系统实际运行输出 | 让对抗AI看到"系统实际做了什么" |
| 方法论 | 关键设计决策 | 为什么这么做，试过什么，废弃了什么 |
| 数据集 | 完整题目/样本 | 对抗AI可以对照答案审计 |
| 任务书 | 攻击方向 + 交付要求 | 告诉对抗AI"你要找什么" |

## 文件结构

```
adversarial-verify/
├── SKILL.md                              # 核心指令（Claude Code 加载入口）
├── README.md                             # 英文文档
├── README_CN.md                          # 本文件
├── LICENSE                               # MIT
└── templates/                            # 4 个可复用模板
    ├── package-structure.md              # 材料包结构清单
    ├── task-document.md                  # 对抗AI任务书
    ├── message-template.md               # 沟通文案模板
    └── execution-command-template.md     # 回复执行命令模板
```

## License

MIT — 详见 [LICENSE](LICENSE)。
