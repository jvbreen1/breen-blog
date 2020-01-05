---
title: "What should I test?"
date: 2020-01-05T03:20:00Z
description: >-
  How to ship quickly by testing the right things
---

I was mentoring someone recently who was new to testing in node.js and express.js. We got on the topic of knowing what to test, and how to tell when our tests are "good enough." There is often a natural tension between writing tests and shipping code - we want to cover every edge case and eventuality, but we also want to meet our deadlines and get products out to customers.

The goal of this post is to provide some guidelines that you may find helpful when writing tests.

## Test a few edge cases

When writing tests, especially unit tests, it can be enticing to cover every possible edge case. However, sometimes it's useful to spend a little time analyzing the risky parts of your code. If you know a particular section of code is more likely to fail, or if a failure in that section will lead to Very Bad Things™, make sure to cover it more thoroughly. If you're implementing core business logic or writing a shared library, consider writing more tests than if you were implementing an internal tool or low-risk feature. Look for the high-traffic areas of your codebase and do what you can to be reasonably confident in those areas.

## Stop fighting with test fixtures

If you're spending most of your development time updating JSON test fixtures, you're probably not enjoying yourself. Fixture-based tests using realistic sample data can be helpful, especially when working with legacy code that's complex or not well-understood. When used to test several different code paths and integration points, testing quickly turns into an infinite loop of "wait, why did this test break again?"

When writing fixture-based tests, try to stick to common use cases and trim down test fixtures to what's needed to verify the code under test. This ensures that the code handles the most common scenarios well, and you'll cut down on time spent updating test fixtures if an external API changes or if someone adds or removes a field in an unrelated section of code. If you're using Typescript, you can make use of its type system to ensure that your test objects match the structure of your function parameters.

## Use the language to your advantage

JavaScript is a very flexible language. One of the most powerful tools when it comes to testing is the ability to overwrite a module's functionality. This allows us to provide mock functionality and abstract away the pieces of the system that we aren't currently testing. For example, if I'm testing an Express.js controller, I'll often mock the function calls it makes to API clients and databases. [Jest](https://jestjs.io/) provides some powerful spy and mock functionality that allows you verify that a function call happens or that a Promise rejects with a specific error message.

## Admit defeat and move on

Sometimes a piece of code is just a pain to test. It could be using a finnicky library. There might be a bug that only pops up during the sunset on every fifth sunday. Whatever the reason, it's perfectly okay to say "nope, not gonna waste any more time on this," throw your hands up, and move on. Some JavaScript libraries do not interact well with popular testing frameworks (I'm looking at you, `class` constructors), and you're going to spend a lot of time and energy trying to make them work consistently. At a certain point, testing something manually and making a note to come back later is a more productive use of your time.

## Code coverage is a means, not an end

A very common question I see and hear is "What's the right level of code coverage?" and I'm not sure I have a very good answer. It really depends on your goals and what you're building. Code coverage metrics should be an intentional tradeoff between testing and shipping. Set your coverage levels to a reasonable default - I often start with 80% coverage - and adjust up or down as you go. If you're spending most of your time writing tests in order to meet those coverage levels, feel free to lower your coverage requirements a bit. If you're shipping a lot of bugs, or if you're unable to refactor your code for fear of breaking something, consider raising your coverage requirements.

It's very easy to fall into the trap of chasing higher and higher coverage levels, but you will eventually run into diminishing returns. Some code doesn't need automated tests - I recently deleted a suite of tests that were effectively verifying that I knew how to set and get environment variables. One of my colleagues turned down coverage requirements for a bit so that we could ship more quickly through a bit of a crunch period. Coverage metrics are not an end in themselves. They're a means to an end - an imperfect one at that. Coverage metrics will tell you how much of your code happens to run during tests, but they won't tell you how much value each test adds.

## Conclusion

If you're trying to ship quickly, you absolutely should be writing tests. Every test you write comes with both costs and benefits, and it's up to you to find a balance between the two. I hope these guidelines help you to find that balance.