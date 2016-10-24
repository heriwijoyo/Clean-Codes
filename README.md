# Clean Codes Discussion

Chapter #11: Systems
---

Separate Constructing a System from Using It
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
