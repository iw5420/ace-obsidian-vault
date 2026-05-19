# 📂 06_llm_lifecycle_&_customization
> 專注於大模型領域適應與客製化的底層核心工程（Time Budget: 52 Hours）

## 🗺️ 核心子主題
* **[_lifecycle_plan.md](_lifecycle_plan.md.md)** (84h 全生命週期衝刺對照表)
    * 錨定 7 個開發模組之執行時間線與空間架構線的防走歪清單。
* **[06.1_data_curation_&_packing](./06.1_data_curation_&_packing.md)** (16h)
    * 利用 `multiprocessing` 進行多進程並行文字清理、手寫 MinHash + LSH 演算法實作海量去重，以及工業級 Continued Pre-Training (CPT) Packing（文本打包）防算力浪費機制。
* **[06.2_training_dynamics_&_mps](./06.2_training_dynamics_&_mps.md)** (24h)
    * 捨棄 High-level 封裝，手寫 Low-level PyTorch Custom Training Loop，實作 Gradient Accumulation（梯度累積）控制，並利用 PyTorch Profiler 進行 Apple Silicon MPS 的 Execution Trace 記憶體與算力瓶頸分析。
* **[06.3_alignment_&_dpo_loss](./06.3_alignment_&_dpo_loss.md)** (10h)
    * 建立 DPO 偏好數據校驗管道、純手寫 $L_{DPO}$ 損失函數，並進行矩陣對數機率（Log Likelihood）公式之數學推導與 LaTeX 註解。