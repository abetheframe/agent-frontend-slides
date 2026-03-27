---
theme: default
title: 從 Chatbox 到 Copilot
info: |
  下一代 Agent 前端架構
  AG-UI & CopilotKit 探索
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
highlighter: shiki
colorSchema: dark
fonts:
  sans: DM Sans
  mono: JetBrains Mono
  provider: google
---

<div class="flex flex-col items-center justify-center h-full">

<div class="mb-6 px-4 py-1.5 rounded-full border border-teal-400/20 bg-teal-400/[0.06] text-xs font-mono text-teal-300/80 tracking-wide">
  AG-UI & CopilotKit 探索 × Brainstorming
</div>

# 從 Chatbox 到 Copilot

<div class="w-24 h-[2px] bg-gradient-to-r from-teal-400 to-violet-400 mx-auto mt-5 mb-5 rounded-full"></div>

## 下一代 Agent 前端架構

<div class="mt-8 flex gap-6 text-sm text-slate-500 font-mono">
  <span>Abe Chen</span> 
  <span>2026.03.27</span>
</div>

</div>

<!--
大家好，今天想跟大家聊一個我最近在研究的東西 — AG-UI 和 CopilotKit。

簡單來說，這是一套讓 AI Agent 不再只是聊天機器人，而是能真正融入我們產品 UI 的技術方案。

今天的目標不是要教大家怎麼寫 code，而是先搞懂這東西是什麼、能幹嘛，然後一起想想我們的產品可以怎麼用。所以後半段會有 brainstorm 的時間，大家等等可以自由發想。
-->

---
layout: default
---

# 議程

<div class="grid grid-cols-2 gap-4 mt-6">

<div class="p-5 rounded-xl border border-white/[0.06] bg-white/[0.02] relative overflow-hidden">
  <div class="absolute top-0 left-0 w-1 h-full bg-teal-400/60 rounded-r"></div>
  <div class="flex items-baseline gap-3 mb-2">
    <span class="text-teal-400 font-mono text-sm font-semibold">01</span>
    <span class="font-bold text-white">AG-UI 基礎概念與運作</span>
  </div>
  <div class="text-sm text-slate-400 pl-8">什麼是 AG-UI？Protocol Triangle 全景觀</div>
</div>

<div class="p-5 rounded-xl border border-white/[0.06] bg-white/[0.02] relative overflow-hidden">
  <div class="absolute top-0 left-0 w-1 h-full bg-violet-400/60 rounded-r"></div>
  <div class="flex items-baseline gap-3 mb-2">
    <span class="text-violet-400 font-mono text-sm font-semibold">02</span>
    <span class="font-bold text-white">市場應用案例 & Generative UI</span>
  </div>
  <div class="text-sm text-slate-400 pl-8">實際應用 + 三種 Generative UI 模式</div>
</div>

<div class="p-5 rounded-xl border border-white/[0.06] bg-white/[0.02] relative overflow-hidden">
  <div class="absolute top-0 left-0 w-1 h-full bg-amber-400/60 rounded-r"></div>
  <div class="flex items-baseline gap-3 mb-2">
    <span class="text-amber-400 font-mono text-sm font-semibold">03</span>
    <span class="font-bold text-white">Open Brainstorm</span>
  </div>
  <div class="text-sm text-slate-400 pl-8">團隊自由討論：我們的後台系統能怎麼用？</div>
</div>

<div class="p-5 rounded-xl border border-white/[0.06] bg-white/[0.02] relative overflow-hidden">
  <div class="absolute top-0 left-0 w-1 h-full bg-pink-400/60 rounded-r"></div>
  <div class="flex items-baseline gap-3 mb-2">
    <span class="text-pink-400 font-mono text-sm font-semibold">04</span>
    <span class="font-bold text-white">Idea 收斂</span>
  </div>
  <div class="text-sm text-slate-400 pl-8">彙整討論結果，挑選可執行的 POC 方向</div>
</div>

</div>

<!--
今天大概分四段：

第一段講 AG-UI 的基本概念，讓大家知道這是什麼、在整個 Agent 生態系裡的定位。
第二段看市場上的應用案例，還有 Generative UI 這個比較新的概念。
第三段是 open brainstorm，大家自由討論，想想我們的後台系統有哪些地方可以用。
最後收斂一下，看有沒有值得做 POC 的方向。

brainstorm 的部分多留一點時間，那是今天的重頭戲。
-->

---
layout: two-cols-header
---

# 為什麼需要 AG-UI？

<div class="text-sm text-gray-400 mb-4">業界現狀：AI Agent 大多只活在「聊天視窗」裡</div>

::left::

<div class="pr-8">

### 業界現狀

- Agent 只能輸出文字，使用者自己 copy-paste
- 想讓 Agent 操作 UI？每個團隊自己造 WebSocket 橋接
- 沒有標準協定，換框架就要重寫
- 前端和 Agent 之間沒有狀態同步機制

</div>

::right::

<div v-click class="pl-8">

### 有了 AG-UI {.text-green-400}

- Agent 能即時串流更新 UI 元件
- 統一 event-based 協定，各框架通用
- **Shared State** — Agent 和 UI 雙向即時同步
- **Human-in-the-Loop** — Agent 可暫停等使用者確認

</div>

<!--
在講 AG-UI 是什麼之前，先講一下為什麼需要它。

左邊是目前業界大部分的做法 — AI Agent 基本上就是一個 chatbox。Agent 只能輸出文字，使用者拿到之後要自己 copy-paste 到系統裡。如果你想讓 Agent 直接操作 UI，每個團隊都要自己造一套 WebSocket 或 polling 的橋接方案，沒有標準。

右邊是有了 AG-UI 之後 — Agent 能直接串流更新 UI 元件，前後端有共享狀態可以即時同步，而且 Agent 可以在執行過程中暫停，等使用者確認之後再繼續。最重要的是，這是一個統一的 event-based 協定，換 Agent 框架不用重寫前端。

簡單說，AG-UI 就是要讓 Agent 從「只會聊天」進化成「能操作你的 App」。
-->

---
layout: default
---

# AG-UI 是什麼？

<div class="text-sm text-slate-400 mb-5">Agent-User Interaction Protocol — 開放的輕量級協定，透過 SSE 串流 JSON 事件</div>

