# LFUCache
Design and implement a data structure for a Least Frequently Used (LFU) cache.

Implement the LFUCache class:

LFUCache(int capacity) Initializes the object with the capacity of the data structure.
int get(int key) Gets the value of the key if the key exists in the cache. Otherwise, returns -1.
void put(int key, int value) Update the value of the key if present, or inserts the key if not already present. When the cache reaches its capacity, it should invalidate and remove the least frequently used key before inserting a new item. For this problem, when there is a tie (i.e., two or more keys with the same frequency), the least recently used key would be invalidated.
To determine the least frequently used key, a use counter is maintained for each key in the cache. The key with the smallest use counter is the least frequently used key.

When a key is first inserted into the cache, its use counter is set to 1 (due to the put operation). The use counter for a key in the cache is incremented either a get or put operation is called on it.

 
```
Example 1:

Input
["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

Explanation
// cnt(x) = the use counter for key x
// cache=[] will show the last used order for tiebreakers (leftmost element is  most recent)
LFUCache lfu = new LFUCache(2);
lfu.put(1, 1);   // cache=[1,_], cnt(1)=1
lfu.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
lfu.get(1);      // return 1
                 // cache=[1,2], cnt(2)=1, cnt(1)=2
lfu.put(3, 3);   // 2 is the LFU key because cnt(2)=1 is the smallest, invalidate 2.
                 // cache=[3,1], cnt(3)=1, cnt(1)=2
lfu.get(2);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,1], cnt(3)=2, cnt(1)=2
lfu.put(4, 4);   // Both 1 and 3 have the same cnt, but 1 is LRU, invalidate 1.
                 // cache=[4,3], cnt(4)=1, cnt(3)=2
lfu.get(1);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,4], cnt(4)=1, cnt(3)=3
lfu.get(4);      // return 4
                 // cache=[3,4], cnt(4)=2, cnt(3)=3
 

Constraints:

0 <= capacity, key, value <= 104
At most 105 calls will be made to get and put.

```


```
/** 
 * Your LFUCache object will be instantiated and called as such:
 * var obj = new LFUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */
 ```

# Code

<details><summary>Click to expand</summary>
 
 ```js
  /** 
 
    The plan here is to keep a hashmap for each key and value to make sure get is O(1).
    
    For deciding which value to remove we keep a hashmap with count as key so that 
    for a given interaction count there is an array for each node. Each array which holds
    nodes with a given amount of iteration will act as a queue where we append later additions
    and remove earlier ones since they will be older.
    i.e. 
    {
        1: [{key: 'favorite food', value: 'chicken', count: 1}, {key: 'computer', value: 'macbook', count: 1}],   
        2: [{key: 'favorite drink', value: 'water', count: 2}]
    }
    So our algorithm will get the lowest count array in this it is 1
    [{key: 'favorite food', value: 'chicken', count: 1}, {key: 'computer', value: 'macbook', count: 1}]
    then it will remove the first of the list since it would be the oldest
    
    @member countHash {[retrieveCount: number]: Node[] }
    @member hash {[key: string]: Node } for quick retrival
    Node will be { date: string, key: string, value: any, count: number }
    
    
 */

 /** 
 
    The plan here is to keep a hashmap for each key and value to make sure get is O(1).
    
    For deciding which value to remove we keep a hashmap with count as key so that 
    for a given interaction count there is an array for each node. Each array which holds
    nodes with a given amount of iteration will act as a queue where we append later additions
    and remove earlier ones since they will be older.
    i.e. 
    {
        1: [{key: 'favorite food', value: 'chicken', count: 1}, {key: 'computer', value: 'macbook', count: 1}],   
        2: [{key: 'favorite drink', value: 'water', count: 2}]
    }
    So our algorithm will get the lowest count array in this it is 1
    [{key: 'favorite food', value: 'chicken', count: 1}, {key: 'computer', value: 'macbook', count: 1}]
    then it will remove the first of the list since it would be the oldest
    
    @member countHash {[retrieveCount: number]: Node[] }
    @member hash {[key: string]: Node } for quick retrival
    Node will be { date: string, key: string, value: any, count: number }
    
    
 */

class LFUCache {
    capacity;
    hash; 
    countHash;
    count;
    
    constructor(capacity){
        if (capacity <= 0) throw "capacity must be at least 1 or larger"
        this.capacity = capacity; 
        this.countHash = {};  // {capacity: Node[]}
        this.hash = {}; // {key}
        this.count = 0;
    }
    put(key, value){
        // case 1: new value
        if (!(key in this.hash)){ 
            // case 1.1: over capacity, remove the least used
            if (this.capacity === this.count){ // here we have a list of each count
                const leastUsedCount = parseInt(Object.keys(this.countHash).sort((a,b) => a - b)[0]);
                let keyToDelete;
                // if there is only 1 then we remove it 
                if (this.countHash[leastUsedCount].length === 1){
                    keyToDelete = this.countHash[leastUsedCount][0].key;
                    this.countHash[leastUsedCount] = [];
                }
                else { // otherwise since the array is acting as a queue we we know we want to remove the oldest which will be at the beginning of the list
                    const [firstNode, ...rest] = this.countHash[leastUsedCount];
                    keyToDelete = firstNode.key;
                    this.countHash[leastUsedCount] = rest;
                }
                delete this.hash[keyToDelete];  // remove from hash
                this.count--;
            }
            
            // create new node and add it 
            const node = { key: key, value: value, count: 1};
            this.hash[key] = node;
            if (1 in this.countHash) this.countHash[1].push(node)
            else this.countHash[1] = [node];
            this.count++;
        }
        // case 2: existing value
        else {
            // since key exists we will get the original node
            const originalNode = {...this.hash[key] };
            this.hash[key].count++;
            // we will increment the count and insert it into the list. If it does not exist we will create it
            if (!(this.hash[key].count in this.countHash)) this.countHash[this.hash[key].count] = [this.hash[key]];
            else this.countHash[this.hash[key].count].push(this.hash[key]);
            
            // we will filter out the original node from the current count array. If that makes the array empty then we will delete that key from countHash
            if (this.countHash[originalNode.count].length === 1) delete this.countHash[originalNode.count]
            else this.countHash[originalNode.count] = this.countHash[originalNode.count].filter(x => x.key !== originalNode.key);
        }
           
    }
    
    
    
    /** 
     * @param {number} key
     * @return {number}
     */
    get(key){}
    
   
}


/** 
 * Your LFUCache object will be instantiated and called as such:
 * var obj = new LFUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */

//  let x = new LFUCache(1);
//  x.put(1,1)
//  x.put(2,2)


```

</details>
