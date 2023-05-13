# 数据结构学习笔记



## 栈 & 队列

利用栈先进后出特性解决问题。

### 从字符串中移除星号

[2390. 从字符串中移除星号]: https://leetcode.cn/problems/removing-stars-from-a-string/description/?envType=study-plan-v2&amp;id=leetcode-75

**示例 1：**

```
输入：s = "leet**cod*e"
输出："lecoe"
解释：从左到右执行移除操作：
- 距离第 1 个星号最近的字符是 "leet**cod*e" 中的 't' ，s 变为 "lee*cod*e" 。
- 距离第 2 个星号最近的字符是 "lee*cod*e" 中的 'e' ，s 变为 "lecod*e" 。
- 距离第 3 个星号最近的字符是 "lecod*e" 中的 'd' ，s 变为 "lecoe" 。
不存在其他星号，返回 "lecoe" 。
```

```js
/**
 * @param {string} s
 * @return {string}
 */
var removeStars = function(s) {
    let res = []
    for(let i=0;i<s.length;i++){
        if(s[i]!="*"){
            res.push(s[i])
        }else{
            res.pop()
        }
    }
    return res.join("")
};
```



### 行星碰撞

给定一个整数数组 `asteroids`，表示在同一行的行星。

对于数组中的每一个元素，其绝对值表示行星的大小，正负表示行星的移动方向（正表示向右移动，负表示向左移动）。每一颗行星以相同的速度移动。

找出碰撞后剩下的所有行星。碰撞规则：两个行星相互碰撞，较小的行星会爆炸。如果两颗行星大小相同，则两颗行星都会爆炸。两颗移动方向相同的行星，永远不会发生碰撞。

**示例 1：**

```
输入：asteroids = [5,10,-5]
输出：[5,10]
解释：10 和 -5 碰撞后只剩下 10 。 5 和 10 永远不会发生碰撞。
```

**示例 2：**

```
输入：asteroids = [8,-8]
输出：[]
解释：8 和 -8 碰撞后，两者都发生爆炸。
```

**示例 3：**

```
输入：asteroids = [10,2,-5]
输出：[10]
解释：2 和 -5 发生碰撞后剩下 -5 。10 和 -5 发生碰撞后剩下 10 。
```

**解决方案：**用栈模拟行星碰撞，遍历数组中的每一颗行星时，用变量alive记录当前行星是否存活。

```js
var asteroidCollision = function(asteroids) {
    const stack = [];
    for (const aster of asteroids) {
        let alive = true;
        while (alive && aster < 0 && stack.length > 0 && stack[stack.length - 1] > 0) {
            alive = stack[stack.length - 1] < -aster; // aster 是否存在
            if (stack[stack.length - 1] <= -aster) {  // 栈顶行星爆炸
                stack.pop();
            }
        }
        if (alive) {
            stack.push(aster);
        }
    }
    const size = stack.length;
    const ans = new Array(size).fill(0);
    for (let i = size - 1; i >= 0; i--) {
        ans[i] = stack.pop();
    }
    return ans;
};

```

我的暴力解决方法仍需改进！

```js
/**
 * @param {number[]} asteroids
 * @return {number[]}
 */
var asteroidCollision = function (asteroids) {
    let res = [], last = null
    for (let i = 0; i < asteroids.length; i++) {
        //结果为空 或者 行星方向向右
        if (res.length < 0 || asteroids[i] > 0) {
            res.push(asteroids[i])
            last = asteroids[i]
            continue
        } else {  //行星向左边行驶
            if (last > 0) {   //发生碰撞
                while (last > 0) {
                    //console.log(res, last, asteroids[i])
                    if (last + asteroids[i] === 0) {
                        res.pop()
                        last = res[res.length - 1]
                        break
                    } else if (last + asteroids[i] < 0) {
                        res.pop()
                        last = res[res.length - 1]
                        if ((last && last < 0) || res.length<1) {
                            res.push(asteroids[i])
                            last = asteroids[i]
                            break
                        }
                    } else {
                        break
                    }
                }
            } else {
                res.push(asteroids[i])
                last = asteroids[i]
            }
        }
    }

    return res
};
```



### 字符串解码

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 `encoded_string` 正好重复 `k` 次。注意 `k` 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 `k` ，例如不会出现像 `3a` 或 `2[4]` 的输入。

**示例 1：**

```
输入：s = "3[a]2[bc]"
输出："aaabcbc"
```

**示例 2：**

```
输入：s = "3[a2[c]]"
输出："accaccacc"
```

**示例 3：**

```
输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"
```

**示例 4：**

```
输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"
```

**解决方案**

本题中可能出现括号嵌套的情况，比如 2[a2[bc]]，这种情况下我们可以先转化成 2[abcbc]，在转化成 abcbcabcbc。我们可以把字母、数字和括号看成是独立的 TOKEN，并用栈来维护这些 TOKEN。具体的做法是，遍历这个栈：

- 如果当前的字符为数位，解析出一个数字（连续的多个数位）并进栈
- 如果当前的字符为字母或者左括号，直接进栈
- 如果当前的字符为右括号，开始出栈，一直到左括号出栈，出栈序列反转后拼接成一个字符串，此时取出栈顶的数字（此时栈顶一定是数字，想想为什么？），就是这个字符串应该出现的次数，我们根据这个次数和字符串构造出新的字符串并进栈
- 重复如上操作，最终将栈中的元素按照从栈底到栈顶的顺序拼接起来，就得到了答案。注意：这里可以用不定长数组来模拟栈操作，方便从栈底向栈顶遍历。

```js
/**
 * @param {string} s
 * @return {string}
 */
var decodeString = function(s) {
    let strstack = [], num = 0
    for(let i=0;i<s.length;i++){
        if(!isNaN(s[i])){  //获取数字
            num = num*10+Number(s[i])
        }else if(s[i]==="]"){ //出栈解码
            let top = strstack.pop()
            let str = ""
            while(top!="["){
                str = top + str
                top = strstack.pop() 
            }
            str = str.repeat(strstack.pop())
            strstack.push(str)
        }else{  //否则全部压入栈中
            if(num>0){
                strstack.push(num)
                num = 0
            }
            strstack.push(s[i])
        }
    }
    let res = strstack.join("")
    return res
}
```



## 树



