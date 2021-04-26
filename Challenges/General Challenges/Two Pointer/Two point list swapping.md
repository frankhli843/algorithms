```js
// put all zeros on the right and then return the number of zeros
function zerosOnRight(list){
  if (!list || list.length === 0) return 0;
  let i = 0;
  let j = list.length;
  // while these two values are not equal and i is still valid
  while (i !== j && i < list.length){
    const left = list[i];
    const right = list[j];
    // if left is 0 try to replace with j pointer
    if (left === 0){
      // if j pointer is 0 then move j fowrad
      if (right === 0) j++;
      // otherwise swap them
      else {
        list[i] = right;
        list[j] = left;
      }
    }
    // otherwise increment i for the next value
    else {
      i++
    }
     
  }

  // [1,2,0], i = 2, length - 2 = 1
  // [1,2,3], i = 3, length - 3 = 0
  // [0,0,0], i = 0, length - 0 = length
  return list.length - i;
}
```