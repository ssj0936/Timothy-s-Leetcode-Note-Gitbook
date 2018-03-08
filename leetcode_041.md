# 41. First Missing Positive

---

## 題目URL

[https://leetcode.com/problems/first-missing-positive/description/](https://leetcode.com/problems/first-missing-positive/description/)

## 題目敘述

Given an unsorted integer array, find the first missing positive integer.

> For example,  
> Given \[1,2,0\] return 3,  
> and \[3,4,-1,1\] return 2.

Your algorithm should run in O\(n\) time and uses constant space.

給一個為排序的整數陣列，找出其中缺失的最小正整數

**注意:**時間複雜度需要O\(n\)內，且只能用常數的空間複雜度

例如：

> \[1,2,0\] =&gt; 其中缺少的最小正整數為3
>
> \[3,4,-1,1\] =&gt;\[-1,1,3,4\] =&gt; 其中缺小的最小正整數是2

## 想法

除了幹你老師，別無他想，問題不難，難在他的附加條件

最直接的想法：

> 就是先sort，然後loop過一遍array把每個元素放set裡，接著用set.has\(\)，從1開始去檢查有沒有存在於set中

...

但這樣做sort一定不會低於O\(nlogn\)，空間複雜度也會變成O\(n\)

你要嘛是看不懂字不然就是藐視王法，完全無視附加條件，除了拍手我想不到什麼方法讚揚你的勇氣

接著想到可以用swap，所以有了第一版想法：

> 對nums push一個0\(避免原array中沒有0，方便作業\)，接著用swap的方式，把nums中負數挪到陣列左邊\(或是從array中移除\)\(O\(n\)\)，然後針對大於0的部分，用swap的方式，把0放到nums\[0\]，把1放到nums\[1\]...依此類推。歸類完之後，再loop一遍，第一個index與其值不同的index，則為result。\(譬如\[0,1,65,3,4,6,9...\]，其中nums\[2\] = 65 != 2則為第一個沒放對位置的元素，result為2\)
>
> 其中swap歸類的部分卡了很久，最後想到swap完pointer不一定要往前進，時間也可以控制在O\(n\)內，使用swap也符合空間複雜度為常數的部分

## 

## 第一版解法

> ```js
> /**
>  * @param {number[]} nums
>  * @return {number}
>  */
> /*
> Given an unsorted integer array, find the first missing positive integer.
> For example,
> Given [1,2,0] return 3,
> and [3,4,-1,1] return 2.
>
> Your algorithm should run in O(n) time and uses constant space.
> */
>
> /**
>  * 原本的構想：
>  * 對nums push一個0
>  * 先用swap的方式，把nums中負數挪到陣列左邊(或是從array中移除)(O(n))
>  * 然後針對大於0的部分，用swap的方式，把0放到nums[0]，把1放到nums[1]...依此類推
>  * 歸類完之後，在loop一遍，第一個index與其值不同的index，則為result
>  */
> var firstMissingPositive = function (nums) {
>     if(nums.length == 0) return 1;
>     nums.push(0);
>     var head, tail, temp;
>     indexHead = 0;
>     indexTail = nums.length - 1;
>
>     while (indexHead <= indexTail) {
>         while (nums[indexHead] < 0 && indexHead < nums.length) {
>             ++indexHead;
>         }
>
>         while (nums[indexTail] >= 0 && indexTail >= 0) {
>             --indexTail;
>         }
>
>         if (indexHead > indexTail) {
>             break;
>         } else {
>             //swap
>             temp = nums[indexHead];
>             nums[indexHead] = nums[indexTail];
>             nums[indexTail] = temp;
>         }
>     }
>     // console.log(nums);
>     // console.log(indexHead);
>     // console.log(indexTail);
>
>     //indexHead is pointing the start of positive part(0 is excluded)
>     var newHeadIndex = indexHead;
>     while (indexHead < nums.length) {
>         if ((nums[indexHead] != indexHead - newHeadIndex) && nums[indexHead] < nums.length - newHeadIndex) {
>             let index1 = indexHead,
>                 index2 = nums[indexHead] + newHeadIndex;
>             // console.log(`${index1}:${nums[index1]}|${index2}:${nums[index2]}`)
>             if (nums[index1] == nums[index2]) {
>                 // console.log(nums);
>                 ++indexHead;
>                 continue;
>             } else {
>                 let temp = nums[index1];
>                 nums[index1] = nums[index2];
>                 nums[index2] = temp;
>                 // console.log(nums);
>             }
>         } else {
>             ++indexHead;
>         }
>     }
>
>     indexHead = newHeadIndex;
>     while (indexHead < nums.length) {
>         // console.log(nums[indexHead]);
>         // console.log(nums[indexHead]);
>         if (nums[indexHead] != (indexHead - newHeadIndex))
>             return indexHead - newHeadIndex;
>         else ++indexHead;
>     }
>
>     function getShiftIndex(currentIndex, newHeadIndex) {
>         return currentIndex - newHeadIndex;
>     }
> };
>
>
> // var nums = [1];
> // var nums = [3, 4, -1, 1, -9, 5, 65, -8, 0, 54, 0, -4, 22];
> // var nums = [2,5,74,1,5,9,4,1,556,8,4,33,64,15,7,5];
> var result = firstMissingPositive(nums);
> console.log(result);
> ```

做完覺得好棒給自己拍手，被拍手聲清空思緒之後，發現多此一舉...

好像根本不必特別把負數獨立出來，所以有了第二版的解法：

## 第二版解法

```js
/**
 * 但後來發現，先把負數取出的步驟多此一舉
 * 
 */
var firstMissingPositive2 = function (nums) {
    if(nums.length == 0) return 1;
    nums.push(0);
    var indexHead = 0;
    while (indexHead < nums.length) {
        if ((nums[indexHead] != indexHead) && nums[indexHead] < nums.length) {
            let index1 = indexHead,
                index2 = nums[indexHead];
            // console.log(`${index1}:${nums[index1]}|${index2}:${nums[index2]}`)
            if (nums[index1] == nums[index2] || nums[indexHead]<0) {
                // console.log(nums);
                ++indexHead;
                continue;
            } else {
                let temp = nums[index1];
                nums[index1] = nums[index2];
                nums[index2] = temp;
                // console.log(nums);
            }
        } else {
            ++indexHead;
        }
    }

    indexHead = 0;
    while (indexHead < nums.length) {
        // console.log(nums[indexHead]);
        // console.log(nums[indexHead]);
        if (nums[indexHead] != (indexHead))
            return indexHead;
        else ++indexHead;
    }
    return indexHead;
}

// var nums = [1];
var nums = [3, 4, -1, 1, -9, 5, 65, -8, 0, 54, 0, -4, 22];
// var nums = [2,5,74,1,5,9,4,1,556,8,4,33,64,15,7,5];
var result = firstMissingPositive2(nums);
console.log(result);
```

## 圖解

假設原先的array長這樣：

> \[3, -2, 4, -1, 1 ,0\]
>
> pointer = 0 ，由於nums\[0\] ==3 != 0，所以把nums\[0\]和nums\[3\] swap

↓ swap

> \[-1, -2, 4, 3, 1 ,0\]
>
> pointer = 0 ，由於nums\[0\] == -1 != 0，但因為沒有nums\[-1\]所以不做swap，pointer向前

↓

> \[-1, -2, 4, 3, 1 ,0\]
>
> pointer = 2\(跳過1，因為nums\[1\]==-2 不處理\) ，由於nums\[2\] == 4 != 2，所以把nums\[2\]和nums\[4\] swap

↓ swap

> \[-1, -2, 1, 3, 4 ,0\]
>
> pointer = 2 ，由於nums\[2\] == 1 != 2，所以把nums\[2\]和nums\[1\] swap

↓ swap

> \[-1, 1, -2, 3, 4 ,0\]
>
> pointer = 2 ，由於nums\[2\] == -2 != 2，但因為沒有nums\[-2\]所以不做swap，pointer向前

↓

> \[-1, 1, -2, 3, 4 ,0\]
>
> pointer = 5\(跳過3,4 因為他們符合index == nums\[index\]\) ，由於nums\[5\] == 0 != 5，所以把nums\[5\]和nums\[0\] swap

↓ swap

> \[0, 1, -2, 3, 4 ,-1\]
>
> pointer = 5，由於nums\[5\] == -1 != 5，但因為沒有nums\[-2\]所以不做swap，pointer向前，但也沒得往前了，所以結束

最後把nums 從頭 loop一遍，發現：nums\[0\] == 0 \(OK\)，nums\[1\] == 1 \(OK\)，nums\[2\] == -2 \(不OK\)，所以return 2

## 結果

Status:Accepted

Runtime:76ms

Your runtime beats 100.00 % of javascript submissions.

