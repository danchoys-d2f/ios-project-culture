# Project structure

As D2F does both iOS and Android, it's natural to have a similar structure between the two platforms, yet this structure must not intefere with the best platform's practices. This page describes the basic setup that is used in iOS projects.

## Table of contents

* [Naming](#naming)
* [Hierarchy](#hierarchy)
  * [Model](#group-hierarchy-model)
  * [Components](#group-hierarchy-components)
  * [Modules](#group-hierarchy-modules)
  * [Resources](#group-hierarchy-resources)
  * [Supporting Files](#group-hierarchy-supporting-files)
* [Sort order](#sort-order)

## Naming

To be consistent with the traditional naming convention of the platform, we name categories with capitalized words, separated by spaces.

Example:

```
// Preferred:
View Controllers
Proof Documents
Item Details
```

```
// Not preferred:
View_controllers
view_controllers
view-controllers
ViewControllers
01_view_controllers
```

## Hierarchy

Suggested is the following group hierarchy:

```
$(TARGET_NAME)/
-- AppDelegate.swift
-- Model/
-- Components/
-- -- Views/
-- -- View Controllers/
-- -- Protocols/
-- -- Extensions/
-- -- Utilities/
-- Modules/
-- Resources/
-- Supporting Files/
Products/
Pods/
Frameworks/
```

Lets walk over these groups and discuss some of them in a more detail.

#### <a id="group-hierarchy-model"></a>Model

This group refers to __M__ in the popular __MV*__ patterns like __MVC__, __MVP__ or __MVVM__. 

It contains the API client, server details, model objects, mapping code, providers and managers. What this group does not contain is anything related to UI. In other words, none of these files, contained in this category, should be importing __UIKit__ or dealing with data formatting.

It is a good approach to treat the entire __Model__ group as a self-contained framework, that is imported from the UI layer. Sometimes it can even be extracted as a separate dynamic framework target to be used, for example, in both iOS and tvOS targets.

Usually it is beneficial to add UI-related properties or helper methods to the model objects. You shouldn't do it though by adding your properties directly to the model object. You shall declare an extension and put it into __Components/Extensions/__ group instead. Example:

```swift
// User.swift
struct User {
    let firstName: String
    let lastName: String
}

// User+Extensions.swift
extension User {
    var fullName: String {
      return "\(firstName) \(lastName)"
    }
}
```

#### <a id="group-hierarchy-components"></a>Components

This group has all the reusable stuff, that is shared between the different modules. If you think that some views or view controllers may appear in different places of your app, consider putting them into __Components/Views/__ and __Components/View Controllers/__ respectively. Same applies to the other subgroups. If your new UI-related declaration does not fit into any of the existing categories, it should go into __Components/Utilities/__

#### <a id="group-hierarchy-modules"></a>Modules

Here is where your application user interfaces are declared and implemented. Depending on the size of the app, this can be organized differently; following is just an example of how a single module can look like in a mid-sized project containing 10-15 screens.

```
Modules/
-- Login/
-- -- Login.storyboard
-- -- Views/
-- -- -- UserNameTableViewCell.swift
-- -- -- UserPasswordTableViewCell.swift
-- -- LoginViewController.swift
-- -- LoginViewModel.swift
```

Here each module has a dedicated storyboard, at least one view controller and optionally a view subgroup, where local to the module views are declared.

#### <a id="group-hierarchy-resources"></a>Resources

Thats somewhat similar to `res` on Android. In this group you will find the image assets, custom fonts, localization files, custom configuration plists and so on.

#### <a id="group-hierarchy-supporting-files"></a>Supporting Files

Being more of a legacy, rather than direct need, this group contains the Info.plist, LaunchScreen.storyboard and sometimes the entitlements file. This is the only group that is not backed by a directory in the file system.

## Sort order

All groups, containing lists of homogeneous items should be sorted alphabetically. This includes, but is not limited by the following:

* All subgroups of the __Components/__ group
* __Modules/__
* __Resources/__
