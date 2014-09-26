iOS Style Guide
===============

The style guide we (sometimes) (try to) adhere to on the mobile team at TripAdvisor. We follow many of the same principles outlined in the 
[NYTimes Style Guide](https://github.com/NYTimes/objective-c-style-guide).

## Apple Documentation

Some references to the "Official" Documentation. We follow recommendations here unless otherwise noted below.

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Contents
* [Naming](#naming)
* [Properties and Methods](#properties-and-methods)
* [Spacing](#spacing)
* [Literals](#literals)


## Naming
- Verbosity
- Return time and inputs should be obvious
- Properties, put nonatomic and strong even if default


## Properties and Methods

Prefer dot-notation for getting and setting properties of objects. Use brackets for methods.

**Example**

```objc

```

> One benefit of keeping style different for properties and methods is that it makes it more obvious how the messages are sent.



## Spacing
- Use proper spacing



## Literals

## Ternary Operator
- Only 1 per line.
- Use name ?: @"unknown" as a short syntax

## Designated Initializers
- Future proof yourself
- If the argument is required and not there, throw exception, return nil

## Constants

## Enums
- Switch-Case: no defaults


## Singletons


