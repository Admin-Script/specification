# 1 Introduction

AdminScript's main goal is to be user-friendly and create an intuitive way to write commands.
This document standardizes AdminScript syntax, operators, types, and other functionality for
developer and user reference.

## 1.1 What is AdminScript?

AdminScript is a scripting language targetting command interfaces that need to remain simple
while providing powerful ways for interaction. AdminScript targets apps interacting via
a primitive command interface, which allows the user to shorten invocation string length and use the
app with greater efficacy.

# 2 Terms and definitions

This section defines the terms used throughout the specification.

## 2.1 Accepted Characters

For this document, a character is a Unicode code point.

All statements, variable names in AdminScript are limited to ASCII letters, digits, and the underscore character (`[0-9A-Za-z_]`), although none
of the names can begin with a digit. The regex to match names is `[A-Za-z_][0-9A-Za-z_]*`.

Lines are valid AdminScript statements. An accepted line ending is, a new line character (LF: `U+000A`),
a carriage return (CR: `U+000D`), and a carriage return followed by a newline character (CR+LF: `U+000A U+000D`)

The null character (`U+0000`) is not accepted and should be replaced with the replacement character (`U+FFFD`)

# 3 Syntax

## 3.1 Statement

A statement is a functional keyword in AdminScript.

Certain statements may be built-in, these statements are included in this spec and must be implemented
in AdminScript implementations. 

On the other hand, since AdminScript is extensible, a developer may add statements 
to the language, to differentiate between these two, the added statements  will be called "developer statement(s)"

A statement is invoked by `statement [ARGS]`

## 3.2 Variables

Variables are names in a scope which store certain data, they are dynamic in type.
A variable cannot be named after any built-in names, this behavior is called a hard name.

Custom statements are soft by default but, they can be marked as hard statements.

A variable declarations is simply `variable = <value>`

# 4 Operators

## 4.1 Filter

The filter operator `:` (`U+003A`) creates a filter context object which validates values
the constructed object is a parameter that can be stored or passed to a statement, a statement may try
to parse the filter's underlying condition to optimize the execution.

The filter operator creates a local scope, in this scope the expression has access to the value being filtered
and public attributes of the value are available as variables in the scope.

Intuitively the filter operator can be thought of as an inline subroutine that returns boolean values.

`statement check_data : check_data >= 1`

Here the statement receives a filter object and it returns the first pass or all data that passes the filter.

## 4.2 Pipe

The pipe operator `|` (`U+007C`) allows passing returned values as the last argument to the next statement.

`statement "data" | statement2`

here if `statement` returns an array then `statement2` will be invoked **for each** element
if a non-array value is returned it passed as is

## 4.3 Pass

The pass operator `->` (`U+002D U+003E`) is similar to the pipe operator except, it passes the entire value as is.

`statement "data" -> statement2`

If the statement returns an array then it is passed as is to `statement2`

## 4.4 Arithmetic Operators

### 4.4.1 Addition Operator

`+` - Return the sum of two numbers

`a + b`

### 4.4.2 Substraction Operator

`-` - Return arithmetic difference of two numbers

`a - b`

### 4.4.3 Multiplication Operator

`*` - Return the product of two numbers

`a * b`

### 4.4.4 Division Operator

`/` - Divide two numbers.

`a / b`

### 4.4.5 Modulus Operator

`%` - Return remainder of division of left operand and right operand

`a % b`

### 4.4.6 Unrary Addition Operator

`+[a]` - Return the number as is.

`+9`

### 4.4.7 Unrary Substraction (Additive Inverse) Operator

`-[a]` - Return the additive inverse of a number

`-9`

## 4.5 Logical Operators

### 4.5.1 Logical And Operator

`&&` - Return the second truthy value, if both operands truthy, else return the falsy operand.
If both falsy then return second. Do not evaluate the second statement if the first is false.

`a && b`

### 4.5.2 Logical Or Operator

`||` - Return the first truthy operand, else return the second falsy operand.
Do not evaluate the second statement if the first is true.

