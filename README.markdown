# URBN Objective-C Style Guide
This style guide outlines the coding conventions of the iOS team at URBN. We used the [NYT iOS Style Guide](https://github.com/NYTimes/objective-c-style-guide) as a starting point since it had great formatting, covered a breadth of topics, and closely aligned with our opinions on Objective-C style.

## Table of Contents
* [Whitespace](#whitespace)
* [Code Organization](#code-organization)
* [Modern Objective-C](#modern-objective-c)
* [Naming](#naming)
* [Properties](#properties)
* [Methods](#methods)
* [Conditionals](#conditionals)

## Whitespace
* Use vertical white space judiciously to organize units of code, especially within method bodies. 
* End files with a newline after `@end`.
* Don’t leave trailing whitespace.
* Use 4 spaces, not a tab for indentation. This is the default in Xcode. 

## Code Organization
* Group code in implementation files by logical units.
* Use `#pragma mark`’s to separate these logical units of code.
* As a blueprint for implementation organization is as follows:
  * Super Method Overrides
  * Class specific methods
  * Protocols
* Lifecycle methods should appear in the order in which they will be called.

**Example**
```objc
@implementation CustomViewController
- (instancetype)init {
    self = [super init];
    return self;
}

#pragma mark - Lifecycle
- (void)viewDidLoad {
    [super viewDidLoad];
}

#pragma mark - CustomViewController
- (void)internalOrPublicMethod {
}

#pragma mark - UITableViewDelegate/DataSource
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
}
@end
```

## Modern Objective-C
* Apple has modernized Objective-C and you should take full advantage of the newer syntax.
* Literal syntax should be used where ever possible.
  * `@1` (NSNumber), `@“”` (NSString), `@[]` (NSArray), `@{}` (NSDictionary)
* However do not abuse this syntax to create mutable copies:
  * **GOOD** `[NSMutableArray new]`
  * **BAD** `[@[] mutableCopy]`
* Do not use the `@synthesize` compiler directive unless you need to override the default instance variable name or you need to implement an optional property from a protocol.
* Use `@property`’s instead of iVars.
* Constructors should return `instancetype` instead of `id`.
* Use constants instead of `#define`’s wherever possible.
* Use `NS_ENUM` & `NS_OPTIONS` instead of `enum`.

## Naming
* Objective-C is self documenting because of the verbose nature of its naming conventions. This should be adhered to wherever possible.
* Consider using a organization (URBN) or app specific (ANT) prefix for all classes.
* Categories methods should be prefixed similarly with an underscore between the prefix and the method name.
* Classes, ENUMS, Bitmasks, and Constants should be uppercase camel case.
* Methods and Variables should be lowercase camel case.
* Constants should be namespaced properly with the class name and their description to avoid potential collisions.

**Example**
```objc
//Constant
static const CGFloat kURBNTextFieldLoadingIndicatorAnimationDuration = .25f;

//ENUM & Bitmask
typedef NS_ENUM(NSUInteger, ANTBannerStyle) {
    ANTBannerStyleDefault,
    ANTBannerStyleSuccess,
    ANTBannerStyleWarning,
    ANTBannerStyleError
};

typedef NS_OPTIONS(NSUInteger, ANTFormDatePicking) {
    ANTFormDatePickingYear = 0,
    ANTFormDatePickingMonth = 1 << 0,
    ANTFormDatePickingDay = 1 << 1
};

//Classes
URBNTextField.h

//Categories
- (void)urbn_showLoading:(BOOL)loading animated:(BOOL)isAnimated;
```

## Properties
* As previously stated, properties are to be used instead of iVars.
* Properties should be explicitly declared with atomicity & memory management rules every time in that order. Optionally you can declare access rules and conventions after atomicity & memory management.
* Dot syntax should be used to access properties instead of bracket syntax.
* There should be exactly one space between the `@property` keyword and the qualifiers. One space between the qualifiers and the type. And (if applicable) a space before the pointer with the point attached to the property name.
* Property names should read like a noun.

**This**
```objc
@property (nonatomic, strong, readonly) NSObject *myObject;
@property (atomic, weak) NSObject *myOtherObject;
```

**Not This**
```objc
@property (strong, readonly, nonatomic) NSObject *myObject;
@property (weak) NSObject *myOtherObject;
```

## Methods
* Methods should be small units of code that accomplish a single task. Endless methods that contain a menagerie of logic are not preferred.
* Method names should read like a sentence.
* Methods should have a space after the scope identifier (+/-). No space after the return type and a space between each segment. There should be no spaces within the segments (except for pointer syntax in parameter types).
* There should not be an empty line at the beginning or ending of a method.
* There should be exactly 1 line between methods.
* There should be no space between a `#pragma mark` and the following method.
* Calls to `super` should always have an empty line following them.
* `return`s should allows have an empty line before them.
* Use single empty lines within methods to group code logically.
* Method signatures in implementation files should flow naturally and not have returns between the segments. The practice of method segments on new lines is *acceptable* for complex method signatures in header files.

**Examples**
```objc
@interface MyClass : UIViewController

- (NSString *)doSomethingWithObject:(id)object string:(NSString *)string index:(int)index;
```
```objc
@implementation MyClass

#pragma mark - LifeCycle
- (void)viewDidLoad {
    [super viewDidLoad];
    
    UILabel *lbl = [UILabel new];
    [self.view addSubview:lbl];
    
    UIImageView *imgView = [[UIImageView alloc] initWithImage:self.myImage];
    [self.view addSubview:imgView];
}

#pragma mark - MyClass
- (NSString *)doSomethingWithObject:(id)object string:(NSString *)string index:(int)index {
    string = [string stringByAppendingString:@"BOOM"];
    
    return string;
}
```
## Conditionals
* Conditionals should **always** utilize curly braces. Even if it can be written on a single line.
* Curly braces should open on the same line as the condition and close on their own line.
* Each keyword in a conditional (`if/else if/else`) should begin on their own line. Do not place a keyword on the same line as a closing brace from the previous condition.
* The final conditional in a group of conditions should be followed by a single empty line unless it is the end of a method, conditional, block, etc.
* There should be a single space between the keyword, condtion, and curly brace.
* There should be no empty line at the top or bottom of a conditional body (much like a method body).

**Example**
```obj
if (condition == 0) {
    return 0;
}
else if (otherCondition == 1 && condition == 1) {
    myString = @"BOOM";
    return 1;
}
else {
    [self doSomethingElseHere];
} 
```
### Ternary Operator
* Ternary operators should only be used to express simple conditions.
* Use a single space to separate segments of the ternary statement.

**This**
```objc
result = a > b ? x : y;
```
**Not This**
```objc
result = a > b ? x = c > d ? c : d : y;
```