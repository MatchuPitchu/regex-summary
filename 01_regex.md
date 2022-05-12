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
    - `const regex1 = new RegExp('hello');` -> `regex1` becomes regular expression obj
  - `litteral syntax`: recommended way
    - `const regex2 = /world/;`
    - `/` starts and ends a RegEx
    - `regex2` becomes a string obj wrapper
- test RegEx agains value with RegEx object methods:
  - `regex.test(<testValue>)`: returns true if pattern is found in passed string or false if not
  - `regex.exec(<testValue>)`: returns array of matches and additional info (index of match in input, input value)
    - Attention: when uses with RegEx and global flag (see below) and you find multiple matches, then `exec()` only returns first match, when you call it again, then it returns second match and so on
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
const txt = 'Programming courses always startS with hello world.'

const sFollowedBySpace = /s\s/g;
const sInsensitivFollowedBySpace = /s\s/gi;

console.log(txt.match(sFollowedBySpace)); // -> 2 matches
console.log(txt.match(sInsensitivFollowedBySpace)); // -> 3 matches
```

## How RegEx Engine Executes Matching

- engine checks character by character from the beginning if pattern matches

  - Process: checks first character (-> every character, not only letters) of RegEx with first character of text -> if match, then check second letter of text -> if match, then check third letter of text -> if no match, then it returns to second character of text and starts checking once again against first character of RegEx -> if no match, continue with next character of text against first character of RegEx -> if no match, continue ... -> if once complete pattern matches, then first match is returned -> if `global flag` is set, it continues checking for matches

  ```JavaScript
  const regex = /hello/;
  const text = 'here help hello';
  regex.test(text);
  ```

## Metacharacters: Overview, more specific below

- in comparison to literal characters (-> e.g. simple letter or string like `@h_e-llo1!`), metacharacters are used to represent other characters to make RegEx pattern more flexible for matches

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

  const regex = /h.t/g
  const text = 'that hot? h@th t hoot'
  text.match(regex) // 4 matches: ['hat', 'hot', 'h@t', 'h t']

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

- Escaping metacharacters: use `\<metacharacter>` to include the literal value and NOT the representation (e.g. `* = wildcard`, `\* = *`) of a metacharacter in the RegEx pattern

  - if not sure if escaping needed, use nevertheless `\` -> means only that next character is taken literally

  ```JavaScript
  const regex = /d\./g
  const text = 'could word.'
  text.match(regex) // 1 match: ['d.'] -> without escaping 2 matches: ['d ', 'd.']
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
const regex1 = /gr[ae]y/g; // a OR e inside [] produces a match for gray OR grey, but NOT graey
const regex2 = /[a-d][ i]/g // matches every position in a text with letter a, b, c OR d followed by whitespace OR i
const regex3 = /[0-9a-zA-Z]/g; // matches all single characters when they are in range of 0-9, a-z or A-Z
const regex4 = /1[0-5]/g; // matches all numbers from 10 to 15
```

- exclude a character set with `^`

  ```JavaScript
  const regex = /0x[^0-9A-F]/g; // matches 0x + NO 0-9A-F as third character (e.g. matches 0xg)
  ```

- important: metacharacters act as literal characters in a set (-> no escaping needed)

  ```JavaScript
  const regex = /gr[ae]y[ .]/g; // matches 'gray ', 'grey ', 'gray.', 'grey.'
  ```

- exception: metacharacters that you MAY need to escape in a set

  - `-`: if it's obvious that hyphen is in a position to indicate a range, but you wanna use it as literal character, then you have to escape it (e.g. `/[0\-4]/` -> matches 0, - or 4)

    ```JavaScript
    const regex1 = /[1-4]/; // matches single number between 1 and 4
    const regex2 = /[1\-4.]/; // matches 1, -, 4 or .
    const regex3 = /[-.]/; // matches \ or .
    ```

  - `^`: if you use it at the start of a set, but you wanna use it as literal character, then you have to escape it (e.g. `/[\^0-2]/g` -> matches single caracter ^ or 0-2)
  - `\`, `]`: always needed to escape it if you wanna use it as literal character

### Shorthand for Character Sets

- `\d` = `[0-9]`
- `\w` = `[a-zA-Z0-9_]` -> Attention: `äöüÄÖÜ` NOT included
- `\s` = `[ \t\r\n]`
- `\D`, `\W`, `\S` -> negate set like `\D = [^0-9]` etc.

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
  - `/a[a-z]+/`: matches all words starting with 'a' and next characters are lowercase letters -> e.g. 'als', 'aber' etc.
- `?`: matches zero OR one occurrence of the prev left character
  - `/a[a-z]?/`: matches e.g. 'a', 'ab', 'ak' etc.
  - `/apples?/`: matches singular or plural of apple
- `*`: matches zero OR more occurences
  - `/a[a-z]*/`: matches e.g. 'a', 'ab', 'aaa', 'aber' etc.
  - `/warning!*/`: matches e.g. 'warning', 'warning!!!!' etc.

### Specifying Repetition Amount

- `{min, max}`: matches min to max occurrences
- `{x}`: matches exactly x occurrences
  - `/#[0-9A-Z]{6}/gi`: matches hex color codes e.g. #ff0000, #C0C0C0
