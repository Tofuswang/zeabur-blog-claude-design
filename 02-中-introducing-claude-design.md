# Claude Design 完整介紹：從一段對話到可實作的設計稿

> 層級：中 — Introducing Claude for Design
> 目標搜尋意圖：Investigational — 想評估 Claude Design 這個工具
> 關鍵字方向：Claude Design 是什麼, Claude Design 教學, Claude Design vs Figma, Claude Design handoff

---

## Claude Design 是什麼？跟 Artifacts 有什麼不同？

如果你用過 Claude 的 Artifacts 功能 — 在聊天過程中即時產生一段可預覽的 HTML — 你可能會以為 Claude Design 只是它的升級版。但兩者的定位完全不同。

Artifacts 是對話的副產物。你問 Claude 一個問題，它順手產出一段 code 讓你預覽，對話結束之後那段 code 就留在聊天記錄裡，沒有自己的生命週期。Claude Design 則是一個獨立的設計工作區，有持久的 canvas，你可以在上面反覆迭代、累積設計資產、最後輸出一個完整的 handoff bundle 交給開發工具。

具體來說，差異在三個層面。第一，Artifacts 產出的是一次性的 code snippet，Claude Design 產出的是一個完整的專案結構，包含元件拆分、CSS 變數系統、圖片資產和 SVG。第二，Artifacts 沒有狀態，每次對話都是從零開始；Claude Design 會記住你的設計系統、你選過的配色和字型、你上傳過的素材。第三，Artifacts 的產出只能在聊天介面裡預覽；Claude Design 的 handoff bundle 是設計來被 Claude Code 或其他 coding agent 直接消費的，裡面有明確的 README 告訴 agent 怎麼讀、從哪個檔案開始。

## 怎麼讓 Claude Design 讀懂你的品牌？

Claude Design 最有價值的能力之一是它可以從既有素材裡提煉設計語言。你不需要自己寫一份設計規範，只要把相關的東西丟給它，它會自己歸納。

可以餵給它的素材包括：

- **現有網站的網址** — 它會讀取頁面結構、配色、字型選擇，理解你目前的視覺風格
- **程式碼** — 如果你有現成的 codebase，它會從 CSS 變數、元件結構裡提取設計系統
- **Logo 和品牌圖片** — 它會分析色彩組成，衍生出一套配色方案
- **競品或參考網站** — 告訴它你喜歡某個網站的哪個部分，它會抽取那個元素的設計邏輯

舉一個實際的例子：我們把一家製造業公司的舊網站網址和 logo 丟給 Claude Design，它從 logo 的紅藍黃三原色裡衍生出四組完整的配色方案 — 品牌藍、鍛橘、深紅、石墨灰 — 每一組都包含主色、輔助色、背景色和文字色的完整定義。這些不是隨機生成的顏色，而是從品牌既有的視覺資產裡有邏輯地推導出來的。

## 一次對話拿到三個設計方向：Tweaks Panel 怎麼用

Claude Design 的設計產出不是一個固定的 mockup，而是一套可調參的系統。

當你給它一個設計任務，它通常會產出多個設計方向（Direction），每個方向代表一種不同的視覺調性和佈局策略。以一個 B2B 企業官網的案例來說，我們拿到了三個方向：

- **Direction A：Clean Industrial** — 白底、大量留白、大字型，走 Apple/Tesla 式的極簡工業感
- **Direction B** — 另一種佈局策略和視覺重量分配
- **Direction C** — 第三種取向

每個方向都附帶一個 Tweaks Panel。這個面板讓你可以即時切換：

- **配色方案** — 點一下就能在四組配色之間切換，立刻看到整個頁面的視覺變化
- **字型組合** — 在 Inter Tight（現代無襯線）、Sora（幾何無襯線）、Source Serif（編輯感襯線）之間切換
- **Hero 文案變體** — 同一個版面，換上不同角度的標題和副標題
- **區塊開關** — 可以打開或關閉特定的頁面模組，像是數據條、產業列表等

