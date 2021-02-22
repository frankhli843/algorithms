```js
/**
 * Create a method which takes an object and converts the object to a string.

Examples:
stringify({ a: 42, b: "123" }) => "{"a":42,"b":"123"}"
stringify({ a: 42, b: "123" }) => '{"a":42,"b":"123"}' 
stringify({
    a: 42, 
    b: "123",
    c: {
        d: [2]
    }
}) => '{"a":42,"b":"123","c":{"d":5}}'
// ['one', 'two', 1, { name: ‘aws’}]

// { name: "andrew", age: 24, married: false, single: true }
// { array: ['one', 'two'] }
// { array: ['one', 2, false, null, { value: 'five', or: 2}] }
// { array: ['one', 'two'], nothing: null }
// { name: 'andrew', address: { streetAddress: '21st street', city: 'New York', state: 'NY'}}

**/

// Kiran once again thanks for the interview I learned a lot!

function stringify(inputObject) {
    let result = "";
    if (typeof inputObject === "undefined"){
        return "undefined";
    }
    if (typeof inputObject === "boolean"){
        return inputObject.toString();
    }
    // string
    if (typeof inputObject === "string"){
      
        return `"${inputObject}"`;
    }
    // number
    if (typeof inputObject === "number"){
        return inputObject.toString();
    }
    // array
    if (inputObject instanceof Array){
        // if empty array
        if (inputObject.length === 0){
            return "[]";
        }
        else { 
            return "[" + inputObject.map(v => {return stringify(v)}).join(',') + "]";
        }
    }
    // object or null
    if (typeof inputObject === "object"){
        // check if null...
        if (inputObject === null){
            return "null";
        }
        // empty
        else if (Object.keys(inputObject).length === 0){
            return "{}";
        }
        // some things in it
        else {
            return "{" + Object.keys(inputObject).map(k => { return `"${k}":${stringify(inputObject[k])}`}).join(',') + "}"
        }
    }
    // function
    return "null";
}

function test(actual){
    const myVersion = stringify(actual);
    const actualVersion = JSON.stringify(actual);
    if (myVersion === actualVersion){
        console.log(`\n\nPassed: ${actualVersion}`)
    }
    else {
        console.log(`\n\nFailed: ${actualVersion}`);
        console.log(myVersion);
        console.log(actualVersion);
    }
    return myVersion === actualVersion;
}


test({name: "Frank"})
test({name:"andrew",age:24,married:false,single:true})
test({ array: ['one', 'two'] })
test({ array: ['one', 2, false, null, { value: 'five', or: 2}] })
test({ array: ['one', 'two'], nothing: null })
test({ name: 'andrew', address: { streetAddress: '21st street', city: 'New York', state: 'NY'}})


```