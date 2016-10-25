# Clean Codes Discussion

## Chapter #11: Systems

## Separate Constructing a System from Using It
```swift
protocol NetworkService {
  func requestJSONForMethod(method: RequestMethod, URL: String, parameter: RequestParameter?) -> JSON
}

class AuthenticatedNetworkService: NetworkService {
  func requestJSONForMethod(method: RequestMethod, URL: String, parameter: RequestParameter?) -> JSON {

    self.updateHeaderWithToken()
    // detail implementation of network request
  }
}

class NetworkServiceFactory {
  private var authNetworkService: NetworkService?

  func authenticatedNetworkService() -> NetworkService? {
    if authNetworkService == nil {
	    authNetworkService = AuthenticatedNetworkService()
    }
    return authNetworkService
  }
}
```

Pros:
+ No overhead of construction unless the object is actually be used -> faster startup time
+ Never return nil

Cons:
+ Hard-coded dependency on `AuthenticatedNetworkService`
+ Mixed construction logic with normal runtime processing -> nil test & its block -> break SRP

Key:
+ Don't let a little convenient idioms break the modularity.


### Separation of Main
```swift
class HomeViewController: UIViewController {
  var viewModel: HomeViewModel?
  var networkServiceFactory: NetworkServiceFactory?

  convenience init() {
    self.init(nibName: "HomeViewController", bundle: nil)

    configureViewModel()
    configureBubbleViewFactory()
  }

  func configureViewModel() {
    viewModel = HomeViewModel()
  }
}
```
key: `main` function builds the object necessary for the system, then pass them to the application, which simply uses them. The application has no knowledge of the construction process.

### Factories
```swift
protocol BubbleViewFactory {
  func createBubbleView() -> UIView
}

class LargeBubbleViewFactory: BubbleViewFactory {
  func createBubbleView() -> UIView {
    // implementation of large bubble view factory
  }
}

func configureBubbleViewFactory() {
  largeBubbleViewFactory = LargeBubbleViewFactory()
}

// somewhere inside the application
...
let largeBubbleView = largeBubbleViewFactory.createBubbleView()
...
```
keys:
+ [Abstract Factory Pattern](https://sourcemaking.com/design_patterns/abstract_factory)
+ `main` will construct object of factory implementation
+ application will use the object without knowing about the construction process

### Dependency Injection
keys:
+ In the context of dependency management, an object should not take responsibility for instantiating dependencies itself.
+ [JNDI (Java Naming Directory Interface)](https://docs.oracle.com/cd/E19644-01/817-5449/djjndi.html) lookups are partial implementation of DI.
+ [Java Spring Frameworks](https://spring.io/) provides the best known DI container for java.

## Scaling Up
+ We should implement today's stories, then refactor and expand the system to implement new stories tomorrow.
+ System architecture can grow incrementally, so maintain the proper separation of concern.

### Cross-Cutting Concerns
+ Concerns like persistence tend to cut across the natural object boundaries of a domain.
+ Using persistence as an example, you would declare which objects and attributes should be persisted, then delegate the task to persistence frameworks.

### Java Proxies
Drawbacks:
+ Lot of code even for a simple case
+ Use byte-manipulation libraries
+ So, they make it hard to create clean code

### Pure Java AOP Frameworks
...

### AspectJ Aspect
...

### Test Drive the System Architecture
+ Decoupled architecture

### Optimize Decision Making
+ Modularity and separation of concerns make decentralized management and decision making possible.

### Use Standards Wisely, When They Add Demonstrable Value
...

### System Need Domain-Specific Languages
...
