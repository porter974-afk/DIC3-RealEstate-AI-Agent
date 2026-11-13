# A2Z Writing SOP and Course Rubric Alignment — Real Estate Decision AI Agent

**研究題目：** 結合實價登錄、空間特徵與可解釋人工智慧之房地產決策代理人  
**對照文件（依使用者提供的解析文字）：** 《A2Z outline example.pdf》、《期末專題提案作業規範.pdf》、《房價期末報告.pdf》  
**文件性質：** A2Z 架構對照、作業規範檢核與後續重構依據（2026-11-13）。

> **證據界線：** 本文件整理使用者提供的 A2Z 深度解析，並非直接逐頁查核上述三份 PDF。文中的 10,638 筆、115 年第 3 季、23 篇文獻、34 頁 PDF、GitHub／DESIGN.md 連結及「符合」判斷均為**原解析所述，尚待原始文件確認**。原解析所附 1–20 等數字為來源標記而非可用的完整引文，故不沿用為學術參考文獻編號。此專案目前仍是 Proposal，不能將規劃中的實驗視為已完成結果。

## 1. A2Z SOP 對照

### 1.1 Abstract — A–F

| 代碼 | A2Z 要求 | 原解析指出的房價提案內容 | 編修／驗收重點 |
|---|---|---|---|
| A | Attention Getter / Motivation | 臺灣住宅決策需綜合價格、面積、位置與法規；實價登錄不等同完整市場行情 | 說明資訊落差及決策價值 |
| B | But / Challenge | 單一估價數字無法呈現資料不確定性與適用範圍 | 明確區分觀測成交價、統計摘要與模型估計 |
| C | Cure / Solution | 以受控 ReAct + RAG 為核心的房地產 AI Agent | 突出工具協調、知識檢索與可驗證性 |
| D | Development / Method | 結構化統計工具、RAG、Evidence Gate | 定義工具白名單、證據檢查與 Safe Stop |
| E | Experiments / Evaluation | 原解析稱使用臺中市政府地政局 115 年第 3 季買賣案件 **10,638 筆**官方單一快照資料作可行性盤點 | 須核對資料下載來源、日期、篩選口徑、樣本數及盤點結果；不可當成模型實驗成績 |
| F | Findings / Expected Outcomes | Proposal 階段的資料可行性結論為 **Conditional Go**；效能待驗證 | 清楚區分「資料盤點可行性」與「系統實證有效性」 |

**建議精煉後的研究主張：** 本研究探索受控 ReAct + RAG 房地產代理人，透過結構化交易工具與 Evidence Gate 產生「數值＋樣本範圍＋日期＋來源＋限制」的可追溯回答；資料不足時 Safe Stop。Conditional Go 只代表原解析所述的階段性可行性判斷。

### 1.2 Introduction — G–L

| 代碼 | 功能 | 原解析指出的落實方式 |
|---|---|---|
| G — General | 研究背景 | 實價登錄交易價格的資訊價值與限制 |
| H — However | 現況問題 | 車位、樓層、揭露日期及單價口徑可能造成誤讀 |
| I — In Literature | 既有研究 | 房價預測、空間解釋、RAG 與 Agent 技術 |
| J — Judgement | 研究定位 | 限定臺中住宅成交樣本；不保證成交價，不提供個人財務建議 |
| K — Keypoint | 核心論點 | 「數值＋樣本範圍＋日期＋來源＋限制」 |
| L — List Organization | 章節安排 | 引導讀者閱讀 Related Work、Deep Research、架構、評估與結論 |

### 1.3 Related Work — M–O

| 代碼 | 功能 | 原解析指出的內容 |
|---|---|---|
| M — Methods | 研究方法分類 | 2.1 房價分析／空間解釋（Wang et al.、Chou et al.、SHAP）；2.2 Agent／RAG（Lewis et al.、Yao et al.、Toolformer） |
| N — New Proposed / Research Gap | 研究缺口 | 在**相同住宅任務及評估規則**下，比較受控 ReAct + RAG 與固定 Workflow 的多步工具使用及適當拒答能力 |
| O — Organize | 文獻組織 | 原解析稱涵蓋 23 篇國內外文獻；正式稿需建立 Article Summary Table 並逐篇核對書目 |

