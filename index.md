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

### 4.4.2 Substraction Operator

### 4.4.3 Multiplication Operator

### 4.4.4 Division Operator

### 4.4.5 Modulus Operator

### 4.4.6 Unrary Addition Operator

### 4.4.7 Unrary Substraction (Additive Inverse) Operator

## 4.5 Bitwise Operators

### 4.5.1 Bitwise Negation Operator

### 4.5.2 Bitwise Or Operator

### 4.5.3 Bitwise And Operator

### 4.5.4 Bitwise XOR Operator

### 4.5.5 Bitwise Left Shit Operator

### 4.5.6 Bitwise Right Shift Operator

## 4.6 Logical Operators

### 4.6.1 Logical And Operator

<!-- Second statement is not evaluated if first is false, keyword is "and" -->

### 4.6.2 Logical Or Operator

<!-- Second statement is not evaluated if first is true, keyword is "or" -->

### 4.6.3 Logical Negation Operator

## 4.7 Assignment Operators

### 4.7.1 Assignment

### 4.7.2 Addition Assignment

### 4.7.3 Substraction Assignment

### 4.7.4 Multiplication Assignment

### 4.7.5 Division Assignment

### 4.7.6 Modulus Assignment

### 4.7.7 Bitwise Or Assignment

### 4.7.8 Bitwise And Assignment

### 4.7.9 Bitwise XOR Assignment

### 4.7.10 Bitwise Left Shift Assignment

### 4.7.11 Bitwise Right Sift Assignment

### 4.7.12 Logical Or Assignment

### 4.7.13 Logical And Assignment

## 4.8 Relational Operators

### 4.8.1 Greater Than

### 4.8.2 Lesser Than

### 4.8.3 Equals To

### 4.8.4 Not Equals To

### 4.8.5 Greater Than Or Equals To

### 4.8.6 Lesser Than Or Equals To

