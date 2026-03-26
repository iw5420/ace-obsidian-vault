### ⏳ Complexity Analysis / 複雜度分析

- **Time Complexity (時間複雜度): $O(n)$**
    - **分析**：演算法僅需遍歷一次陣列。
    - 在迴圈內部，`HashSet.add()` 的操作涉及計算 Hash Code 並定位 Bucket，其**平均時間複雜度為 $O(1)$**。
    - **最壞情況**：若發生嚴重的 Hash Collision（哈希衝突），單次操作可能退化，但在 Java 8+ 中，當鏈表長度超過 8 會轉為紅黑樹，確保最壞情況為 $O(\log k)$。
    - **總計**：$O(n \times 1) = O(n)$。
        
- **Space Complexity (空間複雜度): $O(n)$**
    - **分析**：在最壞情況下（即陣列中**沒有重複元素**），我們必須將陣列中的所有 $n$ 個元素全部存入 `HashSet` 中。
    - **空間消耗**：隨輸入資料量 $n$ 線性增長。
- **Tradeoff**：我們利用了 $O(n)$ 的額外空間，將暴力解的 $O(n^2)$ 時間複雜度降至極致的 $O(n)$。
---
### 🛠 技術細節與深度分析 (Technical Deep Dive)
#### 1. HashSet 的底層實作 (Under the Hood)
在 Java 中，`HashSet` 其實是封裝了一個 `HashMap`，並將元素存放在 `Map` 的 `Key` 位置。
- **關鍵機制**：`set.add(e)` 內部呼叫的是 `map.put(e, PRESENT)`。如果 `put` 回傳 `null`，代表該 Key 原本不存在；如果回傳 `PRESENT`（一個虛擬的 Object），代表 Key 已存在。
- **提早結束 (Early Exit)**：一旦 `add()` 回傳 `false`（或檢查到元素已存在），程式立即 return，這在平均情況下比「全部塞完再檢查 size」效率更高。
#### 2. Java 集合框架的效能考量 (Performance Tuning)
針對 **Airwallex** 或 **Coupang** 這類對 Java 效能要求極高的職位，可以補充：
- **Load Factor & Rehash**：`HashSet` 預設的加載因子是 **0.75**。當元素數量達到 `capacity * 0.75` 時會觸發 **Rehash**（擴容一倍並重新分配位置）。若已知陣列長度為 $L$，初始化時使用 `new HashSet<>((int)(L / 0.75) + 1)` 可有效避免中途擴容導致的 CPU 抖動。
- **Memory Overhead**：`HashSet<Integer>` 對於每個 int 都需要封裝成 `Integer` 對象。在 **TenMax** 這類追求效能頂點的場景，這會產生顯著的 GC 壓力與記憶體浪費（一個 `Integer` 對象約佔 16-24 bytes，而原始型別 `int` 僅需 4 bytes）。

### 效能對比表

| **特性**    | **HashSet<Integer>** | **IntOpenHashSet (fastutil)** | **BitSet**     |
| --------- | -------------------- | ----------------------------- | -------------- |
| **記憶體佔用** | 極高 ($>10x$)          | 極低 (接近原生)                     | 最小 (1 bit/val) |
| **GC 壓力** | 高 (產生大量對象)           | 無 (基於陣列)                      | 無              |
| **自動裝箱**  | 有                    | 無                             | 無              |
| **適用場景**  | 通用、數據量小              | **高頻交易、大數據**                  | **密集型整數檢索**    |

#### 3. 排序法 (Sorting) 的備選方案

在面試中，如果面試官限制**「不准使用額外空間」**：
- 可以提出先進行 `Arrays.sort(nums)` 再遍歷。
- 雖然時間複雜度升至 $O(n \log n)$，但空間複雜度可降至 $O(1)$（不計排序本身的遞迴棧空間）。這展現了你在不同資源限制下（CPU vs RAM）權衡利弊（Tradeoff）的能力。