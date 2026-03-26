### ⏳ Complexity Analysis / 複雜度分析

#### Time Complexity (時間複雜度)

設 $N$ 為插入的元素總數，$K$ 為 Bucket 的數量（此處 `SIZE` = 1000）。

> **Note**: 在 **Worst Case** 下（所有 Key 的 Hash 全都碰撞），HashMap 會退化成長度為 $N$ 的 **LinkedList**。

#### Space Complexity (空間複雜度)

- **Total Space: O(K + M)**
    
    - **O(K)**: 預先分配的 Bucket 陣列空間（此處為 1000）。
        
    - **O(M)**: 實際儲存的節點數量（Unique Keys）。
        
- **Auxiliary Space (per op): O(1)**
    
    - 每次操作（put/get/remove）僅需固定數量的輔助指標，不隨資料量增長。

## 📖 Related Knowledge / 相關知識主題

- [[Topic_Name]]
## 📖 Code
```java
public class Solution {  
    // LeetCode 要求的類別名稱  
    class MyHashMap {  
        public class Node {  
            int key, value;  
            Node next;  
            Node(int key, int value) {  
                this.key = key;  
                this.value = value;  
            }  
        }  
  
        private final int SIZE = 1000;  
  
        private final Node[]buckets = new Node[SIZE];  
  
        private int getHash(int key){  
            return (key & 0x7fffffff) % SIZE;  
        }  
  
        public void put(int key, int value) {  
            // 1標記位置  
            int index = getHash(key);  
            Node node = buckets[index];  
            // 2檢查  
            while(node!=null){  
                if (node.key == key) {  
                    node.value = value;  
                    return;  
                }  
                node = node.next;  
            }  
            // 3新增  
            Node newNode = new Node(key, value);  
            newNode.next = buckets[index];  
            buckets[index]=newNode;  
        }  
  
        public int get(int key) {  
            int index = getHash(key);  
            Node node = buckets[index];  
            // 2檢查  
            while(node!=null){  
                if (node.key == key) {  
                    return node.value;  
                }  
                node = node.next;  
            }  
            return -1;  
        }  
  
        public void remove(int key) {  
            // 1初始  
            int index = getHash(key);  
            Node current = buckets[index];  
            Node prevNode = null;  
  
            // 2迴圈 執行邏輯  
            while(current!=null){  
                if (current.key == key) {  
                    //如果是第一個  
                    if(prevNode == null) {  
                        buckets[index] = current.next;  
                    }else{  
                        prevNode.next = current.next;  
                    }  
                    return; //如果已經找到就不用繼續跑迴圈  
                }  
                prevNode = current;  
                current = current.next;  
            }  
        }  
  
        public void remove2(int key){  
            int index = getHash(key);  
            if(buckets[index]==null)return;  
            Node dummy = new Node(-1, -1);  
            dummy.next = buckets[index];  
            Node prev = dummy;  
            while(prev.next!=null){  
                if(prev.next.key==key){  
                    prev.next = prev.next.next;  
                    buckets[index] = dummy.next;  
                    return;  
                }  
                prev = prev.next;  
            }  
        }  
    }  
}
```