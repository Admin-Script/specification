# 1 Introduction

Admin Script's main goal is to be user friendly and create an inutitive way to write commands.
This document standardises adminScript's syntax, operators, types and other functionality for
developer and user reference.

## 1.1 What is adminScript?

AdminScript is a scripting language targetting command interfaces which need to remain simple
while being able to provide powerful ways to interaction. AdminScript targets apps interacting via
a primitive command interface, which allows the user to shorten invoction string length and use the
app with greater efficacy.

# 2 Terms and definations

This section defines terms which are used through out the specification.

## 2.1 Accepted Characters

For the purposes of this document, a character is a unicode code point.

All statements, variable names in adminscript are limited to ascii letters, digits, and underscore character (`[0-9A-Za-z_]`), although none
of the names can begin with a digit. The regex to match names is `[A-Za-z_][0-9A-Za-z_]*`.

Lines are adminScript statement which may optionally contain `;` (`U+003B`) at the ending.

An accepted line ending is, a new line character (LF: `U+000A`), a carriage return (CR: `U+000D`), and a carriage return
followed by new line character (CR+LF: `U+000A U+000D`)

Null character (`U+0000`) is not an accepted character and should be replaced with replacement character (`U+FFFD`) 

# 3 Syntax

## 3.1 Statement

A statement is a functional keyword in adminScript.

Certain statements maybe "built-in", that is these statements were included in this spec and need to implemented
in adminScript implementatons. On the other hand, since adminScript is extensible, a developer may add their own statements
to the language. To differentiate between these two statements, the added statements shall be referred to as "devloper statement(s)".

A statement is simply invoked by `statement [ARG1]`

## 3.2 Variables

Variables are names in a scope which store certain data, they are dynamic in type.
A variable cannot be named after any builtin names, also if a developer specifies then
variables may not also be named after certain developer statements.
