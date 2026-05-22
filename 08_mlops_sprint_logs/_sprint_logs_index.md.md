# 📂 08_mlops_sprint_logs
> 專注於記錄 84 小時衝刺中，各模組之代碼落實、效能指標觀測（Profiling）與踩坑排除（Troubleshooting）的真實戰地日誌。

## 🎯 實戰日誌撰寫規範（Runbook Standard）
為了保持日誌的資深工程質量，每篇模組日誌建議嚴格包含以下三大硬核區塊：
1. **代碼快照與架構設計 (Code Snippets & Architecture)：** 記錄核心類別、自訂張量運算或關鍵介面（Interface）。
2. **效能觀測與實測指標 (Metrics & Profiling)：** 記錄本地環境（M4 Pro Max）的實測數據（吞吐量、記憶體佔用率、去重率等）。
3. **踩坑與排除紀錄 (Troubleshooting & Bug Fixes)：** 記錄遇到的異常（如 OOM、型態衝突）與根本原因分析（Root Cause Analysis）。

---

## 🗺️ 模組實戰日誌目錄
* **[08.1_mod1_config_&_safety](./08.1_mod1_config_&_safety.md)** (6h)
    * **實戰聚焦：** Hydra 階層式組態覆蓋測試、Pydantic v2 BaseSettings 強型態攔截驗證，以及動態 Thread 偵測調配邏輯之實作紀錄。
* **[08.2_mod2_curation_pipeline](08.2_mod2_curation_pipeline.md)** (16h)
    * **實戰聚焦：** Python `multiprocessing` 多核心吞吐瓶頸排除、手寫 MinHash + LSH 在百萬級文本的去重效能（Profiling）報告，以及 pytest 邊界條件單元測試通過紀錄。
* **[08.3_mod3_async_eval_engine](./08.3_mod3_async_eval_engine.md)** (12h)
    * **實戰聚焦：** `asyncio` 併發呼叫外部 API 的速率限制（Rate Limit）踩坑與排除、LLM-as-a-Judge 盲測 Rubric 提示詞優化歷程，以及 ROUGE/BLEU 差異報告自動導出實作。
* **[08.4_mod4_custom_training_loop](./08.4_mod4_custom_training_loop.md)** (24h)
    * **實戰聚焦：** 捨棄 Trainer 封裝親手落實 `loss.backward()` 核心迴路、Gradient Accumulation 記憶體控制實測、Apple Silicon MPS 統一記憶體 OOM 排查，以及 LoRA 權重 Merge 邏輯驗證。
* **[08.5_mod5_dpo_alignment](./08.5_mod5_dpo_alignment.md)** (10h)
    * **實戰聚焦：** 偏好數據（Chosen/Rejected）結構與長度校研管線實作、純手寫 $L_{DPO}$ 損失函數之 PyTorch Tensor 矩陣對數機率運算偵錯（Debugging）紀錄。
* **[08.6_mod6_cpt_data_packing](./08.6_mod6_cpt_data_packing.md)** (6h)
    * **實戰聚焦：** 手寫 Packing（文本打包）演算法之 Token 邊界切分點偵錯、PyTorch `IterableDataset` Streaming 流式加載時之記憶體水位監測紀錄。
* **[08.7_mod7_mlops_&_e2e_ci](./08.7_mod7_mlops_&_e2e_ci.md)** (10h)
    * **實戰聚焦：** 自訂訓練迴路與 WandB / MLflow 非同步即時推播整合、全生命週期整合測試腳本 `test_e2e_lifecycle.py` 在模擬數據下一鍵回歸的自動化驗證紀錄。