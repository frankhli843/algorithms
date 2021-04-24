# Trapping Rain Water
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.
```
Example 1:
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```
![image](../img/trappingRainWater1.png)

```
Example 2:
Input: height = [4,2,0,3,2,5]
Output: 9
```
 
```
Constraints:
n == height.length
0 <= n <= 3 * 104
0 <= height[i] <= 105
```


# Two pointer solution
```js
function trap (height) {
  let result = 0, leftMax= 0, rightMax = 0, l = 0, r = height.length - 1;

  while (l < r){
    if (height[l] < height[r]) {
      if (height[l] >= leftMax) leftMax = height[l];
      else result += leftMax - height[l];
      l++;
    } else {
      if (height[r] >= rightMax) rightMax = height[r];
      else result += rightMax - height[r];
      r--;
    }
  }
  
  return result;
};

```
