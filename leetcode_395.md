# 395.Longest Substring with At Least K Repeating Characters

---

## 題目URL

[https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/description/](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/description/)

## 題目敘述

Find the length of the longest substring **T **of a given string \(consists of lowercase letters only\) such that every character in **T **appears no less than k times.

**Example 1:**

```
Input:
s = "aaabb", k = 3

Output:
3

The longest substring is "aaa", as 'a' is repeated 3 times.
```

**Example 2:**

```
Input:
s = "ababbc", k = 2

Output:
5

The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.
```

給一個字串s，與repeat最低門檻k，找出s之中的substring T，而T之中的每個字母都重複出現至少k次，最後return substring T的長度。

## 想法

想了一陣子才有了很關鍵的想法：

> s之中出現頻率小於k的那些字元，絕對不會出現在substring T之中

所以最初的想法是：

> 1.先把s之中，頻率小於k的字元找出來放到set之中。\(稱作：不合格的字元\)
>
> 2.如果這個set.length=0，代表這整條s都是legal的，則return s.length；否則進行下一步驟
>
> 3.接著把pointer放在最開端，從頭到尾loop一次
>
> 4.如果pointer指到的字元是不合格的字元\(例如：s\[k\]是不合格的字元好了\)，那就以該字元做為切割依據，將該字元之前的子字串\(s\[0\]到s\[k-1\]\)和該字元之後的子字串\(s\[k+1\]到s\[s.length-1\]\)，分別回頭到步驟1並return較大值

所以要用recursive的方式來做

## 解法

> ```js
> /**
>  * @param {string} s
>  * @param {number} k
>  * @return {number}
>  */
> var longestSubstring = function (s, k) {
>     return recursive(s, k);
>
>     function recursive(s, k) {
>         if (s.length == 0) return 0;
>         if (s.length < k) return 0;
>         //check if s is appropriate
>
>         //先抽出s中不合格的元素
>         //數量小於K的元素
>         var sArray = Array.from(s);
>         var sArraySorted = sArray.slice().sort();
>         var inappropriateElement = [];
>
>         var currentWord = null,
>             currentCount = 0;
>         
>         //簡單的說，這邊是在找每個字母的頻率，後來覺得這方法實在很冗又易出錯
>         for (var [i, v] of sArraySorted.entries()) {
>             //結算
>             if (currentWord != v) {
>                 if (currentWord != null) {
>                     if (currentCount < k) inappropriateElement.push(currentWord);
>                 }
>                 currentCount = 0;
>                 currentWord = v;
>             }
>             ++currentCount;
>             if (i == s.length - 1) {
>                 if (currentWord != null) {
>                     if (currentCount < k) inappropriateElement.push(currentWord);
>                 }
>                 currentCount = 0;
>                 currentWord = v;
>             }
>         }
>
>         //如果沒有不合格的字元，那代表整個字串都是符合規定的，回傳長度
>         if(inappropriateElement.length == 0) return s.length;
>
>         //隨機挑選一個不合格的字元，做為切割字串的依據
>         var arrOfIndex = [],index;
>         for(var [i,v] of sArray.entries()){
>             if(inappropriateElement.indexOf(v)!=-1){
>                 arrOfIndex.push(i);
>             }
>         }
>         index = arrOfIndex[parseInt(arrOfIndex.length / 2)];
>         
>         //將切割後的兩子字串分別再去做一次處理，並回傳較大的一方
>         return Math.max(
>           recursive(s.slice(0, index), k),
>           recursive(s.slice(index + 1, s.length), k)
>           );           
>     }
> };
> ```

這樣做的確verify了，也贏過將近八成的人，但其中對於有些處理的方式不是很滿意，像是：

1.找出不合格字元的method

2.切割字串送去遞迴的部分

然後就寫了第二版



## 第二版的解法

```js
/**
 * @param {string} s
 * @param {number} k
 * @return {number}
 */
var longestSubstring = function (s, k) {
    return recursive(s, k);

    function recursive(s, k) {
        if (s.length == 0) return 0;
        if (s.length < k) return 0;

        //先抽出s中不合格的元素
        //數量小於K的元素
        var temp = {};
        var sArray = Array.from(s);
        for (var i of sArray) {
            temp[i] = temp[i] || 0;
            ++temp[i];
        }
        var inappropriateElement = [];
        for (var i in temp) {
            if (temp[i] < k) inappropriateElement.push(i);
        }

        //check if s is appropriate
        if (inappropriateElement.length == 0) {
            return s.length;
        } 
        //這裡使用regex，把s之中的不合格字元當作切割依據
        //並將切出來的每個子字串都送去做recursive
        else {
            var split = s.split(new RegExp(inappropriateElement.join('|')));
            var max = 0;
            for(var i of split){
                var len = recursive(i,k);
                max = (len>max)? len : max;
            }
            return max;
        }
    }
};
```

改進的地方有：

1.找不合格字元的做法，改以用object來做，以字元當作object的property，其value則記錄頻率，最後loop過一遍object即可得到不及格的字元，寫法相對初版簡便優雅許多

2.初版只用一個不及格元素央s切割成兩個，這次是用regex一次取得所有切割子字串，應該會省去很多不必要的判斷



## 結果

Status:Accepted

Runtime: 80ms

Your runtime beats 100.00 % of javascript submissions.



