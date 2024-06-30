# 2024q1 Homework5 (assessment)
contributed by < `YeeeLiang` >

## [<因為自動飲料機而延畢的那一年>](https://blog.opasschang.com/the-story-of-auto-beverage-machine-1/) 讀後感想
 
愛因斯坦曾說，「如果給我一小時解決問題，而且那是足以改變人生的重大問題的話，我會花五十五分鐘去確認我是否問了正確的問題。」

**要先問對問題，才能解決問題**

「設定問題」在建立達成目標所需之手段的過程中，位於最上游。如果這裡就找錯問題，後續的解決方案和方案執行，也會環環相扣地錯下去。於是付出的勞力無法換來成果，變成白忙一場。

**「問題」就是「落差」**
    
「現狀」和「目標」的落差。目的地與現在地之間的差距，就是所謂的「問題」。將目前狀態和理想狀態加以「比較」的時候，問題才會顯現出來。

**我們在朝著什麼目標邁進？**

經驗豐富的管理顧問對經營者進行訪談時，會接二連三地拋出許多問題，像是「為何要將營業額設定成經營指標？」「為何光一個經營企劃部門就要有這麼多員工？」「在這個時間點進行業務員強化的企圖是什麼？」「你們如何向開發部門反饋消費者的意見？」

到底是哪來這麼多問題可以問的？因為管理顧問腦中已事先建構出企業的理想形象，並以該形象為基準，與客戶企業的現狀加以比較。透過比較發現落差，進而得到「認知」。問題不是憑空出現，而是先有比較對象當作基準，透過比較才能找出來的。

因此我認為，解決或問對的問題前需要先累積一定的經驗與知識。

## 問題討論

:::info
問題一：無法意會 Merge Sort 的頭跟尾兩兩合併，想請老師再說明一次。
:::


