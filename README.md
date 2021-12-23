# Updating a Lab (Assignment)

## Learning Goals

- Make changes to a lab's `main` and `solution` branches

## Introduction

One challenge of working with labs is that we need to keep both the `main` and
`solution` branches up to date whenever we make updates. Ideally, the `solution`
branch is a perfect superset of `main`, which may have a commit history that
looks like this:

```txt
        sC  (solution)
       /
mA---mB  (main)
```

When changes need to be made that impact both branches, the following workflow
is preferred:

- Checkout a `<feature>-main` branch off of `main`
- Checkout a `<feature>-solution` branch off of `solution`
- Make the common changes that should be present in both `main` and `solution`
  in your `<feature>-main` branch
- Merge `<feature>-main` into `<feature>-solution`
- Assert that only the changes you want carried over were applied (via git log
  or before the merge)
- Merge the changes from the `<feature>-` branches into their respective
  branches (this may happen via PR on GitHub if you need your changes reviewed)

```txt
           fsE--------fsG   merge: [<feature>-solution] <-- [<feature>-main]
          /          /  \
        sC          /    sI   merge: [solution] <-- [<feature>-solution]
       /           /
mA---mB           / ----mH   merge: [main] <-- [<feature>-main]
       \         /
        fmD---fmF  (<feature>-main)
```

## Updating `main` and `solution`

Let's try this workflow out on your lab from the previous lesson. Imagine a
scenario where we need to add a new test to our lab. This would definitely
impact both the `main` and `solution` branches, so we'll want to make sure our
new code is present on both branches.

To start, create a new `add-test-main` branch off `main` and `add-test-solution`
off `solution`:

```console
$ git branch add-test-main main
$ git branch add-test-solution solution
$ git checkout add-test-main
```

From the `add-test-main` branch, add a new test:

```js
// test/index.test.js
describe("todo", () => {
  it("todo", () => {
    expect(aVariable).to.equal("hello");
  });

  it("todo", () => {
    expect(anotherVariable).to.equal("goodbye");
  });
});
```

Commit your changes:

```console
$ git add test/index.test.js
$ git commit -m 'Add second test'
```

Then merge `add-test-main` into `add-test-solution`:

```console
$ git checkout add-test-solution
$ git merge add-test-main
```

Next, since we've added a test, we'll also need to add some new solution code in
the `add-test-solution` branch:

```js
// index.js
const aVariable = "hello";
const anotherVariable = "goodbye";
```

Make sure your solution works by running `npm test`, then commit your change:

```console
$ git add index.js
$ git commit -m 'Add solution code for second test'
```

Finally, merge the changes from your `<feature>-` branches into `main` and
`solution`:

```console
$ git checkout main
$ git merge add-test-main
$ git checkout solution
$ git merge add-test-solution
```

And push up your finished branches:

```console
$ git checkout main
$ git push
$ git checkout solution
$ git push
```

Since these particular changes didn't involve updating the `README.md` file,
nothing in Canvas will be updated, but the GitHub repo is now good to go with
your changes!

## Conclusion

Phew! That was some complex Git work. In the end, we try to keep a clean commit
history and maintain updates to `main` and `solution` as best we can. But what's
most important is that students have working code, so if you find Git branch
management is an obstacle, talk to the curriculum team about finding other ways
to accomplish what you need to get done.
