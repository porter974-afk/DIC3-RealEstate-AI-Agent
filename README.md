# DIC3 — Real Estate Decision AI Agent

**中文題目：結合實價登錄、空間特徵與可解釋人工智慧之房地產決策代理人**

**English title:** A Real Estate Decision AI Agent Integrating Transaction Records, Spatial Features, and Explainable Artificial Intelligence

> **Project status:** Academic research proposal and literature-based refactor. Proposed models, experiments and contributions have not yet been empirically validated. **OpenSpec is deferred** and is not included in the current submission stage.

## Project Overview / 專案簡介

本研究規劃以臺灣不動產實價登錄公開資料為基礎，結合建物屬性、GIS 空間特徵、房價預測模型、SHAP 可解釋人工智慧及工具導向 AI Agent，提供具資料來源、估價解釋與限制揭露的房地產決策支援。系統定位為研究性輔助工具，不取代專業不動產估價，也不保證未來成交價格。

The proposed research integrates Taiwanese real estate transaction records, spatial features, predictive modeling, explainable AI, and a tool-grounded language-model agent to investigate transparent and traceable housing decision support.

## Documents / 研究成果

| File | Description | Status |
|---|---|---|
| [abstract.md](./abstract.md) | 中文 A–F 研究摘要：動機、挑戰、解方、方法、實驗與預期貢獻 | Proposal draft |
| [research-analysis.md](./research-analysis.md) | 5 Cores、3 Trade-offs、10 Details，研究問題與概念流程 | Proposal draft |
| [introduction.md](./introduction.md) | Introduction Refactor：背景、研究缺口、研究目標、方法與貢獻 | Proposal draft |
| [related-work.md](./related-work.md) | Related Work Refactor：代表性學術文獻、比較矩陣、研究定位及引用 | Literature draft |
| [ieee-abstract.md](./ieee-abstract.md) | 單段式 IEEE 風格英文 Abstract 與 Index Terms | Proposal draft |

## Research Questions / 研究問題

- **RQ1 — Spatial Generalization:** 加入可取得的空間特徵後，能否改善房價預測的時間及跨區域泛化能力？
- **RQ2 — Explainability:** SHAP 與相似交易說明能否改善預測依據的可理解性，並正確傳達不確定性？
- **RQ3 — Agent Reliability:** 工具錨定的 AI Agent 能否提升資料來源正確性、數值一致性與資訊取得效率？

## Five Research Cores / 五大核心

1. Data reliability and governance — 實價登錄資料品質與治理。
2. Spatial context and feature engineering — GIS 空間特徵。
3. Predictive modeling and generalization — 房價預測與泛化。
4. Explainable AI and uncertainty — SHAP 與不確定性。
5. Tool-grounded decision AI agent — 可追溯的自然語言決策支援。

**Three trade-offs:** 預測精度 vs. 可解釋性；空間精度 vs. 資料可得性／隱私；Agent 自主性 vs. 可驗證性／安全性。詳見 [research-analysis.md](./research-analysis.md)。

## Proposed Methodology / 預計研究方法

- **Data:** 官方實價登錄資料；交易型態、價格口徑、缺漏與特殊交易處理。
- **Features:** 面積、屋齡、樓層、建物類型及合法可取得的區位／生活機能特徵。
- **Models:** 線性迴歸基準、Random Forest 與 XGBoost 候選模型。
- **Explanation:** SHAP 特徵貢獻、可比交易及適當的不確定性呈現。
- **Agent:** 使用者需求 → 輸入驗證 → 資料檢索 → 模型推論 → 解釋 → 來源可追溯的報告。
- **Evaluation:** MAE、RMSE、適當條件下的 MAPE、時空驗證、消融實驗、解釋穩定性與 Agent 可靠性。

研究區域、資料期間、樣本數與實驗結果均尚待確認，文件不包含虛構的效能數字。

## Submission Scope / 本階段作業範圍

目前已整理 A–F 摘要、5 Cores／3 Trade-offs／10 Details、Introduction Refactor、Related Work Refactor 與 IEEE 風格英文摘要。OpenSpec 安裝、規格與 workflow 轉換暫緩，不納入本階段。

## Editorial Checks / 文件一致性提醒

- 全部文件以同一研究題目、五項核心與三項研究問題為基礎。
- 所有方法與貢獻均屬規劃或待驗證假設，非已完成實驗結果。
- [Introduction](./introduction.md) 與 [Related Work](./related-work.md) 的引用編號目前**各自獨立**；未來整併成單篇論文時須統一參考文獻及引用順序。
- 正式繳交前，仍應與原始期末提案及課程評分規範逐項核對。
