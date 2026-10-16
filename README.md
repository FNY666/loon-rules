# Loon 远程规则镜像（takedown backup）

本目录是 Loon 配置中引用的上游远程 `.list` 规则文件的镜像备份，
防止上游仓库被下架（takedown）导致配置里的远程规则加载失败。

## 目录结构

- `blackmatrix7/` — 来自 https://github.com/blackmatrix7/ios_rule_script 的 Loon 规则
  （`rule/Loon/<Service>/<Service>.list`，master 分支）
- `acl4ssr/` — 来自 https://github.com/ACL4SSR/ACL4SSR 的规则
  （`Clash/ChinaDomain.list`、`Clash/BanAD.list`、`Clash/LocalAreaNetwork.list`，master 分支）
- `ttyyss2233/` — 来自 https://github.com/ttyyss2233/Tool 的规则
  （`shadowrocket/rules/apns.list`，main 分支）

## 来源清单

完整"镜像文件 → 上游 URL"映射见 `SOURCES.tsv`（制表符分隔：`mirror_path<TAB>upstream_url`）。

## 同步机制

每周自动同步一次上游：`cron: loon-rules-mirror-sync`。
- 上游有更新 → 拉取新版并提交推送。
- 上游被下架/下载失败 → 保留本地最后可用版本，不覆盖。
- 同步只动本目录文件，不碰仓库其他内容。

## 在 Loon 配置中的引用形式

```
https://raw.githubusercontent.com/FNY666/loon-rules/main/<upstream>/<File>.list
```

例如：`.../blackmatrix7/OpenAI.list`

注意：本目录文件为第三方规则的原样镜像，版权归原作者所有，
仅作可用性备份用途。

## custom/ —— 自主调研的 AI 规则（非上游镜像）

`custom/` 下的 53 个 `.list` 文件来自自主调研，是对上游 blackmatrix7 等规则的补充，不是镜像。
- 第一批（2026-10-16）：全网 AI 域名调研（社区 / 团队仓库 / 网站论坛），42 个文件、
  200 个域名，覆盖 40 个 AI 服务。
- 第二批（2026-10-16）：11 个新服务——加密货币与金融科技（Coinbase、Hyperliquid、Wise、Revolut）、
  电商（AliExpress）、在线教育（Coursera、Udemy、edX）、云服务（AWS、Azure、GoogleCloud）。

- 文件命名：`custom/<服务>.list`（如 `custom/Grok.list`、`custom/DeepSeek.list`）
- 区域标记：`Intl`（国外服务，走代理）/ `CN`（国内服务，走直连），见各文件头注释
- `custom/MANIFEST.tsv`：文件名、服务、区域、域名数的对照表

使用示例（Loon Remote Rule）：
```
https://raw.githubusercontent.com/FNY666/loon-rules/main/custom/Grok.list, policy=🤖 AI, tag=Grok, enabled=true
https://raw.githubusercontent.com/FNY666/loon-rules/main/custom/DeepSeek.list, policy=DIRECT, tag=DeepSeek, enabled=true
```
