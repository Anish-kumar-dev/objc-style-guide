# URBN Objective-C Style Guide
This style guide outlines the coding conventions of the iOS team at URBN. We used the [NYT iOS Style Guide](https://github.com/NYTimes/objective-c-style-guide) as a starting point since it had great formatting, covered a breadth of topics, and closely aligned with our opinions on Objective-C style.

## Table of Contents
* [Whitespace](#whitespace)
* [Code Organization](#code-organization)
* [Modern Objective-C](#modern-objective-c)
* [Naming](#naming)
* [Properties](#properties)

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











