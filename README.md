# JavaScript tips and tricks


## Tilde ~ Bitwise Not operator
- Great for checking if something exists in arrays/collections or strings
- Bitwise not operator works like this: `*~N -> -(N+1)*`
- examples:
    1) `~(-1) = 0`
    2) `~(2) = -3`
    3) `~(0) = -1`

````
  const haystack = [1, 2, 3, 4, 5, 7];

  const needleInCollection = 4;
  const foundIndex = haystack.indexOf(needleInCollection); // foundIndex = 3
  if (~foundIndex) { // ~(3) = -4 -> truthy
      // found in haystack
  }

  const needleNotInCollection = 'notInCollectionString';
  const foundIndex2 = haystack.indexOf(needleNotInCollection); // foundIndex = -1
  if (~foundIndex2) { // ~(-1) = 0 -> falsy
    // not found in haystack
  }

````

````
  const foo = 'hello world';

  if ( ~foo.indexOf('w') ) {
    // item in list
  } else {
    // item not in list
  }
````

##### Attention
As of ES7 (2016), Array.includes() can be used for accurate boolean check:
````
const haystack = 'hello world';

haystack.includes('w'); // true
haystack.includes('z'); // false
````


## Truthy and falsy values
Whenever JS expects or converts values to boolean, a truthy value will be converted to `true` and falsy value will be converter to `false`. All values are truthy except these:
* `0`
* `-0`
* `null`
* `undefined`
* `NaN`
* `''` // empty string
* `false`

The `!` operator converts any value to a boolean. Using this operator any falsy value will be converted to `true`. Double `!!` operator can be used to get corresponding boolean for any value. Examples:
* `!!null` //false
* `!!{}` //true


## Short-Circuiting
- Using `||` and `&&` logical operators
- **The logical `&&` operator returns the first operand if it is falsy, otherwise it returns the second operand**
- `&&` can be used as replacement for `if` statements
````
    if (isFetchAssetsAllowed) {
        fetchAssets()    
    }
    EQAUL
    isFetchAssetsAllowed && fetchAssets()
````
- **The logical `||` operator returns the first operand if it is truthy, otherwise it returns the second operand**
- `||` is commonly used for assigning fallback values to local variables
````
    // METHOD 1: Using if/else statements
    let requestAnimFrame = null;

    if (window.requestAnimationFrame) {
      requestAnimFrame = window.requestAnimationFrame;
    }

    else if (window.webkitRequestAnimationFrame) {
      requestAnimFrame = window.webkitRequestAnimationFrame;
    }

    else if (window.mozRequestAnimationFrame) {
      requestAnimFrame = window.mozRequestAnimationFrame;
    }

    // METHOD 2: Using short-circuiting
    const requestAnimFrame2 = (
      window.requestAnimationFrame ||
      window.webkitRequestAnimationFrame ||
      window.mozRequestAnimationFrame ||
      null
    );
    // the value of requestAnimFrame2 will be the first that is truthy!
````
- Note that short-circuiting actually returns the value!


## Numeric Separators
Purpose is to group digits to make long numbers more readable.git
The proposal allows underscores as separators in numeric literals:
````
const distanceEarthSunInKm = 149_600_000; // 149600000
````
### Restristions:
- You can only put underscores between two digits
- Only one underscore is allowed as numeric separator
- Number(), parseInt(), parseFloat() are not supporting numeric Separators


## Conditional Object Properties
The spread operator supports conditional adding of props. Don't have to create 2 separate functions/objects any more.
````
const getUser = ({ isEmailIncluded }) => {
  return {
    name: 'John',
    surname: 'Doe',
    ...isEmailIncluded && { email : 'john@doe.com' }
  }
}
const user = getUser({ isEmailIncluded: true });
console.log(user); // { name: "John", surname: "Doe", email: "john@doe.com" }

const userWithoutEmail = getUser({ isEmailIncluded: false });
console.log(userWithoutEmail); // { name: "John", surname: "Doe" }

````


## Convert to Number
To quickly convert a string to a number, + operator can be used followed by a string
````
let int = "15";
int = +int; // typeof int === 'number'
````


## Quick Float to Integer
Instead of Math.floor(), Math.ceil() or Math.round(), floats can be truncated to integers with the bitwise OR operator - "|" in a faster way.

This operation removes whatever comes after the decimal point.

````
console.log(23.9 | 0);  // Result: 23
console.log(-23.9 | 0); // Result: -23
````
You can get the same rounding effect by using "~~", as above, and in fact any bitwise operator would force a float to an integer.

````
console.log(~~23.9);  // Result: 23
console.log(~~-23.9); // Result: -23
````


## Replace all occurrences in a string
string.replace method replaces on the first occurrence. Easy way to replace all is to use global regex `/g`, but this regex isn't complicated nor difficult to read.
````
let inputString = 'Bob is a big nerd. Bob gets angry when he is not working on a mechanical keyboard.'
console.log(inputString.replace(/Bob/, 'Martin'));
  // 'Martin is a big nerd. Bob gets angry when he is not working on a mechanical keyboard.'
console.log(inputString.replace(/Bob/g, 'Martin'));
  // 'Martin is a big nerd. Martin gets angry when he is not working on a mechanical keyboard.'
````

Another useful regex global - `/i` - case-insensitive replace
````
console.log(inputString.replace(/bob/gi, 'Martin'));
  // 'Martin is a big nerd. Martin gets angry when he is not working on a mechanical keyboard.'
````


## Name your functions
Use meaningful names for clarity and faster code reading.
````
const numbersArr = [1, 2, 3, 4, 5]

numbersArr.filter(num => num % 2 === 0) // BAD - need to think about what this function is doing

const isEven = num => num % 2 === 0) // GOOD - immediately knowing what's going on.
numbersArr.filter(isEven)
````


## Don't pass arguments from one function to another
Array methods call functions that were sent to them with specific arguments.
There is no need to explicitly pass those arguments through another function.
````
const numbersArr = [1, 2, 3, 4, 5]
const multiplyByTwo = num => num * 2

numbersArr.map(num => multiplyByTwo(num)) // BAD - There is no need to explicitly pass num.

numbersArr.map(multiplyByTwo) // GOOD
````
### Attention !
Arrays will call the function with the 3 values: item value, item index and the array itself.
Watch for unwanted effects.
For example, using parseInt() in this way will use second argument as radix and give the wrong results.
