---
title: 'Javascript Template Literals'
date: "2020-11-30"
tags: ["Development","Software","Web-dev","Javascript, Template Literals, Template Strings"]
categories: ["Development","Software","Web-dev","Javascript"]
---

# Javascript Template Literals

>Template literals are string literals allowing embedded expressions.

You can use multi-line strings and string interpolation features (Expression & variable input) with them.

*These were called* **'Template Strings'** *in prior editions of the **ES2015** sepcification*.


## Table of Contents

- [Syntax](#Syntax)
- [Description](#Description)
- [Multi-line strings](#Multi-line-strings)
- [Expression Interpolation](#Expression-Interpolation)
- [Nesting Templates](#Nesting-Templates)
- [Tagged Templates](#Tagged-Templates)
- [Raw strings](#Raw-strings)


## Syntax

``` javascript
`string text`

`string text line 1
 string text line 2`

`string text ${expression} string text`

tag`string text ${expression} string text`
```

## Description

Template literals use the **Backtick** (` `` `) character to enclose them instead of double or single quotes `""` or `''`.

They can contain placeholders. These are denoted by a dollar sign and curly braces `${expression}`. The expression in the placeholder and the text between the backticks get passed to a function.

``` javascript
`string text ${expression} string text`
```

The default function just concatenates the parts into a single string.

If there is a preceeding expression (Tag); which is called a *tagged template*, the tag expression (normally a function) gets called with the trailing template literal.

``` javascript
tag`string text ${expression} string text`
```

To escape a backtick in a template literal, put a backslash (`\`) before the backtick.

``` javascript
//Example equating escape sequence to single backtick
`\`` === '`' // --> true
```

## Multi-line strings

Examples of using Template Literals with multi-line strings.

``` javascript
//Normal Multi-line string

console.log('string text line 1\n'+
'string text line 2');

// "string text line 1
// string text line 2"
```

``` javascript
//Template literal

console.log(`string text line 1
string text line 2`);

// "string text line 1
// string text line 2"
```

## Expression Interpolation

To embed expressions within normal strings, you would use the following syntax:

``` javascript
let a = 5;
let b = 10;
console.log('Fifteen is ' + (a + b) + ' and\nnot ' + (2 * a + b) + '.');
// "Fifteen is 15 and
// not 20."
```

The same example with Template literals:

``` javascript
let a = 5;
let b = 10;
console.log(`Fifteen is ${a + b} and
not ${2 * a + b}.`);
// "Fifteen is 15 and
// not 20."
```

## Nesting Templates

In some cases Nesting templates is the easiest way to have configurable strings.

Using this example in ES5:

``` javascript
let classes = 'header';
classes += (isLargeScreen() ?
  '' : item.isCollapsed ?
    ' icon-expander' : ' icon-collapser');
```

In ES2015 with nested template literals:

``` javascript
const classes = `header ${ isLargeScreen() ? '' :
  `icon-${item.isCollapsed ? 'expander' : 'collapser'}` }`;
```

## Tagged Templates

A more advanced form of template literals are tagged templates.

Tags allow you to parse template literals with a function. The first argument of a tag function contains an array of string values. The remaining arguments are related to the expressions.

The name of the function used for the tag can be whatever you want.

Here is an example of using a Tagged Template:

``` javascript
let person = 'Mike';
let age = 28;

function myTag(strings, personExp, ageExp) {
  let str0 = strings[0]; // "That "
  let str1 = strings[1]; // " is a "

  // There is technically a string after
  // the final expression (in our example),
  // but it is empty (""), so disregard.
  // let str2 = strings[2];

  let ageStr;
  if (ageExp > 99){
    ageStr = 'centenarian';
  } else {
    ageStr = 'youngster';
  }

  // We can even return a string built using a template literal
  return `${str0}${personExp}${str1}${ageStr}`;
}

let output = myTag`That ${ person } is a ${ age }`;

console.log(output);
// That Mike is a youngster
```

## Raw strings

The special raw property, available on the first argument to the tag function, allows you to access the raw strings as they were entered, without processing escape sequences.

``` javascript
function tag(strings) {
  console.log(strings.raw[0]);
}

tag`string text line 1 \n string text line 2`;
// logs "string text line 1 \n string text line 2" ,
// including the two characters '\' and 'n'
```

In addition, the **String.raw()** method exists to create raw strings, just like the default template function and string concatenation would create.

``` javascript
let str = String.raw`Hi\n${2+3}!`;
// "Hi\n5!"

str.length;
// 6

Array.from(str).join(',');
// "H,i,\,n,5,!"
```