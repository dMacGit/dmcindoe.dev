---
title: 'Summary Notes on Truthy & Ternary Concepts in Javascript'
draft: false
author: "David"
date: 2020-06-24
authorlink: ""
permalink: /posts/2020/06/24/Javascript-Truthy-and-Ternary-concepts/
description: "Some notes on Truthy & Ternary Concepts"
featuredImage: "/images/js-logo.png"
featuredImagePreview: "/images/js-logo.png"
tags: ["Development","Software","Web-dev","Javascript"]
categories: ["Development","Software","Web-dev","Javascript"]
toc:
  auto: true
---
A breakdown of Truthy & Ternary Concepts in Javascript.
<!--more-->
## Truthy & Falsy Evaluations

Taking the notion that when a variable is created, even without being assigned a value, it can still be evaluated. Most un-assigned values to variables are evaluted as **false** and thus can be said to be **Falsy**.
See below list for examples. 
When a variable is assigned a value, (Not matching the exmaples from the list) the variable is evaluated to **true**. And it can be said that the variable is **Truthy**.

Example of **Falsy** evaluated values are as follows:

- **0**
- Empty strings like **""** or **''**
- **null** which represents when there is no value at all
- **undefined** which represents when declared variable lacks a value.
- **NaN**, or Not a Number.

**Example Code Block**

``` javascript
let favoritePhrase = ''; // Evaluates to 'false'. Is Falsy

// The Else clause will trigger as the variable 'favouritePhrase' is falsy, and the conditional evaluates to false. 
// if (favoritePhrase) === if ('') === if (false)

if (favoritePhrase) {
  console.log("This string doesn't seem to be empty.");
} else {
  console.log('This string is definitely empty.');
}
```

## Truthy and Falsy Assignment

You can write shorthand code for some forms of variable assignements, using logical operators, and **Short-circuit evaluation**.
For example if you take the code block for **username** assignment to a variable.

``` javascript 
let defaultName;
if (username) {
  defaultName = username;
} else {
  defaultName = 'Stranger';
}
```

Using logical operation as well as Truthy & Falsy assinment priciples whe can simplify this to a line.

``` javascript
let defaultName = username || 'Stranger';
```

Because **||** or statements check the left-hand condition first, the variable **defaultName** will be assigned the actual value of **username** if is **truthy**, and it will be assigned the value of **'Stranger'** if **username** is **falsy**. This concept is also referred to as **short-circuit evaluation**.

**Another Example Block**

``` javascript
let tool = 'marker';

// Using short-circuit-evaluation to assign  writingUtensil variable below:
let writingUtensil = tool || 'pen'

console.log(`The ${writingUtensil} is mightier than the sword.`);
```

## Ternary Operator

The ternary operator allows for a compact syntax in the case of binary (choosing between two choices) decisions. It accepts a condition followed by a `?` operator, and then two expressions separated by a `:`. If the condition evaluates to **truthy**, the first expression is executed, otherwise, the second expression is executed.

``` javascript 
let price = 10.5;
let day = "Monday";

day === "Monday" ? price -= 1.5 : price += 1.5;
```

**Other Example code blocks**

``` javascript
let isLocked = false;

isLocked ? console.log('You will need a key to open the door.'):
console.log('You will not need a key to open the door.');


let isCorrect = true;

isCorrect ? console.log('Correct!') :
console.log('Incorrect!');

let favoritePhrase = 'Love That!';

favoritePhrase === 'Love That!' ? console.log('I love that!') :
console.log("I don't love that!");
```