資料參考：[你所不知道的 C 語言: linked list 和非連續記憶體](https://hackmd.io/@sysprog/c-linked-list)

#### **頭跟尾兩兩合併：**

從固定第一條串列改成頭跟尾兩兩合併，直到剩一條為止，比起前一方法的每次都用愈來愈長的串列跟另一條串列合併，頭尾合併在多數的情況下兩條串列的長度比較平均，合併會比較快。

當合併完頭尾後，偶數長度會少一半，奇數長度則為 `(listsSize + 1) / 2`，奇數更新的方式也可以用在偶數長度上。

TODO: 解釋 remove_list_node 的 indirect pointer 運作原理
> 搭配案例來說明

:::success
"Indirect pointer" 意味著使用指向指標的指標，這使得能夠修改指標所指向的內容，而不是僅僅修改指標本身。

`remove_list_node` 函式用於從鏈結串列中刪除節點。當我們使用 indirect pointer (間接指標) 的時候，實際上是通過指向指向節點的指標來進行操作。這樣做的好處是，我們可以直接存取到我們想要修改的指標，而不需要逐一走訪每個節點。尤其是當操作的節點數量非常大時，這樣做可以提高效率。

![間接指標](https://hackmd.io/_uploads/r1-9SWsNC.jpg)

如上圖，如果我們想要刪除節點 3，我們可以使用 indirect pointer。將 indirect pointer 指向指向節點 3 的指標。通過這種方式，直接存取並修改指向節點 3 的指標，從而刪除節點 3，而無需逐一走訪每個節點。因此，時間的複雜度為 O(1)。

總的來說，使用 indirect pointer 可以提高在鏈結串列中進行刪除操作的效率，特別是當我們需要頻繁地刪除節點時。

實作程式碼如下，原有三個節點存在，利用 indirect pointer 的方法將第二個節點刪除。
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node *next;
} Node;

typedef struct List {
    Node *head;
} List;

void remove_list_node(List *list, Node *target)
{
    Node **indirect = &list->head;
    while (*indirect != target)
        indirect = &(*indirect)->next;
    *indirect = target->next;
    free(target); 
}

void print_list(List *list) {
    Node *current = list->head;
    while (current != NULL) {
        printf("%d ", current->data);
        current = current->next;
    }
    printf("\n");
}

int main() {
    List list;
    list.head = (Node*)malloc(sizeof(Node));
    list.head->data = 1;
    list.head->next = (Node*)malloc(sizeof(Node));
    list.head->next->data = 2;
    list.head->next->next = (Node*)malloc(sizeof(Node));
    list.head->next->next->data = 3;
    list.head->next->next->next = NULL;

    printf("Original list: ");
    print_list(&list);
    
    remove_list_node(&list, list.head->next);
    
    printf("List after removal: ");
    print_list(&list);
    
    Node *current = list.head;
    while (current != NULL) {
        Node *temp = current;
        current = current->next;
        free(temp);
    }
    
    return 0;
}
```
這段程式碼在一開始定義了兩個結構，`Node` 表示鏈結串列中的節點，包含一個整數和指向下一個節點的指標；`List` 表示整個鏈結串列，包含一個指向鏈結串列 Head 的指標。

然後，用 `remove_list_node` 函式從鏈結串列中移除指定的節點。該函式接受兩個參數，一個是指向鏈結串列的指標，另一個是要被移除的節點的指標。在函式內部，我們使用了一個間接指標 `indirect`，這個指標指向將要更新的地方。然後，逐一走訪鏈結串列，直到找到指向要刪除節點的指標，然後更新這個指標，跳過該節點，並釋放其佔用的記憶體。

最後，使用 GDB 進行 debug 及運行程式碼。
```c
~$ ./a.out
```

可得到下面的結果：
```c
Original list: 1 2 3
List after removal: 1 3
```

:::

:::info
問題二：Merge Sort 其中之一的 Divide and Conquer 的程式碼，為什麼是`listsSize - m`，而不是 `listsSize - m/2` 呢?
:::

在[你所不知道的 C 語言: linked list 和非連續記憶體](https://hackmd.io/@sysprog/c-linked-list)文中，Divide and Conquer 的這段程式碼<s>看不太懂</s>：

:::danger
誠實面對自己
:::

```c
    int m = listsSize >> 1;
    struct ListNode *left = mergeKLists(lists, m);
    struct ListNode *right = mergeKLists(lists + m, listsSize - m);

    return mergeTwoLists(left, right);
```
Divide and Conquer 是計算出中間位置 ｎ，然後將 lists 陣列分為兩部分：lists[0] 到 lists[ｎ-1] 是左半部分，lists[ｎ] 到 lists[listsSize] 是右半部分。
在使用遞迴呼叫 mergeKLists 函式，對左右兩部分進行合併，分別得到左半部分和右半部分的合併結果。

但是為什麼上面的程式碼是`listsSize - m`，而不是`listsSize - m/2`呢?
如下：
```diff
    int m = listsSize >> 1;
    struct ListNode *left = mergeKLists(lists, m);
-   struct ListNode *right = mergeKLists(lists + m, listsSize - m);
+   struct ListNode *right = mergeKLists(lists + m, listsSize-(m/2));

    return mergeTwoLists(left, right);
```

TODO: 重作第一次作業，彙整其他學員的成果 (挑出優秀的學員成果、重現實驗、檢驗出處 [數學推導]，並修正錯誤)，著重在程式開發的技巧 (e.g., indirect pointer, git commit message 的撰寫, 排序演算法的調整, 排序的 [stability](https://en.wikipedia.org/wiki/Sorting_algorithm))

:::success
我選擇了 `25077667` 以及 `vax-r` 這兩位同學的 lab0 實作筆記作為參考，在[`25077667`同學的筆記](https://hackmd.io/@25077667/SkpLspWnT)加入了很多創新的想法並改寫了原本作業給的程式碼；而[`vax-r`同學的筆記](https://hackmd.io/@vax-r/linux2024-homework1)則是整理的相當清晰易懂。

[lab0 的作業](https://hackmd.io/@sysprog/linux2024-lab0/%2F%40sysprog%2Flinux2024-lab0-a)要求是實作 a chain of circular queues，這並非是單純一個 queue，而是被鏈結串列所串連在一起的多個環狀佇列。如圖：

![quece.c圖](https://hackmd.io/_uploads/B1en_pDrC.jpg)

### `q_new()`

使用 `calloc` 函式來分配記憶體，確保新的佇列 head 節點 `new_qhead` 被初始化為零，並分配足夠大小以容納 `struct list_head` 的資料。當分配失敗時（`new_qhead` 為 NULL），函式返回 NULL，無法創建新的佇列。假若一切順利，函式則返回指向新佇列 head 節點的指標。
```C
/* Create an empty queue */
struct list_head *q_new()
{
    struct list_head *new_qhead = calloc(1, sizeof(struct list_head));
    if (!new_qhead)
        return NULL;
    INIT_LIST_HEAD(new_qhead);
    return new_qhead;
}
```
進一步探討 `malloc` 與 `calloc` 的差異為何？`malloc` 函式（memory allocation）用於分配指定大小的記憶體區塊，並返回一個指向該記憶體區塊起始位置的指標。但`malloc`分配的記憶體中的內容是未初始化的，意旨它們的值可能是隨機的或者舊的內容。而`calloc` 函式（contiguous allocation）將分配的每一位元組初始化為零。因此可預知`malloc` 的效能比 `calloc` 快，因為 `calloc` 需要額外的時間來初始化記憶體，但內容相對的不安全。

### `q_insert_head()`

這個函式的目的是將一個新的 element 插入到佇列的 head。在這裡 使用了Linux核心的連結串列結構 `struct list_head *head`，將 head 指向佇列的 head。
```c
/* Insert an element at head of queue */
bool q_insert_head(struct list_head *head, char *s)
{
    if (!head)
        return false;
    element_t *new_ele = malloc(sizeof(element_t));
    if (!new_ele)
        return false;
    INIT_LIST_HEAD(&new_ele->list);
    new_ele->value = strdup(s);
    if (!new_ele->value) {
        free(new_ele);
        return false;
    }
    list_add_tail(&new_ele->list, head);
    return true;
}

```

### `q_size()`

計算佇列中 element 數量的函式。使用 `list_for_each` 逐一走訪佇列。每走完一次，就將 size 變數加一，以計算佇列中的element 數量。
```c
/* Return number of elements in queue */
int q_size(struct list_head *head)
{
    if (!head)
        return 0;

    int size = 0;
    struct list_head *cur;
    list_for_each (cur, head)
        size++;
    return size;
}

```

### `q_delete_mid()`

刪除佇列中中間的節點。使用 `struct list_head *head` 指向佇列的 head 節點的指標。使用兩個指標 `first` 和 `second` 來尋找中間節點。通過從佇列的兩端同時向中間移動，直到 `first` 和 `second` 相遇或者相鄰。
```c
/* Delete the middle node in queue */
bool q_delete_mid(struct list_head *head)
{
    // https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/
    if (!head || list_empty(head))
        return false;

    struct list_head *first = head->next;
    struct list_head *second = head->prev;
    while((first != second) && (first->next != second)) {
        first = first->next;
        second = second->prev;
    }
    element_t *node = list_entry(first, element_t, list);
    list_del(first);
    free(node->value);
    free(node);
    return true;
}

```
### `q_delete_dup()`

刪除佇列中所有具有重複字串的節點。以下程式碼主要運用了雙重循環來處理重複節點的刪除操作，並且在操作過程中釋放了相關的記憶體。
* 在循環中，首先檢查是否存在重複節點。條件 `&tmp->list != head && !strcmp(cur->value, tmp->value)` 確保 tmp 不是頭節點並且 `cur` 和 `tmp` 的值相同。
* 如果有重複，則刪除 `cur` 節點，並釋放其內存。設置 `dup = true;` 表示有重複節點被處理。
* 如果前一個節點已經有重複節點被刪除（即 `dup == true`），則直接刪除 `cur` 節點，並重置 `dup = false;`。
```c
/* Delete all nodes that have duplicate string */
bool q_delete_dup(struct list_head *head)
{
    if (!head || list_empty(head))
        return false;

    bool dup = false;
    element_t *cur, *tmp;
    list_for_each_entry_safe (cur, tmp, head, list) {
        if (&tmp->list != head && !strcmp(cur->value, tmp->value)) {
            list_del(&cur->list);
            free(cur->value);
            free(cur);
            dup = true;
        } else if (dup) {
            list_del(&cur->list);
            free(cur->value);
            free(cur);
            dup = false;
        }
    }
    return true;
}

```

### `q_swap`

透過雙向鏈節串列（double linked list）的資料結構來實現交換每兩個相鄰節點的功能。在每次迴圈中，`first` 和 `second` 分別指向相鄰的兩個節點。如果 `second` 指向的是 head，則跳出迴圈，避免處理最後一對可能是**奇數**個節點的情況。
```c
/* Swap every two adjacent nodes */
void q_swap(struct list_head *head)
{
    if (!head || list_empty(head))
        return;

    struct list_head *first, *second;
    list_for_each_safe (first, second, head) {
    if (second == head)
        break;
        first->prev->next = second;
        second->prev = first->prev;
        first->next = second->next;
        first->prev = second;
        second->next->prev = first;
        second->next = first;
    }
}

```

### `q_reverseK`

將鏈節串列每 k 個節點進行一次反轉。這裡使用了 `q_size` 函式來計算鏈節串列的節點總數，然後將其除以 k，得到需要進行反轉的次數 `times`，每次反轉都處理 k 個節點。

`tail` 是一個指向 `struct list_head` 的指標，用於在每個迭代中找到反轉的結束點。`tmp` 是一個新的空鏈節串列，用來暫時存放反轉後的 k 個節點。

`new_head` 是最終結果的新鏈節串列的頭。使用 `list_for_each` 來逐一走訪鏈節串列，直到找到第 k 個節點（即 `tail`），或者是逐一走訪完整個鏈節串列。確保 `tail` 指向每次反轉的結束位置。

接著使用 `list_cut_position` 將從 `head` 到 `tail->prev` 的節點切下來，並將它們放入 `tmp` 鏈節串列中。然後使用 `q_reverse` 函式來反轉 `tmp` 鏈節串列中的節點。

最後使用 `list_splice_tail_init` 將 `tmp` 鏈節串列中的節點接入 `new_head` 鏈節串列的尾部，並且初始化 `tmp` 鏈節串列以便下一次使用。
```c
/* Reverse the nodes of the list k at a time */
void q_reverseK(struct list_head *head, int k)
{
    if (!head)
        return;

    int times = q_size(head) / k;
    struct list_head *tail;

    LIST_HEAD(tmp);
    LIST_HEAD(new_head);

    for (int i=0; i < times; i++) {
        int j = 0;
        list_for_each(tail, head) {
            if (j >= k)
                break;
            j++;
        }
        list_cut_position(&tmp, head, tail->prev);
        q_reverse(&tmp);
        list_splice_tail_init(&tmp, &new_head);
    }
    list_splice_init(&new_head, head);
}

```




