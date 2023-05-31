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

### 深度优先

#### 二叉树右视图



#### 子节点最近公共祖先

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

![binarytree](D:\大四\前端开发实习学习准备\Frontend-projects\Notebook\Data Structure\pics\binarytree.png)

```js
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出：5
解释：节点 5 和节点 4 的最近公共祖先是节点 5 。因为根据定义最近公共祖先节点可以为节点本身。
```

**公共祖先条件判断**

最近的节点左右两边有p、q  或者 当前节点为p或者q的值且其左右两边任意一边存在p，q

```
(left&&right) || (node.val == p) && (left||right) || (node.val==q) && (left||right)
```

```js
var lowestCommonAncestor = function (root, p, q) {
    //node.val 互不相同 
    //公共祖先条件：(left&&right) || (node.val == p) && (left||right) || (node.val==q) && (left||right)
    let flag = false, res = -1
    function dfs(node) {
        if (flag) return false //找到就剪枝
        if (!node) return false
        let left = dfs(node.left)
        let right = dfs(node.right)
        //判断该节点是否为公共祖先
        if (left && right ||
            (node.val === p.val && (left || right)) ||
            (node.val === q.val && (left || right))) {
            flag = true  //found
            res = node
        }
        if (node.val == p.val || node.val == q.val) return true
        return (left || right)
    }
    dfs(root)
    return res
};
```



#### 路径总和 III

给定一个二叉树的根节点 `root` ，和一个整数 `targetSum` ，求该二叉树里节点值之和等于 `targetSum` 的 **路径** 的数目。

**路径** 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

![pathsum3-1-tree](D:\大四\前端开发实习学习准备\Frontend-projects\Notebook\Data Structure\pics\pathsum3-1-tree.jpg)

```js
//实例
输入：root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
输出：3
解释：和等于 8 的路径有 3 条，如图所示。
```

**解决方案**

对每一个节点求解符合条件的路径数目。

```js
//计算该节点下符合条件的路径
function rootSum(node, sum) {
    if (!node) return 0
    //返回结果值
    let ret = 0
    if ((sum - node.val) === 0) ret += 1
    ret += rootSum(node.left, sum - node.val)
    ret += rootSum(node.right, sum - node.val)
    return ret
}
//遍历节点
var pathSum = function (root, targetSum) {
    if (!root) return 0
    //返回结果值
    let ret = 0
    //找本节点下符合条件的所有路径
    ret += rootSum(root, targetSum)
    //再往下左右节点去找
    ret += pathSum(root.left, targetSum)
    ret += pathSum(root.right, targetSum)
    return ret
};
```



#### 二叉树中的最长交错路径



```js
var longestZigZag = function(root) {
    let res = 0
    //计算当前节点的最长交错路径
    function maxPath(node,flag,level){
        if(!node) return level
        node.val = -1
        let left = 0,right =0
        //剪枝：往没开辟过的地方遍历
        if(flag){
            if(node.left && node.left.val<0) return level
            left = maxPath(node.left,false,level+1)
        }else{
            if(node.right && node.right.val<0) return level
            right = maxPath(node.right,true,level+1)
        }
        return Math.max(left,right)
    }
    //遍历每一个节点
    function travelNode(node){
        if(!node) return
        let ret = Math.max(maxPath(node.left,false,0),maxPath(node.right,true,0))
        res = (ret>res)?ret:res
        travelNode(node.left)
        travelNode(node.right)
    }
    
    travelNode(root)
    return res
};
```





### 二叉搜索树

二叉搜索树有以下性质：

左子树的所有节点（如果有）的值均小于当前节点的值；
右子树的所有节点（如果有）的值均大于当前节点的值；
左子树和右子树均为二叉搜索树。



####  删除二叉搜索树中的节点

给定一个二叉搜索树的根节点 **root** 和一个值 **key**，删除二叉搜索树中的 **key** 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：

1. 首先找到需要删除的节点；
2. 如果找到了，删除它。

![del_node_1](D:\大四\前端开发实习学习准备\Frontend-projects\Notebook\Data Structure\pics\del_node_1.jpg)

```js
输入：root = [5,3,6,2,4,null,7], key = 3
输出：[5,4,6,2,null,null,7]
解释：给定需要删除的节点值是 3，所以我们首先找到 3 这个节点，然后删除它。
一个正确的答案是 [5,4,6,2,null,null,7], 如下图所示。
另一个正确答案是 [5,2,6,null,4,null,7]。
```

**Solution**

- 没找到删除节点：直接返回。
- 找到删除节点：
  1. 无左右节点：直接删除该节点；
  2. 仅无左节点或者仅无右节点：让不为空的左or右补上该节点；
  3. 左右节点都不为空：当删除节点有两个子节点时，你需要找到右子树中最小的节点，并将其值赋给要删除的节点，然后再删除右子树中最小的节点。

在删除节点时，不能改变参数node `node = null` 来尝试删除节点，这只是将 `node` 变量指向了 `null`，并不会影响原始树结构。正确的删除节点方式是将父节点的指针指向正确的子节点。

```js
var deleteNode = function (root, key) {
    //树为空
    if (!root) return root
    if (root.val === key) {
        if (!root.left && !root.right) return null
        if (!root.left) return root.right
        if (!root.right) return root.left
        //存在左右节点
        let cur = root.right
        while(cur.left){
            cur = cur.left
        }
        cur.left = root.left
        root = root.right
        delete root
        return root
    }
    //往下寻找
    if(root.val<key){
        root.right = deleteNode(root.right,key)
    }else{
        root.left = deleteNode(root.left,key)
    }
    return root
};
```

