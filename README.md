## Table of Contents
* [Naming](#naming)
	* [Functions](#functions)
	* [Variables](#variables)
	* [Images](#images)
	* [Protocols](#protocols)
	* [Constants](#constants)
* [Spacing](#spacing)
* [Ternary Operator](#ternary)
* [Error Handling](#error)
* [Extensions](#extensions)
* [Comments](#comments)
* [CGRect Functions](#CGRect)

##Naming 

Names should be descriptive and camel cased. Type names should be capitalized, while function and variable names should start with a lower case letter.

Abbreviations and acronyms should be avoided when possible.  The Swift [API Design Guidelines](https://swift.org/documentation/api-design-guidelines/#follow-case-conventions) should be followed by uniformly uppercasing or lowercasing	any acronyms or initialisms.

**Example:**
```swift
var utf8Bytes: [UTF8.CodeUnit]
var isRepresentableAsASCII = true
var userSMTPServer: SecureSMTPServer
```

**Not:**
```swift
var uTF8Bytes: [UTF8.CodeUnit]
var isRepresentableAsAscii = true
var userSmtpServer: SecureSmtpServer
```

###Functions
 
The function name should not include any parameters. All parameters should be named.  The opening brace should be on a new line.

**Example:**

```swift
	func loadImage(withName name: String) -> UIImage
	{ 
	}
```

###Variables

Variables should be named descriptively, with the variable’s name clearly communicating what the variable is and pertinent information a programmer needs to use that value properly. Type inference should be relied on whenever possible.

**Example:**

- ```let title: String``` - It is reasonable to assume a “title” is a string.
- ```let titleHTML: String``` - This indicates a title that may contain HTML which needs parsing for display. “HTML” is needed for a programmer to use this variable effectively.
- ```let titleAttributedString: NSAttributedString``` - A title, already formatted for display. AttributedString hints that this value is not just a vanilla title, and adding it could be a reasonable choice depending on context.
- ```let now: NSDate``` - No further clarification is needed.
- ```let URL: NSURL``` vs. ```let URLString: String``` - In situations when a value can reasonably be represented by different classes, it is often useful to disambiguate in the variable’s name.
- ```let releaseDateString: String``` - Another example where a value could be represented by another class, and the name can help disambiguate.
- Single letter variable names should be avoided except as simple counter variables in loops.

Bool values should not included “is”, “did”, or any other modifier.

**Example:**

```swift
	var loaded: Bool
```

**Not:**

```swift
	var isLoaded: Bool
	var didLoad: Bool
```

Static variables should be camel-case with the first letter capitalized.  All other variables should be camel-case with the first letter lowercase.

**Example:**

```swift
	static var RequestTimeOut = 1000
	
	var requestCount: Int
```

###Images

Image names should be named consistently to preserve organization. They should be named as one camel case string with a description of their purpose, followed by the un-prefixed name of the class or property they are customizing (if there is one), followed by a further description of color and/or placement, and finally their state.

**Example:**

RefreshBarButtonItem / RefreshBarButtonItem@2x RefreshBarButtonItemSelected / RefreshBarButtonItemSelected@2x
RefreshBarButtonItemWhite / RefreshBarButtonItemWhite@2x
RefreshBarButtonItemWhiteSelected / RefreshBarButtonItemWhiteSelected@2x

Images that are used for a similar purpose should be grouped in respective groups in an Images folder or Asset Catalog.

###Protocols

In a delegate or data source protocol, the first parameter to each method should be the object sending the message.

This helps disambiguate in cases when an object is the delegate for multiple similarly-typed objects, and it helps clarify intent to readers of a class implementing these delegate methods.

**Example:**

```swift
	func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) 
	{
		…
	}
```

**Not:**

```swift
	func didSelectTableRowAtIndexPath(indexPath: NSIndexPath)
	{
		…
	}
```

###Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. Constants can be declared by static variables, or by an enum with raw values.

**Example:**

```
	static var HomeViewControllerIdentifier = “Home View Controller”

	static var MaximumRefreshTime = 10000

	enum Keys: String {
		case Filename: “filename”
		case Image = “image”
		case Thumbnail = “thumbnail”
	}
```

#Spacing

Function braces always open on a new line and have a space before the return

**Example:**
```
	func doSomething() -> Int
	{
		print(“something”)

		return 1
	}
```

Other braces (if/else/switch/while etc.) always open on the same line as the statement but close on a new line. `else` statements should be on a new line. 

**Example:**
```
	if (user.isHappy) {
		// Do something
	}
	else {
		// Do something else
	}
```
There should be exactly one blank line between functions to aid in visual clarity and organization.
Whitespace within functions should be used to separate functionality (though often this can indicate an opportunity to split the function into several, smaller functions).
Each import should be on a new line.
There should be a blank line between imports and type definitions.

#Ternary Operator

The ternary operator, ? , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an if statement, or refactored into named variables.

**Example:**

```
	result = a > b ? x : y;
```

**Not:**

```
	result = a > b ? x = c > d ? c : d : y;
```

#Error Handling

Throwing errors and using do / catch syntax is preferred when a method returns a result type synchronously.  When throwing an error isn’t possible, as in the case of a value returned asynchronously, a Result Type can be used with an associated return value or error.

**Example:**

```
	enum Result {
		case .Success(Int)
		case .Failure(NSError)
	}

	func doSomething() throws -> Int

	func doSomethingAsynchronously(completion: Result)
```

#Extensions

Extensions may be used to concisely segment functionality and should be named to describe that functionality.

If an extension has its own file, the file should be named to describe the functionality.  If the extension is in a shared file, it should be named using ```// MARK: Extension Name```

**Example:**
```
	// MARK:  Image Processing
	extension UIImage { … }
```

Functions and properties added in extensions on types not owned by Fish Hook should be prefixed with `fhk_`. This avoids unintentionally overriding an existing method, and it reduces the chance of two categories from different libraries adding a method of the same name.

**Example:**

```
	extension NSDictionary {
		func fhk_objectOrNullForKey(key: String)
	}
```

**Not:**

```
	extension NSDictionary {
		func objectOrNullForKey(key: String)
	}
```

#Comments

When they are needed, comments should be used to explain why a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. This does not apply to those comments used to generate documentation.

#CGRect Functions

Swift has extended CGRect to add getters for dimensional values, which means the CGRect functions are no longer required.  Computed properties can be accessed directly

Note: This is opposite of the convention used in Objective-C

**Example:**

```
	let leftEdge = view.frame.minX
```

**Not:**

```
	let leftEdge = CGRectMinX(view.frame)
```

#Xcode Project

Any Xcode groups created should be reflected by folders in the filesystem. Code should be grouped by type, and further grouped by feature when possible.

#File Organization

Files should have the required imports first, followed by the Type definition.  Within the type definition, public properties should be listed first, followed by initializers, and finally private properties.

Private functions should be separated into a private extension.

Whenever possible, code should be grouped into extensions by behavior and labeled with ```//MARK: -```, for example conformance to a protocol, or a collection of functions that handle user input.

**Example:**

```
	import FishHookFramework

	class Foo {

		var bar: Bar

		init(bar: Bar)
		{
			…
		}

		func doThing()
		{
			…
		}

		private secretBar: Bar
	}

	private extension Foo {
		func doSecretThing()
		{
			…
		}
	}

```

#Nested Types

When a class exists solely to be used internally by another class, it should be declared within the scope of the class that uses it.  Nested types should never be referenced outside of their definition context.  If the type is needed outside of that context, it should not be nested.

**Example:**

```
	struct Builder {
		enum InternalError: ErrorType {
			…
		}
	}
```

#Optionals

Using the ```!``` operator undermines type safety and is considered a code smell.  The exception to this rule is properties that must be set but cannot be set at initialization time.  For example, IBOutlets or required delegates.  In these cases where a missing value is considered a programmer error, we prefer to crash as early as possible and the use of the ```!``` operator is allowed.  Alternatively, a guard let clause can be used followed by a fatal error if including a readable message would be useful at the time of the error.

**Example:**

```
	@IBOutlet weak var loginButton: UIButton!

	guard let member = member else {
		fatalError(“a member is required before attempting to show member detail”
	}
```

#Closures

Trailing closure syntax should not be used when a function takes more than one closure, such as a function with separate success and failure handlers.

**Example:**

```
	controller.doSomething(successHandler: {…}, failureHandler: {…})
```

**Not:**

```
	controller.doSomething(successHandler: {…}) {…}
```

Single operation closures should be condensed to one line and use an implicit return when it does not negatively impact readability, otherwise the first line of the closure should be on a new line following the opening brace. When using trailing closure syntax, there should be a space before the opening brace.

**Example:**

```
	let goodApples = apples.filter { $0.isGood }

	let goodApples = fruit.filter {
		if let apple = $0 as Apple {
			return apple.isGood
		}

		re	turn false
	}
```

**Not:**

```
	let goodApples = apples.filter {
		return $0.isGood
	}
```
