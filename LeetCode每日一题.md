# [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

> 给定一个未排序的整数数组，找出最长连续序列的长度。
>
> 要求算法的时间复杂度为 O(n)。

示例:

```
输入: [100, 4, 200, 1, 3, 2]
输出: 4
解释: 最长连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

解：

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var longestConsecutive = function(nums) {
    if (nums.length==0 || nums.length==1){
        return nums.length
    }
    nums = nums.sort((a,b)=>{
        return a - b
    })
    nums = [...new Set(nums)];
    let count = 0;
    for(let i=0;i<nums.length;i++){
        // 可能存在多组连续的数，留下大长度
        let tempCount = 0;
        let tempNum = nums[i];
        // 判断是否连续
        if (tempNum-1 === nums[i-1]) continue
        while(nums.indexOf(tempNum+1)>-1){
            // 存在连续的数，数量加一
            tempNum += 1
            tempCount += 1;
        }
        if (tempCount>count){
            count = tempCount;
        }
    }
    return count + 1;
};
```

# [9. 回文数](https://leetcode-cn.com/problems/palindrome-number/)

> 判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

示例1：

```
输入: 121
输出: true
```

示例2：

```
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

示例3：

```
输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
```

解：

```javascript
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    let x1 = x.toString().split("").reverse().join("");
    if (x != x1 || x < 0){
        return false
    }else {
        return true
    }
};
```

进阶：能不能不将整数转为字符串来解决这个问题？

> 利用while结合x%10的余数，赋值给一个变量*10累加实现数组翻转，值得注意的一点是要判断不断让x/10取整

```javascript
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
        let s = 0;
        let x1 = x;
        while (x1 > 0) {
            s = s * 10 + x1 % 10;
            x1 = parseInt(x1 / 10);
        }
        return s == x;
};

作者：shinyzhang-5
链接：https://leetcode-cn.com/problems/palindrome-number/solution/hui-wen-shu-by-shinyzhang-5/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

