# Move zeroes: do not maintain order
```js
Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements and return the number of 0s

Note that you must do this in-place without making a copy of the array.


 

Example 1:
Input: nums = [0,1,0,3,12]
Output: 2


Constraints:
1 <= nums.length <= 104
-231 <= nums[i] <= 231 - 1
```

# Solution
```js
    /**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
[0, 1] -> [1,0]
[0, 2, 1] -> [1, 2, 0]
[a, b, c, ..., z]
 p1            p2


 [12, 1, 3, 0, 0]
     p1     p2
 
 p2 and p1 could terminate at the same place
 [1, 1, 1]
        p2
        p1
 
 again same place 
 [0]
  p1
  p2
  
  it would seem like p1 and p2 could either be point at the same location 
  or p2 < p1 when we need to terminate
 */
function moveZeroes(nums) {
     // algorithm starts the either ends of the array swapping zeros to the end
    let count = 0;
    let nextZero = 0;
    let nextNonZero = nums.length-1;
    const stillValid = () => { // when these two variables are equal then we are done
        return nextZero !== nextNonZero;
    };
    while (stillValid()){
        // move left pointer until we encounter a zero 
        while (stillValid() && nums[nextZero] !== 0){
            nextZero += 1;
        }
        
        // move right pointer until we counter a non zero 
        while (stillValid() && nums[nextNonZero] === 0){
            count += 1;  // we passed a zero so count that
            nextNonZero -= 1;
        }
         // swap places
        if (stillValid()){
            [nums[nextZero], nums[nextNonZero]] = [nums[nextNonZero], nums[nextZero];
        }
       
    }
    return count;
};

```


# Move zeroes: maintain order
Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements.

Note that you must do this in-place without making a copy of the array.

 

Example 1:

Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
Example 2:

Input: nums = [0]
Output: [0]
 

Constraints:
1 <= nums.length <= 104
-231 <= nums[i] <= 231 - 1

# Solution
```js
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
function moveZeroes(nums) {
    // let p, c be pointers that start at index 0
    // we declare that everything behind p is non zero and everything between c and p is zero
    // this is true at the initial position note that nothing is behind p and
    // nothing is between p and c
    // [x, y, z, ...]
    //  p 
    //  c
    // our algorithm will ensure this by 
    // only moving c forward when the current number is not 0
    // only advancing p when we are making a swap when c is pointing at a non zero number
    // [..., 2, 4, 0, 0, 1, ...]
    //             p     c
    let p = 0;
    nums.forEach((c, i) => {
        // if c is 0 then we don't do anything and let the loop continue
        if (c !== 0){ 
            [nums[i], nums[p]] = [nums[p], nums[i]];
            p += 1;
        }
    })
};

```