<!-- Protocol 三本柱：$nav.clicks 驅動 inline style transition -->
<div class="proto-grid" :style="{ gap: $nav.clicks >= 1 ? '0.6rem' : '1.25rem', marginBottom: $nav.clicks >= 1 ? '0.75rem' : '1.5rem' }">
  <div class="proto-card" :style="{ padding: $nav.clicks >= 1 ? '0.5rem 0.85rem' : '1.75rem 1.5rem' }">
    <div class="proto-bar bg-blue-400/60"></div>
    <div class="proto-name text-blue-400" :style="{ fontSize: $nav.clicks >= 1 ? '0.7rem' : '1.125rem' }">MCP</div>
    <div class="proto-role" :style="{ fontSize: $nav.clicks >= 1 ? '0.8rem' : '1.375rem' }">Agent ↔ Tool</div>
    <div class="proto-desc" :style="{ maxHeight: $nav.clicks >= 1 ? '0px' : '4em', opacity: $nav.clicks >= 1 ? 0 : 1, marginTop: $nav.clicks >= 1 ? '0px' : '0.25rem' }">Agent 呼叫外部工具和資料源<br/>大家最熟的那個</div>
  </div>
  <div class="proto-card" :style="{ padding: $nav.clicks >= 1 ? '0.5rem 0.85rem' : '1.75rem 1.5rem' }">
    <div class="proto-bar bg-violet-400/60"></div>
    <div class="proto-name text-violet-400" :style="{ fontSize: $nav.clicks >= 1 ? '0.7rem' : '1.125rem' }">A2A</div>
    <div class="proto-role" :style="{ fontSize: $nav.clicks >= 1 ? '0.8rem' : '1.375rem' }">Agent ↔ Agent</div>
    <div class="proto-desc" :style="{ maxHeight: $nav.clicks >= 1 ? '0px' : '4em', opacity: $nav.clicks >= 1 ? 0 : 1, marginTop: $nav.clicks >= 1 ? '0px' : '0.25rem' }">多 Agent 之間的協作溝通<br/>Google 主推</div>
  </div>
  <div class="proto-card proto-agui" :style="{ padding: $nav.clicks >= 1 ? '0.5rem 0.85rem' : '1.75rem 1.5rem', borderColor: $nav.clicks >= 1 ? 'rgba(45,212,191,0.45)' : 'rgba(45,212,191,0.2)', background: $nav.clicks >= 1 ? 'rgba(45,212,191,0.08)' : 'rgba(45,212,191,0.04)', boxShadow: $nav.clicks >= 1 ? '0 0 20px rgba(45,212,191,0.15)' : 'none' }">
    <div class="proto-bar bg-teal-400/80"></div>
    <div class="proto-name text-teal-400" :style="{ fontSize: $nav.clicks >= 1 ? '0.7rem' : '1.125rem' }">AG-UI ★</div>
    <div class="proto-role" :style="{ fontSize: $nav.clicks >= 1 ? '0.8rem' : '1.375rem' }">Agent ↔ User</div>
    <div class="proto-desc" :style="{ maxHeight: $nav.clicks >= 1 ? '0px' : '4em', opacity: $nav.clicks >= 1 ? 0 : 1, marginTop: $nav.clicks >= 1 ? '0px' : '0.25rem' }">Agent 與前端 UI 即時互動<br/><strong class="text-teal-400/80">前端最該關注的</strong></div>
  </div>
</div>

<div v-click class="grid grid-cols-4 gap-3 mb-4">
  <div class="text-center p-4 rounded-lg border border-amber-400/15 bg-amber-400/[0.03]">
    <div class="text-amber-400 text-2xl mb-1.5">💬</div>
    <div class="text-white font-semibold text-sm mb-1">Text Streaming</div>
    <div class="text-slate-500 text-xs leading-snug">逐字串流輸出<br/>即時顯示回應</div>
  </div>
  <div class="text-center p-4 rounded-lg border border-blue-400/15 bg-blue-400/[0.03]">
    <div class="text-blue-400 text-2xl mb-1.5">🔧</div>
    <div class="text-white font-semibold text-sm mb-1">Tool Calls</div>
    <div class="text-slate-500 text-xs leading-snug">前端定義 Action<br/>Agent 遠端觸發</div>
  </div>
  <div class="text-center p-4 rounded-lg border border-emerald-400/15 bg-emerald-400/[0.03]">
    <div class="text-emerald-400 text-2xl mb-1.5">🔄</div>
    <div class="text-white font-semibold text-sm mb-1">Shared State</div>
    <div class="text-slate-500 text-xs leading-snug">前後端狀態同步<br/>不用手動 polling</div>
  </div>
  <div class="text-center p-4 rounded-lg border border-pink-400/15 bg-pink-400/[0.03]">
    <div class="text-pink-400 text-2xl mb-1.5">✋</div>
    <div class="text-white font-semibold text-sm mb-1">Human-in-the-Loop</div>
    <div class="text-slate-500 text-xs leading-snug">Agent 暫停等確認<br/>使用者掌握主控權</div>
  </div>
</div>

<div v-click class="p-4 rounded-xl border border-white/[0.06] bg-white/[0.02]">
  <div class="flex items-center gap-3 mb-3">
    <span class="text-teal-400 font-bold text-sm">AG-UI × CopilotKit</span>
    <span class="text-slate-600 text-xs font-mono">—— 協定 vs 框架</span>
  </div>
  <div class="grid grid-cols-2 gap-x-8 gap-y-1.5 text-sm">
    <div class="flex items-start gap-2">
      <span class="text-teal-400 mt-0.5">›</span>
      <span class="text-slate-300">AG-UI 是<strong>協定</strong>（像 HTTP），CopilotKit 是<strong>框架</strong>（像 Axios）</span>
    </div>
    <div class="flex items-start gap-2">
      <span class="text-teal-400 mt-0.5">›</span>
      <span class="text-slate-300">Barkai 兄弟（2023 西雅圖）從內部痛點出發，開源成獨立協定</span>
    </div>
    <div class="flex items-start gap-2">
      <span class="text-teal-400 mt-0.5">›</span>
      <span class="text-slate-300">GitHub <strong>29k+ ⭐</strong> · 超過 <strong>10% Fortune 500</strong> 在 production 使用</span>
    </div>
    <div class="flex items-start gap-2">
      <span class="text-teal-400 mt-0.5">›</span>
      <span class="text-slate-300">已被 <strong>Google、LangChain、AWS、Microsoft、PydanticAI、Mastra</strong> 等採用</span>
    </div>
    <div class="flex items-start gap-2">
      <span class="text-teal-400 mt-0.5">›</span>
      <span class="text-slate-300">前端 SDK 支援 <strong>React、Vue、Svelte</strong>；後端支援 <strong>Python、Node.js</strong></span>
    </div>
  </div>
