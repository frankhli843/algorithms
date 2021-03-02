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
  value;
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

  // use Dijkstra's Shortest Path First algorithm
  // Let n be the number of nodes
  // Let e be the number of edges
  // then this runs in O(elog(n))
  shortestWordsToLink(beginWord, endWord){
    // TODO implmenet this
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
    // we want a list of possible matches here
    // for example "dog" has matches "*og", "d*g", and "do*"
    word.split("").forEach((c, i) => {
      const splitWord = word.split("");
      splitWord[i] = "*";
      this.matches[splitWord.join("")] = true;
    })
  }
}

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