- `{min,}`: matches min or more occurrences

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

  const regex = /\d{2,3}?-/g; // with following hypen, lazy "?" is not important anymore
  const text = '12- 23344 244-'
  text.match(reg) // ['12-', '244-']
  ```

## Anchored Expressions

- allows to match at start and end of string
  - `^`: anchors match to the start of the line
  - `$`: anchors match to the end of the line
- use both to restrict exactly length and shape of a string
  - `/^\d{2}$/`: matches distinct strings '12', '25' etc.
- `/^The/m`: with multi-line flag matches string 'The' at the start of each NEW line (nach Zeilenumbruch in one string)

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

## Specifying Options with `|` (OR operator)

- `|`: with OR operator, express alternatives in RegEx pattern to cover more exactly possible targets

```JavaScript
const regex = /\bmonday|tuesday|wednesday|thursday|friday|saturday|sunday\b/gi;

// alternate through expressions with '|'
const regex2 = /\b[a-z]{3}day\b|\b[a-z]{4}\b|\b[a-z]{6}day\b/gi;
```

## Groupings

- `(<regex>)`: group an expression together in a RegEx
- advantages:

  - a metacharacter would apply to the previous left group with expressions

    ```JavaScript
    // example: find 'text' pattern with a RegEx
    const text = 'a5c3a2b1d1'
    const regex = /([a-d][1-5]){5}/g

    // example: avoid to define word boundaries (\b) on every alternative
    const regex2 = /\b(monday|tuesday|wednesday)\b/gi;
    ```

  - you can capture a portion of a string for later use

    ```JavaScript
    // example: extract year, month, day without string split method and instead use grouping
    const format = 'yyyy/mm/dd' // separator could be everything, m and d could be 1 or 2 digits

    const data = '2018/3/9'
    const regex = /^(\d{4})[-./](\d{1,2})[-./](\d{1,2})$/
    const array = regex.exec(data); // ["2018/3/9", "2018", "3", "9"] -> thanks to grouping, arr of matching results includes also groups
    ```

  - you can repeat a captured text of a group with a `group backreference` (-> `\NumberOfGroupInRegExPattern`)

    - NOTICE: you're NOT repeating the pattern (-> like `{x}`), you are repeating the actual captured text (-> if `9` is captured text based on a pattern, then `9` is repeated)

    ```JavaScript
    const regex = /(yo)\1/g
    const text = 'yoyo'
    text.match(regex) // ['yoyo']

    // example: match date with same month and day
    const regex1 = /^(\d{4})[-./](\d{1,2})[-./]\2$/ // \2 repeats captured text of 2. group (-> '(\d{1,2})')
    const text1 = '2018-9-9' // matches, because m and d is '9'

    // example: match opening and closing HTML tag and whole text inside
    const regex2 = /<(\w+>)[\w\s]+<\/\1/g;
    const text2 = '<p>This is a paragraph</p>'
    regex2.exec(text2) // ['<p>This is a paragraph</p>']
    ```

```JavaScript
// example: iterate through the data provided. Use RegEx to store names in new Array but change the order of the name so first name is listed first and last name is last
const data = ["Jensen, Dale", "Smith, Andrea", "Jorgensen, Michael", "Vasefi, Annika", "Lopez, Monica", "Crockett, Steven"];
const regex = /(?<last>\w+), (?<first>\w+)/; // global flag "g" not needed, could cause problem (look above info for exec())
// a)
const result = data.map((value) => {
  const matchResults = regex.exec(value);
  if(matchResults !== null) {
    return `${matchResults.groups.first} ${matchResults.groups.last}`; // OR: `${matchResults[2]} ${matchResoults[1]}` without naming groups
  } else return null
})

