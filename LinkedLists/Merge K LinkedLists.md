# Prompt
```js
You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.
Merge all the linked-lists into one sorted linked-list and return it.

 

Example 1:
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
Example 2:

Input: lists = []
Output: []
Example 3:

Input: lists = [[]]
Output: []
 

Constraints:
k == lists.length
0 <= k <= 10^4
0 <= lists[i].length <= 500
-10^4 <= lists[i][j] <= 10^4
lists[i] is sorted in ascending order.
The sum of lists[i].length won't exceed 10^4.
```
# My Solution
```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
class ListNode {
   constructor(val, next){
     this.val = val===undefined ? 0 : val;
     this.next = next===undefined ? null : next;
   }
   toString(){
     return stringLinked(this);
   }
 }


/**
 * @param {ListNode[]} lists
 * @return {ListNode}
 */
function mergeKLists(lists) {
    // we have linkedlist of linkedlists i.e. a->null
    // BASE CASE -1: the list is empty just return an empty ListNode
    if (lists.length === 0)
        return new ListNode();
    // BASE CASE 0: there is one sorted linkedlist so just return itself
    else if (lists.length === 1){
        return lists[0];
    }
    // CASE 1: There is current and one list after it i.e. [a,b]
    else if (lists.length === 2){
        return mergeLists(lists[0], lists[1]);
    }
    // CASE 2: There are 2 or more after it i.e. a->b->...->z->null
    else {
        // we want to sort the current list when the rest are sorted
        return mergeLists(lists[0], lists.slice(1,lists.length));
    }
};

/**
 * function merges two linked lists
 * @param {ListNode[]} a
 * @param {ListNode[]} b
 * @return {ListNode}
 */
function mergeLists(a, b){
    // BASE CASE: at least one pointer is now null 
    if (!a) return b;
    else if (!b) return a;
    else if (a.val > b.val){  // b is smaller so find the next for it 
        b.next = mergeLists(a, b.next);
        return b
    }
    else {  // a is smaller so find next for it
        a.next = mergeLists(a.next, b);
        return a;
    }
}



function stringLinked(list){
  let result = `${list.val}`;
  let current = list;
  while (current.next){
    current = current.next;
    result += `->${current.val}`;
  }
  return result
}

testCase = [
  new ListNode(4, new ListNode(5, new ListNode(6))),  // 4->5->6
  new ListNode(1, new ListNode(2, new ListNode(3))) //1->2->3
];


function test(desc, expected, actual){
  const e = JSON.stringify(expected);
  const a = JSON.stringify(actual);
  if (a === e)
    console.log(`${desc} passed`)
  else{
    console.log(`${desc} failed\n expected:${e}\n actual:${a}`)
  }
}

test('simple test case 1', '1->2->3->4->5->6', stringLinked(mergeKLists(testCase)))
```

# Henrik
```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode[]} lists
 * @return {ListNode}
 */
function mergeKLists(lists) {
  let output = [], nodes = [...lists], current, smallest, smallestIndex = 0;
  let i = 0;
  while(true) {
    smallest = undefined;
    for(let j = 0, l = nodes.length; j < l; j++) {
      current = nodes[j]?.val;
      if(current) {
        if(!smallest) 
          smallest = current;
        if(current <= smallest) {
          smallest = current 
          smallestIndex = j;        
        }  
      }
    }     
    if(!nodes[smallestIndex]) break;
    output.push(nodes[smallestIndex].val);
    if(!!nodes[smallestIndex]) 
      nodes[smallestIndex] = nodes[smallestIndex].next;
    i++;
    console.log(nodes);
  }
  return output;
};

function ListNode(val, next) {
  this.val = (val===undefined ? 0 : val)
  this.next = (next===undefined ? null : next)
 }
 // node[[2], [3]]
let one1 = new ListNode(3);
let two1 = new ListNode(2, one1);
let node1 = new ListNode(1, two1);

let a2 = new ListNode(4);
let one2 = new ListNode(3, a2);
let two2 = new ListNode(2, one2);
let node2 = new ListNode(1, two2);

const input = [node1, node2];

console.log(mergeKLists(input));
```
