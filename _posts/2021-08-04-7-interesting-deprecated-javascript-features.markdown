---
layout: post
title: "7 interesting deprecated JavaScript features"
date: 2021-08-04 17:29:55 +0200
tags: javascript history web-development
categories: software-development
---

Since it’s birth 26 years ago at Netscape, JavaScript has come a long way. A language which was used only to interact with Java applets and do simple DOM manipulation is now used for writing back-ends and desktop and mobile applications too. Ecosystem grew by a big margin as well as the community. Just like every other language, JavaScript had (and still has) rough edges and quirks. We’re stuck with some of them because of the backward compatibility. Some are, (un)fortunately, mostly or completely gone. Some of these can still be used but it’s highly discouraged.

&nbsp;
## `Object.prototype.watch` and `Object.prototype.unwatch` methods

Unce upon a time there was an easy way to watch for the property changes on an object.

```js
var cat = {};
cat.watch("name", function (propertyName, previousValue, newValue) {
  return "Mr. " + newValue;
});

cat.name = "Oswald";
console.log("Hello " + cat.name + "!"); // Hello Mr. Oswald!

cat.unwatch("name");

cat.name = "Luna";
console.log("Hello " + cat.name + "!"); // Hello Luna!
```

**Alternative**

Nowadays you can use `Proxy` for this purpose.

```js
const handler = {
  set: (obj, prop, value) => {
    if (prop === 'name') {
      obj[prop] = `Mr. ${value}`;
    }
  }
};

let cat = new Proxy({}, handler);

cat.name = "Oswald";
console.log("Hello " + cat.name + "!"); // Hello Mr. Oswald!

cat = { ...cat }; // this will remove behavior imposed by Proxy

cat.name = "Luna";
console.log("Hello " + cat.name + "!"); // Hello Luna!

```
&nbsp;

## `with` statement

We all know how horrible working with long chain of properties can be. Fortunately, there is a way around it. Unfortunately – you shouldn’t use it.

```js
const cat = {
  details: {
    passport: {
      location: {
        city: 'New York'
      }
    }
  }
};

with (cat.details.passport.location) {
  city = 'Meowyork';
}
```

There are two reasons why you shouldn’t use `with` statement.

* There is no place for optimization inside it, since you can’t predict if variable will refer to a property or to an outside variable.
* It violates lexical scope, making program analysis very hard or even infeasible.

Also, it will be impossible for you to use it in ES6+ (or ES5 with the strict mode turned on). Strict mode prohibits it’s usage.

**Alternative**

The best you can do is to declare a variable which will hold the chain of properties instead.

```js
const cat = {
  details: {
    passport: {
      location: {
        city: 'New York'
      }
    }
  }
};

const catLocation = cat.details.passport.location;
catLocation.city = 'Meowyork';
```
&nbsp;
## Expression closures

Long before arrow functions were even in a plan, there were expression closures. They allowed you to omit curly braces and return statements from the method definitions completely.

```js
var cat = function() "Luna";

var favorites = {
  food: function() "Tuna"
};
```

**Alternative**

This has been removed in favor of using standard ES syntax.

```js
var cat = function() { return "Luna"; }

var favorites = {
  food: function() { return "Tuna"; }
};
```

Nowadays you can also use arrow functions and method definitions (both introduced in ES6).

```js
const cat = () => "Luna";

const favorites = {
  get food() { return "Tuna"; }
};
```
&nbsp;
## `Object.observe` and `Object.unobserve` methods

Back in the days there was also an easy way of getting an information about any changes to an object.

```js
var cat = {
  name: "Oswald"
};

Object.observe(cat, function(changes) {
  console.log(changes);
});

cat.name = "Luna";
// [{ name: 'name', object: <obj>, type: 'update', oldValue: 'Oswald' }]

Object.unobserve(cat);

cat.name = "Max";
```

There were the similar methods for arrays too – `Array.observe` and `Array.unobserve`.

**Alternative**

You can do this with `Proxy` too.

```js
const cat = new Proxy({ name: "Oswald" }, {
  get: (target, prop) => {
    console.log({ type: "get", target, prop });
    return Reflect.get(target, prop);
  },
  set: (target, prop, value) => {
    console.log({ type: "set", target, prop, value });
    return Reflect.set(target, prop, value);
  }
});

cat.name = "Luna";
// { type: 'set', target: <obj>, prop: 'name', value: 'Luna' }

cat.name;
// { type: 'get', target: <obj>, prop: 'name' }
```
&nbsp;
## `let` expressions and `let` blocks

In ES6, two statements for declaring block-scoped variables have been introduced; `let` and `const`. For the brief period of time, there were non-standard extensions to the let statement. These were `let` expressions and `let` blocks.

`let` blocks provided a way to instantiate a block where variables can have different values, without affecting the same-named ones outside of that block.

```js
var catName = "Oswald";
var catAge = 2.5;

let (catName = "Luna", catAge = 2) {
  console.log(catName + "(" + catAge + " years old)"); // Luna (2 years old)
}

console.log(catName + "(" + catAge + " years old)"); // Oswald (2.5 years old)
```

`let` expressions did the similar thing but on the expression level.

```js
var catName = "Oswald";

let(catName = "Luna") console.log(catName); // Oswald

console.log(catName); // Luna
```

**Alternative**

Since `let` is block scoped you can just declare variables again inside inner scope and change them there.

```js
let catName = "Oswald";
let catAge = 2.5;

{
  let catName = "Luna", catAge = 2;
  console.log(catName + "(" + catAge + " years old)"); // Luna (2 years old)
}

console.log(catName + "(" + catAge + " years old)"); // Oswald (2.5 years old)
```
&nbsp;
## HTML wrapper methods on strings

They are basically bunch of methods which wrapped your string with tags like `bold`, `blink`, `font`, `small`, `big`, `i` etc.

```js
"Some teeny-tiny text".fontsize(3);    // <font size="3">Some teeny-tiny text.</font>
"Some tiny text.".small();             // <small>Some tiny text.</small>
"Some yuuuge text.".big();             // <big>Some yuuge text.</big>
"Talk to the hand!".bold();            // <b>Talk to the hand!</b>
"You have been terminated.".blink();   // <blink>You have been terminated.</blink>
```

**Alternative**

There are no alternatives for this monstrosity.

&nbsp;&nbsp;

## `ParallelArray`

This one was an experimental feature introduced by Mozilla in the Firefox (specifically, version 17 of the Gecko engine). It’s purpose was to enable data-parallelism by executing multiple functions in parallel. If it wasn’t possible, they would be executed in the sequential order.

```js
var cats = new ParallelArray(["Oswald", "Luna", "Max"]);
cats.map(function(name) {
  return "😸 " + cat;
});
```

**Alternative**

Today you can use `Promise.all` to accomplish this.

&nbsp;

## Conclusion

It’s fantastic to see how much JavaScript has progressed for the last 26 years. Who could’ve thought that language made in 10 days can become one of the most dominant in the industry? I believe it’s a good practice to take a step back and see how things worked in the past. That can teach us not to repeat the same mistakes anymore. It can also make us more grateful for the things we have today. Even though I have a fair share of criticism for JavaScript, I can’t wait to see what’s coming in the next two decades.