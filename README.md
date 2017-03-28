# PlainScript

PlainScript is a small, plain, scripting language. Its only real purpose is to serve as a starting point for student language design and implementation projects. Here’s an example program in PlainScript:

```
def sum_of_digits(n):
    if n < 0:
        return sum_of_digits(-n)
    elif n < 10:
        return n
    else:
        return sum_of_digits(n / 10) + (n % 10)

print(sum_of_digits(n: 8835299))
```

Yes, it looks a lot like Python, but it’s _not_ a Python subset. There are some differences.

See the file [plainscript.ohm](https://github.com/rtoal/plainscript/blob/master/syntax/plainscript.ohm) for the official syntax. The syntax assumes the source code has been preprocessed and had indents and dedents inserted according the [Python indentation algorithm](https://docs.python.org/3/reference/lexical_analysis.html).

## Data Types

As of now, PlainScript has only two data types:
  * `number` (IEEE 754 64-bit)
  * `bool` (`true` and `false`).

The language is dynamically and weakly typed. Numbers and booleans are compatible with each other as in JavaScript. Sigh.

## Declarations

Variables are declared with `let`:
```
let x, y, z = 0, sqrt(3), 2
```
The expressions on the right hand side are evaluated in arbitrary order (or even in parallel) and then assigned to the newly created variables on the left hand side. The variables do not come into scope until _after_ the declaration is complete. So the following script prints `15`:
```
let x = 10
if true:
    let x = x + 5    # Right hand side refers to the outer x
    print(x)
```
It is illegal to have multiple declarations of the same identifier within a scope. Scopes are:
  * the entire program
  * the parameters plus the body of a function
  * bodies of while-statements and if-statement cases.

Inner scopes make holes in outer scopes, and inner declarations always shadow outer ones.

Function declarations look like this:
```
def f(a, b=a+1, c=5):
    let d = 8
    if b < 0:
        return a * b + c
    f(1, b - 10)
```
Each parameter comes into scope immediately after it is declared, so parameters can use earlier parameters in their default expressions. Parameters and local variables live in the same scope. The function name belongs to the outer scope. Conceptually it is comes into existence after the definition line, so (1) you _can_ call a function recursively, but you _cannot_ call the function in the default expressions of its own parameters.

Parameters without default expressions are called _required_ parameters; those with default expressions are called _optional_ parameters. You may not place a required parameter after an optional one.

## Expressions

A PlainScript expression is one of:
  * a boolean literal, either `true` and `false`
  * a numeric literal, e.g., 6.22e+28
  * a variable reference
  * an expression prefixed with `-` or `!`
  * an infix expression with operator `+`, `-`, `*`, `/`, `%`, `<`, `<=`, `==`, `!=`, `>=`, `>`, `and`, or `or`
  * a function call

It is an error to reference a variable that has not been declared.

A function call has the form:
```
f(5*3, true, c=5, d=10*g(1,2))
```
The arguments are evaluated in arbitrary order (or even in parallel) and passed to the callee. An argument prefixed with a parameter name is called a _keyword_ argument; arguments not prefixed are called _positional_ arguments. As in Python, positional arguments must come before keyword arguments.

All required parameters of a function must be covered in the call. If keyword arguments are present, the identifier must match one of the parameters. It is an error to match a parameter multiple times.

## Statements

The statements are:

  * a variable declaration (defined above).
  * a function declaration (defined above).
  * an assignment statement, e.g. `x, y = y, x+y`, in which the right-hand-sides are first evaluated in arbitrary order (or even in parallel) and then assigned to the corresponding variables on the left-hand-sides, which must be previously declared.
  * a function call (defined above).
  * a `break` statement, which must appear in a loop.
  * a `return` statement, which may or may not have an expression to return. It is an error to have a return statement outside of a function body.
  * an `if` statement
  * a `while` statement

## Predefined Environment

The top-level scope of a PlainScript program extends a global environment defining:
```
def print(_):
    ...

def sqrt(_):
    ...
```

## Future Plans

This language as it stands isn’t good for much. You should extend it to make something cool. Here are things you can add:

  * Constants
  * A string type
  * String literals
  * String interpolation
  * A list type
  * A set type
  * A tuple type
  * A dictionary type
  * Operators for strings, lists, sets, tuples, and dictionaries
  * Static typing
  * Generics
  * Type inference
  * Function overloading
  * Algebraic (Sum) types
  * Pattern Matching
  * Classes
  * Processes
  * (First-class) function types
