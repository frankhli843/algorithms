# Prompt
```
Given a string s which consists of lowercase or uppercase letters, return the length of the longest palindrome that can be built with those letters.

Letters are case sensitive, for example, "Aa" is not considered a palindrome here.

 

Example 1:

Input: s = "abccccdd"
Output: 7
Explanation:
One longest palindrome that can be built is "dccaccd", whose length is 7.
Example 2:

Input: s = "a"
Output: 1
Example 3:

Input: s = "bb"
Output: 2
```
# Code
```js
function test(desc, expected, actual){
	$('#main').append(
  	s(expected) === s(actual)
    	? `Test ${desc} passed!<br /><br />`
      : `Test ${desc} failed!<br />------expected: ${s(expected)}<br />------actual: ${s(actual)}<br /><br />`
  );
}

test("testing the test", true, false)
test("babad", longestPalindrome("babad"), "bab")
test("cbbd", longestPalindrome("cbbd"), "bb")
test("a", longestPalindrome("a"), "a")
test("ac", longestPalindrome("ac"), "a")

// ---------------------- your code below ----------------------------------

function longestPalindrome(s) {
    // case 0: check for invalid cases
    if (!s || s.length < 1)
        return 0;
    // case 1: check for the simpliest case
     if (s.length === 1)
         return 1
        
    // track the start index and end index for the longest found
    let start = 0;
    let end = 0;
    let max = 0;  // tracks the longest length found so far
    // for each letter in s we will iterate through
    for (let i = 0; i < s.length; i++){
        // case 1: our palidrome starts at the center as a single letter
        const len1 = expandAroundCenter(s, i, i);
        // case 2: our palidrome is two letters so we try the current one and the one afterwards
        const len2 = expandAroundCenter(s, i, i+1);
        const localMax =Math.max(len1, len2); 
        // if either of these two lengths is longer than our current max we update it
        
        if (max < localMax){
     
                //  0 1 2 3 4
                //  a b c b a
                //  ^   ^i  ^
                // length of 5; Start needs to be at 0. 2 - 2 = 0 so i - (len - 1)/2
                // End needs to be at 4. 2 + 2 = 4. So i + Math.floor(len/2) 
                //  0 1 2 3 
                //  a b b a
                //  ^ ^i  ^
                // length of 4; Start needs to be at 0. 1 - Math.floor((len - 1)/2) = 0 so i - len-1/2
                // End needs to be 3. 1 + 2 = 3 so i + len/2
                start =  i - Math.floor((localMax - 1)/2);
                end = i + Math.floor(localMax/2);
                max = localMax;
        }
           
    }
    return s.slice(start, end + 1)
};

function expandAroundCenter(s, leftIndex, rightIndex){
    let left = leftIndex;
    let right = rightIndex;
    // we loop out while it is still valid checking each time if the letters match
    while (left >= 0 && right < s.length && s[left] === s[right]){
        left--;
        right++;
    }
    // return the distance between the resulting indexes
    return right - left - 1;
    
}
```