# Move zeroes: do not maintain order

<details> <summary>click to expand </summary>

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

</details>


# Move zeroes: maintain order

<details> <summary>click to expand </summary>

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

</details>

# Remove element

<details> <summary>click to expand </summary>

Given an array nums and a value val, remove all instances of that value in-place and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

Clarification:

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by reference, which means a modification to the input array will be known to the caller as well.

Internally you can think of this:

// nums is passed in by reference. (i.e., without making a copy)
int len = removeElement(nums, val);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
 

Example 1:

Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2]
Explanation: Your function should return length = 2, with the first two elements of nums being 2.
It doesn't matter what you leave beyond the returned length. For example if you return 2 with nums = [2,2,3,3] or nums = [2,2,0,0], your answer will be accepted.
Example 2:

Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3]
Explanation: Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4. Note that the order of those five elements can be arbitrary. It doesn't matter what values are set beyond the returned length.
 

Constraints:

0 <= nums.length <= 100
0 <= nums[i] <= 50
0 <= val <= 100

# Solution
```js
    /**
    * @param {number[]} nums
    * @param {number} val
    * @return {number}
    
    [1,1,3,3]
        l
            c
    */
    function removeElement(nums, val) {
        // appraoch move all entries with val to the end then compute new length
        let p = 0; // we know that p and everything before is val
        // everything between p and c is val
        nums.forEach((c,i) => {
            if (c !== val){ // swap to front if not value
                [nums[p], nums[i]] = [nums[i], nums[p]]
                p += 1;
            }
        })
        nums.length = p
    };
```

</details>


# Remove Duplicates from Sorted Array

<details> <summary>click to expand </summary>

Given a sorted array nums, remove the duplicates in-place such that each element appears only once and returns the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

Clarification:

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by reference, which means a modification to the input array will be known to the caller as well.

Internally you can think of this:

// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
 

Example 1:

Input: nums = [1,1,2]
Output: 2, nums = [1,2]
Explanation: Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the returned length.
Example 2:

Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4]
Explanation: Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively. It doesn't matter what values are set beyond the returned length.
 

Constraints:

0 <= nums.length <= 3 * 104
-104 <= nums[i] <= 104
nums is sorted in ascending order.

# Solution
```js
    /**
    * @param {number[]} nums
    * @return {number}
    [...sorted, ...duplicates, ...new values]
            s
                                f
    s and everything before s is no duplicated
    everything between s and f is a duplicate 
    */
    var removeDuplicates = function(nums) {
        if (nums.length === 0) return 0;
        
        let slow = 0; // everything behind this is not a duplicate
        // everything between slow and fast is a duplicate
        nums.forEach((number, fast) => {
            // we will switch places slow and fast are not the same number
            if (nums[slow] !== nums[fast]){
                slow++; // we move up one since we will allow for one of the type
                nums[slow] = nums[fast];
            }
        })
        nums.length = slow + 1;
    };
```

</details> 

# Remove Duplicates from Sorted Array II

<details> <summary>click to expand </summary>

Given a sorted array nums, remove the duplicates in-place such that duplicates appeared at most twice and return the new length.

Do not allocate extra space for another array; you must do this by modifying the input array in-place with O(1) extra memory.

Example 1:
Input: nums = [1,1,1,2,2,3]
Output: 5, nums = [1,1,2,2,3]
Explanation: Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3 respectively. It doesn't matter what you leave beyond the returned length.

Example 2:
Input: nums = [0,0,1,1,1,1,2,3,3]
Output: 7, nums = [0,0,1,1,2,3,3]
Explanation: Your function should return length = 7, with the first seven elements of nums being modified to 0, 0, 1, 1, 2, 3 and 3 respectively. It doesn't matter what values are set beyond the returned length.
 

Constraints:
1 <= nums.length <= 3 * 104
-104 <= nums[i] <= 104
nums is sorted in ascending order.

# Solution 
```js
/**
 * @param {number[]} nums
 * @return {number}    
  [1, 1, 1, 2, 2, 2, 3]
         s
            f

 count: 3
 
 */
var removeDuplicates = function(nums) {
    let slow = 1;

    nums.slice(1).forEach((n, fast) => {
        if (nums[fast] === nums[fast -1]) count += 1;
        else count = 1;
        
        if (count <= 2){
            fast += 1;
            nums[slow] = nums[fast]
            slow += 1;
        }
    })
    nums.length = slow;
};
```

</details>