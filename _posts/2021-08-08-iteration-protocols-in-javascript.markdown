---
layout: post
title: "Iteration protocols in JavaScript"
date: 2021-08-08 18:38:46 +0200
tags: javascript
categories: software-development
---

No matter on which level you are as a JavaScript developer, you have used iterators and iterables so far, even though you may haven’t been aware of that. But what exactly they are and what’s their purpose?

&nbsp;
## Iterables

Each object which implements `@@iterator` method (expressed via `[Symbol.iterator]`) is an *iterable*. It serves as a definition for the behavior which object will have when it’s iterated on (for example with the `for...of` statement). There are built-in iterables like `String`, `Map`, `Set`, `Array`, `TypedArray` and others but you can build your own too.

```js
let runningStats = {
  Mike: 6,
  Emma: 9,
  Billy: 11,
};

// creates an iterable which will return custom objects
runningStats[Symbol.iterator] = () => {
  let i = 0;
  const pairs = Object.entries(runningStats);

  return {
    next: () => {
      // signal that iterating has been finished
      if (i === pairs.length) {
        return { value: undefined, done: true };
      }

      let currentPair = pairs[i++];

      return {
        value: { name: currentPair[0], kilometers: currentPair[1] },
        done: false,
      };
    }
  }
};

for (const personStats of runningStats) {
  console.log(personStats);
}
```

Which will give us the following output:

```js
{ "name": "Mike", "kilometers": 6 }
{ "name": "Emma", "kilometers": 9 }
{ "name": "Billy", "kilometers": 11 }
```

Therefore, we can say that iterable is each object which conforms to the *iterable protocol* described above. You can look at the protocols as some kind of interfaces. And since strings and sets for example are already iterables, you can iterate over them without defining `[Symbol.iterator]` method:

```js
const str = "word";

for (const char of str) {
  console.log(char);
}

const set = new Set([1, 1, 2, 2, 3, 3]);

for (const number of set) {
  console.log(number);
}
```

Output:

```
w
o
r
d
1
2
3
```

Fun fact: `Set` and various other iterables accept iterables as an argument. You would be able too see it in the `Set` example above by passing a string or a map. Sometimes there are limitations though – `Map` for example accepts only array-like iterables.

&nbsp;
## Iterators

If you take a closer look at the example of the iterable above you’ll see that we return an object with the `next()` method. That object is an *iterator*. Of course, not every object which has the `next()` method is an iterator. Your method needs to return an object which contains at least following two properties; `value` (any JavaScript value) and `done` (boolean). Not doing so would result in a `TypeError` when the method is called. This is called *iterator protocol*.

Let’s see how we can get the iterator from the iterable we made above.

```js
const iterator = runningStats[Symbol.iterator]();

console.log(iterator.next()); // { value: { "name": "Mike", "kilometers": 6 }, done: false }
console.log(iterator.next()); // { value: { "name": "Emma", "kilometers": 9 }, done: false }
console.log(iterator.next()); // { value: { "name": "Billy", "kilometers": 11 }, done: false }
console.log(iterator.next()); // { value: undefined, done: true }

// Any subsequent calls of the next() method will return the same result
console.log(iterator.next()); // { value: undefined, done: true } 
```

Using iterators directly like this could be useful when we want to skip certain element(s) when looping over an iterable.

```js
const food = ["carrot", "apple", "banana", "plum", "peach"];

const iterator = food[Symbol.iterator]();
iterator.next(); // skip the first one

for (const fruit of iterator) {
  console.log(fruit);
} 
```

Which would give us the following output:

```
apple
banana
plum
peach
```

&nbsp;
### Infinite iterators

You don’t need to impose limits on the number of elements in your iterators. Sometimes it’s useful to have infinite iterators which we can use multiple times.

```js
const infiniteList = (start) => {
  let value = start;

  return {
    next: () => ({ value: value++, done: false }),
  };
}

const iterator = infiniteList(6);

for (const _ of new Array(100)) {
  iterator.next();
}

console.log(iterator.next().value); // 106
```

Okay, so let’s try to use `for...of` statement to loop over this iterator – at the end, it’s more elegant, isn’t it?

```js
const infiniteList = (start) => {
  let value = start;

  return {
    next: () => ({ value: value++, done: false }),
  };
}

const iterator = infiniteList(6);

for (const element of iterator) {
  console.log(element);
}
```

And run it...

![typeerror iterator not iterable](/assets/iterator-is-not-iterable.png)

Oops! Looks like we got an error. It says `iterator is not iterable`. What’s going on?

