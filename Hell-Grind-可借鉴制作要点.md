# Hell Grind 可借鉴制作要点手册

> 整理日期：2026-09-28（SGT）  
> 用途：把 Higgsfield《Hell Grind》公开制作方法里**值得迁移**的部分，收成一份可反复查阅的工作手册。  
> 不怕长；标了「原文摘录」的尽量保留英文可复制块；标了「归纳」的是综合公开报道后的实操建议。

---

## 0. 先说结论（给你用的版本）

1. **确实公开了**：不是营销空话。Higgsfield 把《Hell Grind》相关的大量生成记录、提示词、资产与流程摊开（报道口径约 11 万+ 条记录、按场次分文件夹），并配合 Academy / Cinema Studio 教学。
2. **最值得抄的不是某一句「神 prompt」**，而是三件事：  
   - 每镜末尾同一套 **12 行技术底座**；  
   - 单镜 **超长、固定骨架**（先状态、再几何、再分拍动作）；  
   - **少写禁止词、多写正面规格**（尤其动作戏）。
3. **不能指望复制粘贴就复刻同片**：角色一致很大程度依赖平台里的 `@引用` / Elements / 三视图资产；通用 Seedance / 其他文生视频只能迁移「写法」，不能迁移整套托管资产。
4. **和你现有工具链的关系**：这是「扩散模型长片流水线」经验，和 Remotion（shotcraft / talkcraft）、auto-motion（字幕→MG）互补，不互相替代。

---

## 1. 背景与公开了什么

### 1.1 片子本身（公开宣传口径）

| 项 | 口径 |
|---|---|
| 片名 | Hell Grind |
| 制作方 | Higgsfield AI |
| 时长 | 约 95 分钟（报道写 95:06） |
| 成本 | 约 $500,000（多为算力） |
| 周期 | 约 14 天量级（团队含多位导演/摄影/剪辑协作） |
| 曝光 | 2026 戛纳电影市场（Marché du Film）等；外媒有报道 |
| 定位 | 公司自称更像「用长片教下一代 AI 电影人」，并推广 Cinema Studio / Academy |

### 1.2 公开包里有什么

据小互等对公开档案的梳理（他们声称从公开 API 拉取约 4.1 万条记录做统计）：

- **115,446** 条量级的公开生成记录，按场景分约 **108** 个文件夹；
- 点开单条可见：当时用的 **完整 prompt、模型、参数、资产下载链接**；
- 片子本身也可在 Higgsfield 相关页观看；
- 另有 **Higgsfield Academy** 课程化讲解（含 prompt-builder skill、分镜流程等）。

### 1.3 「开源」该怎么理解

- **开的是制作账本与提示词档案**，方便学习与在他们生态里复用；  
- **不是**把整条商业片生产线变成与平台无关的纯本地开源软件；  
- CEO 等公开表态也强调：单镜要写清角色、环境、运镜、光、动作、连续性，一致性往往还要靠 **Cinema Studio 内已建资产**。

---

## 2. 最值得抄：12 行「技术底座」（Technical Foundation）

### 2.1 原文摘录（可直接当 snippet）

几乎每镜 prompt **末尾**都贴同一段。小互统计最高频行出现约 **8,015** 次。内容无关题材，可当你的默认片尾规格：

```text
Style: 8K IMAX. Photorealistic — no 3D render, no game engine,
  no game-cutscene aesthetic.
Cinematography: Emmanuel Lubezki × Roger Deakins.
Camera: Physical cine lens. 180° shutter motion blur.
Lighting: Natural light only — contre-jour backlight, camera on
  shadow side, atmospheric haze throughout. Key light from sky
  and windows only.
Color: 60:30:10 — dominant / secondary / accent.
Skin: Pore-level realism — vellus hair, asymmetric moles,
  capillary flush, pore-shadow matching on-set light.
Physics: Gravity and inertia respected — mass has real weight,
  correct contact shadows. No floating props.
Acting: Hollywood — micro-pauses before reactions, precise
  eye-line, wet living eyes with catch-lights, visible breath
  and chest rise.
Composition: Rule of thirds + golden ratio. Every person moving
  from frame one.
Continuity: Characters, props, environment identical across
  every cut. No identity drift.
Technical: 24fps smooth motion. 8K detail. No jitter.
Audio: Environmental SFX only. No music. No subtitles.
```

