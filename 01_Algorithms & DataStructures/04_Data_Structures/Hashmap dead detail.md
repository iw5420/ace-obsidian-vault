### 假設場景

- **初始狀態**：在舊陣列的 Index 1 處，有一條鏈表 `A -> B`（A 的 `next` 指向 B，B 的 `next` 指向 `null`）。
- **執行緒 1 (T1)**：剛準備搬 A，記住了 `nextTemp = B`，然後就被系統暫停了。
### 執行緒 2 (T2) 介入：全速搬家
這時候 T2 啟動了，它也要把 Index 1 的東西搬到新陣列（假設新位置也是 Index 1）。
#### 步驟 T2-1：搬移 A
1. 取出 A。
2. 令 `A.next = newBuckets[1]`（目前是 `null`）。
3. `newBuckets[1] = A`。
- **記憶體現況**：`A.next` 為 `null`。

#### 步驟 T2-2：搬移 B（關鍵點！）
1. 取出 B。
2. 令 `B.next = newBuckets[1]`（**此時新桶子的頭是 A！**）。
3. 所以，**`B.next` 指向了 A**。
4. `newBuckets[1] = B`。
- **記憶體現況**：`B.next` 指向了 `A`，而 `A.next` 依然是 `null`。
**此時，T2 的任務完成了。它把鏈表變成了 `B -> A`。**

### 災難發生：執行緒 1 (T1) 醒過來了
T1 恢復執行，它手裡還握著剛才暫停前的變數：`node = A`, `nextTemp = B`

#### 步驟 T1-1：完成 A 的搬移
1. 它執行 `A.next = newBuckets[1]`。注意！此時 T2 已經把 `newBuckets[1]` 變成 **B** 了。
2. 所以，**`A.next` 指向了 B**。
3. `newBuckets[1] = A`。
- **現在的慘劇**：記憶體裡 `B.next` 指向 `A`（T2 做的），而 `A.next` 又指向 `B`（T1 剛做的）。
#### 步驟 T1-2：繼續處理 B
1. T1 執行 `node = nextTemp`（也就是 B）。
2. 進入下一輪迴圈，執行 `nextTemp = node.next`（也就是 `B.next`）。
3. 因為 `B.next` 指向 `A`，所以 `nextTemp` 又變回了 **A**。
4. **程式就這樣陷入了 A 找 B、B 找 A 的無限循環。**