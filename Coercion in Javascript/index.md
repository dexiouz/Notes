# Coercion in javascript
Have you ever been intrigued by the result of these javascript questions? that 

```js
[] + [] = ""
[] + {} = "[object Object]"
{} + {} = NaN
{} + [] = 0
[] + 5  = "5"
```

Well, I was both intrigued and shocked. That is why I delved in to find out what's happening there. In this article I will share my findings with you and you will be able to see how we came about those answers.

## Data Types and typeof
There are seven data types in javascript namely: `null, undefined, boolean, string,number,object and symbol`. 
  
The `typeof` operator is used to determine the data type of a javascript operand. In the expression 1 + 2, both 1 and 2 are `operands`. The `typeof` returns a string like so:

```js
typeof "string"  ==> 'string'
typeof 4         ==> 'number'
typeof undefined ==> 'undefined'
typeof true      ==> 'boolean'
typeof {}        ===> 'object'
typeof Symbol()  ===> 'symbol'
typeof null      ===> 'object'

//===================================
typeof []                 // 'object'
typeof function func(){}.  // 'function'
```


Notice that `null` is the only data type which is falsy and returns an 'object' from `typeof` check.

And that `typeof` function and array returns 'function' and 'object'. The reason is that `functions` and `array` are subtypes of the object type. 

# Coercion
Javascript is dynamically typed. This means that a variable's type can be changed. That is, a variable which is a string can end up being another type such as an object like this:
```js
  let person = "Chidera"  //string data type
  person = {}               // has become an object data type
```

**Coercion**, also known as "type conversion" is used to 'convert one data type to another'. ie, used to for instance convert a string to a number or number to string etc.

There are two types of coercion namely: "`Explicit and implicit coercion`". 

Explicit coercion happens when types are converted directly using syntax such as `String(), Number() or Boolean()`.

```js
let num = 4; //number type
let str = 'person' // string type
let bool = true // boolean type

// Explicit conversion syntax gives
String(4) = '4' // number 4 has been converted to string
Number(true) = 1  // boolean true has been converted to number 
Boolean(str) = true // string "str" has been converted to boolean
```
Implicit coercion happens less apparently when certain operators and expressions are used. For instance
```js
let age = 13;
let info = 'The student is' + age 'years old' // The student is 13 years old;
```

