# URBN Objective-C Style Guide
This style guide outlines the coding conventions of the iOS team at URBN. We used the [NYT iOS Style Guide](https://github.com/NYTimes/objective-c-style-guide) as a starting point since it had great formatting, covered a breadth of topics, and closely aligned with our opinions on Objective-C style.

## Table of Contents
* [Whitespace](#whitespace)

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
