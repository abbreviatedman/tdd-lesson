# TDD Lecture Codealong

You can play with the currently set up environment from this codealong endpoint to start making your own tests!

### Steps To Get Going

- Install `jest` with `npm install`, which will look at the `dependencies` section of the `package.json` file and install what's there--in this case, it's just `jest`.
- Run the tests with `npm test` for a one-time run or `npm run watch` for a run-again-on-save workflow.

### Introduction to Test-Driven Development (TDD)

Today, we're diving into the world of Test-Driven Development, often abbreviated as TDD. This development methodology, while sometimes seen as a bit rigorous, holds the promise of writing cleaner, more reliable code and, ultimately, leading to higher-quality software.

#### What is TDD?

At its core, TDD is a software development process where you write tests for your functions, features, or modules _before_ you write the actual implementation. It might sound a bit counterintuitive at first—why test something that doesn't exist yet? However, the benefits of this approach are substantial:

1. **Early Error Detection**: By writing tests first, you catch errors early in the development cycle.
2. **Improved Design**: Writing tests forces you to think through the requirements and design of your code carefully.
3. **Refactoring Safety Net**: With a suite of tests always available, you can refactor your code with confidence, knowing that any regression will be quickly identified.
4. **Better Documentation**: Tests serve as an excellent form of documentation, showing how the code is supposed to behave.

#### How TDD Works

The TDD process can be broken down into a simple cycle known as **Red-Green-Refactor**:

1. **Red**: Write a test for a new feature or function. Initially, this test will fail (hence, the term "Red").
2. **Green**: Write just enough code to make the test pass. This might be the simplest implementation possible.
3. **Refactor**: Clean up the code while keeping the tests green. Refactor to improve the structure and design of your code without changing its external behavior.

#### Why Adopt TDD?

TDD might seem like extra work upfront, but it can save significant time and headaches in the long run. It encourages a mindset of thoroughness and precision, leading to fewer bugs and more maintainable code. Many experienced developers and industry leaders swear by TDD as a cornerstone of their development practices.

As we go through today's lesson, you'll get hands-on experience with writing tests first and seeing the Red-Green-Refactor cycle in action. By the end, you'll have a solid foundation in TDD and be ready to apply these principles to your own projects.

Let's embark on this journey to write better, more reliable code through the power of Test-Driven Development!

### Walk-Through

We'll start with a simple `add` function that takes two numbers and returns their sum. Here's the test we'll write:

```javascript
it('add function adds two numbers', () => {
  const result = add(1, 2);
  expect(result).toBe(3);
});
```

This test is straightforward: we're calling the `add` function with `1` and `2` and expecting the result to be `3`. Of course, we haven't written the `add` function yet, so this test will fail. Let's write the implementation:

```javascript
function add(x, y) {
  const answer = x + y;

  return answer;
}
```

Now, when we run the test, it should pass. We've successfully followed TDD!

However, there is a problem with this test: with only one example (1 + 2), we can't be sure that the `add` function works for all cases. For example, this code would pass the test as well!

```javascript
function add(x, y) {
  return 3;
}
```

Although this might seem like a silly example, it highlights the importance of writing comprehensive tests. There are many real-world scenarios where a function might pass a single test but fail in other cases, including due to harder-to-see hard-coding or other issues.

Let's add a few more test cases to ensure our `add` function works as expected:

```javascript
it('add function adds two numbers', () => {
  const result = add(1, 2);
  expect(result).toBe(3);

  const result2 = add(5, 7);
  expect(result2).toBe(12);
});
```

Now, we have two test cases for the `add` function. If we run the tests, they will no longer pass with the `return 3` version, but will pass with the previous, non-hardcoded version.

We've successfully tested our function with multiple cases, ensuring it works as expected. However, there's still a problem with our `add` function: it only works with numbers. If we pass in strings, it will concatenate them instead of adding them. Let's add a test case to catch this issue:

```javascript
it('add function handles numbers in string format', () => {
  const result = add('1', '2');
  expect(result).toBe(3);
  const result2 = add('5', '7');
  expect(result2).toBe(12);
});
```

As you can see, our function does not pass these tests. But we can write some code that does:

``` javascript
function add(x, y) {
  const result = Number(x) + Number(y);

  return result;
}
```

Thanks to our tests, we caught this issue early and were able to fix it before it became a problem. This is the power of Test-Driven Development!

Now, while we have finished this function, it's not perfect. Here's where the third step from TDD. We had Red (tests failing), and Green (tests passing), and now we have Refactor. Let's refactor our `add` function to make it more readable and succinct.

