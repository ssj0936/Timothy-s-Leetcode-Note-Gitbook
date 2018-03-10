# 142. Linked List Cycle II

---

## 題目URL

[https://leetcode.com/problems/linked-list-cycle-ii/description/](https://leetcode.com/problems/linked-list-cycle-ii/description/)

## 題目敘述

Given a linked list, return the node where the cycle begins. If there is no cycle, return`null`.

**Note:**Do not modify the linked list.

**Follow up**:  
Can you solve it without using extra space?

判斷給的linklist是否存在cycle，若有的話return開始cycle的那個node；反之return null。

此外注意，不可以對linklist做修改

一樣有加分題：不用額外的空間解題

## 想法

不給活路...規定不可以modify linklist，所以上次加property那招不能用了。最後想到的初步想法是：

> 建立一個set，然後從head開始loop每一個node並用set.has\(\)來檢查這個node是否存在set之中，如果有那就return這個node；反之就把這個node add到set之中。\(set的相同元素判斷機制是===，所以這邊用set判定是OK的\)

## 初版解法

> ```js
> var detectCycle = function(head) {
>     var set = new Set();
>     while (head != null && head.val != null) {
>         if(set.has(head)) return head;
>         set.add(head);
>         head = head.next;
>     }
>     return null;
> };
> ```

這樣做法完全沒有問題也被leetcode accept了，但一樣用了額外的空間。想了一天實在沒有頭緒，最後還是不爭氣地去google了。

沒想到又是快慢指標解法，不過還要加上少許數學

## 第二版解法

```js
if (head == null) return null;

    //fastPivot one time move forward 2 elements
    //slowPivot one time move forward 1 elements
    //2 pivot will meet each other when this link lit has a cycle in it
    if (head.next == null) return null;
    if (head.next.next == null) return null;

    var fastPivot = head.next.next,
        slowPivot = head.next;

    if (fastPivot == null || slowPivot == null) return null;

    while (fastPivot != slowPivot) {
        if (fastPivot.next != null && fastPivot.next.next != null && slowPivot.next != null) {
            fastPivot = fastPivot.next.next;
            slowPivot = slowPivot.next;
        } else {
            return null;
        }
    }

    //move one of pivot to head
    //and 2 pivot move tha same distance, that is 1.
    slowPivot = head;
    while(slowPivot != fastPivot){
        slowPivot = slowPivot.next;
        fastPivot = fastPivot.next;
    }
    return slowPivot;
```

這解法不說原理根本不懂：



假設linklist中，非cycle部分的長度為x，cycle部分長度為y，而cycle起點到兩pivot相遇的位置其距離為k，並兩個pivot跑了t回合之後相遇，所以：

slowPivot走過的長度為：$$x+my+k$$

fastPivot走過的長度為$$x+ny+k$$

\(每個pivot都是先經過非cycle的部分+幾圈的cycle+剩下一點點的路程\)  
slowPivot一次走兩格，slowPivot一次走一格，依照行走距離可得：

$$2x+2my+2k = x+ny+k$$

$$→ x+k = (n-2m)y$$



挖O咧，從結論看起來：

從兩Pivot相遇的位置\(k\)，再往前走x的距離，就會回到cycle的起點

工作這幾年根本沒用過數學，這解法看起來根本奇技淫巧。

## 結果

Status:Accepted

Runtime: 76ms

Your runtime beats 100.00 % of javascript submissions.

