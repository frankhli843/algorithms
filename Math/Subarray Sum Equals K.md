# Prompt
Given an array of integers nums and an integer k, return the total number of continuous subarrays whose sum equals to k.
## Examples
Example 1:
Input: nums = [1,1,1], k = 2
Output: 2
Example 2:

Input: nums = [1,2,3], k = 3
Output: 2
 
## Constraints:
1 <= nums.length <= 2 * 104
-1000 <= nums[i] <= 1000
-107 <= k <= 107

## My attempt 
```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
function subarraySum(nums, k) {
    let count = 0;
    let sum = 0;
    const storeHash = { 0: 1 }; // starts at sum as 0 { sumAtIndex: number } 
    nums.forEach(currentNumber => {
        sum += currentNumber;
        // for the value sum is away from k we check how many times we have
        // encountered a sublist that has such a value. We can exclude 
        // those combinations to get our result so we know there is 
        // that many ways to get the answer.
        if (sum -k in storeHash) count += storeHash[sum -k];
        
        // Store the current sum in our hash to represent another sublist 
        // that equals to a given amount
        if (sum in storeHash) storeHash[sum] += 1;
        else storeHash[sum] = 1;
    })
    return count;
};
```



