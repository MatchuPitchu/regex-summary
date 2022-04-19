# Summary Course Mastering Regular Expressions

> Udemy Course: <https://www.udemy.com/course/mastering-regular-expressions-in-javascript/learn/>
> Tool for testing RegEx: `RegEx Pal` <https://www.regexpal.com/>

- Concept `Regular Expression` developed in 1950 by mathematician Stephen Keene
- in 1997 Philip Hazel developed PCRE (Perl Compatible Regular Expression) standard for use in many modern tools
- Syntax, Grammar and Patterns of how to use RegEx is common in multiple programming languages

## Different ways to use them in JavaScript

- Regular Expressions are Objects
- 2 ways to create RegEx Objects:
  - using the constructor and pass in the pattern you're looking for as argument
    - `const regex1 = new RegExp('hello');`
    - `regex1` becomes a regular expression obj
  - `litteral syntax`: recommended way
    - `const regex2 = /world/;`
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
- `s`: dotAll Flag: added with ECMAScript 2018, with this flag, the `wildcard` metacharacter matches all(!) characters (without the normal exception of some control characters like new line)

```JavaScript
const txt = 'Programming courses always startS with a hello world example.'

const sFollowedBySpace = /s\s/g;
const sInsensitivFollowedBySpace = /s\s/gi;

console.log(txt.match(sFollowedBySpace)); // -> 2 matches
console.log(txt.match(sInsensitivFollowedBySpace)); // -> 3 matches
```

## How RegEx Engine Executes Matching

- engine checks character by character from the beginning if pattern matches
  - Process: checks first character (-> every character, no only letters) of RegEx with first character of text -> if match, then check second letter of text -> if match, then check third letter of text -> if no match, then it returns to second character of text and starts checking once again against first character of RegEx -> if no match, continue with next character of text against first character of RegEx -> if no match, continue ... -> if once compllete pattern matches, then first match is returned -> if `global flag` is set, it continues checking for matches

```JavaScript
const foo = /hello/;

here help hello
```

## Metacharacters

- in comparison to literal characters (-> e.g. simple letter or string like `hello`), metacharacters are used to represent other characters to make RegEx pattern more flexible for matches

```JavaScript
^ // Caret: beginning of a string OR negated set (-> [^0-9] matches any character which is NOT in this set)

const phoneNums = [
  '801-766-9754',
  '435-666-1212',
  '801-796-8010',
  '435-222-8013',
];
const regex = /^801/; // 801 in the beginning of string
const result = phoneNums.filter((value) => regex.test(value)); //  [ '801-766-9754', '801-796-8010' ]

$ // Dollar sign: end of a string
. // Wildcard: represents any single character with the exception of some control characters like new line

const reg = /h.t/g
const text = 'that hot? h@th t hoot'
text.match(reg) // 4 matches: ['hat', 'hot', 'h@t', 'h t']


* // Asterisk or star: matches zero, one or more of the previous
+ // Plus sign: matches one or more of the previous
? // Question mark: matches zero OR one of the previous
=
!
:
| // Vertical bar or pipe symbol: matches previous OR next character/group
\ // Used to escape a special character
/
() // Parenthesis: group characters

[] // Square bracket: defines a set of characters

const reg = /gr[ae]y/g; // a OR e inside [] produces a match for gray OR grey, but NOT graey
const reg2 = /[a-d][ i]/g // matches every position in a text with letter a, b, c OR d followed by whitespace OR i

// important: metacharacters act as literal characters in a set (-> no escaping needed)
const reg3 = /gr[ae]y[ .]/g; // matches 'gray ', 'grey ', 'gray.', 'grey.'

{} // Curly brace: matches a specified number of occurrences of the previous
```

- Escaping metacharacters: use `\<metacharacter>` to include the literal value and NOT the representation (e.g. `* = wildcard`) of a metacharacter in the RegEx pattern

  - if not sure if escaping needed, use nevertheless `\` -> means only that next character is taken literally

  ```JavaScript
  const reg = /d\./g
  const text = 'could word.'
  text.match(reg) // 1 matche: ['d.'] -> without escaping 2 matches: ['d ', 'd.']
  ```

## Control Characters

```JavaScript
\t // tab
\v // vertical tab
\n // newline
\r // carriage return (zu Anfang nächste Zeile)
```

## Character Sets

```JavaScript
// gray or grey
const reg = /gr[ae]y/g; // a OR e inside [] produces a match
```

## Repetition

## Groupings

## Anchored Expressions

## Lookahead Assertions

## Using Unicode

## Useful Regular Expressions
