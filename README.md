# Clean Codes Discussion

## Chapter #11: Systems

### Separate Constructing a System from Using It
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


#### Separation of Main
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

#### Factories
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

### Scaling
