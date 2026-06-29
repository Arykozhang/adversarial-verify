# Adversarial Verify · 对抗验证

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-skill-D97757?style=flat-square)](https://claude.ai/code)

**把系统/方案/代码打包，发给独立的对抗AI做深度诊断。** 不是"提几个优化建议"——是让另一个AI用"我不同意你的做法"的心态，找到你习以为常但实际上是瓶颈的设计选择。

## 这是什么？

一个 Claude Code Skill。当你卡在某个瓶颈——分数提不上去、方案有盲区、不知道从哪里改进——它帮你把整个项目打包成对抗AI能理解的材料包，并生成结构化的攻击任务书。

跟普通的"问AI意见"不同：
- 普通问法：把问题描述一下，AI给几个建议
- 对抗验证：把项目历史、迭代过程、失败案例、核心代码、输出样例**全部打包**，再指定攻击方向，让AI做系统性诊断

## 快速开始

```bash
git clone https://github.com/Arykozhang/adversarial-verify.git \
  ~/.claude/skills/adversarial-verify
```

然后在 Claude Code 中输入：

```
/adversarial-verify
```

触发词：`对抗验证`、`找AI评审`、`四轮验证`、`adversarial verify`、`魔鬼代言人`

## 工作流

1. **确认项目范围** — 当前水平、对标基准、已知瓶颈
2. **按模板组装材料包** — 7类材料（背景/样本/源码/输出/方法论/数据/任务书），扁平目录，不超过50份文件
3. **生成任务书** — 4个攻击方向 + 具体交付要求
4. **生成沟通文案** — 可直接粘贴到对抗AI对话框
5. **打包交付** — 用户上传文件 + 发送文案
6. **处理回复** — P0（阻断）→ 立即修 / P1（重要）→ 排期修 / P2（优化）→ 记录

## 为什么需要完整打包？

对抗AI没有你之前对话的记忆。如果你只说"帮我改进这个系统"，它只能泛泛而谈。但如果你告诉它：
- 你试过AST方案后来废弃了（避免它再建议AST）
- 你在泛型场景下漏报率特别高（它可以直接切入）
- 你的LLM prompt里缺了什么信息（它能一眼看出盲区）

——它就能给出**你真正需要的诊断**，而不是重复你已经知道的建议。

## 文件结构

```
adversarial-verify/
├── SKILL.md                              # 核心指令（Claude Code 加载入口）
├── README.md                             # 本文件
├── LICENSE
└── templates/                            # 4个可复用模板
    ├── package-structure.md              # 材料包结构清单（7类20项）
    ├── task-document.md                  # 对抗AI任务书模板
    ├── message-template.md               # 沟通文案模板
    └── execution-command-template.md     # 回复执行命令模板
```

## License

MIT — 详见 [LICENSE](LICENSE)。
