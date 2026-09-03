---
name: mingdao-ticket-general
description: 帮用户在 mingdao.com 服务管理›工单反馈 提交产品工单（BUG / 需求建议 / 使用咨询）。当用户说「开工单 / 提工单 / raise a ticket / 报 bug / 提需求」时使用。通用版：提交人自动取当前登录用户，公司固定为上海万企明道软件有限公司，反馈用户/邮箱/我的明道顾问不填、走表单默认。内含字段 ID 和已验证的 API 格式。提交前必须让用户确认。
---

# Mingdao 工单（服务管理›工单反馈）— 通用版

## 铁律
1. **先出 draft，用户说 OK 才能 `record create`**。工单是对外提交的正式记录，未经确认不得创建；误提交后要用 `record delete` 善后。
2. 描述用**简体中文**。
3. 描述格式
   - **BUG**：3W（谁/什么时候/做了什么）+ **复现步骤 + 预期结果 vs 实际结果 + 影响范围**
   - **需求/建议**：3W + 期望方案 + 业务理由（可加需求等级 1–5）
   - **使用咨询**：场景 + 具体问题
4. 示意图要**整张屏幕截图**（平台要求 full screen）。问用户截图文件在哪。

## 前置条件
- 已安装 hap-cli（`hap --version`，需 ≥ 0.8.25），并已 `hap auth login` 登录 **mingdao.com**（Host `https://www.mingdao.com`）。
- 用 `hap auth whoami` 确认：Host 是 mingdao.com、Current Org 是 **MH & MD**（`fe288386-3d26-4eab-b5d2-51eeab82a7f9`）。不对就 `hap auth use` / `hap auth set-current-org` 切换。
- 从 whoami 记下 **Account ID**，用于「提交人」字段。

## 固定 ID
| 项 | ID |
|---|---|
| App 服务管理 | `5d5dd87b-aa27-4b3f-b52c-307ca3e582f0` |
| Worksheet 工单反馈 | `5be531b05d234200011047eb` |
| 公司记录（上海万企明道软件有限公司） | `3f1b493e-1b1d-49b4-a7e9-ed3d37cfa1ac` |

> 公司表里另有一条「上海万企明道软件有限公司（PSH测试中）」(`7e5276fd-…`)，**不要用**。

## 步骤
1. 用户通常只给截图 / 附件 + 一两句问题描述。**不要反复追问**：先读截图和附件，自己推断反馈类型（默认 BUG）、环境（默认 SaaS）、终端（默认 Web），按铁律 3 的格式把标题 + 描述整理成 draft 给用户看，把推断的字段列出来让用户一并确认或修改。信息实在不够才问，一次问完。
2. 选应用（`record create` 没有 `--app-id`）：
   ```bash
   hap app select 5d5dd87b-aa27-4b3f-b52c-307ca3e582f0
   ```
3. 上传截图，拿到 descriptor（fileID / key / serverName / filePath 等）：
   ```bash
   hap upload "FILE.png" --worksheet-id 5be531b05d234200011047eb --app-id 5d5dd87b-aa27-4b3f-b52c-307ca3e582f0
   ```
4. 用户确认 OK 后创建：
   ```bash
   hap worksheet record create 5be531b05d234200011047eb --fields-json '<JSON>'
   ```
   - `--fields-json` 必须是 array：`[{"id":"<fieldId>","value":<原生 JSON>}]`，value 用原生 array/object，**不要 stringify**。
5. 成功 response 里 `autoid` = 工单号，回报给用户。

## 要填的字段（均经 API 验证）
| 字段 | ID | 值格式 |
|---|---|---|
| 反馈类型 | `5eec25be2bd8270001a5de67` | `["BUG"]` / `["需求/建议"]` / `["使用咨询"]`（建单后不可改） |
| 标题 | `5be531b0b0e8f000017e6560` | 一句话概括 |
| 描述 | `5be531b0b0e8f000017e6561` | 见铁律 3 |
| 示意图 | `5be531b0b0e8f000017e6562` | `{"attachments":[{fileID,fileSize,allowDown:true,docVersionID:"",fileName,fileExt,originalFileName,key,serverName,filePath}],"knowledgeAtts":[],"attachmentData":[]}` — 用 upload descriptor；裸 array 会「附件保存失败」 |
| 环境 | `62922451f37febb7cdcc66a8` | `["SaaS"]` / `["私有部署"]` / `["OEM"]` |
| 终端 | `644b4adecd3ee1e7b686ead7` | `["Web"]` / `["App"]` / `["H5"]`（可多选） |
| 提交人 | `5be536f1442bf31568e74384` | `[{"accountId":"<whoami 的 Account ID>"}]`（纯字符串会「服务异常」） |
| 公司 | `6200e4740d3e531977656929` | `["3f1b493e-1b1d-49b4-a7e9-ed3d37cfa1ac"]`（固定） |
| 需求等级 | `64eff4ab0f39a93162efa70d` | 1–5（需求类才填） |

**不要填**（走表单默认）：反馈用户 `658949c8e86fbf3934e8b489`、邮箱 `67f3cef422681a42c1cff554`、我的明道顾问 `629224053d60068c201e3f47`。表单上标必填的 邮箱 / 公司全称或ID 等是外部门户用的，API 创建可跳过。

## 示例 fields-json（BUG）
```json
[
  {"id":"5eec25be2bd8270001a5de67","value":["BUG"]},
  {"id":"5be531b0b0e8f000017e6560","value":"工作表视图切换后筛选条件丢失"},
  {"id":"5be531b0b0e8f000017e6561","value":"【谁/何时/做了什么】…\n【复现步骤】1. … 2. …\n【预期结果】…\n【实际结果】…\n【影响】…"},
  {"id":"5be531b0b0e8f000017e6562","value":{"attachments":[{...upload descriptor...}],"knowledgeAtts":[],"attachmentData":[]}},
  {"id":"62922451f37febb7cdcc66a8","value":["SaaS"]},
  {"id":"644b4adecd3ee1e7b686ead7","value":["Web"]},
  {"id":"5be536f1442bf31568e74384","value":[{"accountId":"<Account ID>"}]},
  {"id":"6200e4740d3e531977656929","value":["3f1b493e-1b1d-49b4-a7e9-ed3d37cfa1ac"]}
]
```

## 善后 / 排错
- 删错单：`hap worksheet record delete 5be531b05d234200011047eb --row-ids <rowid>`
- API 报错看日志：`hap config log level DEBUG` → `~/.hap-cli/hap-cli.log`
- mingdaoSandbox MCP 认证有问题（10001），一律用 hap-cli。
