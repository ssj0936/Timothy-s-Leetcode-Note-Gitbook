# 141. Linked List Cycle

---

## 題目URL

[https://leetcode.com/problems/linked-list-cycle/description/](https://leetcode.com/problems/linked-list-cycle/description/)



## 題目敘述

Given a linked list, determine if it has a cycle in it.

Follow up:  
Can you solve it without using extra space?



判斷給的linklist是否存在cycle，另外有加分條件：不用額外的空間解題



## 想法

第二個做的EASY等級題目，可是卻莫名其妙的卡了很久

一開始的想法很簡單：就是逐個node以之當作head然後一直loop到底，如果最後是以null結尾那就代表是一本道沒有cycle；反之如果一直陷在無線迴圈中就代表有cycle

...

有了這個念頭之後簡直想打自己三巴掌，竟然想要判定有沒有陷在無線迴圈裏面...



最後的實現方法為：

> 然後從頭loop每一個node，檢查若該node有存在一個自訂的property，代表已經經過了所以有cycle return true；反之，就將這個node新增那個自訂的property

這樣解的時間複雜度為O\(n\)，空間複雜度也是O\(n\)



## 初版解法

> ```js
> /**
>  * Definition for singly-linked list.
>  * function ListNode(val) {
>  *     this.val = val;
>  *     this.next = null;
>  * }
>  */
>
> /**
>  * @param {ListNode} head
>  * @return {boolean}
>  */
>
>
> function ListNode(val) {
>     this.val = val;
>     this.next = null;
> }
>
>
> //solve 1
> var hasCycle1 = function (head) {
>     while (head != null && head.val != null) {
>         if('isPass' in head) return true;
>
>         head['isPass'] = true;
>         head = head.next;
>     }
>     return false;
> };
>
> ```

這樣做有達到標準但沒加到分，我們用了額外的空間。

升天不夠，更要絕頂升天

但老實說...真心想不到要怎麼~~絕頂升天~~不用額外的空間，最後只好google到了這個奇技淫巧

孤陋寡聞，一看到這個解法真的驚為天人，成功判斷且也沒用到額外空間，讓我絕頂升天，我說腦內啡



## 第二版解法

```js
var hasCycle = function (head) {
    if (head == null) return false;

    //fastPivot one time move forward 2 elements
    //slowPivot one time move forward 1 elements
    //2 pivot will meet each other when this link lit has a cycle in it
    if (head.next == null) return false;
    if (head.next.next == null) return false;

    var fastPivot = head.next.next,
        slowPivot = head.next;

    if (fastPivot == null || slowPivot == null) return false;

    while (fastPivot != slowPivot) {
        if (fastPivot.next != null && fastPivot.next.next != null && slowPivot.next != null) {
            fastPivot = fastPivot.next.next;
            slowPivot = slowPivot.next;
        } else {
            return false;
        }
    }
    return true;
};
```

簡單的說：

> 如果兩個程度不同的跑者，在同一個環形跑道上無限的奔跑，那除了在起跑點之外，一定會在環形跑道的某處相遇第二次；相反的，如果是在筆直且有盡頭的跑道上跑，那兩人只有在起跑點有相遇

所以用兩個pointer，fastPivot一次走兩個node，slowPivot一次走一個node，用這個規則一直跑下去\(while\)，如果出現了fastPivot == slowPivot的狀況，代表有cycle return true；反之就return false



## 結果

Status:Accepted

Runtime: 76ms

Your runtime beats 100.00 % of javascript submissions.

快慢指標解法，不知道是不是錯覺，感覺是生涯中第一次看到，當然也有可能是上演算法時我都在吃早餐的關係...

