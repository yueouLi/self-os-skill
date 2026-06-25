# /我的操作系统 — 自我认知系统生成器

生成一个「像填 object 一样认识自己」的交互式 HTML 自我复盘工具。
五个 Tab：变量数据 / 逻辑链 / 引导提问 / 人生坐标轴 / 核心叙事。

## 触发
用户输入 `/我的操作系统`

---

## Step 0 — 选择模式

使用 AskUserQuestion:
- Question: "你想要哪种模式？"
- Header: "生成模式"
- Options:
  - "空白模板" — 生成干净的 HTML，自己在浏览器里填（5分钟内拿到工具）
  - "Claude 引导复盘" — 你来问我问题，根据我的回答生成有内容的个性化版本（需要20-30分钟）

---

## Step 1a — 空白模板模式

### 询问名字
发一条消息："你叫什么名字？用来命名文件。"

记录为 `$NAME`。

### 生成文件
用 Write 工具，读取模板内容：
```bash
cat "/Users/yueouli/.claude/skills/我的操作系统/template.html"
```

将 `自我认知系统 { }` 替换为 `$NAME 的操作系统 { }`，
将 localStorage key `self-knowledge-v3` 替换为 `self-os-$NAME-v1`，
写入 `~/Desktop/$NAME的操作系统.html`。

### 打开
```bash
open ~/Desktop/"$NAME的操作系统.html"
```

告诉用户：文件在桌面，浏览器里可以直接编辑，数据自动保存到本地。

**结束。**

---

## Step 1b — 引导复盘模式

### 先问名字
发一条消息："好，我来引导你做一次认知复盘。先告诉我你的名字，方便我个性化标题。"

记录为 `$NAME`。

---

### 采访轮次（每轮一条消息问完，等用户完整回答后再进入下一轮）

**轮 1 — 环境（CONST）**

发消息：
> 我们先从你的成长环境开始。以下三个问题一起回答就好，不用一一标注：
>
> 1. 你的父母最常对你说的一句话是什么？那句话让你相信了什么？
> 2. 18岁前有没有让你特别愤怒或受伤、但当时无力改变的事？后来它影响了你什么？
> 3. 用一个词描述你原生家庭的氛围——那个氛围现在还在你身上吗？

用户回答后，提炼为 `$ENV`：3-5个简短要点（原生家庭底色2-3条 + 关键事件1-2条）。

---

**轮 2 — 天赋（CONST）**

发消息：
> 现在聊聊你的天赋和天花板：
>
> 1. 有没有一件事你不费力就比周围人做得好？或者别人经常来找你帮什么忙？
> 2. 什么事让你进入心流——完全忘了时间、不觉得累？
> 3. 你在哪件事上试了所有方法，始终突破不了顶尖水平？（这可能是真正的天花板）

用户回答后，提炼为 `$TALENT`：擅长2-3条 + 天花板1-2条。

---

**轮 3 — 运气（CONST）**

发消息：
> 来聊聊你生命里的运气——那些不在你控制里的事：
>
> 1. 有没有一个"如果没有那件事或那个人，我的路会完全不同"的时刻？
> 2. 你哪次失败，后来证明是最好的事？它带给你什么是成功带不来的？
> 3. 你在出生/成长环境中无意间得到了哪些别人没有的优势？

用户回答后，提炼为 `$LUCK`：好运2-3条 + 坏运转化1-2条。

---

**轮 4 — 价值观（VAR）**

发消息：
> 接下来是你的价值观——你真正在乎什么：
>
> 1. 你上一次真的愤怒，是因为哪个原则被触犯了？
> 2. 你会为了什么主动降低自己的利益？（这里藏着你真正的优先级）
> 3. 哪件事你明知道"不聪明"也要做，因为它符合你的内心？

用户回答后，提炼为 `$VALUES`：2-3个核心价值观，每个包含"表现"和"导致"。

---

**轮 5 — 思维方式（VAR）**

发消息：
> 现在聊聊你的思维模式——你怎么处理信息和做决定：
>
> 1. 遇到别人批评你，你的第一反应是什么？（先怀疑对方、先怀疑自己、还是搁置判断？）
> 2. 你解决新问题的第一步通常是什么？查资料、找人问、自己推导、还是先动手试？
> 3. 你有没有一个"不管别人怎么说，这件事我有自己判断"的领域？那个判断从哪里来？

用户回答后，提炼为 `$MINDSET`：2-3个思维模式，每个包含"表现"和"导致"。

---

**轮 6 — 努力（VAR）**

发消息：
> 最后是努力——你在做什么，为什么做：
>
> 1. 你现在最在做的2-3件事是什么？为什么是它们，而不是别的事？
> 2. 什么事让你越做越有劲（不是越做越累）？
> 3. 你的"自律"长什么样——不是理想中的，是实际上的？

