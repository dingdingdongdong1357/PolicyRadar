社會科學程式設計｜期末專案 Proposal  
作者：社會五丁祈  

# PolicyRadar：租屋補貼政策網路支持度分析工具（YouTube + Dcard + AI）

### 專案摘要
PolicyRadar 是一個結合 YouTube 與 Dcard 輿論資料，並運用 AI 進行分析的 **政策輿情分析 CLI 工具 (Command Line Interface)**，用於追蹤「租屋補貼」政策的**支持度、反對理由、輿情趨勢與爭議點**。使用者可透過指令互動進行查詢，並可手動更新資料 (`--update`) 以獲得最新結果。

## 一、專案描述

台灣長期面臨租屋市場價格失衡與青年租屋負擔問題，政府推出的「租金補貼方案」雖有意幫助青年，但在網路上引發高度爭議與不同意見，例如「圖利房東」、「無法改善結構問題」、「行政效率低」等說法。

然而，**目目前缺乏一個能以資料佐證的工具，整理並量化這些政策輿情現象**，不論學術研究、媒體分析或一般公民理解，都難以掌握網路討論的真實比例與焦點。
 **本專案的價值**  
- 用**數據了解輿論**
- 分析政策支持/反對的**核心論點**
- 建立一個**可重複使用、可更新**的政策輿情分析工具

## 二、使用者故事（User Stories）

- As a **公共政策研究者**，I want to **定量分析租屋補貼的支持與反對比例**，so that **我可以在研究中引用具體的輿情證據**。
- As a **記者或內容製作者**，I want to **快速查看網路上主要的爭議點與情緒走向**，so that **能在報導中補上背景分析**。
- As a **一般公民**，I want to **用簡單方式理解大家怎麼看這個政策**，so that **我能形成自己的觀點，而不是被帶風向**。

## 三、主要功能（MVP）

| 功能 | 說明 | CLI 指令範例 |
|------|------|---------------|
| 支持度分析 | 顯示支持/反對比例 | `python policyradar.py --metric support` |
| 輿論趨勢 | 顯示熱度變化與討論量 | `python policyradar.py --trend 30` |
| 主要爭議點摘要 | AI 自動彙整支持與反對理由 | `python policyradar.py --view reasons` |
| 資料更新 | 抓最新 YouTube + Dcard 輿情資料 | `python policyradar.py --update` |
| 平台比較 | 比較 YouTube 與 Dcard立場差異 | `python policyradar.py --compare platform` |

## 四、Tech Stack（使用技術）

| 領域 | 技術 |
|------|------|
| 語言與執行 | Python 3.x |
| 網路資料擷取 | YouTube Data API v3、Dcard HTTP Requests |
| 資料處理 | pandas、json |
| AI 處理 | GPT（自動立場分類與爭議摘要） |
| 文字處理 | jieba（中文斷詞） |
| CLI 互動 | typer |
| 資料儲存 | CSV/JSON |
| 設定管理 | `.env`（儲存 API 金鑰） |


## 五、檔案結構（目錄架構）

```

policyradar/
│
├── src/
│   ├── cli.py              # 主 CLI 介面
│   ├── fetch_youtube.py    # 取得 YouTube 資料
│   ├── fetch_dcard.py      # 取得 Dcard 資料
│   ├── preprocess.py       # 清理 & 合併資料
│   ├── analyze.py          # 計算支持度/趨勢
│   ├── ai_label.py         # GPT 自動分類
│   └── utils.py            # 共用工具函式
│
├── data/
│   ├── raw/                # 原始資料
│   └── processed/          # 處理後資料
│
├── .env                    # API 設定（不公開）
├── requirements.txt
├── README.md
└── instructions.md

```

```
flowchart LR
    %% PolicyRadar 系統架構圖（雙流程）

    U([使用者 User])
    CLI[[CLI 指令介面 (policyradar.py)]]

    subgraph UpdateFlow[資料更新管線 (--update)]
        F[資料擷取：YouTube API + Dcard]
        RAW[(data/raw)]
        P[資料前處理：清洗 / 去重 / 正規化]
        AI[[AI 分析 (GPT)：立場分類 + 爭議摘要]]
        PROC[(data/processed)]
    end

    subgraph QueryFlow[查詢/輸出管線 (--support / --trend / --reasons)]
        M[統計分析：支持度 / 情緒 / 趨勢]
        OUT[[輸出結果：CLI 顯示 / 圖表 / 報告]]
    end

    U --> CLI
    CLI -- --update --> F --> RAW --> P --> AI --> PROC
    CLI -- 查詢指令 --> M
    PROC --> M --> OUT


---

## 六、範例（Example Input / Output）

### 查詢支持度比例
**指令：**
```

python policyradar.py --metric support

```
**輸出：**
```

支持度：41.7%
反對度：58.3%
樣本數：8432 則留言（來源：YouTube + Dcard）

```

---

### 更新資料
**指令：**
```

python policyradar.py --update

```
**輸出：**
```

已更新資料（近30天）
YouTube留言：+5234
Dcard文章：+198
AI 分析已完成

```

## 七、AI 協作說明（符合作業要求 ）

本專案規劃過程使用 AI 進行協作，包含：

* **設計功能架構**
* **拆解需求與整理 CLI 規格**
* **提出檔案架構與資料流程圖**
* **協助產生技術規劃與 Proposal 文件**




