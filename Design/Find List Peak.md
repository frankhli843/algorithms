# Find List Peak
A peak element is an element that is strictly greater than its neighbors.

Given an integer array nums, find a peak element, and return its index. If the array contains multiple peaks, return the index to any of the peaks.

You may imagine that `nums[-1] = nums[n] = -∞`.

 ```
Example 1:

Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
Example 2:

Input: nums = [1,2,1,3,5,6,4]
Output: 5
Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.
 
```
# Constraints:
```
1 <= nums.length <= 1000
-231 <= nums[i] <= 231 - 1
nums[i] != nums[i + 1] for all valid i.
```

Follow up: Could you implement a solution with logarithmic complexity?

# My Attempt
```js
/**
 * We want to also calculate the index of the element itself without having 
 * to transverse it already
 * [a, b, c, d, e, f]
 *  0  1  2  3  4  5
 *        ^ in the first round our pivot value is 2 
 * we move left so we have a smaller array which includes c
 * [a, b, c]
 *  0  1  2]
 *     ^ in the first round our pivot value is 1
 * 
 * Thus we can see that in each pivot the index of the middle value will always 
 * be floor(currentPivot/2)
 */
function peakElement(list, pivotIndex = -1){
  // base case 0: less than 3 we cannot compare
  if (list.length < 3){
    return -1;
  }
  
  middleIndex = Math.floor((list.length - 1)/2);
  pivotIndex = pivotIndex === -1 
    ? middleIndex // first call
    : Math.floor(pivotIndex/2) // calculate new index at level

  // [a, b, c, d, e]
  //        ^
  const current = list[middleIndex]; 
  // case 1: check if middle is element 
  if (list[middleIndex-1] < current && current > list[middleIndex+1]){
    return pivotIndex;
  }
  // case 2: check if left is element  [a, b, c]
  const left = peakElement(
    list.slice(0, middleIndex+1),
    pivotIndex
  ); 
  if (left !== -1){
    return left;
  }
  // case 3: check if right is element [c, d, e] 
  const right = peakElement(
    list.slice(pivotIndex, list.length),
    pivotIndex
  )
  if (right !== -1){
    return right;
  }
  // otherwise none found
  return -1
}

const j = (i) => { return JSON.stringify(j) };

const test = (desc, expected, actual) => {
  console.log(j(expected) === j(actual) 
    ? `${desc} test passed!`
    : `${desc} test failed\n  expected:${expected}\n  actual:${actual}`
  )
}
test("[1,2,3,1]", 2,peakElement([1,2,3,1]))
test("[1,2,1,3,5,6,4]", 5,peakElement([1,2,1,3,5,6,4]))
```
