# Real Estate Decision AI Agent — 5 Cores, 3 Trade-offs, 10 Details

**研究題目：結合實價登錄、空間特徵與可解釋人工智慧之房地產決策代理人**  
**Document status:** Proposal draft / planned methodology; not validated experimental findings.  
**Scope:** Research architecture and evaluation planning only. OpenSpec is explicitly out of scope for this submission.

## 1. Research Positioning

本研究旨在將臺灣不動產實價登錄資料、建物屬性、地理空間資訊與可解釋機器學習整合為房地產決策支援 AI Agent。系統以資料來源與模型推論為依據，協助使用者比較住宅、理解價格影響因素及辨識估價不確定性，而非宣稱能提供法定估價或保證成交價格。

核心研究問題（RQ）：
- **RQ1:** 加入可取得的空間特徵後，是否能提升房價預測在不同時間與區域的泛化能力？
- **RQ2:** XAI 是否能使模型預測的主要依據更容易理解，同時保留足夠的預測品質？
- **RQ3:** 工具導向的 AI Agent 是否能在不虛構房價或資料來源的前提下，改善使用者取得比較資訊的效率？

## 2. Five Cores — 五大核心思維

### C1. Data Reliability and Governance — 實價登錄資料品質與治理
- **問題：** 成交資料可能存在缺值、重複、特殊交易、欄位異質性與揭露時間落差。
- **方法：** 建立來源版本紀錄、欄位字典、交易類型篩選、去重、異常值標記與可重現清理流程；區分成交總價與每單位面積價格。
- **可驗證成果：** 有效樣本比例、缺值率、排除規則、資料時間涵蓋率與資料處理紀錄。

### C2. Spatial Context and Feature Engineering — 空間特徵與區域情境
- **問題：** 相同建物條件在不同地段可能有顯著不同的成交價格。
- **方法：** 在資料授權與可得性允許下，整合行政區、座標或適當精度的位置、交通設施、生活機能、土地使用與鄰近交易等特徵；記錄各項空間資料的日期與測量方法。
- **可驗證成果：** 空間特徵清單、涵蓋率、空間特徵消融實驗，以及跨區域預測誤差比較。

### C3. Predictive Modeling and Generalization — 房價預測與泛化能力
- **問題：** 複雜模型可能擬合歷史交易，卻無法準確預測未來或未見區域。
- **方法：** 以線性迴歸等可解釋基準模型，對照 Random Forest 與 XGBoost；依實際應用情境設計時間切分、區域分組或空間封鎖驗證。
- **可驗證成果：** MAE、RMSE、MAPE（對零值或異常分母加以處理）、不同價格區間與區域的誤差分布，以及模型訓練成本。

### C4. Explainable AI and Uncertainty — 可解釋性與不確定性
- **問題：** 單一估價數字不足以說明推論依據，也可能使使用者過度信任模型。
- **方法：** 使用 SHAP 檢視全域與個別預測的特徵貢獻，搭配相似交易案例與資料來源；評估適當的預測區間或誤差範圍。
- **可驗證成果：** 特徵重要性穩定度、個案解釋一致性、預測區間覆蓋率與使用者理解度。SHAP 表示模型貢獻，不應直接解讀為因果效果。

### C5. Tool-Grounded Decision AI Agent — 資料錨定的決策代理人
- **問題：** 通用 LLM 可能產生不存在的成交案例、錯誤數值或未經證實的購屋建議。
- **方法：** 讓 LLM 處理自然語言需求、任務規劃與報告表達；由受控工具執行資料檢索、數值計算、模型預測與解釋。資訊不足時要求澄清，並顯示資料日期、來源與限制。
- **可驗證成果：** 工具呼叫正確率、來源引用完整率、無根據陳述比例、任務完成率與使用者操作時間。

## 3. Three Trade-offs — 三項關鍵技術取捨

### T1. Predictive Accuracy vs. Interpretability — 預測精度與可解釋性
- **衝突：** 複雜集成模型可能取得較低誤差，但不如簡單模型容易解釋。
- **設計決策：** 同時保留透明基準模型及非線性候選模型；以預先定義的誤差、穩定性、解釋成本評估，而非只選最低 MAE。
- **驗證方式：** 比較模型誤差、SHAP 穩定度、推論時間與使用者對估價原因的理解。

### T2. Spatial Detail vs. Data Availability and Privacy — 空間精度與資料可得性／隱私
- **衝突：** 精細位置可能提高估價能力，但資料取得、地理編碼品質與個別交易位置揭露可能受限。
- **設計決策：** 採分層位置特徵（行政區、網格、距離等），只使用合法取得的資料，避免公開不必要的精確住址。
- **驗證方式：** 比較不同空間解析度的涵蓋率、誤差與資料保護風險。

### T3. Agent Autonomy vs. Verifiability and Safety — 代理自主性與可驗證／安全性
- **衝突：** 高自主性可減少使用者操作，但可能增加不受控檢索、數值幻覺與錯誤建議。
- **設計決策：** 採工具白名單、輸入驗證、來源追蹤及低信心澄清；不得將 LLM 自行生成的價格當成模型估價。
- **驗證方式：** 以缺漏欄位、矛盾需求、無資料區域及惡意提示等案例測試錯誤處理與安全降級。

## 4. Ten Implementation Details — 十項具體研究與實作細節