### 2.2 逐行：防什么（归纳 + 档案统计口径）

| 行 | 主要在防什么 | 档案中约出现次数（小互） |
|---|---|---|
| Style：8K IMAX / 写实，并点名禁 3D、游戏引擎、过场动画感 | AI 默认「塑料 CG / 游戏过场」味；点名禁止比空喊 realistic 有效 | ~5,820 |
| Cinematography：Lubezki × Deakins | 用摄影指导名字当风格锚，比一堆形容词稳 | ~5,219 |
| Camera：电影镜头 + 180° 快门运动模糊 | 电子锐利、每帧过脆；180° 是电影运动模糊物理基线 | ~6,693 |
| Lighting：自然光、逆光、相机在阴影侧、大气雾、主光来自天空/窗 | **最关键之一**；AI 默认平光高亮 | ~4,942 |
| Color：60:30:10 | 色彩失控、花 | ~6,444 |
| Skin：毛孔、毳毛、不对称痣、毛细血管红、毛孔阴影跟现场光 | 「皮肤真实」太空；拆成可执行细节 | ~6,822 |
| Physics：重力惯性、接触影、禁漂浮道具 | 悬空、脚不沾地 | ~6,490 |
| Acting：反应前微停、视线、湿润眼神高光、呼吸胸廓 | 「演得像人」拆成微动作 | ~4,146 |
| Composition：三分/黄金分割 + **从第 1 帧起人人在动** | 后半句关键：否则容易站桩合影 | ~7,252 |
| Continuity：角色道具环境跨镜一致、无身份漂移 | 长片命脉 | ~7,706 |
| Technical：24fps、8K、无抖动 | 帧率与清晰度底线 | ~8,015 |
| Audio：仅环境音效，无配乐、无字幕 | 模型爱私自配乐/烧字幕，要显式关掉 | ~7,513 |

### 2.3 使用注意（归纳）

- **Style 行里的 “no game…”**：后面动作戏章节会讲到——模型常「看见 game 一词就往游戏感走」。底座仍保留这写法是档案事实；你自己改写时，**优先用正面光材质描述**，少堆叠 “not a game”。  
- 摄影指导姓名是风格锚，不是法律授权；商用注意品牌/肖像与引用惯例。  
- 底座适合贴在**每镜结尾**；前面写本镜剧情与几何。

---

## 3. 单镜长提示词：七段骨架（在底座之前）

### 3.1 量级（档案事实）

- 中位 prompt 长度约 **16,501** 字符（约三千英文词量级）；  
- 最长约 **39,801** 字符；  
- 常见生成单元：**约 15 秒** 一段。

### 3.2 七段结构（小互从长 prompt 统计）

顺序相对固定，底座在最后：

| # | 区块 | 写什么 | 为何重要 |
|---|---|---|---|
| ① | **Character current state** | 伤势、衣破位置、表情、此刻外貌 | **最大块**。模型无跨镜记忆；不写会「自愈/换装」 |
| ② | **SCENE: continuation…** | 承接上一镜什么状态 | 约 34% 长 prompt 显式出现 |
| ③ | **What this shot is about** | 导演意图锚点（这一镜要完成什么） | ~16% |
| ④ | **GEOMETRY: staging** | 谁在哪、距离、朝向 | ~37%；空间乱则穿帮 |
| ⑤ | **DIALOGUE & SFX** | 对白 + 即使无对白也写风、脚步等 | ~36% |
| ⑥ | **ACTION + 6 beats** | 15 秒切成约 **6 拍**，每拍一句 | ~48%；防动作糊成一团 |
| ⑦ | **KEY RULES** | 必须有 / 绝不能有 | ~33% |

