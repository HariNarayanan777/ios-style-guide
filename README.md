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
* [Conditionals](#conditionals)
* [Literals](#literals)


## Naming
- **Needs Owner**
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
Take advantage of spaces to improve readability, follow Apple examples where available.

Indent using 4 spaces. Avoid Tabs (change this preference in Xcode, otherwise you will have to deal with strange merge conflicts and misaligned code reviews)

**Example**

```objc

- (NSString *)methodWithString:(NSString *)string andAnotherArgument:(NSUInteger)integerArgument {
    // Include a space after the type, before the *, with no space between the * and the variable name.
    NSString *anotherString = @"here is another string";
    
    // For conditionals, include a space between if and (.
    if (!integerArgument) {
        return nil;
    }
    
    // include spaces after commas to improve readability
    return [NSString stringWithFormat:@"%@, %@, %d", string, anotherString, integerArgument];
    
}
```

> The -/+ should be the first character of the line, followed by a space. 
> There should be no spaces within the return type (void), or between the type and the method name.

## Structure / Grouping Code
- **Needs Owner**
- Keep like methods together, in an expected order (inits at the top, deallocs there as well, all view helpers together)
- Group and separate code areas with `#pragma` marks

## Conditionals
- **Needs Owner**
- Braces on same line
- Proper spacing before ()
- Don't compare against nil, or 0
- for strings, arrays, check length count
- For short statements, keep simple w/ 1 line (but avoid lacking braces and putting result on a separate line)

**Example**

```objc
- (BOOL)myTestMethodForInput:(NSString *)input {
    
    if (input.length) {
        
    } else if () {
        
    } else {
        
    }

}

```



## Literals
Prefer literals where you can for `arrays`, `dictionaries`, and `NSNumbers`. Be careful about accidental nil inserts that can cause crashes

**Example**

```objc
NSArray *array = @[@1, @2, @3];
NSDictionary *dictionary = @{@"key": @"value"};
NSNumber *boolNumber = @YES;
NSNumber *integerNumber = @100;

NSUInteger number = 100;
[array addObject:@(number)];
```

## Ternary Operator

Take advantage of the simplicity of the Ternary Operator, but don't get too complicated. Be nice to future programmers who will try to quickly understand your work. If it takes more then 2 seconds to understand the logic, use an if statement instead. Clarity > lines of code.

**Example**

```objc
return ([self.reviews count] == 1 ? @"1 review" : [NSString stringWithFormat:@"%d reviews", [self.reviews count]]);
```

```objc
// If checking for nil and returning value if present, you can abbreviate the operator like this
return self.user.name ?: @"A TripAdvisor Member";
```


## Designated Initializers

Protect yourself and your code. If you have a required parameter in an initializer and it is not present, warn the developer using an `NSInvalidArgumentException`. Return `nil` as well when you want to avoid moving forward in a bad state

**Example**
```objc
- (instancetype)initWithUser:(TAUser *)user {
    if (!user) {
        [NSException raise:NSInvalidArgumentException format:@"`user` cannot be `nil`."];
        return nil;
    }
    
    if (self = [super init]) {
        _user = user
    }
    
    return self;  
}

```


## Constants

Prefer `static const`  over `#define`. For public constants, prefix with TA and the name of the class where relevant. For private/internal constants, prefix with kTA.

**Example**

```objc

// Example.h
extern const NSUInteger TAExampleNumReviews;
extern NSString * const TAExampleDefaultName;

// Example.m
static const NSUInteger TAExampleNumReviews = 20;
static NSString * const TAExampleDefaultName = @"A TripAdvisor Member";

static const CGFLoat kTAExamplePrivateConstant = 10.5;
```

> Using constants gives better information about the type, and helps to avoid simple mistakes
> The use if kTA vs. TA makes the scope of the constant more obvious. In general, the more obvious the better.


## Types

Prefer `NSInteger` over `int`, and `CGFloat` over `float`. When applicable, prefer `NSUInteger` over `NSInteger`




## Enums
Use `NS_ENUM` where possible (and NS_Option)

In Switch-Case statements, avoid the use of default. By avoiding you will receive warnings when you forget to support a value in the enum, which can help to future-proof your code if someone adds a new value. In many cases, you can remove the default case and instead return the value later in the method

**Example**

```objc
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = nil;
    switch (indexPath.section) {
        case TAPOIDetailsSectionHeader: {
            cell = [TAResultsCell headerCell];
            break;
        }
        
        case TAPOIDetailsSectionName: {
            cell = [TAResultsCell sectionNameCell];
            break;
        }
            
        case TAPOIDetailsSectionOverview: {
            cell = [TAResultsCell overviewCell];
            break;
        }
        
        // warning: Enumeration values 'TAPOIDetailsSectionNew' and 'TAPOIDetailsSectionUnsupported' not handled in switch
    }
            
    if (!cell) {
        cell = [TAResultsCell defaultCell];
    }
    
    return cell
}



```


## Singletons
When creating singletons, be thread-safe and use `dispatch_once`

**Example**

```objc
+ (TALoginManager *)sharedInstance {
    static TALoginManager *sharedInstance = nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        sharedInstance = [[TALoginManager alloc] init];
    });
    
    return sharedInstance;
}

```


## Blocks
**Needs Owner**

- Prefer Weak references in blocks, this avoids potential future retain cycles, but also it's very rare we ever want to reference/update anything if it has become nil;

## Exceptions and Errors
**Needs Owner**
- Check return over value of error object. use NSException to indicate programmer errors, use NSError to indicate other unexpected errors.

## Categories
**Needs Owner**
- Use categories for helper methods on models (keep models clean)