這等於一次拿到了數十種排列組合。傳統流程裡你可能需要請設計師分別做三版 mockup、每版兩種配色、再加上文案 A/B test，光是產出這些交付物就是一週的工作量。在 Claude Design 裡，這些都是即時可切換的參數。

## Handoff Bundle：從設計稿到 Claude Code 實作

當你在 Claude Design 裡定案之後，它會輸出一個 handoff bundle。這個 bundle 的結構是刻意設計來給 coding agent 讀的，不是給人讀的。

一個典型的 handoff bundle 包含：

```
project/
├── Yea Shinn Site A.html    ← 主要設計檔（進入點）
├── src/
│   ├── shared.jsx            ← 共用元件（Nav、Footer 等）
│   ├── direction-a.jsx       ← 設計方向 A 的完整實作
│   ├── direction-b.jsx       ← 設計方向 B
│   ├── direction-c.jsx       ← 設計方向 C
│   ├── product-page.jsx      ← 額外頁面：產品詳情
│   ├── rfq-form.jsx          ← 額外頁面：報價表單
│   ├── logo-paths.js         ← SVG logo 路徑資料
│   └── yea-shinn-logo.svg    ← 向量 logo
├── images/                   ← 所有圖片資產
├── screenshots/              ← 設計對比截圖
└── README.md                 ← 給 coding agent 的指示
```

README 裡會明確告訴 coding agent：「讀 `Yea Shinn Site A.html`，這是使用者觸發 handoff 時打開的檔案，從上到下讀完，然後順著它的 import 把所有元件都讀一遍，理解整體結構之後再開始實作。」

這裡有一個關鍵的設計哲學：handoff bundle 裡的 code 是 prototype，不是 production code。它用的是 CDN 載入的 React + Babel 即時編譯，目的是讓設計在瀏覽器裡能直接跑起來預覽，但不適合直接部署。你的 coding agent（例如 Claude Code）的任務是「看著這個 prototype 的視覺產出，用適合目標 codebase 的技術重新實作」— 匹配視覺結果，而不是複製 prototype 的內部結構。

把這個 bundle 丟給 Claude Code，它會讀完所有檔案、理解設計意圖、然後用你指定的技術棧（純 HTML、React、Next.js 都行）重新寫一份 production-ready 的程式碼。從設計到可部署的 code，中間不需要任何人工的 design-to-code 翻譯。

## Claude Design vs Figma vs v0：什麼時候用哪個？

這三個工具解決的問題不同，不是互相取代的關係。

**Figma** 的核心價值在協作和精確控制。當你有一個設計團隊、需要建立和維護一套設計系統、需要多人同時編輯和評論、需要對每一個 pixel 有精確的控制權，Figma 仍然是最合適的工具。它的 plugin 生態系和 Dev Mode 也讓設計到開發的 handoff 更系統化。

**v0 by Vercel** 擅長的是快速生成 UI 元件和頁面。你描述一個元件或頁面，它直接產出可以貼進 Next.js 專案的 code。它的優勢在於跟 Vercel 生態系的深度整合 — 產出的 code 天生就是 Next.js + Tailwind + shadcn/ui 的風格。適合你已經在用 Vercel 技術棧、需要快速產出單一元件或頁面的場景。

**Claude Design** 的獨特定位在於「設計探索 + 系統化產出」。它不只產出一個方向，而是產出多個可比較的方向，每個方向都帶有可調參數。它的 handoff bundle 不綁定特定框架，你可以用任何技術棧來實作。適合你還在探索設計方向、需要快速比較多個方案、或者需要從零建立一個完整設計系統的場景。

簡單的決策框架：

| 場景 | 推薦工具 |
|---|---|
| 團隊協作、設計系統維護 | Figma |
| 快速產出 Next.js 元件 | v0 |
| 探索設計方向、完整設計專案 | Claude Design |
| 已有明確設計，需要快速實作 | Claude Code（直接寫） |

在實際工作流裡，這些工具不是互斥的。你可能在 Claude Design 裡探索方向、在 Figma 裡做精確微調、用 Claude Code 來實作。選擇的依據是你當下處在設計流程的哪個階段，而不是對某個工具的忠誠度。
