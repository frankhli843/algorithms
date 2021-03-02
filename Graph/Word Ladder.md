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
    wordList.forEach(word => { graph.addWord(word); })
    return graph.shortestWordsToLink(beginWord, endWord);
};

class WordGraph {
  nodes;
  edges;
  constructor(){
    this.nodes = {};
    this.edges = {};
  }

  /**
   * Let n be the number of nodes then this runs in O(n) since it needs to check
   * every node for a possible match
   */
  addWord(word){
    // If no node yet then we will add it
    if (!this.nodes[word]){
      const newNode = new Node(word);
    
      // cycle through each existing node and add edges if there is a match
      const nodeList = Object.keys(this.nodes);
      for (let i = 0; i < nodeList.length; i += 1){
        this._addEdges(newNode, this.nodes[nodeList[i]]);
      }

      this.nodes[word] = new Node(word);
  
    }
  }

  /**
  * This takes two nodes and adds an edge between them i.e. { "dog-cog": true }
  */
  _addEdges(newNode, currentNode){
    
    const newNodeMatchesKeys = Object.keys(newNode.matches);
    // we take the list of matchables from our new node match them to the currentNode
    for (let i = 0; i < newNodeMatchesKeys.length-1; i += 1){
      const currMatch = newNodeMatchesKeys[i];
      if (currentNode.matches[currMatch]){ // if there is a match then we add 
        // i.e. "dog" and "cog" matches then edges will have { "dog-cog"}
        // the order won't matter because we will have a helper that checks both
        this.edges[`${newNode.word}-${currentNode.word}`] = true;
        return;
      }
    }
  }

  /**
    *  This checks when given two words whether an edge exists for them
   * This runs in O(1) time since the edges are stored in a hashmap
   */
  hasEdge(word1, word2){
    const a = this.nodes[word1];
    const b = this.nodes[word2];
    if (!a || !b){
      return false;
    }
    if (this.edges[`${a.word}-${b.word}`] || this.edges[`${b.word}-${a.word}`])
      return true;
    return false;
  }

 
  /**
  use Dijkstra's Shortest Path First algorithm
  Let n be the number of nodes
  Let e be the number of edges
  then this runs in O(elog(n))
   */
  shortestWordsToLink(beginWord, endWord){
    // Step 1: Mark all nodes unvisited. Create a set of all the unvisited nodes called the unvisited set.
    // Step 2: Assign to every node a tentative distance value: set it to zero for our initial node and to infinity for all other nodes. Set the initial node as current.[16]
    // Step 3: For the current node, consider all of its unvisited neighbors and calculate their tentative distances through the current node. Compare the newly calculated tentative distance to the current assigned value and assign the smaller one. For example, if the current node A is marked with a distance of 6, and the edge connecting it with a neighbor B has length 2, then the distance to B through A will be 6 + 2 = 8. If B was previously marked with a distance greater than 8 then change it to 8. Otherwise, the current value will be kept.
    // Step 4: When we are done considering all of the unvisited neighbors of the current node, mark the current node as visited and remove it from the unvisited set. A visited node will never be checked again.
    // Step 5: If the destination node has been marked visited (when planning a route between two specific nodes) or if the smallest tentative distance among the nodes in the unvisited set is infinity (when planning a complete traversal; occurs when there is no connection between the initial node and remaining unvisited nodes), then stop. The algorithm has finished.
    // Step 6: Otherwise, select the unvisited node that is marked with the smallest tentative distance, set it as the new "current node", and go back to step 3.
    return 0;
  }
}

class Node {
  word;
  matches;
  /**
   * Let n be the number of characters in the word then this would run in O(n)
   */
  constructor(word){
    this.word = word;
    this.matches = {};
    // we want a list of possible matches here so that we can compare later on.
    // "dog" and "cog" are matches and they will both have *cog in their matches map
    // for example "dog" has matches "*og", "d*g", and "do*"
    word.split("").forEach((c, i) => {
      // for each letter append the word with that letter replaced by *
      const splitWord = word.split("");
      splitWord[i] = "*";
      this.matches[splitWord.join("")] = true;
    })
  }
}

// ==================================== TESTING CODE ==================================================

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
    Object.keys(n.matches)
  );

  let w = new WordGraph();
  w.addWord("cog");
  w.addWord("dog");
  console.log(w.edges)
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