</div>

<style>
  .proto-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    transition: gap 0.9s cubic-bezier(0.16, 1, 0.3, 1),
                margin-bottom 0.9s cubic-bezier(0.16, 1, 0.3, 1);
  }
  .proto-card {
    border-radius: 1rem;
    border: 1px solid rgba(255,255,255,0.06);
    background: rgba(255,255,255,0.02);
    position: relative;
    overflow: hidden;
    transition: padding 0.9s cubic-bezier(0.16, 1, 0.3, 1),
                border-color 0.9s cubic-bezier(0.16, 1, 0.3, 1),
                background 0.9s cubic-bezier(0.16, 1, 0.3, 1),
                box-shadow 0.9s cubic-bezier(0.16, 1, 0.3, 1),
                border-radius 0.9s cubic-bezier(0.16, 1, 0.3, 1);
  }
  .proto-bar {
    position: absolute;
    top: 0; left: 0;
    width: 3px; height: 100%;
    border-radius: 0 4px 4px 0;
  }
  .proto-name {
    font-family: 'JetBrains Mono', monospace;
    font-weight: 700;
    padding-left: 0.75rem;
    margin-bottom: 0.25rem;
    line-height: 1.4;
    transition: font-size 0.9s cubic-bezier(0.16, 1, 0.3, 1);
  }
  .proto-role {
    color: white;
    font-weight: 700;
    padding-left: 0.75rem;
    margin-bottom: 0.25rem;
    line-height: 1.4;
    transition: font-size 0.9s cubic-bezier(0.16, 1, 0.3, 1);
  }
  .proto-desc {
    color: #94a3b8;
    font-size: 0.875rem;
    padding-left: 0.75rem;
    line-height: 1.6;
    overflow: hidden;
    transition: max-height 0.9s cubic-bezier(0.16, 1, 0.3, 1),
                opacity 0.6s cubic-bezier(0.16, 1, 0.3, 1),
                margin-top 0.9s cubic-bezier(0.16, 1, 0.3, 1);
  }
  @keyframes agui-glow {
    0%, 100% { box-shadow: 0 0 16px rgba(45, 212, 191, 0.1); }
    50% { box-shadow: 0 0 28px rgba(45, 212, 191, 0.25); }
  }
</style>

<!--
AG-UI 是一個開放協定，透過 SSE 串流 JSON 事件，讓 Agent 後端跟前端 UI 即時同步。

目前這是 Agent Protocol 三本柱。
三個協定各自負責不同層面：
- MCP：Agent ↔ 工具。Anthropic 推的，讓 Agent 能呼叫外部工具、查資料庫。大家應該有聽過。
- A2A：Agent ↔ Agent。Google 推的，解決多 Agent 協作、任務派發的問題。
- AG-UI：Agent ↔ 使用者。今天的主角，也是前端最直接相關的。前兩個都在 server 端，AG-UI 是唯一面向 UI 這側的協定。

AG-UI 定義的四種核心事件類型：Messages 負責訊息串流、Tool Calls 負責工具呼叫、State Patches 負責狀態同步、Lifecycle 負責生命週期管理。

AG-UI 跟 CopilotKit 的關係：AG-UI 是協定，CopilotKit 是框架，就像 HTTP 跟 Axios。

背景：2023 年兩兄弟在西雅圖創立 CopilotKit，從自己的內部痛點出發開源，他們之前在做另一個產品，過程中幫內部工具做了一個 AI Copilot，發現生產力提升很明顯。後來他們把這套 Copilot 基礎設施開源出來，結果社群反應非常強烈，就 pivot 成 CopilotKit 作為核心產品。

團隊其實很小，核心大概就幾個人，但 GitHub 上已經有 29k+ stars，超過 10% 的 Fortune 500 企業在 production 用。他們在做 CopilotKit 的過程中，為了解決「Agent 怎麼跟前端溝通」這個問題，內部發展出一套 event-based 的串流機制。2025 年 5 月，他們把這套機制獨立出來開源，就是 AG-UI。發布第一天就拉了 LangGraph、CrewAI、Mastra 當合作夥伴，之後 Google、AWS、Microsoft 也陸續加入。
-->

---
layout: default
---

# REST API vs Agent API

<div class="text-sm text-slate-400 mb-6">從 Request-Response 到 Event Stream</div>

| 維度 | REST API | Agent API（AG-UI） |
|------|----------|-------------------|
| 通訊模式 | Request → Response，一來一回 | SSE 串流，持續推送事件 |
| 回傳格式 | JSON data | 事件流（messages, tool calls, state patches） |
| 狀態管理 | Stateless，前端自己管 | Shared State，前後端即時同步 |
| UI 更新 | 前端拿到資料後自己 render | Agent 直接驅動 UI 更新 |
| 互動方向 | 單向：使用者 → API | 雙向：Agent 也能暫停等使用者確認 |

<div v-click class="mt-6 text-center py-3 px-6 rounded-xl border border-teal-400/20 bg-teal-400/[0.04]">
<span class="text-base font-semibold text-teal-300">AG-UI 讓 Agent 從「回傳資料」進化成「驅動 UI」</span><span class="text-slate-400 text-sm"> — 預設 SSE，也支援 WebSocket</span>

</div>

<!--
這頁對我們工程師最有幫助 — 從我們每天在用的 REST API 角度來看，Agent API 到底差在哪裡。

通訊模式：REST 是 request-response 一來一回，Agent API 是 SSE 串流持續推送事件。這個概念大家不陌生，跟 ChatGPT 逐字打字出來的效果是一樣的技術。

回傳格式：REST 回的是 JSON data，Agent API 回的是一連串事件流 — 包含 messages、tool calls、state patches。

狀態管理：REST 是 stateless 前端自己管，Agent API 有 shared state 前後端即時同步。

UI 更新：REST 是前端拿到資料之後自己 render，Agent API 是 Agent 直接驅動 UI 更新。