`a || b`

### 4.5.3 Logical Negation Operator

`![a]` - Negate the operands truthiness. It is not a bit-wise negation.

`!a`

## 4.6 Relational Operators

### 4.6.1 Greater Than

`>` - Check if the left operand is greater than the right.

`a > b`

### 4.6.2 Lesser Than

`<` - Check if the right operand is greater than the left.

`a < b`

### 4.6.3 Equals To

`==` - Check if both operands are equal.

`a == b`

### 4.6.4 Not Equals To

`!=` - Check if both operands are unequal.

### 4.6.5 Greater Than Or Equals To

`>=` - Check if the left operand is greater or equal to the right operand.

`a >= b`

### 4.6.6 Lesser Than Or Equals To

`<=` - Check if the right operand is greater or equal to the left operand.

`a <= b`

## 4.7 Assignment Operator

`=` - Assign a name in the scope to a value

`a = 9` `name = <literal>`

## 4.8 Dot Attribute Access Operator

`.` - Return the attribute of a value.

None of the builtin types have no attributes hence this is only applicable for custom types.
See `7.1` for more details.

# 5 Types

## 5.1 Integer

`int` - bit-width is either 32 or 64 depending on the platform, signed. bit width is not needed if implemented
in a platform that allows indefinite width for integers, however appropriate measures against DOS attacks shall
be put in place.

Upon overflowing the engine shall raise an error for handling by the app.

`+0` and `-0` evaluate as equal.

Operations supported
- All operations

Example - `a = -123`

## 5.2 Float

The type is named `float`

IEEE 754, bit-width is maximum precision given by the platform.

`+0.0` and `-0.0` are equal values.

`+inf` and `-inf` are distinct values.

`NaN` and all other values are distinct.

Operations supported
- All Arithmetic, relational and logical

Example - `a = 0.0` `a = -2.567`

## 5.3 String

The type is named `string`

An array of characters, this type is immutable.

Operations supported
- `+`
  Concatenate strings
- All relational
  Compared the code point of characters

Example - `a = 'string'` `a = "foo"`

## 5.4 Array

The type is named `array`

An array of multiple objects, this type is mutable.

Operations supported
- `+`
  Make a new array with elements from both)
- `==`
  Check equal
- `!=`
  Check inequal

Truthiness - `false` if array has 0 elements.

Example - `[1,2,3]`

## 5.5 Time

A time type, comparable with timestamps which are strings, integers, floats. Timestamps start
from Unix echo.

can be constructed by `time(1924972200)` using a unix timestamp ()
or by `time("2000-01-01")` where the stamp is iso 8071 compatible and is matched by
the regex
`\d{4}-[01]\d-[0-3]\d(?:[^zZ][0-2]\d(?::[0-5]\d(?::[0-6]\d(?:\.\d{3}(?:\d{3})?)?)?)?)?(?:[zZ]|[+-][0-2]\d:[0-5]\d(?::[0-5]\d(?:\.\d{6})?)?)?`

or to simplify the accepted string time is
`YYYY-MM-DD[(any character not 'z' or 'Z')HH[:MM[:SS[.fff[fff]]]]]['z' or 'Z' or (('+' or '-')HH:MM[:SS[.ffffff]])]`
where `[...]` represents an optional match.

For **implementors** who wish to use regex for matching and are willing to handle post-validation
themselves the following basic regex can be used
`(\d{4})-(\d{2})-(\d{2})(?:[^zZ](\d{2})(?::(\d{2})(?::(\d{2})(?:\.(\d{3})(?:(\d{3}))?)?)?)?)?(?:([zZ])|([+-])(\d{2}):(\d{2})(?::(\d{2})(?:\.(\d{6}))?)?)?`
The above regex matches 14 groups
The above regex does not validate the string, hence implementors should handle nuances related to iso 8071 like leap seconds.

Operations supported
- All relational

## 5.6 Boolean

This type is named `bool`