// b)
const result = data.map((value) => value.replace(regex, '$2 $1')); // NOT working with naming groups (?<first>), only with numbers of groups
```

### Non-capturing group

- `(?:<regex>)`
  - matches `regex` but does NOT remember the match
  - matched substring cannot be recalled from resulting array's elements `([1], ..., [n])`
    - is NOT in output of `exec()` and
  - matched substring cannot be recalled from predefined RegExp obj properties `($1, ..., $9)`
    - can NOT referenced with `/NumberOfGroupInRegExPattern`

### Naming Groups

- `(?<NAME>...)` + `\k<NAME>` to name and refere to captured groups

  ```JavaScript
  // example: match opening and closing HTML tag and whole text inside
  const regex = /<(?<tag>\w+>)[\w\s]+<\/\k<tag>/g;
  ```

### Lookahead Group

- `(?=<regex>)`: positive lookahead group; a string is only a match if it is followed by this group, BUT this group will not be a part of the final matching result

  ```JavaScript
  // example: capture a domain name without the top level domain .com
  const regex = /\w+(?=\.com)/g;
  const text = 'www.youtube.com https://google.com posteo.com'
  regex.exec(text); // ['youtube', 'google', 'posteo']
  ```

  - example: define a pattern for a password with help of lookahead groups
    - NOTICE: string is finally captured only for last expression (`.*`), BUT only if lookahead conditions are fullfilled for string
    - `(?=.{8,})`: looks if string has at least 8 characters, BUT this group does NOT capture any of those
    - `(?=.*[A-Z])`, `(?=.*[a-z])`, `(?=.*[0-9])`: 2. until 4. group starts also directly in beginning of string (`^`), because a lookahead group does NOT capture anything;
      - meaning: 0 or more wildcard characters, but at least a) 1 uppercase letter, b) 1 lowercase letter, c) 1 number
    - `.*`: at end of string could be 0 or more wildcard characters

  ```JavaScript
  const regex = /^(?=.{8,})(?=.*[A-Z])(?=.*[a-z])(?=.*[0-9]).*$/; // min 8 characters, includes min 1 uppercase, min 1 lowercase, min 1 number
  const password = 'aB1abc23';
  ```

- `(?!<regex>)`: negative lookahead group; force the left previous pattern to only be a match if it did NOT include the negative lookahead group

  ```JavaScript
  const regex = /^(?=.{8,})(?=.*[A-Z])(?=.*[a-z])(?!.*[0-9]).*$/;
  // `(?!.*[0-9])`: final match does NOT include a number
  ```

### Lookbehind Group

- `(?<=<regex>)`: positive lookbehind group; similar to lookahead group, BUT match must contain the pattern of the lookbehind group before it instead of after it
  - Attention: only supported in JavaScript with ES2018

```JavaScript
// example: match only numbers if $ or € sign is before
const regex = /(?<=\$|€)\d+/g;
const text = '$10 costs $50: 20';
text.match(regex); // ['10', '50']
```

- `(?<!<regex>)`: negative lookbehind group

## Using Unicode

- unicode provides a standard uniform way for representing characters
- specifiying unicode characters: `\UNICODE` (e.g. `\u0065`)
- use case for unicode: characters that are not so easy to type with the keyboard; so you can copy & paste these charaters or search for the unicode <https://unicode-table.com/de/>
  - `/[\u201C\u201D]/g` -> opening and closing quote

### ES6 Unicode and u Flag

- some extended Unicodes (with more than 4 characters) and the associated new `u flag` were introduced with ES6

  - Violinschlüssel: `/\u{1D11E}/`

  ```JavaScript
  const gclef = "𝄞-clef";
  const regex1 = /^.-clef/;
  regex1.test(gclef); // false, since without u flag Violinschlüssel is considered as two charaters (since unicode is longer than 4 characters)

  const regex2 = /^.-clef/u;
  regex2.test(gclef); // true

  const regex3 = /^\u{1D11E}/u;
  regex3.test(gclef); // true
  ```

## Useful Regular Expressions

### Matching Email Address

```JavaScript
const regex = /.+@.+\..+/g; // simple pattern