互動方向：REST 是單向的使用者打 API，Agent API 是雙向的 — Agent 也能主動暫停等使用者確認，這就是 Human-in-the-Loop。

核心思維轉變：Agent 不再只是 API，而是 UI 的驅動者。
-->

---
layout: default
---

<div class="h-full flex flex-col">

<div>

# Server-Sent Events (SSE) 事件生命週期

<div class="text-sm text-slate-400">一次 Agent 互動中，事件如何流動？（~16 種標準事件）</div>

</div>

<div class="flex items-center justify-center gap-4 flex-1">

<!-- START -->
<div class="flex flex-col items-center shrink-0">
  <div class="px-4 py-3 rounded-xl border border-teal-400/30 bg-teal-400/10 text-teal-400 font-mono text-xs font-bold">RUN_STARTED</div>
</div>

<!-- Arrow -->
<div class="text-slate-600 text-lg shrink-0">→</div>

<!-- Middle: 3 parallel lanes -->
<div class="flex-1 flex flex-col gap-3">

<div v-click class="flex items-center gap-2 px-4 py-3 rounded-xl border border-amber-400/20 bg-amber-400/[0.04]">
  <div class="text-amber-400 font-bold text-xs w-20 shrink-0">Messages</div>
  <div class="flex gap-1.5 items-center">
    <span class="px-2 py-0.5 rounded bg-amber-400/10 text-amber-400/80 font-mono text-[10px]">START</span>
    <span class="text-slate-600 text-xs">→</span>
    <span class="px-2 py-0.5 rounded bg-amber-400/10 text-amber-400/80 font-mono text-[10px]">CONTENT ×N</span>
    <span class="text-slate-600 text-xs">→</span>
    <span class="px-2 py-0.5 rounded bg-amber-400/10 text-amber-400/80 font-mono text-[10px]">END</span>
  </div>
  <div class="ml-auto text-[10px] text-slate-500">逐字串流輸出</div>
</div>

<div v-after class="flex items-center gap-2 px-4 py-3 rounded-xl border border-blue-400/20 bg-blue-400/[0.04]">
  <div class="text-blue-400 font-bold text-xs w-20 shrink-0">Tool Calls</div>
  <div class="flex gap-1.5 items-center">
    <span class="px-2 py-0.5 rounded bg-blue-400/10 text-blue-400/80 font-mono text-[10px]">START</span>
    <span class="text-slate-600 text-xs">→</span>
    <span class="px-2 py-0.5 rounded bg-blue-400/10 text-blue-400/80 font-mono text-[10px]">ARGS</span>
    <span class="text-slate-600 text-xs">→</span>
    <span class="px-2 py-0.5 rounded bg-blue-400/10 text-blue-400/80 font-mono text-[10px]">END</span>
  </div>
  <div class="ml-auto text-[10px] text-slate-500">工具呼叫過程</div>
</div>

<div v-after class="flex items-center gap-2 px-4 py-3 rounded-xl border border-emerald-400/20 bg-emerald-400/[0.04]">
  <div class="text-emerald-400 font-bold text-xs w-20 shrink-0">State</div>
  <div class="flex gap-1.5 items-center">
    <span class="px-2 py-0.5 rounded bg-emerald-400/10 text-emerald-400/80 font-mono text-[10px]">SNAPSHOT</span>
    <span class="text-slate-600 text-xs">→</span>
    <span class="px-2 py-0.5 rounded bg-emerald-400/10 text-emerald-400/80 font-mono text-[10px]">DELTA ×N</span>
  </div>
  <div class="ml-auto text-[10px] text-slate-500">狀態即時同步</div>
</div>

<div v-click class="mt-6 text-xs text-slate-500 text-center">
三種事件可並行發生，全部透過同一條 SSE 連線推送 · 預設 SSE，也支援 WebSocket
</div>

</div>

<!-- Arrow -->
<div class="text-slate-600 text-lg shrink-0">→</div>

<!-- END -->
<div class="flex flex-col items-center shrink-0">
  <div class="px-4 py-3 rounded-xl border border-violet-400/30 bg-violet-400/10 text-violet-400 font-mono text-xs font-bold">RUN_FINISHED</div>
</div>

</div>



</div>

<!--
這頁說明一次 Agent 互動中事件是怎麼流動的。

SSE（Server-Sent Events）本質上就是一條長連線 HTTP，server 持續 push 事件給前端，AG-UI 定義了大約 16 種標準事件格式。

整個互動從 RUN_STARTED 開始，到 RUN_FINISHED 結束。中間有三條事件流可以並行：

Messages — 逐字串流輸出，就是 ChatGPT 那種打字效果。START、一連串 CONTENT 事件、然後 END。

Tool Calls — Agent 呼叫工具的過程。前端可以在 tool call START 時就顯示「Agent 正在查詢中...」，不用等工具跑完。

State — 前後端共享狀態的同步。SNAPSHOT 是全量快照，DELTA 是後續的增量更新。前端可以直接把這個 state map 到 React 元件。

重點是這三條都跑在同一條 SSE 連線上，前端不需要管任何 polling 或 WebSocket 連線管理，CopilotKit SDK 都幫你處理好了。
-->

---
layout: default
---

# CopilotKit × React

<div class="text-sm text-gray-400 mb-6">基於 AG-UI 的 React SDK，直接裝進 Vite / Next.js 專案</div>

<div class="grid grid-cols-3 gap-4 mt-2">

<div class="ck-card p-5 rounded-xl border border-white/[0.06] bg-white/[0.02]" style="animation-delay: 0.3s">
  <div class="text-teal-400 font-bold mb-2 text-base">Chat UI</div>
  <div class="text-sm text-slate-400 leading-relaxed">開箱即用的聊天介面，支援串流、tool calls</div>
</div>

<div class="ck-card p-5 rounded-xl border border-white/[0.06] bg-white/[0.02]" style="animation-delay: 0.7s">
  <div class="text-emerald-400 font-bold mb-2 text-base">Generative UI</div>
  <div class="text-sm text-slate-400 leading-relaxed">Agent 動態產生並更新 React 元件</div>
</div>

<div class="ck-card p-5 rounded-xl border border-white/[0.06] bg-white/[0.02]" style="animation-delay: 1.1s">
  <div class="text-violet-400 font-bold mb-2 text-base">Shared State</div>
  <div class="text-sm text-slate-400 leading-relaxed"><code>useCoAgentStateRender</code> — Agent 和 UI 即時同步</div>
