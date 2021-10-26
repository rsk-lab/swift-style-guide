# Swift Style Guide

Make sure to read [Apple's API Design Guidelines](https://swift.org/documentation/api-design-guidelines/).

Specifics from these guidelines + additional remarks are mentioned below.

This guide was last updated for Swift 5 on October 26, 2021.

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
        - [3.10 Using `guard` Statements](#310-using-guard-statements)
    - [4. File Structure](#4-file-structure)
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
final class SomeObject {
    
    // MARK: - Functions
    
    func someFunction() {
        
        if let x = x {
            
            /* ... */
        }
        else if let y = y {
            
            /* ... */
        }
        else if let z = z {
            
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

* **1.7** Empty declarations should be written in empty braces `{}`. Make it clear that the declaration was meant to be empty and not just a missing `// TODO:`.

```swift
// PREFERRED
extension UIRectCorner: Hashable {}

// NOT PREFERRED
extension UIRectCorner: Hashable { }

// EVEN LESS PREFERRED
extension UIRectCorner: Hashable {
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
let communityMembers = ["Ruslan Skorb", /* ... */]
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
let appUserID = 1

// Prefer RSKLabLLC to RskLabLlc.
final public class RSKLabLLC {

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
    
    // MARK: - Open Attributes
    
    @IBInspectable open var cornerRadius: CGFloat {
        
        /* ... */
    }
}

// NOT PREFERRED
@IBDesignable open class IBDesignBtn: UIButton {

    // MARK: - Open Attributes
    
    @IBInspectable open var rad: CGFloat {
        
        /* ... */
    }
}
```

* **2.6** Include type information in constant or variable names when it is not obvious otherwise.

```swift
// PREFERRED
let createNoteBarButtonItem: UIBarButtonItem

// PREFERRED
// It is ok not to include `String` in the name of the property here
// because it's obvious from the property name that it's a string.
var searchTerm: String?

// NOT PREFERRED
// For the sake of consistency, we should put the type name at the end of the
// property name and not at the start.
let barButtonItemNotes: UIBarButtonItem

// NOT PREFERRED
// This is not a button, so the name should not end with `Button`,
// use `createNoteBarButtonItem` instead.
let createNoteButton: UIBarButtonItem

// NOT PREFERRED
// This is a label, so it should be `startLoggingLabel`.
let startLogging: Label

// NOT PREFERRED
// This is a table view controller - not a table view.
let notesTableView: NotesViewNotesTableViewController

// NOT PREFERRED
// As mentioned previously, we don't want to use abbreviations,
// so don't use `VC` instead of `ViewController`.
let notesTableVC: NotesViewNotesTableVC
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

* **2.8** Name a `protocol` using the suffix `Protocol`.

```swift
protocol ObjectProtocol {}

protocol RectProtocol: ObjectProtocol {
    
    // MARK: - Attributes
    
    var height: CGFloat { get set }
    
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
    
    ("Ruslan", "Skorb")
}

let communityMemberName = self.communityMemberName
let firstName = communityMemberName.firstName
let lastName = communityMemberName.lastName
```

* **3.1.5** Be wary of retain cycles when creating delegates for your classes; typically, these properties should be declared `weak`.

* **3.1.6** All instance properties and functions should be fully-qualified with `self`, including within closures.

* **3.1.7** Be careful when calling `self` directly from an escaping closure as this can cause a retain cycle - use a [capture list](https://developer.apple.com/library/ios/documentation/swift/conceptual/Swift_Programming_Language/Closures.html#//apple_ref/doc/uid/TP40014097-CH11-XID_163) when this might be the case:

```swift
someFunctionWithEscapingClosure { [weak self] in
    
    self?.doSomething()
}
```

```swift
someFunctionWithEscapingClosure { [unowned self] in
    
    self.doSomething()
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
if let appStoreReceiptURL = bundle.appStoreReceiptURL {
    
    let data: Data
    do {
        
        data = try Data(contentsOf: appStoreReceiptURL, options: .alwaysMapped)
    }
    catch {
        
        /* ... */
    }
    /* ... */
}
else {
    
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
open struct CommunityMember {
    
    /* ... */
}

// NOT PREFERRED
open
struct CommunityMember {
    
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

* **3.4.3** Prefer lists of possibilities (e.g. `case 1, 2, 3:`) to using the `fallthrough` keyword where possible.

* **3.4.4** If you have a default case that shouldn't be reached, preferably throw an error (or handle it in some other similar way such as asserting).

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
    fatalError("Unknown `type`")
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

1. Using `// MARK: -` comments to separate your protocol implementation from the rest of your code.
2. Using an extension outside your `class`/`struct` implementation code, but in the same source file.
3. Using an extension outside your `class`/`struct` implementation code in another source file.

### 3.7 Properties

* **3.7.1** If making a read-only, computed property, provide the getter without the `get {}` around it.

```swift
override var flipsHorizontallyInOppositeLayoutDirection: Bool {
    
    true
}
```

* **3.7.2** When using `get`, `set`, `willSet`, and `didSet`, indent these blocks.
* **3.7.3** Though you can create a custom name for the new or old value for `willSet`/`didSet` and `set`, use the standard `newValue`/`oldValue` identifiers that are provided by default.

```swift
private(set) var selectedViewController: UIViewController? {
    
    didSet {
        
        oldValue?.willMove(toParent: nil)
        oldValue?.view.removeFromSuperview()
        oldValue?.removeFromParent()
        
        /* ... */
    }
}

@objc dynamic var noteText: String? {
    
    get {
        
        self.noteTextController.noteText
    }
    set {
        
        self.noteTextController.noteText = newValue
    }
}
```

* **3.7.4** You can declare a singleton property as follows:

```swift
final class SomeObject {
    
    /* ... */
    
    // MARK: - Attributes
    
    static let shared = SomeObject()
    
    /* ... */
}
```

### 3.8 Closures

* **3.8.1** If the types of the parameters are obvious, it is OK to omit the type name, but being explicit is also OK. Sometimes readability is enhanced by adding clarifying detail and sometimes by taking repetitive parts away - use your best judgment and be consistent.

```swift
// Omitting the type.
doSomething { result in
    
    /* ... */
}

// Using shorthand in a map statement.
[1, 2, 3].flatMap {

    String($0)
}
```

* **3.8.2** Keep parameter names on same line as the opening brace for closures.

### 3.9 Arrays

* **3.9.1** Prefer using a `for item in items` syntax when possible as opposed to something like `for i in 0 ..< items.count`. If you need to access an array subscript directly, make sure to do proper bounds checking. You can use `for (index, value) in items.enumerated()` to get both the index and the value.

### 3.10 Using `guard` Statements

* **3.10.1** In general, we prefer to use an "early return" strategy where applicable as opposed to nesting code in `if` statements. Using `guard` statements for this use-case is often helpful and can improve the readability of the code.

```swift
// PREFERRED
func communityMember(at index: Int) -> CommunityMember? {
    
    guard index >= 0 && index < self.communityMembers.count else {
        
        // return early because the index is out of bounds
        return nil
    }
    return self.communityMembers[index]
}
```

## 4. File Structure

The following list should be the standard organization of all your Swift files:

Before the type declaration:

```swift
import CoreData.NSManagedObjectID
import MessageUI.MFMailComposeViewController
import RSKUIKit
```

Inside the type declaration:

```swift
// MARK: - <#Accessibility#> Attributes

// MARK: - Deinitializer

// MARK: - <#Accessibility#> Initializers

// MARK: - <#Accessibility#> Functions

// MARK: - <#Protocol#>
```

Each section above should be organized by accessibility:

```swift
open

public

internal (by default, do not need to be specified explicitly)

fileprivate

private
```

After the type declaration:

```swift
<#accessibility#> extension
```

Import statements, properties, initializers and functions inside `// MARK: - `, as well as protocols should be placed in alphabetical order.

## 5. Documentation/Comments

### 5.1 Documentation

All `public` functions/classes/properties/constants/structs/enums/protocols/etc. should be documented.

After writing a doc comment, you should option click the function/property/class/etc. to make sure that everything is formatted correctly.

Be sure to check out the full set of features available in Swift's comment markup [described in Apple's Documentation](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html#//apple_ref/doc/uid/TP40016497-CH2-SW1).

Guidelines:

* **5.1.1** Use a triple slash for doc comments (`///`).

* **5.1.2** Add a new line before and after the doc comment that takes up more than one line.

* **5.1.3** When mentioning code, use code ticks - \`

```swift
///
/// Initializes and returns a new object that conforms to `RectProtocol` with the specified `origin` and `size`.
///
/// - Parameters:
///     - origin: The coordinates of the origin of the rectangle.
///     - size: The size of the rectangle.
///
```

### 5.2 Other Commenting Guidelines

* **5.2.1** Always leave a space after `//`.
* **5.2.2** When using `// MARK: -`, leave a newline after it.

```swift
class Task {
    
    // MARK: - Attributes
    
    let attributesDispatchQueue: DispatchQueue
    
    /* ... */
    
    // MARK: - Initializers
    
    init(targetQueue: DispatchQueue, workDispatchQoS: DispatchQoS) {
        
        /* ... */
    }
}
```
