# 📂 07_AI_Platform_&_MLOps
> 專注於運用後端架構嚴謹紀律框住 AI 系統的質量、觀測與軟體工程實踐（Time Budget: 32 Hours）

## 🗺️ 核心子主題
* **[07.1_Dynamic_Configuration_&_Safety](./07.1_Dynamic_Configuration_&_Safety.md)** (6h)
    * Hydra 階層式 YAML 組態設計、Pydantic v2 超參數與 M4 Pro Max 統一記憶體環境 Runtime 守門員機制。
* **[07.2_Experiment_Tracking_&_Observability](./07.2_Experiment_Tracking_&_Observability.md)** (6h)
    * 原生訓練迴路與 WandB / MLflow 非同步對接、Unified Memory 分配率與 Loss 觀測儀表板建置。
* **[07.3_Asynchronous_Evaluation_&_Metrics](./07.3_Asynchronous_Evaluation_&_Metrics.md)** (12h)
    * ROUGE-1/2/L、BLEU、Exact Match 指標封裝、`asyncio` 高併發 LLM-as-a-Judge 盲測引擎與 Rubric 設計。
* **[07.4_Automated_E2E_Validation](./07.4_Automated_E2E_Validation.md)** (8h)
    * 針對 Data Cleaner 的 `pytest` 邊界測試、`test_e2e_lifecycle.py` 自動化一鍵整合測試（CI/CD-Ready 驗證）。