</div>

<div class="ck-card p-5 rounded-xl border border-white/[0.06] bg-white/[0.02]" style="animation-delay: 1.5s">
  <div class="text-amber-400 font-bold mb-2 text-base">Human-in-the-Loop</div>
  <div class="text-sm text-slate-400 leading-relaxed">Agent 暫停執行，等使用者確認或編輯</div>
</div>

<div class="ck-card p-5 rounded-xl border border-white/[0.06] bg-white/[0.02]" style="animation-delay: 1.9s">
  <div class="text-pink-400 font-bold mb-2 text-base">Frontend Actions</div>
  <div class="text-sm text-slate-400 leading-relaxed"><code>useCopilotAction</code> — 前端註冊 action 給 Agent</div>
</div>

<div class="ck-card p-5 rounded-xl border border-white/[0.06] bg-white/[0.02]" style="animation-delay: 2.3s">
  <div class="text-blue-400 font-bold mb-2 text-base">Framework 整合</div>
  <div class="text-sm text-slate-400 leading-relaxed">LangGraph / CrewAI / ADK / PydanticAI</div>
</div>

</div>

<div class="ck-card mt-5 text-center" style="animation-delay: 2.8s">

```bash
npm install @copilotkit/react-core @copilotkit/react-ui
```

</div>

<style>
  .ck-card {
    opacity: 0;
    animation: ck-fade-in 0.5s cubic-bezier(0.16, 1, 0.3, 1) forwards;
  }
  @keyframes ck-fade-in {
    from { opacity: 0; transform: translateY(12px); }
    to   { opacity: 1; transform: translateY(0); }
  }
</style>

<!--
CopilotKit 是基於 AG-UI 的 React SDK，可以直接裝進我們的 Vite 或 Next.js 專案。

Chat UI — 開箱即用的聊天介面元件，支援串流跟 tool calls。
Generative UI — 最酷的功能，Agent 可以動態產生並更新 React 元件，不只是輸出文字。
Shared State — 用 useCoAgentStateRender 這個 hook，讓 Agent 跟 UI 即時同步狀態。
Human-in-the-Loop — 讓 Agent 可以暫停執行，等使用者確認或編輯之後再繼續。
Frontend Actions — 用 useCopilotAction 讓前端註冊 action 給 Agent 呼叫，等等 code 頁會看到範例。
Framework 整合 — 支援 LangGraph、CrewAI、ADK、PydanticAI 等主流框架。

安裝就是一行 npm install，很簡單。

【Framework 整合補充】
這邊的 Framework 指的是後端 Python 那側用來建構 Agent 邏輯的工具：

LangGraph: LangChain 團隊出品，用 graph（節點 + 邊）定義 Agent 工作流程。適合複雜分支、條件判斷、多步驟推理的場景。目前業界最主流。

CrewAI: 多 Agent 協作框架，把不同 Agent 組成「團隊」，各自有角色分工（Researcher、Writer 等）。上手容易，適合需要平行處理任務的情境。

ADK（Agent Development Kit）: Google 出品，跟 Gemini 整合很深，走 Google Cloud 生態的首選。

PydanticAI: Pydantic 團隊（FastAPI 同一群人）做的框架，強調型別安全和結構化輸出，跟 FastAPI 整合最自然。比較新但成長很快。

共通點：這些框架都可以透過 CopilotKit SDK 包成一個 endpoint，讓前端用 AG-UI 協定溝通。不管後端選哪個框架，前端接法都一樣。
-->

---
layout: default
---

# Generative UI 三種模式

<div class="text-sm text-gray-400 mb-6">從最穩定到最自由 — 依需求選擇適合的模式</div>

<div class="grid grid-cols-3 gap-5">

<div class="genui-card p-5 rounded-xl border border-emerald-400/20 bg-emerald-400/[0.04] flex flex-col" style="animation-delay: 0.3s">
  <div class="flex items-baseline gap-2 mb-3">
    <span class="text-emerald-400 font-bold text-base">Controlled</span>
    <span class="text-slate-500 text-xs">靜態元件</span>
  </div>
  <div class="text-sm text-slate-300 leading-relaxed space-y-1.5 mb-4">
    <div>› 開發者預建 React 元件</div>
    <div>› Agent 只選擇顯示哪個 + 傳資料</div>
    <div>› Hook: <code>useFrontendTool</code></div>
  </div>
  <div class="mt-auto pt-3 border-t border-white/[0.06] space-y-1.5">
    <div class="flex items-start gap-2 text-sm"><span class="text-green-400 font-bold shrink-0">Pro</span><span class="text-slate-300">最穩定，適合 production</span></div>
    <div class="flex items-start gap-2 text-sm"><span class="text-red-400 font-bold shrink-0">Con</span><span class="text-slate-300">所有元件要先寫好</span></div>
  </div>
  <div class="mt-3 text-xs text-slate-500">例：天氣卡片、圖表元件</div>
</div>

<div class="genui-card p-5 rounded-xl border border-blue-400/20 bg-blue-400/[0.04] flex flex-col" style="animation-delay: 0.9s">
  <div class="flex items-baseline gap-2 mb-3">
    <span class="text-blue-400 font-bold text-base">Declarative</span>
    <span class="text-slate-500 text-xs">宣告式</span>
  </div>
  <div class="text-sm text-slate-300 leading-relaxed space-y-1.5 mb-4">
    <div>› Agent 用 JSON spec 描述 UI</div>
    <div>› 前端自動渲染元件</div>
    <div>› Hook: <code>A2UI renderer</code></div>
  </div>
  <div class="mt-auto pt-3 border-t border-white/[0.06] space-y-1.5">
    <div class="flex items-start gap-2 text-sm"><span class="text-green-400 font-bold shrink-0">Pro</span><span class="text-slate-300">彈性和穩定之間的平衡</span></div>
    <div class="flex items-start gap-2 text-sm"><span class="text-red-400 font-bold shrink-0">Con</span><span class="text-slate-300">需定義 component registry</span></div>
  </div>
  <div class="mt-3 text-xs text-slate-500">例：A2UI / json-render.dev</div>
</div>

