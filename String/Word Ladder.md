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

# Tests
```
/**
 * @param {string} beginWord
 * @param {string} endWord
 * @param {string[]} wordList
 * @return {number}
 */
function ladderLength(beginWord, endWord, wordList) {
    
};

function S(input){
  return JSON.stringify(input);
}

function test(desc, expected, actual){
  const e = S(expected);
  const a = S(actual);
  console.log(
    a === e 
      ? `\nTest '${desc}' passed!`
      : `\nTest '${desc}' failed\n  expected:${e}\n  expected:${a}`
  )
}

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
```

