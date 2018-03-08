# 8. String to Integer \(atoi\)

---

## 題目URL

[https://leetcode.com/problems/string-to-integer-atoi/description/](https://leetcode.com/problems/string-to-integer-atoi/description/)[    
](/h ttps://leetcode.com/problems/3sum/description/)

## 題目敘述

Implement`atoi`to convert a string to an integer.

**Hint:**Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.

**Notes:**It is intended for this problem to be specified vaguely \(ie, no given input specs\). You are responsible to gather all the input requirements up front.

**Requirements for atoi:**

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned. If the correct value is out of the range of representable values, INT\_MAX \(2147483647\) or INT\_MIN \(-2147483648\) is returned.

簡單的說，就是要實作atoi的功能，把字串轉換成整數

atoi function會把在第一個非空白字元出現前的**空白**省略，然後從這個字元開始解讀數字字串的部分\(正負符號也會被成功判讀，但是optional的不一定要加\)，數字字串之後可以接著其他字串，並不影響數字的轉換atoi會把那些多餘的字元濾掉。最後，需要考慮到轉換數字output的上下限，若超過上下限則return上下限\(INT\_MAX \(2147483647\) 或INT\_MIN \(-2147483648\)\)

我超級不懂C，直接看幾個例子：

| input | output | 備註 |
| :--- | :--- | :--- |
| "     +51651adawdgg" | 51651 | 前面空白省略，有正數符號，數字後的字串省略，剩下的數字部分為51651 |
| "             " | 0 | 全都空白，return 0 |
| "  -w9" | 0 | 空白字元之後有負數符號，但接著的是字元而非數字，為無效轉換，return 0 |
| "  -09w" | -9 | 空白字元之後有負數符號，接著的是"09"轉換為9，return -9 |

## 想法

我本來想說，直接用Javascript的parseInt就好了，帶著僥倖的心態送出之後得到wrong answer，才發現parseInt的規則沒有atoi那麼多毛。

字串處理方法，構想是：

> 1.先把字串前面的空白截掉\(loop一遍，找到第一個非空白字元index，然後用slice做出substring\)
>
> 2.判斷有沒正負符號，記錄下來，有的話再削去一個字元
>
> 3.擷取最大長度的連續數字字串並記錄\(loop一遍，找到第一個非數字index，然後用slice做出substring\)
>
> 4.把數字部分轉換成數字，並乘上正負符號，檢查上下限，return

## 解法

> ```js
> /**
>  * @param {string} str
>  * @return {number}
>  */
> var myAtoi = function(str) {
>     //no need to parse empty string
>     if (str.length == 0) return 0;
>
>     const INT_MAX = 2147483647,
>         INT_MIN = -2147483648,
>         NUMBER = "0123456789";
>
>     //remove empty part
>     var index = 0;
>     while (str[index] == ' ') {
>         ++index;
>     }
>     str = str.slice(index, str.length);
>     if (str.length == 0) return 0;
>
>     //extract number part
>     var sign = null,
>         number = null;
>
>     if (str[0] == "+" || str[0] == "-") {
>         sign = str.slice(0, 1);
>         str = str.slice(1, str.length);
>         if (str.length == 0) return 0;
>     }
>
>     index = 0;
>     while (NUMBER.indexOf(str[index]) != -1) ++index;
>     number = str.slice(0, index);
>
>     //convert to number
>     number = arrToNumber(number);
>
>     if (sign != null && sign == "-") {
>         number = -1 * number;
>     }
>
>     if (number > INT_MAX) return INT_MAX;
>     else if (number < INT_MIN) return INT_MIN;
>     else return number;
>
>     function arrToNumber(arr) {
>         var length = arr.length,
>             result = 0;
>
>         for (let i = 0; i < length; ++i) {
>             result += arr[i] * Math.pow(10, (length - i - 1));
>         }
>         return result;
>     }
> };
>
> var string = "    +51651adawdgg";
> myAtoi(string);
> ```

## 結果

Status:Accepted

Runtime:84ms

Your runtime beats 100 % of javascript submissions.



後來發現 function arrToNumber 好像不需要自己寫，直接用parseInt就好了...

這題通過率超低，我想應該是大家直接把這當IDE線上try and error吧

