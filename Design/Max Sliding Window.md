# Max Sliding Window
You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

 
```js
Example 1:

Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation: 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

```js
Example 2:

Input: nums = [1], k = 1
Output: [1]
Example 3:

Input: nums = [1,-1], k = 1
Output: [1,-1]
Example 4:

Input: nums = [9,11], k = 2
Output: [11]
Example 5:

Input: nums = [4,-2], k = 2
Output: [4]
```

```js
Constraints:

1 <= nums.length <= 105
-104 <= nums[i] <= 104
1 <= k <= nums.length
```

# My Attempt
```js


function maxSliding(nums, k) {
    const q = new Queue();  // our queue a list of { index: number, originalNumber: number} 
    const res = [];

    nums.forEach((currentNumber, i) => {
      // remove all values from queue that is smaller than the current number
      q.removeSmallerThan(currentNumber);
      // add the current number
      q.append(new Node(i, currentNumber));
  
      // remove first element from queue if it's outside the window
      //(_, _ [a),b,c,d], queue: [a], k = 3, i = 0
      // [a,(b,c,d)], queue [a,...], k = 3, i = 3 this is when we don't want a anymore
      // so when k == i the number is no longer in the window and we want to remove it 
      if (q.getFirst().index === i - k) 
        q.removeStart();
        
      // add the first of the queue if the window has at least k elements. This 
      // is because we start with (_, _, firstElement)
      // if for example k = 3, anything after this: [(a,b,c),d]
      if (i >= k - 1) 
        res.push(q.getFirst().number);
    })
    return res;    
};

class Node { 
  index = null; 
  number = null; 
  constructor(index, number){
    this.index = index;
    this.number = number;
  }
}

class Queue {
  list = [];

  getFirst(){ 
    if (this.list.length !== 0) 
      return this.list[0]; 
    else 
      return new Node() 
  }
  getLast(){ 
      if (this.list.length !== 0) 
        return this.list[this.list.length - 1] 
      else 
        return new Node()}
  removeLast(){ return this.list.pop()}
  append(item){ this.list.push(item) }
  removeStart(){ return this.list.shift() }

  // remove from end of q anything that is smaller than the current number
  removeSmallerThan(currentNumber){
    while (this.list && this.getLast().number <= currentNumber) {
      this.removeLast();
    }
  }

}


function s(i){
  return JSON.stringify(i);
}

function test(desc, actual, expected){
  const e = s(expected);
  const a = s(actual);
  console.log(
    a === e 
      ? `${desc} Passed!`
      : `${desc} Failed!\n  expected:${expected}\n  actual:${actual}`
  )
}

test(
  "Test case 1",
  maxSliding([1,3,-1,-3,5,3,6,7], 3),
  [3,3,5,5,6,7]
)

test(
  "Test case 2",
  maxSliding([1], 1),
  [1]
)

test(
  "Test case 3",
  maxSliding([1,-1], 1),
  [1,-1]
)

test(
  "Test case 4",
  maxSliding([9,11], 2),
  [11]
)

test(
  "Test case 5",
  maxSliding([4,-2], 2),
  [4]
)
```
