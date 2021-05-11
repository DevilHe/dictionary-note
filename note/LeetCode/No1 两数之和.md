#### 1. 两数之和

给定一个整数数组 <strong>nums</strong>  和一个整数目标值 <strong>target</strong>，请你在该数组中找出 <strong>和为目标值</strong> 的那  <strong>两个</strong>  整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

##### 示例

输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

##### 代码

```

/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
  var temp = []
  for(let i=0;i<nums.length;i++) {
    var a = target - nums[i]
    if(temp[a] !== undefined) {
      return [temp[a], i]
    }
    temp[nums[i]] = i
  }
};
console.log('twoSum', twoSum([2,7,11,15], 9))
// [0, 1]

```
