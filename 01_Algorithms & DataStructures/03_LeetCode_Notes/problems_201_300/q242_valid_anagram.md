### ⏳ Complexity Analysis / 複雜度分析

- **Time Complexity (時間複雜度): $O(n)$**
    - **定義**: 設 $n$ 為字串 `s` 的長度，$m$ 為字串 `t` 的長度。
    - **分析**:
        1. **長度檢查**: $O(1)$。
        2. **單次遍歷**: 演算法使用一個 `for` 迴圈同時遍歷 `s` 與 `t`（前提是長度相等，$n = m$）。在迴圈中，`charAt` 與陣列索引存取皆為 $O(1)$ 操作。
        3. **結果驗證**: 最後遍歷長度固定的計數陣列（26 位元），耗時為 $O(26)$，即 $O(1)$。
    - **總計**: 整體執行時間與字串長度成線性關係，為 $O(n)$。
        
- **Space Complexity (空間複雜度): $O(1)$**
    - **分析**:
        1. **輔助空間 (Auxiliary Space)**: 僅建立了一個長度為 26 的整數陣列 `int[26]`。
        2. **常數特性**: 無論輸入字串的長度是 10 還是 1,000,000，該陣列的大小始終固定為 26（對應 a-z）。
    - **注意**: 如果題目要求支援 **Unicode**（如中文字元或 Emoji），則空間複雜度會取決於字元集的大小（如使用 `HashMap`），但在本題限定小寫字母的情境下，空間被視為常數級別。
---
### 🛠 技術細節與邊界分析 (Technical Deep Dive)
#### 1. 固定長度陣列優化 (Array vs. HashMap)
在處理有限字元集（如 a-z）時，使用 `int[26]` 優於 `HashMap<Character, Integer>`：
- **效能**: 陣列在記憶體中是連續分配的，具有更好的 **CPU Cache Locality**，且省去了 Hash Function 計算與處理 Collision 的開銷。
- **面試點**: 這展現了你對**資料規模**的敏感度。能根據題目給定的 Constraints（小寫英文字母）選擇最輕量化的資料結構，是資深工程師的表現。
#### 2. 字元偏移量技巧 (Character Offsetting)

程式碼中使用 `s.charAt(i) - 'a'` 來計算索引：
- **原理**: 利用 Java 中 `char` 類型可自動提升為 `int` 的特性，將字母對應到 0-25 的整數空間。
- **優點**: 程式碼簡潔且直觀，避免了繁瑣的 `switch-case` 或多重 `if` 判斷。
#### 3. 早期失敗機制 (Early Failure)
在進入核心邏輯前，優先執行 `if (s.length() != t.length()) return false;`：
- **策略**: 這是一種 **Guard Clause** 模式，能有效過濾無效輸入，確保後續迴圈邏輯在對等的前提下執行，並將最壞情況的時間複雜度降為 $O(1)$（當長度不符時）。
#### 4. Unicode 擴充性 (Scalability)
如果面試官追問：「若輸入改為全球語系字元該如何修改？」
- **解法**: 應改用 `HashMap` 儲存頻率。
- **代價**: 空間複雜度將從 $O(1)$ 提升至 $O(K)$（$K$ 為不同字元的數量）。
## 📖 Code
```java
public boolean isAnagram(String s, String t) {  
    if(s.length()!=t.length())return false;
      
    int[]count = new int[26];  
    
    for(int i = 0; i <s.length(); i++){  
        count[s.charAt(i)-'a']++;  
        count[t.charAt(i)-'a']--;  
    }  
    
    for(int i : count){  
        if(i!=0)return false;  
    }  
    
return true;  

}
```