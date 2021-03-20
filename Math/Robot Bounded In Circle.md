# Prompt
On an infinite plane, a robot initially stands at (0, 0) and faces north. The robot can receive one of three instructions:

"G": go straight 1 unit;
"L": turn 90 degrees to the left;
"R": turn 90 degrees to the right.
The robot performs the instructions given in order, and repeats them forever.

Return true if and only if there exists a circle in the plane such that the robot never leaves the circle.

## Example
```
Example 1:

Input: instructions = "GGLLGG"
Output: true
Explanation: The robot moves from (0,0) to (0,2), turns 180 degrees, and then returns to (0,0).
When repeating these instructions, the robot remains in the circle of radius 2 centered at the origin.
Example 2:

Input: instructions = "GG"
Output: false
Explanation: The robot moves north indefinitely.
Example 3:

Input: instructions = "GL"
Output: true
Explanation: The robot moves from (0, 0) -> (0, 1) -> (-1, 1) -> (-1, 0) -> (0, 0) -> ...
 
```
## Constraints:

1 <= instructions.length <= 100
instructions[i] is 'G', 'L' or, 'R'.

<details><summary>My JS solution</summary>

```js
/**
 * @param {string} instructions
 * @return {boolean}
 
          
   facing this way 
          r 
          
[
    -2 [-2, -1, 0,  1,  2],
    -1 [-2, -1, 0,  1,  2],
     0 [-2, -1, 0,  1,  2],
     1 [-2, -1, 0,  1,  2],
     2 [-2, -1, 0,  1,  2],
]

true turns: a turn where the robot moves forward at least once afterwards and was not negated by another turn or by 3 consecutive turns in the same direction
no circle
    has a forward movement
        no turns
        the turns negate one another
        there is less than 3 true turns in a given direction

 - if you turn left then right or right then left then you negate the direction
 */
function isRobotBounded(instructions) {  
    // case: no instructions return true 
    if (instructions.length)
        return true;
    
    // counts the types of commands
    // {'G': number, 'L': number, 'R': number, negatedTurns: 0}
    const instructionMap = { // runs n times 
        negatedTurns: 0,  // RL or LR
        threeSixtyTurns: 0, // RRR or LLL
        'G': 0,
        'L': 0,
        'R': 0,
        meaningfulLeft: 0,
        meaningfulRight: 0
    };  
    const isTurn = (value) => value === 'L' || value === 'R';
    // helper method modifies left or right by amount
    const modMeaningful = (value, amount) => {
        if (value === "L")
            instructionMap.meaningfulLeft += amount;
        else
            instructionMap.meanginfulRight += amount;
    }
    instructions.forEach((command, i) => {
        instructionMap[command] += 1;
        if (i > 0 && isTurn(command)){
            // check for LR or RL negation
            if (instructions[i-1] && command !== instructions[i-1]){
                instructionMap.negatedTurns += 1;
                modMeaningful(command, -1);
            }
            // check for three sixy turns
            // RRR
            else if (i > 1 
                && isTurn(command) 
                && instructions[i-1] === command 
                && instructions[i-2] === command){
                instructionMap.threeSixtyTurn += 1;
                modMeaningful(command, -2);
            }
             // we have a meaningful turn, i.e. a turn with a move
            else {
                modMeaningful(command, 1);
            }
           
        }
    })

    // case: no forward movement then robot is just turning 
    if (instructionMap.G === 0)
        return true;
    // otherwise we know that is at least one forward movement 
    // case: no turns then unbounded, it is heading somewhere fast
    if (instructionMap.L === 0 && instructionMap.R === 0)
        return false;
    // case: all turns negate one another so the robot never turns back
    if (instructions.length = 
        instructionMap.G 
        + instructions.negatedTurns * 2 
        + instructionMap.threeSixtyTurns * 3)
        return false;
    
    // if right and left don't equal one another then the robot is heading in a turning orientation and we know it will eventually come back 
    return instructionMap.meaningfulRight === instructionMap.meanginfulLeft;
    
};

```

</details>