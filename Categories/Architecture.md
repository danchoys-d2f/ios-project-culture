[< Back](../README.md)

# Architecture

Here you will find the general application architecture described in more details. Once again, we are trying to reach a certain level of similarity with the approach taken by our team on Android, yet this is not the main goal.

This page is updated over time to reflect changes to the recommended approach.

## Table of contents

* [Pattern](#pattern)
  * [Model](#model)

## Pattern

![MVP pattern diagram](../Images/mvp.png)

In general, our project follow the __MVP__ pattern where the three letters stand for the following layers:

* __Model__ - contains the model objects, providers, data managers, DTOs, mapping code, etc.
* __View__ - contains the user interface and consumes the user actions.
* __Presenter__ - links the above two together by feeding data into the __View__ and performing actions in response to events, received from the __View__.

Router is an additional entity in this setup, responsible for performing navigation between different screens. This is completely optional and can be included into either the __View__ or __Presenter__.

**Important note**

>Different tutorials over the web put a big emphasis on so-called __testability__ and __reusability__ when talking about patterns. We believe that while both things are important, they are hard to achieve on practice with most of the presentation code being one-to-one coupled with the view code. This is why the entire reason why we are following a specific pattern is because we want to achieve a separation of concerns to make the code cleaner and easier to read.

### Model

![Model diagram](../Images/model.png)

The diagram above represents a sample structure of the __Model__. It is very important to understand that different apps require a different structure and there is no silver bullet here too. In a simple project the entire model may be represented by a single array, while in more complex apps it can be an entire hierarchy of objects, responsible for different areas.

In this article we are going to cover a more complex case where an app is talking to different __Data sources__, including Web Service (WS), Database (DB) or File System (FS).

Above all the data sources lie the __Data Providers__, that incapsulate the details about how to access or write the data. There shall be no universal interface for a provider, because the sources they are built on top may vary significantly and attempts to make one will turn into a terrible headache =].

The providers are consumed by the __Data Manager__, which encapsulates the data-related business logic. Data manager serves as an interface for the __Model__ and is used from the __Presenter__. In certain cases a data manager will talk to different data providers while executing a single action. It is most convenient to organize such communication with the use of [Functional Reactive Programming](https://en.wikipedia.org/wiki/Functional_reactive_programming). If your are new to reactive programming, it can be complicated to get it at first. We suggest walking through the articles referenced in [Helpful links](./HelpfulLinks.md).

One thing, not represented in the diagram is the __Entities__. These objects are the semantical bits your application operates on. They can represent real-life objects or people, documents, etc. There are no special constrains on their implementation, but usually they classes or structures that contain no business logic, but rather encapsulate a set of properties.

Most of the time it is not convenient to have a single data manager, taking care of the entire app's data flows, because such data managers become big and messy. Instead much better is to have multiple data managers, each of which responsible for its own area. The granularity depends on the app, yet as a start you may try having a data manager per entity type.

It is not a good idea to have the data providers create the stores themselves or to have the data managers instantiate the data providers. Instead they need to get such dependencies from the outside at the time of creation. This approach is called __Dependency Injection__ and is covered in more detail in the [corresponding section](#dependency-injection).

## Credits

Ⓒ Devoteam Digital Factory 2018
