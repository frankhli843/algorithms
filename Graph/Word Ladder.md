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
  // TODO think of a way to overcome cycles
  const graph = new WordGraph();
  // add words in wordlist to graph
  wordList.forEach(word => {
    graph.add(word);
  })
  return graph.shortestWordsToLink(beginWord, endWord);
};


class Node {
  value; 
  matches; // array of matches
  /**
   * Let n be the number of characters in the word then this would run in O(n)
   */
  constructor(value){
    this.value = value;
    this.matches = [];
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
    // if beginWord and endWord are the same then return 0
    if (beginWord === endWord){
      return 0;
    }

    // add begin word
    const beginNode = this.add(beginWord);
    // a queue where first in and first out order
    const q = []; 
    // nothing have been visited yet
    const visited = this.nodes.map(n => false);
    // set distance to all be infinity
    const distance = this.nodes.map(n => {
      if (n.value === beginWord) return 0;  // distance 0 to itself
      else return Infinity; // we don't know the distance yet
    })
    // beginWord is new so first add it
    q.push(beginNode)
    while(q.length !== 0){ // while q is not empty)
      const currentNode = q.shift(); // pop first element in array
      const nodeIndex = this.getNodeIndex(currentNode.value);
      if (!visited[nodeIndex]){ // only want to look at unvisited
        visited[nodeIndex] = true; // we have now visited it
        // get all nodes connected to this node to add in what to visit next
        const nodeChildren = this.nodeChildren(currentNode.value);
        // for all connected edges we want to check if we have reached out goal
        for (let i = 0; i < nodeChildren.length; i++){
          const childNode = nodeChildren[i]; 
          const childIndex = this.getNodeIndex(childNode.value);
          // we increase the distance by 1 in relation to our current node for children
          // consider for example when we are at our first node the distance is 0
          // for its children then we would want the distance to be 1 and their children 2 etc
          distance[childIndex] = distance[nodeIndex] + 1;
          // if currentNode word matches then we have our match 
          if (childNode.value === endWord){
            // since what we want is number of words not number of edges we add 1 to include the first word
            return distance[childIndex] + 1; 
          }
          // otherwise add this node to the queue
          else {
            q.push(childNode); // adds it to the end of the queue
          }
            
        }
      }
    }
     // no link is found return 0;
      return 0;
  }
  
}


// =============== TESTING CODE =============

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

  const n = new Node("dog");
  test(
    "Testing Node('dog') for the matches list: ['*og', 'd*g', 'do*']", 
    ['*og', 'd*g', 'do*'], 
    n.matches
  );

  let w = new WordGraph();
  w.add("cog");
  w.add("dog");
  test(
    "WordGraph add for cog and dog results in an edge to each other",
    true,
    w.hasEdge("cog", "dog")
  )


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
}
runTestCases()
```

