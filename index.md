# 1 Introduction

Admin Script's main goal is to be user friendly and create an inutitive way to write commands.
This document standardises adminScript's syntax, operators, types and other functionality for
developer and user reference.

## 1.1 What is adminScript?

AdminScript is a scripting language targetting command interfaces which need to remain simple
while being able to provide powerful ways for interaction. AdminScript targets apps interacting via
a primitive command interface, which allows the user to shorten invoction string length and use the
app with greater efficacy.

# 2 Terms and definations

This section defines terms which are used through out the specification.

## 2.1 Accepted Characters

For the purposes of this document, a character is a unicode code point.

All statements, variable names in adminscript are limited to ascii letters, digits, and underscore character (`[0-9A-Za-z_]`), although none
of the names can begin with a digit. The regex to match names is `[A-Za-z_][0-9A-Za-z_]*`.

Lines are valid adminScript statements. An accepted line ending is, a new line character (LF: `U+000A`),
a carriage return (CR: `U+000D`), and a carriage return followed by new line character (CR+LF: `U+000A U+000D`)

Null character (`U+0000`) is not an accepted character and should be replaced with replacement character (`U+FFFD`)

# 3 Syntax

## 3.1 Statement

A statement is a functional keyword in adminScript.

Certain statements maybe "built-in", that is these statements were included in this spec and need to implemented
in adminScript implementatons. On the other hand, since adminScript is extensible, a developer may add their own statements
to the language. To differentiate between these two statements, the added statements shall be referred to as "devloper statement(s)".

A statement is simply invoked by `statement [ARGS]`

## 3.2 Variables

Variables are names in a scope which store certain data, they are dynamic in type.
A variable cannot be named after any builtin names, also if a developer specifies then
variables may not also be named after certain developer statements.

A variable declarations is simply `variable = <value>`

# 4 Operators

## 4.1 Filter

The filter operator `:` (`U+003A`) creates a filter context object which validates values
the constructed object is a parameter which can stored or passed to a statement, a statment may try
to parse the filter's underlying condition in order to optimise the execution.

The filter operator creates a local scope, in this scope the expression has access to the value being filtered
and some attributes of the value are available as variables in the scope.

Intuitively the filter operator can be thought of as an inline subroutine which returns boolean values.

`statement check_data : check_data >= 1`

Here the statement receives a filter object and it returns the first pass or all data that passes the filter.

## 4.2 Pipe

The pipe operator `|` (`U+007C`) allows to pass returned values as the last argument to the succeeding statement.

`statement "data" | statement2`

here if `statement` returns an array then `statement2` will be invoked **for each** element
if a non-array is returned the value is passed as is

## 4.3 Pass

The pass operator `->` (`U+002D U+003E`) is similar to the pipe operator except it passes the entire value as is.

`statement "data" -> statement2`

If statment returns an array then the array is passed as is to `statement2`

## 4.4 Arithmetic Opertors

### 4.4.1 Addition Operator

`+` - Adds two numbers

`a + b`

### 4.4.2 Substraction Operator

`-` - Substracts two numbers

`a - b`

### 4.4.3 Multiplication Operator

`*` - Multiply two numbers

`a * b`

### 4.4.4 Division Operator

`/` - Divide two numbers.

`a / b`

### 4.4.5 Modulus Operator

`%` - Retrun remainder of division of left operand and right operand

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
If both falsy then return second. Doesn't evaluate second statement if first is falsy.

`a && b`

<!-- Second statement is not evaluated if first is false, keyword is "and" -->

### 4.5.2 Logical Or Operator

`||` - Return the first truthy operand if any, else return the second falsy operand.

`a || b`

<!-- Second statement is not evaluated if first is true, keyword is "or" -->

### 4.5.3 Logical Negation Operator

`![a]` - Negate the operands truthiness. This is not a bitwise negation.

`!a`

## 4.6 Relational Operators

### 4.6.1 Greater Than

`>` - Check if left operand greater.

`a > b`

### 4.6.2 Lesser Than

`<` - Check if right operand greater.

`a < b`

### 4.6.3 Equals To

`==` - Check if both operand equal.

`a == b`

### 4.6.4 Not Equals To

`!=` - Check if both operand inequal.

### 4.6.5 Greater Than Or Equals To

`>=` - Check if left operand greater or equal to right operand.

`a >= b`

### 4.6.6 Lesser Than Or Equals To

`<=` - Check if right operand greater or equal to left operand.

`a <= b`

## 4.7 Assignment Operator

`=` - Assign a name in the scope to a value

`a = 9` `name = <literal>`

# 5 Types

## 5.1 Integer

`int` - bit-width is either 32 or 64 depending on platform, signed. bit width may not be folllowed if implemented
platform allows indefinite width.

`+0` and `-0` evaluate as equal.

Example - `a = -123`

## 5.2 Float

The type is named `float`

IEEE 754, bit-width is maximum precision given by platform.

`+0.0` and `-0.0` evaluate equal

`+inf` and `-inf` evaluate distinct

`NaN` and all other values evaluate distinct.

Example - `a = 0.0` `a = -2.567`

## 5.3 String

The type is named `string`

An array of characters, this type is immutable.

Example - `a = 'string'` `a = "foo"`

## 5.4 Array

The type is named `array`

An array of multiple objects, this is mutable

Example - `[1,2,3]`

## 5.5 Time

A time type, comparable with timestamps which are strings, integers, floats. Timestamps start
from unix echo.

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
