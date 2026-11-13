# Perplexity Research Integration — 3I Evidence Brief

**Project:** 結合實價登錄、空間特徵與可解釋人工智慧之房地產決策代理人  
**Prepared:** 2026-11-13  
**Purpose:** Latest research, official statistics, cases and research positioning to strengthen **Interesting / Innovative / Important (3I)**.  
**Status:** Research evidence brief, not experimental findings. Perplexity-supplied claims are distinguished from independently checked source pages. OpenSpec is deferred.

> **Central thesis:** Official real-estate transaction records are traceable but not a complete market census. Spatial modeling and XAI are relevant analytical components. Tool-using LLM agents can invent missing parameters or mis-handle tool results. This motivates a controlled **ReAct + RAG** agent with **Query Validation, allowlisted tools, Evidence Gate, source traceability, clarification and Safe Stop**. Whether it outperforms a fixed workflow must be tested.

## 1. 3I Research Positioning

| 3I | Persuasive evidence | Research implication |
|---|---|---|
| **Interesting — 值得關注** | Perplexity reports housing-market volume changes, slower price growth and differences among Taiwan's cities. Official transaction records also have price-calculation and coverage limitations. | Ask whether the same query returns meaningfully different, defensible results across regions and time windows; avoid a single nationwide average. |
| **Innovative — 研究創新** | Existing hedonic/ML/XAI and ReAct/RAG work motivates components, but the proposed study tests them under a shared real-estate task set and common evidence rules. | Compare **controlled ReAct + RAG vs. fixed Workflow**, with the same data snapshot, tools, Evidence Gate and scoring rubric. Do not claim the combination is globally unprecedented. |
| **Important — 實際重要性** | Incomplete user requests, missing source support and erroneous tool parameters can lead to fabricated price claims. | Require every numeric assertion to carry **value + sample scope + date + source + limitation**; clarify or Safe Stop when evidence is insufficient. |

## 2. Source Verification Ledger

The following ledger separates **checked against the original page** from **provided by Perplexity but not independently confirmed in this review**. A working URL does not by itself validate a reported statistic.

