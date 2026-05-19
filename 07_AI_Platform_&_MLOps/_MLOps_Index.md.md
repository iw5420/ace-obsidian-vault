# 📂 07_ai_platform_&_mlops
> 專注於運用後端架構嚴謹紀律框住 AI 系統的質量、觀測與軟體工程實踐（Time Budget: 32 Hours）

## 🗺️ 核心子主題
* **[07.1_dynamic_config_&_safety](./07.1_dynamic_config_&_safety.md)** (6h)
    * 基於 Hydra 實作階層式 YAML 組態設計，引入 Pydantic v2 進行超參數強型態邊界校驗，並建置 M4 Pro Max 統一記憶體（Unified Memory）環境之 Runtime 守門員偵測機制。
* **[07.2_tracking_&_observability](./07.2_tracking_&_observability.md)** (6h)
    * 實作原生自訂訓練迴路與 WandB / MLflow 的非同步（Asynchronous）資料對接，建置 Unified Memory 分配率、記憶體頻寬與 Loss 實時觀測儀表板。
* **[07.3_async_evaluation_&_metrics](./07.3_async_evaluation_&_metrics.md)** (12h)
    * 封裝 `ROUGE-1/2/L`、`BLEU`、`Exact Match` 等標準 NLP 評估指標，並利用 Python `asyncio` 建置高併發外部 LLM-as-a-Judge 盲測引擎與嚴格的評分提示詞（Rubric）架構。
* **[07.4_automated_e2e_validation](./07.4_automated_e2e_validation.md)** (8h)
    * 遵循 TDD 規範落實針對 Data Cleaner 邊界條件的 `pytest` 單元測試，並撰寫 `test_e2e_lifecycle.py` 全生命週期自動化一鍵整合測試，達成系統 CI/CD-Ready 驗證。