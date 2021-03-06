# Official MySabay SDK for iOS

This is the official MySabay SDK for native iOS application. To use this SDK, you can follow the guides below or download the test with the example project we have in this repository.

## Create your application

Create your MySabay application if you don't have one yet at [MySabay App Dashboard](https://kh.mysabay.com:8443/index.html) and copy your `appId` 
and `appSecret` for the integration.

## Installing

The SDK is published on Cocoapods, so you can simply install into your project with pod or you can also download and manually install at [MySabay SDK](https://github.com/sabay-digital/app.ios.sdk.mysabay.com.public/raw/master/MySabaySdk.zip)

```bash
pod 'MySabaySdk'
```

## Configuration
Next, it is time to config your Xcode project to update Info.plist, AppDelegate and SceneDelegate.

**Info.plist**

> Add the following contents to your Info.plist file 

```xml
<key>MySabayAppId</key>
<string>{appId}</string>
<key>MySabayAppName</key>
<string>{appName}</string>
<key>MySabayAppSecret</key>
<string>{appSecret}</string>
```

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
	    <string>Viewer</string>
	    <key>CFBundleURLName</key>
	    <string>aog</string>
	    <key>CFBundleURLSchemes</key>
	    <array>
	        <string>mysabay{appId}</string>
	    </array>
	</dict>
</array>
```

> !!! Replace `{appId}`, `{appName}` and `{appSecret}` with your `appId`, `appName` and `appSecret`. 


**AppDelegate.swift**

```swift
import MySabaySdk

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    MSMySabayManager.shared.addTransactionObserver()
    return true
}
    
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
    return MSMySabayManager.shared.handleOpenUrl(url: url)
}
    
func applicationWillTerminate(_ application: UIApplication) {
    MSMySabayManager.shared.removeTransactionObserver()
}
```

**SceneDelegate.swift**

```swift
func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {
    MSMySabayManager.shared.handleOpenUrl(contexts: URLContexts)
}
```

## Integration

> Note that in order to use the store and checkout function, the user must login first. 
> Follow the guide below for each functions provided by the SDK:



*  **Login**

```swift
import MySabaySdk

MSMySabayManager.shared.logIn(fromController: self) { result in
    switch result {
        case .loginSuccess(let token):
            print("\(token.tokenString!)")
            break
        case .loginFailed(let error):
            print(error.localizedDescription)
            break
        }
    }
}
```


*  **Store and checkout**

```swift
import MySabaySdk

MSMySabayManager.shared.openStore(fromController: self) { result in
    switch result {
        case .purchaseMySabay(let hash):
            print("\(hash)")
            //some work after success with MySabay
            break
        case .purchaseApple(let product):
            print("\(product.productIdentifier)")
            //some work after success with Apple In-App purchase
            break
        case .purchaseFailed(let error):
            print("\(error.localizedDescription)")
            break
    }
}
```



*  **Get profile**

```swift
import MySabaySdk

MSMySabayManager.shared.getUserProfile { result in
    switch result {
        case .fetchSuccess(let profile):
            //work with profile
            break
        case .fetchFailed(let error):
            print("\(error.localizedDescription)")
            break
    }
}
```



*  **Refresh token**

```swift
import MySabaySdk

MSMySabayManager.shared.refreshToken { result in
    switch result {
        case .refreshSuccess(let token):
            //work with token
            break
        case .refreshFailed(let error):
            print("\(error.localizedDescription)")
            break
    }
}
```



*  **Get current token**

```swift
if let token = MSToken.currentToken {

}
```



*  **Check valid token**

```swift
if MSToken.isValid {

}
```


*  **Logout**

```swift
MSMySabayManager.shared.logOut()
```

