## ⏳ 複雜度分析

### 1. Time Complexity (時間複雜度)

原稿提到退化成 LinkedList 是正確的，但遺漏了 JDK 8 引進的 **Treeify (紅黑樹化)** 機制。

- **Average Case: $O(1)$**
    
    在 Hash 函數均勻分布的情況下，查找、插入、刪除均為常數時間。
    
- **Worst Case (Hash Collisions):**
    
    - **JDK 7 以前：$O(N)$**。所有元素在同一個 Bucket，退化為單向鏈結串列。
        
    - **JDK 8 以後：$O(\log N)$**。當單一 Bucket 的節點數超過閾值（TREEIFY_THRESHOLD = 8）且總陣列長度大於 64 時，鏈結串列會轉換為**紅黑樹 (Red-Black Tree)**。
        
- **Amortized (攤銷):** 考慮到陣列擴容（Resize）時需要 Rehash，單次插入在極少數情況下會達到 $O(N)$，但平均攤銷後仍為 $O(1)$。
    

---

### 2. Space Complexity (空間複雜度)

原稿的 $O(K + M)$ 描述正確，但在面試中建議區分「靜態預配」與「動態擴張」：

- **Total Space: $O(K + M)$**
    
    - $K$: 桶數 (Capacity)。Java 預設為 16，且始終保持為 $2^n$。
        
    - $M$: 元素數量 (Size)。
        
- **Load Factor (負載因子):** 需提到 `0.75`。這決定了空間利用率與碰撞機率的權衡。空間複雜度不僅是 $O(M)$，還包含為了減少碰撞而預留的閒置空間。
    
- **Auxiliary Space: $O(1)$**
    
    這部分沒問題，操作過程中的指標更換是常數級別。
    

---

## 💡 Critical Thinking
1. **Hash 攻擊風險**：若外部輸入的 Key 是經過設計的（故意產生相同 HashCode），$O(N)$ 的退化會導致 CPU 飆升，引發 DoS 攻擊。這也是為什麼 JDK 8 要引入紅黑樹（將 Worst Case 壓制在 $O(\log N)$）。
    
2. **記憶體佈局 (Memory Footprint)**：`HashMap.Node` 物件本身有開銷。在極端高併發下，頻繁的 Resize 會造成嚴重的 GC 壓力。
    
3. **Thread-Safety**：在複雜度分析後，順帶提一句 HashMap 非線程安全，在併發環境下（如支付系統）需考慮 `ConcurrentHashMap`，其空間複雜度會涉及更複雜的 `Segment` 或 `CounterCell`。

# HashMap 核心機制：動態擴容 (Resize)

### 🚀 為什麼需要 Resize？

當 `HashMap` 中的元素過多時，**Hash 碰撞（Collision）** 的機率會大幅增加。若不擴容，所有元素會擠在少數幾個桶子（Buckets）中，導致查詢時間從理想的 $O(1)$ **退化** 為鏈表的 $O(n)$。

### ⚙️ 觸發條件：負載因子 (Load Factor)

- **預設容量 (Initial Capacity)**：`16`
    
- **負載因子 (Load Factor)**：`0.75`
    
- **觸發公式**：`count / capacity >= 0.75`
    
    - _註：0.75 是空間利用率與查詢時間的統計學黃金比例。_
        

---

### 🛠️ 實作程式碼 (MyHashMap2)

這份實作包含了完整的 `put`、`size`、`getCapacity` 以及核心的 `resize` 邏輯。

