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

## Recommendations for Accurate Regular Expressions

- when possible, define the quantity of repeated expressions
- narrow the scope (-> wildcard, number, letters, specific number ...) of repeated expressions
- provide clear starting and ending points (-> anchors `^`, `$`, `\b` or `\B`)
- test RegEx with multiple data sets

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
const text = 'here help hello';
foo.test(text);
```

## Metacharacters

- in comparison to literal characters (-> e.g. simple letter or string like `hello`), metacharacters are used to represent other characters to make RegEx pattern more flexible for matches

```JavaScript
^ // Caret: a) beginning of a string OR b) exclude a character set (-> [^0-9] matches any character which is NOT in this set, in other words: next character can NOT come from this set)

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
? // Question mark: a) matches zero OR one of the previous, b) forces previous left expression in a pattern to be lazy
=
!
:
| // Vertical bar or pipe symbol: matches previous OR next character/group
\ // Used to escape a special character
/ // start and end of regex definition
() // Parenthesis: group characters
[] // Square bracket: defines a set of characters
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
\s // space: includes whitespace, tab, newline, carriage return
```

## Character Sets

- is defined by square brackets `[]`

```JavaScript
const reg = /gr[ae]y/g; // a OR e inside [] produces a match for gray OR grey, but NOT graey
const reg2 = /[a-d][ i]/g // matches every position in a text with letter a, b, c OR d followed by whitespace OR i

// important: metacharacters act as literal characters in a set (-> no escaping needed)
const reg3 = /gr[ae]y[ .]/g; // matches 'gray ', 'grey ', 'gray.', 'grey.'
// exception: hyphen (-) acts as metacharacter to indicate a range; need to escape it if literal character is wished, but is only mandatory where no confusion with range indication could happen
const reg4 = /[1-4]/; // matches single number between 1 and 4
const reg5 = /[1\-4.]/; // matches 1, -, 4 or .
const reg6 = /[-.]/; // matches \ or .
const reg7 = /[0-9a-zA-Z]/g; // matches all single characters when they are in range of 0-9, a-z or A-Z
const reg8 = /1[0-5]/g; // matches all numbers from 10 to 15
```

- exclude a character set with `^`

```JavaScript
const reg = /0x[^0-9A-F]/g; // matches 0x + NO 0-9 or A-F as third character (e.g. 0xG)
```

- metacharacters that you MAY need to escape in a set
  - `-`: if it's obvious that hyphen is in a position to indicate a range, but you wanna use it as literal character, then you have to escape it (e.g. `/[0\-4]/` -> matches 0, - or 4)
  - `^`: if you use it at the start of a set, but you wanna use it as literal character, then you have to escape it (e.g. `/[\^0-2]/g` -> matches single caracter ^ or 0-2)
  - `\`, `]`: always needed to escape it if you wanna use it as literal character

### Shorthand for Character Sets

- `\d` = `[0-9]`
- `\w` = `[a-zA-Z0-9_]`
- `\s` = `[ \t\r\n]`
- `\D`, `\W`, `\S` = negate set like `[^0-9]` etc.

```JavaScript
// Exercise: pattern for telephone numbers starting with 801 and having this format nnn-nnn-nnnn

const phoneNums = [
  '801-766-9754',
  '801-545-5454',
  '435-666-1212',
  '801-796-8010',
  '435-555-9801',
  '801-009-0909',
  '435-222-8013',
  '801-777-66553',
  '801-777-665-',
  '801-77A-6655',
  '801-778-665',
];

const regex = /^801-\d{3}-\d{4}$/;

