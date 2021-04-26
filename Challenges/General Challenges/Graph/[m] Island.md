```js
/**
 * @param {character[][]} grid
 * @return {number}
 
 Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

Example 1:
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1

Example 2:
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
 */
var numIslands = function(grid) {
    
    const addValidFourDirections = (i, j, q) => {
        if (i !== 0) q.push([i-1, j])  // add top
        if (i !== grid.length-1) q.push([i+1, j]) // add bottom 
        if (j !== 0) q.push([i, j-1]) // add left  
        if (j !== grid[0].length -1) q.push([i, j+1]) // add right 
    }
    
    if (!grid || grid.length === 0) return 0;

    // keep count of islands
    let islands = 0;
    const q = [];
    // Iterate through each grid
    grid.forEach((row, i) => {
        row.forEach((type, j) => {
            // check if visited
            if (type === '1'){
                type = 'v';
                islands += 1;
                // add all relevant four directions to look for connections
                addValidFourDirections(i,j,q);
            }
            // look in all possible directions for 1 and mark as visited
            while (q.length !== 0){
                const [x, y] = q.shift();
                if (grid[x][y] === '1'){
                    grid[x][y] = 'v';
                    addValidFourDirections(x,y,q);
                }
                    
            }
        })
    })
    return islands;
};
```