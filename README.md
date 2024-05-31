
contributed by `<YeeeLiang>`

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

:::info
問題二：Merge Sort 其中之一的 Divide and Conquer 的程式碼，為什麼是`listsSize - m`，而不是`listsSize - m/2`呢?
:::

在[你所不知道的 C 語言: linked list 和非連續記憶體](https://hackmd.io/@sysprog/c-linked-list)文中，Divide and Conquer 的這段程式碼看不太懂：

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

:::info
問題三：第 5 次作業上傳問題。
:::

填寫 Google 表單上傳第 5 次作業時，遭遇困難。

已排除問題：

1. 表單每題問題已通過審查
2. 截止時間前上傳

上傳多次，但仍找不出原因為何？
上傳紀錄如
[<第5次作業上傳紀錄>](https://drive.google.com/file/d/1Jskc-GNp3Na6CECIGrZdr0pgXXBGc7nm/view?usp=sharing)。

5/31今天早上以相同的內容再次填寫表單，由手機登入頁面查看有上傳成功。但電腦重新整理或關掉重開頁面，都沒有上傳成功。因此不確定是否有成功上傳作業？

* [電腦](https://drive.google.com/file/d/1geXinlrNATyOg3eP-FQSRZXA0A_7DDZt/view?usp=sharing)重新整理又重開頁面後，且時間軸晚於手機。

![螢幕擷取畫面 2024-05-31 084757](https://hackmd.io/_uploads/r1B9058NC.jpg)


* 手機畫面

![IMG_5778](https://hackmd.io/_uploads/ByqRJoIVA.jpg)





