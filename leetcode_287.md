# 287.Find the Duplicate Number

---

## 題目URL

[https://leetcode.com/problems/find-the-duplicate-number/description/](https://leetcode.com/problems/find-the-duplicate-number/description/)



## 題目敘述

Given an array nums containing n+ 1 integers where each integer is between 1 and n\(inclusive\), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

**Note:**  


1. You **must not **modify the array \(assume the array is read only\).
2. You must use only constant,O\(1\) extra space.
3. Your runtime complexity should be less than`O(n^2)`.
4. There is only one duplicate number in the array, but it could be repeated more than once. 

給一個整數陣列，內含n+1個整數，其範圍落在1與n之間，其中必定會有一個數字出現兩次以上，請找出該數字

注意：

1. 不可以修改陣列內容
2. 空間複雜度為常數
3. 時間複雜度為O\(n^2\)
4. 只有一個數字出現大於一次



## 想法

"不可以修改陣列內容"，OK所以不能sort，如果可以sort的話就好解很多了

"空間複雜度為常數"，OK所以也不能複製一份自己sort，真是個小壞壞完全知道我這豬腦在想什麼

最單純的想法：

> 兩層loop，然後比較兩個pointer指到的東西，只要發現指到的不是同一個東西但值一樣，那就return該值

## 解法

> ```js
> //brutal force
> var findDuplicate = function(nums) {
>     for(var [i,v] of nums.entries()){
>         for(var j = nums.length-1;j>i;--j){
>             if(v == nums[j]) return v
>         }
>     }
>     return null;
> };
> ```

送出後意外的accept了，我以為會有執行時間超過之類的error跑出來，話雖如此，看了一下排行榜

...

Your runtime beats 19.76 % of javascript submissions.

幹你媽的我不能接受！



## 第二版的解法

```js
var findDuplicate2 = function(nums) {
    if(nums.length == 2) return 1;

    var head = 0;
    var fastPivot = nums[nums[head]];
    var slowPivot = nums[head];

    while(fastPivot != slowPivot){
        fastPivot = nums[nums[fastPivot]];
        slowPivot = nums[slowPivot];
    }

    slowPivot = head;
    while(fastPivot != slowPivot){
        fastPivot = nums[fastPivot];
        slowPivot = nums[slowPivot];
    }
    return fastPivot;
};
```

其實要說一下，我解的上一題是142. linklist cycle II，要是沒有這個提示我還真想不出來：

> 簡單的說就是把它當link list，把array中的值當作是linklist的next\(的index\)
>
> 例如：\[1,3,5,6,7,4,3,3\]
>
> =&gt;head 是nums\[0\]，其值為1，所以next是nums\[1\]
>
> 接下來linklist第二位，其值為nums\[1\] = 3,所以next是nums\[3\]
>
> 所以當loop linklist的途中，經過重複的元素兩次時會形成cycle。而cycle起點的index，則為陣列中重複的值。

可以看到code的部分，根本跟142. linklist cycle II沒什麼兩樣

## 結果

Status:Accepted

Runtime: 72ms

Your runtime beats 100.00 % of javascript submissions.

沒有上下題的提示，真的想不出來，腦袋還是太僵了...