&nbsp;
## Differences between iterators and iterables

We saw from the example with the `food` array that iterator was usable both by calling `next()` method and inside `for...of` statement. So, why our iterator doesn’t work like that? Well, it’s because **not every iterator is iterable**.

Remember that the iterable protocol says that we need `[Symbol.iterator]` method on our object for it to be iterable? The thing is that standard iterators have it and it looks like this:

```js
[Symbol.iterator]() {
  return this;
}
```

So handy, isn’t it? That means we can just add it to our iterator to make it an iterable. Oh, and while we’re at it, let’s change the iterator to be finite to avoid our tab crashing like the Dogecoin in May.

```js
// use non-arrow function syntax so that this won't return value of the outer scope
const finiteList = function(start, end) {
  let value = start;

  return {
    next: () => {
      if (value === end) {
        return { value: undefined, done: true };
      }

      return { value: value++, done: false };
    },
    [Symbol.iterator]() {
      return this;
    }
  };
}

const iterator = finiteList(6, 16);

for (const element of iterator) {
  console.log(element);
}
```

Output:

```
6
7
8
9
10
11
12
13
14
15
```

Voilà! We made an iterator which is also an iterable.

Fun fact: There is another way to make our iterator iterable by inheriting from [%IteratorPrototype% object](https://tc39.es/ecma262/#sec-%23iteratorprototype%23-object), however, this way is too cumbersome.

Thankfully, there is even easier way to create iterable iterators.

&nbsp;
## Generators

ES6 introduced generator functions which are functions returning special kind of iterator – `Generator`. `Generator` adheres to both, iterator and iterable protocol. You’ll recognize them easily by the asterix (*) sign before their name. Let’s see how both, finite and infinite list functions from above would look like when written as generator functions.

```js
function* infiniteList(start) {
  let value = start;

  while (true) {
    yield value++;
  }
}

const infiniteIterator = infiniteList(6);

console.log(iterator.next().value); // 6
console.log(iterator.next().value); // 7
console.log(iterator.next().value); // 8
console.log(iterator.next().value); // 9

function* finiteList(start, end) {
  let value = start;
  while (value < end) {
    yield value++;
  }
  return value;
}

const finiteIterator = finiteList(6, 16);

// skip 4 steps
for (const _ of new Array(4)) {
  finiteIterator.next();
}

for (const num of finiteIterator) {
  console.log(num);
}
```

Step by step description of what happens;

* Generator function is called, returning a `Generator` object
* Calling `next()` method executes it until `yield` occurs.
* `yield` defines a value which will be returned. Once `yield` is reached, execution at that point stops and all variable bindings are saved for the future calls.
* Each subsequent `next()` call continues execution from the last reached point.
* `return` from a generator function says that it’s a final value of the iterator.

Let’s give another, more straightforward example;

```js
function* lilIterator() {
  let value = 0;

  yield value++;
  yield value++;
  yield value++;

  return value;
}

const iterator = lilIterator();

// next() is called, execution is stopped at the first yield which returns 0, value is now 1
console.log(lilIterator.next().value);

// next() is called, execution is stopped at the second yield which returns 1, value is now 2
console.log(lilIterator.next().value);

// next() is called, execution is stopped at the third yield which returns 2, value is now 3
console.log(lilIterator.next().value);

// next() is called, at this point generator function has return which means that iterator will be finished with value 3
console.log(lilIterator.next().value);

// any subsequent next() calls will return { value: undefined, done: true }, so output here would be undefined
console.log(lilIterator.next().value);
```

If we didn’t add `return` statement at the end of the generator function, iterator would finish after the third `yield`. And since in our example for infinite list we had `yield` inside `while(true) {}` loop, we ended up with an iterator which returns values infinitely.

&nbsp;
## Conclusion

I hope this article helped you to get a better understanding of iteration protocols. There are some stuff I didn’t mention (like using `yield*` for delegating to another generator function) because they wouldn’t add much point for the article. I encourage you to experiment on your own and practice these concepts in your spare time. I showed you some small examples but iterators are much more powerful than that – you’ll see this as you progress in your career (if you haven’t already).

Let’s sum up the key points;

* **Iterable** is an object which adheres to the *iterable protocol*, meaning it has a `[Symbol.iterator]` property whose value is a method returning an **iterator**.
* **Iterator** is an object which adheres to the *iterator protocol*, meaning it has a `next()` method which returns an object with at least `value` and `done` properties.
* Iterator **can** but **doesn’t have** to be an iterable.
* We can use generator functions for creating objects adhering to both, iterable and iterator protocol.