<div class="genui-card p-5 rounded-xl border border-violet-400/20 bg-violet-400/[0.04] flex flex-col" style="animation-delay: 1.5s">
  <div class="flex items-baseline gap-2 mb-3">
    <span class="text-violet-400 font-bold text-base">Open-ended</span>
    <span class="text-slate-500 text-xs">全生成</span>
  </div>
  <div class="text-sm text-slate-300 leading-relaxed space-y-1.5 mb-4">
    <div>› Agent 直接輸出 HTML/CSS</div>
    <div>› 前端用 iframe sandbox 渲染</div>
    <div>› Hook: <code>MCP Apps integration</code></div>
  </div>
  <div class="mt-auto pt-3 border-t border-white/[0.06] space-y-1.5">
    <div class="flex items-start gap-2 text-sm"><span class="text-green-400 font-bold shrink-0">Pro</span><span class="text-slate-300">最大自由度</span></div>
    <div class="flex items-start gap-2 text-sm"><span class="text-red-400 font-bold shrink-0">Con</span><span class="text-slate-300">不建議 production 用</span></div>
  </div>
  <div class="mt-3 text-xs text-slate-500">例：MCP Apps</div>
</div>

</div>

<!-- 穩定 → 自由 spectrum bar -->
<div v-click class="mt-5 flex items-center gap-3 px-2 spectrum-wrap">
  <span class="text-xs font-mono text-emerald-400 shrink-0">穩定</span>
  <div class="flex-1 relative h-3 flex items-center">
    <div class="absolute inset-x-0 h-[3px] rounded-full spectrum-bar" style="background: linear-gradient(90deg, #34d399, #60a5fa, #a78bfa)"></div>
    <div class="absolute left-[0%] w-2 h-2 rounded-full bg-emerald-400 ring-2 ring-emerald-400/30 spectrum-dot-l"></div>
    <div class="absolute right-[0%] w-2 h-2 rounded-full bg-violet-400 ring-2 ring-violet-400/30 spectrum-dot-r"></div>
  </div>
  <span class="text-xs font-mono text-violet-400 shrink-0">自由</span>
</div>

