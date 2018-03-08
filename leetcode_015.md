# 15. 3Sum

---

## 題目URL

[https://leetcode.com/problems/3sum/description/  
](/h ttps://leetcode.com/problems/3sum/description/)

## 題目敘述

Given an array S of n integers, are there elements a, b, c in S such that a+b+c= 0? Find all unique triplets in the array which gives the sum of zero.

Note : The solution set must not contain duplicate triplets.

> For example, given array S = \[-1, 0, 1, 2, -1, -4\],
>
> A solution set is: \[
>
> \[-1, 0, 1\],
>
> \[-1, -1, 2\]
>
> \]

給一個陣列S內有n個整數，return所有其中三元素相加總合為0的element 陣列，注意：return 的值中不可有重複的

## 想法

天真，真是天真，做完2 sum，就以為3 sum好像也蠻簡單的，殊不知3 sum跟3some一樣困難

最暴力的解法，做3次loop跑每一個組合\(O\(n^3\)\)，並把符合的組合放到results的array中，最後檢查results裡面的值有沒有重複\(O\(m^3\)\)，果不其索然得到了Time Limit Exceeded的結果

最後這題其實自己沒有完全想出來\(只想到要先sort，之後就困頓了一天\)，最後從WIKI才發現這是個有名的難題

解法：先把array sort過，用三個pointer，pointer\_1從頭掃到尾，pointer\_2從pointer\_1後一位開始往後退，pointer\_3，從S尾端往前進，思維就是，若三個pointer指到的數字總和&gt;0，那pointer\_2往後退一格；若三個pointer指到的數字總和&lt;0，那pointer\_3往前進一格；若三個pointer指到的數字=0，把組合放進results裡面\(當然要先檢查重複\)，時間複雜度，為n+\(n-1\)+\(n-2\)+...+1 = O\(n^2\)

## 解法

> ```js
> /**
>  * @param {number[]} nums
>  * @return {number[][]}
>  */
> var threeSum = function(nums) {
>     if (nums.length < 3) return [];
>
>     nums = numberSort(nums);
>
>     var i0, i1, i2,len = nums.length;
>     var results = [];
>     var set = new Set();
>     for (i0 = 0, i1 = i0 + 1, i2 = len - 1; i0 < len - 2; ++i0, i1 = i0 + 1, i2 = len - 1) {
>         while (i1 < i2) {
>             if (nums[i0] + nums[i1] + nums[i2] > 0) {
>                 --i2;
>             } else if (nums[i0] + nums[i1] + nums[i2] < 0) {
>                 ++i1;
>             } else {
>                 //以set作為判斷有沒有重複的工具
>                 //因為set判斷是否存在的方式，需先轉換為字串，不可以直接用array
>                 if(!set.has(`${nums[i0]}_${nums[i1]}_${nums[i2]}`)){
>                     set.add(`${nums[i0]}_${nums[i1]}_${nums[i2]}`);
>                     results.push([nums[i0] , nums[i1] , nums[i2]]);
>                 }
>
>                 --i2;
>                 ++i1;
>             }
>         }
>     }
>
>     return results;
>
>     function numberSort(nums) {
>         return nums.sort((a, b) => {
>             return a - b;
>         });
>     }
> };
>
> var number = [-1,0,1,2,-1,-4,-1,-1,-1,-1];
> var results = threeSum(number);
> console.log(results);
> ```

結果

Status:Accepted

Runtime:396 ms

Your runtime beats 30.30 % of javascript submissions.

分數其實很低，判斷有沒有重複解的部分可以再改進，在其他地方看到有一個解是說：

既然已經sort過，那在找到解之後，pointer\_2和pointer\_3可以繼續前進把重複解濾掉，那就不用set了
