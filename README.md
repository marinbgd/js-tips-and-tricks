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
