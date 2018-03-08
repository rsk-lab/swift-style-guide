# Swift Style Guide

Make sure to read [Apple's API Design Guidelines](https://swift.org/documentation/api-design-guidelines/).

Specifics from these guidelines + additional remarks are mentioned below.

This guide was last updated for Swift 4.0 on March 7, 2018.

## Table Of Contents

- [Swift Style Guide](#swift-style-guide)
    - [1. Code Formatting](#1-code-formatting)
    - [2. Naming](#2-naming)
    - [3. Coding Style](#3-coding-style)
        - [3.1 General](#31-general)
        - [3.2 Access Modifiers](#32-access-modifiers)
        - [3.3 Custom Operators](#33-custom-operators)
        - [3.4 Switch Statements and `enum`s](#34-switch-statements-and-enums)
        - [3.5 Optionals](#35-optionals)
        - [3.6 Protocols](#36-protocols)
        - [3.7 Properties](#37-properties)
        - [3.8 Closures](#38-closures)
        - [3.9 Arrays](#39-arrays)
        - [3.10 Error Handling](#310-error-handling)
        - [3.11 Using `guard` Statements](#311-using-guard-statements)
    - [4. Dependencies/File Structure](#4-dependenciesfile-structure)
        - [4.1 Import Statements](#41-import-statements)
        - [4.2 File Structure](#42-file-structure)
    - [5. Documentation/Comments](#5-documentationcomments)
        - [5.1 Documentation](#51-documentation)
        - [5.2 Other Commenting Guidelines](#52-other-commenting-guidelines)

## 1. Code Formatting

* **1.1** Use 4 spaces for tabs.
* **1.2** Ensure that there is a newline at the end of every file.
* **1.3** Ensure that there is no trailing whitespace (Xcode->Preferences->Text Editing->Automatically trim trailing whitespace).
* **1.4** The opening brace should be followed by an empty line. Give breathing room when scanning for code.
* **1.5** Do not place opening braces on new lines - we use the [Stroustrup style](https://en.m.wikipedia.org/wiki/Indentation_style#Variant:_Stroustrup).

```swift
internal final class SomeClass {

    internal func someMethod() {
    
        if x == y {
        
            /* ... */
        }
        else if x == z {
        
            /* ... */
        }
        else {
        
            /* ... */
        }
    }

    /* ... */
}
```

* **1.6** Don’t use one-liners (except for protocols).

```swift
// PREFERRED
guard let firstValue = firstValue else {

    return
}

// NOT PREFERRED
guard let firstValue = firstValue else { return }
```

* **1.7** Add one empty line between functions. Give breathing room between code blocks.

* **1.8** Empty declarations should be written in empty braces `{}`. Make it clear that the declaration was meant to be empty and not just a missing `TODO`.

```swift
// PREFERRED
extension Level: Equatable {}

// NOT PREFERRED
extension Level: Equatable { }

// EVEN LESS PREFERRED
extension Level: Equatable {
}
```

* **1.9** When writing a type for a property, constant, variable, a key for a dictionary, a function argument, a protocol conformance, or a superclass, don't add a space before the colon.

```swift
// Specifying type.
let membersViewController: MembersViewController

// Dictionary syntax (note that we left-align as opposed to aligning colons).
let membersDictionary: [String: Any] = [

    "CTO": "Ruslan Skorb",
    /* ... */
]

// Declaring a function.
internal func someFunction<T, U: SomeProtocol>(firstArgument: U, secondArgument: T) where T.RelatedType == U {

    /* ... */
}

// Calling a function.
someFunction(someArgument: "R.SK Lab")

// Superclasses.
internal final class MembersViewController: UIViewController {

    /* ... */
}

// Protocols.
extension MembersViewController: UITableViewDataSource {

    /* ... */
}
```

* **1.10** In general, there should be a space following a comma.

```swift
let membersArray = ["Ruslan Skorb", /* ... */]
```

* **1.11** There should be a space before and after a binary operator such as `+`, `==`, or `->`. There should also not be a space after a `(` and before a `)`.

```swift
let result = 10 + (18 / 3) * 5

if 2 + 2 == 5 {

    fatalError("Is the Matrix broken?")
}

internal func code(with tea: Tea) -> Happiness {

    /* ... */
}
```

* **1.12** We follow Xcode's recommended indentation style (i.e. your code should not change if CTRL-I is pressed). When declaring a function that spans multiple lines, prefer using that syntax to which Xcode, as of version 9.2, defaults.

```swift
// Xcode indentation for a function declaration that spans multiple lines
internal func someFunctionWithManyParameters(parameterOne: String,
                                             parameterTwo: String,
                                             parameterThree: String) {
    
    // Xcode indents to here for this kind of statement
    print("\(parameterOne) \(parameterTwo) \(parameterThree)")
}

// Xcode indentation for a multi-line `if` statement
if firstValue > (secondValue + thirdValue)
    && fourthValue == .someEnumValue {

    // Xcode indents to here for this kind of statement
    print("\(firstValue) \(fourthValue)")
}
```

* **1.13** When calling a function that has many parameters, put each argument on a separate line with a single extra indentation.

```swift
someFunctionWithManyArguments(
    firstArgument: localProperty,
    secondArgument: resultFromSomeFunction(),
    thirdArgument: someOtherLocalProperty)
```

* **1.14** When dealing with an implicit array or dictionary large enough to warrant splitting it into multiple lines, treat the `[` and `]` as if they were braces in a method, `if` statement, etc. Closures in a method should be treated similarly.

```swift
someFunctionWithABunchOfArguments(
    someStringArgument: "R.SK Lab",
    someArrayArgument: [
    
        "Ruslan Skorb",
        /* ... */
    ],
    someDictionaryArgument: [
    
        "CTO": "Ruslan Skorb",
        /* ... */
    ],
    someClosure: { parameter1 in
    
        print(parameter1)
    })
```

* **1.15** Prefer using local constants or other mitigation techniques to avoid multi-line predicates where possible.

```swift
// PREFERRED
let firstCondition = x == firstReallyReallyLongPredicateFunction()
let secondCondition = y == secondReallyReallyLongPredicateFunction()
let thirdCondition = z == thirdReallyReallyLongPredicateFunction()
if firstCondition && secondCondition && thirdCondition {

    // do something
}

// NOT PREFERRED
if x == firstReallyReallyLongPredicateFunction()
    && y == secondReallyReallyLongPredicateFunction()
    && z == thirdReallyReallyLongPredicateFunction() {
    
    // do something
}
```

## 2. Naming

* **2.1** There is no need for Objective-C style prefixing in Swift (e.g. use just `AppDelegate` instead of `RSKAppDelegate`).

* **2.2** Use `PascalCase` for type names (e.g. `struct`, `enum`, `class`, `typedef`, `associatedtype`, etc.).

* **2.3** Use `camelCase` (initial lowercase letter) for function, method, property, constant, variable, argument names, enum cases, etc.

* **2.4** When dealing with an acronym or other name that is usually written in all caps, actually use all caps in any names that use this in code. The exception is if this word is at the start of a name that needs to start with lowercase - in this case, use all lowercase for the acronym.

```swift
// "HTML" is at the start of a constant name, so we use lowercase "html".
let htmlBodyContentString = "<p>R.SK Lab</p>"

// Prefer using ID to Id.
let profileID = 1

// Prefer URLFinder to UrlFinder.
internal final class URLFinder {

    /* ... */
}
```

* **2.5** All constants that are instance-independent should be `static`. All such `static` constants should be placed in a marked section of their `class`, `struct`, or `enum`. For classes with many constants, you should group constants that have similar or the same prefixes, suffixes and/or use cases.

```swift
// PREFERRED    
internal final class SomeClassName {

    // MARK: - Constants
    
    internal static let numberOfItemsInRow = 3
    
    internal static let reuseIdentifier = "SomeClassNameReuseIdentifier"
    
    internal static let shared = SomeClassName()
}

// NOT PREFERRED
internal final class SomeClassName {

    // Don't use `k`-prefix.
    internal static let kReuseIdentifier = "SomeClassNameReuseIdentifier"
    
    // Don't namespace constants.
    internal enum Constant {
    
        internal static let numberOfItemsInRow = 3
    }
}
```

* **2.6** For generics and associated types, use a `PascalCase` word that describes the generic. If this word clashes with a protocol that it conforms to or a superclass that it subclasses, you can append a `Type` suffix to the associated type or generic name.

```swift
internal class SomeClass<Model> { /* ... */ }

internal protocol Modelable {

    associatedtype Model
}

internal protocol Sequence {

    associatedtype IteratorType: Iterator
}
```

* **2.7** Names should be descriptive and unambiguous.

```swift
// PREFERRED
internal class RoundAnimatingButton: UIButton { /* ... */ }

// NOT PREFERRED
internal class CustomButton: UIButton { /* ... */ }
```

* **2.8** Do not abbreviate, use shortened names, or single letter names.

```swift
// PREFERRED
internal class RoundAnimatingButton: UIButton {

    internal let animationDuration: NSTimeInterval

    internal func startAnimating() {
    
        let firstSubview = subviews.first
    }

}

// NOT PREFERRED
internal class RoundAnimating: UIButton {

    internal let aniDur: NSTimeInterval

    internal func srtAnmating() {
    
        let v = subviews.first
    }
}
```

* **2.9** Include type information in constant or variable names when it is not obvious otherwise.

```swift
// PREFERRED
internal final class UserViewController: UIViewController {

    internal let showUserTeamAnimationDuration: TimeInterval
    
    internal let userAvatarImageView: UIImageView
    
    // It is ok not to include string in the ivar name here because it's obvious
    // that it's a string from the property name.
    internal let userFirstName: String
    
    // When working with a view controller, table view
    // controller, collection view controller, split view controller, etc.,
    // fully indicate the type in the name.
    let popupTableViewController: UITableViewController
    
    // When working with outlets, make sure to specify the outlet type in the
    // property name.
    @IBOutlet weak var submitButton: UIButton!
}

// NOT PREFERRED
internal final class UserViewController: UIViewController {

    // This isn't a `UIImage`, so shouldn't be called image
    // use `userAvatarImageView` instead.
    let userAvatarImage: UIImageView

    // This isn't a `String`, so it should be `textLabel`.
    let text: UILabel

    // `animation` is not clearly a time interval
    // use `animationDuration` or `animationTimeInterval` instead.
    let animation: TimeInterval

    // This is not obviously a `String`
    // use `transitionText` or `transitionString` instead.
    let transition: String

    // This is a view controller - not a view.
    let popupView: UIViewController

    // As mentioned previously, we don't want to use abbreviations, so don't use
    // `VC` instead of `ViewController`.
    let popupVC: UIViewController

    // Even though this is still technically a `UIViewController`, this property
    // should indicate that we are working with a *Table* View Controller.
    let popupViewController: UITableViewController

    // For the sake of consistency, we should put the type name at the end of the
    // property name and not at the start.
    @IBOutlet weak var buttonSubmit: UIButton!

    // We should always have a type in the property name when dealing with outlets
    // for example, here, we should have `firstNameLabel` instead.
    @IBOutlet weak var firstName: UILabel!
}
```
* **2.10** Name your function with words that describe its behavior.

```swift
// PREFERRED
internal func remove(at index: Index) -> Element { /* ... */ }

// NOT PREFERRED
internal func remove(index: Index) -> Element { /* ... */ }
```

* **2.11** When naming function arguments, make sure that the function can be read easily to understand the purpose of each argument.

* **2.12** As per [Apple's API Design Guidelines](https://swift.org/documentation/api-design-guidelines/), a `protocol` should be named as nouns if they describe what something is doing (e.g. `Collection`) and using the suffixes `able`, `ible`, or `ing` if it describes a capability (e.g. `Equatable`, `ProgressReporting`). If neither of those options makes sense for your use case, you can add a `Protocol` suffix to the protocol's name as well. Some example `protocol`s are below.

```swift
// Here, the name is a noun that describes what the protocol does.
internal protocol Unique {

    var uuid: UUID { get }
    
    /* ... */
}

// Here, the protocol is a capability, and we name it appropriately.
internal protocol JSONEncodable {

    func toJSONEncoded() -> [String: Any]

    /* ... */
}

// Suppose we have an `InputTextView` class, but we also want a protocol
// to generalize some of the functionality - it might be appropriate to
// use the `Protocol` suffix here.
internal protocol InputTextViewProtocol {

    var inputText: String { get }

    /* ... */
}
```

## 3. Coding Style

### 3.1 General

* **3.1.1** Prefer `let` to `var` whenever possible.

* **3.1.2** Prefer the composition of `map`, `filter`, `reduce`, etc. over iterating when transforming from one collection to another. Make sure to avoid using closures that have side effects when using these methods.

```swift
// PREFERRED
let stringOfInts = [1, 2, 3].flatMap({

    return String($0)
})
// ["1", "2", "3"]

// NOT PREFERRED
var stringOfInts: [String] = []
for integer in [1, 2, 3] {

    stringOfInts.append(String(integer))
}

// PREFERRED
let evenNumbers = [4, 8, 15, 16, 23, 42].filter({

    return $0 % 2 == 0
})
// [4, 8, 16, 42]

// NOT PREFERRED
var evenNumbers: [Int] = []
for integer in [4, 8, 15, 16, 23, 42] {

    if integer % 2 == 0 {

        evenNumbers.append(integer)
    }
}
```

* **3.1.3** Prefer not declaring types for constants or variables if they can be inferred anyway.

* **3.1.4** If a function returns multiple values, prefer returning a tuple to using `inout` arguments (it’s best to use labeled tuples for clarity on what you’re returning if it is not otherwise obvious). If you use a certain tuple more than once, consider using a `typealias`. If you’re returning 3 or more items in a tuple, consider using a `struct` or `class` instead.

```swift
internal var memberName: (firstName: String, lastName: String) {

    return ("Ruslan", "Skorb")
}

let memberName = self.memberName
let firstName = memberName.firstName
let lastName = memberName.lastName
```

* **3.1.5** Be wary of retain cycles when creating delegates/protocols for your classes; typically, these properties should be declared `weak`.

* **3.1.6** Be careful when calling `self` directly from an escaping closure as this can cause a retain cycle - use a [capture list](https://developer.apple.com/library/ios/documentation/swift/conceptual/Swift_Programming_Language/Closures.html#//apple_ref/doc/uid/TP40014097-CH11-XID_163) when this might be the case:

```swift
someFunctionWithEscapingClosure() { [weak self] (error) in

    // you can do this

    self?.doSomething()

    // or you can do this

    guard let strongSelf = self else {

        return
    }

    strongSelf.doSomething()
}
```

* **3.1.7** Don't use labeled breaks.

* **3.1.8** Don't place parentheses around control flow predicates.

```swift
// PREFERRED
if x == y {

    /* ... */
}

// NOT PREFERRED
if (x == y) {

    /* ... */
}
```

* **3.1.9** Avoid writing out an `enum` type where possible - use shorthand.

```swift
// PREFERRED
imageView.setImageWithURL(url, type: .person)

// NOT PREFERRED
imageView.setImageWithURL(url, type: AsyncImageView.Type.person)
```

* **3.1.10** When writing methods, keep in mind whether the method is intended to be overridden or not. If not, mark it as `final`, though keep in mind that this will prevent the method from being overwritten for testing purposes. In general, `final` methods result in improved compilation times, so it is good to use this when applicable. Be particularly careful, however, when applying the `final` keyword in a library since it is non-trivial to change something to be non-`final` in a library as opposed to have changing something to be non-`final` in your local project.

* **3.1.11** When using a statement such as `else`, `catch`, etc. that follows a block, put this keyword on a new line. Again, we are following the [Stroustrup style](https://en.m.wikipedia.org/wiki/Indentation_style#Variant:_Stroustrup) here. Example `if`/`else` and `do`/`catch` code is below.

```swift
if someBoolean {

    // do something
}
else {

    // do something else
}

do {

    let fileContents = try readFile("filename.txt")
}
catch {

    print(error)
}
```

* **3.1.12** Prefer `static` to `class` when declaring a function or property that is associated with a class as opposed to an instance of that class. Only use `class` if you specifically need the functionality of overriding that function or property in a subclass, though consider using a `protocol` to achieve this instead.

* **3.1.13** If you have a function that takes no arguments, has no side effects, and returns some object or value, prefer using a computed property instead.

### 3.2 Access Modifiers

* **3.2.1** Write the access modifier keyword first.

```swift
// PREFERRED
private static let somePrivateNumber: Int

// NOT PREFERRED
static private let somePrivateNumber: Int
```

* **3.2.2** The access modifier keyword should not be on a line by itself - keep it inline with what it is describing.

```swift
// PREFERRED
open class Member {

    /* ... */
}

// NOT PREFERRED
open
class Member {

    /* ... */
}
```

* **3.2.3** If a property needs to be accessed by unit tests, you will have to make it `internal` to use `@testable import ModuleName`. If a property *should* be private, but you declare it to be `internal` for the purposes of unit testing, make sure you add an appropriate bit of documentation commenting that explains this. You can make use of the `- warning:` markup syntax for clarity as shown below.

```swift
///
/// This property defines the company's name.
/// - warning: Not `private` for `@testable`.
///
internal let companyName = "R.SK Lab"
```

* **3.2.4** Prefer `private` to `fileprivate` where possible.

* **3.2.5** When choosing between `public` and `open`, prefer `open` if you intend for something to be subclassable outside of a given module and `public` otherwise. Note that anything `internal` and above can be subclassed in tests by using `@testable import`, so this shouldn't be a reason to use `open`. In general, lean towards being a bit more liberal with using `open` when it comes to libraries, but a bit more conservative when it comes to modules in a codebase such as an app where it is easy to change things in multiple modules simultaneously.

### 3.3 Custom Operators

Prefer creating named functions to custom operators.

If you want to introduce a custom operator, make sure that you have a *very* good reason why you want to introduce a new operator into global scope as opposed to using some other construct.

You can override existing operators to support new types (especially `==`). However, your new definitions must preserve the semantics of the operator. For example, `==` must always test equality and return a boolean.

### 3.4 Switch Statements and `enum`s

* **3.4.1** Since `switch` cases in Swift break by default, do not include the `break` keyword if it is not needed.

* **3.4.2** The `case` statements should line up with the `switch` statement itself as per default Swift stan2dards.

* **3.4.3** When defining a case that has an associated value, make sure that this value is appropriately labeled as opposed to just types (e.g. `case resetPassword(email: String)` instead of `case resetPassword(String)`).

```swift
internal enum API {

    case resetPassword(email: String)
}

internal var parameters: [String: Any]? {

    switch self {

    case .resetPassword(let email):
        return [

            "email": email
        ]
    }
}
```

* **3.4.4** Prefer lists of possibilities (e.g. `case 1, 2, 3:`) to using the `fallthrough` keyword where possible.

* **3.4.5** If you have a default case that shouldn't be reached, preferably throw an error (or handle it in some other similar way such as asserting).

```swift
internal func handleLevel(_ level: Int) throws {

    switch digit {

    case 0, 1, 2, 3, 4, 5, 6, 7, 8, 9:
        print("Level: \(level)")

    default:
        throw Error(message: "The given level is invalid.")
    }
}
```

### 3.5 Optionals

* **3.5.1** The only time you should be using implicitly unwrapped optionals is with `@IBOutlet`s. In every other case, it is better to use a non-optional or regular optional property. Yes, there are cases in which you can probably "guarantee" that the property will never be `nil` when used, but it is better to be safe and consistent. Similarly, don't use force unwraps.

* **3.5.2** Don't use `as!` or `try!`.

* **3.5.3** If you don't plan on actually using the value stored in an optional, but need to determine whether or not this value is `nil`, explicitly check this value against `nil` as opposed to using `if let` syntax.

```swift
// PREFERERED
if someOptional != nil {

    // do something
}

// NOT PREFERRED
if let _ = someOptional {

    // do something
}
```

* **3.5.4** Don't use `unowned`. You can think of `unowned` as somewhat of an equivalent of a `weak` property that is implicitly unwrapped (though `unowned` has slight performance improvements on account of completely ignoring reference counting). Since we don't ever want to have implicit unwraps, we similarly don't want `unowned` properties.

```swift
// PREFERRED
internal weak var window: UIWindow?

// NOT PREFERRED
internal unowned var window: UIWindow

// NOT PREFERRED
internal weak var window: UIWindow!
```

* **3.5.5** When unwrapping optionals, use the same name for the unwrapped constant or variable where appropriate.

```swift
guard let someValue = someValue else {

    return
}
```

### 3.6 Protocols

When implementing protocols, there are two ways of organizing your code:

1. Using `// MARK:` comments to separate your protocol implementation from the rest of your code.
2. Using an extension outside your `class`/`struct` implementation code, but in the same source file.

Keep in mind that when using an extension, however, the methods in the extension can't be overridden by a subclass, which can make testing difficult. If this is a common use case, it might be better to stick with method #1 for consistency. Otherwise, method #2 allows for cleaner separation of concerns.

### 3.7 Properties

* **3.7.1** If making a read-only, computed property, provide the getter without the `get {}` around it.

```swift
internal var isHiddenMembersView: Bool {

    if someBool == true {

        return true
    }
    return false
}
```

* **3.7.2** When using `get {}`, `set {}`, `willSet`, and `didSet`, indent these blocks.
* **3.7.3** Though you can create a custom name for the new or old value for `willSet`/`didSet` and `set`, use the standard `newValue`/`oldValue` identifiers that are provided by default.

```swift
internal var level = .senior {

    willSet {

        print("will set to \(newValue)")
    }

    didSet {

        print("did set from \(oldValue) to \(self.level)")
    }
}

internal var password: String? {

    get {

        return self.passwordTextField.text
    }

    set {

        self.passwordTextField.text = password
    }
}
```

* **3.7.4** You can declare a singleton property as follows:

```swift
internal final class PhotoLibrary {

    internal static let shared = PhotoLibrary()

    /* ... */
}
```

### 3.8 Closures

* **3.8.1** If the types of the parameters are obvious, it is OK to omit the type name, but being explicit is also OK. Sometimes readability is enhanced by adding clarifying detail and sometimes by taking repetitive parts away - use your best judgment and be consistent.

```swift
// Omitting the type.
doSomethingWithClosure() { (response) in

    print(response)
}

// Explicit type.
doSomethingWithClosure() { (response: NSURLResponse) in

    print(response)
}

// Using shorthand in a map statement.
[1, 2, 3].flatMap({

    return String($0)
})
```

* **3.8.2** If specifying a closure as a type, wrap it in parentheses. Always wrap the arguments in the closure in a set of parentheses - use `()` to indicate no arguments and that nothing is returned.

```swift
let completionBlock: ((Bool) -> ()) = { (success) in

    print("Success? \(success)")
}

let completionBlock: (() -> ()) = {

    print("Completed!")
}

let completionBlock: (() -> ())? = nil
```

* **3.8.3** Keep parameter names on same line as the opening brace for closures.

* **3.8.4** Don't use trailing closure syntax because the meaning of the closure is not always obvious without the parameter name.

```swift
// PREFERRED
updateUsername(username, onComplete: { (error) in

    print(error)
})

// NOT PREFERRED
updateUsername(username) { (error) in

    // `onComplete` or `onFailure`?

    print(error)
}
```

### 3.9 Arrays

* **3.9.1** Prefer using a `for item in items` syntax when possible as opposed to something like `for i in 0 ..< items.count`. If you need to access an array subscript directly, make sure to do proper bounds checking. You can use `for (index, value) in items.enumerated()` to get both the index and the value.

* **3.9.2** Never use the `+=` or `+` operator to append/concatenate to arrays. Instead, use `.append()` or `.append(contentsOf:)` as these are far more performant (at least with respect to compilation) in Swift's current state. If you are declaring an array that is based on other arrays and want to keep it immutable, instead of `let myNewArray = arr1 + arr2`, use `let myNewArray = [arr1, arr2].joined()`.

### 3.10 Error Handling

Suppose a function `someFunction` is supposed to return a `String`, however, at some point it can run into an error. A common approach is to have this function return an optional `String?` where we return `nil` if something went wrong.

Example:

```swift
internal func readFile(named filename: String) -> String? {

    guard let file = openFile(named: filename) else {

        return nil
    }

    let fileContents = file.read()
    file.close()

    return fileContents
}

internal func printSomeFile() {

    let filename = "some_file.txt"

    guard let fileContents = readFile(named: filename) else {

        print("Unable to open file \(filename).")
        return
    }
    print(fileContents)
}
```

Instead, we should be using Swift's `try`/`catch` behavior when it is appropriate to know the reason for the failure.

You can use a `struct` such as the following:

```swift
internal struct InternalError: Error {

    // MARK: - Internal Properties

    internal let file: StaticString

    internal let function: StaticString

    internal let line: UInt

    internal let message: String

    // MARK: - Lifecycle

    internal init(message: String, file: StaticString = #file, function: StaticString = #function, line: UInt = #line) {

        self.file = file
        self.function = function
        self.line = line
        self.message = message
    }
}
```

Example usage:

```swift
internal func readFile(named filename: String) throws -> String {

    guard let file = openFile(named: filename) else {

        throw Error(message: "Unable to open file named \(filename).")
    }

    let fileContents = file.read()
    file.close()

    return fileContents
}

internal func printSomeFile() {

    do {

        let fileContents = try readFile(named: "some_file.txt")
        print(fileContents)
    }
    catch {

        print(error)
    }
}
```

### 3.11 Using `guard` Statements

* **3.11.1** In general, we prefer to use an "early return" strategy where applicable as opposed to nesting code in `if` statements. Using `guard` statements for this use-case is often helpful and can improve the readability of the code.

```swift
// PREFERRED
internal func user(at index: Int) -> User? {

    guard index >= 0 && index < self.users.count else {

        // return early because the index is out of bounds
        return nil
    }

    let user = self.users[index]
    return user
}

// NOT PREFERRED
internal func user(at index: Int) -> User? {

    if index >= 0 && index < self.users.count {

        let user = self.users[index]
        return user
    }
    return nil
}
```

* **3.11.2** When unwrapping optionals, prefer `guard` statements as opposed to `if` statements to decrease the amount of nested indentation in your code.

```swift
// PREFERRED
guard let viewModel = self.viewModel else {

    return
}
self.imageView.image = viewModel.imageViewImage

// NOT PREFERRED
if let viewModel = self.viewModel {

    self.imageView.image = viewModel.imageViewImage
}

// EVEN LESS PREFERRED
if self.viewModel == nil {

    return
}
self.imageView.image = self.viewModel!.imageViewImage
```

* **3.11.3** When deciding between using an `if` statement or a `guard` statement when unwrapping optionals is *not* involved, the most important thing to keep in mind is the readability of the code. There are many possible cases here, such as depending on two different booleans, a complicated logical statement involving multiple comparisons, etc., so in general, use your best judgement to write code that is readable and consistent. If you are unsure whether `guard` or `if` is more readable or they seem equally readable, prefer using `guard`.

```swift
// An `if` statement is readable here.
if isOperationFailed == true {

    return
}

// A `guard` statement is readable here.
guard isOperationSuccessful == true else {

    return
}

// Double negative logic like this can get hard to read - i.e. don't do this.
guard isOperationFailed != false else {

    return
}
```

* **3.11.4** If choosing between two different states, it makes more sense to use an `if` statement as opposed to a `guard` statement.

```swift
// PREFERRED
if isMemberUser == true {

    print("Glad to see you again, \(user.firstName)")
}
else {

    print("Would you like to be a team member?")
}

// NOT PREFERRED
guard isMemberUser == true else {

    print("Would you like to be a team member?")
    return
}

print("Glad to see you again, \(user.firstName)")
```

* **3.11.5** You should also use `guard` only if a failure should result in exiting the current context. Below is an example in which it makes more sense to use two `if` statements instead of using two `guard`s - we have two unrelated conditions that should not block one another.

```swift
if let firstValue = firstValue {

    print("\(firstValue)")
}

if let secondValue = secondValue, secondValue > 10 {

    print("\(secondValue)")
}
```

* **3.11.6** Often, we can run into a situation in which we need to unwrap multiple optionals using `guard` statements. In general, combine unwraps into a single `guard` statement if handling the failure of each unwrap is identical (e.g. just a `return`, `break`, `continue`, `throw`, or some other `@noescape`).

```swift
// Combined because we just return.
guard let firstValue = firstValue,
    let secondValue = secondValue,
    let thirdValue = thirdValue else {

    return
}

// Separate statements because we handle a specific error in each case.
guard let firstValue = firstValue else {

    throw Error(message: "Unwrapping firstValue failed.")
}

guard let secondValue = secondValue else {

    throw Error(message: "Unwrapping secondValue failed.")
}

guard let thirdValue = thirdValue else {

    throw Error(message: "Unwrapping thirdValue failed.")
}
```

## 4. Dependencies/File Structure

### 4.1 Import Statements

Import statements should be at the very top of the code file, and they should be listed in alphabetical order.

```swift
import CoreStore
import Foundation
import Moya
import RxSwift
import UIKit
```

The exception is for imports that are for testing only; they should be placed at the bottom of the list, in alphabetical order.

```swift
import Moya
import UIKit
@testable import SomeLibrary
```

### 4.2 File Structure

The following list should be the standard organization of all your Swift files, in this specific order:

Before the type declaration:

```swift
private class

private struct

private enum
```

Inside the type declaration:

```swift
// MARK: - <#Accessibility#> Views

// MARK: - <#Accessibility#> Superclass Properties

// MARK: - <#Accessibility#> Properties

// MARK: - <#Accessibility#> Static Properties

// MARK: - <#Accessibility#> Class Properties

// MARK: - <#Accessibility#> Static API

// MARK: - <#Accessibility#> Class API

// MARK: - <#Accessibility#> Lifecycle

// MARK: - <#Accessibility#> Actions

// MARK: - <#Accessibility#> API

// MARK: - <#Name#>DataSource

// MARK: - <#Name#>Delegate
```

After the type declaration:

```swift
<#accessibility#> extension
```

Each section above should be organized by accessibility:

```swift
open

public

internal

fileprivate

private
```

## 5. Documentation/Comments

### 5.1 Documentation

If a function is more complicated than a simple O(1) operation, you should generally consider adding a doc comment for the function since there could be some information that the method signature does not make immediately obvious. If there are any quirks to the way that something was implemented, whether technically interesting, tricky, not obvious, etc., this should be documented. Documentation should be added for complex classes/structs/enums/protocols and properties. All `public` functions/classes/properties/constants/structs/enums/protocols/etc. should be documented as well (provided, again, that their signature/name does not make their meaning/functionality immediately obvious).

After writing a doc comment, you should option click the function/property/class/etc. to make sure that everything is formatted correctly.

Be sure to check out the full set of features available in Swift's comment markup [described in Apple's Documentation](https://developer.apple.com/library/tvos/documentation/Xcode/Reference/xcode_markup_formatting_ref/Attention.html#//apple_ref/doc/uid/TP40016497-CH29-SW1).

Guidelines:

* **5.1.1** Use a triple slash for doc comments (`///`).

* **5.1.2** Add a new line before and after the doc comment that takes up more than one line.

* **5.1.3** Use the new `- parameter` syntax as opposed to the old `:param:` syntax (make sure to use lower case `parameter` and not `Parameter`). Option-click on a method you wrote to make sure the quick help looks correct.

```swift
internal final class User {

    ///
    /// This method feeds a certain food to a person.
    ///
    /// - parameter user: The user you want to check.
    /// - parameter team: The team to which the user, probably, belongs.
    /// - returns: `true` if the user is a member of the team; `false` otherwise.
    ///
    internal func isMember(_ user: User, of team: Team) -> Bool {

        // ...
    }
}
```

* **5.1.4** If you’re going to be documenting the parameters/returns/throws of a method, document all of them, even if some of the documentation ends up being somewhat repetitive (this is preferable to having the documentation look incomplete). Sometimes, if only a single parameter warrants documentation, it might be better to just mention it in the description instead.

* **5.1.5** For complicated classes, describe the usage of the class with some potential examples as seems appropriate. Remember that markdown syntax is valid in Swift's comment docs. Newlines, lists, etc. are therefore appropriate.

```swift
///
/// ## Feature Support
///
/// This class does some awesome things. It supports:
///
/// - Feature 1
/// - Feature 2
/// - Feature 3
///
/// ## Examples
///
/// Here is an example use case indented by four spaces because that indicates a
/// code block:
///
///     let someAwesomeThing = SomeAwesomeClass()
///     someAwesomeThing.makeSomethingAwesome()
///
/// ## Warnings
///
/// There are some things you should be careful of:
///
/// 1. Thing one
/// 2. Thing two
/// 3. Thing three
///
internal final class SomeAwesomeClass {

    /* ... */
}
```

* **5.1.6** When mentioning code, use code ticks - \`

```swift
///
/// This does something with a `UIViewController`, perchance.
/// - warning: Make sure that `someValue` is `true` before running this function.
///
internal func someFunction() {

    /* ... */
}
```

* **5.1.7** When writing doc comments, prefer brevity where possible.

### 5.2 Other Commenting Guidelines

* **5.2.1** Always leave a space after `//`.
* **5.2.2** Always leave comments on their own line.
* **5.2.3** When using `// MARK: - Whatever`, leave a newline after the comment.

```swift
internal final class User {

    // MARK: - Internal Properties

    internal let name: String

    // MARK: - Lifecycle

    internal init(name: String) {

        /* ... */
    }
}
```
