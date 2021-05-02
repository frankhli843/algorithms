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

<details><summary>My original solution: hashmap, hashmap, list 316ms</summary>
 
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

class LFUCache {
    capacity;
    hash; 
    countHash;
    count;
    
    constructor(capacity){
        this.capacity = capacity; 
        this.countHash = new Map();  // {capacity: Node[]}
        this.hash = new Map(); // {key}
        this.count = 0;
    }
    put(key, value){
        // no capacity so we don't need anything
        if (this.capacity === 0) return;
        
        // case 1: new value
        if (!(key in this.hash)){ 
            // case 1.1: over capacity, remove the least used
            if (this.capacity === this.count){ // here we have a list of each count
                const leastUsedCount = Object.keys(this.countHash).sort((a,b) => a - b)[0];
                let keyToDelete;
                // if there is only 1 then we remove it 
                if (this.countHash[leastUsedCount].length === 1){
                    keyToDelete = this.countHash[leastUsedCount][0].key;
                    delete this.countHash[leastUsedCount];
                }
                else { // otherwise since the array is acting as a queue we we know we want to remove the oldest which will be at the beginning of the list
                    keyToDelete = this.countHash[leastUsedCount].shift().key;
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
            const originalCount = this.hash[key].count;
            this.hash[key].count++;
            this.hash[key].value = value;
            // we will increment the count and insert it into the list. If it does not exist we will create it
            if (!(this.hash[key].count in this.countHash)) this.countHash[this.hash[key].count] = [this.hash[key]];
            else this.countHash[this.hash[key].count].push(this.hash[key]);
            
            // we will filter out the original node from the current count array. If that makes the array empty then we will delete that key from countHash
            if (this.countHash[originalCount].length === 1) delete this.countHash[originalCount]
            else this.countHash[originalCount] = this.countHash[originalCount].filter(x => x.key !== key);
        }
           
    }
    
    
    
    /** 
     * @param {number} key
     * @return {number}
     */
    get(key){
        if (key in this.hash){
            // since key exists we will get the original node
           const originalCount = this.hash[key].count;
            this.hash[key].count++;
            // we will increment the count and insert it into the list. If it does not exist we will create it
            if (!(this.hash[key].count in this.countHash)) this.countHash[this.hash[key].count] = [this.hash[key]];
            else this.countHash[this.hash[key].count].push(this.hash[key]);

            // we will filter out the original node from the current count array. If that makes the array empty then we will delete that key from countHash
            if (this.countHash[originalCount].length === 1) delete this.countHash[originalCount]
            else this.countHash[originalCount] = this.countHash[originalCount].filter(x => x.key !== key);
            return this.hash[key].value;
        }
        return -1
    }
    
   
}
/** 
 * Your LFUCache object will be instantiated and called as such:
 * var obj = new LFUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */
```

</details>

<details><summary>second iteration: hashmap, hashmap, double linked list 236ms</summary>
 
 ```js
   /** 
  In this iteration I replaced a regular list with a linked list to remove the O(n) complexity of removing the first from the list
 */
 class LFUCache {
    capacity;
    hash; 
    countHash;
    count;
    
    constructor(capacity){
        // console.log(`l = new LFUCache(${capacity})`)
        this.capacity = capacity; 
        this.countHash = {};  // {capacity: Node[]}
        this.hash = {}; // {first: Node, last: Node}
        this.count = 0;
    }
    put(key, value){
        // console.log(`l.put(${key}, ${value})`)
        // no capacity so we don't need anything
        if (this.capacity === 0) return;
        
        // case 1: new value
        if (!(key in this.hash)){ 
            // case 1.1: over capacity, remove the least used
            if (this.capacity === this.count){ // here we have a list of each count
                const leastUsedCount = Object.keys(this.countHash).sort((a,b) => a - b)[0];  // get the smallest count
                const keyToDelete = this.countHash[leastUsedCount].first.key;
                this.countHash[leastUsedCount].first = this.countHash[leastUsedCount].first.next;
                if (this.countHash[leastUsedCount].first) this.countHash[leastUsedCount].first.parent = null;       
                // if there is now nothing pointing at the given count then we can remove this.countHash[leastUsedCount]
                if (!this.countHash[leastUsedCount].first) delete this.countHash[leastUsedCount];
                delete this.hash[keyToDelete];  // remove from hash
                this.count--;
            }
            
            // create new node and add it 
            this.hash[key] = { key: key, value: value, count: 1, next: null, parent: null};
            // case 1.21: count of 1 already exists
            if (1 in this.countHash) { // change the last pointer and append it to the linkedlist chain
                const secondLast = this.countHash[1].last;
                secondLast.next = this.hash[key];
                this.hash[key].parent = secondLast;
                this.countHash[1].last = this.hash[key];
            }
            // case 1.22: create a new count of 1 chain
            else 
                this.countHash[1] = { first: this.hash[key], last: this.hash[key] }; 
    
            this.count++;
        }
        // case 2: existing value
        else {
            this.hash[key].value = value;
            this.incrementCount(key);
        }
    }
    
    
    
    /** 
     * @param {number} key
     * @return {number}
     */
    get(key){
        // console.log(`l.get(${key})`)
        if (key in this.hash){
            this.incrementCount(key);
            return this.hash[key].value;
        }
        return -1
    }
    
    incrementCount(key){
        const originalNode = {...this.hash[key]};
        const originalCount = originalNode.count;
        this.hash[key].count++;
        const newCount = this.hash[key].count;
        

        // Adding into new count list
        if (!(newCount in this.countHash)) { // count does not exist right now
            this.countHash[newCount] = {first: this.hash[key], last: this.hash[key]};
        }
        else { // already exists then we insert it into the list
            const secondLast = this.countHash[newCount].last;
            this.countHash[newCount].last = this.hash[key];
            this.hash[key].parent = secondLast;
            secondLast.next = this.hash[key];
        }

        // we will filter out the original node from the original count linkedlist 
        // case 1: if the linkedlist has the node as first and last then its the only node so we remove the list all together
        if (this.countHash[originalCount].first.key === key && this.countHash[originalCount].last.key === key) 
            delete this.countHash[originalCount];
        // case 2: otherwise we need to remove it from the linkedlist
        else {  
            // case 2.1: first of the list
            if (this.countHash[originalCount].first.key === key) {
                this.countHash[originalCount].first = originalNode.next;
                originalNode.next.previous = null;
            } 
            // case 2.2: at the end of the list
            else if (this.countHash[originalCount].last.key === key) {
                originalNode.parent.next = null;
                this.countHash[originalCount].last = originalNode.parent;
            }
            // case 2.3: somewhere in the middle
            else {
                originalNode.parent.next = originalNode.next;
                originalNode.next.parent = originalNode.parent;
            }
        } 
    }
   
}
```

</details>

<details><summary>Fastest on record: 188ms</summary>
 
 ```js
 /**
 * @param {number} capacity
 */
var LFUCache = function(capacity) {
  this.keyToFreqMap = new Map();
  this.freqToKeyMap = new Map();
  this.keyToValMap = new Map();
  this.capacity = capacity;
  this.leastFreq = 0;

};

/** 
 * @param {number} key
 * @return {number}
 */
LFUCache.prototype.get = function(key) {
  if (this.keyToValMap.has(key)) {
    this.leastFreq = freqAddOne(this.keyToFreqMap, this.freqToKeyMap, key, this.leastFreq);
    return this.keyToValMap.get(key);
  } else {
    return -1;
  }
};

/** 
 * @param {number} key 
 * @param {number} value
 * @return {void}
 */
LFUCache.prototype.put = function(key, value) {
  if (this.capacity > 0) {
    // pop out least frequency used
    if (this.keyToValMap.size >= this.capacity && !this.keyToValMap.has(key)) {
      // Step 1: figure which one is the least frequenty used
      var leastFreqSet = this.freqToKeyMap.get(this.leastFreq)
      // Since set is inserted according insertion order, the head of the set must be the least recently used item
      var leastUsedKey = leastFreqSet.values().next().value;
      
      // remove it from all our maps
      leastFreqSet.delete(leastUsedKey);
      if (leastFreqSet.size === 0) {
        this.freqToKeyMap.delete(this.leastFreq);
        // Since the last entry on the leastFreq set is removed, so the next leastFreq will be +1 from current
        this.leastFreq++;
      }
      this.keyToFreqMap.delete(leastUsedKey);
      this.keyToValMap.delete(leastUsedKey);
    }
    // We haven't seen this before, so leastFreq will be set back to 1
    if (!this.keyToValMap.has(key)) {
      this.leastFreq = 1;
    }
    // update our leastFreq count
    this.leastFreq = freqAddOne(this.keyToFreqMap, this.freqToKeyMap, key, this.leastFreq);
    this.keyToValMap.set(key, value);
  }
};

function freqAddOne(keyToFreqMap, freqToKeyMap, key, leastFreq) {
  // Add this key to our cache
  // update keyToFreqMap
  var freq = keyToFreqMap.get(key) + 1 || 1;
  keyToFreqMap.set(key, freq);

  // update freqToKeyMap
  if (freq > 1) {
    var oldFreq = freq - 1;
    var oldFreqSet = freqToKeyMap.get(oldFreq);
    oldFreqSet.delete(key);
    if (oldFreqSet.size === 0) {
      freqToKeyMap.delete(oldFreq)
      if (oldFreq === leastFreq) leastFreq++;
    }
  }
  var set = freqToKeyMap.get(freq) || new Set();
  set.add(key);
  freqToKeyMap.set(freq, set);
  return leastFreq;
}
```