This is a bi-stated data type.
Recommended bit width is 1 byte.

Supported operation
- All arithmetic such that `true` = `1` & `false` = 0
- All relational

Constructor
- `bool(a)`
  Equivalent to `!(!a)`

# 6 Custom Types

## 6.1 Attributes

All access of attributes of the type will be done via a method specifed by the implementation.
The signature for the method includes the attribute as a string.

The type should return AdminScript compatible types from the dot access operator or the implementation may cast them.

## 6.2 Methods

Methods can be returned as functions from the dot attribute access operator method
that is `a.b()` is evaluated as `a.get_attribute("b")()`, methods receive no special treatement
and are treated as attributes.

## 6.3 Restrictions

Attribute access on a type which is not an AdminScript compatible type is restricted, that is
the type must have a dot operator method and must be a registered type.

For example, In a python engine without attribute access regulation
```
func = sometype.somemethod
builtins = func.__builtins__
import = builtins.__getitem__("__import__")
```
This input will give access to every module in python.

The point of a dot operator method is to allow AdminScript compatible types,
which would avoid the security issue altogather since with only type regulation (checking if a type is registered)
there remains a possibility of escaping.

With an explicit dot operator method in every custom type and type regulation it would not have been possible
to execute `builtins = func.__builtins__` since the function is not a registered type and nor did it have an explicit dot access operator.

## Custom Types as Namespaces

Custom types when stripped of all operators except dot, act as a simple namespaces which can be used
as modules.

# 7 Operator Implementation Details

Since Admin Script is extensible this section describes the API to define operator behavior for custom types.

*This section assumes that the underlying language has a way to bind the type and functions which will be used as its operator.
The simplest way to do this is to implement the type as a class in modern languages. Hence operator functions will be referred to as
"methods" but it just refers to functions bound to the custom type.*

## Definations for Section 7
- `Unsupported` - A value that can be returned by the operator method to indicate that performed operation is not supported with the other type.
  This also refers to the alternative method of signaling that an operation is unsupported, such as using a special unsupported exception.

## Operator Model For Custom Types

If an operator is supported by a type then it must have a "method" for executing the operation.
This method should return or signal `Unsupported` if the specified operation isn't supported.
It may also raise an error that should be properly chained and raised higher for handling by application.

In which case the right operand's right-hand side operator variant method should be invoked,
that is to illustrate, in `a + b`
first, `a.add(b)` is invoked. if it returns/signals `Unsupported` then
`b.radd(a)` is invoked. if it also does not support the operation then an error is raised to be handled by the app.

## Special Operators

All logical operators are controlled by the same method which returns the truthiness.
To illustrate.

In `a || b`<br/>
First `a.truthy()` is invoked.<br/>
if `true` then `a` is returned else `b` is returned.

## 7.1 Dot Attribute Access Operator

The Dot attribute access operator has a dedicated methods which returns the specified operator,
to illustrate `a.b` is ran as `a.get_attrbute("b")` the name of the method is left to the the implementation
and `a.b = ...` is prohibited.

## Conventions

- Custom types **cannot** change the behavior for unary operators.

- In an operator method.
  If the type does not support an operation with the other operand, the operator method should return `Unsupported` or signal the same.
  If the type strictly does not support an operation with the other operand then it should raise an error

# 8 External Functions

AdminScript provides no way to define subroutines itself, however it allows functions from the underlying language
to be interacted with directly. There is one limitation that the functions can only be called with positional arguments.

Functions are added to the engine when a name is reserved for a native function, a type with methods is added.
Functions can be called directly with the AdminScript types or arguments can be converted before call the behaviour depends
on the implementation where such a problem may not exist if underlying types are reused.

## 8.1 Restrictions

All function and methods of custom types must be declared, so that implementations can prevent function calls that are not
declared and maybe unsafe to execute. While this is redundant if custom types properly implement attribute access, however
it is better to be redundant for security.

# 9 Builtin Statements

Currently there are no standardised builtin statements, implementations are free to add any they prefer.