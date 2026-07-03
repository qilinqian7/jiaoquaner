# jiaoquaner 焦圈儿平台插件

焦圈儿（jiaoquaner）平台各 AI 接口能力的 Claude Code 技能库。以一个**统一入口**收敛公共鉴权、计费纪律与错误码，再路由到各具体能力子技能。

## 包含的技能

| 技能 | 触发场景 | 接口 |
|------|----------|------|
| `jiaoquaner:jiaoquaner` | 平台统一入口：判断意图、建立公共上下文（鉴权/计费/错误码）后分发 | — |
| `jiaoquaner:humanize` | 降 AI 率 / 去 AI 化 / 让文本更像人写 | `POST /api/v1/agent/reduce-ai-rate` |

## 安装

```
/plugin marketplace add <你的用户名>/jiaoquaner-plugin
/plugin install jiaoquaner@jiaoquaner
/reload-plugins
```

本地开发时也可直接指向目录：

```
/plugin marketplace add /path/to/jiaoquaner-plugin
/plugin install jiaoquaner@jiaoquaner
```

## 配置 API Key

技能优先读环境变量，二选一：

```bash
export jiaoquaner_api_key=sk-xxxxxx   # 推荐、可复用；需在启动会话前 export
```

或本次运行临时赋值 `API_KEY=sk-xxxxxx`。Key 通过焦圈儿的 `POST /api/v1/apikeys`（JWT 认证）创建，仅返回一次。**切勿把 key 写进任何文件或提交到 git。**

## 计费说明

- 每次调用按 API Key 所属用户的**焦耳余额真实扣费**，失败不扣费。
- 接口均同步、单次返回，无批量/并发；技能不会私自加重试循环反复扣费。

## 扩展新能力

焦圈儿新增接口时：

1. 在 `skills/` 下新建子技能目录，如 `skills/xxx/SKILL.md`；
2. 子技能只写该接口特有的请求体 / 响应 / 专属错误码；
3. 在入口 `skills/jiaoquaner/SKILL.md` 的「能力路由」表加一行意图 → 子技能映射；
4. 公共鉴权 / 计费 / 错误码继续复用入口，避免重复。

## 许可

[MIT](./LICENSE)
