# Coercing primitives in javascript

## Understanding Coercion
There are seven data types in javascript namely: `null, undefined,string,number,symbol, boolean` and `object`. The first six are called `primitives`. The last one, Object is called `non-primitive`. 

Javascript is dynamically typed. Meaning that a variable can start off as a type(for instance as a string data type) and end up as another type(say as a number). For instance 
```js
  let person = "human"  //string data type
  person = 10               // has become a number data type
```
**Coercion**, also known as "type conversion" is used to 'convert one data type to another'. ie, used to for instance convert a string to a number or number to string etc. 

Primitives can be coerced or changed to either `Number, String` or `Boolean`.
### Coerce primitives to Number
Primitives are changed to `number` using the `comparison operators`. The comparison operators include "`>, >=, <, =<`" etc.
Let's see how this is done

**Example 1: numeric string and number**
```js
 "6" >= 1
//Solution,
//Convert '6' (string) to number gives 6 (number), hence
 6 >= 1 // true
 true 
```

**Example 2: boolean and number**
```js
false > 3
//Solution:
//Convert false to number gives 0, hence
0 > 3 // false
false
```

**Example 3: null and number**
```js
null > 4 
//Solution:
//Convert null to number gives 0, hence
0 > 4 // false
false
```

**Example 4: text string and number**
```js
'hello' > 4 
//Solution:
//Convert 'hello' to number gives 1, hence
1 > 4 // false
false
```
**Example 5: undefined and number**
```js
undefined > '9' 
//Solution:
//Convert undefined to number gives NaN, hence
NaN > 4 // false
false
```
**Example 6: boolean and numeric string**
```js
true > 4 
//Solution:
//Convert true to number gives 1, hence
1 > 4 // false
false
```

### Coerce primitives to String
Primitives are coerced to `string` using the `unary + operator`. If any of the operands is a string, it coerces them to string. This is called `operator overboarding`.

**Example 1: boolean + string + number**
```js
true + "Is" + 5 // "trueIs5"
```
**Example 2: string + null**
```js
"hello" + null // "hellonull"
```
**Example 3: string + boolean**
```js
"hello" + true // "hellotrue"
```
**Example 4: string + undefined**
```js
"hello" + undefined // "helloundefined"
```
**Example 4: number + string**
```js
4 + "hello" // "4hello"
```

### Coerce primitives to Boolean
Primitives are changed to Boolean using the logical operators. The logical operators include "`&&, ||, !`". 
Evaluation is based on Logical operators.

### Examples using the and(&&) operator

**Example 1: number and number**
```js
345 && 567 // 567 
//resolved as
true && true
567
```
**Example 2: number and string**
```js
345 && "hello" // "hello" 
//resolved as
true && true
"hello"
```
**Example 3: null and string**
```js
null && "hello" // null 
//resolved as
false && true
null
```
**Example 4: string and string**
```js
"person" and "hello" // "hello" 
//resolved as
true && true
"hello"
```
**Example 5: null and boolean**
```js
null && true // null
//resolved as
false && true
null
```
**Example 6: number and boolean**
```js
4 && true // true
//resolved as
true && true
true
```
**Example 6: null and undefined**
```js
null && undefined // null
//resolved as
false && false
null
```
### Examples using the or(||) operator

**Example 1: number || number**
```js
345 || 567 // 345 
//resolved as
true || true
345
```
**Example 2: number || string**
```js
345 || "hello" // 345
//resolved as
true || true
345
```
**Example 3: null || string**
```js
null || "hello" // "hello" 
//resolved as
false || true
"hello"
```
**Example 4: string || string**
```js
"person" || "hello" // "person"
//resolved as
true || true
"person"
```
**Example 5: null || boolean**
```js
null || true // true
//resolved as
false || true
true
```
**Example 6: number || boolean**
```js
4 || true // 4
//resolved as
true || true
4
```
**Example 6: null || undefined**
```js
null || undefined // undefined
//resolved as
false || false
undefined
```

## Conclusion
The dynamic nature of javascript makes that data types can be changed from one type to another and coercion is that mechanism of achieving this feat. Primitives can be coerced to either Boolean, Number or String.

For a more detailed explanation of coercion, you can read up this sister article on [Coercion in javascript](https://hashnode.com/post/coercion-in-javascript-ckh2i0wt30319ajs1e88ygnxv). 

