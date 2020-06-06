# Tangle

`Tangle` is a literate programming tool designed to make the writing of software
tutorials and blog posts as easy as possible.

## Some background on literate programming

In normal programming, software is written in a programming language which may
be interspersed with comments which explain why or how the code works. Literate
programming inverts this - you write in a natural language (such as English),
and intersperse snippets of source code. This document is then run through a
program which pulls out and concatenates the source code snippets, and passes
them to the compiler to be run. Donald Knuth, who came up with literate
programming, named this process 'tangling', which is what this tool is named
after.

This makes for very readable programs, but also creates a lot of extra work for
the programmer, which is why I think it hasn't caught on and become the
standard way of programming.

## Software tutorials and blog posts and the problem Tangle solves

An interesting thing about literate programs is that they closely resemble
modern software tutorials and blog posts, which contain explanations in a
natural language, interspersed with code snippets.

If you've written one of these, you might have had the issue that it's
difficult to validate that the code in the snippets is correct. My old solution
to this was to have two files open:

1. The tutorial with its code snippets
2. A file containing just the code, which I can run and validate

This works, but is quite manual. If there's a bug in the code, or I want to add
a new feature, I need to change code in two places.

Tangle aims to solve this problem. It takes a tutorial or blogpost written in
Markdown, pulls out any source code defined in code blogs, stitches them
together and writes them to a file. You can then run the file (or run tests
against it) to check that everything's working as expected.

## A simple example

Let's imagine we've written a blog post about iterating over arrays in
JavaScript:

````markdown
# Array iteration in JavaScript

Let's look at three ways to iterate over an array in JavaScript. In each
example, we'll iterate over the array and print out each item.

Let's define the array to iterate over:

```javascript 1
let numbers = [1, 2, 3, 4, 5];
```

The first way is to keep track of an incrementing number `i`, which we use to
index into the array:

```javascript 2
for (let i = 0; i < numbers.length; i++) {
  console.log(numbers[i]);
}
```

Next, JavaScript has a `for...of` statement, which lets us iterate without
having to manage an index:

```javascript 3
for (let number of numbers) {
  console.log(number);
}
```

Finally, JavaScript arrays have a method `forEach`, which takes a function and
calls it for each item in the array:

```javascript 4
numbers.forEach(number => {
  console.log(number);
});
```
````

### Code block numbers

This is normal Markdown, with one exception. After the language definition next
to each code block, there's a number. This number tells `Tangle` the order to
put the code snippets in. Here, we're writing the snippets in the order they're
defined, but you can use this to describe your code in a different order to how
it's run.

You can also repeat code block numbers to redefine a particular block. This is
useful if you want to show for example a naive implementation and then a more
sophisticated one.

### Running Tangle

We can run `Tangle` on this file to pull out the bits of JavaScript and write
them to a file:

```sh
$ tangle --outfile iteration.js README.md
```

This generates a file `iteration.js`, which we can successfully run, letting us
know our code is valid.