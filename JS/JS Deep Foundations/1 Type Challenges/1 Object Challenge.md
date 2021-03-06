# Level 1: Object.is 
```js
// The Object.is() method determines whether two values are the same value.
// TODO: define polyfill for `Object.is(..)`


Object.is = function ObjectIs(x, y) {



};

// tests:
function test(input, result){
  if (!result)
    console.log('test failed: ' + input)
  else
    console.log('Test Passed!')
}
test('Object.is(42, 42) === true', Object.is(42, 42) === true);
test('Object.is("foo", "foo") === true', Object.is("foo", "foo") === true);
test('Object.is(false, false) === true', Object.is(false, false) === true);
test('Object.is(null, null) === true', Object.is(null, null) === true);
test('Object.is(undefined, undefined) === true', Object.is(undefined, undefined) === true);
test('Object.is(NaN, NaN) === true', Object.is(NaN, NaN) === true);
test('Object.is(-0, -0) === true', Object.is(-0, -0) === true);
test('Object.is(0, 0) === true', Object.is(0, 0) === true);

test('Object.is(-0, 0) === false', Object.is(-0, 0) === false);
test('Object.is(0, -0) === false', Object.is(0, -0) === false);
test('Object.is(0, NaN) === false', Object.is(0, NaN) === false);
test('Object.is(NaN, 0) === false', Object.is(NaN, 0) === false);
test('Object.is(42, "42") === false', Object.is(42, "42") === false);
test('Object.is("42", 42) === false', Object.is("42", 42) === false);
test('Object.is("foo", "bar") === false', Object.is("foo", "bar") === false);
test('Object.is(false, true) === false', Object.is(false, true) === false);
test('Object.is(null, undefined) === false', Object.is(null, undefined) === false);
test('Object.is(undefined, null) === false', Object.is(undefined, null) === false);

```
<details><summary>Solution Object.is</summary>
  
# Solution 
```js
// The Object.is() method determines whether two values are the same value.
// TODO: define polyfill for `Object.is(..)`


Object.is = function ObjectIs(x, y) {
  if (typeof x !== typeof y)
    return false;

  else if (typeof x === 'number' && typeof y === 'number'){
    const isNaN = (q) => { return q !== q; }
    // CASE NaN: NaN does not equal itself 
    if (isNaN(x) && isNaN(y))
      return true;
    // CASE -0
    const isNegZero = (q) => { return q === 0 && 1 / q === -Infinity}
    const xIsNegZero = isNegZero(x);
    const yIsNegZero = isNegZero(y);
    if (xIsNegZero || yIsNegZero)
      return xIsNegZero && yIsNegZero
  }
  return x === y;
};

// tests:
function test(input, result){
  if (!result)
    console.log('test failed: ' + input)
  else
    console.log('Test Passed!')
}
test('Object.is(42, 42) === true', Object.is(42, 42) === true);
test('Object.is("foo", "foo") === true', Object.is("foo", "foo") === true);
test('Object.is(false, false) === true', Object.is(false, false) === true);
test('Object.is(null, null) === true', Object.is(null, null) === true);
test('Object.is(undefined, undefined) === true', Object.is(undefined, undefined) === true);
test('Object.is(NaN, NaN) === true', Object.is(NaN, NaN) === true);
test('Object.is(-0, -0) === true', Object.is(-0, -0) === true);
test('Object.is(0, 0) === true', Object.is(0, 0) === true);

test('Object.is(-0, 0) === false', Object.is(-0, 0) === false);
test('Object.is(0, -0) === false', Object.is(0, -0) === false);
test('Object.is(0, NaN) === false', Object.is(0, NaN) === false);
test('Object.is(NaN, 0) === false', Object.is(NaN, 0) === false);
test('Object.is(42, "42") === false', Object.is(42, "42") === false);
test('Object.is("42", 42) === false', Object.is("42", 42) === false);
test('Object.is("foo", "bar") === false', Object.is("foo", "bar") === false);
test('Object.is(false, true) === false', Object.is(false, true) === false);
test('Object.is(null, undefined) === false', Object.is(null, undefined) === false);
test('Object.is(undefined, null) === false', Object.is(undefined, null) === false);
```

</details>

