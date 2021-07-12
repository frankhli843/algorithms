# Fruit Into Baskets
You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array fruits where fruits[i] is the type of fruit the ith tree produces.

You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:

You only have two baskets, and each basket can only hold a single type of fruit. There is no limit on the amount of fruit each basket can hold.
Starting from any tree of your choice, you must pick exactly one fruit from every tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.
Once you reach a tree with fruit that cannot fit in your baskets, you must stop.
Given the integer array fruits, return the maximum number of fruits you can pick.

Example 1:

Input: fruits = [1,2,1]
Output: 3
Explanation: We can pick from all 3 trees.
Example 2:

Input: fruits = [0,1,2,2]
Output: 3
Explanation: We can pick from trees [1,2,2].
If we had started at the first tree, we would only pick from trees [0,1].
Example 3:

Input: fruits = [1,2,3,2,2]
Output: 4
Explanation: We can pick from trees [2,3,2,2].
If we had started at the first tree, we would only pick from trees [1,2].
Example 4:

Input: fruits = [3,3,3,1,2,1,1,2,3,3,4]
Output: 5
Explanation: We can pick from trees [1,2,1,1,2].


Constraints:

1 <= fruits.length <= 105
0 <= fruits[i] < fruits.length

# Solution, 388 ms, faster than 32.26%
```js
/**
 * @param {number[]} fruits
 * @return {number}
 */
function totalFruit(fruits) {
    let left = 0, max = 0;
    const hash = {};
    
    fruits.forEach((fruitType, right) => { // iterate by right pointer
        hash[fruitType] = hash[fruitType] + 1 || 1; // add right fruit type to hash
        while (Object.keys(hash).length > 2){  // decrease left pointer until there are only 2 types of fruit
            const leftFruit = fruits[left];
           
            if (hash[leftFruit] > 0)
                hash[leftFruit] = hash[leftFruit] - 1;
            
             // if there is no fruits left then we remove that key
            if (hash[leftFruit] === 0)
                delete hash[leftFruit];
            
            // increment left pointer forward 
            left++;
        }
        // calculate the localMax
        max = Math.max(max, right - left + 1);
    })
    // return the max
    return max;
};
```

# Solution with Map, 164 ms, faster than 52.36%
```js
/**
 * @param {number[]} fruits
 * @return {number}
 */
function totalFruit(fruits) {
    
    let left = 0, max = 0;
    const hash = new Map();
    
    fruits.forEach((fruitType, right) => { // iterate by right pointer
        hash.set(fruitType, hash.get(fruitType) + 1 || 1);  // add right fruit type to hash
        while (hash.size > 2){  // decrease left pointer until there are only 2 types of fruit
           const leftFruit = fruits[left];
            if (hash.get(leftFruit) > 0)
                hash.set(leftFruit, hash.get(leftFruit) -1);
            
             // if there is no fruits left then we remove that key
            if (hash.get(leftFruit) === 0)
                hash.delete(leftFruit)
            
            // increment left pointer forward 
            left++;
        }
        // calculate the localMax
        max = Math.max(max, right - left + 1);
    })
    // return the max
    return max;
    
};
```