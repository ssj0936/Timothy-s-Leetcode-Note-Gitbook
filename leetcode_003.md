# 3. Longest Substring Without Repeating Characters

---

* ## 題目URL

[https://leetcode.com/problems/longest-substring-without-repeating-characters/description/](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)



* ## 題目敘述

Given a string, find the length of the **longest substring **without repeating characters.

**Examples:**

Given`"abcabcbb"`, the answer is`"abc"`, which the length is 3.

Given`"bbbbb"`, the answer is`"b"`, with the length of 1.

Given`"pwwkew"`, the answer is`"wke"`, with the length of 3. Note that the answer must be a **substring**,`"pwke"`is a subsequence and not a substring.



給一個string，從其中找到一個最長的substring T，且T之中所有的字元都不重複

**注意**：是substring而不是subsequence，例如input是`"pwwkew"`的話，那output應該是"wke"而不是"pwke"

## 

* ## 想法

老師，這題我有印象！我記得演算法課有上過

初步的想法是：

> 兩層迴圈兩個pointer，第一層pointerA從頭跑到尾，而第二層的pointerB則從pointerA的位置跑到尾巴
>
> 每一回合都找出：以pointerA為開頭的longest substring，若pointerB開始吃到該回合中的重複字元C，那就把pointerA往後移，移到原本位置後第一個C的位置
>
> 舉個例子：若現在s="pwummkew" pointerA指著s\[0\]=="p"，而pointerB跑到s\[4\]=="m"發現這個字元已經重複過了，所以這一回合得到的longest substring是：pwum，那下一回合pointerA其實不需要乖乖地往下一格開始做，因為如果substring之中還存在那個重複的字元，那一定會卡在同一個地方，所以不如把pointerA往前挪挪到第一個出現重複字元的地方也就是s\[3\]，如此一來就可以省去一些loop的時間



* ## 初版解法

> ```js
> /**
>  * @param {string} s
>  * @return {number}
>  */
> var lengthOfLongestSubstring = function(s) {
>     var sArray = Array.from(s);
>
>     var start = 0,
>         end = sArray.length;
>     var LSS = "";
>     
>     for (var i = start; i < end; ++i) {
>         let set = new Set(),
>             substringIndex = i,
>             substring = '';
>         //如果該字元沒有重複，就持續把這該字元加入substring
>         while (!set.has(sArray[substringIndex]) && substringIndex < sArray.length) {
>             set.add(sArray[substringIndex]);
>             substring += sArray[substringIndex];
>             ++substringIndex;
>         }
>         
>         //最後比較目前的取得的LSS和紀錄的LSS哪個長
>         if (substring.length > LSS.length) {
>             LSS = substring;
>             end = (s.length) - LSS.length;
>         }
>
>         //repeat char就是s[substringIndex]
>         //把pointerA往後挪到下一個repeat char出現的位置
>         if (substringIndex != s.length) {
>             latestRepeatChar = s[substringIndex];
>             while (s[i] != latestRepeatChar) {
>                 ++i;
>             }
>         }
>     }
>     return LSS.length;
> };
> ```

做完還沾沾自喜，比初步想法還多做了一個減少迴圈次數的方法\(end = \(s.length\) - LSS.length;\)

結果執行時間400多ms，只贏了20%左右的submit...我對不起我的演算法老師



後來想一想，把pointer往後挪這部分可以改做在已經紀錄的LSS上，改成只做O\(n\)的時間而已



* ## 第二版解法

```js
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    var LSS = "",
        maxLength = 0;
    for (var i = 0; i < s.length; ++i) {
        let char = s[i];
        
        //means first appearence in LSS
        if (LSS.indexOf(char) == -1) {
            LSS += char;
            if (LSS.length > maxLength) {
                maxLength =  LSS.length;
            }
        } 
        //mean this char already appeared in LSS
        else {
            let indexOfFirstRepeatCharInLSS = LSS.indexOf(char);
            LSS = LSS.substr(indexOfFirstRepeatCharInLSS + 1);
            LSS += char;
        }
    }
    return maxLength;
};
```

有幾個改變：

1.判斷是否repeat直接改成用indexOf來做，後來發現用set其實沒有快多少

2.上一版pointerA往後挪的動作，直接做在紀錄的LSS上，直接用substr把LSS中第一個repeat字元之前的字串砍掉

整體的作業流程就變成：

> 前置作業：設定LSS=""，LSSLlength=0
>
> 1. pointerA從頭跑到尾
>
> 2. 每一回合pointerA指到的字元\(C\)如果沒存在於LSS之中，LSS+=C且LSSLlength = Math.max\(LSS.length, LSSLlength\)
>
> 3. 反之如果pointerA指到的字元\(C\)如果已經存在於LSS之中，那就回過頭加工LSS，找出LSS中第一個C出現的位置並將該位置之前的字元全部剃除
>
> 最後跑完迴圈，return LSSLlength;

## 結果

Status:Accepted

Runtime:94ms

Your runtime beats 100 % of javascript submissions.

