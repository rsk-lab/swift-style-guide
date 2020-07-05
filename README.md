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
* **1.3** Ensure that there is no trailing whitespace (Xcode->Preferences->Text Editing->Editing->Automatically trim trailing whitespace).
* **1.4** The opening brace should be followed by an empty line. Give breathing room when scanning for code.
* **1.5** Do not place opening braces on new lines - we use the [Stroustrup style](https://en.m.wikipedia.org/wiki/Indentation_style#Variant:_Stroustrup).

```swift
final class SomeClass {
    
    // MARK: - Functions
    
    func someFunction() {
    
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

* **1.6** Add one empty line between functions. Give breathing room between code blocks.

* **1.7** Empty declarations should be written in empty braces `{}`. Make it clear that the declaration was meant to be empty and not just a missing `TODO`.

```swift
// PREFERRED
extension InAppNotification: Equatable {}

// NOT PREFERRED
extension InAppNotification: Equatable { }

// EVEN LESS PREFERRED
extension InAppNotification: Equatable {
}
```

* **1.8** When writing a type for a property, constant, variable, a key for a dictionary, a function argument, a protocol conformance, or a superclass, don't add a space before the colon.

```swift
// Specifying type.
let communityViewController: CommunityViewController

// Dictionary syntax (note that we left-align as opposed to aligning colons).
let communityMembersDictionary: [String: Any] = [

    "Founder": "Ruslan Skorb",
    /* ... */
]

// Declaring a function.
func someFunction<T, U: SomeProtocol>(firstArgument: U, secondArgument: T) where T.RelatedType == U {

    /* ... */
}

// Calling a function.
someFunction(someArgument: "R.SK Lab")

// Superclasses.
final class CommunityViewController: UIViewController {

    /* ... */
}

// Protocols.
extension CommunityViewController: UICollectionViewDataSource {

    /* ... */
}
```

* **1.9** In general, there should be a space following a comma.

```swift
let communityMembersArray = ["Ruslan Skorb", /* ... */]
```

* **1.10** There should be a space before and after a binary operator such as `+`, `==`, or `->`. There should also not be a space after a `(` and before a `)`.

```swift
let colorRed = topColorRed + percent * (bottomColorRed - topColorRed)
```

## 2. Naming

* **2.1** Use `PascalCase` for type names (e.g. `struct`, `enum`, `class`, `typealias`, `associatedtype`, etc.).

* **2.2** Use `camelCase` (initial lowercase letter) for function, method, property, constant, variable, argument names, enum cases, etc.

* **2.3** When dealing with an acronym or other name that is usually written in all caps, actually use all caps in any names that use this in code. The exception is if this word is at the start of a name that needs to start with lowercase - in this case, use all lowercase for the acronym.

```swift
// "URL" is at the start of a constant name, so we use lowercase "url".
let urlSession = URLSession(configuration: .default)

// Prefer using ID to Id.
let userID = 1

// Prefer RSKLab to RskLab.
final class RSKLab {

    /* ... */
}
```

* **2.4** Names should be descriptive and unambiguous.

```swift
// PREFERRED
class IBDesignableButton: UIButton {
    
    /* ... */
}

// NOT PREFERRED
class CustomButton: UIButton {
    
    /* ... */
}
```

* **2.5** Do not abbreviate, use shortened names, or single letter names.

```swift
// PREFERRED
@IBDesignable open class IBDesignableButton: UIButton {
    
    // MARK: - Open Properties
    
    @IBInspectable open var cornerRadius: CGFloat {
        
        /* ... */
    }
}

// NOT PREFERRED
@IBDesignable open class IBDesignBtn: UIButton {

    // MARK: - Open Properties
    
    @IBInspectable open var r: CGFloat {
        
        /* ... */
    }
}
```

* **2.6** Include type information in constant or variable names when it is not obvious otherwise.

```swift
// PREFERRED
final class NotesViewController: UIViewController {

    // MARK: - Private Bar Button Items
    
    private let createNoteBarButtonItem: UIBarButtonItem
    
    // MARK: - Private View Controllers
    
    // When working with a view controller, table view controller,
    // collection view controller, split view controller, etc.,
    // fully indicate the type in the name.
    private let notesTableViewController: NotesViewNotesTableViewController
}

// PREFERRED
final class NotesViewNotesTableViewDataController {
    
    // MARK: - Properties
    
    // It is ok not to include string in the ivar name here because it's obvious
    // that it's a string from the property name.
    var normalizedTitleSearchTerm: String? {
        
        /* ... */
    }
}

// NOT PREFERRED
final class NotesViewController: UIViewController {
    
    // MARK: - Private Bar Button Items
    
    // This isn't a `UIButton`, so shouldn't be called button
    // use `createNoteBarButtonItem` instead.
    let createNoteButton: UIBarButtonItem
    
    // For the sake of consistency, we should put the type name at the end of the
    // property name and not at the start.
    private let barButtonItemNotes: UIBarButtonItem
    
    // MARK: - Private Views

    // This isn't a `String`, so it should be `startLoggingLabel`.
    private let startLogging: Label
    
    // MARK: - Private View Controllers
    
    // This is a table view controller - not a table view.
    private let notesTableView: NotesViewNotesTableViewController
    
    // As mentioned previously, we don't want to use abbreviations, so don't use
    // `VC` instead of `ViewController`.
    private let notesTableVC: NotesViewNotesTableVC
}
```
* **2.7** Name your function with words that describe its behavior.

```swift
// PREFERRED
func remove(at index: Index) -> Element {
    
    /* ... */
}

// NOT PREFERRED
func remove(index: Index) -> Element {
    
    /* ... */
}
```

* **2.8** As per [Apple's API Design Guidelines](https://swift.org/documentation/api-design-guidelines/), a `protocol` should be named as nouns if they describe what something is doing (e.g. `Collection`) and using the suffixes `able`, `ible`, or `ing` if it describes a capability (e.g. `Equatable`, `ProgressReporting`). If neither of those options makes sense for your use case, you can add a `Protocol` suffix to the protocol's name as well. Some example `protocol`s are below.

```swift
// Here, the name is a noun that describes what the protocol does.
protocol Unique {
    
    // MARK: - Properties
    
    var uuid: UUID { get }
    
    /* ... */
}

// Here, the protocol is a capability, and we name it appropriately.
protocol Reusable {
    
    // MARK: - Static Properties
    
    static var reuseIdentifier: String { get }
    
    /* ... */
}

// Suppose we have an `InputTextView` class, but we also want a protocol
// to generalize some of the functionality - it might be appropriate to
// use the `Protocol` suffix here.
protocol InputTextViewProtocol {
    
    // MARK: - Properties
    
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
let strings = [1, 2, 3].flatMap {
    
    String($0)
}
// ["1", "2", "3"]

// NOT PREFERRED
var strings: [String] = []
for integer in [1, 2, 3] {
    
    strings.append(String(integer))
}

// PREFERRED
let evenNumbers = [4, 8, 15, 16, 23, 42].filter {
    
    $0 % 2 == 0
}
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
var communityMemberName: (firstName: String, lastName: String) {
    
    return ("Ruslan", "Skorb")
}

let communityMemberName = self.communityMemberName
let firstName = communityMemberName.firstName
let lastName = communityMemberName.lastName
```

* **3.1.5** Be wary of retain cycles when creating delegates/protocols for your classes; typically, these properties should be declared `weak`.

* **3.1.6** All instance properties and functions should be fully-qualified with `self`, including within closures.

* **3.1.7** Be careful when calling `self` directly from an escaping closure as this can cause a retain cycle - use a [capture list](https://developer.apple.com/library/ios/documentation/swift/conceptual/Swift_Programming_Language/Closures.html#//apple_ref/doc/uid/TP40014097-CH11-XID_163) when this might be the case:

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
let cancelAlertAction = UIAlertAction(title: cancelAlertActionTitle, style: .cancel, handler: nil)

// NOT PREFERRED
let cancelAlertAction = UIAlertAction(title: cancelAlertActionTitle, style: UIAlertAction.Style.cancel, handler: nil)
```

* **3.1.10** When using a statement such as `else`, `catch`, etc. that follows a block, put this keyword on a new line. Again, we are following the [Stroustrup style](https://en.m.wikipedia.org/wiki/Indentation_style#Variant:_Stroustrup) here. Example `if`/`else` and `do`/`catch` code is below.

```swift
if managedObjectContext.hasChanges == true {
    
    /* ... */
}
else {
    
    /* ... */
}

let data: Data?
do {
    
    if let appStoreReceiptURL = bundle.appStoreReceiptURL {
        
        data = try Data(contentsOf: appStoreReceiptURL, options: .alwaysMapped)
    }
    else {
        
        data = nil
    }
}
catch {
    
    /* ... */
}
```

* **3.1.11** Prefer `static` to `class` when declaring a function or property that is associated with a class as opposed to an instance of that class. Only use `class` if you specifically need the functionality of overriding that function or property in a subclass, though consider using a `protocol` to achieve this instead.

* **3.1.12** If you have a function that takes no arguments, has no side effects, and returns some object or value, prefer using a computed property instead.

### 3.2 Access Modifiers

* **3.2.1** Write the access modifier keyword first.

```swift
// PREFERRED
private static let somePrivateInt: Int

// NOT PREFERRED
static private let somePrivateInt: Int
```

* **3.2.2** The access modifier keyword should not be on a line by itself - keep it inline with what it is describing.

```swift
// PREFERRED
open class CommunityMember {

    /* ... */
}

// NOT PREFERRED
open
class CommunityMember {

    /* ... */
}
```

### 3.3 Custom Operators

Prefer creating named functions to custom operators.

If you want to introduce a custom operator, make sure that you have a *very* good reason why you want to introduce a new operator into global scope as opposed to using some other construct.

You can override existing operators to support new types (especially `==`). However, your new definitions must preserve the semantics of the operator. For example, `==` must always test equality and return a boolean.

### 3.4 Switch Statements and `enum`s

* **3.4.1** Since `switch` cases in Swift break by default, do not include the `break` keyword if it is not needed.

* **3.4.2** The `case` statements should line up with the `switch` statement itself as per default Swift standards.

* **3.4.3** When defining a case that has an associated value, make sure that this value is appropriately labeled as opposed to just types (e.g. `case projectInquiries(email: String)` instead of `case projectInquiries(String)`).

```swift
enum ContactOption {

    case generalQuestions(email: String)
}

switch contactOption {

case let .generalQuestions(email):
    self.presentMailComposeViewController(email: email)
}
```

* **3.4.4** Prefer lists of possibilities (e.g. `case 1, 2, 3:`) to using the `fallthrough` keyword where possible.

* **3.4.5** If you have a default case that shouldn't be reached, preferably throw an error (or handle it in some other similar way such as asserting).

```swift
switch type {
    
case .delete:
    /* ... */
    
case .insert:
    /* ... */
    
case .move:
    /* ... */
    
case .update:
    /* ... */
    
@unknown default:
    fatalError("controller(_:didChange:at:for:newIndexPath:) - unsupported type")
}
```

### 3.5 Optionals

* **3.5.1** If you don't plan on actually using the value stored in an optional, but need to determine whether or not this value is `nil`, explicitly check this value against `nil` as opposed to using `if let` syntax.

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

### 3.6 Protocols

When implementing protocols, there are three ways to organize your code:

1. Using `// MARK:` comments to separate your protocol implementation from the rest of your code.
2. Using an extension outside your `class`/`struct` implementation code, but in the same source file.
3. Using an extension outside your `class`/`struct` implementation code in another source file.

### 3.7 Properties

* **3.7.1** If making a read-only, computed property, provide the getter without the `get {}` around it.

```swift
var isHiddenMembersView: Bool {

    if someBool == true {

        return true
    }
    return false
}
```

* **3.7.2** When using `get {}`, `set {}`, `willSet`, and `didSet`, indent these blocks.
* **3.7.3** Though you can create a custom name for the new or old value for `willSet`/`didSet` and `set`, use the standard `newValue`/`oldValue` identifiers that are provided by default.

```swift
var level = .senior {

    willSet {

        print("will set to \(newValue)")
    }

    didSet {

        print("did set from \(oldValue) to \(self.level)")
    }
}

var password: String? {

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
final class PhotoLibrary {

    static let shared = PhotoLibrary()

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
func readFile(named filename: String) -> String? {

    guard let file = openFile(named: filename) else {

        return nil
    }

    let fileContents = file.read()
    file.close()

    return fileContents
}

func printSomeFile() {

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
struct InternalError: Error {

    // MARK: - Properties

    let file: StaticString

    let function: StaticString

    let line: UInt

    let message: String

    // MARK: - Lifecycle

    init(message: String, file: StaticString = #file, function: StaticString = #function, line: UInt = #line) {

        self.file = file
        self.function = function
        self.line = line
        self.message = message
    }
}
```

Example usage:

```swift
func readFile(named filename: String) throws -> String {

    guard let file = openFile(named: filename) else {

        throw Error(message: "Unable to open file named \(filename).")
    }

    let fileContents = file.read()
    file.close()

    return fileContents
}

func printSomeFile() {

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
func user(at index: Int) -> User? {

    guard index >= 0 && index < self.users.count else {

        // return early because the index is out of bounds
        return nil
    }

    let user = self.users[index]
    return user
}

// NOT PREFERRED
func user(at index: Int) -> User? {

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

// MARK: - Lifecycle

// MARK: - Handling actions

// MARK: - <#Accessibility#> Superclass Functions

// MARK: - <#Accessibility#> Functions

// MARK: - <#Accessibility#> Static Functions

// MARK: - <#Accessibility#> Class Functions

// MARK: - <#Protocol#>
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

Protocols should be placed in alphabetical order.

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
final class User {

    ///
    /// This method feeds a certain food to a person.
    ///
    /// - parameter user: The user you want to check.
    /// - parameter team: The team to which the user, probably, belongs.
    /// - returns: `true` if the user is a member of the team; `false` otherwise.
    ///
    func isMember(_ user: User, of team: Team) -> Bool {

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
final class SomeAwesomeClass {

    /* ... */
}
```

* **5.1.6** When mentioning code, use code ticks - \`

```swift
///
/// This does something with a `UIViewController`, perchance.
/// - warning: Make sure that `someValue` is `true` before running this function.
///
func someFunction() {

    /* ... */
}
```

* **5.1.7** When writing doc comments, prefer brevity where possible.

### 5.2 Other Commenting Guidelines

* **5.2.1** Always leave a space after `//`.
* **5.2.2** Always leave comments on their own line.
* **5.2.3** When using `// MARK: - Whatever`, leave a newline after the comment.

```swift
final class User {

    // MARK: - Properties

    let name: String

    // MARK: - Lifecycle

    init(name: String) {

        /* ... */
    }
}
```
