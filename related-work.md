# Related Work — Real Estate Decision AI Agent

**研究題目：結合實價登錄、空間特徵與可解釋人工智慧之房地產決策代理人**  
**Status:** Literature-grounded proposal draft (2026-11-13). No project experiments have yet been conducted.  
**Companion files:** [Abstract](./abstract.md) · [Research Analysis](./research-analysis.md) · [Introduction](./introduction.md)

## 2. Related Work

### 2.1 Hedonic Valuation and Machine-Learning House Price Prediction

房價分析的經典基礎之一是特徵價格模型（hedonic pricing model）。Rosen [1] 從產品差異化與隱含價格的角度，提供理解住宅面積、區位、建物品質等屬性如何與價格相關的經濟學架構。此理論可作為房價迴歸模型的概念基礎，但在實際預測問題中，研究者仍須處理非線性關係、特徵交互作用及資料分布差異。

相較於單純線性估計，機器學習提供處理多維資料及複雜關係的候選方法。Chen 與 Guestrin [2] 提出的 XGBoost 為可擴充的梯度提升樹系統，特別強調稀疏資料處理及計算效率；該研究並非專門針對房地產資料，因此不能直接據此斷言 XGBoost 在臺灣房價資料上優於所有其他模型。本研究將以線性迴歸為透明基準，並將 Random Forest、XGBoost 作為候選方法，在相同資料切分與評估條件下比較其表現。

**與本研究的連結：** 以特徵價格觀點定義建物與環境變數，並透過多模型比較檢驗非線性方法是否具有實際預測增益。研究不預先假定某模型為最佳。

### 2.2 Spatial Dependence and Generalization

不動產具有高度區位依賴性，地理相近物件可能共享交通、學區、公共設施或區域市場條件。Bourassa、Cantoni 與 Hoesli [3] 比較多種考慮空間相依性的房價預測方法，包含一般最小平方法、鄰近殘差調整、地理統計與趨勢曲面方法。其研究以美國 Louisville 約 13,000 筆交易資料進行比較，發現在該研究設定下，結合細分次市場變數的地理統計模型表現較佳。此結論具有特定資料與模型條件，不應直接套用為臺灣市場的普遍定律。

空間資料的評估設計亦相當重要。Roberts 等人 [4] 討論具有時間、空間及階層相依結構資料的交叉驗證策略，指出若忽略資料依賴性，隨機交叉驗證可能低估預測誤差。雖然該文主要來自生態建模領域，其方法論對房價資料的時空相依評估仍具參考價值。房地產研究若將相近位置或相近時點的交易隨機分配至訓練集與測試集，可能得到過於樂觀的評估結果。

**與本研究的連結：** 擬建立建物屬性模型與加入 GIS 特徵的模型，並比較隨機、時間及適當的區域分組／空間封鎖驗證。空間特徵的效益將以跨時間及跨區域誤差衡量，而不僅以隨機切分成績判斷。

### 2.3 Explainable Artificial Intelligence for Valuation

高效能預測模型若欠缺可理解的依據，可能降低其在重大財務決策中的實用性。Ribeiro、Singh 與 Guestrin [5] 提出的 LIME 使用局部可解釋模型近似黑箱預測，以協助理解個別結果；其原始研究重點為一般分類器的解釋，並非直接驗證房價估價系統。

Lundberg 與 Lee [6] 提出 SHAP，建立加法式特徵歸因的統一架構，可為個別預測分配特徵貢獻值。對房價預測而言，SHAP 可協助描述某筆預測中，面積、屋齡、區位或交通特徵如何影響模型輸出。然而，SHAP 解釋的是特定模型的預測行為，而非直接證明某項住宅特徵對市場價格具有因果影響。若特徵高度相關，解釋結果也可能受模型與歸因設定影響。

**與本研究的連結：** 以 SHAP 作為主要解釋工具，結合相似交易案例與模型不確定性，並評估解釋穩定性及使用者理解度。避免僅提供單一價格或將特徵歸因誤當因果證據。

### 2.4 LLM Agents, Tool Use, and Grounded Decision Support

大型語言模型可以將自然語言需求轉換為多步驟資訊處理任務，但其自行生成的數字與事實不一定可驗證。Yao 等人 [7] 提出的 ReAct 將語言模型的推理與外部行動交替結合，使模型得以透過外部工具取得資訊、更新工作狀態並處理部分錯誤情境。該研究在問答、事實查核及互動任務中驗證此設計概念，但未直接證明其可用於房價估值或不動產購屋建議。

本研究借鑑「推理／規劃與工具行動分工」的概念，將語言模型限制於意圖理解、問題澄清、任務協調與結果呈現；交易查詢、數值計算、價格預測及 SHAP 解釋則由可測試、可追蹤的工具執行。當資料不足或模型超出適用範圍時，系統應揭露限制或要求補充條件，而非憑語言模型自行生成成交案例。

**與本研究的連結：** 擬比較工具錨定 Agent、直接 LLM 回答與傳統查詢流程，評估來源引用完整率、數值一致性、工具呼叫正確率及任務完成效率。

### 2.5 Comparative Literature Matrix