**關鍵區別：** 研究缺口不是泛稱「現有房價模型不夠準」，而是**受控代理流程是否能在相同證據標準下，比固定流程更有效處理多步查詢與安全停止**。SHAP 與 GIS 是重要領域／方法背景，但不應掩蓋此主要比較問題。

### 1.4 Proposed Scheme — P–PM

原解析以 PA–PF 表示研究目標、背景、解法及設計，並以 PG–PI 表示架構圖、流程圖及 Pseudocode。對應模組如下：

| 模組 | 主要責任 | 驗收證據 |
|---|---|---|
| Query Validation | 驗證地區、期間、交易類型、價格口徑及查詢完整性 | 不合法或不完整輸入的處理紀錄 |
| ReAct Orchestrator | 規劃受控多步工具呼叫 | 工具呼叫序列、合法性及停止條件 |
| Structured Transaction Tools | 針對官方交易快照進行可重現查詢及統計 | 樣本數、篩選條件、數值與資料版本 |
| RAG Domain Knowledge | 檢索法規、資料欄位、統計口徑及領域說明 | 片段來源、日期、引用可追溯性 |
| Evidence Gate | 在回答前檢查主張是否具足夠證據 | 數值、範圍、日期、來源、限制的完整性 |
| Safe Stop | 證據不足、查詢越界或資料不可靠時停止價格主張 | 正確拒答率、錯誤拒答率及降級說明 |

**概念流程：**

`User Query → Query Validation → ReAct/Workflow Routing → Transaction Tools and/or RAG → Evidence Gate → Evidence-backed Answer or Safe Stop`

**摘要 Pseudocode（供重構設計使用，非聲稱已實作）：**

```text
01  request ← validate(user_query)
02  if request.invalid: return clarification_or_safe_stop
03  plan ← orchestrate_allowed_tools(request)
04  transaction_evidence ← query_structured_data(plan)
05  domain_evidence ← retrieve_rag_sources(plan)
06  evidence_check ← verify_scope_date_source_limits(transaction_evidence, domain_evidence)
07  if not evidence_check.sufficient: return safe_stop_with_reason
08  return grounded_answer_with_numeric_scope_date_source_and_limits
```

正式版本應再補上 Flowchart 圖形、例外路徑、工具介面與可重現測試案例。原解析稱原 PDF 另有帶行號 Pseudocode 與 GitHub 程式碼連結；此處未獨立核實。

### 1.5 Simulation and Evaluation — Q–V

原解析指出第 5 章分成兩組評估：

| 面向 | 指標 | 檢驗問題 |
|---|---|---|
| 資料／價格分析 | MAE、RMSE、R² | 若有價格預測子任務，估價表現與基準相比如何？ |
| Agent 回答可靠性 | 工具合法性、引用支持率、安全停止率 | 受控 Agent 是否正確呼叫工具、以證據支持主張並在不足時拒答？ |
| 比較實驗（建議明確化） | 任務完成率、多步任務成功率、錯誤拒答率、延遲與成本 | ReAct + RAG 相對固定 Workflow 的實際利弊為何？ |

**實驗公平性：** 比較組應使用同一份交易資料快照、相同任務集、相同允許工具、相同 Evidence Gate 標準與相同評分規則。若資料盤點尚未完成，不得將 10,638 筆等同可用於訓練的住宅樣本數。

### 1.6 Conclusion — W–Z

| 代碼 | 功能 | 預期內容 |
|---|---|---|
| W — What Proposed | 提出什麼 | 受控 ReAct + RAG 房地產決策代理人 |
| X — eXcel | 特色 | 工具白名單、Evidence Gate、Safe Stop |
| Y — Yields | 預期產出 | 可追溯回答機制、可行性盤點與公平比較評估方案 |
| Z — Zen / Closing | 結語與限制 | Proposal 階段、Conditional Go、資料限制與後續實驗 |

## 2. Course Rubric Alignment / 課程評分規範對照

**原解析指出：** Proposal 占課程總成績 10%；以下六項配比合計 100%。此配比與達成度均待《期末專題提案作業規範.pdf》核對。

