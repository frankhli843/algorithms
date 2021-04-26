# Prompt
Given an array of points where points[i] = [xi, yi] represents a point on the X-Y plane and an integer k, return the k closest points to the origin (0, 0).

The distance between two points on the X-Y plane is the Euclidean distance (i.e, √(x1 - x2)2 + (y1 - y2)2).

You may return the answer in any order. The answer is guaranteed to be unique (except for the order that it is in).

## Example
```
Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation:
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].

Example 2:
Input: points = [[3,3],[5,-1],[-2,4]], k = 2
Output: [[3,3],[-2,4]]
Explanation: The answer [[-2,4],[3,3]] would also be accepted.
```
<details><summary>My attempt</summary>

```js
/**
 * @param {number[][]} points
 * @param {number} k
 * @return {number[][]}
 let n be number of points given in input points
 Time complexity: O(n) since we iterate through points once to collect by floored distance 
 and then we sort and collect k amount
 */
function kClosest(points, k){
    if (points.length === 0 || k === 0)
        return [];
    
    // we want k amount of such distances ordered by the closest coordinates
    // since we want the closest we will need to look through the whole list
    // we set a hashmap {distance: Coordinates[]} 
    const mapDistance = new Map();
    // for each point in points there is a distance from [0,0]
    points.forEach(point => {
        const distance = Math.sqrt((point[0])*(point[0]) + (point[1])*(point[1]));
        const floorDistance = Math.floor(distance);
        const pointValue = [...point, distance]
        if (mapDistance.has(floorDistance)) 
            mapDistance.get(floorDistance).push(pointValue);
        else 
            mapDistance.set(floorDistance, [pointValue]);
    })
    // iterate through the hashmap in and append to result array until done
    const result = [];
    // iterate through the map in ascending order, our map looks like 
    // { 1: [[x,y,distance]], ... }
    [...mapDistance.keys()].sort((a,b) => a - b).some(floorDistance => {
        // for each distance we have [x, y, distance] we short is ascending according to distance
        const sortedDistance = mapDistance.get(floorDistance).sort((a, b) => a[2] - b[2]);
        const collectedEnough = sortedDistance.some(point => {
            if (result.length === k) return true;
            result.push([point[0], point[1]]);
        })
        if (collectedEnough) return true;
    })
    return result;
}
```

</details>

# Lessons learned 
- Dealing with numbers make sure to be aware of float values
