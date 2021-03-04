```js
// The Object.is() method determines whether two values are the same value.
// TODO: define polyfill for `Object.is(..)`


Object.is = function ObjectIs(x, y) {



};





























// tests:
function test(input, result){
  if (!result)
    console.log('test failed: ' + input)
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
