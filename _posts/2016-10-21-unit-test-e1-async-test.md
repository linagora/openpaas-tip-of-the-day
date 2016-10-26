---
layout: post
title: Unit test tips & tricks - Episode 1 - Avoid timeout in async test
tip-number: 08
tip-username: Sang D. Ngo
tip-username-profile: https://github.com/heroandtn3
tip-tldr: How to avoid timeout error in async test cases
---

We write unit test everywhere in OpenPaas, some of them are asynchronous, let's see an example:

```javascript
getModule()
  .findByDomainId()
  .then(function(configuration) {
    expect(configuration.modules).to.have.length(2);
    done();
  });
```

It won't be a problem if it passes, but what if it fails?

```
Error: timeout of 20000ms exceeded. Ensure the done() callback is being called in this test.
```

Well, the test case got hang for 20s then printed out an error message that is not
helpful at all. Why the `done()` was not called? Did the promise reject? Did
the promise take too much time to resolve? You start debuging the failed test by
commenting the `expect` statement:

```javascript
getModule()
  .findByDomainId()
  .then(function(configuration) {
    //expect(configuration.modules).to.have.length(2);
    done();
  });
```

And it passed!

```
Done, without errors.
```

Obviously, the problem comes from the `expect` statement, you continue with:

```javascript
getModule()
  .findByDomainId()
  .then(function(configuration) {
    console.log('Length:', configuration.modules.length);
    //expect(configuration.modules).to.have.length(2);
    done();
  });
```

```
Length: 1
```

Cool! Finally, you found the source of the problem. You fix your code or your mock,
the test comes green, you feel so happy.

Well, it would not be easy like that when your test fails on your CI service,
especially with random failure. You may already knew, sometimes we have random failure
and the error message is just `Error: timeout of 20000ms exceeded`. It drives you
crazy because you don't know why is that, the error message is not helpful.
When you try to debug it on your local machine, you cannot reproduce the failure because it's simply a random failure!

Here is the secret: **always call `done` when error occurs** (of course call it with useful error message)

Now, let's modify your test like this:

```javascript
getModule()
  .findByDomainId()
  .then(function(configuration) {
    expect(configuration.modules).to.have.length(2);
    done();
  })
  .catch(done);
```

```
AssertionError: expected [ Array(1) ] to have a length of 2 but got 1
```

The error message is much more helpful now. But hey, what if the promise rejects
with `undefined` error message? Your test will become a success even though it
should be a failure. To avoid that, always make sure you call `done` with something:

```javascript
getModule()
  .findByDomainId()
  .then(function(configuration) {
    expect(configuration.modules).to.have.length(2);
    done();
  })
  .catch(done.bind(null, 'this promise should resolve'));
```

```
Error: done() invoked with non-Error: this promise should resolve
```

But the error is not really helpful, it doesn't tell you why it didn't resolve.

This is better:

```javascript
getModule()
  .findByDomainId()
  .then(function(configuration) {
    expect(configuration.modules).to.have.length(2);
    done();
  })
  .catch(function(err) {
    done(err || 'this promise should resolve');
  });
```

```
AssertionError: expected [ Array(1) ] to have a length of 2 but got 1
```

That's quite cumbersome, let's try ES6 syntax:

```javascript
getModule()
  .findByDomainId()
  .then(configuration => {
    expect(configuration.modules).to.have.length(2);
    done();
  })
  .catch(err => done(err || 'should resolve'));
```

```
AssertionError: expected [ Array(1) ] to have a length of 2 but got 1
```

That's perfect now.

See you in the next episode. Happy unit testing!