First, we'll remove the intermediate variable `answer`.

```javascript
function add(x, y) {
  return Number(x) + Number(y);
}
```

If we re-run the tests, we can see that we're still passing. This is the power of TDD--we get _instant_ feedback on our changes, ensuring that we don't break anything in the process, and giving us the confidence we need to feel free to change our code without fear.

Now that we have one statement in our function, it makes sense to convert it to an arrow function. Let's do that:

```javascript
const add = (x, y) => Number(x) + Number(y);
```

Now, thanks to Test-Driven Development, we have what is likely very higher-quality code than we would have without it.

### Extending The Functionality - 3 Numbers

Now that we have a solid `add` function, let's extend its capabilities. We'll add a new feature that allows it to handle 3 numbers.

This is a great opportunity to come up with the test cases together as a class! What are some scenarios we should test for when adding 3 numbers?

Here is one possible test case:

```javascript
it('add function handles adding 3 numbers', () => {
  const result = add(1, 2, 3);
  expect(result).toBe(6);

  const result2 = add(5, 7, 9);
  expect(result2).toBe(21);
});
```

Run those tests, and you'll see that they fail.

Let's implement the `add` function to handle adding 3 numbers.

```javascript
const add = (x, y, z) => Number(x) + Number(y) + Number(z);
```

Uh oh! Our previous tests are failing. Why is that? It's because we changed the function signature from `(x, y)` to `(x, y, z)`. This is a great example of how TDD can help catch issues early on. Let's update our function to handle either 2 _or_ 3 numbers:

```javascript
const add = (x, y, z) => {
  if (z === undefined) {
	return Number(x) + Number(y);
  }

  return Number(x) + Number(y) + Number(z);
};
```

### Handling Arbitrary Numbers of Arguments

Now, let's extend our `add` function to handle an arbitrary number of arguments. This is a great opportunity to use the `...rest` syntax in JavaScript. First, though, think about (on your own or as an exercise) what test cases you would write for this new functionality.

Here's one possible solution:

```javascript
it('add function handles adding an arbitrary number of arguments', () => {
  const result = add(1, 2, 3, 4, 5);
  expect(result).toBe(15);

  const result2 = add(5, 7, 9, 11, 13, 15);
  expect(result2).toBe(60);
});
```

Now, let's implement the `add` function to handle an arbitrary number of arguments:

```javascript
const add = (...numbers) => {
  let sum = 0;
  for (const num of numbers) {
	sum += Number(num);
  }

  return sum;
};
```

`...rest` syntax allows us to pass in any number of arguments, which are then collected into an array called `numbers`. We can then iterate over this array, summing up the numbers to get the final result.

Run the tests, and you should see that they pass. We've successfully extended our `add` function to handle an arbitrary number of arguments, and we've used TDD to guide us through the process.

### Conclusion

In today's lesson, we embarked on a journey through the principles and practices of Test-Driven Development (TDD). We began by understanding TDD in the abstract, emphasizing its importance in writing cleaner, more reliable code and the significant benefits it brings to software development, including early error detection, improved design, and safer refactoring.

We then rolled up our sleeves and put TDD into practice by writing tests for a simple function that adds two numbers. This hands-on approach allowed us to experience the Red-Green-Refactor cycle firsthand:

1. **Red**: We wrote a test for our add function, which initially failed because the implementation was missing.
2. **Green**: We implemented the simplest possible version of our add function to make the test pass.
3. **Refactor**: We then refactored our code, ensuring it was clean and maintainable while keeping all tests green.

Next, we extended our function's capabilities by refactoring it to handle string-formatted numbers. This not only demonstrated the flexibility and power of TDD but also showed how refactoring can be done confidently with a solid suite of tests in place. Each step of our journey reinforced the importance of thinking through our design before coding, and how tests can serve as both documentation and a safety net.

Through this lesson, we have seen how TDD helps in:

- **Ensuring functionality**: By writing tests first, we defined what our function should do before implementing it.
- **Maintaining confidence**: With each refactor, our tests assured us that we hadn't broken existing functionality.
- **Documenting behavior**: The tests themselves served as clear documentation of how our function is expected to behave.

As you continue to develop and refine your coding practices, keep the principles of TDD in mind. It's not just about writing tests—it's about adopting a mindset that prioritizes reliability, maintainability, and thoughtful design. TDD is a powerful tool in your developer toolkit, helping you build robust software that stands the test of time.

The more you use TDD, the more natural and beneficial it will become. Happy testing!
