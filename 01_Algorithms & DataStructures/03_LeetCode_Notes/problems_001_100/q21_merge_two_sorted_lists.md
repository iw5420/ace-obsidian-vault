## ⏳ Complexity Analysis / 複雜度分析

### Time Complexity (時間複雜度): $O(N + M)$

- **定義**: 設 $N$ 為 `list1` 的長度，$M$ 為 `list2` 的長度。
- **分析**:
    - 演算法使用 `while` 迴圈同時遍歷兩個鏈表，每次比較後僅移動其中一個指標與 `tail` 指標。
    - 在最壞情況下（兩個鏈表的數值交錯），我們需要走完 $N+M$ 個節點。
    - 最後剩餘的鏈表掛載操作 `tail.next = ...` 為 $O(1)$。
    - **總計**: 線性時間複雜度 $O(N + M)$。
### Space Complexity (空間複雜度): $O(1)$
- **分析**:
    - **Auxiliary Space (輔助空間)**: 僅使用了 `dummy` 節點與 `tail` 指標，這些空間不隨輸入資料量增長。
    - **In-place Operation (原地操作)**: 該實作並沒有建立新的節點（除了 Dummy Node），而是透過修改現有節點的 `next` 指標來重新串接鏈表。
    - **注意**: 如果面試官問及遞迴解法，則空間複雜度會因遞迴棧（Recursion Stack）而退化至 $O(N + M)$。
---
## 🛠 技術細節與邊界分析 (Technical Deep Dive)
### 1. Dummy Head 技巧
使用 `ListNode dummy = new ListNode(0)` 是處理鏈表問題的標準最佳實踐：
- **優點**: 避免了判斷「合併後的第一個節點是誰」的邏輯，統一了插入流程，最後只需返回 `dummy.next`。
- **面試點**: 這展現了你對代碼整潔度（Clean Code）的追求，減少了條件分支（If-else）的複雜度。
### 2. 穩定性 (Stability)
- 程式碼中判斷條件為 `list1.val <= list2.val`：
    - 這確保了當兩個節點數值相等時，優先保留 `list1` 的節點順序。
    - 這使該合併算法具有**穩定性 (Stability)**，這在排序演算法（如 Merge Sort）中至關重要。
### 3. 指標掛載 (Final Attachment)
- `tail.next = list1 == null ? list2 : list1;`
    - 這行程式碼優雅地處理了長度不等的情況。由於輸入是有序鏈表，一旦其中一條耗盡，直接串接另一條的剩餘部分即可，不需要繼續迴圈。
## 📖 Code
```java
public class Solution {  
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {  
        ListNode dummy = new ListNode(0);  
        ListNode tail = dummy;  
        while(list1!=null &&list2!=null){  
            if(list1.val<=list2.val){  
                tail.next = list1;  
                list1 = list1.next;  
            }else{  
                tail.next = list2;  
                list2 = list2.next;  
            }  
            tail = tail.next;  
        }  
        tail.next = list1==null?list2:list1;  
        return dummy.next;  
    }  
}
```