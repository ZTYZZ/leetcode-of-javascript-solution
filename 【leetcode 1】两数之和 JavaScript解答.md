## 题目

给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

**示例:**

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

## 解答

#### 1. 暴力搜索法

使用暴力搜索，将给定数组从`i = 0`开始遍历，再将数组从 `i+1`，开始遍历，找到`target-nums[i]`就终止。复杂度为O(n的平方)。

```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    var result = [];
    for(var i = 0;i<nums.length;i++) {
        for(var j = i+1;j<nums.length;j++) {
            var num2 = target-nums[i]
            if(num2 === nums[j]) {
                result[0] = i;
                result[1] = j;
                break;
            }
        }
            
    }
    return result;
};
```

#### 2. 使用Object（键值对）

由于我们无法立即从数组中得到`target-nums[i]`的数值，所以不得不从头到尾再查找一遍，所以我们使用JavaScript中的对象，来手动实现一个`map`。

```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    var newNums = {};
    var result =[];
    for(var i = 0;i<nums.length;i++) {
        newNums[nums[i]] = i;
    }
    for(var i = 0;i<nums.length;i++) {
        
        var num2 = target-nums[i];
        if(typeof newNums[num2] !== "undefined" && newNums[num2]!==i) {
            result[0] = i;
            result[1] = newNums[num2];
            break;
        }
    }
    return result;
};
```

通过第一遍遍历，将他的键和值互换，生成一个`map`。之后就可以进行O(1)的时间查找了。但是需要O(2n)的复杂度。还可以怎么优化呢？

#### 3. 一遍Object

有没有可能变创建边查呢？因为我们要找的是两个数，这两个数的位置是固定的。一个数永远在另一个数的后面，所以在遍历的过程中就直接可以查找。

```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    var newNums = {};
    var result =[];

    for(var i = 0;i<nums.length;i++) {
        
        var num2 = target-nums[i];
        if(typeof newNums[num2]!=="undefined") {
            result[0] = newNums[num2];
            result[1] = i;
            break;
        }
        else {
            newNums[nums[i]] = i;
        }
    }
    return result;
};
```

实现，但是实际的时间并没有明显的提升，还有哪些地方可以优化的呢？

#### 4. Map方法

查了一下，发现JavaScript实现了一个`Map`对象，所以可以直接使用`Map`，来提高性能。学习链接如下[MDN：Map](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)

```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    var newNums = new Map();
    var result =[];

    for(var i = 0;i<nums.length;i++) {
        
        var num2 = target-nums[i];
        if(newNums.has(num2)) {
            result[0] = newNums.get(num2);
            result[1] = i;
            return result;
        }
        else {
            newNums.set(nums[i],i);
        }
    }
};
```



github同步更新链接：欢迎f**k! [leetcode](https://github.com/ZTYZZ/leetcode-of-javascript-solution)