const regex2 = /^[^\s@]+@[^\s@.]+\.[^\s@.]+$/g; // exclude in some sets space, @ or .
```

### Matching a Twitter Name

```JavaScript
const txt = '@flohr_mi';
const regex = /^@?(\w+)$/g; // 0 or 1 @, captures group containing only the name without '@'
```

### Testing Passwords

```JavaScript
// 1) Create multiple RegEx, one for each criteria and test password against all
const txt = 'A2BC3/>_zyxd';
const regex1 = /^.{8,32}$/g; // length 8-32
const regex2 = /\w/g; // letter or underscore
const regex3 = /\d/g; // digit
const regex4 = /[^\w\s]/g; // every non word character and no space

// 2) Create one RegEx with
const regex = /^(?=.{8,})(?=.*[A-Z])(?=.*[a-z])(?=.*[0-9])(?=.*[^\w\s]).*$/; // min 8 characters, includes min 1 uppercase, min 1 lowercase, min 1 number, min 1 special character
```

### Using String.replace()

```JavaScript
// 1) Basic example
const txt = '<b>This is Bold</b>';
const regex = /b>/g;
const newTxt = txt.replace(regex, 'strong');

// 2) Switching first and last name
const data = ["Smith, Andrea", "Jorgensen, Michael", "Vasefi, Annika"];
const regex = /(\w+), (\w+)/g
const newData = data.map((value) => value.replace(regex, '$2 $1')); // NOT working with naming groups (?<first>), only with numbers of groups
```

### Matching a Word by another Word

```JavaScript
// 1) Match hello ... world
const txt = 'Hello more text world is nice.';
const regex = /\b(?:hello\W+(?:\w+\W+){0,5}world)\b/ig; // (?:\w+\W+){0,5} -> 0-5 words in between, but not part of the results as own(!) groups

// 2) Match hello ... world AND world ... hello
const txt = 'World more text hello is nice.';
const regex = /\b(?:hello\W+(?:\w+\W+){0,5}world)|(?:world\W+(?:\w+\W+){0,5}hello)\b/ig;
```

### Validating Dates

```JavaScript
// Match dd/mm/yyyy AND d/m/yy AND any other combination in between, but yyy is not valid
const regex = /^(?<day>3[01]|[12][0-9]|0?[1-9])\/(?<month>1[0-2]|0?[1-9])\/(?<year>[0-2][0-9]{3})$/g;
```

### Capturing Matched Text

```JavaScript
// Extract all numbers and capture those numbers, then sum the numbers
const phrase = "First number: 32; second number 100. Last number is 15.";
const regex = /\d+/g;
const result = phrase.match(regex).reduce((acc, currentValue) => +acc + +currentValue); // 147
```

### Discovering Information about a Match

```JavaScript
// Retrieve starting index of match, the length of the match and the actual match
const phrase = "First number: 32; second number 100. Last number is 15.";
const regex = /\d+/; // no global flag
const result = regex.exec(phrase)
console.log(result.index); // starting index of match: 14
console.log(result[0].length); // length of the match: 2
console.log(result[0]); // "32"
```

### Iterating over Matches

```JavaScript
// Iterate over each match and log the information to the console
const phrase = "First number: 32; second number 100. Last number is 15.";
const regex = /\d+/g;
let match;
// when used global flag with regex, exec() creates and then updates for regex Obj with each execution the property of "lastIndex" which is the first index after the last match (in other words: it's the last index where the last match stopped)
while (match = regex.exec(phrase) !== null) { // if no match, null is returned
  console.log('Match:', match[0]);
  console.log('LastIndex:', regex['lastIndex']); // 16, 35, 54
}

const iterator = phrase.matchAll(regex);
console.log(iterator.next()); // Obj with property "value", that's an array with other properties
// with each call of next(), you are moving through matches of the string
```