# Level 2: implement `==`
- implement your own version of == to gain an understanding of JS types. Do not use `==`, `===`, `Object.is`
```js
// The Object.is() method determines whether two values are the same value.
// TODO: define polyfill for `Object.is(..)`

/**
 * 7.2.14Abstract Equality Comparison
The comparison x == y, where x and y are values, produces true or false. Such a comparison is performed as follows:

If Type(x) is the same as Type(y), then
Return the result of performing Strict Equality Comparison x === y.
If x is null and y is undefined, return true.
If x is undefined and y is null, return true.
If Type(x) is Number and Type(y) is String, return the result of the comparison x == ! ToNumber(y).
If Type(x) is String and Type(y) is Number, return the result of the comparison ! ToNumber(x) == y.
If Type(x) is Boolean, return the result of the comparison ! ToNumber(x) == y.
If Type(y) is Boolean, return the result of the comparison x == ! ToNumber(y).
If Type(x) is either String, Number, or Symbol and Type(y) is Object, return the result of the comparison x == ToPrimitive(y).
If Type(x) is Object and Type(y) is either String, Number, or Symbol, return the result of the comparison ToPrimitive(x) == y.
Return false.

 */
function eq(x, y){

  return strictEq(x,y)
}

/**
 7.2.15 Strict Equality Comparison
The comparison x === y, where x and y are values, produces true or false. Such a comparison is performed as follows:

If Type(x) is different from Type(y), return false.
If Type(x) is Number, then
  If x is NaN, return false.
  If y is NaN, return false.
  If x is the same Number value as y, return true.
  If 
  If x is -0 and y is +0, return true.
  Return false.
Return SameValueNonNumber(x, y).
 */
function strictEq(x,y){
  // CASE 1: different types
  if (typeof x !== typeof y){
    return false;
  }
  // CASE 2: number
  else if (typeof x === 'number'){
    // isNaN case
    if (isNaN(x || isNaN(y))){
      return false
    }
    // same number
    else if (x === y){
      return true;
    }
    // x is +0 and y is -0, return true.
    else if (x === 0 && isNegativeZero(y)){
      return true;
    }
    else if (y === 0 && isNegativeZero(x)){
      return true;
    }
    return false;
  }
  // CASE 3: Same type not number
  return sameValueNonNumber(x,y);

}

// NaN is the only type that does not equal itself
function isNaN(x){ 
  return x !== x;
}

function isNegativeZero(x){
  return x === 0 && 1/0 === -Infinity;
}

/**
 * 7.2.12
 * SameValueNonNumber ( x, y )
The internal comparison abstract operation SameValueNonNumber(x, y), where neither x nor y are Number values, produces true or false. Such a comparison is performed as follows:

Assert: Type(x) is not Number.
Assert: Type(x) is the same as Type(y).
If Type(x) is Undefined, return true.
If Type(x) is Null, return true.
If Type(x) is String, then
If x and y are exactly the same sequence of code units (same length and same code units at corresponding indices), return true; otherwise, return false.
If Type(x) is Boolean, then
If x and y are both true or both false, return true; otherwise, return false.
If Type(x) is Symbol, then
If x and y are both the same Symbol value, return true; otherwise, return false.
If x and y are the same Object value, return true. Otherwise, return false.
 */
function sameValueNonNumber(x,y){
  // CASE 1: Null or Undefined
  if (typeof x === "null" || typeof x === 'undefined'){
    return true;
  }
  // CASE 2: String
  else if (typeof x === "string"){
    if (x.length !== y.length)
      return false;
    const xList = x.split("");
    const yList = y.split("");
    for (let i = 0; i < xList.length; i+=1){
      if (xList[i] !== yList[i])
        return false;
    }
    return true;
  }
  // CASE 3: bool
  else if (typeof x === 'boolean'){
    return sameValueNonNumber(x.toString(), y.toString());
  }
  else if (typeof x === 'symbol'){
    return x === y; // same value check
  }
  else if (typeof x === 'object'){
    return x === y; // same value check
  }
}



// tests:
function S(input){
  return JSON.stringify(input);
}
function test(desc, expected, actual,){
  if (expected !== actual){
    console.log(`Test failed for: '${desc}'\n  expected:${expected}\n  actual:${actual}`);
  return '';
  }
}

class Class {}

const testTypes = [
  -2,
  -2.0,
  2.0,
  2, 
  "2", 
  "word", 
  NaN, 
  Infinity, 
  -Infinity, 
  "two", 
  undefined, 
  null, 
  ()=>{}, 
  function(){},
  {},
  true,
  false,
  "true",
  "false",
  1,
  2,
  new Class(),
  Symbol("Symbol"),
  0,
  -0,
  Date.now()
]

unique = {}
testTypes.forEach(x => {
  testTypes.forEach(y => {
    test(`${S(x)} == ${S(y)}`, x == y, eq(x,y));
  })
})



```