const result = phoneNums.filter((value) => regex.test(value)); // ['801-766-9754', '801-545-5454', '801-796-8010', '801-009-0909']
```

## Repetition in Patterns

- `+`: matches one OR more occurrences of the prev left character
  - `/[A-Z]+/`: matches e.g. 'AB', 'ABC', 'AAAA' etc.
  - `/[A-Z][a-z]+/`: matches words starting with uppercase e.g. 'Auto' etc.
  - `/a[a-z]+/`: matches all words starting with a until next character that is NOT a lowercase letter -> e.g. 'als', 'aber' etc.
- `?`: matches zero OR one occurrence of the prev left character
  - `/a[a-z]?/`: matches e.g. 'a', 'ab', 'ak' etc.
  - `/apples?/`: matches singular or plural of apple
- `*`: matches zero OR more occurences
  - `/a[a-z]*/`: matches e.g. 'a', 'ab', 'aaa', 'aber' etc.
  - `/warning!*/`: matches e.g. 'warning', 'warning!!!!' etc.

### Greediness and Laziness in Regular Expressions

- `Greediness`: by default(!) RegEx are greedy (= gierig), i.e. they try to match as many characters as possible

  - matching engine grabes first whole line and then returns character by character to find the `<\/p>`

  ```JavaScript
  const regex = /<p>.*<\/p>/;
  const text = '<p>This is first paragraph.</p><p>Paragraph number two.</p>'
  text.match(reg) // matches whole text line, even if </p> is in between, acts like /<p>.*/
  ```

- `Laziness`: you can make RegEx lazy (= faul) with `?`, i.e. they try to match as litte as possible

  - matching engine takes first complete match for pattern (`<p>`) and then searches character by character `<\/p>`

  ```JavaScript
  const regex = /<p>.*?<\/p>/; // add ? after * to make this RegEx pattern lazy
  const text = '<p>This is first paragraph.</p><p>Paragraph number two.</p>'
  text.match(reg) // matches only first <p>...</p> or both <p>...</p> with global flag
  ```

  ```JavaScript
  // example: lazy digits pattern
  const regex = /\d{2,3}?/g; // ? makes, that pattern matches minimum, here only 2 digits
  const text = '1 23344 24'
  text.match(reg) // ['23', '34', '24']

  const regex = /\d{2,3}?-/g; // with following hypen, lazy "?" is not import anymore
  const text = '12- 23344 244-'
  text.match(reg) // ['12-', '244-']
  ```

### Specifying Repetition Amount

- `{min, max}`: matches min to max occurrences
- `{x}`: matches exactly x occurrences
  - `/#[0-9A-Z]{6}/gi`: matches hex color codes e.g. #ff0000, #C0C0C0
- `{min,}`: matches min or more occurrences

## Anchored Expressions

- allows to match at start and end of string
  - `^`: anchors match to the start of the line
  - `$`: anchors match to the end of the line
- use both to restrict exactly length and shape of a string
  - `/^\d{2}$\`: matches distinct strings '12', '25' etc.
- `/^The\m`: with multi-line flag matches string 'The' at the start of each NEW line (nach Zeilenumbruch in one string)

## Word Boundaries

- to ensure that pattern matches only an entire word

  - `\b`: word boundary; pattern bounded by a non-word character
  - `\B`: Nonword boundary; pattern bounded by a word character
  - these metacharacters reference a position, NOT an actual character

  ```JavaScript
  const regex = /\bplan\b/g;
  const text = 'Inplant this idea: plan to plant multiple trees on this planet.'
  text.match(regex) // ['plan'] -> matches only exactly this word consisting of word caracters (= \w) and left and right non-word characters

  const regex2 = /\bplan to\b/g;
  text.match(regex2) // ['plan to']

  const regex3 = /\bplan/g;
  text.match(regex3) // ['plan', 'plan', 'plan'] -> matches all 'plan' with at left side a non-word character

  const regex4 = /\Bplan\B/g;
  const text2 = 'Inplant this idea'
  text2.match(regex4) // ['plan'] -> matches 'plan' inside 'Inplant' since 'plan' is bounded at left and right by word characters

  // exercise: Replace days of the week in a text with 'Monday'
  const regex5 = /\b[mtwfs][a-z]{1,4}[nsir]day\b/gi;
  const text3 = 'Each and every Tuesday, at the beginning of the day, we hold a staff meeting. At the Friday staff meeting, you will receive assignments for the following Thursday.';
  const newText = text3.replace(regex5, "Monday");
  ```

## Groupings

## Lookahead Assertions

## Using Unicode

## Useful Regular Expressions