其它常见区块（可并存）：**CAMERA ~58%**、**NEGATIVE ~25%**、**LIGHTING ~24%**、带 ⚠️ 警告标记 ~30%。

### 3.3 「模型没有记忆」——每镜重写状态（归纳）

视频模型不知道三分钟前主角被捅了一刀。若本镜不重写：

- 伤口消失、衣服复原、发型重置；  
- 道具左右手互换；  
- 场景物品「重生」。

**实操**：维护一份「角色此刻状态卡」，每镜开头整段粘贴再改本镜增量。

### 3.4 可复制的空模板（归纳）

```text
[HEADER]
EXACT N CHARACTERS — NO DUPLICATES
Shot ID: S__-__ | Duration: 15s | Model: ______

[CHARACTER STATE — paste every shot]
Name @handle:
- Face / hair / scars / makeup right now:
- Costume damage & dirt right now:
- Props in hands / on body:
- Emotional performance (actions, not adjectives): muscle / breath / eyes…

[SCENE CONTINUATION]
Previous shot ended with:
This shot picks up:

[INTENT]
This shot exists to show:

[GEOMETRY]
Camera side: frame-left / frame-right (camera POV only)
Landmarks:
Distances (meters):
Who faces whom:
Camera must NEVER cross: ______

[DIALOGUE & SFX]
Dialogue:
Environment SFX (even if silent scene):

[ACTION — 6 beats for ~15s]
Beat1 (0–2.5s):
Beat2:
Beat3:
Beat4:
Beat5:
Beat6:

[CAMERA] (optional but recommended)
Lens mm / height / move with numbers (not vague adjectives):

[KEY RULES]
MUST:
MUST NOT: (prefer rewrite as positive MUST when possible)

[TECHNICAL FOUNDATION — paste 12 lines]
…（第二节原文）…
```

---

## 4. 角色与场景资产：怎么稳住「同一个人」

### 4.1 三视图 / 多视图角色表（公开报道归纳）

常见做法：

- **脸部特写** + **全身正面** + **全身背面**（有的流程强调背面/无头全身等变体，按团队规范来）；  
- 定稿前做 **压力测试**：同一角色不同姿势、光线生成多次，仍要一眼可认；  
- 平台侧用 **`@角色名`** 把短描述绑到参考图（Elements），生成时引用同一 handle。

### 4.2 场景也要「版本化」

以「中央博物馆」类核心场景为例（百科/报道口径）：

- 先出一张主视觉，再多视角拼参考；  
- 随剧情准备多版：**初到 / 战后破损 / 高潮阴暗** 等，避免全片场景永远崭新。

### 4.3 复杂站位：平面图 + 低模（报道口径）

- 纯文字 **平面图**：地标、左右关系、机位、**不可越轴的线**；  
- 复杂调度可辅以低多边形场景帮助想清楚遮挡与距离。

### 4.4 空间写法习惯（社区/实践帖归纳，可迁移）

- 左右只用 **frame-left / frame-right**（相机视角），别写「主角左边」这种模型易懵的相对词；  
- 位置钉在地标 + **米制距离**；  
- 写清相机站哪一侧、哪条线永不越过；  
- 对话戏可只给房间一角，空间越小，模型乱摆人越少；  
- **开场约 1 秒大远景定场**：谁在哪、光从哪来；有人建议这一秒加极短语气词，方便模型把「定场」当独立镜。

---

## 5. 最贵的教训：少写「不要」，多写「要什么」

### 5.1 经典翻车（Higgsfield / 官方复盘口径）

1. **否定陷阱**  
   - 写 “no handheld object, no knife handle”，模型仍给握刀手；  
   - 改成正面调度：抬臂、画幅明确露出空手。  