```java
package com.coding.gym.leetcode.common;

/**
 * 具備動態擴容能力的 HashMap 實作
 * 重點：Rehash (重新計算索引) 以確保資料分佈正確
 */
public class MyHashMap2 {
    class Node {
        int key, value;
        Node next;
        Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    private int capacity = 16; 
    private int count = 0;
    private final double LOAD_FACTOR = 0.75;
    private Node[] buckets = new Node[capacity];

    public int size() { return this.count; }
    public int getCapacity() { return this.capacity; }

    private int getHash(int key, int currentCapacity) {
        // 確保為正數並取模
        return (key & 0x7fffffff) % currentCapacity;
    }

    public void put(int key, int value) {
        // 1. 檢查是否達到擴容閾值
        if ((double) count / capacity >= LOAD_FACTOR) {
            resize();
        }

        int index = getHash(key, capacity);
        Node node = buckets[index];
        while (node != null) {
            if (node.key == key) {
                node.value = value; // Key 已存在，更新 Value
                return;
            }
            node = node.next;
        }

        // 2. 插入新節點 (使用頭插法)
        Node newNode = new Node(key, value);
        newNode.next = buckets[index];
        buckets[index] = newNode;
        count++;
    }

    /**
     * 核心搬家邏輯：容量翻倍並重新雜湊 (Rehash)
     */
    private void resize() {
    int oldCapacity = capacity;
    capacity = oldCapacity * 2; 
    Node[] newBuckets = new Node[capacity];

    for (int i = 0; i < oldCapacity; i++) {
        Node node = buckets[i];
        
        // 增加這兩個指標，用來在新桶子裡「排隊」
        Node head = null; 
        Node tail = null;

        while (node != null) {
            Node nextTemp = node.next; // 暫存下一個，避免斷鏈
        
            int newIndex = getHash(node.key, capacity);
            // --- 尾插法核心邏輯 ---
            if (tail == null) {
                head = node;     // 第一個來的，當隊頭
            } else {
                tail.next = node; // 後來的，接在目前隊尾的後面
            }
            tail = node;         // 更新隊尾指標，指向剛加入的這個人

            node = nextTemp;
        }

        // 當一個舊桶子全部處理完，把整串排好的隊伍掛上新桶子
        if (head != null) {
            tail.next = null;  
            newBuckets[getHash(head.key, capacity)] = head; 
        }
    }
    this.buckets = newBuckets;
}

    public int get(int key) {
        int index = getHash(key, capacity);
        Node node = buckets[index];
        while (node != null) {
            if (node.key == key) return node.value;
            node = node.next;
        }
        return -1;
    }
}
```

### 1. Java 7 的「頭插法」（逆序搬家）

它的邏輯是：**拿到誰，就把誰塞到新桶子的最前面。**

```Java
// 假設原本桶子裡是 A -> B -> C
while (node != null) {
    Node nextTemp = node.next; // 先記住下一個是 B
    
    // 把 A 塞到新桶子的頭部
    node.next = newBuckets[index]; // A.next 指向 null (目前新桶子是空的)
    newBuckets[index] = node;      // 新桶子的頭變成 A
    
    node = nextTemp; // 換處理 B... 
    // 下一輪 B 進來，B.next 會指向 A，新桶子頭變成 B。順序就變成 B -> A
}
```

**結果：** 搬完後順序變成 **`C -> B -> A`**。

**死循環原因：** 如果執行緒 1 搬到一半（A 指向 B），執行緒 2 進來搬完了（變成 B 指向 A），執行緒 1 回頭繼續搬 A 時，就會發現 A 指向 B、B 又指向 A，**「環」**就產生了。
細節:[[Hashmap dead detail]]



### 2. Java 8 的「尾插法」（順序搬家）

Java 8 引入了兩個指標：`head`（頭）和 `tail`（尾），確保順序不變。

```Java
Node head = null, tail = null;

while (node != null) {
    // 保持 A -> B -> C 的順序，永遠接在尾巴後面
    if (head == null) {
        head = node; // 第一個進來的 A 既是頭也是尾
    } else {
        tail.next = node; // 第二個進來的 B，接在 A 的屁股後面
    }
    tail = node; // 更新尾巴位置到最新的節點
    
    node = node.next;
}
// 最後把整條 A -> B -> C 一次掛上新桶子
newBuckets[index] = head;
```

**結果：** 搬完後順序依然是 **`A -> B -> C`**。
**優點：** 因為順序沒變，就算多個執行緒同時操作，也不會出現「你指向我、我指向你」的打結狀況。

---

### 3. Java 8 的「位元分流」 (Bitwise Partition)
這是一個更神的操作。Java 8 不再重新做 `%` 運算，而是看 Key 的 Hash 值在「新增加的那一位元」是 0 還是 1。

```Java
// 假設舊容量是 16 (二進位 10000)
// 我們直接用 key & 16 (00010000) 來判斷
if ((e.hash & oldCap) == 0) {
    // 這個 Key 運氣好，剛好對應新位元是 0
    // 直接留在【原位置】(例如 Index 3)
} else {
    // 這個 Key 的新位元是 1
    // 搬到【原位置 + 舊容量】(例如 Index 3 + 16 = 19)
}
```

### 為什麼這很厲害？
1. **快**：位元運算 `&` 比取模 `%` 快非常多。
2. **均勻**：因為 Hash 值是隨機的，所以大約會有一半的資料留在原位，一半搬走，完美地把擁擠的鏈表「拆成兩半」。
---
### 總結給 Obsidian 的話：

- **Java 7 (頭插)**：像是在疊盤子，晚拿到的疊在最上面 $\rightarrow$ **順序反轉** $\rightarrow$ **易死鎖**。
- **Java 8 (尾插)**：像是在排隊，先到的排前面 $\rightarrow$ **順序不變** $\rightarrow$ **安全且分佈均勻**。
## 📝 關聯題目
- [[q706_design_hashmap]]
- 