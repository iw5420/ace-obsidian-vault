# 🏛️ Ace’s Vault
**A Structured Knowledge Management System for Senior Java Engineers & AI Platform Architects**

## 🧠 Core Philosophy
* **First Principles:** Mastering underlying mechanisms (**JVM / JMM / Tensor Ops**) and memory models to ensure predictable system behavior under load.
* **Architectural Logic:** A systematic approach to decomposing complex business requirements into scalable, decoupled backend services and probabilistic AI pipelines.
* **Just-in-Time (JIT) Adaptation:** A high-density repository optimized for rapid knowledge synthesis, designed for high-concurrency technical decision-making, LLM customization, and senior-level/外商 tech interviews.

## 🗺️ Knowledge Themes
* **[00_Templates](./00_Templates/_Templates_Index.md)**
    * Standardizing note structures for algorithms, LLM evaluation rubrics, and technical analysis.
* **[01_Algorithms_&_DataStructures](./01_Algorithms/Algorithm%20MOC.md)**
    * **Algorithm Patterns:** Maintaining analytical sharpness through categorized data structures and core pattern practice. Emphasizes resource trade-offs.
* **[02_Java_Deep_Dive](./02_Java_Deep_Dive/_Java_Index.md)**
    * **Java Performance & Concurrency:** **JVM** tuning, **JMM** deep dives, thread-safe design, and lock-free programming for high-throughput systems.
* **[03_System_Design_&_Distributed](./03_System_Design_&_Distributed/_System_Design_Index.md)**
    * **Distributed Systems & Infrastructure:** Data consistency patterns, distributed transactions, and optimization for **RDBMS**, **Redis**, **Kafka**, and **ElasticSearch**.
* **[04_Cloud_&_Infrastructure](./04_Cloud_&_Infrastructure/_Cloud_Index.md)**
    * **Frameworks & Cloud:** Advanced **Spring Boot / Cloud** implementation (**RBAC / JWT**), **GCP** deployment, and **CI / CD** automation.
* **[05_Engineering_Practices](./05_Engineering_Practices/_Engineering_Index.md)**
    * **Engineering Rigor:** Applying **SOLID** principles and Design Patterns. Structured methodology for performance profiling and bottleneck identification.
* **[06_LLM_Lifecycle_&_Customization](./06_LLM_Lifecycle_&_Customization/_LLM_Customization_Index.md)**
    * **Probabilistic Core Optimization:** Industrial data curation (**MinHash LSH**), data density maximization (**Packing**), custom low-level **PyTorch + MPS** training loops with **Gradient Accumulation**, and mathematical alignment via **DPO Loss**. Target alignment with **NVIDIA NeMo** ecosystem.
* **[07_AI_Platform_&_MLOps](./07_AI_Platform_&_MLOps/_MLOps_Index.md)**
    * **AI Infrastructure & Safety:** Robust runtime configuration via **Hydra / Pydantic v2**, real-time experiment tracking with **WandB / MLflow**, high-concurrency non-asynchronous evaluation (**LLM-as-a-Judge**), and **TDD / E2E Integration Testing** for CI/CD-Ready lifecycle verification.

## 🛠️ Tooling
* **Platform:** Obsidian (Markdown-based)
* **Visuals:** Using **Mermaid.js** for sequence diagrams, system architecture flows, and LLM data pipeline lifecycles.
* **Equations:** Rendered via standard LaTeX math blocks for precise loss function derivations (e.g., DPO Log Likelihood).

---

# 🏛️ Ace’s Vault
**專為資深 Java 工程師與 AI 落地平台架構師打造的結構化知識管理系統**

## 🧠 核心理念
* **第一性原理：** 深入掌握底層運作機制 (**JVM / JMM / 張量運算**) 與記憶體模型，確保系統與模型訓練在高負載下的行為可預測性。
* **架構邏輯：** 系統化地將複雜業務需求拆解為可擴展、低耦合的後端服務，以及可控的機率性 AI 客製化管線。
* **現學現用 (JIT)：** 高密度的知識綜合庫，專為高併發技術決測、LLM 落地生命週期管理與頂級外商高階職位面試設計。

## 🗺️ 知識主題
* **[00_Templates](./00_Templates/_Templates_Index.md)**
    * 標準化筆記格式，包含刷題、技術分析模板與大模型盲測 Rubric 標準。
* **[01_Algorithms_&_DataStructures](./01_Algorithms/Algorithm%20MOC.md)**
    * **演算法模式：** 透過分類的資料結構與核心模式練習，保持分析問題的敏銳度，強調底層運作的資源花費取捨。
* **[02_Java_Deep_Dive](./02_Java_Deep_Dive/_Java_Index.md)**
    * **Java 效能與高併發：** **JVM** 調優、垃圾回收策略與 **JMM** 深度解析。執行緒安全設計與高吞吐量系統的無鎖編程。
* **[03_System_Design_&_Distributed](./03_System_Design_&_Distributed/_System_Design_Index.md)**
    * **分散式系統與基礎設施：** 資料一致性模型、分散式事務與共識邏輯。**RDBMS**、**Redis**、**Kafka** 與 **ElasticSearch** 的效能優化。
* **[04_Cloud_&_Infrastructure](./04_Cloud_&_Infrastructure/_Cloud_Index.md)**
    * **開發框架與雲端：** 進階 **Spring Boot / Cloud** 實作，專注於 **RBAC** 與安全 **JWT** 架構。混合雲部署策略 (**GCP**)、容器編排與 **CI / CD** 自動化。
* **[05_Engineering_Practices](./05_Engineering_Practices/_Engineering_Index.md)**
    * **工程嚴謹性與方法論：** 運用 **SOLID** 原則與設計模式構建企業級軟體。系統化效能剖析 (**Profiling**)、瓶頸識別與疑難雜症排查流程。
* **[06_LLM_Lifecycle_&_Customization](./06_LLM_Lifecycle_&_Customization/_LLM_Customization_Index.md)**
    * **大模型客製化生命週期（核心引擎）：** 工業級私有文本清理與 **MinHash LSH** 並行去重、消除 Padding 的大文本 **Packing（資料打包）技術**。捨棄 Trainer 封裝，手寫 **PyTorch + MPS** 低階訓練迴路與 **Gradient Accumulation（梯度累積）**；實作 **DPO（直接偏好最佳化）** 矩陣對數幾率損失函數。全面對標 **NVIDIA NeMo** 工具鏈底層邏輯。
* **[07_AI_Platform_&_MLOps](./07_AI_Platform_&_MLOps/_MLOps_Index.md)**
    * **企業級 AI 落地基礎設施（平台外殼）：** 基於 **Hydra + Pydantic v2** 的硬體自適應強型態組態系統。整合 **WandB / MLflow** 實時觀測 M4 Pro Max 統一記憶體頻寬與訓練指標。基於 `asyncio` 的高併發 **LLM-as-a-Judge 自動化盲測引擎**。遵循 **TDD** 規範，建構 CI/CD-Ready 的 **End-to-End 全生命週期自動化整合測試**。

## 🛠️ 工具規範
* **平台：** Obsidian (基於 Markdown)
* **視覺化：** 使用 **Mermaid.js** 繪製後端架構流程圖、時序圖與大模型數據全生命週期管道。
* **數學公式：** 涉及對齊（Alignment）與損失函數（Loss Function）之處，使用標準 LaTeX 進行數學推導與精確渲染。
