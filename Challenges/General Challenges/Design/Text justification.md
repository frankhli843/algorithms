# Text Justification
- Given an array of words and a width maxWidth, format the text such that each line has exactly maxWidth characters and is fully (left and right) justified.
- You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces ' ' when necessary so that each line has exactly maxWidth characters.
- Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line do not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.
- For the last line of text, it should be left justified and no extra space is inserted between words.
- Note:
    - A word is defined as a character sequence consisting of non-space characters only.
    - Each word's length is guaranteed to be greater than 0 and not exceed maxWidth.
    - The input array words contains at least one word.

## Example 1:
```
Input: words = ["This", "is", "an", "example", "of", "text", "justification."], maxWidth = 16
Output:
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```

## Example 2:
```
Input: words = ["What","must","be","acknowledgment","shall","be"], maxWidth = 16
Output:
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]
Explanation: Note that the last line is "shall be    " instead of "shall     be", because the last line must be left-justified instead of fully-justified.
Note that the second line is also left-justified because it contains only one word.
```

## Example 3:
```
Input: words = ["Science","is","what","we","understand","well","enough","to","explain","to","a","computer.","Art","is","everything","else","we","do"], maxWidth = 20
Output:
[
  "Science  is  what we",
  "understand      well",
  "enough to explain to",
  "a  computer.  Art is",
  "everything  else  we",
  "do                  "
]
```

## Constraints:
```
1 <= words.length <= 300
1 <= words[i].length <= 20
words[i] consists of only English letters and symbols.
1 <= maxWidth <= 100
words[i].length <= maxWidth
```
<details><summary>68 ms</summary>

# 68 ms submission
```js
var fullJustify = function(words, maxWidth) {
    const res = [[]];
    res[0].letters = 0;
    for (let word of words) {
        let row = res[res.length - 1];
        if (row.length && row.letters + row.length + word.length > maxWidth) {
            res.push([]);
            row = res[res.length - 1];
            row.letters = 0;
        }
        row.push(word);
        row.letters += word.length;
    }
    for (let r = 0; r < res.length; r++) {
        let row = res[r];
        if (row.length === 1 || r === res.length - 1) {
            res[r] = row.join(' ') + ' '.repeat(maxWidth - row.letters - row.length + 1);
            continue;
        }
        let line = row[0];
        let spaces = maxWidth - row.letters;
        let minSpaces = ' '.repeat(Math.floor(spaces / (row.length - 1)));
        let addSpace = spaces % (row.length - 1);
        for (let w = 1; w < row.length; w++) {
            line += minSpaces + (w <= addSpace ? ' ' : '') + row[w];
        }
        res[r] = line;
    }
    return res;
};

```

</details>

<details><summary>64 ms</summary>

# 64 ms submission

```js
function fullJustify(words, maxWidth) {
    const res = [[]];
    res[0].letters = 0;
    for (let word of words) {  // first we go through and figure out how many words we can fit in a given row
        let row = res[res.length - 1];
        if (row.length && row.letters + row.length + word.length > maxWidth) {  // new row condition each word will need at least one space 
            res.push([]);
            row = res[res.length - 1];  // really creative way of getting the latest list
            row.letters = 0;  // I learned that you can do this since it is an object
        }
        row.push(word);
        row.letters += word.length;
    }
    for (let r = 0; r < res.length; r++) {
        let row = res[r];  // ['firstWord', 'secondWord', ...] 
        if (row.length === 1 || r === res.length - 1) {  // if its the last row then we left justify
            res[r] = row.join(' ') + ' '.repeat(maxWidth - row.letters - row.length + 1);
            continue;
        }
        let line = row[0];  // start with the first word
        let spaces = maxWidth - row.letters;  // this is the total spaces possible
        let minSpaces = ' '.repeat(Math.floor(spaces / (row.length - 1)));
        let addSpace = spaces % (row.length - 1);
        
        for (let w = 1; w < row.length; w++) {
            line += minSpaces + (w <= addSpace ? ' ' : '') + row[w];
        }
        res[r] = line;
    }
    return res;
};
```


</details>

<details><summary>60 ms</summary>

# 60 ms submission double pass
```js
var fullJustify = function(words, maxWidth) {
    const res = [[]];
    res[0].letters = 0;
    for (let word of words) {
        let row = res[res.length - 1];
        if (row.length && row.letters + row.length + word.length > maxWidth) {
            res.push([]);
            row = res[res.length - 1];
            row.letters = 0;
        }
        row.push(word);
        row.letters += word.length;
    }
    for (let r = 0; r < res.length; r++) {
        let row = res[r];
        if (row.length === 1 || r === res.length - 1) {
            res[r] = row.join(' ') + ' '.repeat(maxWidth - row.letters - row.length + 1);
            continue;
        }
        let line = row[0];
        let spaces = maxWidth - row.letters;
        let minSpaces = ' '.repeat(Math.floor(spaces / (row.length - 1)));
        let addSpace = spaces % (row.length - 1);
        for (let w = 1; w < row.length; w++) {
            line += minSpaces + (w <= addSpace ? ' ' : '') + row[w];
        }
        res[r] = line;
    }
    return res;
};

```

```
Console output
fullJustify([This,is,an,example,of,text,justification.], 16)
[
  [ 'This', 'is', 'an', letters: 8 ],
  [ 'example', 'of', 'text', letters: 13 ],
  [ 'justification.', letters: 14 ]
]
============ second loop =============
row: [This,is,an]
    row length: 3
    joined row: This,is,an
    line: 'This'
       spaces: 8
       min spaces: 4
       row[w]: 'is'
          add spaces: 0
          modded line: 'This    is'
       row[w]: 'an'
          add spaces: 0
          modded line: 'This    is    an'
row: [example,of,text]
    row length: 3
    joined row: example,of,text
    line: 'example'
       spaces: 3
       min spaces: 1
       row[w]: 'of'
          add spaces: 1
          modded line: 'example  of'
       row[w]: 'text'
          add spaces: 1
          modded line: 'example  of text'
row: [justification.]
    row length: 1
```

# Single pass

```js
var fullJustify = function(words, maxWidth) {
    for (let res = [[]], i = 0, letters = 0; i <= words.length; letters += words[i++].length) {
        let row = res[res.length - 1];
        if (i === words.length || row.length && letters + row.length + words[i].length > maxWidth) {
            if (row.length === 1 || i === words.length) {
                res[res.length - 1] = row.join(' ') + ' '.repeat(maxWidth - letters - row.length + 1);
                if (i === words.length) return res;
            } else {
                let line = row[0];
                let spaces = maxWidth - letters;
                let minSpaces = ' '.repeat(Math.floor(spaces / (row.length - 1)));
                let addSpace = spaces % (row.length - 1);
                for (let w = 1; w < row.length; w++) {
                    line += minSpaces + (w <= addSpace ? ' ' : '') + row[w];
                }
                res[res.length - 1] = line;
            }
            res.push([]);
            letters = 0;
        }
        res[res.length - 1].push(words[i]);
    }
};
```

</details>

