# Word Ladder

A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words such that:

- The first word in the sequence is `beginWord`.
- The last word in the sequence is `endWord`.
- Only one letter is different between each adjacent pair of words in the sequence.
- Every word in the sequence is in `wordList`.

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return *the **number of words** in the **shortest transformation sequence** from* `beginWord` *to* `endWord`*, or* `0` *if no such sequence exists.*

```
Example 1:

Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
Explanation: One shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog" with 5 words.
Example 2:

Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: 0
Explanation: The endWord "cog" is not in wordList, therefore there is no possible transformation.
 

Constraints:
1 <= beginWord.length <= 10
endWord.length == beginWord.length
1 <= wordList.length <= 5000
wordList[i].length == beginWord.length
beginWord, endWord, and wordList[i] consist of lowercase English letters.
beginWord != endWord
All the strings in wordList are unique.
```

# Code

<details><summary>Functional Approach</summary>
 
 ```js
 /**
 * https://github.com/frankhli843/algorithms/blob/master/Graph/Word%20Ladder.md
 * @param {string} beginWord
 * @param {string} endWord
 * @param {string[]} wordList
 * @return {number} the number of words from the wordList that form the chain
 * 
 * We will want to have a graph structure like so: 
 * "dog" -> "cog" -> "cob"
 */
function ladderLength(beginWord, endWord, wordList) {
  // handle case where it is the same word
  if (beginWord === endWord) 
    return 0;


  const wordSet = new Set([beginWord, ...wordList]); // add beginWord and remove duplicates 

  const nodes = {};
  // for each word generates the matachables
  wordSet.forEach(w => { 
    nodes[w] = {
      word: w, visited: false, distance: Infinity, 
      matchList: w.split("").map((x,i) => { 
        // i.e. ['do*', d*g', 'do*']
        const l = w.split("");
        l[i] = "*";
        return l.join("");
      })
    }
  })
  nodes[beginWord] = { 
    ...nodes[beginWord], 
    // we are only counting words in wordList 
    distance: wordList.includes(beginWord) ? 0 : 1
  }; 


  const q = [nodes[beginWord]]; // queue where we will do a bfs
  while (q.length > 0){
    const current = q.shift(); // first of list
    if (!current.visited){
      current.visited = true;
      // get the children matching current node 
      const children = Object.values(nodes)
        .map(c => { // "dog" matches with "bog" b/c they both have "*og" in their matchList
          const isMatch = current.matchList.some(
            m => c.matchList.includes(m) && !c.visited);
          if (isMatch) return c;
        })
        .filter(x => !!x);  // remove undefined
        
      // we will want to check each child if it matches our end goal otherwise add to queue
      for (let i = 0; i < children.length; i++){
        const child = children[i];
        if (child.word === endWord){   
          return current.distance + 1;
        }
        else {
          child.distance = current.distance + 1;
          q.push(child);
        }
      }
    }
  }
  return 0;
};
```


</details>


<details><summary>Object Oriented Approach</summary>
 
