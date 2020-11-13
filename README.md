# Code Challenge

Write one JavaScript statement where indicated that causes the code below to _always_ produce a number between 10 and 20.

```js
let x = 2;
let y = 8;

const a = function (b) {
  return function (c) {
    return x + y + Math.abs(b) + c;
  };
};

// add your one statement here:

const fn = a(x);
x = 4;

console.log(fn(Math.random() * 10));
```

## Solving Code Challenges

Code challenges are commonplace in interviewing for engineering roles. The purpose of them is to see how you approach a problem. When confronted with a challenge like the one above, the most important thing to do is **not panic**. You _**can**_ do this!!! Your state of mind is so very important and interviewing is already stressful enough, so just take a breath and enter the zone...

### Step By Step

The key to tackling this particular problem is to walk through each line of code and evaluate it at each step. Let's take this code apart line-by-line:

1. `let x = 2;` This line is easy. Here a variable called `x` is assigned the value of `2`. The `let` keyword tells us the value _could be_ changed in the future. So, for now, we'll just ignore this since it's just a declaration.
2. `let y = 8;` This line is the same as above. It's just a declaration of a variable called `y` with a temporary assignment of a value of `8`.
3. `const a = function (b) {...}` The next line isn't as scary as it seems. All that's going on here is that we've created a variable called `a` and assigned a function to it. We can also see that the anonymous function assigned to `a` is going to receive one argument.
4. `return function (c) {...}` Okay, this is maybe not that easy on the eyes, but it's not hard to decipher either if we separate it from all of the other stuff going on around it. This says, when we call the function assigned to `a`, it returns another function that will also need to be passed one argument!
5. `return x + y + Math.abs(b) + c;` Hmmmm, this part isn't hard at all! It's just a returning the sum of 4 variables. Later, when this function is invoked, we'll have to remember that _if_ the value of `b` is less than 0, we'll need to negate it to produce its absolute value before adding up the sum. For right now, we won't worry about that though.
6. `const fn = a(x);` Again, don't be thrown off by all these variables. Isolating this one, we can see that the only thing that is happening here is that we're creating a new variable called `fn` and assigning it a value. What value? Well, the value will be the return value of _"invoking the function `a` and passing it the **value** of `x` as an argument"_. Hmmmm... let's take a moment to think through that:

When we call `a(x)` the first thing that happens is the `x` is evaluated to determine its value. So:

```js
a(x);

// is the same as saying

a(2); // so *inside* our function b = 2
```

Next, the function is invoked, causing it to `return` the function in its return statement, which is what gets assigned to the variable `fn`. At this point, we know what `b` is and since this function has been executed, it can't change so `b` is evaluated and stored. We can think of this as:

```js
// The return value of `a(2)` is:
const fn = function (c) {
  return x + y + Math.abs(2) + c;
};

// Since 2 is > 0, Math.abs(2) is just 2:
const fn = function (c) {
  return x + y + 2 + c;
};
```

Of course, `x` and `y` can be filled in here but they are outside of our function's scope and were declared with the `let` keyword, so they _might_ change before this function is invoked. If we evaluate them right now, this would be:

```js
// x = 2
// y = 8
// If x and y don't change later...
const fn = function (c) {
  // b is 2
  return 2 + 8 + 2 + c; // or 12 + c
};
```

Now we've simplified this down to a pretty digestable function. Let's see what happens next!

7. `x = 4;` Alrighty, as we thought might happen, the next line reassigns the value of `x` setting it to `4`. That means that if things don't change again before we invoke the function stored in the `fn` variable, it would be:

```js
// x = 4
// y = 8
// If x and y don't change (again) later...
const fn = function (c) {
  // b is 2
  return 4 + 8 + 2 + c; // or 14 + c
};
```

8. `console.log(fn(Math.random() * 10));` This is a bit hard to read because of all the nesting, but break up the parts, evaluating each one from the inside out and it's really quite simple:

- Start with `Math.random() * 10`. This returns a number between 0 and 10.
- The random number is then used as the argument that is passed to the function stored in the variable `fn`. Therefore, we can say that whatever the random number is will be the value of `c` inside our function.

```js
// all we know is c is greater than 0 but less than 10
return 4 + 8 + 2 + c; // or 14 + some number between 0 and 10
```

- Lastly, `console.log` whatever the function `fn` returns.

## Finding the Solution

What does this tell us? It tells us that the one line of code that we write has to result in the sum of `x + y + b` being `10` in order for the result that the function returns to always be between 10 and 20! Why? Well, we know that `c` will always be between `0` and `10`, so if we want the total to be between 10 and 20 the remaining numbers must sum up to exactly 10!

So... what can we do to change the sum of `x + y + b`? We already know that `b` can't be changed, so that will always be `2`. Therefore, we're looking at `x + y + 2` having to sum up to 10. We also know that changing `x` would be fruitless, because the value of `x` will be reassigned after the one statement that we're allowed to add to the code. That means we can think of this as: `4 + y + 2` or `6 + y` must add up to `10`.

Hey, that's not hard to figure out at all! We just have to assign `y` a value of `4`!

```js
let x = 2;
let y = 8;

const a = function (b) {
  return function (c) {
    return x + y + Math.abs(b) + c;
  };
};

// add your one statement here:
// SOLUTION:
y = 4;

const fn = a(x);
x = 4;

console.log(fn(Math.random() * 10));
```

:tada: Congratulations! With patience and logic, we've solved this challenge!