| ID | 工作項目 | 預計方法／交付成果 | 驗證重點 |
|---|---|---|---|
| D01 | 定義研究範圍與目標變數 | 明確指定住宅類型、地區、研究期間與總價或單價預測目標 | 範圍可重現，避免混淆價格單位 |
| D02 | 取得與版本化實價登錄資料 | 記錄官方來源、取得日期、資料期間與欄位字典 | 可追溯性、合法使用、版本一致 |
| D03 | 資料清理與特殊交易處理 | 缺值、重複、極端值、非典型交易標記與處理規則 | 清理前後樣本數及排除原因 |
| D04 | 建立建物特徵 | 面積、屋齡、樓層、建物型態等，避免使用預測時不可得欄位 | 特徵缺漏率與時間可得性 |
| D05 | 建立 GIS 空間特徵 | 行政區、交通與生活機能距離等；核對座標系統及資料日期 | 地理編碼成功率、空間涵蓋率 |
| D06 | 設計資料切分與防洩漏流程 | 時間切分及空間／區域分組驗證；前處理僅在訓練折擬合 | 未來資訊與近鄰洩漏控制 |
| D07 | 建立與調校預測模型 | 線性基準、Random Forest、XGBoost；以驗證資料調參 | MAE、RMSE、MAPE、分群誤差 |
| D08 | 可解釋性與不確定性分析 | SHAP 全域／局部解釋、相似案例、適用的預測區間 | 解釋穩定性、區間覆蓋率 |
| D09 | 設計 AI Agent 互動與工具介面 | 使用者輸入→意圖解析→資料查詢→模型預測→解釋→報告；不足時澄清 | 工具呼叫、來源完整率、錯誤降級 |
| D10 | 對照與消融實驗 | 比較有／無空間特徵、有／無解釋及不同互動方式 | 效能差異、操作時間、可理解度 |

## 5. Proposed Agent Workflow — 概念性流程（非 OpenSpec）

1. **User Query:** 使用者輸入物件條件、地區、預算或比較需求。
2. **Intent and Validation:** 解析意圖，確認交易類型、面積單位、位置與必要條件。
3. **Data Retrieval:** 從已授權的交易資料與空間特徵資料集中查詢，附來源及日期。
4. **Feature Pipeline:** 執行與訓練階段一致的特徵轉換，檢查缺漏與資料分布。
5. **Model Inference:** 由已驗證的模型產生估價及適當的不確定性資訊。
6. **Explanation:** 產生 SHAP 特徵貢獻、可比交易與限制說明。
7. **Decision Support:** 整理比較報告、可能的價格影響因素及需要進一步查證的事項。
8. **Safety and Traceability:** 若資料不足、超出模型適用範圍或來源不可驗證，回覆限制而非虛構答案。

## 6. Experimental Design and Evidence Requirements

- **Baseline:** 無空間特徵的簡單模型；必要時增加區域固定效果或其他合理基準。
- **Ablation A:** 移除空間特徵，觀察誤差與跨區域泛化變化。
- **Ablation B:** 比較有無 SHAP 解釋的理解度與任務完成品質；不得將解釋性當作預測精度的替代指標。
- **Ablation C:** 比較直接 LLM 回答與工具錨定 Agent 的來源正確率及數值一致性。
- **Reporting:** 記錄資料期間、切分方式、樣本量、模型版本、超參數、隨機種子與評估指標；未完成實驗前不填寫虛構數據。
- **Validity threats:** 時空分布偏移、選擇偏誤、特殊交易、地址精度不足、相似案例洩漏、SHAP 解釋的相關特徵歧義。

## 7. Academic Value and Expected Contributions

1. **資料層貢獻：** 提供具資料治理與可重現性的臺灣住宅交易資料處理流程。
2. **方法層貢獻：** 量化空間特徵在不同驗證設定下對房價預測的增益與限制。
3. **解釋層貢獻：** 探討 SHAP 與相似交易案例能否改善估價透明度及使用者理解。
4. **系統層貢獻：** 建立工具錨定、可追溯且具錯誤降級能力的房地產決策 Agent 原型。
5. **評估層貢獻：** 同時檢驗預測、解釋、可靠性與互動效能，避免僅以單一誤差指標判斷系統價值。

**注意：** 上述為預期研究價值與可驗證假設，不代表已獲得優於既有方法的結果。

## 8. Reference Methodology (supporting design choices)

- scikit-learn, *Cross-validation: evaluating estimator performance*. https://scikit-learn.org/stable/modules/cross_validation.html
- Esri, *Evaluate Predictions with Cross-validation*（Random K-fold 與 Spatial K-fold 的用途差異）. https://doc.esri.com/en/arcgis-pro/latest/tool-reference/spatial-statistics/evaluate-predictions-with-cross-validation.html
- Lundberg & Lee (2017), *A Unified Approach to Interpreting Model Predictions*（SHAP 方法）. https://arxiv.org/abs/1705.07874

## 9. Submission Notes

- This file addresses **5 Cores, 3 Trade-offs, 10 Details** for the DIC3 proposal-refactor assignment.
- It is aligned with the A–F research outline in [abstract.md](./abstract.md).
- OpenSpec installation and workflow conversion are deferred and are **not part of this submission stage**.
- Before final submission, reconcile the proposed methods, geographic scope and claims with the original course proposal and rubric.
