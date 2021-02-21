# 146. LRU Cache

Implement the LRUCache class:

LRUCache(int capacity) Initialize the LRU cache with positive size capacity.
int get(int key) Return the value of the key if the key exists, otherwise return -1.
void put(int key, int value) Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.
Follow up:
Could you do get and put in O(1) time complexity?

 

Example 1:
```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]

Output
[null, null, null, 1, null, -1, null, -1, 3, 4]
```

Explanation
```
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```
 

Constraints:
```
1 <= capacity <= 3000
0 <= key <= 3000
0 <= value <= 104
At most 3 * 104 calls will be made to get and put.
```

```js
class LRUCache {
    hash;
    list;
    capacity;
    /**
    * @param {number} capacity
    */
    constructor(capacity){
        this.hash = {};
        this.list = new LList();
        this.capacity = capacity;
    }
    
    /** 
     * @param {number} key
     * @return {number}
     */
    get(key){
        // if it does not exist then return -1
        if (!this.hash[key])
            return -1;
        // if it does exist 
        else {
            
            const node = this.hash[key];
            // move item to front of the list
            this.list.moveFront(node);
            // return the value
            return node.value;
        }
            
    }
    
    /** 
     * @param {number} key 
     * @param {number} value
     * @return {void}
     */
    put(key, value){
        if (!(key in this.hash)){  // if key does not exist 
            // create new LList node
            // make a new entry in hash
            this.hash[key] = this.list.add(key, value);
        }
        else {   // otherwise
            // update the value
            const node = this.hash[key];
            node.value = value;
            // move node to the front
            this.list.moveFront(node);
        }
           
        // if hash count is higher than capacity remove last node
        if (Object.keys(this.hash).length > this.capacity){
            const lastNode = this.list.removeLast();
            delete this.hash[lastNode.key];
        }
    }
    
}

class LList {
    parent;
    child;
    key;
    value;
    constructor(key, value){
        this.key = key;
        this.value = value;
    }
    
    // returns lList
    add(key, value){
        // no other entries
        if (!this.child)
            return this._addFirst(key, value);
        // has others in list
        else 
            return this._insertFront(key, value);
            
    }
    
    // returns lList
    _addFirst(key, value){
        const node = new LList(key, value);
        this.child = node;
        node.parent = this;
        return node;
    }
    
    // returns lList
    _insertFront(key, value, node = null){
        if (!node)
            node = new LList(key, value);
        // head -> 1, insert 2
        const origHeadChild = this.child;
        this.child = node;
        node.parent = this;
        node.child = origHeadChild;
        origHeadChild.parent = node;
        return node;
    }
    
    // return true if moved, false is already at front
    moveFront(node){
        // already front 
        if (!node.parent.parent)
            return false;
        // last
        else if (!node.child){
            this._moveFowardLast(node);
        }
        // somewhere in the middle
        else {
            this._moveForwardMiddle(node);
        }
        return true;
        
        
    }
    
    _moveFowardLast(node){
        node.parent.child = undefined;
        this._insertFront(node.key, node.value, node);
    }
    
    _moveForwardMiddle(node){
        // head -> s -> ... -> a -> node -> c -> ....
      
        const a = node.parent;
        const c = node.child;
        a.child = c;
        c.parent = a;
        
        const s = this.child;
        this.child = node; 
        node.parent = this;
        node.child = s;
        s.parent = node;
    }
    
    // returns the node that is removed from list
    removeLast(){
        let current = this;
        while (current.child.child){
            current = current.child;
        }
        const lastNode = current.child;
        current.child = undefined;
        return lastNode;
    }
    
    toS(inputString = ""){
            let current = this;
            while (current.child){
                current = current.child;
                inputString += `(${current.key},${current.value})->`
            }
        
        return inputString;
    }
    
}



/** 
 * Your LRUCache object will be instantiated and called as such:
 * var obj = new LRUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */

const test = (desc, expected, actual) => {
    const e= JSON.stringify(expected);
    const j = JSON.stringify(actual);
    
    if (e === j)
        console.log(`${desc} Test Passed! `)
    else
        console.log(`${desc} Test Failed\n  expected:${e}\n  actual:${j}`)
}



const tests = () => {
    test("Testing the test", true, true);
        console.log('============================================');
    let list = new LList();
    list._addFirst(1,1);
    test("_addFirst()", 1, list.child.key); // head -> 1

    list._insertFront(2,2);
    test("_insertFront(2,2)", '2->1', `${list.child.key}->${list.child.child.key}`); // head ->

    list = new LList();
    list.add(1,1);
    list.add(2,2);
    test("add()", '2->1', `${list.child.key}->${list.child.child.key}`);
    console.log('============================================');
    list = new LList();
    list.add(1,1);
    list.add(2,2); // 2 -> 1

    let two = list.child;
    let one = list.child.child;
    test("moveFoward() when already front", false, list.moveFront(two));
    console.log('============================================');
    list = new LList();
    list.add(1,1);
    list.add(2,2); // 2 -> 1

    two = list.child;
    one = list.child.child;
    list._moveFowardLast(one);
    test("moveFoward() last", 1, list.child.key);
    test("moveFoward() last child is undefined", undefined, list.child.child.child);
    console.log('============================================');
    list = new LList();
    list.add(1,1);
    list.add(2,2); 
    list.add(3,3);

    three = list.child;
    two = list.child.child;
    one = list.child.child.child;
    console.log(`${list.child.key}->${list.child.child.key}->${list.child.child.child.key}`)
    // 3->2->1
    list._moveForwardMiddle(two);
    test("_moveForwardMiddle", '2->3->1',  `${list.child.key}->${list.child.child.key}->${list.child.child.child.key}`);
    console.log('============================================');
    list = new LList();
    list.add(1,1);
    list.add(2,2); 
    list.add(3,3);
    console.log(`${list.child.key}->${list.child.child.key}->${list.child.child.child.key}`)
    const lastNode = list.removeLast();
    test("removeLast()", '3->2->undefined',  `${list.child.key}->${list.child.child.key}->${list.child.child.child}`);
    test("removeLast() last node", 1,  lastNode.key);
    console.log('============================================');
    let l = new LRUCache(2);
    list = l.list;
    l.put(1,1);
    l.put(2,2);
    l.put(3,3);
    test("put() '3->2->undefined'", '3->2->undefined',  `${list.child.key}->${list.child.child.key}->${list.child.child.child}`);
    // head -> 3 -> 2 
    test("put() 3 will remove the last", -1, l.get(1));
    l.put(3,33);
    test(`put() can replace value state: ${list.toS()}`, 33, l.get(3));
    console.log('============================================');
    l = new LRUCache(3);
    list = l.list;
    l.put(1,1);
    l.put(2,2);
    l.put(3,3);
    l.get(1);
    test(`get 1 should move to front: ${list.toS()}`, 1, list.child.key);
}

const runningTests = () => {
    let l = new LRUCache(2);
    let list = l.list;
    l.put(1,1);
    console.log(`l.put(1,1); ${list.toS()}`);
    l.put(2,2);
    console.log(`l.put(2,2); ${list.toS()}`);
    l.get(1);
    console.log(`l.get(1);; ${list.toS()}`);
    l.put(3,3);
    console.log(`l.put(3,3); ${list.toS()}`)
    l.put(2);
    console.log(`l.put(2); ${list.toS()}`)
    l.put(4,4);
    console.log(`l.put(4,4); ${list.toS()}`)
    l.get(1);
    console.log(`l.get(1);; ${list.toS()}`)
    l.get(3);
    console.log(`l.get(3); ${list.toS()}`)
    l.get(4);
    console.log(`l.get(4); ${list.toS()}`)
}
```