2. **「Not a game」反而强化游戏感**  
   - 动作戏即使写十几遍 not game / no CGI，模型仍抓住 “game” 往过场动画走；  
   - **删掉否定堆砌**，改写真实灯光、材质、物理接触；  
   - **用多机位硬切** 代替一条跟拍长镜头（游戏感常来自第三人称长跟）。

3. **形容词弱、数字强**  
   - 弱：「快速推轨」「猛烈环绕」；  
   - 强：「从 8m 推到 4m，高度从 3m 降到 2m」「比常规电影推轨快约 3 倍」。

### 5.2 动作戏修复两步（官方 Rebuild 视频口径）

1. **禁令 → 正面描述**（光、材质、接触、重量）；  
2. **一场打戏拆成多镜硬切**，例如：  
   - 50mm 环绕；  
   - 24mm 低机位；  
   - 85mm 特写；  
   - 35mm 全景；  
   再挑片拼接，而不是一条「永远跟在背后」的长镜头。

### 5.3 表演写法：情境优先（报道口径）

- 少写 sad / angry 等情绪形容词；  
- 多写 **肌肉动作、停顿、视线、呼吸**；  
- 对白节奏与停顿写进拍点；有条件时用模型的 **声线/音色 profile** 跨镜锁定。

### 5.4 生成经济学（报道数字，作心态参考）

- 常以 15 秒为单元；大约每生成 20 条里选 3–4 条能用；  
- 中段大战可吃掉 **数千** 次生成；后期熟练场次可降到几十次量级——**前期学费会极高**。

---

## 6. 镜头语言可迁移对照（归纳）

档案与复盘里反复出现的思路：

| 意图 | 更可执行的写法 |
|---|---|
| 电影感运动模糊 | 180° shutter；物理电影镜头 |
| 打破游戏跟拍感 | 多焦距硬切，而不是单次长 orbit behind hero |
| 定场 | 开场 1s 广角；地标 + 米制站位 |
| 情绪特写 | 85mm 级特写 + 眼神高光/微停 |
| 冲击力 | 24mm 低机 + 明确运动距离/时间 |
| 色彩不花 | 60:30:10 主/次/点缀 |
| 光不「广告片平光」 | 逆光、阴影侧机位、窗/天光主光、大气雾 |

> 小互文中还提到「焦距参考表」等会员深文内容；本稿以免费公开段 + 官方复盘可核实部分为准。需要表格式焦点对照时，建议直接进他们公开档案按场景点开 CAMERA 段汇总。

---

## 7. 平台绑定 vs 可迁移清单

### 7.1 强依赖 Higgsfield / Cinema Studio 的

- `@角色` / Elements 一键挂参考；  
- 与 Studio UI 绑定的连续性工具、部分 voice profile；  
- Academy 里的官方 prompt-builder skill 与课程工作流；  
- 「点开即下资产」的托管档案体验。

### 7.2 可迁移到 Seedance / 通用文生视频 / 你自己的流水线

- 12 行技术底座（按模型微调）；  
- 七段骨架 + 每镜重写状态卡；  
- 三视图角色包 + 场景多版本；  
- 文字平面图 / 越轴线；  
- 数字运镜、六拍动作、正面描述代替否定堆；  
- 动作戏「多机位硬切再剪」；  
- 大批量生成 + 严选（3–4/20）心态。

### 7.3 和你已有 fork 的分工（提醒）

| 工具 | 擅长 |
|---|---|
| Hell Grind 方法 | 写实/奇幻长片感、扩散模型分镜一致性 |
| video-shotcraft / talkcraft | Remotion 产品片、口播讲解 |
| auto-motion | SRT→竖屏 MG 批量产线 |
| TapNow / Seedance / MiniMax 等 | 实际出片引擎 |

---

## 8. 实操：开工模板与检查表

### 8.1 项目开工一次做齐

