# Word Break II
Given a string s and a dictionary of strings wordDict, add spaces in s to construct a sentence where each word is a valid dictionary word. Return all such possible sentences in any order.

Note that the same word in the dictionary may be reused multiple times in the segmentation.

 
```
Example 1:
Input: s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]
Output: ["cats and dog","cat sand dog"]
Example 2:

Input: s = "pineapplepenapple", wordDict = ["apple","pen","applepen","pine","pineapple"]
Output: ["pine apple pen apple","pineapple pen apple","pine applepen apple"]
Explanation: Note that you are allowed to reuse a dictionary word.

Example 3:
Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
Output: []
```
 

Constraints:
1 <= s.length <= 20
1 <= wordDict.length <= 1000
1 <= wordDict[i].length <= 10
s and wordDict[i] consist of only lowercase English letters.
All the strings of wordDict are unique.

# Code 
```js
const debugMode = false;

/**
 * take a string and then break them up so that the entire input is 
 * composed of valid words for wordDict
 * 
 * Let n length of s 
 * Let p be the lenght of wordDict  
 */
const wordBreak = function(s, wordDict) {
 return addValidWordBreaks(s, wordDict);
};

// mutates resultList, each call we deal with a smaller sublist until we 
const addValidWordBreaks = (s, wordDict, resultList=[], builtSentenceList = []) => {
  debugMode && console.log(`s: '${s}', resultList: [${resultList}], buildSentenceList: [${builtSentenceList}]`)
  // BASE CASE: s is empty or all that remains is a word which means we are at the end so push to resultList
  if (s === "" || s in wordDict){
    resultList.unshift(builtSentenceList.join(" "))
    builtSentenceList.length = 0;
  }
  // CASE 2: there is still words left in s so we want to keep iterating forward 
  else {
      wordDict.forEach(word => {
        const nextBranches = [];
        if (s.startsWith(word)){
          builtSentenceList.push(word);
          addValidWordBreaks(s.slice(word.length), wordDict, resultList, builtSentenceList)
        }
      })
    }
  return resultList;
}

const S = (i) => {return JSON.stringify(i) };
const t = (desc, exp, act) => {
  console.log(S(exp) === S(act) 
    ? `Test passed!\n ${desc}`
    : `Test failed! :( \n  ${desc}\n  expected:${S(exp)}\n  actual:${S(act)}`
    )
}

t('testing the test', true, true)
t(
  'Input: s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]',["cats and dog","cat sand dog"], 
  wordBreak("catsanddog", ["cat","cats","and","sand","dog"]),
  ["cats and dog","cat sand dog"]
)

```
