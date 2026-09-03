# mingdao-ticket-general

Claude Code skill：用 hap-cli 在 mingdao.com **服务管理›工单反馈** 提交产品工单（BUG / 需求建议 / 使用咨询）。

## 安装

```bash
git clone https://github.com/Oscarw212/mingdao-ticket-general.git ~/.claude/skills/mingdao-ticket-general
```

或者直接把 `SKILL.md` 放到 `~/.claude/skills/mingdao-ticket-general/SKILL.md`。

## 前置条件

- hap-cli ≥ 0.8.25（`hap --version`）
- `hap auth login` 登录 **mingdao.com**，`hap auth whoami` 确认 Current Org 是 **MH & MD**

## 用法

在 Claude Code 里说「帮我开个工单 / 报 bug / 提需求」即可触发。Claude 会先给出标题 + 描述 draft，你确认后才会真正创建工单。

默认值：
- 提交人 = 当前登录用户
- 公司 = 上海万企明道软件有限公司
- 反馈用户 / 邮箱 / 我的明道顾问 = 表单默认，不填