用户回答后，提炼为 `$EFFORT`：当前方向2-3条 + 自律定义2-3条。

---

### 提炼逻辑链

基于6轮内容，自行提炼 3-5 条最关键的因果关系，向用户展示：

> 根据你说的，我看到这几条底层逻辑：
> 
> **Chain 1（IF/THEN）**：IF [条件] → THEN [结果] → ∴ [结论]
> **Chain 2（LOOP）**：[起点] → [步骤2] → [步骤3] → ↺ 循环
> ...

然后问：
> 这些对吗？想修改、删除哪条，或者补充别的逻辑链？

等用户确认或修改，更新为 `$CHAINS`。

---

## Step 2 — 生成个性化 HTML（引导模式）

读取模板：
```bash
cat "/Users/yueouli/.claude/skills/我的操作系统/template.html"
```

用 Write 工具生成 `~/Desktop/$NAME的操作系统.html`，做以下替换/填充：

### 1. 标题
- `<title>自我认知系统 { }</title>` → `<title>$NAME 的操作系统 { }</title>`
- `<h1>自我认知系统 { }</h1>` → `<h1>$NAME 的操作系统 { }</h1>`
- localStorage key `self-knowledge-v3` → `self-os-$NAME-v1`

### 2. 数据 Tab — tag-item 格式

每个 tag-item 用这个格式插入：
```html
<div class="tag-wrap"><span class="tag-item" contenteditable>内容写这里</span><span class="x" onclick="removeTag(this)">×</span></div>
```

在 `环境` card 的 `原生家庭底色` 下插入 `$ENV` 中家庭相关的要点（描述行 + 影响行）。
在 `成长关键事件` 下插入 `$ENV` 中关键事件要点。
在 `天赋` card 的 `abilities` 下插入 `$TALENT`（擅长行 + 天花板行）。
在 `运气` card 的 `luck_factors` 下插入 `$LUCK`（好运行 + 坏运行）。
在 `价值观` card 中，每个核心价值观建一个 prop-block，填入表现和导致。
在 `思维方式` card 中，每个思维模式建一个 prop-block，填入表现/依赖/导致。
在 `努力` card 的 `当前方向` 下填入在做/为什么，`自律定义` 下填入原则/反例/导致。

### 3. 逻辑链 — chain-card 格式

**IF/THEN/∴ 格式：**
```html
<div class="chain-card">
  <span class="del-chain" onclick="removeChain(this)">× 删除</span>
  <span class="kw">IF&nbsp;&nbsp;&nbsp;</span><span class="cond" contenteditable>条件</span><br>
  <span class="kw">THEN&nbsp;</span><span class="res" contenteditable>结果</span><br>
  <span class="op">∴&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="conc" contenteditable>因此</span>
</div>
```

**LOOP 格式：**
```html
<div class="chain-card">
  <span class="del-chain" onclick="removeChain(this)">× 删除</span>
  <span class="kw">LOOP&nbsp;</span><span class="cond" contenteditable>起点</span><br>
  <span class="op">&nbsp;↓&nbsp;&nbsp;&nbsp;</span><span contenteditable style="color:#555">步骤2</span><br>
  <span class="op">&nbsp;↓&nbsp;&nbsp;&nbsp;</span><span contenteditable style="color:#555">步骤3</span><br>
  <span class="op">&nbsp;↺&nbsp;&nbsp;&nbsp;</span><span class="res" contenteditable>回到起点，复利累积</span>
</div>
```

**矛盾 ⚡ 格式：**
```html
<div class="chain-card">
  <span class="del-chain" onclick="removeChain(this)">× 删除</span>
  <span class="kw">⚡&nbsp;&nbsp;&nbsp;</span><span class="cond" contenteditable>价值/行为A</span><br>
  <span class="op">vs&nbsp;&nbsp;&nbsp;</span><span class="cond" contenteditable>价值/行为B</span><br>
  <span class="kw">WHEN&nbsp;</span><span contenteditable style="color:#555">什么情境下冲突</span><br>
  <span class="op">∴&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="res" contenteditable>如何取舍</span>
</div>
```

将 `$CHAINS` 中每条链以对应格式插入 `<div class="logic-grid" id="logic-grid">` 内。

---

## Step 3 — 打开并告知

```bash
open ~/Desktop/"$NAME的操作系统.html"
```

告诉用户：
- 文件保存在桌面
- 五个 Tab 都已解锁：可以继续在「④ 人生坐标轴」钉时间节点，在「⑤ 核心叙事」写操作系统总结
- 数据自动保存到浏览器本地，刷新不丢失
- 引导提问 Tab 里有更多深度问题可以继续挖掘
