[< Back](../README.md)

# Style guide

This page tries to give a comprehensive description of the code culture that is reinforced in our projects along with some common mistakes frequently found when doing code review =]

## Table of contents

* [Basics](#basics)
* [Snippets](#snippets)
  * [View controller marks](#view-controller-marks)
  * [View marks](#view-marks)
  * [Private declarations](#private-declarations)
  * [Segue identifiers](#segue-identifiers)
  * [Presenter declaration](#presenter-declaration)

## Basics

In most cases it is more helpful to use some generally accepted approach, rather than re-inventing the wheel, and here it is also like this. Unless stated otherwise in this document, we follow the [style guide declared here](https://github.com/raywenderlich/swift-style-guide). This guide contains most of the important rules that improve the readability and consistence of code. In the following sections you will find things that are different from the Ray Wenderlich's guide.

### Code organization

Ray's team suggests to organize code with extensions and indeed it is a nice idea. Though it has a significant problem in it. Due to the [method dispatch mechanism](https://www.raizlabs.com/dev/2016/12/swift-method-dispatch/) used in Swift, at times it may not be possible to override methods, e.g. the the protocol conformance code, declared in an extension, in subclasses. To work around it, you will have to move such code back into the class declaration, thus making your code inconsistent.

Therefore suggested is usage of `MARK`s to organize your classes. In our projects we have the following default sections:

```swift
// MARK: - Public properties
// MARK: - Private properties
// MARK: - Initialization
// MARK: - Public API
// MARK: - Private API
```

Tips:

* If a property is declared as `private(set)`, it shall still go into the __Public properties__ section since it is visible outside the class.
* If you are overriding some public method in a subclass, it shall still remain in the __Public API__ section. Do not create an __Overrides__ section or similar.
* Always specify a restrictive access modifier when adding a new partly or completely private class member.
* If a type has public or private nested declarations, they shouldn't contain sections inside them and must appear at the beginning of their encapsulating type declaration above the __Public properties__ section with the public types going first.

Protocol conformance code goes after the __Private API__ section. Each such section must have its own `MARK` and have a sentence-like naming. There is no strict rule in which order the protocol conformance sections need to appear. Example:

```swift
// MARK: - Table view data source

override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return 1
}

// MARK: - My view delegate

func myViewDidRequestResetting(_ view: MyView) {
    data.reset()
}
```

Usually we declare some utility private types or extensions in the file that contains a public type declaration. For example, we may have a `struct` that contains segue identifiers for a view controller. We put such declarations below the main class declaration, separating the private part with a special mark:

```swift
// MARK: - Private declarations -
```

Note that this `MARK` has a trailing dash. This helps to visually separate this section when viewing the overview dropdown in Xcode:

[Xcode dropdown](../Images/dropdown.png)

Note that some standard types like `UIViewController` or `UIView` have some specific well-established sections. Refer to [Snippets](./Snippets.md) to find out more.


## Credits

Ⓒ Devoteam Digital Factory 2018