```js
/**
 * https://github.com/frankhli843/algorithms/blob/master/Graph/Word%20Ladder.md
 * @param {string} beginWord
 * @param {string} endWord
 * @param {string[]} wordList
 * @return {number}
 * 
 * We will want to have a graph structure like so: 
 * "dog" -> "cog" -> "cob"
 */
function ladderLength(beginWord, endWord, wordList) {
  const graph = new WordGraph();
  [...wordList, beginWord, endWord].forEach(word => {
    graph.add(word);
  })
  return graph.shortestWordsToLink(beginWord, endWord);
};


class Node {
  value; 
  matches; // array of matches
  distance; // number used to measure distances for graph traversals  
  visited; // bool used in graph tranversals whether something is visited
  /**
   * Let n be the number of characters in the word then this would run in O(n)
   */
  constructor(value){
    this.value = value;
    this.matches = [];
    this.distance = Infinity;
    this.visited = false;
    // we want a list of possible matches here so that we can compare later on.
    // "dog" and "cog" are matches and they will both have *cog in their matches map
    // for example "dog" has matches "*og", "d*g", and "do*"
    // for each letter in the word
    value.split("").forEach((l, i) => {
      // for each letter append the word with that letter replaced by *
      const wordList = value.split("");
      wordList[i] = "*";
      this.matches.push(wordList.join(""));
    })
  }
}
class WordGraph {
  nodes;  // Node[]
  edges;  // [Node, Node]
  constructor(){
    this.nodes = [];
    this.edges = [];
  }

  // returns the node
  add(word){
    if (!this.wordExists(word)){
      const node = new Node(word);
      // cycle through each existing node and add edges if there is a match
      this.nodes.forEach(currNode => { this._addEdges(node, currNode); });
      this.nodes.push(node); // add new node to the family :D
      return node;
    }
    return this.getNode(word);
  }

  wordExists(word){
    for (let i = 0; i < this.nodes.length; i++){
      if (this.nodes[i].value === word){
        return true;
      }
    }
    return false;
  }


  _addEdges(newNode, currentNode){
    // we take the list of matchables from our new node match them to the currentNode
    newNode.matches.forEach(nMatch => {
      currentNode.matches.forEach(cMatch => {  // if there is a match then we add 
        if (nMatch === cMatch) 
          this.edges.push([newNode, currentNode])
      })
    })
  }
  


  hasEdge(word1, word2){
    for (let i = 0; i < this.edges.length; i++){
      const a = this.edges[i][0];
      const b = this.edges[i][1];
      if ((a.value === word1 && b.value === word2) 
      || (b.value === word1 && a.value === word2)){
        return true;
      }
    }
    return false;
  }

  getNode(word){
     for (let i = 0; i < this.nodes.length; i++){
      if (this.nodes[i].value === word){
        return this.nodes[i];
      }
    }
    throw "could not find find node for " + word;
  }

  getNodeIndex(word){
    for (let i = 0; i < this.nodes.length; i++){
      if (this.nodes[i].value === word){
        return i;
      }
    }
   return -1;
  }

  // function will return a list of all nodes connected to the node with the given word
  nodeChildren(word){
    // a match on either side is fine since edges are not directional
    return this.edges.map(e => {
      if (e[0].value === word){ return e[1]}
      else if (e[1].value === word){ return e[0]}
    }).filter(node => node);
  }

  shortestWordsToLink(beginWord, endWord){
    console.log(`beginWord: ${beginWord}, endWord: ${endWord}`)
      // if beginWord and endWord are the same then return 0
      if (beginWord === endWord) return 0;
      
      // set all nodes to infinity distance 
      this.nodes.forEach(n => {
        n.distance = Infinity;
        n.visited = false;
      });

      const beginWordNode = this.getNode(beginWord);
      beginWordNode.distance = 1; // starts at one since we are counting words

      const q = [beginWordNode]; // start off with queue

      while (q.length !== 0){
        console.log(`  queue: [${q.map(a => a.value)}], starting at start of while loop again`)
        const currentNode = q.shift();
        console.log(`    currentNode:'${currentNode.value}'`)
        if (!currentNode.visited){
          console.log(`    node not yet visited so going to add its children`)
          currentNode.visited = true;
          const children = this.nodeChildren(currentNode.value);
          console.log(`    children: [${children.map(c => c.value)}]`)
          for (let i = 0; i < children.length; i++){
            const child = children[i];
            if (child.value === endWord){
              console.log(`Match found! distance ${currentNode.distance + 1}`)
              return currentNode.distance + 1;
            }
            else{
              console.log(`    Add ${child.value} to queue`)
              child.distance = currentNode.distance + 1;
              q.push(child);
            }
              
          }
        }
      }
      return 0; // nothing is found return 0
    }
}
```

</details>

<details><summary>Test Code</summary>
 
```js
function S(input){
  return JSON.stringify(input);
}

function test(desc, expected, actual){
  const e = S(expected);
  const a = S(actual);
  console.log(
    a === e 
      ? `\nTest '${desc}' passed!`
      : `\nTest '${desc}' failed\n  expected:${e}\n  actual:${a}`
  )
}

function runTestCases(){
  test("Testing the test", true, true)

  test(
    'beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]',
    5,
    ladderLength("hit", "cog", ["hot","dot","dog","lot","log","cog"])
  )
  test(
    'Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]',
    0,
    ladderLength("hit", "cog", ["hot","dot","dog","lot","log"])
  )
   test(
    'Input: "hot", "dog", ["hot","dog","cog","pot","dot"]',
    3,
    ladderLength("hot", "dog", ["hot","dog","cog","pot","dot"])
  )
}
runTestCases()
```

</details>