- [ ] 每个主角：三视图（或等效多视图）+ `@handle` 命名表  
- [ ] 每个主角：初始 / 中段狼狈 / 终局 等 **状态卡** 版本  
- [ ] 每个主场景：干净版 / 破坏版 / 高潮光色版  
- [ ] 每个主场景：一页文字平面图（地标、米、机位、越轴线）  
- [ ] 全片共用：**技术底座** snippet（第二节）  
- [ ] 决定默认单镜时长（建议 10–15s）与「每批生成条数 / 选用比例」预算  

### 8.2 写每一镜前

- [ ] 粘贴最新角色状态卡（伤口衣着道具）  
- [ ] 写清承接上一镜什么  
- [ ] GEOMETRY 只用 frame-left/right + 米 + 地标  
- [ ] ACTION 拆成约 6 拍，每拍不超过约 1–3 句  
- [ ] 运镜用数字，不用空形容词  
- [ ] KEY RULES：能改成「必须看见…」就不要写「不要…」  
- [ ] 末尾贴技术底座  
- [ ] 若有对白：音色/声线说明与拍点对齐  

### 8.3 出片后快速质检（拒片清单）

- [ ] 是否游戏过场感（长跟拍、悬浮运镜、塑料材质）？  
- [ ] 脚是否沾地、道具是否漂浮？  
- [ ] 角色脸/发型/服装是否漂移？  
- [ ] 是否多生人或重复角色？  
- [ ] 是否私自加字幕/配乐？  
- [ ] 是否站桩不动？  
- [ ] 光是否平亮广告感？  
- [ ] 动作是否糊成一团（拍点不够）？  
- [ ] 与上一镜空间左右是否跳轴？  

### 8.4 动作戏专用

- [ ] 删除堆叠的 “not a game / no CGI”  
- [ ] 改写光与材质正面描述  
- [ ] 拆成 ≥3 个不同焦距/机位的硬切  
- [ ] 手部特写用「空手可见」类正面调度，少用 no knife  

### 8.5 迷你示例：把「否定」改成「正面」（归纳）

**弱：**

```text
no knife, no weapon in hand, not a video game, no CGI
```

**强：**

```text
His right hand rises empty into frame; fingers open; palm catches window light;
knuckles and skin pores visible; nothing grips the palm.
Camera: 85mm, chest-up, locked off.
Cut to 24mm low angle as he steps 2 meters forward in 1.2 seconds.
Lighting: contre-jour from the tall museum windows; camera on shadow side; light haze.
```

---

## 9. 参考链接

- Hell Grind 相关页（示例剧集入口）：https://higgsfield.ai/original-series/hell-grind/episode-1  
- Higgsfield Academy：https://higgsfield.ai/academy  
- 小互深度拆解（含 12 行底座与七段统计；后半可能付费）：https://best.xiaohu.ai/en/article/higgsfield-hell-grind-opensource/  
- 行业报道示例：DNYUZ / CompleteAITraining 等对 Academy 开放提示词的报道（2026-08）  
- 官方动作戏重建讲解：YouTube search `Hell Grind: Rebuilding Our Biggest Fight Scene from Scratch`

---

## 10. 版权与使用边界（务必读）

- 提示词写法可学；**影片画面、角色设计、明星/运动员 likeness、原声**等另有权利，勿直接商用搬运。  
- 「开源制作方法」≠ 你可以宣称整片素材 MIT。  
- 本手册整理自公开报道与可访问摘要，**不是** Higgsfield 官方文档全文转载；统计数字以原披露方为准，可能随档案更新变化。

---

## 11. 一页纸速查（打印版）

1. 每镜末尾贴 **12 行底座**。  
2. 每镜开头重写 **角色此刻状态**。  
3. 中间按 **意图 → 几何 → 声 → 六拍动作 → 规则**。  
4. 运镜与动作用 **数字**；左右用 **画面左右**。  
5. **少写 no，多写要看见什么**。  
6. 动作戏：**多机位硬切**，别一条游戏跟拍。  
7. 角色靠 **多视图资产 + @引用**；场景做 **多版本**。  
8. 接受 **海选**：多数生成会扔，好的留下剪。

—— 完 ——
