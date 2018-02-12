 Essential JS stuff to know
[JS Interview Q](https://www.toptal.com/javascript/interview-questions)
## `var a = b = 3;`
 is equal to
 ```
 var b = 3;
 var a = b;
 ```
 Therefore, a is `undefined` and b is `3`
 
## `use strict` mode 
Some of the key benefits of strict mode include:

**Makes debugging easier.** Code errors that would otherwise have been ignored or would have failed silently will now generate errors or throw exceptions, alerting you sooner to problems in your code and directing you more quickly to their source.

**Prevents accidental globals.** Without strict mode, assigning a value to an undeclared variable automatically creates a global variable with that name. This is one of the most common errors in JavaScript. In strict mode, attempting to do so throws an error.

**Eliminates this coercion.** Without strict mode, a reference to a this value of null or undefined is automatically coerced to the global. This can cause many headfakes and pull-out-your-hair kind of bugs. In strict mode, referencing a a this value of null or undefined throws an error.

**Disallows duplicate parameter values.** Strict mode throws an error when it detects a duplicate named argument for a function (e.g., function foo(val1, val2, val1){}), thereby catching what is almost certainly a bug in your code that you might otherwise have wasted lots of time tracking down.

**Note:** It used to be (in ECMAScript 5) that strict mode would disallow duplicate property names (e.g. var object = {foo: "bar", foo: "baz"};) but as of ECMAScript 2015 this is no longer the case.

**Makes eval() safer.** There are some differences in the way eval() behaves in strict mode and in non-strict mode. Most significantly, in strict mode, variables and functions declared inside of an eval() statement are not created in the containing scope (they are created in the containing scope in non-strict mode, which can also be a common source of problems).

**Throws error on invalid usage of delete.** The delete operator (used to remove properties from objects) cannot be used on non-configurable properties of the object. Non-strict code will fail silently when an attempt is made to delete a non-configurable property, whereas strict mode will throw an error in such a case.
 
## NaN
The `NaN` property represents a value that is “not a number”. This special value results from an operation that could not be performed either because one of the operands was non-numeric (e.g., "abc" / 4), or because the result of the operation is non-numeric.

While this seems straightforward enough, there are a couple of somewhat surprising characteristics of NaN that can result in hair-pulling bugs if one is not aware of them.

For one thing, although `NaN` means “not a number”, its type is, believe it or not, Number:
```
console.log(typeof NaN === "number");  // logs "true"
```
Additionally, `NaN` compared to anything – even itself! – is `false`:
```
console.log(NaN === NaN);  // logs "false"
```
A semi-reliable way to test whether a number is equal to `NaN` is with the built-in function `isNaN()`, but even using `isNaN()` is an imperfect solution.

A better solution would either be to use `value !== value`, which would only produce true if the value is equal to NaN. Also, ES6 offers a new `Number.isNaN()` function, which is a different and more reliable than the old global isNaN() function.

## Numbers
Numbers in JS are represented as floats. Therefore
```
console.log(0.1 + 0.2); => 0.300000000004
console.log(0.1 + 0.2 == 0.3); => false
```
## setTimeout(fn, 0)
A browser has to do a number of things pretty much all at once, and just one of those is execute JavaScript. But one of the things JavaScript is very often used for is to ask the browser to build a display element. This is often assumed to be done synchronously (particularly as JavaScript is not executed in parallel) but there is no guarantee this is the case and JavaScript does not have a well-defined mechanism for waiting.

The solution is to "pause" the JavaScript execution to let the rendering threads catch up. And this is the effect that setTimeout() with a timeout of 0 does. It is like a thread/process yield in C. Although it seems to say "run this immediately" it actually gives the browser a chance to finish doing some non-JavaScript things that have been waiting to finish before attending to this new piece of JavaScript.

(In actuality, setTimeout() re-queues the new JavaScript at the end of the execution queue. See the comments for links to a longer explanation.)

IE6 just happens to be more prone to this error, but I have seen it occur on older versions of Mozilla and in Firefox.
## String + Number
`1 + "2" + "2" Outputs: "122"` **Explanation:** The first operation to be performed in 1 + "2". Since one of the operands ("2") is a string, JavaScript assumes it needs to perform string concatenation and therefore converts the type of 1 to "1", 1 + "2" yields "12". Then, "12" + "2" yields "122"

`1 + +"2" + "2" Outputs: "32"` **Explanation:** Based on order of operations, the first operation to be performed is +"2" (the extra + before the first "2" is treated as a unary operator). Thus, JavaScript converts the type of "2" to numeric and then applies the unary + sign to it (i.e., treats it as a positive number). As a result, the next operation is now 1 + 2 which of course yields 3. But then, we have an operation between a number and a string (i.e., 3 and "2"), so once again JavaScript converts the type of the numeric value to a string and performs string concatenation, yielding "32".

`"A" - "B" + "2" Outputs: "NaN2` **Explanation:** Since the - operator can not be applied to strings, and since neither "A" nor "B" can be converted to numeric values, "A" - "B" yields NaN which is then concatenated with the string "2" to yield “NaN2”.
## Closure
A **closure** is an inner function that has access to the variables in the outer (enclosing) function’s scope chain. The closure has access to variables in three scopes; specifically: (1) variable in its own scope, (2) variables in the enclosing function’s scope, and (3) global variables.
