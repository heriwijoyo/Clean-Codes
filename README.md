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
	    authNetworkService = AuthenticationNetworkService()
    }
    return authNetworkService
  }
}
```
