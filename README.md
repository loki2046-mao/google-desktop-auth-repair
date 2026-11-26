# Google Desktop Auth Repair

面向 macOS Gemini 与 Antigravity 的 Agent Skill：根据失败阶段选择修复方法，保留备份、回滚和真实对话验收。

## 从真实故障整理而来

官方渠道购买会员后，Gemini Mac 仍然发一句就报错。作者借助 Codex 完成备份、干净初始化、首次使用确认，再恢复原配置，三个账号均恢复对话。此前还处理过 Antigravity 模型进程未继承代理的问题。

这份 Skill 把这些经验拆成可执行分支。Gemini 的精确后端根因尚未证实，不将有效修复流程当成官方根因结论。

| 分支 | 历史结果 |
|---|---|
| Gemini 已登录但对话报错：初始化后恢复原配置 | 三账号对话通过 |
| Antigravity 未继承代理：带代理环境完整重启 | 原生对话返回 OK；普通图标重开不保证继承 |
| Antigravity 旧请求失败、新会话复测 | 不改配置连续返回 OK、OK2，仅证明当前恢复 |
| Antigravity CLI 模型目录查询超时 | 注入现有代理后目录返回，未验证CLI生成 |
| Gemini OAuth 交换超时：按应用代理排查 | 配置通过，历史登录验收缺失 |

地区相关错误另有[账号关联地区、Play国家、付款资料与家庭组的核验及更正指南](references/region-eligibility.md)，包含官方入口、操作步骤和变更后验收；不承诺通过改地区绕过资格。

## 安装到 Codex

需要具备本机文件、终端与界面操作能力的 Agent。单纯把文字发给聊天机器人不会自动修复应用。

```sh
git clone https://github.com/loki2046-mao/google-desktop-auth-repair.git ~/.codex/skills/google-desktop-auth-repair
```

目标目录若已存在，请先检查现有内容，不覆盖私人配置。安装后在新的会话中确认 Skill 可用。

直接说「Gemini 登录不了」「Antigravity 登录失败」或「Gemini 一发消息就报错」，支持自动选择 Skill 的客户端可以按描述匹配；也可显式调用 `$google-desktop-auth-repair`。是否自动加载取决于客户端。

## 执行边界

- 重置前备份，失败时回滚；不删除钥匙串或混合认证文件。
- 密码、验证码与身份验证由用户完成；新增权限、条款和系统扩展按工具规则确认。
- 不关闭 TLS 验证，不承诺解除地区、年龄、组织或会员资格限制。
- 成功标准是真实 App 回复且重启复测通过；不保证所有付费权益或永久不复发。
- 当前是经过结构校验的流程型 Skill，不是后台常驻软件，也不会安装后自行更改网络。

详细操作从 [SKILL.md](SKILL.md) 按需进入对应参考。非 Google 官方项目。MIT License。