<style>
  .genui-card {
    opacity: 0;
    animation: genui-in 0.6s cubic-bezier(0.16, 1, 0.3, 1) forwards;
  }
  @keyframes genui-in {
    from { opacity: 0; transform: translateY(14px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  .spectrum-bar {
    clip-path: inset(0 100% 0 0);
  }
  .spectrum-dot-l, .spectrum-dot-r {
    opacity: 0;
  }
  .spectrum-wrap:not(.slidev-vclick-hidden) .spectrum-bar {
    animation: spectrum-grow 0.9s cubic-bezier(0.16, 1, 0.3, 1) 0.1s forwards;
  }
  .spectrum-wrap:not(.slidev-vclick-hidden) .spectrum-dot-l {
    animation: spectrum-dot-fade 0.3s ease 0.1s forwards;
  }
  .spectrum-wrap:not(.slidev-vclick-hidden) .spectrum-dot-r {
    animation: spectrum-dot-fade 0.3s ease 0.9s forwards;
  }
  @keyframes spectrum-grow {
    from { clip-path: inset(0 100% 0 0); }
    to   { clip-path: inset(0 0% 0 0); }
  }
  @keyframes spectrum-dot-fade {
    from { opacity: 0; transform: scale(0.3); }
    to   { opacity: 1; transform: scale(1); }
  }
</style>

<!--
Generative UI 是整個領域裡比較新的概念，值得多講一下。

三種模式從左到右自由度遞增：

Controlled — 靜態元件，開發者把所有可能的 React 元件先建好，Agent 只負責選要顯示哪個、傳什麼資料。用 useFrontendTool 實作。最穩定，適合 production。

Declarative — Agent 用 JSON spec 描述 UI 結構，前端有個 renderer 自動產生元件。彈性跟穩定之間的折衷點。json-render.dev 有 live playground 可以直接試。

Open-ended — Agent 直接輸出 HTML/CSS，前端用 iframe sandbox 渲染。自由度最高，但有 XSS 風險，不建議直接用在 production。

對我們來說的價值是什麼？如果要做 POC，強烈建議從 Controlled 模式開始：
- 風險最低，元件可測試、可 review
- 可以先把最常用的幾個 UI 元件（圖表、表單、確認框）做成 Generative UI，讓 Agent 在對話中動態渲染
- 這樣的效果對使用者來說會非常直覺：「我說要看報表，他直接在聊天視窗裡把圖表畫出來」

https://www.copilotkit.ai/examples

copilotkit demos: https://dojo.ag-ui.com/pydantic-ai/feature/agentic_chat

Controlled: https://analytics-with-c1.vercel.app/?threadId=f761aa55-3234-4a63-b462-4d41bc9904df

Declarative: https://json-render.dev/

Open-ended: 就是你們在 claude 或 chatgpt 內看到的
-->

---
layout: default
---

# 市場上的應用案例

<div class="text-sm text-gray-400 mb-6">CopilotKit 官方 Examples — 都有 source code</div>

<div class="grid grid-cols-2 gap-3">

<div class="case-card p-4 rounded-xl border border-white/[0.06] bg-white/[0.02] flex gap-4 items-start" style="animation-delay: 0.3s">
  <div class="text-2xl mt-0.5">📊</div>
  <div>
    <div class="font-bold text-teal-400 text-sm">AI Dashboard</div>
    <div class="text-xs text-slate-400 mt-1 leading-relaxed">數據儀表板 + AI 助手，用自然語言問問題，Agent 即時產生圖表</div>
    <div class="text-[10px] text-slate-600 font-mono mt-2">Data</div>
  </div>
</div>

<div class="case-card p-4 rounded-xl border border-white/[0.06] bg-white/[0.02] flex gap-4 items-start" style="animation-delay: 0.7s">
  <div class="text-2xl mt-0.5">📋</div>
  <div>
    <div class="font-bold text-emerald-400 text-sm">AI Kanban Board</div>
    <div class="text-xs text-slate-400 mt-1 leading-relaxed">專案管理看板，Agent 自動更新任務狀態</div>
    <div class="text-[10px] text-slate-600 font-mono mt-2">Project</div>
  </div>
</div>

<div class="case-card p-4 rounded-xl border border-white/[0.06] bg-white/[0.02] flex gap-4 items-start" style="animation-delay: 1.1s">
  <div class="text-2xl mt-0.5">💬</div>
  <div>
    <div class="font-bold text-amber-400 text-sm">Conversational Forms</div>
    <div class="text-xs text-slate-400 mt-1 leading-relaxed">對話式表單，用 Agent 引導填寫複雜表單</div>
    <div class="text-[10px] text-slate-600 font-mono mt-2">UX</div>
  </div>
</div>

<div class="case-card p-4 rounded-xl border border-white/[0.06] bg-white/[0.02] flex gap-4 items-start" style="animation-delay: 1.5s">
  <div class="text-2xl mt-0.5">🤝</div>
  <div>
    <div class="font-bold text-violet-400 text-sm">AI CRM (Atomic CRM)</div>
    <div class="text-xs text-slate-400 mt-1 leading-relaxed">全功能 CRM 內建 AI 助手，管理客戶和交易</div>
    <div class="text-[10px] text-slate-600 font-mono mt-2">SaaS</div>
  </div>
</div>

<div class="case-card col-span-2 p-4 rounded-xl border border-white/[0.06] bg-white/[0.02] flex gap-4 items-start" style="animation-delay: 1.9s">
  <div class="text-2xl mt-0.5">🔬</div>
  <div>
    <div class="font-bold text-pink-400 text-sm">Research Canvas</div>
    <div class="text-xs text-slate-400 mt-1 leading-relaxed">研究畫布，AI 幫你規劃、追蹤和整理研究進度</div>
    <div class="text-[10px] text-slate-600 font-mono mt-2">Productivity</div>
  </div>
</div>

</div>

<div class="case-card mt-3 text-center text-xs text-slate-600 font-mono" style="animation-delay: 2.4s">
copilotkit.ai/examples · github.com/CopilotKit/CopilotKit/tree/main/examples
</div>

<style>
  .case-card {
    opacity: 0;
    animation: case-in 0.5s cubic-bezier(0.16, 1, 0.3, 1) forwards;
  }
  @keyframes case-in {
    from { opacity: 0; transform: translateY(12px); }
    to   { opacity: 1; transform: translateY(0); }
  }
</style>

<!--
這幾個都是 CopilotKit 官方的 showcase，每個都有完整 source code 可以直接 fork 來改。

為什麼要看這些？因為它們跟我們的後台系統非常相似：

AI Dashboard — 跟我們的報表頁面很像，使用者用自然語言問「上週哪個活動效果最好？」，Agent 即時產生圖表。不用自己寫複雜的查詢條件。

AI Kanban Board — 跟我們的任務管理流程類似，Agent 可以幫你把 backlog 自動分類、更新狀態。

Conversational Forms — 這個對我們最有價值！行銷人員設定一個活動可能要填 20 幾個欄位，用 Agent 對話引導填寫體驗會好很多。「你想辦什麼類型的活動？這個優惠碼的限制條件是什麼？」

Atomic CRM — 完整的 SaaS 產品加上 AI 助手，可以直接當成我們改造後台的參考架構。

核心觀察：這些案例都有一個共同模式 — 原本需要使用者「操作 UI」的東西，變成了「跟 AI 對話」。對我們後台來說，這就是最直接的使用者體驗升級。
-->

---
layout: two-cols-header
---

# 實戰：怎麼接進專案？

<div class="text-sm text-slate-400">整合到現有 React 專案</div>

::left::

<div class="pr-3">

### Step 1 — 整合流程 {.text-teal-400}

```jsx {all|1-3|5-8|9-17}{lines:true}
// 1. 安裝
// npm i @copilotkit/react-core
//       @copilotkit/react-ui

// 2. Wrap 你的 App
import { CopilotKit } from '@copilotkit/react-core';
import { CopilotSidebar } from '@copilotkit/react-ui';

<CopilotKit runtimeUrl="/api/copilotkit">
  <CopilotSidebar>
    <YourApp />
  </CopilotSidebar>
</CopilotKit>;
```

</div>

::right::

<div class="pl-3">

### Step 2 — 加入 Hooks {.text-teal-400}

```jsx {all|1-6|8-14}{lines:true}
// 註冊 Action 給 Agent 呼叫
useCopilotAction({
  name: "create_campaign",
  parameters: [{ ... }],
  handler: (args) =>
    createCampaign(args)
})

// Agent 狀態 → 即時渲染 UI
useCoAgentStateRender({
  name: "report_agent",
  render: ({ state }) =>
    <ChartPreview
      data={state.chart} />
})
```


</div>

<!--
整合流程非常直覺，就兩步驟：

Step 1 — Wrap App：安裝兩個 npm 套件，用 CopilotKit provider wrap 你的 App，傳入後端 endpoint 位址。加一個 CopilotSidebar 元件，聊天視窗就出現了。

Step 2 — 加 Hooks：

useCopilotAction — 前端在這裡定義「Agent 可以做什麼事」。比方說 create_campaign，Agent 在對話中就能自動呼叫這個 function 建立活動。
useCoAgentStateRender — Agent 後端的狀態可以直接 render 成 React 元件。比方說 Agent 正在查詢報表，你可以即時在聊天視窗裡顯示一個 loading 中的圖表骨架，資料回來就自動填進去。

這兩個 hook 就是 AG-UI 價值的核心體現：前端不需要知道 Agent 什麼時候會呼叫什麼、不需要 polling 狀態，全部都是 event-driven 即時更新。
-->

---
layout: two-cols
---

# 後端怎麼接？

<div class="text-sm text-gray-400 mb-2">Python + FastAPI</div>

```python {all|1-5|7-16|18-20}{lines:true}
from fastapi import FastAPI
from copilotkit.integrations.fastapi \
  import add_fastapi_endpoint
from copilotkit import \
  CopilotKitRemoteEndpoint, \
  LangGraphAgent

app = FastAPI()

sdk = CopilotKitRemoteEndpoint(
  agents=[
    LangGraphAgent(
      name="my_agent",
      graph=your_graph
    )
  ]
)

add_fastapi_endpoint(
  app, sdk, "/copilotkit"
)
```

::right::

<div class="ml-4 mt-8">

### 重點整理



| 重點          | 說明                                               |
| ------------- | -------------------------------------------------- |
| 一個 endpoint | `add_fastapi_endpoint` 掛上去就能跟前端溝通        |
| 框架自由選    | LangGraph、CrewAI、PydanticAI 都支援               |
| 也能不用框架  | `CopilotKitRemoteEndpoint` + `Action` 就夠輕量 POC |
| 前後端分離    | 前端 Vite + React，後端 FastAPI，各自部署          |



</div>

<!--
後端接法也很乾淨，幾乎不用動既有的 FastAPI 架構。

import 三個東西：FastAPI 本體、add_fastapi_endpoint 這個 helper、然後 CopilotKitRemoteEndpoint 和你要用的 Agent 框架（這邊用 LangGraph）。

建立 SDK 實例，把你的 agent graph 傳進去，然後一行 add_fastapi_endpoint 就把所有 AG-UI 事件串流的邏輯掛上去。

幾個重點：
- 前後端完全分離部署，前端 Vite + React，後端 FastAPI，各自獨立
- 框架自由選：你想用 LangGraph、CrewAI、PydanticAI 都行，換框架不影響前端接法
- 如果想最輕量的 POC，甚至不用任何框架，直接用 CopilotKitRemoteEndpoint + 幾個 Action 就夠了

這是整個系統中後端的「最後一哩路」— 讓你的 Python Agent 邏輯能透過標準協定跟前端溝通，而且不用自己管任何 SSE 的格式細節。
-->

---
layout: default
---

<div class="h-full flex flex-col">

<div>

# 架構全景

<div class="text-sm text-slate-400">前後端分離，一條 SSE 連線串起所有事</div>

</div>

<div class="flex-1 flex items-center justify-center">
  <img src="/architecture.svg" class="w-full" />
</div>

</div>

<!--
這張圖是整個系統的全景。

左邊是前端 — React + Vite，用 CopilotKit SDK 提供 Chat UI 跟各種 hooks。

中間是後端 — FastAPI，一個 CopilotKit endpoint 就接收所有來自前端的事件。

右邊是 Agent runtime — LangGraph 或 CrewAI，這裡跑 Agent 的實際邏輯：推理、工具呼叫、決策樹。

最右邊是 LLM — GPT-4、Claude，或任何支援的模型。

最重要的一條線是最左邊這兩條 — 前端到後端用的就是 AG-UI / SSE：
- 上面那條虛線是 request，使用者的訊息跟 action results 送到後端
- 下面那條實線是 response，AG-UI 事件流回來：messages、tool calls、state、HITL

架構設計原則：
- 前後端各自部署，不互相耦合
- 前端永遠不需要知道 Agent 內部在做什麼，只需要接 AG-UI 事件
- 換 Agent 框架（LangGraph → CrewAI）不影響前端任何 code
-->

---
layout: center
class: text-center
---

# Open Brainstorm


<div class="grid grid-cols-1 gap-3 text-left max-w-2xl mx-auto">

<div class="bs-card p-4 rounded-xl border border-white/[0.06] bg-white/[0.02] text-sm flex gap-3 items-start" style="animation-delay: 0.5s">
  <span class="text-teal-400 font-mono font-bold text-xs mt-0.5 shrink-0">01</span>
  <span class="text-slate-300">行銷人員設定活動要填一堆欄位，能不能用 Agent 對話引導完成？</span>
</div>

<div class="bs-card p-4 rounded-xl border border-white/[0.06] bg-white/[0.02] text-sm flex gap-3 items-start" style="animation-delay: 1.2s">
  <span class="text-emerald-400 font-mono font-bold text-xs mt-0.5 shrink-0">02</span>
  <span class="text-slate-300">報表頁面能不能讓行銷人員用中文問問題，Agent 即時產生圖表？</span>
</div>

<div class="bs-card p-4 rounded-xl border border-white/[0.06] bg-white/[0.02] text-sm flex gap-3 items-start" style="animation-delay: 1.9s">
  <span class="text-amber-400 font-mono font-bold text-xs mt-0.5 shrink-0">03</span>
  <span class="text-slate-300">素材上傳後，Agent 能不能自動辨識內容、建議分類和標籤？</span>
</div>

<div class="bs-card p-4 rounded-xl border border-white/[0.06] bg-white/[0.02] text-sm flex gap-3 items-start" style="animation-delay: 2.6s">
  <span class="text-violet-400 font-mono font-bold text-xs mt-0.5 shrink-0">04</span>
  <span class="text-slate-300">活動頁面設定能不能讓 Agent 即時預覽 + 調整？</span>
</div>

<div class="bs-card p-4 rounded-xl border border-teal-400/20 bg-teal-400/[0.04] text-sm flex gap-3 items-start" style="animation-delay: 3.3s">
  <span class="text-teal-400 font-mono font-bold text-xs mt-0.5 shrink-0">05</span>
  <span class="text-slate-200">如果要做一個 POC demo，最小可行範圍是什麼？</span>
</div>

</div>

<div class="mt-6 text-slate-600 text-xs font-mono bs-card" style="animation-delay: 4.2s">
throw ideas / no wrong answers / let's converge
</div>

<style>
  .bs-card {
    opacity: 0;
    animation: bs-slide-in 0.6s cubic-bezier(0.16, 1, 0.3, 1) forwards;
  }
  @keyframes bs-slide-in {
    from {
      opacity: 0;
      transform: translateX(-20px);
    }
    to {
      opacity: 1;
      transform: translateX(0);
    }
  }
</style>

<!--
好，前半段講完了。現在開放討論。

我先拋幾個方向：

01 — 對話式設定活動。行銷人員每次設定活動都要填一大堆欄位，很多都是重複性的決策。如果能讓 Agent 用問答引導完成，不但體驗好，還能避免漏填。

02 — 自然語言報表查詢。現在看報表要自己選維度、篩選條件，不直覺。讓使用者直接問「上週哪個 SMS 活動 CTR 最高？」，Agent 直接回一張圖表，比較好用。

03 — 素材自動標記。圖片上傳之後，Agent 辨識內容、建議分類和標籤，大幅降低人工整理成本。

04 — 活動頁即時預覽。設定活動頁面的內容時，Agent 可以即時渲染預覽，使用者邊改邊看效果。

05 — POC 最小範圍。前四個都做太大，如果我們想先驗證可行性，最小的切入點是什麼？一個功能、兩週、一個人可以跑出 demo？

大家可以針對這些延伸，或是提自己的想法。沒有對錯，先把點子丟出來。
-->