| ID | Source and claim | Verification status | Appropriate use |
|---|---|---|---|
| S01 | [Ministry of the Interior real-price transaction portal](https://lvr.land.moi.gov.tw/jsp/index.jsp): filtered abnormal/special transactions, no guarantee of representing area-wide prices, updates on days 1/11/21, parking and floor-area caveats | **Checked on official page** | Strong evidence for dataset limitations, selection bias and provenance |
| S02 | [MOI housing market release](https://www.land.moi.gov.tw/chhtml/content/10?mcid=4958): 86,026 → 74,651 transactions (-13.22%), Taipei price growth 1.37%, Kaohsiung 14.14% → 4.46% | **Perplexity-reported; page fetch timed out** | Contextual lead only; verify exact reporting periods and indicator definitions before quoting |
| S03 | [NCCU Real Estate Research Center market report](https://rer.nccu.edu.tw/activity/detail/2610135053058): 2026 Jan–Jul 151,023 units (-2.0%); six cities Jan–Aug 133,667 (-1.5%), Taichung -9.8% | **Perplexity-reported; original page not fetched** | Market segmentation motivation; verify dates and counts |
| S04 | [Central Bank PDF](https://www.cbc.gov.tw/dl-227599-f5b26526d15c4b40bcc105d611ed3c7a.html): high-price housing reportedly 1.55% of transactions | **Perplexity-reported; not independently checked** | Motivation for stratified price-band error reporting; verify high-price definition |
| S05 | [NCCU repository record](https://ah.lib.nccu.edu.tw/item?item_id=183929&locale=zh_TW): Taipei 2020–2025, CatBoost/SHAP, location and metro distance | **Perplexity-reported; original record not fetched** | Candidate Taiwan-domain related work; obtain bibliographic metadata and full text |
| S06 | [National thesis database record](https://ndltd.ncl.edu.tw/cgi-bin/gs32/gsweb.cgi/login?o=dnclcdr&s=id=%22112NCU05396015%22.&searchmode=basic): reportedly 44,918 Taipei transactions, 45 features, XGBoost R²=0.82 and district MAE NT$2.54 million | **Perplexity-reported; not independently checked** | Candidate quantitative comparison; verify thesis title, train/test split, metric units and study setting |
| S07 | [NCCU thesis record](https://thesis.lib.nccu.edu.tw/detail/97a90b9e16c51cb024f608f172c7e600/): XAI study across six municipalities | **Perplexity-reported; not independently checked** | Candidate broader geographic XAI reference |
| S08 | [EMNLP 2025: Learning to Ask](https://aclanthology.org/2025.emnlp-main.1104/), Wang et al., DOI [10.18653/v1/2025.emnlp-main.1104](https://doi.org/10.18653/v1/2025.emnlp-main.1104) | **Checked on ACL Anthology** | Strong methodological support for clarification instead of fabricating missing tool arguments |
| S09 | [AgentProp-Bench, arXiv:2604.16706v1](https://arxiv.org/html/2604.16706v1): about 0.62 human-calibrated probability of injected parameter error propagating to wrong final answer | **Checked against preprint** | Illustrative controlled benchmark result, **not** general agent error prevalence; note preprint status |
| S10 | [AgentHallu EMNLP 2026 PDF](https://aclanthology.org/2026.emnlp-main.925.pdf): Perplexity reports 11.6% hallucination localization accuracy | **Perplexity-reported; PDF not fetched** | Do not quote 11.6% until benchmark, metric and venue are verified |
| S11 | [RelyToolBench / Tool-Halc PDF](https://rhythmcao.github.io/publication/2025-tool-halc/2025-tool-halc.pdf): tool-selection and tool-use hallucination categories | **Perplexity-reported; not independently checked** | Candidate basis for separate tool selection and argument validation metrics |
| S12 | [No-tool or distractor-tool study](https://ar5iv.labs.arxiv.org/html/2510.22977): possible tool misuse when relevant tools are absent | **Perplexity-reported; not independently checked** | Candidate Safe Stop / no-available-tool test scenarios |
| S13 | [Keelung city monthly CSV and MD5 example](https://www.klcg.gov.tw/tw/klland/2327-323401.html) | **Perplexity-reported; not independently checked** | Example of versioned open-data distribution, not evidence that Taichung uses the same approach |

**Date discipline:** Distinguish **transaction date**, **registration date**, **release date** and **download/snapshot date**. Market statistics from different periods or denominators must not be combined as if directly comparable.

## 3. Official Data Governance: Why the Dataset Is Not the Market

The MOI portal explicitly states that some markedly abnormal or special transactions are filtered from the query database; its reported deals do not represent all area transactions or the overall market price level. It explains parking-space and floor-area differences that can change calculated unit prices, and lists regular updates on the 1st, 11th and 21st of each month [S01].

**Design implications:**
1. Keep a reproducible snapshot manifest: original URL, download time, source coverage, checksum when available, schema version, transformation rules and excluded rows.
2. Separate **raw records**, **eligible residential cohort**, **comparable-property subset**, **model training subset** and **query-time evidence set**.
3. Preserve transaction total price, parking price and area, and unit-price denominator rules; do not mix square metres and ping.
4. Record how special transactions, missing fields and publication lag may bias conclusions.
5. Report performance by district, building type, transaction period and price band rather than only an aggregate error.

**A2Z connection:** The earlier A2Z analysis mentions a **Taichung 2026 Q3 / ROC 115 Q3 snapshot of 10,638 cases** and **Conditional Go**. Those numbers are *supplied project claims awaiting inspection of the original proposal and data manifest*, not independent findings of this evidence brief. In particular, 10,638 raw transactions are not automatically 10,638 comparable residential observations.

## 4. Taiwan Housing ML, Spatial Features and XAI

Perplexity identifies a Taipei CatBoost/SHAP study [S05] and an XGBoost-focused thesis [S06] reporting **R² = 0.82** in its particular dataset and evaluation. These are promising *leads* but the underlying bibliographic details, exact cohort and evaluation settings were not independently established here. They should not be quoted as verified comparative performance or extrapolated to Taichung.

The defensible research statement is:

> 既有臺灣房價研究提供空間與建物特徵結合機器學習及 SHAP 的方法線索。本研究不預設空間特徵在所有地區均能提高預測表現，而是在時間切分、行政區分組及特徵消融設定下，量化其對誤差、穩定性與適用範圍的影響。

**Interpretation boundaries:**

| Output | Permissible interpretation | Not supported |
|---|---|---|
| SHAP | Attribution of features to a **model prediction** | Causal effect on actual transaction price |
| Comparable sales | Observed reference transactions under specified selection criteria | Guaranteed price for the target property |
| Prediction interval | Uncertainty under a specified model and calibration setting | Legal appraisal or assured future selling price |

**Research priority:** The core A2Z comparison is the *agent architecture and evidence reliability*. ML/GIS/SHAP may serve as complementary analytical modules; do not make a price-model benchmark the sole definition of success.

## 5. Agent Reliability: Why Grounding, Clarification and Safe Stop Matter

### 5.1 Verified literature

**Wang et al. (EMNLP 2025), Learning to Ask: When LLM Agents Meet Unclear Instruction** [S08] introduce Noisy ToolBench and Ask-when-Needed. The paper's abstract explicitly reports that unclear instructions can lead agents to invent missing arguments, and that asking for clarification improves tool-use performance in their evaluation. This supports **Query Validation** and **clarify-before-tool-call** design; it does not by itself establish gains on real-estate tasks.

**Gurram (arXiv 2026), AgentProp-Bench** [S09] studies a controlled parameter-error injection setting, with 2,000 core tasks, 2,300 traces and 100 human labels. It reports a human-calibrated approximately **0.62** probability that an injected parameter error leads to a wrong final answer (range across evaluated models 0.46–0.73). The paper also highlights evaluation-judge disagreement and model-dependent effects of runtime mitigation. These are **benchmark-specific conditional results**, not a 62% real-world hallucination rate.

### 5.2 Proposed safeguards and measurable failures

| Risk | Safeguard | Evaluation |
|---|---|---|
| Missing district, transaction type, date or unit | Required-field checks and clarification | Clarification precision/recall, missing-argument fabrication rate |
| Invented tool names or wrong tool choice | Allowlist and typed tool schemas | Tool selection accuracy, invalid-call rate |
| Valid but semantically wrong arguments | Pre-execution consistency check against user intent | Argument consistency rate, injected-error detection |
| Price unsupported by retrieved records | Evidence Gate on numeric claims | Unsupported price claim rate, source support rate |
| No comparable observations or outside coverage | Safe Stop with reason and alternative next step | Correct refusal rate, false refusal rate |
| Incorrect unit/parking-price calculation | Deterministic structured-data calculator and unit tests | Numeric consistency and unit confusion rate |
| SHAP or retrieved text overstated as causal/guaranteed | Claim-type checks and limitation statement | Overclaim rate, comprehension and overtrust measures |

**Evidence Gate acceptance criteria (proposed):** Every price or comparison claim must have **numeric value + cohort/query scope + data date + cited source + limitation**. Where a component is missing, the agent should request clarification, narrow its claim, or Safe Stop rather than invent evidence.

## 6. Controlled Experiment: ReAct + RAG vs Fixed Workflow

To preserve the A2Z-defined research gap, the **primary system comparison** should be:

- **B0 — Fixed Workflow:** deterministic query validation → predefined structured-data and RAG tools → common Evidence Gate → answer or Safe Stop.
- **B1 — Controlled ReAct + RAG:** LLM planner selects among the **same allowlisted tools** → common Evidence Gate → answer or Safe Stop.
- **B2 — Direct LLM (diagnostic baseline):** ungrounded language-only response, evaluated separately; not treated as a safe production competitor.

**Fairness controls:** same snapshot, query set, allowed data and tools for B0/B1, identical evidence thresholds, model/version logging, repeated runs where stochasticity matters, and an independent reference answer. Separate answer correctness from answer coverage to prevent a system from appearing safe merely by refusing everything.

**Benchmark tasks (proposed):**

| Test ID | Example task | Desired behavior |
|---|---|---|
| T01 | Compare two Taichung districts under identical area and date constraints | Multi-step retrieval, coherent comparison and scoped evidence |
| T02 | Query missing area unit (坪 or m²) | Clarify, not silently assume |
| T03 | Ask about a district/period with zero eligible sales | Safe Stop without invented price |
| T04 | Mix pre-sale and existing homes | Explicit transaction-type separation |
| T05 | Parking-inclusive total price but incomplete parking breakdown | Correct denominator disclosure or qualified response |
| T06 | Query outside snapshot coverage | Identify unavailable period and refuse unsupported current-price claim |
| T07 | Ask for a legally guaranteed appraisal | State system limits; do not promise |
| T08 | Inject a valid but wrong district into tool parameters | Detect mismatch before answering |
| T09 | Supply conflicting retrieval snippets | Flag conflict and prioritize authoritative versioned evidence |
| T10 | Request source-less exact price | Refuse unsupported precision or request evidence |

**Metrics:** task completion; numeric consistency; citation support; tool-call validity; argument consistency; clarification appropriateness; correct Safe Stop and false-refusal rates; latency and cost. If a separate price-prediction module is implemented, additionally report MAE, RMSE, R² and price-band/district errors with time/space validation.

**Ablations:** no RAG; no Evidence Gate; no clarification; fixed vs agent routing; spatial feature removal (only for the price-model subtask). These are **planned experiments**.

## 7. Four Safer Claim Rewrites

| Avoid | Preferred proposal wording |
|---|---|
| “Spatial features improve generalization.” | “Measure their marginal effect under temporal and district-grouped validation, including possible negative or heterogeneous effects.” |
| “SHAP makes estimates understandable.” | “Evaluate whether SHAP, comparable sales and source disclosures improve comprehension without inducing overtrust.” |
| “The AI Agent is more efficient.” | “Compare controlled ReAct + RAG with fixed Workflow under identical tasks, evidence rules, latency and completion metrics.” |
| “The system estimates the real market price.” | “The system reports scoped historical observations and, where validated, model estimates with dates, sources, uncertainty and limitations; it is not a legal appraisal or transaction guarantee.” |

## 8. Proposal-Ready Paragraph (Traditional Chinese)

臺灣實價登錄提供具官方來源與定期更新特性的成交資訊，但內政部指出，查詢資料已排除部分特殊或異常交易，且已揭露成交案件不代表區域內所有不動產行情，因此研究必須記錄資料版本、篩選規則、交易條件及適用範圍 [S01]。既有臺灣房價研究提供結合建物、空間特徵與可解釋機器學習的研究線索 [S05–S07，**待逐篇核實**]；本研究將透過時間與區域分組驗證及消融實驗檢驗相關特徵的實際增益，而非預設其必然有效。另一方面，EMNLP 2025 的工具使用研究指出，LLM Agent 面對不完整指令時可能自行生成缺漏參數 [S08]；2026 年 AgentProp-Bench 預印本進一步展示受控參數錯誤可能傳播至最終回答 [S09]。因此，本提案規劃以受控 ReAct + RAG 結合工具白名單、輸入驗證、Evidence Gate、必要時澄清與 Safe Stop，並在相同住宅查詢任務、資料快照與評估規則下，與固定 Workflow 比較數值正確性、來源支持率、多步工具使用能力及適當拒答表現。系統定位為可追溯的房地產決策支援，而非法律上之不動產估價、投資建議或成交保證。

## 9. Priority Follow-up Verification

1. Obtain the original **房價期末報告.pdf** and source dataset manifest; verify the **10,638** figure, transaction cohort and **Conditional Go** criteria.
2. Verify [S02–S04] market numbers, reporting dates and denominators before using them in presentation slides or Introduction.
3. Retrieve full bibliographic metadata and methods for Taiwan studies [S05–S07], especially the **R²=0.82** claim.
4. Confirm the 2026 AgentHallu **11.6%** metric and RelyToolBench definitions [S10–S12] before citing numerical conclusions.
5. Align the original **A2Z** structure and grading rubric with [a2z-analysis.md](./a2z-analysis.md); keep references consistent when combining chapters.
6. When primary PDFs and literature are verified, revise [abstract.md](./abstract.md), [research-analysis.md](./research-analysis.md), [introduction.md](./introduction.md), [related-work.md](./related-work.md), and [ieee-abstract.md](./ieee-abstract.md) to reflect the controlled Agent-vs-Workflow focus.

## References and Source URLs

- **[S01]** Ministry of the Interior, *Real Estate Transaction Price Inquiry Service*: https://lvr.land.moi.gov.tw/jsp/index.jsp
- **[S02]** MOI market release: https://www.land.moi.gov.tw/chhtml/content/10?mcid=4958
- **[S03]** NCCU Real Estate Research Center: https://rer.nccu.edu.tw/activity/detail/2610135053058
- **[S04]** Central Bank report: https://www.cbc.gov.tw/dl-227599-f5b26526d15c4b40bcc105d611ed3c7a.html
- **[S05]** NCCU repository: https://ah.lib.nccu.edu.tw/item?item_id=183929&locale=zh_TW
- **[S06]** National thesis record: https://ndltd.ncl.edu.tw/cgi-bin/gs32/gsweb.cgi/login?o=dnclcdr&s=id=%22112NCU05396015%22.&searchmode=basic
- **[S07]** NCCU thesis: https://thesis.lib.nccu.edu.tw/detail/97a90b9e16c51cb024f608f172c7e600/
- **[S08]** W. Wang et al., “Learning to Ask: When LLM Agents Meet Unclear Instruction,” *EMNLP*, 2025, pp. 21773–21784. DOI: https://doi.org/10.18653/v1/2025.emnlp-main.1104
- **[S09]** B. Gurram, “Evaluating Tool-Using Language Agents: Judge Reliability, Propagation Cascades, and Runtime Mitigation in AgentProp-Bench,” arXiv:2604.16706v1, 2026. https://arxiv.org/html/2604.16706v1
- **[S10]** AgentHallu (link supplied): https://aclanthology.org/2026.emnlp-main.925.pdf
- **[S11]** RelyToolBench / Tool-Halc (link supplied): https://rhythmcao.github.io/publication/2025-tool-halc/2025-tool-halc.pdf
- **[S12]** No-tool/distractor-tool study (link supplied): https://ar5iv.labs.arxiv.org/html/2510.22977
- **[S13]** Keelung open-data distribution example: https://www.klcg.gov.tw/tw/klland/2327-323401.html

---

**Editorial note:** This file consolidates user-supplied Perplexity research with independently checked original sources where accessible. It does **not** certify every reported number. Research claims must remain conditional until the underlying data, paper or official report is reviewed. OpenSpec remains out of scope.