| Ref. | Research | Main contribution | Scope / limitation relevant to this proposal | Planned use |
|---|---|---|---|---|
| [1] | Rosen (1974) | 特徵價格與隱含市場的理論架構 | 理論基礎；非完整 AI 估價系統 | 定義建物與環境價格特徵 |
| [2] | Chen & Guestrin (2016) | 可擴充梯度提升樹 XGBoost | 通用演算法，非臺灣房價實證 | 非線性模型候選與效能比較 |
| [3] | Bourassa et al. (2010) | 比較房價預測中的空間相依處理方法 | 美國單一研究資料情境，外部效度待檢驗 | 空間特徵與次市場模型設計 |
| [4] | Roberts et al. (2017) | 時空依賴資料的交叉驗證策略 | 主要為跨領域方法論，非房價系統 | 時間／空間切分與防止樂觀偏誤 |
| [5] | Ribeiro et al. (2016) | LIME 局部模型解釋 | 原始方法以一般分類問題為主 | 解釋方法的比較背景 |
| [6] | Lundberg & Lee (2017) | SHAP 特徵歸因架構 | 歸因不等於因果，需檢驗穩定性 | 個案房價解釋與透明度 |
| [7] | Yao et al. (2023) | ReAct 推理與工具行動整合 | 非房地產領域的直接實證 | Agent 工具使用與來源追蹤 |

> **閱讀範圍說明：** 此表依已核對的出版資訊與論文摘要／公開內容建立，屬提案階段的代表性文獻比較，並非完整系統性文獻回顧；不宣稱已窮盡臺灣房價或房地產 Agent 研究。

### 2.6 Research Gap and Proposed Positioning

前述研究分別提供了房價特徵建模、空間依賴處理、模型解釋及工具使用的理論與方法基礎。然而，這些代表性文獻並未共同構成一套針對臺灣公開實價登錄資料、同時驗證時空泛化、估價解釋、來源追溯與自然語言決策互動的完整評估框架。這是**本提案選擇切入的整合研究問題**，而非對全球所有既有研究均缺乏此類系統的斷言。

本研究擬從以下四個面向補足上述整合需求：

1. **資料可信度：** 建立交易資料清理、特殊交易辨識、欄位定義與版本追蹤流程。
2. **時空泛化：** 以時間與空間驗證及消融實驗，檢驗 GIS 特徵是否帶來可重現的增益。
3. **解釋與限制：** 結合 SHAP、相似交易與適當的不確定性呈現，避免將估價數字誤認為確定行情。
4. **工具錨定互動：** 讓 Agent 將自然語言需求轉為可追溯的查詢與模型推論，並測試其數值可靠性及錯誤降級能力。

研究成果須經實驗驗證，不能僅由架構整合推論其預測準確度或使用者效益必然優於既有方法。

### 2.7 Mapping to Research Questions and Planned Evaluation

| Research question | Literature basis | Planned comparison | Evaluation evidence |
|---|---|---|---|
| **RQ1** 空間特徵與泛化 | [1], [2], [3], [4] | 基準模型 vs. 空間特徵模型；不同切分策略 | MAE、RMSE、MAPE、跨區域與跨時間誤差 |
| **RQ2** 解釋與理解 | [5], [6] | 有／無 SHAP 及相似案例說明 | 解釋穩定性、理解度、對不確定性的判讀 |
| **RQ3** Agent 可靠性 | [7] | 工具錨定 Agent vs. 直接 LLM／傳統查詢 | 來源完整率、數值一致性、任務完成率與時間 |

## References (IEEE-style numbering)

[1] S. Rosen, “Hedonic Prices and Implicit Markets: Product Differentiation in Pure Competition,” *Journal of Political Economy*, vol. 82, no. 1, pp. 34–55, 1974. doi: [10.1086/260169](https://doi.org/10.1086/260169).

[2] T. Chen and C. Guestrin, “XGBoost: A Scalable Tree Boosting System,” in *Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining*, 2016, pp. 785–794. doi: [10.1145/2939672.2939785](https://doi.org/10.1145/2939672.2939785).

[3] S. C. Bourassa, E. Cantoni, and M. Hoesli, “Predicting House Prices with Spatial Dependence: A Comparison of Alternative Methods,” *Journal of Real Estate Research*, vol. 32, no. 2, pp. 139–160, 2010. doi: [10.1080/10835547.2010.12091276](https://doi.org/10.1080/10835547.2010.12091276).

[4] D. R. Roberts *et al*., “Cross-validation strategies for data with temporal, spatial, hierarchical, or phylogenetic structure,” *Ecography*, vol. 40, no. 8, pp. 913–929, 2017. doi: [10.1111/ecog.02881](https://doi.org/10.1111/ecog.02881).

[5] M. T. Ribeiro, S. Singh, and C. Guestrin, “Why Should I Trust You?: Explaining the Predictions of Any Classifier,” in *Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining*, 2016, pp. 1135–1144. doi: [10.1145/2939672.2939778](https://doi.org/10.1145/2939672.2939778).

[6] S. M. Lundberg and S.-I. Lee, “A Unified Approach to Interpreting Model Predictions,” in *Advances in Neural Information Processing Systems*, vol. 30, 2017. [Online]. Available: https://proceedings.neurips.cc/paper_files/paper/2017/hash/8a20a8621978632d76c43dfd28b67767-Abstract.html

[7] S. Yao *et al*., “ReAct: Synergizing Reasoning and Acting in Language Models,” in *International Conference on Learning Representations (ICLR)*, 2023. [Online]. Available: https://mlanthology.org/iclr/2023/yao2023iclr-react/

---

**Verification and editorial notes**

- Bibliographic records and source abstracts were checked against publisher, proceedings, or scholarly index pages on **2026-11-13**. DOI and publisher links above provide direct verification.
- Some source articles may require institutional access for full-text reading. The discussion deliberately avoids claims about unverified tables, detailed numerical results or exhaustive literature coverage.
- Citation numbers in this standalone Related Work file are **local to this file**. Before assembling a single manuscript, renumber citations consistently with `introduction.md` and any other chapters.
- This is a literature-based proposal refactor, **not** an experimental results section.
- OpenSpec is deferred and **not included** in this step.
