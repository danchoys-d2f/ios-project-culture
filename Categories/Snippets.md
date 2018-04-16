[< Back](../README.md)

# Snippets

On this page you will find useful snippets that will help you to build your code faster and keep it more consistent with the rest of the project.

## Table of contents

* [How to create a snippet](#how-to-create-a-snippet)
* [Snippets](#snippets)
  * [View controller marks](#view-controller-marks)
  * [View marks](#view-marks)
  * [Private declarations](#private-declarations)
  * [Segue identifiers](#segue-identifiers)
  * [Presenter declaration](#presenter-declaration)

## How to create a snippet

This process is perfectly described by [Mattt Thompson](https://github.com/mattt) on [this page](http://nshipster.com/xcode-snippets/).

## Snippets

Note that neither the snippet name nor the shortcut is something to follow strictly. It is important though to keep the code part the same.

### View controller marks

* Platform: All
* Language: Swift
* Completion Shortcut: `vcmark`
* Completion Scopes: Class Implementation

```swift
// MARK: - Public properties

// MARK: - Outlets

// MARK: - Private properties

// MARK: - View controller view's lifecycle

override func viewDidLoad() {
    super.viewDidLoad()
}

// MARK: - Navigation

// MARK: - Actions

// MARK: - Private API
```

### View marks

* Platform: All
* Language: Swift
* Completion Shortcut: `vmark`
* Completion Scopes: Class Implementation

```swift
// MARK: - Public properties

// MARK: - Outlets

// MARK: - Private properties

// MARK: - Public API

override func awakeFromNib() {
    super.awakeFromNib()
    updateView()
}

// MARK: - Navigation

// MARK: - Actions

// MARK: - Private API

private func updateView() {
}
```

### Private declarations

* Platform: All
* Language: Swift
* Completion Shortcut: `privdec`
* Completion Scopes: Top Level

```swift
// MARK: - Private declarations -
```

### Segue identifiers

* Platform: All
* Language: Swift
* Completion Shortcut: `segid`
* Completion Scopes: Top Level

```swift
private struct SegueIdentifiers {
  static let <#name#> = "<#identifier#>"
}
```

### Presenter declaration

* Platform: All
* Language: Swift
* Completion Shortcut: `presdec`
* Completion Scopes: Top Level

```swift
protocol <#Scene name#>View: BaseView {
}

class <#Scene name#>Presenter: BasePresenter {

    // MARK: - Public properties

    unowned let view: <#Scene name#>View

    // MARK: - Initialization

    init(view: <#Scene name#>View, navigator: Navigator) {
        self.view = view
        super.init(navigator)
    }

    // MARK: - Public API

    override func onDidLoad() {
        super.onDidLoad()
    }

}
```

## Credits

Ⓒ Devoteam Digital Factory 2018
