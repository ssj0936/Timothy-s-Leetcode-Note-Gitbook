# 200.Number of Islands

---

## 題目URL

[https://leetcode.com/problems/number-of-islands/description/](https://leetcode.com/problems/number-of-islands/description/)

## 題目敘述

Given a 2d grid map of`'1'`s \(land\) and`'0'`s \(water\), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

```
11110
11010
11000
00000
```

Answer: 1

**Example 2:**

```
11000
11000
00100
00011
```

Answer: 3



給你一個二維陣列，如上面例子般那樣展開像是地圖一樣，把0當作海洋，1當作陸地，請return這張地圖上有幾座小島



## 想法

不得不說，這題目蠻有趣的至少是第一次看到，但說穿了就是deep first search。初步的想法是：

> 從地圖的左上角開始，往下往右一行一行的loop過每一個點，要是該點是"陸地"，那就把這點的內容從1改成C，並從這點開始向上下左右開始"著色"這塊陸地，將之領地全部從1塗成C\(這裡會以recursive function來實現\)，這一塊陸地全部塗完之後，count++，最後loop完後return count



## 解法

> ```js
> /**
>  * @param {character[][]} grid
>  * @return {number}
>  */
> var numIslands = function(grid) {
>     var foundCount = 0;
>     
>     //就是loop每一個點
>     for(var [x,subArr] of grid.entries()){
>         for(var [y,value] of subArr.entries()){
>             if(value != "0" && value != "c" ){
>                 recursiveFindLand(x,y);
>                 ++foundCount;
>             }
>         }
>     }
>     return foundCount;
>
>     function recursiveFindLand(x,y,from){
>         //如果這點已經被塗過了 那就return
>         if(grid[x][y] == "0" || grid[x][y] == "c") return;
>
>         //把這點塗成"C"
>         grid[x][y] = "c";
>         
>         //針對該點上下左右的點去做塗色，當然要做index有沒有超出的判斷
>         if(grid[0].length > (y+1)){
>             recursiveFindLand(x,y+1);
>         }
>         if(0 <= (y-1)){
>             recursiveFindLand(x,y-1);
>         }
>         if(grid.length > (x+1)){
>             recursiveFindLand(x+1,y);
>         }
>         if(0<=(x-1)){
>             recursiveFindLand(x-1,y);
>         }
>     }
> };
> ```

## 結果

Status:Accepted

Runtime: 80ms

Your runtime beats 100.00 % of javascript submissions.



感覺是個很簡單的recursive 學校作業，但難易度卻是medium

