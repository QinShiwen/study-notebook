# 算法学习笔记

## 动态规划

动态规划是一种解决多阶段决策问题的算法，它将问题分解成若干个子问题，并且在求解过程中利用了子问题的重复性质。通过将每个子问题的解保存在一个表格中，避免了重复求解相同的子问题，提高了算法的效率。以下是动态规划算法的一般解题思路和技巧：

1. 定义状态：将原问题拆分成若干个子问题，并定义每个子问题的状态。
2. 定义状态转移方程：通过已知状态推导出未知状态的转移方程，这是动态规划的核心。
3. 确定初始状态：找到最小的子问题的解，定义为初始状态。
4. 利用状态转移方程和初始状态，递推求解问题。
5. 优化空间复杂度：在实现时可以考虑对状态表格进行压缩，只保留必要的状态。



### 用最小花费爬楼梯

- 给你一个整数数组 `cost` ，其中 `cost[i]` 是从楼梯第 `i` 个台阶向上爬需要支付的费用。一旦你支付此费用，即可选择向上爬一个或者两个台阶。

- 你可以选择从下标为 `0` 或下标为 `1` 的台阶开始爬楼梯。

- 请你计算并返回达到楼梯顶部的最低花费。

- 案例。

  ```js
  输入：cost = [10,15,20]
  输出：15
  解释：你将从下标为 1 的台阶开始。
  - 支付 15 ，向上爬两个台阶，到达楼梯顶部。
  总花费为 15 。
  ```

- Solution

  ```js
  /**
   * @param {number[]} cost
   * @return {number}
   */
  var minCostClimbingStairs = function(cost) {
      //step = list.length+1 dn = min(d[n-2]+d[n],d[n-1]+d[n])
      let len = cost.length
      for(let i=2;i<len;i++){
          cost[i] = Math.min(cost[i-2]+cost[i],cost[i-1]+cost[i])
      }
      return Math.min(cost[len-1],cost[len-2])
  };
  ```

  

### 打家劫舍







### 多米诺和托米诺平铺

有两种形状的瓷砖：一种是 `2 x 1` 的多米诺形，另一种是形如 "L" 的托米诺形。两种形状都可以旋转。

![img](https://assets.leetcode.com/uploads/2021/07/15/lc-domino.jpg)

给定整数 n ，返回可以平铺 `2 x n` 的面板的方法的数量。**返回对** `109 + 7` **取模** 的值。

平铺指的是每个正方形都必须有瓷砖覆盖。两个平铺不同，当且仅当面板上有四个方向上的相邻单元中的两个，使得恰好有一个平铺有一个瓷砖占据两个正方形。

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/07/15/lc-domino1.jpg)

```js
输入: n = 3
输出: 5
解释: 五种不同的方法如上所示。
```

**示例 2:**

```
输入: n = 1
输出: 1
```

**解题方法**

找规律+动态规划+缓存

**方法1：**

考虑这么一种平铺的方式：在第 i 列前面的正方形都被瓷砖覆盖，在第 i 列后面的正方形都没有被瓷砖覆盖（i 从 1开始计数）。那么第 i列的正方形有四种被覆盖的情况：

一个正方形都没有被覆盖，只有上方的正方形被覆盖，只有下方的正方形被覆盖，上下两个正方形都被覆盖。

![1](D:\大四\前端开发实习学习准备\Frontend-projects\Notebook\Algorithm\pics\1.png)

```js
//2*n多维
//规律1：第i块瓷砖的状态是由i-1块瓷砖决定的
//有四种可能性状态 0:空 1:上满下空 2:下空上满 3:全满
// d[i][0] <= d[i][3]; d[i][1] <= d[i][0] & d[i][2]
// d[i][2] <= d[i][0] & d[i][1]
// d[i][3] <= d[i][0] & d[i][1] & d[i][2] & d[i][3]
let d = Array.from(new Array(n+1),()=>new Array(4).fill(0))
//初始状态
d[0][3] = 1
const mod = 1e9 + 7
for(let i =1;i<=n;i++){
    d[i][0] = d[i-1][3]
    d[i][1] = (d[i-1][0] + d[i-1][2]) % mod
    d[i][2] = (d[i-1][0] + d[i-1][1]) % mod
    d[i][3] = (d[i-1][0] + d[i-1][1] + d[i-1][2] + d[i-1][3]) % mod
}
return d[n][3]
```

**方法2：**

```js
//规律2：f(0) => f(1)=1 => f(2)=2 => f(3)=5 => f(4)=11
//f(x)=2*f(x-1)+f(x-3)
let d = new Array(n).fill(0)
d[0] = 1, d[1] = 1, d[2] = 2
for(let i = 3;i<=n;i++){
    d[i] = (2*d[i-1]+d[i-3]) % (1e9+7)
}
return d[n]
```





## 贪心算法

### 买卖股票的最佳时机含手续费





### Dota2 参议院

Dota2 的世界里有两个阵营：`Radiant`（天辉）和 `Dire`（夜魇）

Dota2 参议院由来自两派的参议员组成。现在参议院希望对一个 Dota2 游戏里的改变作出决定。他们以一个基于轮为过程的投票进行。在每一轮中，每一位参议员都可以行使两项权利中的 **一** 项：

- **禁止一名参议员的权利**：参议员可以让另一位参议员在这一轮和随后的几轮中丧失 **所有的权利** 。
- **宣布胜利**：如果参议员发现有权利投票的参议员都是 **同一个阵营的** ，他可以宣布胜利并决定在游戏中的有关变化。

[Dota2 参议院]: https://leetcode.cn/problems/dota2-senate/description/?envType=study-plan-v2&amp;id=leetcode-75

**解决方案**

我们以天辉方的议员为例。当一名天辉方的议员行使权利时：

- 如果目前所有的议员都为天辉方，那么该议员可以直接宣布天辉方取得胜利；

- 如果目前仍然有夜魇方的议员，那么这名天辉方的议员只能行使「禁止一名参议员的权利」这一项权利。显然，该议员不会令一名同为天辉方的议员丧失权利，所以他一定会挑选一名夜魇方的议员。那么应该挑选哪一名议员呢？容易想到的是，**应该贪心地挑选按照投票顺序的下一名夜魇方的议员。**这也是很容易形象化地证明的：既然只能挑选一名夜魇方的议员，那么就**应该挑最早可以进行投票的那一名议员**；如果挑选了其他较晚投票的议员，那么等到最早可以进行投票的那一名议员行使权利时，一名天辉方议员就会丧失权利，这样就得不偿失了。

因此使用两个队列radiant和dire存储两个议员组中每位议员的投票时间，设n为所有议员的数量。

1. 当radiant或者dire为空时，宣布另一方的胜利。
2. 若都不为空：对比两个队列首位元素的投票时间。
   - 若radiant的首元素小，则其先把dire的首元素投出去。radiant首元素弹出将时间增加n后再放入数组中。
   - 哪一方的队列先空，则另一方胜利。

```js
/**
 * @param {string} senate
 * @return {string}
 */
var predictPartyVictory = function (senate) {
    let radiant = [], dire = [], len = senate.length
    for(let i = 0;i<len;i++){
      (senate[i]==="R")?radiant.push(i):dire.push(i)
    }
    
    while(radiant.length && dire.length){
      if(radiant[0]<dire[0]){
        radiant.push(radiant[0]+len)
      }else{
        dire.push(dire[0]+len)
      }
      radiant.shift()
      dire.shift()
    }

    return radiant.length>0?"Radiant":"Dire"
};
```


