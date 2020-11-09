给定一个整数数组 nums和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。
```
var arr = [];
var twoSum = function(nums, target) {
    var len = nums.length;
    for(var i=0;i<len-1;i++){
        for(var j=1;j<len-1;j++){
            if(nums[i] + nums[j] === target){
                arr.push(i);
                arr.push(j);
            }
        }
    }
    return arr
};
```