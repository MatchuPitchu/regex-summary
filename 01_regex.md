# Summary Course Mastering Regular Expressions

> <https://www.udemy.com/course/mastering-regular-expressions-in-javascript/learn/>

## Basics

- Concept `Regular Expression` developed in 1950 by mathematician Stephen Keene
- in 1997 Philip Hazel developed PCRE (Perl Compatible Regular Expression) standard for use in many modern tools
- Syntax, Grammar and Patterns of how to use RegEx is common in multiple programming languages

## Different ways to use them in JavaScript

- Regular Expressions are Objects
- 2 ways to create RegEx Objects:
  - using the constructor and pass in the pattern you're looking for as argument
    - `let regex1 = new RegExp('hello');`
    - `regex1` becomes a regular expression obj
  - `litteral syntax`: recommended way
    - `let regex2 = /world/;`
    - `/` starts and ends a RegEx
    - `regex2` becomes a string obj wrapper
- test RegEx agains value with RegEx object methods:
  - `regex.test(<testValue>)`: returns true if pattern is found in passed string or false if not
  - `regex.exec(<testValue>)`: returns array of matches and additional info (index of match in input, input value)
    - to notice: when uses with RegEx and global flag (see below) and you find multiple matches, then exec() only returns first match, when you call it again, then it returns second match and so on
- test Regex agains value with string wrapper object methods:
  - `testValue.match(<regex>)`: returns array of matches like `exec()`
  - `testValue.search(<regex>)`: returns index of matched string in testValue
  - `testValue.replace(<regex>, <valueThatReplaces>)`: returns new string; replaces matches with a string; does NOT mutate initial value
  - `testValue.split(<regex>)`: splits a string into an array; uses regex match as a delimiter; removes regex match from string and returns array with separated strings

```JavaScript
const txt = 'Programming always starts with a hello world example.'

const regex1 = new RegExp('hello');
const regex2 = /worlds/;
const space = /\s/;

console.log(regex1.test(txt)); // -> true
console.log(regex2.test(txt)); // -> false

console.log(txt.match(regex1));
console.log(txt.split(space)); // -> ['Programming', 'always' ...]
```

## Regular Expressions Flags (-> Modifiers)

- `/pattern/flags;` or `new RegExp('pattern', 'flags')`
- `g`: global, match more than one occurrence
- `i`: case insensitive match, case does NOT matter
- `m`: multi-line match

```JavaScript
const txt = 'Programming courses always startS with a hello world example.'

const sFollowedBySpace = /s\s/g;
const sInsensitivFollowedBySpace = /s\s/gi;

console.log(txt.match(sFollowedBySpace)); // -> 2 matches
console.log(txt.match(sInsensitivFollowedBySpace)); // -> 3 matches

```

## Testing Regular Expressions

## Defining Patterns

## Metacharacters

## Character Sets

## Repetition

## Groupings

## Anchored Expressions

## Lookahead Assertions

## Using Unicode

## Useful Regular Expressions
