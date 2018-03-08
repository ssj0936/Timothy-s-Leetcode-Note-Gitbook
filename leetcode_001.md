# **1.Two Sum**

---

## **題目URL **

[https://leetcode.com/problems/two-sum/description/](https://leetcode.com/problems/two-sum/description/)

## **題目敘述**

Given an array of integers, return **indices **of the two numbers such that they add up to a specific target. You may assume that each input would have **exactly **one solution, and you may not use the same element twice.

> Given nums = \[2, 7, 11, 15\], target = 9,
>
> 
Because nums\[0\] + nums\[1\] = 2 + 7 = 9,
>
> return \[0, 1\].



給一個**整數array**與一個**target**，回傳陣列中，兩個相加所得之和為target的**index**，注意：每個array中只會有剛好一個solution，且不可以用同一個元素兩次  


---

## **想法**

第一題，簡單的試水溫

最暴力的方法就是跑兩次for，loop一遍整個array，時間O\(n^2\)

比較快的方法：把每個元素放到set\(map\)裡面，用for跑一遍array，用has來確定有沒有解



## **解法**

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var arr = [3, 2, 4],
    target = 6;

var twoSum = function (nums, target) {
    var map = new Map();
    nums.forEach((item, index) => {
        map.set(item, index);
    });

    for (var [index, value] of nums.entries()) {
        let diff = target - value;
        if (map.has(diff) && index != map.get(diff)) {
            return [index,map.get(diff)];
        }
    }
};

var result = twoSum(arr, target);
console.log(result);
```



## **結果**

Status:Accepted

Runtime:**76 ms**

Your runtime beats 100.00 % of javascript submissions.


