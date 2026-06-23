# SWD 违规词检测 · NapCat 插件

群消息智能审核插件，支持正则/关键词匹配，自动撤回、禁言、踢出、警告。

## 功能

- 🔍 **双模式匹配** — 精确关键词 + 正则表达式
- ⚡ **自定义动作** — 撤回 / 禁言 / 踢出 / 警告，每条规则自由组合
- 📢 **禁言 @ 提醒** — 禁言后自动 @ 用户，消息模板可配
- 👥 **群聊开关** — 白名单 / 黑名单 / 全开，精准控制生效范围
- 🔰 **管理豁免** — 群主和管理员可选跳过检查
- 📥 **TXT 词库导入** — 一行一词，支持 `#` 注释，合并或拆分导入
- 📊 **WebUI 仪表盘** — 实时统计 + 违规日志 + 命中排行
- 📋 **规则管理** — 可视化 CRUD，JSON 导入导出，在线测试匹配
- 🧪 **规则测试** — 输入文本预览命中结果，不执行动作

## 安装

1. 将 `napcat-plugin-swd` 文件夹放入 NapCat 的 `plugins/` 目录
2. 修改 NapCat 的 `napcat.mjs`，在白名单中添加 `"napcat-plugin-swd"`：

```js
// 搜索 official plugin whitelist，在 Set 中添加：
const rme = new Set([
  "napcat-plugin-builtin",
  "napcat-plugin-cleaner",
  "napcat-plugin-ssqq",
  "napcat-plugin-qce",
  "napcat-plugin-swd"  // ← 添加
]);
```

3. 重启 NapCat

## 配置

### 全局设置

| 配置项 | 说明 | 默认值 |
|--------|------|--------|
| `enable` | 总开关 | `true` |
| `groupMode` | 群聊范围：`all` / `whitelist` / `blacklist` | `all` |
| `groupList` | 群号列表 | `[]` |
| `adminExempt` | 豁免管理员 | `true` |
| `enableWarnMessage` | 发送警告消息 | `true` |
| `logRetention` | 日志保留条数 | `500` |

### 规则字段

| 字段 | 说明 |
|------|------|
| `name` | 规则名称 |
| `pattern` | 匹配模式（关键词或正则） |
| `isRegex` | `true` = 正则，`false` = 关键词 |
| `actions` | 动作数组：`recall` / `mute` / `kick` / `warn` |
| `muteDuration` | 禁言时长（秒） |
| `muteNotify` | 禁言时是否 @ 提醒 |
| `muteNotifyMessage` | 禁言提醒消息模板 |
| `warnMessage` | 警告消息模板 |
| `enabled` | 是否启用 |

### 消息模板变量

| 变量 | 含义 |
|------|------|
| `{user_id}` | QQ 号 |
| `{nickname}` | 昵称 |
| `{card}` | 群名片 |
| `{rule_name}` | 规则名 |
| `{match_detail}` | 匹配到的词 |
| `{group_name}` | 群名称 |
| `{group_id}` | 群号 |
| `{mute_duration}` | 禁言秒数 |
| `{time}` | 当前时间 |

### @ 用户

在消息模板开头加 `[CQ:at,qq={user_id}]` 即可：

```
[CQ:at,qq={user_id}] 违规发言已被撤回。触发规则：{rule_name}，匹配词：{match_detail}
```

## API

所有路由基于 `/api/Plugin/ext/napcat-plugin-swd/`（需认证）。

| 方法 | 路径 | 说明 |
|------|------|------|
| `GET` | `/status` | 运行状态 + 统计 |
| `GET/POST` | `/config` | 读写配置 |
| `GET` | `/rules` | 规则列表 |
| `POST` | `/rules` | 添加规则 |
| `POST` | `/rules/update` | 更新规则 |
| `POST` | `/rules/delete` | 删除规则 |
| `POST` | `/rules/toggle` | 启用/禁用规则 |
| `GET` | `/logs` | 违规日志 |
| `POST` | `/logs/clear` | 清空日志 |
| `POST` | `/test` | 测试匹配 |
| `POST` | `/import-words` | 导入 TXT 词库 |
| `GET/POST` | `/groups` | 群聊配置 |

## 项目结构

```
napcat-plugin-swd/
├── package.json
├── index.mjs                 # 主逻辑
├── README.md
└── webui/
    ├── dashboard.html        # 仪表盘
    └── rules-manager.html    # 规则管理
```

## License

MIT
