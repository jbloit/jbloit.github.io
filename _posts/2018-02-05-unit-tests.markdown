---
layout: post
title: "Unit tests"
date: "2018-02-05 13:55:45 +0100"
---

It's about time


# XCode tests cheat sheet

### Give the tests access to classes and methods in your app:
```swift
@testable import BullsEye
```

### given/when/then structure
```swift
func testExample() {
    // This is an example of a functional test case.
    // Use XCTAssert and related functions to verify your tests produce the correct results.

    // 1. given
    let guess = gameUnderTest.targetValue + 5

    // 2. when
    _ = gameUnderTest.check(guess: guess)

    // 3. then
    XCTAssertEqual(gameUnderTest.scoreRound, 95, "Score computed from guess is wrong")
}
```
### CI / automating tests

https://savvyapps.com/blog/continuous-integration-ios-jenkins


## links
- [RW tutorial](https://www.raywenderlich.com/150073/ios-unit-testing-and-ui-testing-tutorial)
