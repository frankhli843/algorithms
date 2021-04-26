# Serialize and Deserialize Binary Tree

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

```
Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]
            2
root -> 1       5
            3 
                4 
```

```
Example 2:
Input: root = []
Output: []

Example 3:
Input: root = [1]
Output: [1]

Example 4:
Input: root = [1,2]
Output: [1,2]
```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `-1000 <= Node.val <= 1000`

# My Attempt

```js
/**
 Try to determine a formula to caluclate the index

                null
            b
                null
root -> a       
                e
            c 
                d 
 
[a,b,c,null,null,d,e]
 0 1 2. 3    4.  5 6
(a) node count: 1, index: 0, left: 0 + 1 = 1, right: 0 + 2 = 2
(b) node count: 2, index: 1, left: 1 + 2 = 3, right  1 + 3 = 4
(c) node count: 3, index: 2, left: 2 + 3 = 5, right  2 + 4 = 6

Let node count be x where the first node is root, the second is the left child, third is right child so on.

The index of a given node call it p can be calculated by x - 1.
The index of p left child can be calculated 2x - 1
The index of p right child can be calulated 2x 
 */

class BinaryTree {
  left; 
  right;
  value;
  constructor(value, left, right){
    this.value = value;
    this.left = left;
    this.right = right;
  }
}


function serialTree(tree, result = []){
  // base case: tree is null then just push self and done
  if (tree === null){
    result.push(null);
  }
  else {
    serialTree(tree.left, result);
    serialTree(tree.right, result);
  }
  return result;
}

```



