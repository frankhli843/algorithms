---
marp: false
---

# Algorithms 
- I believe in taking some time out everyday to sharpen my skills. Part of that is to learn new languages and frameworks while working on projects and the other part is practicing algorithms. 
- If you notice any bugs please submit a ticket!


# Table of Contents
- These are sorted by the optimial solution used
- [Design](https://github.com/frankhli843/algorithms/tree/master/Challenges/General%20Challenges/Design)
- [Graph](https://github.com/frankhli843/algorithms/tree/master/Challenges/General%20Challenges/Graph)
- [Math](https://github.com/frankhli843/algorithms/tree/master/Challenges/General%20Challenges/Math)
- [Recusion](https://github.com/frankhli843/algorithms/tree/master/Challenges/General%20Challenges/Recusion)
- [Sliding Window](https://github.com/frankhli843/algorithms/tree/master/Challenges/General%20Challenges/Sliding%20Window)
- [Tree](https://github.com/frankhli843/algorithms/tree/master/Challenges/General%20Challenges/Tree)
- [Two Pointer](https://github.com/frankhli843/algorithms/tree/master/Challenges/General%20Challenges/Two%20Pointer)

# Lessons Learned
- Don't name things in a new and unique way. 
    - For example I was making a graph node class that was handling words. I gave node the attribute word instead of value. Halfway through I forgot and kept using value which caused so much grief.
- Don't try to clever and use a hashmap early on when you it to be an array most of the time. It might make things faster but it'll slow you down so much when you forget whether it is an object or a key. Instead use a TODO and just label that it could be a hashmap.
- JS
    - .map will return undefined for anything not returned use .filter(a) to removed that. Note though that this will remove 0
- When in doubt start with brute force and then optimize by logically restricting.
- If you are taking a pointers approach than set up the number of points required for a given calculation.
- Be careful when calculating and you are dividing by length. Could be 0.

# JS Fiddle template
```js
<form>
  <div id="main">
  
  </div>
</form>

$(document).ready(function () {

    $('head').append('<meta name="theme-color" content="#fd9">');
    

});

function s(x){ return JSON.stringify(x); }

function test(desc, expected, actual){
	$('#main').append(
  	s(expected) === s(actual)
    	? `Test ${desc} passed!`
      : `Test ${desc} failed! expected: ${s(expected)}, actual: ${s(actual)}`
  );
}

test("testing the test", true, false)

html {
  background: #643;
  color: #222;
}
textarea {
  border: 0px;
  background: #000;
  color: #fff;
  padding: 10px;
  outline: 0;
}

.main {
  color: white;
  background: black;
}
```
