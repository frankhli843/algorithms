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

// implement the equivalent of js ==
function eq(x, y){
  return "not implemented";
}

// tests:
function S(input){
  return JSON.stringify(input);
}
function test(desc, expected, actual,){
  if (expected === actual){
    console.log(`Test passed for: '${desc}''`)
  }
  else {
    console.log(`Test failed for: '${desc}'\n  expected:${expected}\n  actual:${actual}`)
  }
}

class Class {}

const testTypes = [
  BigInt(9007199254740991),
  BigInt(2),
  -BigInt(2),
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

testTypes.forEach(x => {
  testTypes.forEach(y => {
    test(`${S(x)} == ${S(y)}`, x == y, eq(x,y))
  })
})


```
