# Safescript - the pure and immutable JavaScript subset

Goals: It takes too long to convince peope to learn Elm. But the principles
of immutability and purity are so good.

Idea: Use a Js subset to bring those principles to developers, with a
syntax they already know. Hopefully this will make it easy for developers
to adjust to these principles. So that when they look at Elm they won't have
to learn the syntax and the concepts but just the syntax.

Since we are focusing on maintaining JS syntax, we want the syntax to be a strict
subset of JS. However, the semantics may differ from the equivalent semantics
in JS.

What can we achieve:
- a syntax that avoids reassignment (immutability)
- a syntax that avoids mutation (immutability)
- a syntax that makes Promises safer (explicit side effects)
- expression over statements
- only arrow functions
- purity???
- types???

Could be a
- linting rule
- compiler
- babel plugin

## Conditions

Sweet JS already does this!!!
Ifs need an else branch. They should return implicitly

```javascript
// BAD
if (isTrue) {
  a = 3;
}

// GOOD: This evalues to 7
if (isTrue) {
  3 + 4;
} else {
  console.log(4);
  2;
}

Idea: Could compile to ternary if that's more performant.

```

## Promises

Promises should be and not do side effects until they are "uncorqued", deviating
from native JS behavior.

We can use promises to wrap side effects into a container that implies impurity.
Kind of like Haskell's `IO a` type.

Force anything that isn't pure through a promise.

## Loops

There should be no `for`, `for .. in` or `while` loops. We allow recursion and `declarative loops`,
and all sort's of pure array methods map, filter etc. (not forEach)

```javascript
// In JS this evaluates to 3, the last value in the array
for (const a of [1, 2, 3]) {
  a
}

// In Safescript this could evaluate to [1, 2, 3], the last value in the array
// maybe returning undefined could filter the value out
for (const a of [1, 2, 3]) {
  a
}
```

## Variables

No `var`, no `let`, no globals, just `const` so that reassignment is not possible. This will
encourage good behavior.
Also forbid property assignment.

```javascript
// GOOD:
const a = 3;
const a = { ...obj, a: 3 };
const a = [...arr, 3];

// BAD:
let a = 3;
var a = 3;
a = 3;
obj.prop = 3;
obj[2] = 3;
```

## A small std lib could help do things in pure ways, easier.

We will only

```javascript
import { core } from 'safescript'

core.Math.random() // returns a promise with a random number
core.Date.now() // returns a promise with a the current date
```

## Switch

Needs a default case, returns the expression(JS does this already). Implies
a break after every `case`.

```javascript
switch ('Peter') {
  case 'Peter':
    3;
  case 'Peter2':
    3;
    break; // break is allowed but not needed
  default: 4;
}
```


## Functions

No function keyword or variations. Only arrow functions used. Implicit return
at end of block but `return` is allowed.

```javascript
// GOOD
const fn = () => {};

// BAD
function fn() {};
new Function('x', 'x + x')
```


