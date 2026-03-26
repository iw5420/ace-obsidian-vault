## ⏳ Complexity Analysis / 複雜度分析

### Time Complexity (時間複雜度): $O(N)$
- **分析**:
    - 演算法僅需**遍歷一次**陣列。
    - 在迴圈內部，`HashMap.containsKey()` 與 `HashMap.get()` 的平均時間複雜度皆為 **$O(1)$**。
    - **總計**: $O(N \times 1) = O(N)$。
### Space Complexity (空間複雜度): $O(N)$
- **分析**:
    - 最壞情況下（例如目標值在陣列最後，或根本不存在），我們需要將前 $N-1$ 個元素全部存入 `numMap` 中。
    - 空間消耗隨輸入資料量 $N$ 線性增長。
    - **Tradeoff**: 我們犧牲了空間（額外的 Map）來換取極致的時間效能。
---

## 🛠 技術細節與深度分析 (Technical Deep Dive)
### 1. One-Pass Hash Table (單次遍歷)
你實作的是「邊存邊找」的 One-Pass 方案，這比先將所有數據放入 Map 再找的 Two-Pass 方案更優：
- **優點**: 減少了一次完整的遍歷。
- **處理重複**: 它能自然處理重複數字（例如 `target=6`, `nums=[3,3]`）。當遇到第二個 `3` 時，第一個 `3` 已經在 Map 裡了，程式會立即回傳。
### 2. Java HashMap 實作考量
在面試中，提到以下幾點會展現你對 **Java 內部機制** 的掌握：
- **Initial Capacity**: 如果已知 `nums.length`，建立 Map 時可以指定初始容量 `new HashMap<>(nums.length / 0.75 + 1)`，以避免在執行過程中觸發多次 **Rehash (擴容)**，這在處理大數據時對效能很有幫助。
- **Autoboxing**: 注意 `int` 到 `Integer` 的自動裝箱（Autoboxing）開銷。在極致效能場景（如 TenMax 的廣告引擎），可能會改用原始型別集合類庫（如 fastutil 或 Trove）。
### 3. 防禦性編程 (Defensive Programming)
你加入的 `Boundary Check` 是專業表現：
- 判斷 `nums == null` 與 `nums.length < 2` 展現了對生產環境代碼穩健性的重視。
## 📖 Code
```java
public class Solution {  
    public int[] twoSum(int[] nums, int target) {  
        // [Key Point] Boundary Check  
        if (nums == null || nums.length < 2) {  
            return new int[0];  
        }  
  
        // [Key Point] Space-Time Tradeoff  
        Map<Integer, Integer> numMap = new HashMap<>();  
  
        for (int i = 0; i < nums.length; i++) {  
            int complement = target - nums[i];  
  
            // [Key Point] O(1) Average Lookup  
            if (numMap.containsKey(complement)) {  
                return new int[]{numMap.get(complement), i};  
            }  
            numMap.put(nums[i], i);  
        }  
  
        return new int[0];  
    }  
}
```