| 評分項目 | 配比（原解析） | 原解析指出的證據 | 本次檢核判斷 |
|---|---:|---|---|
| Introduction | 20% | 研究動機、資料限制、研究問題與邊界 | 有相應內容；須核對 A–F／G–L 的章節要求 |
| Related Work | 20% | 房價／空間與 RAG／Agent 兩類研究；原解析稱 23 篇文獻 | **待補獨立 Article Summary Table**，並核對全部文獻 |
| Deep Research | 20% | 5 個核心原則、3 項方法爭論、10 個驗收問題 | 須以原 PDF 第 3 章核對，不能用舊版 10 個實作細節直接替代 10 個驗收問題 |
| Proposed AI Agent + System Architecture | 20% | Query Validation、ReAct、結構化工具、RAG、Evidence Gate | 模組架構明確；需核對圖表、介面及安全機制 |
| Flowchart + Pseudocode + Technical Feasibility | 15% | 流程、01–08 行 Pseudocode、官方快照資料盤點 | 須確認是否具正式圖形及完整資料盤點證據 |
| GitHub + DESIGN.md + 完整度 | 5% | 原解析稱原 PDF 含私有庫／DESIGN.md URL 及 34 頁備份 | **本 Repository 尚未確認有 DESIGN.md**；不能認定已達標 |

**重要：** 「完全符合／高度符合」是使用者提供的原解析評語，**不是本次獨立評分結論**。是否符合須依實際 PDF、老師規範及 GitHub 交付物逐項驗收。

## 3. Three Key Strengths / 三大研究亮點

1. **Academic Honesty — 學術誠實：** 清楚區分資料盤點的 Conditional Go 與尚未驗證的模型／Agent 效能，不虛構實驗結果。
2. **Evidence Gate & Safe Stop — 領域安全：** 證據不足時停止產出價格主張；任何回答均應附上數值、樣本範圍、日期、來源與限制。
3. **Rubric-oriented Research Design — 規範導向：** 以 A–Z SOP 串接研究問題、文獻、5／3／10 深度研究、系統架構及評估方案。

## 4. Discrepancies with Current GitHub Drafts / 與既有草稿的差異

| 現有檔案 | 目前重點 | 依新提供 A2Z 分析應調整之處 |
|---|---|---|
| [abstract.md](./abstract.md) | GIS／模型比較／SHAP | 將 ReAct + RAG、Evidence Gate、Safe Stop、臺中資料快照與 Conditional Go 提升為主要敘事 |
| [research-analysis.md](./research-analysis.md) | 5 個技術核心、3 個技術取捨、10 個實作工作 | 對照原報告 5 個研究設計原則、3 個專業爭論、10 個**深度理解與驗收問題** |
| [introduction.md](./introduction.md) | 臺灣住宅、空間泛化與模型解釋 | 聚焦臺中住宅樣本、價格資訊邊界、五元素證據原則及 Agent 安全停止 |
| [related-work.md](./related-work.md) | 7 篇代表性文獻，偏 GIS／SHAP | 增加 RAG、ReAct、Toolformer 及原報告領域研究；建立 Article Summary Table；核對所稱 23 篇 |
| [ieee-abstract.md](./ieee-abstract.md) | GIS／SHAP／價格模型為主 | 後續重構時應以受控 ReAct + RAG 與公平 Workflow 對照為主軸 |

**版本策略：** 本次先將 A2Z 對照分析新增為獨立文件，**不直接覆寫**既有研究章節，避免未取得原始 PDF 前將尚待確認的細節寫入正式稿。後續取得三份 PDF 並核對後，再按本表逐檔重構。

## 5. Verification Checklist / 待核對事項

- [ ] 取得《A2Z outline example.pdf》核對 A–Z 各段落字母定義與章節映射。
- [ ] 取得《期末專題提案作業規範.pdf》核對 Proposal 10% 與六項評分配比。
- [ ] 取得《房價期末報告.pdf》核對臺中市 115 年第 3 季 10,638 筆資料的來源、篩選與 Conditional Go 判定。
- [ ] 核對原報告所稱 23 篇文獻，建立獨立 Article Summary Table。
- [ ] 核對 5 核心、3 爭論、10 驗收問題的原文與位置。
- [ ] 核對 Flowchart、Pseudocode、程式碼 URL、34 頁 PDF 與 DESIGN.md。
- [ ] 統一 Introduction 與 Related Work 引文編號。
- [ ] 在確認原始資料後同步更新 Abstract、Introduction、Related Work 與 IEEE Abstract。

---

**Scope note:** OpenSpec 暫不處理。此文件僅用於 A2Z／課程規範對照與研究重構規劃，不代表已完成 OpenSpec workflow 或系統實作。
