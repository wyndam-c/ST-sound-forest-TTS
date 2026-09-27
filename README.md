# 声林 TTS · 多引擎语音插件 for SillyTavern



一个用于 SillyTavern（酒馆）的多引擎 TTS 语音扩展：一个插件，六个引擎随便切。

> **本仓库为二改分支**：fork 自 [**@lynn33-2728**](https://github.com/lynn33-2728) 的 [**ST-sound-forest-TTS**](https://github.com/lynn33-2728/ST-sound-forest-TTS)（上游原分支 `main`，共同贡献者）。
> 上游仓库：https://github.com/lynn33-2728/ST-sound-forest-TTS
> 上游分支：https://github.com/lynn33-2728/ST-sound-forest-TTS/tree/main
> 本分支在原有 5 引擎之外**新增「小米 MiMo」**，并把说明同步进了本 README。向原作者与所有共同贡献者致谢 🙏

- **硅基流动 SiliconFlow**：CosyVoice2，支持在线克隆音色
- **火山引擎**：大模型语音合成，内置 100+ 音色（通用 / 角色扮演 / 方言 / 多语种），支持 `ICL_` 声音复刻
- **MiniMax**：Speech-2.8 / 2.6 / 02 系列模型，支持声音复刻音色 ID
- **MOSS**：MOSS-TTS，使用 `voice_id` 合成语音；音色克隆在 Mossland 官网完成
- **Fish Audio**：S2.1 Pro Free / S2.1 Pro / S2 Pro，读取账号音色或手填 `reference_id`；音色克隆在官网完成
- **小米 MiMo**（二改新增）：`mimo-v2.5-tts` 系列，支持预置音色 / 音色设计 / 音色克隆，返回 wav；官方与 token-plan 免费通道二选一

本项目基于 [hjl2004-10/extension](https://github.com/hjl2004-10/extension) 修改演进，感谢原作者的项目基础。

## 功能一览

- 侧栏式设置面板：**API / 文本截取 / 缓存 / 日志** 四大块，点哪看哪
- 引擎一键切换，五家配置独立保存，互不影响
- **文本截取设置各引擎共用**：符号提取、标签块规则、全文发送上限，设一次全都生效
- **火山长文本自动拆分**：超过单段安全长度时按句子拆开，依次生成并连续播放
- 自动朗读角色消息（可开关），每条消息旁有手动 ▶ 按钮
- **缓存面板**：六个引擎的语音缓存并列显示，重听不扣费，可下载音频、可单删、可清空
- 多人角色音色：群聊按角色名分配音色，六个引擎分开存
- 自绘悬浮进度条：可拖动、可调速、可下载，PC / 平板 / 手机都适配
- 日志面板：每一步合成过程可见，排查问题不求人

## 安装

### 方法一：GitHub 下载 zip（推荐）

下载 zip：

```text
https://github.com/wyndam-c/ST-sound-forest-TTS/archive/refs/heads/main.zip
```

解压后把文件夹重命名为 `ST-sound-forest-TTS`，放到：

```text
SillyTavern/data/default-user/extensions/ST-sound-forest-TTS
```

### 方法二：git clone（可一键更新）

```text
git clone https://github.com/wyndam-c/ST-sound-forest-TTS
```

放进 `SillyTavern/data/default-user/extensions/` 下即可。这样以后在酒馆里点「扩展更新」就会直接从这个仓库拉最新版；
如果之前装的是上游版本（remote 指向 `lynn33-2728/ST-sound-forest-TTS`），把更新路径切过来即可：

```text
git -C SillyTavern/data/default-user/extensions/ST-sound-forest-TTS remote set-url origin https://github.com/wyndam-c/ST-sound-forest-TTS.git
```

然后刷新 SillyTavern 页面，打开「扩展 → 声林 · 多引擎语音（TTS）」配置 API。

## 各引擎配置

| 引擎 | 需要填写 | 获取地址 |
|---|---|---|
| 硅基流动 | API 密钥 | [siliconflow.cn](https://siliconflow.cn) |
| 火山引擎 | AppID + Access Key | [火山引擎语音控制台](https://console.volcengine.com/speech/overview)（开通「大模型语音合成」） |
| MiniMax | API Key（无需 GroupID） | [MiniMax 开放平台](https://platform.minimaxi.com)（账户管理 → 接口密钥） |
| MOSS | API Key + voice_id | [Moss API 平台](https://platform.mosi.cn/docs/getting-started/auth/) |
| Fish Audio | API Key + 音色 ID | [Fish Audio 开发者页面](https://fish.audio/zh-CN/developers/) |
| 小米 MiMo | API Key（可选免费通道 Key） | 小米 MiMo 开放平台 |

说明：

- 火山引擎、MOSS 和 Fish Audio 的语音合成请求经酒馆服务端 `/proxy` 中转，请使用较新版本 SillyTavern；MiniMax 直连官方接口，避免与酒馆 Basic Auth 的鉴权头冲突。
- 声音复刻：硅基在插件里直接上传音频克隆；火山 / MiniMax 在各自平台控制台复刻后，把音色 ID 填进「自定义音色ID」即可。
- MOSS 的 TTS 接口只接受 `voice_id`，不接受直接把参考音频塞进朗读请求；请到 [Mossland 音色设计](https://mossland.studio/voice/design) 完成克隆，在「我的」音色卡片点复制图标取得 `voice_id`，粘贴回插件即可。
- Fish Audio 先在[官网](https://fish.audio/zh-CN/app/)完成克隆，再到插件点“刷新我的音色”，或手动粘贴音色 ID。测试连接只检查 Key 与音色列表，不需要音色 ID。默认使用 `s2.1-pro-free` 开发者档，失败不会自动切换到付费模型。
- MiniMax 语音按字符计费；若已订阅 Token Plan，请使用「订阅 Key」填入 API Key 栏。

## 出处与致谢

- **上游原仓库（共同贡献者）**：[lynn33-2728/ST-sound-forest-TTS](https://github.com/lynn33-2728/ST-sound-forest-TTS) —— 本仓库 fork 自它的 `main` 分支，原作者 **[@lynn33-2728](https://github.com/lynn33-2728)**。
- 上游原分支链接：<https://github.com/lynn33-2728/ST-sound-forest-TTS/tree/main>
- 更早的来源：[hjl2004-10/extension](https://github.com/hjl2004-10/extension)。

原项目版权归原作者所有，本二改分支保留全部原始署名。新增 / 修改部分由本仓库维护者添加。项目按 MIT License 发布，详见 [LICENSE](LICENSE)。

---

## 二改说明：新增「小米 MiMo」引擎（v2.3.0-mimo）

在原 5 引擎（硅基流动 / 火山 / MiniMax / MOSS / Fish Audio）基础上，**新增第 6 个引擎：小米 MiMo TTS**。

### 它是什么

小米 MiMo 的 TTS 走 OpenAI 兼容的 `POST {host}/v1/chat/completions`，请求头用 `api-key`，返回 `choices[0].message.audio.data`（base64 wav）。因此它天然适配本插件的「一引擎 = 一套配置」结构。

三种合成模式：

| 模式 | 模型 | 说明 |
|---|---|---|
| 预置音色 | `mimo-v2.5-tts` | 官方 9 个音色：MiMo 默认 / 冰糖 / 茉莉 / 苏打 / 白桦 / Mia / Chloe / Milo / Dean |
| 音色设计 | `mimo-v2.5-tts-voicedesign` | 用一段文字描述生成音色（性别、年龄、口音、音高、音色质感、气质） |
| 音色克隆 | `mimo-v2.5-tts-voiceclone` | 上传 mp3 / wav / m4a 参考音频（≤8MB），以 base64 随请求发送 |

### 配置

1. 引擎下拉选「小米 MiMo」。
2. 填 **API Key**（官方 `api.xiaomimimo.com`）。也可勾选「改用免费通道」并填免费 Key（`token-plan-sgp.xiaomimimo.com`）。
3. 选合成模式，填对应内容；「朗读风格」可选，为预置/克隆模式的全局演绎风格。
4. 点「测试连接」验证，或到「通用 → TTS测试」用当前引擎试听。

### 兼容说明

- 直连若报 CORS，勾选「经酒馆 /proxy 转发」（走酒馆服务端中转，需要新版酒馆）。
- 返回音频为 **wav**；缓存面板的下载会保存为 `.wav`。
- 「多人角色音色」映射只在**预置音色**模式下逐角色生效；设计 / 克隆模式对所有角色统一。
- 缓存 / 日志 / 悬浮播放条 / 文本截取等公共能力与其它引擎完全一致。

---

## 修复：自动朗读「播的是上一段」（v2.3.1-mimo）

### 问题

开着自动朗读时，新消息来了听到的却是**上一段**的语音（上一段已经加载好/缓存好的那段），有时还会同一条反复播、几段音频互相打断。

### 原因

TTS 合成是异步的，长文本一次要几十秒：

1. **旧请求迟到会「抢播」** —— 上一次朗读的合成慢、晚一步才返回，回来时照样直接播放，就把新的一段盖掉了。
2. **同一条文字被重复合成** —— 自动朗读和手动点 ▶ 都会触发，同一段文字被并发请求多次（既费额度，又会有多个音频先后冒出来播）。
3. **历史消息重渲染也会触发自动朗读** —— 编辑旧消息、重画、重新载入聊天都会重新渲染消息，插件会把它当成「新消息」，于是把上一段已缓存的音频又播一遍。

### 修复

- **朗读代次（generation）**：每次新朗读领一个递增编号，**只有最新一次**的合成结果允许出声，旧请求迟到的结果一律丢弃。
- **开新一段前先掐掉上一段**：新朗读一开始就停掉正在播放的音频、作废旧的分段播放队列，不再出现「新消息来了，还在读上一段」。
- **同一段文字复用请求**：同一段文字正在合成时，再点 ▶ / 自动朗读会复用这一次请求，不再重复发（省额度，也不会重复播）。
- **自动朗读只认最后一条消息**：只有「聊天里的最后一条消息」才会自动朗读，编辑旧消息 / 重画不再误触发。
- **载入 / 切换聊天不打扰**：进入页面或换聊天时，历史消息一律不再触发自动朗读（不会一进来就播上一条）。

> 修复后版本号显示为 **v2.3.1-mimo**（悬浮播放条右下角 / 插件标题）。如果界面还显示 2.3.0，说明浏览器缓存了旧的 `index.js`，把酒馆页面完全关掉重开（或强制刷新 Ctrl+F5）即可。

---

## 优化：播放速度更好调（v2.3.2-mimo）

- 悬浮播放条上新增 **倍速小按钮**（直接显示当前速度，如 `1.00x`）：**点一下往上跳一档**
  `1.0x → 1.25x → 1.5x → 2x → 2.5x → 3x → 回到 1.0x`。
- 播放条 `⋮` 菜单里的「播放速度」滑杆上限从 2x 提到 **3x**，并加了一排**快捷倍速按钮**（一按即换）。
- 播放速度是**本地变速**：不重新请求、不重新合成、不额外扣费；正在播的音频会**立刻**变速。
- 适用于所有引擎。小米 MiMo / MOSS / Fish 的接口没有语速字段，想让它们快/慢就用这个倍速。
