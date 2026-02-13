# 開發指南：AI 協作學習開發方法

> **目標讀者**：想要透過 AI 協作學習並開發專案的開發者
> **核心理念**：學習與開發並存，而非只是讓 AI 做完所有事
> **更新日期**：2026-02-13

---

## 目錄

1. [核心理念](#核心理念)
2. [AI 協作學習模式](#ai-協作學習模式)
3. [完整開發流程](#完整開發流程)
   - [階段 1：探索與理解](#階段-1探索與理解)
   - [階段 2：規格化](#階段-2規格化)
   - [階段 3：規格審核與調整](#階段-3規格審核與調整)
   - [階段 4：格式驗證](#階段-4格式驗證)
   - [階段 5：UI/UX 設計](#階段-5uiux-設計使用-pencil)
   - [階段 6：學習式實作](#階段-6學習式實作)
   - [階段 7：驗證與封存](#階段-7驗證與封存)
4. [知識管理架構](#知識管理架構)
5. [學習記錄策略](#學習記錄策略)
6. [多人協作指南](#多人協作指南)
7. [實戰範例：報名系統開發](#實戰範例報名系統開發)
8. [檢查清單與最佳實踐](#檢查清單與最佳實踐)
9. [總結](#總結)

---

## 核心理念

### 這不是普通的 AI 編程

傳統的 AI 輔助開發：
```
你：「幫我寫一個報名系統」
AI：[寫了 500 行程式碼]
你：複製貼上
結果：功能完成了，但你什麼都沒學到
```

我們的方法：
```
你：「解釋報名系統的設計思路」
AI：[解釋概念、畫圖、說明取捨]
你：理解了，自己實作核心邏輯
AI：審核你的程式碼，給建議
結果：功能完成了，而且你學會了
```

### 三個核心原則

#### 1. AI 是導師，不是代筆

```
❌ 錯誤心態                    ✅ 正確心態
「幫我做」                    「教我怎麼做」
「給我程式碼」                「解釋為什麼這樣設計」
「我只要結果」                「我要理解過程」
```

#### 2. 學習優先於速度

```
快但不懂                      慢但扎實
════════                     ════════
2 週完成                      4 週完成
AI 寫 90%                     你寫 60%、AI 寫 40%
你只是貼上                    你理解每一行

3 個月後：                    3 個月後：
還是不會                      可以獨立開發
```

#### 3. 主動探索而非被動接受

```
被動學習者                    主動學習者
═══════════                  ═══════════
等 AI 給答案                  問「為什麼」
接受第一個方案                思考其他可能性
遇到錯誤就卡住                把錯誤當學習機會
```

---

## AI 協作學習模式

**核心概念**：根據你對技術的熟悉程度，選擇不同的協作模式。

我們簡化成**兩個模式**，讓選擇更清楚、學習更有效。

### 模式 A：Learn+（學習 + 練習模式）

**適用時機**：不熟悉或半熟悉的技術（無法獨立實作 70% 以上）

**核心理念**：
- 先看到「正確的寫法」（避免養成壞習慣）
- 再「動手練習」（保留學習深度）
- 最後「對比學習」（理解為什麼）

**完整流程**：

```
階段 1：Explain（理解概念）
───────────────────────────────────
你：「Explain how Prisma transactions work
     and why we need them for the registration system.
     Walk me through step by step.」

AI：
- 什麼是 Transaction
- 為什麼需要（原子性、一致性）
- 使用時機
- 常見陷阱

你：持續提問直到理解
───────────────────────────────────

階段 2：Show & Explain（展示標準版）
───────────────────────────────────
你：「Show me the standard pattern and explain line by line」

AI：展示業界標準寫法 + 逐行解釋
```typescript
await prisma.$transaction(async (tx) => {
  // 第 1 行：為什麼用 async (tx)
  // 第 2 行：為什麼用 tx 不是 prisma
  // ...
})
```

你：理解每一行的「為什麼」
───────────────────────────────────

階段 3：Guided Practice（挖空填空）
───────────────────────────────────
AI：「現在我把核心邏輯挖空，你參考剛才的解釋填入」

```typescript
await prisma.$transaction(async (tx) => {
  // TODO: 查詢當前報名數

  // TODO: 檢查容量

  // TODO: 建立報名記錄
})
```

你：填入 TODO（模仿 pattern，不是猜測）
───────────────────────────────────

階段 4：Compare & Reflect（對比學習）
───────────────────────────────────
你：「Review my implementation」

AI：對比你的版本和標準版
- 你寫對的部分 ✓
- 可以改進的地方
- 為什麼標準版更好

你：修正並理解差異
```

**範例對話**：

```
你：「Explain Next.js Server Components vs Client Components.
     I'm new to this concept.」

AI：[詳細解釋]
    - Server Components 在伺服器執行，可以直接查資料庫
    - Client Components 在瀏覽器執行，可以有互動
    - 何時用哪個...

你：「So if I want to show a list of events, should I use
     Server Component?」

AI：「Yes! Because:
     - Event list is mostly static
     - You can fetch from database directly
     - No interactive state needed
     - Better SEO and performance」

你：「What about the 'Register' button?」

AI：「That needs to be a Client Component because:
     - It handles onClick events
     - It manages loading state
     - It shows toast notifications」

你：「Got it. Let me try implementing the events page.」

[你寫完後]

你：「Review my code. Is this the correct pattern?」

AI：[審核並給建議]
```

**時間估計**：比自己寫慢 1.5-2 倍，但**你真的學會了**

---

### 模式 B：Review（審核模式）

**適用時機**：你已經能獨立實作這個功能的 70% 以上，只是需要加速開發

**核心理念**：讓 AI 產出標準實作，你透過審核和提問來確保品質並學習最佳實踐

**流程**：

```
階段 1：AI 直接實作
───────────────────────────────────
你：「Implement the leaderboard component
     with monthly and total rankings」

階段 2：AI 寫完整程式碼
───────────────────────────────────
AI：[提供完整實作]

階段 3：你逐行審核
───────────────────────────────────
仔細閱讀每一行
理解設計決策
找出不合理的地方

階段 4：你提問深入理解
───────────────────────────────────
你：「Why did you use useMemo here?」
你：「Is this the most efficient way to sort?」
你：「What if there are 10,000 users?」

階段 5：你決定是否修改
───────────────────────────────────
如果不滿意 → 要求 AI 調整
如果 OK → 接受並理解為什麼這樣寫
```

**範例對話**：

```
AI：[實作排行榜元件]

你：「I see you're fetching leaderboard data on every render.
     Won't this cause performance issues?」

AI：「Good catch! You're right. Let me add:
     1. useSWR for caching and revalidation
     2. Debounce for tab switching
     3. Server-side caching with Next.js」

你：「Better. But why useSWR instead of React Query?」

AI：「In this case, useSWR is simpler because:
     - Less bundle size
     - Fits well with Next.js
     - Built-in revalidation strategies
     But React Query would also work fine.」

你：「Makes sense. One more thing - the sorting logic
     looks complex. Can you explain the algorithm?」

AI：[解釋排序邏輯]
```

**時間估計**：比自己寫快 50-70%，**學習效果 40%**（主要是審核和提問）

---

### 如何選擇模式？

```
技術熟悉度判斷
════════════════════════════════════

問自己：
「如果現在關掉 AI，我能獨立實作這個功能的 70% 以上嗎？」

┌─────────────────────────────────────┐
│  不行，我不熟悉這個技術              │
│  → Learn+ 模式                       │
│  → 從概念理解開始                    │
│  → 看標準版後再練習                  │
│  → 學習效果最好                      │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│  可以，我已經很熟悉了                │
│  → Review 模式                       │
│  → AI 直接實作                       │
│  → 你審核並提問                      │
│  → 開發速度最快                      │
└─────────────────────────────────────┘
```

**判斷標準範例**：

| 技術 | 判斷標準 | 建議模式 |
|------|---------|---------|
| **Prisma Transaction** | 第一次用 Transaction | Learn+ |
| **Next.js API Route** | 寫過 3-5 個 API Route | Review |
| **React Hook Form** | 只用過 useState | Learn+ |
| **Tailwind CSS** | 已經熟悉 utility class | Review |
| **Zod Validation** | 看過文件但沒用過 | Learn+ |

**重要提醒**：
- **不要高估自己的熟悉度** - 寧可用 Learn+，確保真正理解
- **70% 是關鍵門檻** - 如果無法獨立完成 70%，就用 Learn+
- **隨著專案進行，逐漸轉移** - 開始多用 Learn+，後期多用 Review
- **同一個 task 可以混用** - 熟悉的部分用 Review，不熟的用 Learn+

---

## 完整開發流程

基於 OpenSpec 的 7 階段工作流程，整合 UI/UX 設計與 AI 協作學習。

### 流程總覽

```
功能開發循環
════════════════════════════════════════

階段 1: 探索與理解 (/opsx:explore)
  ↓
階段 2: 規格化 (/opsx:ff <feature>)
  ↓
階段 3: 規格審核與調整
  ↓
階段 4: 格式驗證 (openspec validate)
  ↓
階段 5: UI/UX 設計 (Pencil)              ← 新增！
  ↓
階段 6: 學習式實作 (/opsx:apply)
  ↓
階段 7: 驗證與封存 (/opsx:verify + /opsx:archive)
  ↓
[寫學習筆記 5 分鐘]
  ↓
下一個功能 or 完成專案
```

---

### 階段 1：探索與理解

**目標**：在寫任何程式碼前，先理解「為什麼」和「怎麼做」

**指令**：`/opsx:explore`

**你做什麼**：
1. 描述你想做的功能
2. 說出你目前的理解
3. 提出你的疑問和不確定的地方

**AI 做什麼**：
- 畫圖解釋概念
- 提出不同的設計選項
- 討論優缺點和取捨
- 挑戰你的假設
- 解釋相關技術

**輸出**：
- 清晰的功能邊界
- 設計方向共識
- 你理解了「為什麼要這樣做」

**範例對話**：

```
你：「/opsx:explore 報名系統」

你：「我想做一個報名功能，使用者點按鈕就報名。
     我理解需要：
     1. 檢查使用者是否登入
     2. 建立報名記錄
     3. 顯示成功訊息

     但我不確定：
     - 如何防止重複報名？
     - 如何處理活動額滿？
     - QR Code 要怎麼產生？」

AI：「很好的起點！讓我們逐一討論...」

[AI 畫圖解釋流程]

   使用者點擊報名
          ↓
   檢查登入狀態 ← NextAuth
          ↓
   檢查是否已報名 ← Prisma unique constraint
          ↓
   檢查活動容量 ← Transaction
          ↓
   建立報名記錄 + 生成 QR Token
          ↓
   顯示成功 + QR Code

AI：「關於你的疑問：

1. 防止重複報名：
   - 資料庫層：Prisma unique constraint
   - 應用層：查詢後再建立（但有 race condition）
   - 最佳方案：用 Transaction 包裹整個流程

2. 活動額滿：
   - 查詢當前報名數
   - 在 Transaction 內檢查容量
   - 超過就回傳錯誤

3. QR Code：
   - 產生隨機 token（crypto.randomBytes）
   - 存在報名記錄
   - 用 qrcode 套件產生圖片」

你：「Transaction 是什麼？為什麼需要？」

AI：[解釋 Transaction 概念]

你：「了解了。那 QR Code 裡面要放什麼資料？」

AI：[討論 QR Code 設計]

...持續討論到你完全理解...
```

**檢查點**：
- ✅ 我能向別人解釋這個功能的設計嗎？
- ✅ 我知道會用到哪些技術嗎？
- ✅ 我理解為什麼這樣設計嗎？

---

### 階段 2：規格化

**目標**：將討論的內容轉化成結構化的規格文件

**指令**：`/opsx:ff <feature-name>`

**流程**：
```bash
/opsx:ff registration-system
```

**AI 做什麼**：
自動生成以下文件在 `openspec/changes/registration-system/`：

1. **proposal.md**
   - 這個 change 要做什麼
   - 為什麼要做
   - 預期成果

2. **design.md**
   - 設計決策
   - 技術選擇
   - 架構圖

3. **spec.md**
   - 詳細規格
   - API 介面
   - 資料結構
   - 邊界條件

4. **tasks.md**
   - 任務分解
   - 實作順序
   - 預估難度

**你做什麼**：
- 等待 AI 生成完成
- 準備進入下一階段審核

**時間**：2-5 分鐘（AI 自動生成）

---

### 階段 3：規格審核與調整

**目標**：確保生成的規格符合需求，沒有遺漏或錯誤

**流程**：

```
1. 閱讀所有生成的文件
   ───────────────────────────────────
   逐一打開：
   - proposal.md
   - design.md
   - spec.md
   - tasks.md

2. 檢查內容
   ───────────────────────────────────
   問自己：
   - 這些是我想要的功能嗎？
   - 有沒有遺漏的部分？
   - 有沒有不合理的設計？
   - 任務拆分夠細嗎？

3. 提出修改意見
   ───────────────────────────────────
   你：「spec.md 裡沒有提到 Email 通知，
        報名成功後應該要發確認信」

   你：「tasks.md 的 Task 3 太大了，
        應該拆成兩個：
        - Task 3a: QR Code 生成
        - Task 3b: Email 整合」

4. AI 調整
   ───────────────────────────────────
   AI 根據你的意見修改文件

5. 重複 1-4 直到滿意
   ───────────────────────────────────
```

**審核清單**：

```markdown
proposal.md
- [ ] 目標明確
- [ ] 範圍清楚（做什麼、不做什麼）
- [ ] 與整體專案目標一致

design.md
- [ ] 設計決策有說明理由
- [ ] 考慮了不同選項
- [ ] 架構圖清楚

spec.md
- [ ] 功能描述完整
- [ ] API 介面定義清楚
- [ ] 考慮了 edge cases
- [ ] 錯誤處理有定義

tasks.md
- [ ] 任務拆分合理（不太大不太小）
- [ ] 有清楚的順序
- [ ] 每個 task 目標明確
```

**時間**：20-30 分鐘

---

### 階段 4：格式驗證

**目標**：確保規格文件符合 OpenSpec 標準格式

**指令**：
```bash
openspec validate registration-system
```

**系統檢查**：
- ✅ 必要欄位是否存在
- ✅ 格式是否正確
- ✅ Scenarios 是否定義
- ✅ 規範術語是否正確使用（SHALL/MUST）

**如果驗證失敗**：
```
錯誤訊息範例：
  ✗ spec.md 缺少 "Acceptance Criteria" 章節
  ✗ tasks.md 中 Task 2 缺少 "Definition of Done"

→ 回到階段 3 修正
→ 重新執行 validate
```

**如果驗證通過**：
```
✓ All validations passed
✓ registration-system is ready for implementation

→ 進入階段 5 實作
```

**時間**：1-2 分鐘

---

### 階段 5：UI/UX 設計（使用 Pencil）

**目標**：根據 design.md 的定義，設計所有頁面和元件的 UI/UX

**時機**：在規格驗證通過後，實作之前

**為什麼在這個階段？**

```
有了 design.md 才知道：
✅ 要顯示哪些資料（資料結構）
✅ 有哪些操作（API endpoints）
✅ 狀態如何變化（業務邏輯）

才能設計出：
→ 正確的表單欄位
→ 合理的互動流程
→ 需要的狀態提示
```

**設計工具**：Pencil + shadcn/ui 設計系統

**完整流程**：

```
1. 列出需要設計的頁面/元件
   ───────────────────────────────────
   根據 spec.md 和 tasks.md：
   - 活動列表頁 (event-list.pen)
   - 活動詳情頁 (event-detail.pen)
   - 報名按鈕元件 (register-button.pen)
   - QR Code 顯示 (qr-display.pen)
   ...

2. 使用 Pencil 開始設計
   ───────────────────────────────────
   你：「Create a new Pencil design for event list page.
        Use shadcn/ui design system.

        Based on design.md, each event should show:
        - Title
        - Date and time
        - Location
        - Current registrations / capacity
        - Tags (category)
        - Register button」

   AI：[使用 Pencil + shadcn/ui]
       - 選擇合適的 shadcn 元件 (Card, Badge, Button)
       - 設計響應式布局
       - 設定顏色、間距、字體
       - 生成 event-list.pen

3. 審核設計稿
   ───────────────────────────────────
   你：「Show me a screenshot of event-list.pen」

   AI：[顯示設計稿截圖]

   你：檢查：
       - 資訊是否完整？
       - 布局是否合理？
       - 符合 shadcn/ui 風格？
       - 響應式設計是否正確？

   你：「Adjust the spacing between cards to 24px」
   AI：[修改設計]

4. 儲存設計檔案
   ───────────────────────────────────
   檔案位置：
   openspec/changes/<change-name>/designs/

   例如：
   openspec/changes/registration-system/designs/
   ├── event-list.pen
   ├── event-detail.pen
   ├── register-button.pen
   └── qr-display.pen

5. 更新 tasks.md
   ───────────────────────────────────
   在相關 task 加入設計檔案參考：

   Task 1: 實作活動列表頁
   - **設計稿**：designs/event-list.pen
   - 使用 shadcn/ui Card, Badge 元件
   - 實作響應式 grid 布局
   ...
```

**範例對話**：

```
你：「Let's design the event detail page.

     According to design.md, it should display:
     - Event header (title, image, date, location)
     - Description
     - Organizer info
     - Registration section with:
       - Current count / capacity
       - Register button (if not full)
       - QR code (if already registered)

     Use shadcn/ui components and follow our Tailwind setup.」

AI：[建立 Pencil 設計]
    「I've created event-detail.pen with:

     Layout:
     - Hero section with cover image
     - Two-column layout (details + sidebar)
     - Sticky registration card on sidebar

     Components used:
     - Card for main content
     - Badge for status
     - Button for registration
     - Separator for sections

     Let me show you a screenshot...」

你：「Good! But can you make the registration card
     more prominent? Maybe add a subtle shadow?」

AI：[調整設計]
    「Updated with shadow-lg on the registration card.
     Here's the new version...」

你：「Perfect. Save it and let's move to the next page.」
```

**檢查點**：
- ✅ 所有頁面都有設計稿？
- ✅ 設計符合 shadcn/ui 風格？
- ✅ 使用的元件在 shadcn/ui 中都有？
- ✅ 響應式設計考慮了嗎？
- ✅ tasks.md 已加入設計檔案參考？

**時間**：2-4 小時（取決於頁面數量）

**注意事項**：
- 使用 shadcn/ui 設計系統確保一致性
- 設計時參考 design.md 的資料結構
- 考慮不同狀態（loading, empty, error）
- 響應式設計（mobile, tablet, desktop）

---

### 階段 6：學習式實作

**目標**：用選定的學習模式，一次完成一個 task

**指令**：`/opsx:apply`

**完整流程**：

```
1. 選擇一個 task（從 tasks.md）
   ───────────────────────────────────
   原則：依照順序，從簡單到困難

   例如：
   Task 1: Create Registration Prisma model
   Task 2: Implement registration API route
   Task 3: Create register button component
   Task 4: Generate QR Code
   ...

2. 判斷熟悉度 → 選擇模式
   ───────────────────────────────────
   Task 1 (Prisma model):
     我會基本 Prisma，但不確定 relation
     → Learn+ 模式（學習關聯設定）

   Task 2 (API route):
     完全不懂 Next.js API Routes
     → Learn+ 模式（從概念開始）

   Task 3 (React component):
     我已經很熟悉 React
     → Review 模式（加速開發）

3. 執行選定的模式
   ───────────────────────────────────
   [參考上面「AI 協作學習模式」的 Learn+ 或 Review]

4. 完成後測試
   ───────────────────────────────────
   - 手動測試功能
   - 寫簡單的單元測試（選擇性）
   - 確認符合 task 的 Definition of Done

5. 標記 task 完成
   ───────────────────────────────────
   在 tasks.md 打勾或標記狀態

6. 重複 1-5 直到所有 tasks 完成
   ───────────────────────────────────
```

**Learn+ 模式範例（Task 2: API Route）**：

```
你：「Explain how Next.js 14 API Routes work.
     I need to create POST /api/events/[id]/register」

AI：[解釋概念]
    - 檔案位置：app/api/events/[id]/register/route.ts
    - 匯出 async function POST(req, { params })
    - req 可以用 req.json() 取得 body
    - params.id 是動態參數
    - 回傳 Response.json()

你：「How do I check if user is logged in?」

AI：「Use NextAuth's auth() function:
     import { auth } from '@/lib/auth'

     const session = await auth()
     if (!session) {
       return Response.json({ error: 'Unauthorized' }, { status: 401 })
     }」

你：「Got it. Let me try implementing it.」

[你實作後]

你：「Review my implementation」

AI：[審核並給 feedback]
```

**Learn+ 模式範例（Task 1: Prisma Model）**：

```
你：「I'm not familiar with Prisma relations.
     First explain the concept, then show me the standard pattern,
     then I'll practice with guided TODOs.」

AI：[階段 1: Explain 概念]
    - 什麼是 relation
    - 一對多 vs 多對一
    - onDelete cascade 的用途

AI：[階段 2: Show & Explain 標準版]
    model Registration {
      id String @id @default(cuid())
      userId String
      eventId String

      user User @relation(fields: [userId], references: [id])
      // 解釋：為什麼這樣寫...

      @@unique([userId, eventId])
      // 解釋：防止重複報名...
    }

AI：[階段 3: Guided Practice 挖空填空]
    model Registration {
      id String @id @default(cuid())

      // TODO: Add relation to User
      // TODO: Add relation to Event
      // TODO: Add unique constraint

      createdAt DateTime @default(now())
    }

你：[填入 TODO，參考剛才的標準版]

AI：[階段 4: Compare & Reflect 對比]
    「你寫對了 90%！建議：
     1. Add @@index([eventId]) for queries
     2. Consider adding onDelete: Cascade」
```

**Learn+ 模式範例（Task 3: 活動列表頁 UI - 使用 Pencil）**：

```
你：「I'm not familiar with shadcn/ui components.
     I need to implement the event list page based on
     designs/event-list.pen.

     First, explain the components used in the design,
     then generate code from the Pencil design.」

AI：[階段 1: Explain 概念]
    「Let me analyze the Pencil design...

     The design uses these shadcn/ui components:
     1. Card - Container for each event
     2. Badge - For event category/status
     3. Button - For registration action

     Layout structure:
     - Grid layout (responsive: 1 col mobile, 2 cols tablet, 3 cols desktop)
     - Each card has: image, title, date, location, capacity, button」

AI：[階段 2: Show & Explain 標準版]
    「Here's the code generated from the Pencil design:」

    ```tsx
    import { Card, CardHeader, CardTitle, CardContent } from '@/components/ui/card'
    import { Badge } from '@/components/ui/badge'
    import { Button } from '@/components/ui/button'

    export function EventList({ events }: { events: Event[] }) {
      return (
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          {/* 解釋：Tailwind responsive classes */}
          {events.map((event) => (
            <Card key={event.id}>
              {/* 解釋：shadcn Card 結構 */}
              <CardHeader>
                <Badge>{event.category}</Badge>
                {/* 解釋：Badge 的用途 */}
                <CardTitle>{event.title}</CardTitle>
              </CardHeader>
              <CardContent>
                <p>{event.date}</p>
                <p>{event.location}</p>
                <Button>報名</Button>
                {/* 解釋：為什麼這樣組織 */}
              </CardContent>
            </Card>
          ))}
        </div>
      )
    }
    ```

AI：[階段 3: Guided Practice 挖空填空]
    「Now try implementing the event card yourself.
     I'll provide the structure, you fill in the components:」

    ```tsx
    export function EventList({ events }: { events: Event[] }) {
      return (
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          {events.map((event) => (
            // TODO: Use Card component here

              // TODO: Use CardHeader with Badge and CardTitle

              // TODO: Use CardContent with event details

              // TODO: Add Button for registration

            // TODO: Close Card component
          ))}
        </div>
      )
    }
    ```

你：[填入 TODO，參考 Pencil 設計稿和標準版]

AI：[階段 4: Compare & Reflect 對比]
    「Great job! Your implementation matches the Pencil design.
     A few enhancements:

     1. Add hover effect: className="hover:shadow-lg transition"
     2. Show capacity: {event.registered}/{event.capacity}
     3. Disable button if full: disabled={event.registered >= event.capacity}

     These details were in the Pencil design - good catch if you added them!」
```

**Review 模式範例（Task 4: 報名按鈕 - 使用 Pencil）**：

```
你：「I'm familiar with React and shadcn/ui.
     I'll implement the register button component
     based on designs/register-button.pen」

[你參考 Pencil 設計稿，自己實作]

你：「I have a question about the loading state.
     In the Pencil design, the button shows a spinner.
     Should I use the shadcn Button's loading prop?」

AI：「Yes! The design uses shadcn's built-in loading state:

     <Button disabled={loading}>
       {loading ? <Loader2 className="animate-spin" /> : '報名'}
     </Button>

     But you could also use a separate LoadingButton if you prefer.」

你：「Got it. Here's my implementation, please review.」

AI：[審核]
    「Looks good! Matches the Pencil design perfectly.

     One suggestion: Add optimistic UI update -
     immediately show 'loading' state on click,
     before the API responds.

     Also noticed in the design there's an error state (red border).
     Did you implement that?」

你：「Not yet, let me add the error state...」
```

**時間**：視 task 數量和複雜度，每個 task 1-4 小時

---

### 階段 7：驗證與封存

**目標**：確認實作完成，封存這個 change

**指令**：
```bash
/opsx:verify
/opsx:archive
```

**驗證流程**：

```
1. 執行 /opsx:verify
   ───────────────────────────────────
   AI 檢查：
   - 所有 tasks 都完成了嗎？
   - 實作符合 spec 嗎？
   - 有遺漏的功能嗎？

2. 手動測試
   ───────────────────────────────────
   你自己測試：
   - 功能正常運作
   - 沒有明顯 bug
   - UX 流程順暢

3. 如果驗證通過
   ───────────────────────────────────
   執行 /opsx:archive
   → 封存這個 change
   → 更新 main specs（如果需要）

4. 如果驗證未通過
   ───────────────────────────────────
   → 回到階段 5 補完
   → 重新驗證
```

**封存後**：
- `openspec/changes/registration-system/` 標記為已完成
- 這個 change 的規格更新到 `openspec/specs/`（如果適用）

**時間**：10-20 分鐘

---

### 完成後：學習記錄（5-10 分鐘）

**目標**：花 5-10 分鐘記錄你的學習，為未來的你和其他人留下知識

**流程**：

```
1. 要求 AI 總結
   ───────────────────────────────────
   你：「請總結我在 registration-system 這個 change
        學到的關鍵概念，包括：
        - 新技術或模式
        - 遇到的問題與解法
        - 下次可以改進的地方

        用 3-5 個 bullet points，保持簡潔」

2. AI 生成總結
   ───────────────────────────────────
   AI：
   ## 新學到的概念
   - Prisma Transaction 確保報名建立的原子性
   - Next.js API Route 的錯誤處理模式
   - QR Code 生成使用 crypto.randomBytes 確保唯一性

   ## 遇到的問題
   - 容量檢查的競爭條件 → 用 Transaction 解決
   - QR Token 要夠安全 → 64 字元隨機字串

   ## 下次改進
   - Validation 可以抽成 helper function 重用
   - Error messages 應該更明確給使用者看

3. 你審核並補充（2 分鐘）
   ───────────────────────────────────
   加上你覺得重要但 AI 沒提到的

4. 存檔（1 分鐘）
   ───────────────────────────────────
   複製到：
   openspec/changes/registration-system/learnings.md

5. 重要 patterns 更新 MEMORY.md（選擇性）
   ───────────────────────────────────
   如果這個學習會在其他 change 用到：

   例如：
   「Prisma Transaction 的標準模式」
   → 記錄到 MEMORY.md
   → 之後每個 change 都能參考
```

**learnings.md 範例**：

```markdown
# Learnings: Registration System

## 新學到的概念
- Prisma Transaction 確保報名建立的原子性，避免超收
- Next.js API Route 的錯誤處理：用 try-catch + Response.json
- QR Code token 用 crypto.randomBytes(32).toString('hex')

## 遇到的問題與解法

### 問題 1：容量檢查的競爭條件
**情境**：兩人同時報名最後一個名額
**解法**：用 Prisma $transaction 包裹查詢和建立
```typescript
await prisma.$transaction(async (tx) => {
  const count = await tx.registration.count({ where: { eventId } })
  if (count >= capacity) throw new Error('Full')
  return tx.registration.create({ data: ... })
})
```

### 問題 2：QR Token 安全性
**原本想法**：用 registrationId 就好
**問題**：容易被猜測，可以偽造
**解法**：額外產生隨機 token，掃描時要驗證

## 下次可以改進
- Validation schema 可以抽成 `lib/validations/registration.ts`
- Error handling 可以用統一的 `ApiError` class
- QR Code 圖片可以快取，不用每次都生成

## 相關資源
- Prisma Transaction 文件：https://...
- Next.js API Routes：https://...
```

**為什麼要寫？**
- ✅ 幫助記憶（寫下來才會記得）
- ✅ 快速回顧（忘記時可以看）
- ✅ 幫助他人（團隊成員可以學習）
- ✅ 追蹤成長（看到自己的進步）

**時間**：5-10 分鐘

---

## 知識管理架構

### 為什麼需要多層架構？

```
問題：知識放哪裡？
════════════════════════════════════

同一個知識可能適合放在很多地方：
- 專案規範？
- 開發 pattern？
- 功能規格？
- 學習筆記？

沒有清楚的邊界 → 重複記錄 → 維護困難
```

**解決方案**：四層知識架構，每層有明確的職責

---

### 四層架構

```
知識金字塔
════════════════════════════════════

        CLAUDE.md
       專案憲法層
      「永久不變」
           ↓
        MEMORY.md
       Pattern 層
     「可重用 patterns」
           ↓
         Skills
      自動化工作流
    「重複的操作」
           ↓
     OpenSpec Specs
        規格層
    「功能的定義」
```

---

### 第一層：CLAUDE.md（專案憲法）

**定位**：專案的「憲法」，制定永久不變的規則

**內容**：
- 技術棧選擇（Next.js、Prisma、TypeScript...）
- API 設計規範（錯誤格式、權限檢查）
- 檔案結構標準
- 命名慣例
- 資料庫規範
- 安全性要求
- 開發流程（OpenSpec workflow）
- 學習模式定義（Learn+ / Review）

**更新時機**：幾乎不更新（只有重大決策變更才更新）

**範例**：
```typescript
// ✅ 屬於 CLAUDE.md
所有 API Routes 必須使用統一錯誤格式：
return Response.json(
  { error: string, code?: string },
  { status: number }
)
```

**不屬於**：
- ❌ 具體的實作細節（放 MEMORY.md）
- ❌ 功能規格（放 OpenSpec Specs）
- ❌ 學習心得（放 learnings.md）

---

### 第二層：MEMORY.md（Patterns 層）

**定位**：開發過程中累積的「可重用 patterns」

**內容**：
- Prisma Transaction 標準寫法
- Next.js API Route 結構
- React Server/Client Component 模式
- Zod + react-hook-form 整合
- 常見問題解法（race condition、N+1 query）
- 安全性 patterns（token 生成）

**更新時機**：
- ✅ 同一個 pattern 出現 3 次
- ✅ 團隊成員問過 2 次同樣的問題
- ✅ 發現一個通用的解決方案

**範例**：
```typescript
// ✅ 屬於 MEMORY.md
### Prisma Transaction 標準模式

await prisma.$transaction(async (tx) => {
  // 1. 查詢
  const item = await tx.model.findUnique({ ... })

  // 2. 檢查條件
  if (!item) throw new Error('...')

  // 3. 執行操作
  return tx.model.create({ ... })
})
```

**不屬於**：
- ❌ 專案級規則（放 CLAUDE.md）
- ❌ 功能特定的邏輯（放 OpenSpec spec）
- ❌ 一次性的 workaround

---

### 第三層：Skills（自動化工作流）

**定位**：重複的操作流程，可以自動化

**內容**：（目前 MVP 階段不需要，未來考慮）
- 建立新 API Route 的腳本
- 產生 CRUD 操作的模板
- 測試檔案生成器

**更新時機**：當同一個操作重複 5 次以上

**範例**：
```bash
# 未來可能的 skill
claude-skill create-api-route <name>
→ 自動產生標準 API Route 檔案
```

---

### 第四層：OpenSpec Specs（功能規格）

**定位**：每個功能的詳細定義

**內容**：
- API endpoints 定義
- 資料結構（request/response）
- 業務邏輯規則
- 驗證規則
- 錯誤處理

**更新時機**：每個 change 都會有對應的 spec

**範例**：
```markdown
# 報名系統規格

## API Endpoint
POST /api/events/[id]/register

## Request Body
{ eventId: string }

## Business Rules
- 必須登入
- 不可重複報名
- 檢查活動容量
- 產生 QR token
```

**不屬於**：
- ❌ 通用的實作 pattern（放 MEMORY.md）
- ❌ 專案級規則（放 CLAUDE.md）

---

### 決策樹：知識應該放哪裡？

```
發現一個知識點
     │
     ▼
這是專案級的規則嗎？
（例：API 錯誤格式、檔案結構）
     │
     ├─ 是 → CLAUDE.md
     │
     └─ 否
         ▼
    這是可重用的 pattern 嗎？
    （例：Transaction 寫法、表單處理）
         │
         ├─ 是 → 出現 3 次了嗎？
         │        │
         │        ├─ 是 → MEMORY.md
         │        └─ 否 → 繼續觀察
         │
         └─ 否
             ▼
        這是重複的操作嗎？
        （例：建立 API、產生測試）
             │
             ├─ 是 → 重複 5 次了嗎？
             │        │
             │        ├─ 是 → Skill
             │        └─ 否 → 繼續觀察
             │
             └─ 否
                 ▼
            這是功能規格嗎？
            （例：報名 API 定義）
                 │
                 ├─ 是 → OpenSpec Spec
                 │
                 └─ 否 → learnings.md
                         （個人學習心得）
```

---

### 實際範例：同一個知識的不同層次

**情境**：Prisma Transaction

| 層次 | 內容 | 範例 |
|------|------|------|
| **CLAUDE.md** | 何時必須使用 | 「查詢 + 條件檢查 + 建立/更新 必須用 Transaction」 |
| **MEMORY.md** | 標準寫法 pattern | `await prisma.$transaction(async (tx) => { ... })` |
| **OpenSpec Spec** | 特定功能的應用 | 「報名 API 使用 Transaction 檢查容量」 |
| **learnings.md** | 個人學習心得 | 「我第一次用 Transaction，理解了原子性的重要」 |

---

### 維護原則

**CLAUDE.md**：
- 幾乎不更新（只有重大變更）
- 更新時必須團隊討論

**MEMORY.md**：
- 定期整理（每月檢視）
- 移除過時內容
- 加入新 patterns

**Skills**：
- MVP 階段不建立
- 未來再考慮

**OpenSpec Specs**：
- 每個 change 都要有
- 透過 `openspec validate` 確保格式

**learnings.md**：
- 每個 change 完成後寫
- 5-10 分鐘
- 個人學習記錄

---

## 學習記錄策略

### 為什麼需要記錄？

```
沒有記錄                      有記錄
════════                     ════════

3 個月後...                  3 個月後...
「我之前是怎麼做的？」        打開 learnings.md
忘記了，重新學一次            立刻回想起來

團隊新成員問你               團隊新成員
「這裡為什麼這樣設計？」      自己看 learnings.md
你：「呃...忘了」             理解設計決策

想重構程式碼                 想重構程式碼
不確定當初為什麼這樣寫        learnings.md 記錄了理由
不敢改，怕改壞                有信心地改進
```

### 輕量級筆記法（方案 A）

**原則**：
- 快速（5-10 分鐘）
- 重點式（不是流水帳）
- 行動導向（下次能用）

**三段式結構**：

```markdown
# Learnings: <功能名稱>

## 新學到的概念
[列出 2-3 個關鍵技術或模式]

## 遇到的問題與解法
[記錄 1-2 個有代表性的問題]

## 下次可以改進
[記錄 1-2 個改進點]
```

**AI 輔助流程**：

1. 完成 change 後
2. 問 AI：「總結我學到的關鍵概念」
3. AI 生成初稿（2 分鐘）
4. 你審核和補充（3 分鐘）
5. 複製到 learnings.md（1 分鐘）

**總時間：5-10 分鐘**

### 什麼要記？什麼不記？

```
✅ 要記錄                     ❌ 不用記錄
════════                     ════════

新技術的核心概念              細節的語法（查文件）
設計決策的理由                流水帳式的步驟
遇到的坑和解法                顯而易見的內容
下次可以改進的                臨時的 workaround
通用的 patterns               專案特定的細節
```

**範例**：

```
✅ 好的記錄：
「用 Prisma Transaction 避免報名競爭條件，
 查詢和建立要在同一個 transaction 內」

❌ 不好的記錄：
「先 import { prisma } from '@/lib/db'
 然後 await prisma.$transaction...」
（這是實作細節，看程式碼就知道）

✅ 好的記錄：
「QR Token 要夠隨機（64 字元），
 否則可以被猜測出來」

❌ 不好的記錄：
「今天花了 2 小時做報名功能」
（流水帳，沒有學習價值）
```

### 重要 Patterns 同步到 MEMORY.md

**MEMORY.md 是什麼？**
- 專案的核心知識庫
- 會被載入到 AI 的 system prompt
- 影響所有未來的對話

**什麼要放進 MEMORY.md？**
- ✅ 會重複使用的 patterns
- ✅ 專案的核心原則
- ✅ 常見問題的標準解法
- ❌ 特定功能的細節（放 learnings.md）

**範例**：

```markdown
# MEMORY.md

## 資料庫操作 Patterns

### Prisma Transaction 標準模式
當需要確保多個操作的原子性時：
```typescript
await prisma.$transaction(async (tx) => {
  // 所有操作用 tx 而不是 prisma
  const item = await tx.model.findUnique(...)
  return tx.model.create(...)
})
```

### API Error Handling 統一格式
所有 API Routes 的錯誤回傳：
```typescript
return Response.json(
  { error: '錯誤訊息', code: 'ERROR_CODE' },
  { status: 400 }
)
```

## 驗證 Patterns

### Zod + react-hook-form 標準組合
```typescript
const schema = z.object({ ... })
const form = useForm({
  resolver: zodResolver(schema)
})
```
```

**何時更新 MEMORY.md？**
- 當你在第 3 個 change 遇到相同的 pattern
- 當團隊成員重複問相同的問題
- 當你發現一個通用的解決方案

---

## 多人協作指南

### 為什麼考慮多人協作？

雖然這個專案是你的學習之旅，但：
- 之後可能有其他組織者加入開發
- 你可能想邀請朋友一起學習
- 這套方法可以推廣給其他人用

### 協作時的挑戰

```
單人開發                      多人協作
════════                     ════════

我知道我為什麼這樣寫          別人不知道
我記得坑在哪裡                別人會踩同樣的坑
我的學習節奏                  大家節奏不同
我的 AI 對話記錄              無法共享給別人
```

### learnings.md 的協作價值

**場景 1：新成員加入**

```
新成員 Bob 加入專案

沒有 learnings.md              有 learnings.md
─────────────────             ─────────────────
Bob：「這裡為什麼這樣寫？」    Bob 先看 learnings.md
你：「因為...」               理解了設計決策
Bob：「那個坑在哪？」         知道要注意什麼
你：「要注意...」             直接開始開發

結果：花很多時間講解          結果：快速上手
```

**場景 2：接手別人的功能**

```
Alice 做了報名系統，Bob 要加 Email 通知

沒有 learnings.md              有 learnings.md
─────────────────             ─────────────────
Bob 看程式碼                   Bob 看 learnings.md
不確定為什麼用 Transaction     理解了為什麼
不敢改，怕改壞                 知道可以改哪裡
問 Alice（她可能忘了）         有信心地擴展

結果：進度慢、風險高          結果：快速、安全
```

### 多人協作的最佳實踐

#### 1. 統一的學習模式

**問題**：每個人的學習方式不同，如何協調？

**解法**：

```markdown
# 團隊約定

## 學習模式使用原則
- 新技術：優先用 Learn+ 模式
- 熟悉技術：用 Review 模式
- 不確定時：選擇 Learn+（較慢但紮實）

## Code Review 檢查點
審核別人的 PR 時，確認：
- [ ] 有對應的 learnings.md
- [ ] 關鍵決策有記錄理由
- [ ] 遇到的坑有記錄解法
```

#### 2. learnings.md 格式統一

**問題**：每個人寫法不同，難以閱讀

**解法**：使用統一的範本

```markdown
# Learnings: <功能名稱>

**作者**：[你的名字]
**日期**：[完成日期]
**相關 tasks**：[tasks.md 中的 task 編號]

## 新學到的概念
- [概念 1]
- [概念 2]

## 遇到的問題與解法

### 問題 1：[簡短描述]
**情境**：[什麼情況下發生]
**解法**：[怎麼解決]
```code
[程式碼範例]
```

## 下次可以改進
- [改進點 1]
- [改進點 2]

## 相關資源
- [文件連結]
```

#### 3. 定期知識分享

**問題**：learnings.md 寫了，但沒人看

**解法**：

```
週會流程（30 分鐘）
════════════════════════════════

1. 每人分享（10 分鐘）
   - 這週完成的 change
   - 從 learnings.md 挑 1-2 個重點講
   - 遇到的有趣問題

2. 討論（15 分鐘）
   - 有沒有可以改進的地方
   - 有沒有通用的 pattern
   - 要不要更新 MEMORY.md

3. 下週規劃（5 分鐘）
   - 誰做什麼功能
   - 預期遇到的挑戰
```

#### 4. MEMORY.md 的維護

**問題**：誰來更新 MEMORY.md？

**解法**：

```
責任分配
════════════════════════════════

個人負責：
- 自己的 learnings.md

團隊共同負責：
- MEMORY.md
- 在週會討論後更新

原則：
- 如果一個 pattern 被 3 個人用過
  → 提議加入 MEMORY.md
- 如果一個解法被重複問過 3 次
  → 加入 MEMORY.md
```

---

## 實戰範例：報名系統開發

完整的 walkthrough，展示如何實際使用這套方法。

### 背景

你要開發「報名系統」功能，包括：
- 使用者可以報名活動
- 檢查容量限制
- 防止重複報名
- 生成 QR Code

你的技術背景：
- 會 TypeScript、React
- 不熟 Next.js API Routes
- 沒用過 Prisma
- 不知道怎麼做 QR Code

### Day 1 早上：探索（1 小時）

```
09:00 - 開始探索
──────────────────────────────────────

你：「/opsx:explore」

你：「我想做一個報名系統。使用者點按鈕就能報名活動。

     我目前的理解：
     - 需要一個 API endpoint 處理報名
     - 需要檢查使用者是否登入
     - 需要在資料庫建立記錄

     我的疑問：
     - Next.js 的 API Routes 怎麼寫？
     - 如何防止同一個人報名兩次？
     - 如何處理活動額滿？
     - QR Code 要怎麼產生？」

AI：「很好的起點！我們一步步來...」

[AI 開始解釋和畫圖]

你：[不斷提問]
    「什麼是 Transaction？」
    「unique constraint 怎麼設定？」
    「QR Code 裡面要放什麼？」

10:00 - 探索完成
──────────────────────────────────────

你已經理解：
✓ 報名流程的完整步驟
✓ 要用 Prisma Transaction
✓ 要在 schema 設定 unique constraint
✓ QR Code 用 crypto 產生隨機 token
```

### Day 1 下午：規格化與審核（1 小時）

```
14:00 - 生成規格
──────────────────────────────────────

你：「/opsx:ff registration-system」

AI：[生成 proposal, design, spec, tasks]

14:05 - 審核規格
──────────────────────────────────────

你：[閱讀生成的文件]

你：「spec.md 裡沒提到取消報名，
     但這應該是必要功能」

AI：[更新 spec.md]

你：「tasks.md 的 Task 2 太大，
     應該拆成：
     - Task 2a: 基本 API Route
     - Task 2b: Transaction 邏輯
     - Task 2c: QR Code 生成」

AI：[更新 tasks.md]

你：[繼續審核直到滿意]

14:45 - 驗證格式
──────────────────────────────────────

你：「openspec validate registration-system」

系統：「✓ All validations passed」

15:00 - 準備實作
```

### Day 2-3：實作（每天 3-4 小時）

#### Task 1: Prisma Schema（Learn+ 模式）

```
你：「Generate the Registration model,
     but leave the relations and constraints as TODO」

AI：[提供骨架]

你：[填入 TODO]

你：「Review my implementation」

AI：[給建議]

耗時：30 分鐘
學到：Prisma relation、unique constraint
```

#### Task 2a: 基本 API Route（Learn+ 模式）

```
你：「Explain Next.js API Routes.
     How do I create POST /api/events/[id]/register?」

AI：[詳細解釋]
    - 檔案位置
    - 函式簽名
    - 如何取得 body 和 params
    - 如何回傳 Response

你：「How do I check if user is logged in?」

AI：[解釋 NextAuth]

你：[自己實作基本版本]

你：「Review my code」

AI：[給 feedback]

耗時：2 小時
學到：Next.js API Routes、NextAuth
```

#### Task 2b: Transaction 邏輯（Learn+ 模式）

```
你：「Generate the capacity check and registration creation,
     but leave the transaction wrapper as TODO」

AI：[提供骨架]

你：[實作 transaction]

你：「Review my transaction logic」

AI：[確認正確性]

耗時：1 小時
學到：Prisma Transaction、race condition 處理
```

#### Task 2c: QR Code 生成（Review 模式）

```
你：「Implement QR Code token generation」

AI：[完整實作]

你：[逐行審核]
    「Why use crypto.randomBytes instead of Math.random()?」
    「Why 32 bytes?」
    「Should we hash the token before storing?」

AI：[解釋每個決策]

耗時：30 分鐘
學到：安全的隨機數生成
```

#### Task 3: 前端 Button（Review 模式）

```
AI：[實作 RegisterButton 元件]

你：[審核]
    「Why use optimistic updates here?」
    「What if the API call fails?」

AI：[解釋]

耗時：1 小時
學到：Optimistic UI、錯誤處理
```

### Day 4：驗證與記錄（1 小時）

```
09:00 - 測試功能
──────────────────────────────────────

你：[手動測試]
✓ 可以報名
✓ 額滿時顯示錯誤
✓ 不能重複報名
✓ QR Code 正常顯示

09:30 - 驗證
──────────────────────────────────────

你：「/opsx:verify」

AI：✓ 所有 tasks 完成
    ✓ 實作符合 spec

你：「/opsx:archive」

09:45 - 寫學習筆記
──────────────────────────────────────

你：「總結我在 registration-system 學到的」

AI：[生成總結]

你：[審核和補充]

你：[複製到 learnings.md]

10:00 - 完成！
```

### 成果檢視

**程式碼**：
- ✅ 功能完整運作
- ✅ 有測試
- ✅ 程式碼品質好

**學習**：
- ✅ 理解 Next.js API Routes
- ✅ 學會 Prisma Transaction
- ✅ 知道如何處理 race condition
- ✅ 了解 QR Code 安全性

**文件**：
- ✅ proposal、design、spec、tasks
- ✅ learnings.md 記錄關鍵學習
- ✅ 未來可以快速回顧

**時間**：
- 總計：約 12 小時
- 如果純讓 AI 寫：可能 2 小時
- 但你學到的：**無價**

---

## 檢查清單與最佳實踐

### 每個階段的檢查清單

#### 階段 1：探索 ✓

```
開始前：
□ 我清楚這個功能要解決什麼問題嗎？
□ 我有列出我的疑問嗎？

過程中：
□ 我持續提問了嗎？
□ 我理解 AI 的解釋了嗎？
□ 我要求 AI 畫圖了嗎？

結束時：
□ 我能向別人解釋這個設計嗎？
□ 我知道會用到哪些技術嗎？
□ 我理解為什麼這樣設計嗎？
```

#### 階段 2：規格化 ✓

```
□ proposal.md 目標明確
□ design.md 有說明設計理由
□ spec.md 詳細完整
□ tasks.md 拆分合理
```

#### 階段 3：規格審核 ✓

```
□ 我逐一閱讀了所有文件
□ 我檢查了是否有遺漏
□ 我提出了修改意見
□ AI 已經調整完成
□ 我滿意當前的規格
```

#### 階段 4：驗證 ✓

```
□ 執行了 openspec validate
□ 驗證通過
□ 如果失敗，已修正並重新驗證
```

#### 階段 5：實作 ✓

```
每個 task：
□ 我選擇了合適的學習模式
□ 我理解要做什麼
□ 我完成了實作
□ 我測試了功能
□ 我理解我寫的程式碼
□ 我標記 task 為完成
```

#### 階段 7：驗證與封存 ✓

```
□ 所有 tasks 都完成
□ 功能手動測試通過
□ 執行了 /opsx:verify
□ 執行了 /opsx:archive
□ 寫了 learnings.md
```

### 常見問題 FAQ

#### Q1: 我應該什麼時候用 Learn+ vs Review？

**A**: 問自己：「如果現在關掉 AI，我能獨立實作這個功能的 70% 以上嗎？」

- **不行** → **Learn+ 模式**
  - 從概念開始理解
  - 看標準版後再練習
  - 學習效果最好

- **可以** → **Review 模式**
  - AI 直接實作
  - 你審核並提問
  - 開發速度最快

**實際經驗法則**：
- 第一次遇到的技術 → Learn+
- 用過但不熟練（<70% 把握）→ Learn+
- 已經熟練（≥70% 把握）→ Review

**判斷門檻**：
```
能力                           模式      時間    學習效果
════════════════════════════════════════════════════════
完全不懂                      Learn+    1.5x      ★★★
看過但不會寫                  Learn+    1.5x      ★★★
能寫出 50-60%                 Learn+    1.5x      ★★☆
能寫出 70-80%                 Review    0.5x      ★★☆
很熟練，只是想加速            Review    0.3x      ★☆☆
```

#### Q2: 如果我高估了自己的能力，選錯模式怎麼辦？

**A**: 隨時可以切換！

```
你原本選 Review，但發現審核時完全看不懂
→ 暫停，改用 Learn+ 模式
→ 先理解概念，再回來實作

你原本選 Learn+，但發現你其實很熟
→ 跳過 Explain 階段
→ 直接看標準版就理解
→ 下次可以用 Review
```

#### Q3: 每個 task 都要寫 learnings.md 嗎？

**A**: 不用！只在完成整個 change 後寫一次。

```
❌ 錯誤：
Task 1 完成 → 寫 learnings
Task 2 完成 → 寫 learnings
Task 3 完成 → 寫 learnings

✅ 正確：
Task 1, 2, 3 都完成 → 寫一次 learnings
總結整個 change 的學習
```

#### Q4: learnings.md 要寫多詳細？

**A**: 3-5 個 bullet points 就夠了。

```
❌ 太詳細（流水帳）：
「今天早上 9 點開始，先看了 Next.js 文件，
 然後問 AI 怎麼寫 API Route，AI 解釋了...」

✅ 剛剛好（重點式）：
「學到 Next.js API Route 的標準模式：
 - 檔案位置：app/api/[...]/route.ts
 - 匯出 async function POST/GET
 - 用 auth() 檢查登入」

❌ 太簡略（沒價值）：
「完成了報名功能」
```

#### Q5: 如果團隊成員不想寫 learnings.md 怎麼辦？

**A**: 展示價值，而不是強迫。

```
策略：
1. 你先做示範
   → 寫 2-3 個高品質的 learnings.md

2. 等別人遇到問題
   → 「這個 learnings.md 有記錄喔」
   → 他們會發現有用

3. 形成習慣
   → Code Review 時提醒
   → 週會時分享
```

#### Q6: 我應該把多少時間花在學習 vs 開發？

**A**: 初期 70% 學習、30% 開發；後期反過來。

```
Week 1-2（新手期）
學習 70% | 開發 30%
────────────────────
花時間理解概念
慢慢實作
大量提問

Week 3-4（熟練期）
學習 30% | 開發 70%
────────────────────
基礎已打好
快速開發
偶爾深入研究

之後（專家期）
學習 10% | 開發 90%
────────────────────
遇到新東西才學
主要在產出
```

#### Q7: 如何避免過度依賴 AI？

**A**: 定期「不插電練習」。

```
每週練習（30 分鐘）
════════════════════════════════

挑一個你「以為」已經學會的概念
→ 關掉 AI
→ 白紙寫出來或實作
→ 寫不出來？還沒真的學會
→ 回去用 Learn+ 模式重新學
```

---

## 總結

### 核心要點

```
┌─────────────────────────────────────────┐
│     這套方法的核心精神                   │
└─────────────────────────────────────────┘

1. AI 是導師，不是代筆
   → 你要理解，不只是複製

2. 學習優先於速度
   → 慢就是快，扎實最重要

3. 記錄你的學習
   → 幫助未來的你和其他人

4. 選擇合適的模式
   → 不熟用 Learn+，熟練用 Review

5. 持續驗證理解
   → 能教別人才是真的懂
```

### 預期成果

**4 週後，你會有**：
- ✅ 一個可運作的專案
- ✅ 深入理解 React、Next.js、Prisma
- ✅ 完整的規格文件
- ✅ 詳細的學習記錄
- ✅ 獨立開發的能力

**更重要的是**：
- 🧠 你建立了一套可重複的學習方法
- 🚀 你知道如何快速掌握新技術
- 👥 你的學習成果可以幫助其他人

### 下一步

現在你已經理解了整套方法，準備好開始了嗎？

1. 閱讀 [產品規格（PRODUCT_SPEC.md）](./PRODUCT_SPEC.md)
2. 決定第一個要開發的功能
3. 執行 `/opsx:explore` 開始你的第一個 change
4. 遵循這份指南的流程
5. 記錄你的學習

**祝你學習愉快！** 🎉

---

## 附錄

### 有用的 Prompt 範例

收集一些實用的 prompt，幫助你更有效地跟 AI 協作。

#### 探索階段

```
「Explain [concept] like I'm familiar with [related concept]
 but new to this specific implementation.」

「What are the tradeoffs between [option A] and [option B]
 for [specific use case]?」

「Walk me through the flow of [process] step by step,
 and explain why each step is necessary.」
```

#### 學習階段

```
「Explain [concept] with a simple analogy first,
 then show me a real-world code example.」

「What are the common mistakes people make when using [technology]?」

「Compare [approach A] vs [approach B]. When should I use each?」
```

#### 實作階段

```
「Review my implementation and check:
 1. Is the logic correct?
 2. Are there any edge cases I missed?
 3. Any performance concerns?
 4. Any security issues?」

「This code works, but is there a more idiomatic way
 to write it in [framework/library]?」
```

### 推薦學習資源

- Next.js 官方文件：https://nextjs.org/docs
- Prisma 官方文件：https://www.prisma.io/docs
- TypeScript 手冊：https://www.typescriptlang.org/docs
- shadcn/ui 文件：https://ui.shadcn.com/

### OpenSpec 相關指令速查

```bash
# 探索
/opsx:explore

# 建立 change（快速模式）
/opsx:ff <feature-name>

# 實作
/opsx:apply

# 驗證
/opsx:verify

# 封存
/opsx:archive

# 驗證格式
openspec validate <change-name>

# 列出所有 changes
openspec list
```
