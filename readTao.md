# Swift style guide for my iOS/macOS projects

## Table of contents
* [Correctness](#correctness)
* [Naming](#naming)
	* [Language](#language)
	* [Selectors](#selectors)
	* [Class Prefixes](#class-prefixes)
	* [Image Naming](#image-naming)
	* [Enumerations](#enumerations)
* [Formatting](#formatting)
	* [Whitespace](#whitespace)
	* [Commas](#commas)
	* [Semicolons](#semicolons)
	* [Braces](#braces)
	* [Parentheses](#parentheses)
* [Types](#types)
	* [Constants](#constants)
	* [Optionals](#optionals)
	* [Struct Initializers](#struct-initializers)
	* [Type Inference](#type-inference)
	* [Final](#final)
* [Protocols](#protocols)
* [Function Declarations](#functiondeclarations)
* [Closure Expressions](#closure-expressions)
* [Ternary Operator](#ternary-operator)
* [CGRect Functions](#cgrect-functions)
* [Good Practices](#good-practices)
* [Code Organization](#code-organization)
	* [Import Statements](#import-statements)
	* [Protocol Conformance](#protocol-conformance)
* [Classes and Structures](#classes-and-structures)
* [Use of Self](#use-of-self)
* [Implicit Getters](#implicit-getters)
* [Functions vs Methods](#functions-vs-methods)
* [Return First](#return-first)
* [Access Control](#access-control)
* [Control Flow](#control-flow)
* [Container Literals](#container-literals)
* [Struct Initializers](#struct-initializers)
* [Type Inference](#type_inference)
* [Short type declarations](short-type-declarations)

## Correctness

Consider warnings to be errors. This rule informs many stylistic decisions such as not to use the `++` or `--` operators, C-style for loops, or strings as selectors.

## Naming

Follow case conventions. Names of types (`class`, `struct`, `enum`, and `protocol`) are UpperCamelCase. Everything else is lowerCamelCase.

Acronyms and initialisms that commonly appear as all upper case in American English should be uniformly up- or down-cased according to case conventions

**Preferred**
```swift
var utf8Bytes: [UTF8.CodeUnit]
var isRepresentableAsASCII = true
var userSMTPServer: SecureSMTPServer
var userID: UserID
```

**Not Preferred**
```swift
var UTF8Bytes: [UTF8.CodeUnit]
var isRepresentableAsAscii = true
var userSmtpServer: SecureSMTPServer
var userId: UserID
```

Other acronyms should be treated as ordinary words

```swift
var radarDetector: RadarScanner
var enjoysScubaDiving = true
```

Name variables, parameters, and associated types according to their roles, rather than their type constraints.
Repurposing a type name in this way fails to optimize clarity and expressivity. Instead, strive to choose a name that expresses the entity’s role.

**Preferred**
```swift
var greeting = "Hello"
protocol ViewController {
	associatedtype ContentView: View
}

class ProductionLine {
	func restock(from supplier: WidgetFactory)
}
```

**Not Perferred**
```swift
var string = "Hello"
protocol ViewController {
	associatedtype ViewType : View
}

class ProductionLine {
	func restock(from widgetFactory: WidgetFactory)
}
```

Use _lbl_-prefix for `UILabel` variables and parameters, _txt_-prefix for `UITextField`, `tv`-prefix for `UITextView`, _btn_-prefix for `UIButton`, _constraint_-prefix for `NSLayoutConstraint` ones.

Use `VC`-subfix for `ViewController` variables and parameters.

```swift
let lblTitle: UILabel!
let txtEmail: UITextField!
let btnDone: UIButton!
let constraintHeightOfTableView: NSLayoutConstraint!

let projectDetailVC = ProjectDetailViewController()
```

If an associated type is so tightly bound to its protocol constraint that the protocol name is the role, avoid collision by appending Type to the associated type name

**Preferred**
```swift
protocol Sequence {
	associatedtype IteratorType: Iterator
}
```

### Language
Use US English spelling to match Apple's API.

**Preferred**
```swift
let color = "red"
```

**Not Preferred**
```swift
let colour = "red"
```

### Image Naming
Image names should be named consistently to preserve organization and developer sanity. Images that are used for a similar purpose SHOULD be grouped in respective groups in an Images folder or `Asset` Catalog.

Use like this _screen name_ + `_` + (ic|bg|logo|banner) + `_` + _image description_ + (@2x|@3x).png
e.g. home_ic_back@2x.png or home_banner@2x.png

### Enumerations

Following Apple's API Design Guidelines for Swift 3, use lowerCamelCase for enumeration values.

```swift
enum Shape {
	case rectangle
	case square
	case rightTriangle
	case equilateralTriangle
}
```

### Dictionay
Use under_score type for `Dictionary` key

```swift
let dict = [
	"the_first_key": "value",
	"the_second_key": "The second value"
]
```

_Rationable:_ Same as JSON keys format from our API

**Do not use number for variable types, instead use **

**Preferred**
```swift
enum FilterType: Int {
	case none = 0
	case country = 1
	case city = 2
	case district = 3
}

func showFilterWithType(type: FilterType) {
	switch type {
	case .country:
		// Do something with _Country_ type
	case .city:
		// Do something with _City_ type
	default:
		break
	}
}
```

**Not Preferred**
```swift
func showFilterWithType(type: Int) {
	switch type {
	case 1:
		// Do something with filter type 1
	case 2:
		// Do something else with filter type 2
	default:
		break
	}
}

```
_Rationable:_ The first version is clear and easy to understandable than the second one.

## Formatting
### Spacing

* Indent using 4 spaces rather than tabs to conserve space and help prevent line wrapping. Be sure to set this preference in Xcode and in the Project settings as shown below:

* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.

**Preferred**
```swift
if user.isHappy {
	// Do something
} else {
	// Do something else
}
```

**Not Preferred**
```swift
if user.isHappy
{
  // Do something
}
else {
  // Do something else
}
```

* Use single spaces around return arrows (->) both in functions and in closures.

**Preferred**
```swift
func getLastIndexOfString(string: String) -> Int {
	// ....
}
```

**Not Preferred**
```swift
func getLastIndexOfString(string: String)->Int {
	// ....
}
```

* There should be exactly one blank line between methods and at the end of Swift source file to aid in visual clarity and organization. Whitespace within methods should separate functionality, but having too many sections in a method often means you should refactor into several methods.

```swift
struct MyStruct {
	func functionA() -> Void {
		// Do something
	}
	
	func functionB() -> String {
		// Do something else
	}
}

```

* Colons always have no space on the left and one space on the right. Exceptions are the ternary operator `? :` and empty dictionary `[:]`.

**Preferred**
```swift
class TestDatabase: Database {
	var data: [String: CGFloat] = ["A": 1.2, "B": 3.2]
}
```

**Not Preferred**
```swift
class TestDatabase : Database {
	var data :[String:CGFloat] = ["A" : 1.2, "B":3.2]
}
```
### Commas

Commas (`,`) should have no whitespace before it, and should have either one space or one newline after.

**Perderred**
```swift
let array = [1, 2, 3, 4, 5]
let stringArray = ["This", "is", "string", "array"]
let dict = [
	"key": "value",
	"the_second_key": "The second value"
]
```

**Not Preferred**
```swift
let array = [1,2,3,4,5]
let stringArray = ["This","is","string", "array"]
```

### Semicolons

Swift does not require a semicolon after each statement in your code. They are only required if you wish to combine multiple statements on a single line.

Do not write multiple statements on a single line separated with semicolons.

The only exception to this rule is the `for-conditional-increment` construct, which requires semicolons. However, alternative `for-in` constructs should be used where possible.

**Preferred**
```swift
let swift = "not a scripting language"
```

**Not Preferred**
```swift
let swift = "not a scripting language";
```

### Braces

Empty declarations should be written in empty braces (`{}`)

```swift
do {
	let string = try self.getStringFromFileWithPath(path)
} catch {}

```

But that code above not good enough for debug, we should print the error messeage sent by `catch` like

```swift
do {
	let string = try self.getStringFromFileWithPath(path)
} catch {
	debugPrint(error)
}

```

Close braces (`}`) should not have empty lines before it

**Preferred**
```swift
func isRTLLanguage() -> Bool {
	if let isRTL = NSUserDefaults.standardUserDefaults().objectForKey(LCLCurrentLanguageType) as? Bool {
	
		return isRTL
	}
```

**Not Preferred**
```swift
func isRTLLanguage() -> Bool {
	if let isRTL = NSUserDefaults.standardUserDefaults().objectForKey(LCLCurrentLanguageType) as? Bool {
	
		return isRTL
		
	}
```

## Parentheses

Parentheses around conditionals are not required and should be omitted.

**Preferred**
```swift
if name == "Hello" && email == "mail@mail.com" {
  print("World")
}
```

**Not Preferred**
```swift
if (name == "Hello") {
  print("World")
}
```

## Code Organization

Use extensions to organize your code into logical blocks of functionality. Each extension should be set off with a `// MARK: -` comment to keep things well-organized.

### Import Statements
`import` statements for OS frameworks and external frameworks should be separated and alphabetized.

**Preferred**
```swift
import Foundation
import UIKit

import Alamofire
import SDWebImage
```

**Not Preferred**
```swift
import Foundation
import UIKit
import Alamofire
import SDWebImage
```

Must have exactly one empty row between the last `import` statement and the next statement.

### Protocols

#### Naming Delegate Protocols
Delegate protocols should always be named in "notification" style. "…didUpdate…", "…willChange…", "…shouldChange…"

In a delegate or data source protocol, the first parameter to each method SHOULD be the object sending the message.

This helps disambiguate in cases when an object is the delegate for multiple similarly-typed objects, and it helps clarify intent to readers of a class implementing these delegate methods.

**Preferred**
```swift
public func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int
```
**Not Preferred**
```swift
public func numberOfRowsInSection(section: Int) -> Int
```

### Protocol Conformance

 In particular, when adding protocol conformance to a model, prefer adding a separate extension for the protocol methods. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.

**Preferred**
```swift
class MyViewController: BaseViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
	// table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
	// scroll view delegate methods
}
```

**Not Preferred**
```swift
class MyViewController: BaseViewController, UITableViewDataSource, UIScrollViewDelegate {
	// all methods
}
```

* (Optional) For `UIKit` view controllers, consider grouping lifecycle, custom accessors, and `@IBAction` in separate class extensions.

```swift
class MyViewController: BaseViewController {
	// class stuff here
}

// MARK: - Actions
extension: MyViewController {
	@IBAction func didTapDoneButton(sender: AnyObject) {
		// Handler action
	}
}

// MARK: - APIs
private extension: MyViewController {
	func callAPIToGetUserProfileData() {
	}
}

```

## Classes and Structures
### (Optional) Use of Self

For conciseness, avoid using `self` since Swift does not require it to access an object's properties or invoke its methods.

Use `self` when required to differentiate between property names and arguments in initializers, and when referencing properties in closure expressions (as required by the compiler)

```swift
class BoardLocation {
	let row: Int, column: Int

	init(row: Int, column: Int) {
		self.row = row
		self.column = column
	}

	let closure = {
		print(self.row)
	}
}
```

## Function Declarations

Don’t use “and” to link keywords that are attributes of the receiver.

**Preferred**
```swift
public func runModalForDirectory(path: String, file name: String, types fileTypes: [String]) -> Int
```

**Not Preferred**
```swift
public func runModalForDirectory(path: String, andFile name: String, andTypes fileTypes: [String]) -> Int
```

_Rationale:_ Although “and” may sound good in this example, it causes problems as you create methods with more and more keywords.

* If the method describes two separate actions, use “and” to link them.

```swift
func openFile(fullPath: String, withApplication appName: String, andDeactivate flag: Bool) -> Bool
```

### Ternary Operator

The intent of the ternary operator, `?` , is to increase clarity or code neatness. The ternary SHOULD only evaluate a single condition per expression. Evaluating multiple conditions is usually more understandable as an if statement or refactored into named variables.

**Preferred**
```swift
let result = a > b ? x : y
```

**Not Preferred**
```swift
let result = a > b ? x = c > d ? c : d : y
```

### `CGRect` Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, code MUST use the `CGGeometry` functions instead of direct struct member access. From Apple's [`CGGeometry`](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CGGeometry/) reference:

All functions described in this reference that take `CGRect` data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the `CGRect` data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**Preferred**
```swift
let frame = self.view.frame

let x = CGRectGetMinX(frame)
let y = CGRectGetMinY(frame)
let width = CGRectGetWidth(frame)
let height = CGRectGetHeight(frame)
```

**Not Preferred**
```swift
let frame = self.view.frame

let x = frame.origin.x
let y = frame.origin.y
let width = frame.size.width
let height = frame.size.height
```

## Types

Always use Swift's native types when available. Swift offers bridging to Objective-C so you can still use the full set of methods as needed.

**Preferred**
```swift
let width = 120.0                                    // Double
let widthString = (width as NSNumber).stringValue    // String
```

**Not Preferred**
```swift
let width: NSNumber = 120.0                          // NSNumber
let widthString: NSString = width.stringValue        // NSString
```

In Sprite Kit code, use `CGFloat` if it makes the code more succinct by avoiding too many conversions.

### Constants

Constants are defined using the `let` keyword, and variables with the `var` keyword. Always use `let` instead of `var` if the value of the variable will not change.

**Tip:** A good technique is to define everything using `let` and only change it to `var` if the compiler complains!

You can define constants on a type rather than an instance of that type using type properties. To declare a type property as a constant simply use `static let`. Type properties declared in this way are generally preferred over global constants because they are easier to distinguish from instance properties. Example:

**Preferred**
```swift
enum Math {
	static let e  = 2.718281828459045235360287
	static let pi = 3.141592653589793238462643
}

radius * Math.pi * 2 // circumference
```

**Note:** The advantage of using a case-less enumeration is that it can't accidentally be instantiated and works as a pure namespace.

**Not Preferred**
```swift
let e  = 2.718281828459045235360287  // pollutes global namespace
let pi = 3.141592653589793238462643

radius * pi * 2 // is pi instance data or a global constant?
```

### Optionals

Declare variables and function return types as optional with `?` where a nil value is acceptable.

Use implicitly unwrapped types declared with `!` only for instance variables that you know will be initialized later before use, such as subviews that will be set up in `viewDidLoad`.

When accessing an optional value, use optional chaining if the value is only accessed once or if there are many optionals in the chain

```swift
self.textContainer?.textLabel?.setNeedsDisplay()
```

Use optional binding when it's more convenient to unwrap once and perform multiple operations:

```swift
if let _textContainer = self.textContainer {
	// do many things with textContainer
}
```

When naming optional variables and properties, avoid naming them like `optionalString` or `maybeView` since their optional-ness is already in the type declaration.

For optional binding, shadow the original name with _underscore_ prefix (`_`) when appropriate rather than using names like `unwrappedView` or `actualLabel`.

**Preferred**
```swift
var subview: UIView?
var volume: Double?

// later on...
if let _subview = subview, _volume = volume {
	// do something with unwrapped subview and volume
}
```

**Not Preferred**
```swift
var optionalSubview: UIView?
var volume: Double?

if let unwrappedSubview = optionalSubview {
	if let realVolume = volume {
		// do something with unwrappedSubview and realVolume
	}
}
```

#### Avoid Using Force-Unwrapping of Optionals

If you have an identifier `foo` of type `FooType?` or `FooType!`, don't force-unwrap it to get to the underlying value (`foo!`) if possible.

```swift
if let _foo = foo {
	// Use unwrapped `foo` value in here
} else {
	// If appropriate, handle the case where the optional is nil
}
```

Alternatively, you might want to use Swift's Optional Chaining in some of these cases, such as:

```swift
// Call the function if `foo` is not nil. If `foo` is nil, ignore we ever tried to make the call
foo?.callSomethingIfFooIsNotNil()
```

_Rationale:_ Explicit `if let`-binding of optionals results in safer code. Force unwrapping is more prone to lead to runtime crashes.

#### Avoid Using Implicitly Unwrapped Optionals

Where possible, use `let foo: FooType?` instead of `let foo: FooType!` if `foo` may be nil (Note that in general, `?` can be used instead of `!`).

_Rationale:_ Explicit optionals result in safer code. Implicitly unwrapped optionals have the potential of crashing at runtime.

### Struct Initializers

Use the native Swift struct initializers rather than the legacy CGGeometry constructors.

**Preferred**
```swift
let bounds = CGRect(x: 40, y: 20, width: 120, height: 80)
let centerPoint = CGPoint(x: 96, y: 42)
```

**Not Preferred**
```swift
let bounds = CGRectMake(40, 20, 120, 80)
let centerPoint = CGPointMake(96, 42)
```

Prefer the struct-scope constants `CGRect.infinite`, `CGRect.null`, etc. over global constants `CGRectInfinite`, `CGRectNull`, etc. For existing variables, you can use the shorter `.zero`.

### (Optional) Type Inference

Prefer compact code and let the compiler infer the type for constants or variables of single instances. Type inference is also appropriate for small (non-empty) arrays and dictionaries. When required, specify the specific type such as `CGFloat` or `Int16`.

**Preferred**
```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = ["Mic", "Sam", "Christine"]
let maximumWidth: CGFloat = 106.5
```

**Not Preferred**
```swift
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
let names = [String]()
```

#### Type Annotation for Empty Arrays and Dictionaries

For empty arrays and dictionaries, use type annotation. (For an array or dictionary assigned to a large, multi-line literal, use type annotation.)

**Preferred**
```swift
var names = [String]()
var lookup = [String: Int]()
```

**Not Preferred**
```swift
var names: [String] = []
var lookup: [String: Int] = [:]
```

### Short type declarations

Prefer the shortcut versions of type declarations over the full generics syntax.

**Preferred**
```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
var dictArray: Array<Dictionary<String, AnyObject>>? or var dictArray: [[String: AnyObject]]?
```

**Not Preferred**
```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

## Functions vs Methods

Free functions, which aren't attached to a class or type, should be used sparingly. When possible, prefer to use a method instead of a free function. This aids in readability and discoverability.

Free functions are most appropriate when they aren't associated with any particular type or instance.

**Preferred**
```swift
let sorted = items.mergeSort()  // easily discoverable
rocket.launch()  // clearly acts on the model
```

**Not Preferred**
```swift
let sorted = mergeSort(items)  // hard to discover
launch(&rocket)
```

**Free Function Exceptions**
```swift
let tuples = zip(a, b)  // feels natural as a free function (symmetry)
let value = max(x,y,z)  // another free function that feels natural
```
## Access Control

Full access control annotation in tutorials can distract from the main topic and is not required. Using `private` appropriately, however, adds clarity and promotes encapsulation. Use `private` as the leading property specifier. The only things that should come before access control are the `static` specifier or attributes such as `@IBAction` and `@IBOutlet`.

**Preferred**
```swift
class TimeMachine {  
  private dynamic lazy var fluxCapacitor = FluxCapacitor()
}
```

**Not Preferred**
```swift
class TimeMachine {  
	lazy dynamic private var fluxCapacitor = FluxCapacitor()
}
```

#### Always specify access control explicitly for top-level definitions

Top-level functions, types, and variables should always have explicit access control specifiers:

```swift
public var whoopsGlobalState: Int
internal struct TheFez {}
private func doTheThings(things: [Thing]) {}
```

However, definitions within those can leave access control implicit, where appropriate:

```swift
internal struct TheFez {
	var owner: Person = Joshaber()
}
```

_Rationale:_ It's rarely appropriate for top-level definitions to be specifically `internal`, and being explicit ensures that careful thought goes into that decision. Within a definition, reusing the same access control specifier is just duplicative, and the default is usually reasonable.

`IBAction`s and `IBOutlet`s should be declared `private` if possible

**Preferred**
```swift
class MyViewController {
	@IBOutlet private weak var lblTitle: UILabel!
	
	@IBAction private func didTapDoneButton(sender: AnyObject) {
		// Do something
	}
}

```
**Not Preferred**
```swift
class MyViewController {
	@IBOutlet weak var lblTitle: UILabel!
	
	@IBAction func didTapDoneButton(sender: AnyObject) {
		// Do something
	}
}

```

_Rationale:_ Avoid unwanted access to these variables and actions from other outside classes

#### Final

Classes should start as `final`, and only be changed to allow subclassing if a valid need for inheritance has been identified. Even in that case, as many definitions as possible _within_ the class should be `final` as well, following the same rules.

## Control Flow

Prefer the `for-in` style of `for` loop over the `while-condition-increment` style.

**Preferred**
```swift
for _ in 0..<3 {
	print("Hello three times")
}

for (index, person) in attendeeList.enumerate() {
	print("\(person) is at position #\(index)")
}

for index in 0.stride(to: items.count, by: 2) {
	print(index)
}

for index in (0...3).reverse() {
	print(index)
}
```

**Not Preferred**
```swift
var i = 0
while i < 3 {
	print("Hello three times")
	i += 1
}

var i = 0
while i < attendeeList.count {
	let person = attendeeList[i]
	print("\(person) is at position #\(i)")
	i += 1
}
```

## Return First

When coding with conditionals, the left hand margin of the code should be the "golden" or "happy" path. That is, don't nest `if` statements. Multiple return statements are OK. The `guard` statement is built for this.

**Preferred**
```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

	guard let context = context else { throw FFTError.noContext }
	guard let inputData = inputData else { throw FFTError.noInputData }
    
	// use context and input to compute the frequencies
    
	return frequencies
}
```

**Not Preferred**
```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

	if let context = context {
		if let inputData = inputData {
			// use context and input to compute the frequencies

		return frequencies
    }
	else {
		throw FFTError.noInputData
	}
	}
	else {
		throw FFTError.noContext
	}
}
```

When multiple optionals are unwrapped either with `guard` or `if let`, minimize nesting by using the compound version when possible. Example:

**Preferred**
```swift
guard let _number1 = number1, _number2 = number2, _number3 = number3 else { fatalError("impossible") }
	// do something with numbers
```

**Not Preferred**
```swift
if let _number1 = number1 {
	if let _number2 = number2 {
		if let _number3 = number3 {
			// do something with numbers
		} else {
			fatalError("impossible")
		}
	} else {
		fatalError("impossible")
	}
} else {
	fatalError("impossible")
}
```

#### Implicit Getters
Prefer implicit getters on read-only properties and subscripts

When possible, omit the `get` keyword on read-only computed properties and read-only subscripts.

**Preferred**
```swift
var myGreatProperty: Int {
	return 4
}

subscript(index: Int) -> T {
	return objects[index]
}
```

**Not Preferred**
```swift
var myGreatProperty: Int {
	get {
		return 4
	}
}

subscript(index: Int) -> T {
	get {
		return objects[index]
	}
}
```

_Rationale:_ The intent and meaning of the first version are clear, and results in less code.

## Return
Should have empty lines before `return` statement. Exceptions are the functions or the closures that have exactly only `return` statement.

```swift
func getUserID() -> Int {
	return self.userID
}

func doSomething() {
	let values = self.getSomeValues()

	return values
}

```

### Container Literals

If the collection spans more than a single line, place the opening bracket on the same line as the declaration and place the closing bracket on a new line that is indented to the same level as the opening bracket.

```swift
let stringArray = [
	"This",
	"is",
	"string",
	"array",
]

let dictionary = [
	NSFontAttributeName: UIFont(name: "Helvetica-Bold", size: 12),
	NSForegroundColorAttributeName: UIColor.greenColor()
]
```

## Model-View-Controller
Separate the model from the view. Separate the controller from the view and the model. Use `protocols` for callback APIs.
Separate model from view: don't build assumptions about the presentation into the model or data source. Keep the interface between the data source and the presentation abstract. Don't give the model knowledge of its view. (A good rule of thumb is to ask yourself if it's possible to have multiple presentations, with different states, on a single instance of your data source.)
Separate controller from view and model: don't put all of the "business logic" into view-related classes; this makes the code very unusable. Make controller classes to host this code, but ensure that the controller classes don't make too many assumptions about the presentation.
Define callback APIs with `protocol`, using `@optional` if not all the methods are required.
From [Google](https://google.github.io/styleguide/objcguide.xml?showone=Model/View/Controller#Model/View/Controller)

## This guide is inspired from these sources (most taken from [Raywenderlich](https://github.com/raywenderlich/swift-style-guide/))
* [The Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
* [The Raywenderlich Swift Style Guide](https://github.com/raywenderlich/swift-style-guide/)
* [The Eure Swift Style Guide](https://github.com/eure/swift-style-guide/)
* [The Github Swift Style Guide](https://github.com/github/swift-style-guide/)