All the data types(null, undefined, boolean, string,number and symbol) are called `primitives` except the `object` data type which is called `non-primitive`.
## Primitive coercion
The behaviour of coercion for primitives and non-primitives is quite different. Primitives can be coerced or changed to either Number, String or Boolean.
### Coerce primitives to Number
Primitives are changed to `number` using the `comparison operators`. The comparison operators include "`>, >=, <, =<`" etc.
Let's see how this is done
```js
// string and number
 "6" >= 1
// Solution,
// - Convert '6' (string) to number gives 6 (number), hence
 6 >= 1 //which returns
 true 

// boolean and number
false > 3
// Solution,
// - Convert false to number gives 0, hence
0 > 3 // returns
false
```
### Coerce primitives to String
Primitives are coerced to `string` using the unary + operator. If any of the operands is a string, it coerces them to string. This is called `operator overboarding` For instance
```js
"7" + "hello"
// gives "7hello" because they are both strings

7 + "hello"
// gives "7hello" because one of the operands "hello" is a string
```
### Coerce primitives to Boolean
Primitives are changed to Boolean using the logical operators. The logical operators include "`&&, ||, !`".
```js
345 && "hello"
// evaluates to 
= true && true
// because converting a number to boolean gives you "true", same as converting a string to boolean also gives you "true"
=  true

// Also
true && "hello"
= true && true
= true
```
For a more detailed explanation on primitive coercion, checkout this article I wrote specifically for [Coercing primitives in javascript](https://hashnode.com/post/coercing-primitives-in-javascript-ckh4q0phi05mz39s16ptvhgl8)

## Non - Primitive coercion (Object coercion)
We have seen how primitive coercion is done. In this section we will do same for non-primitives. Recall that we said earlier that the object data type is called non-primitive and that the `typeof` of for functions and arrays will always resolve to "function" and "object" respectively because functions and arrays are _subtypes_ of object.
Just like primitives, objects or non-primitives can be coerced to either Boolean, string or number.

### Coerce non-primitives/objects to Boolean
Logical operators are used to coerces an object to boolean. `Objects will always resolve to true`.
```js
// if we did
console.log({} && [] && "person") // "person"
// we get "person" because
{} && [] && "person" // resolves to
true && true  && true // finally giving
= true

// Also, 
// if we 
console.log({} && true) // "true"
// Same principle above applies
```

### Coerce non-primitives/objects to Number and String
The whole idea of converting a non-primitive to Number and String just means to find a way to convert the non-primitive to primitive.

Every object prototype has two methods that affect coercion. They are `valueOf()` and `toString()`. If you check their typeof you will see they resolve to functions like:
```js
typeof Object.prototype.valueOf() // "function"
typeof Object.prototype.toString() // "function"
```

Now let's check the `valueOf()` and `toString()` of an object.
```js
let myObj = {}
myObj.valueOf() // {}
myObj.toString() // "[object object]"
```
The above outcomes will form the basis of the answers evaluated in this section.
To change an object to number or string, the javascript engine will first perform some checks vis:

1. It will either call valueOf() or toString() on the object based on the object's DefaultValue internal method. Some say it is based on the "hint" passed.  If a number is the hint, valueOf is called else if a string is the hint, valueOf is called. In most cases, valueOf is called first.
2. The next thing is to check if the returned value in (1) is a primitive. If it is a primitive, then return the primitive else continue to (3).
3. Call the next or fallback method. This can be either toString() or valueOf(). That is, if valueOf() was called in (1) above, the next or fallback method becomes toString(), same way, if toString() was called in (1), the fallback method becomes valueOf().
4. Perform (1) again using the fallback method. If the returned value is a primitive, return it else, it throws an error, "`TypeError: Cannot convert object to primitive`".

Recall our last example above where we checked valueOf and toString() of an object. Using the results there let's look at some examples

**Example 1**
```js
let myObj = {}
2 + myObj
```
 2 is already a primitive so let's evaluate the non-primitive. First we call valueOf() on myObj.
 ```js
 myObj.valueOf() // {}
 ```
but it resolves to {} which is also a non-primitve. So we call our fallback method which is in this case toString().
```js
myObj.toString() //"[object object]" 

// we now have
 2 + "[object object]" 
```
`toString()` returns a string for us which is a primitive.

Recall `operator overboarding`?, if any of the operands is a string, it evaluates the whole expression as string. In the above block, "[object object]" is a string. So we have
 ```js
 = "2" + "[object object]" // "2[object object]"
 ```
 Therefore,
 ```js
2 + myObj // "2[object object]"
```
Let us look at another example

**Example 2**
```js
[] + 5
```
 _The unary operator (+) performs string concatenation or numeric addition based on arguement type_. 
 
 Hence, + 5(arguement type) becomes 5(number).Number is now a hint for evaluation according to rule in (1) so we apply valueOf() on the non-primitive, the []
 ```js
[].valueOf() // []
```
It returned an array, another non-primitive. Let's fallback to our next method, which is toStirng().

```js
[].toString() //""
```
It evaluates to an empty string which is a primitive so we have
```js
[] + 5 
// as
"" + 5
```
 Since one of them is a string
```js
"" + 5 //becomes
"" + "5" // "5"
```
Our final solution is what we have below after evaluating two strings
```js
[] + 5 // "5"
```
**Example 3**
```js
[] + {}
```

First we call valueOf() on them.
```js
[].valueOf() // []
{}.valueOf() // {}
```
Non primitive was returned so we call the fallback method, in this case, toString()
```js
[].toString() // ""
{}.toString() // "[object object]"
```
Now we have string primitives like so
```js
"" + "[object object]"
```

Evaluating two primitives finally gives us
```js
"[object object]"
```
Therefore
```js
[] + {} //"[object object]"
```

**Example 4**
```js
[] + []
```
Using valueOf() on them gives
```js
[].valueOf() // []
[].valueOf() // []
```
Because non-primitive was returned we use fallback method which is toString().
```js
[].toString() // ""
[].toString() // ""
```
Our question resolves to 
```js
"" + "" // ""
```
Therefore
```js
[] + [] //""
```

**Example 5**
```js
{} + []
```
Now this one is a bit tricky because it resolves to 0. Let's see how.

Recall when we evaluated `[] + {}` in example 3, we got `"[objectobject]"`

However, for our fifth example, consider the two operands {} and []. For the first one {}, javascript does not interprete it as an object. Rather it interpretes it as a code block. So we have something like this

```js
{} + [] // as
code block + []
```

What javascript does next is to ignore the code block so we have
```js
 + []
```

The unary operator (+) on [] coerces it to a number hence we have 
```js
Number([]) // 0
```
That's how we reolved to 0.

In summary
```js
{} + []
= code block + [] 
```
Neglecting the code block we have
```js
 + []
```

Evaluating [] using the unary +

```js
Number([]) // 0
``` 

**Example 6**
```js
{} + {}
```
Juat like in example 5, javascript interpretes the first operand {} as a block of code. It neglects it and because of the unary (+) operator, it processes the second one like so
```js
Number({}) // NaN
```
Hence 
```js
{} + {} // NaN
```

**Example 7**
```js
'' - false
```
The subtract operand has no operation overboarding so ToNumber is invoked on the operands
```js
Number('') - Number(false) //
// This resolves to
 = 0 - 0 
 ```
 Therefore
```js
'' - false // 0
```



