# mingdao-ticket-general

Claude Code skill：用 hap-cli 在 mingdao.com **服务管理›工单反馈** 提交产品工单（BUG / 需求建议 / 使用咨询）。

## 安装

Claude Code：

```bash
git clone https://github.com/Oscarw212/mingdao-ticket-general.git ~/.claude/skills/mingdao-ticket-general
```

Codex CLI（同一份 SKILL.md，Codex 也支持 Agent Skills 格式）：

```bash
git clone https://github.com/Oscarw212/mingdao-ticket-general.git ~/.codex/skills/mingdao-ticket-general
```

## 前置条件

- hap-cli ≥ 0.8.25（`hap --version`）
- `hap auth login` 登录 **mingdao.com**，`hap auth whoami` 确认 Current Org 是 **MH & MD**

## 用法

把截图 / 附件丢给 agent，简单一两句描述问题，说「帮我开个工单」即可。agent 会自己整理成标题 + 描述（BUG 类会补齐复现步骤 / 预期 vs 实际），先给你看 draft，你确认后才真正创建工单。

默认值：
- 提交人 = 当前登录用户
- 公司 = 上海万企明道软件有限公司
- 反馈用户 / 邮箱 / 我的明道顾问 = 表单默认，不填
