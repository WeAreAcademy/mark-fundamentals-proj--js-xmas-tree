---
module: fundamentals

level: 1

methods:
  - team
  - pair
  - solo

tags:
  - wip
---

# Xmas Tree

<a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-nd/4.0/88x31.png" /></a>

> This is part of Academy's [technical curriculum for **The Mark**](https://github.com/WeAreAcademy/curriculum-mark). All parts of that curriculum, including this project, are licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License</a>.

This project is about writing your first JavaScript functions, using a previously referenced [Xmas Tree](https://www.codewars.com/kata/577c349edf78c178a1000108) kata as inspiration.

## Learning Outcomes

- Skip Jest tests with `.skip`
- Interpret the output of Jest tests
- Run Jest tests in watch mode
- Distinguish between the Jest `toBe` and `toStrictEqual` matchers
- Declare variables in JavaScript with `let` and `const`
- Write a function with a function expression in JavaScript
- Write an `if`/`else` statement in JavaScript
- Write a `for` loop in JavaScript
- Write an arrow function in JavaScript

## Exercise 0: Running and interpreting Jest tests

> 🎯 **Success criterion:** you are able to run the tests for this project locally and see the output (failing tests report) in your terminal.

This repository contains a JavaScript project with tests (but mostly empty main code).

To start with, we'll clone the repository and run the existing tests to check the existing state of the code.

Let's clone the repository:

```bash
cd ~/Developer/Academy # or wherever you're storing Academy repos
git clone https://github.com/WeAreAcademy/mark-fundamentals-proj--js-xmas-tree.git
```

In order to run the code or its tests, we'll need to install all project dependencies:

```bash
cd mark-fundamentals-proj--js-xmas-tree
yarn
```

and we can now run tests:

```bash
yarn test
```

You'll see a lot of terminal output, including the following:

```bash
Tests: 5 failed, 5 total
```

We'll make these tests pass in this project!

## Exercise 1: Skipping Jest tests

> 🎯 **Success criterion:** you run the tests and see that 4 tests are skipped (with 1 running and failing).

Having a lot of tests is helpful - it gives us confidence in the code that we write (or have written), and a structure to guide us in solving the problem at hand.

But tests can sometimes be unhelpful, too:

- Paralysis: "Oh, there's so much to do, I don't know where to start!"
- Noise: "I want to focus on _this_ test first but I keep having to look through these other tests that I _know_ will be failing"

Luckily, the Jest testing library gives us a nice way to skip tests and come back to them later.

Go to the first test and add a `.skip` after the `test`, so it looks like this:

```js
test.skip("findLineWidth finds the line width from a foliage height", () => {
  ... // <-- this is commonly used to signify "omitted detail" in code snippets
});
```

Now, run the tests. This time, the terminal output should include:

```bash
Tests: 4 failed, 1 skipped, 5 total
```

This shows us that Jest is ignoring the 1 test that we added a `.skip` to.

Go ahead and **add `.skip` to all of the tests _apart from for `makeTreeTrunk`_** (which we'll be looking at in the next exercise).

Now, when you run the tests, you should see:

```bash
Tests: 1 failed, 4 skipped, 5 total
```

## Exercise 2: Interpreting Jest tests

> 🎯 **Success criterion:** an explanation and interpretation of the Jest test output at the end of the exercise.

Our Jest output gives us the following:

```
expect(received).toStrictEqual(expected) // deep equality

Expected: ["____#____", "____#____"]
Received: undefined

      13 | 
      14 | test("makeTreeTrunk returns the tree trunk part of the Christmas tree with given foliage height", () => {
    > 15 |   expect(makeTreeTrunk(5)).toStrictEqual(["____#____", "____#____"]);
         |                            ^
      16 |   expect(makeTreeTrunk(3)).toStrictEqual(["__#__", "__#__"]);
      17 | });
      18 | 

  at Object.<anonymous> (xmas-tree.test.js:15:28)
```

This gives us lots of helpful information:

1. Which assertion is failing: it's the one on line 15
2. What the test was expecting to see: `["____#____", "____#____"]`
3. What the test actually received: `undefined`

Let's go to our `makeTreeTrunk` function and add in a simple (if strange) body, where we just `return true`:

```js
function makeTreeTrunk(foliageHeight) {
  return true;
}
```

Now, **predict what the output of Jest will be**, and then run the tests.

Let's make another change:

```js
function makeTreeTrunk(foliageHeight) {
  return [true];
}
```

and run the tests again.

```
 expect(received).toStrictEqual(expected) // deep equality

 - Expected  - 2
 + Received  + 1

   Array [
 -   "____#____",
 -   "____#____",
 +   true,
   ]

      13 | 
      14 | test("makeTreeTrunk returns the tree trunk part of the Christmas tree with given foliage height", () => {
    > 15 |   expect(makeTreeTrunk(5)).toStrictEqual(["____#____", "____#____"]);
         |                            ^
      16 |   expect(makeTreeTrunk(3)).toStrictEqual(["__#__", "__#__"]);
      17 | });
      18 | 

   at Object.<anonymous> (xmas-tree.test.js:15:28)
```

This gives us some interesting and helpful output:

- `- Expected - 2`: There are 2 array elements which we expected but which are missing in what we received
- `+ Received + 1`: There is 1 array element which we received but which we did not expect (and do not want)

Specifically:

```
   Array [
 -   "____#____",
 -   "____#____",
 +   true,
   ]
```

- There are two missing elements of `"____#____"`; and
- There is one unwanted element of `true`

Let's update our function once more:

```js
function makeTreeTrunk(foliageHeight) {
  return ["hello world", "____#____"];
}
```

Now, **predict what the Jest test output might be**, and run the tests.

How does the Jest output help us identify how our test is failing?

## Exercise 3: Jest in watch mode

> 🎯 **Success criterion:** Jest is running in watch mode and automatically rerunning as relevant code changes.

We've been doing a lot of the following cycle:

1. Run tests
2. Change code
3. Repeat

Luckily, Jest has a handy "watch" mode which means it will automatically run when relevant code gets updated.

Navigate to `package.json` and update the `test` script as follows:

```json
"scripts": {
    "test": "jest --watch"
  },
```

Now, when you run `yarn test`, it will run Jest in watch mode.

Go back and make a quick change the `makeTreeTrunk` function (e.g. delete the function body - it doesn't matter exactly how you change it).

Now, when you inspect your terminal (where you ran Jest), you'll see it has automatically re-run the tests!

You can quit out of watch mode with `Ctrl + C`.

Let's go back to our `package.json` and update our scripts to give us a convenient way of running tests in watch mode or not:

```json
"scripts": {
 "test": "jest",
 "test:watch": "jest --watch"
},
```

So, now, you can run either `yarn test` or `yarn test:watch` (with the [colon being a common convention for related `npm` or `yarn` scripts](https://stackoverflow.com/questions/47606101/what-is-colon-in-npm-script-names)).

(Alternatively, you might prefer to configure watch mode to be the default on the `"test"` script, with a `:noWatch` appended to run without watch mode.)

## Exercise 4: `.toBe` vs `.toStrictEqual`

> 🎯 **Success criterion:** you can distinguish between `.toStrictEqual` and `.toBe`

Now, let's change `makeTreeTrunk` to make one assertion pass:

```js
function makeTreeTrunk(foliageHeight) {
  return ["____#____", "____#____"];
}
```

Before you look at the test output, **predict what you think the test output will be**:

<details>
  <summary>SPOILER: test output</summary>
  <pre>
     expect(received).toStrictEqual(expected) // deep equality

    - Expected  - 2
    + Received  + 2

      Array [
    -   "__#__",
    -   "__#__",
    +   "____#____",
    +   "____#____",
      ]

      14 | test("makeTreeTrunk returns the tree trunk part of the Christmas tree with given foliage height", () => {
      15 |   expect(makeTreeTrunk(5)).toStrictEqual(["____#____", "____#____"]);
    > 16 |   expect(makeTreeTrunk(3)).toStrictEqual(["__#__", "__#__"]);
         |                            ^
      17 | });
      18 | 
      19 | test.skip("makeFoliageSegment returns the foliage segment for a given level of a tree of given foliage height", () => {

      at Object.<anonymous> (xmas-tree.test.js:16:28)

  </pre>

  <p>Our first assertion (line 15) is now passing, but the second assertion (line 16) is failing (shown by the arrow), with two missing lines and two additional (unwanted) lines.</p>
</details>

Now, head on over to `.xmas-tree.test.js`, and let's take a look at the different assertions being made.

Each assertion we can see at the moment has one of the following two structures:

```js
// structure one
expect(testedFunction(testValue)).toBe(expectedValue);

// structure two
expect(testedFunction(testValue)).toStrictEqual(expectedValue);
```

These have a similar semantic structure: our test tells us that we should compare `testedFunction(testValue)` to `expectedValue`, and expect that they are similar in some specified way. (If they are not similar in that particular way, the test will fail.)

`.toBe` and `.toStrictEqual` are two different Jest _matchers_, which use different criteria to compare `testedFunction(testValue)` and `expectedValue`.

To demonstrate this, let's change our first `makeTreeTrunk` assertion, to use `.toBe` instead of `.toStrictEqual`:

```js
expect(makeTreeTrunk(5)).toBe(["____#____", "____#____"]);
```

and then run the tests:

```bash
expect(received).toBe(expected) // Object.is equality

If it should pass with deep equality, replace "toBe" with "toStrictEqual"

Expected: ["____#____", "____#____"]
Received: serializes to the same string

      13 | 
      14 | test("makeTreeTrunk returns the tree trunk part of the Christmas tree with given foliage height", () => {
    > 15 |   expect(makeTreeTrunk(5)).toBe(["____#____", "____#____"]);
         |                            ^
      16 |   expect(makeTreeTrunk(3)).toStrictEqual(["__#__", "__#__"]);
      17 | });
      18 | 

  at Object.<anonymous> (xmas-tree.test.js:15:28)
```

Our assertion on line 15, that was passing with the `.toStrictEqual` matcher, now fails with the `.toBe` matcher.

In other words, they're checking for different things:

- `.toBe` checks for _referential identity_
- `toStrictEqual` _recursively_ checks for _deep value equality_

If `.toBe` passes, then `.toStrictEqual` will pass; however, it is possible for `.toBe` to fail and yet for `.toStrictEqual` to pass.

As a simple shortcut for now, it's probably enough for now for you to remember that, when you are writing a test to compare data structures that group other 'simpler' values - i.e. arrays (JavaScript's equivalent of Python lists) or objects (JavaScript's equivalent of Python dictionaries) - that you probably want to use `.toStrictEqual` to do a _deep_ check; otherwise, for 'simple' values (numbers, strings, booleans) it's sufficient to use the simpler/shorter `.toBe` matcher.

The full story here is one about primitive and non-primitive types.

Here are some readings we recommend:

- [Just JavaScript](https://github.com/HackYourFutureBelgium/just-javascript), chapters 1-6 (inclusive) - longer, but worth it
- [Value vs Reference](https://www.educative.io/courses/step-up-your-js-a-comprehensive-guide-to-intermediate-javascript/7nAZrnYW9rG)
- [How to Compare Objects in JavaScript](https://dmitripavlutin.com/how-to-compare-objects-in-javascript/)

To check that you're able to distinguish between `.toBe` and `.toStrictEqual`, you might want to add the following temporary tests, predict which of them will pass/fail, and then check the test output:

```js
test("toBe on arrays - experiment", () => {
  expect([1, 2, 3]).toBe([1, 2, 3]);
  expect(["a", "b", "c"]).toBe(["a", "b", "c"]);
});

test("toStrictEqual on arrays - experiment", () => {
  expect([1, 2, 3]).toStrictEqual([1, 2, 3]);
  expect(["a", "b", "c"]).toStrictEqual(["a", "b", "c"]);
});

test("toBe on booleans - experiment", () => {
  expect(true).toBe(true);
  expect(false).toBe(false);
});

test("toStrictEqual on booleans - experiment", () => {
  expect(true).toStrictEqual(true);
  expect(false).toStrictEqual(false);
});

test("toBe on numbers - experiment", () => {
  expect(1).toBe(1);
  expect(100).toBe(100);
});

test("toStrictEqual on numbers - experiment", () => {
  expect(1).toStrictEqual(1);
  expect(100).toStrictEqual(100);
});

test("toBe on objects - experiment", () => {
  expect({ firstName: "Richard", lastName: "Ng" }).toBe({
    firstName: "Richard",
    lastName: "Ng",
  });
  expect({ a: "A", b: "B", c: "C" }).toBe({ a: "A", b: "B", c: "C" });
});

test("toStrictEqual on objects - experiment", () => {
  expect({ firstName: "Richard", lastName: "Ng" }).toStrictEqual({
    firstName: "Richard",
    lastName: "Ng",
  });
  expect({ a: "A", b: "B", c: "C" }).toStrictEqual({ a: "A", b: "B", c: "C" });
});

test("toBe on strings - experiment", () => {
  expect("hello").toBe("hello");
  expect("world").toBe("world");
});

test("toStrictEqual on strings - experiment", () => {
  expect("hello").toStrictEqual("hello");
  expect("world").toStrictEqual("world");
});
```

Change the `.toBe` matcher back to `.toStrictEqual`, and remove the temporary tests above if you added them - we're now going to try to make the tests pass!

## Exercise 5: Completing the kata

> 🎯 **Success criterion:** all of the initial tests are passing

The tests are laid out in an order that we suggest you approach them (i.e. we suggest you tackle `findLineWidth` first, then `makeTreeTrunk`, and so on).

It will probably be helpful to make use of the `.skip` functionality in Jest to ignore failing tests whilst you're not working on them.

Things which you may find helpful:

1. Documentation on [repeating a JavaScript string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/repeat)
2. [String Interpolation in JavaScript](https://dmitripavlutin.com/string-interpolation-in-javascript/)

See if you can:

- Firstly, get the tests passing using only traditional `function` expressions
- Then, see if you can get the tests passing using concise one-line arrow functions where appropriate (remembering that readability is still very important)

## Exercise 6: Adding Jest assertions

> 🎯 **Success criterion:** you have added at least one assertion to every `test`, and the tests all pass

Now, see if you can add a third assertion to each `test` block (at the moment, they all have two assertions).

If you've written robust solutions (and the test code you're writing is correct), the tests should all still pass! 🎉
