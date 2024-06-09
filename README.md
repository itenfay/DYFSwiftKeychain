English Vision | [中文版](README-zh.md)

## DYFSwiftKeychain

This is used to store text and data in the Keychain([Objective-C Version](https://github.com/itenfay/DYFKeychain)). As you probably noticed Apple's keychain API is a bit verbose. This library was designed to provide shorter syntax for accomplishing a simple task: reading/writing text values for specified keys:

```Swift
let keychain = DYFSwiftKeychain()
keychain.set("User Account Passcode", forKey: "kUserAccPasscode")
let p = keychain.get("kUserAccPasscode")
```

The Keychain library includes the following features:

- Get, set and delete string, boolean and Data Keychain items.
- Specify item access security level.
- Synchronize items through iCloud.
- Share Keychain items with other apps.

[![License MIT](https://img.shields.io/badge/license-MIT-green.svg?style=flat)](LICENSE)&nbsp;
[![CocoaPods Version](http://img.shields.io/cocoapods/v/DYFSwiftKeychain.svg?style=flat)](http://cocoapods.org/pods/DYFSwiftKeychain)&nbsp;
![CocoaPods Platform](http://img.shields.io/cocoapods/p/DYFSwiftKeychain.svg?style=flat)&nbsp;


## Group (ID:614799921)

<div align=left>
&emsp; <img src="https://github.com/itenfay/DYFSwiftKeychain/raw/master/images/g614799921.jpg" width="30%" />
</div>


## Installation

Using [CocoaPods](https://cocoapods.org):

``` 
pod 'DYFSwiftKeychain'
```

Or

```
pod 'DYFSwiftKeychain', '~> 1.2.0'

Or manually add the files from the [SwiftKeychain](https://github.com/itenfay/DYFSwiftKeychain/tree/master/SwiftKeychain) directory.


## What's Keychain?

Keychain is a secure storage. You can store all kind of sensitive data in it: user passwords, credit card numbers, secret tokens etc. Once stored in Keychain this information is only available to your app, other apps can't see it. Besides that, operating system makes sure this information is kept and processed securely. For example, text stored in Keychain can not be extracted from iPhone backup or from its file system. Apple recommends storing only small amount of data in the Keychain. If you need to secure something big you can encrypt it manually, save to a file and store the key in the Keychain.


## Usage

Add `import DYFSwiftKeychain` to your source code.

#### String values

```Swift
let keychain = DYFSwiftKeychain()
keychain.set("User Account Passcode", forKey: "kUserAccPasscode")
let s = keychain.get("kUserAccPasscode")
```

#### Data values

```Swift
let keychain = DYFSwiftKeychain()
keychain.set(data, forKey: "kCommSecureCode")
let data = keychain.getData("kCommSecureCode")
```

#### Boolean values

```Swift
let keychain = DYFSwiftKeychain()
keychain.set(true, forKey: "kFirstInstalledAndLaunched")
let ret = keychain.getBool("kFirstInstalledAndLaunched")
```

#### Removing keys from Keychain

```Swift
let keychain = DYFSwiftKeychain()
let _ = keychain.delete("kFirstInstalledAndLaunched") // Remove single key.
let _ = keychain.clear() // Delete everything from app's Keychain. Does not work on macOS.
```


## Advanced options

#### Keychain item access

Use `withAccess` parameter to specify the security level of the keychain storage. By default the `.accessibleWhenUnlocked` option is used. It is one of the most restrictive options and provides good data protection.

```Swift
let keychain = DYFSwiftKeychain()
keychain.set("xxx", forKey:"Key1", withAccess: .accessibleWhenUnlocked)
```

You can use `.accessibleAfterFirstUnlock` if you need your app to access the keychain item while in the background. Note that it is less secure than the `.accessibleWhenUnlocked` option.

See the list of all available [access options](https://github.com/itenfay/DYFSwiftKeychain/blob/master/SwiftKeychain/DYFSwiftKeychain.swift).


#### Synchronizing keychain items with other devices

Set `synchronizable` property to `true` to enable keychain items synchronization across user's multiple devices. The synchronization will work for users who have the "Keychain" enabled in the iCloud settings on their devices.

Setting `synchronizable` property to `true` will add the item to other devices with the `set` method and obtain synchronizable items with the `get` command. Deleting a synchronizable item will remove it from all devices.

Note that you do not need to enable iCloud or Keychain Sharing capabilities in your app's target for this feature to work.

```Swift
// The first device
let keychain = DYFSwiftKeychain()
keychain.synchronizable = true
keychain.set("See you tomorrow!", forKey: "key12")

// The second device
let keychain = DYFSwiftKeychain()
keychain.synchronizable = true
let s = keychain.get("key12") // Returns "See you tomorrow!"
```

We could not get the Keychain synchronization work on macOS.


#### Sharing keychain items with other apps

In order to share keychain items between apps on the same device they need to have common *Keychain Groups* registered in *Capabilities > Keychain Sharing* settings. 

Use `accessGroup` property to access shared keychain items. In the following example we specify an access group "9ZU3R2F3D4.com.omg.myapp.KeychainGroup" that will be used to set, get and delete an item "key1".

```Swift
let keychain = DYFSwiftKeychain()
keychain.accessGroup = "9ZU3R2F3D4.com.omg.myapp.KeychainGroup" // Use your own access group.

keychain.set("hello world", forKey: "key1")
keychain.get("key1")
keychain.delete("key1")
keychain.clear()
```

*Note*: there is no way of sharing a keychain item between the watchOS 2.0 and its paired device: https://forums.developer.apple.com/thread/5938


#### Check if operation was successful

You can verify if `set`, `delete` and `clear` methods finished successfully by checking their return values. Those methods return `true` on success and `false` on error.

```Swift
let keychain = DYFSwiftKeychain()

if keychain.set("xxx", forKey: "key1") {
  // Keychain item is saved successfully
} else {
  // Report error
}
```

To get a specific failure reason use the `osStatus` property containing result code for the last operation. See [Keychain Result Codes](https://developer.apple.com/documentation/security/1542001-security_framework_result_codes).

```Swift
let keychain = DYFSwiftKeychain()

keychain.set("xxx", forKey: "key1")
if keychain.osStatus != errSecSuccess { /* Report error */ }
```


#### Returning data as reference

Use the `asReference: true` parameter to return the data as reference, which is needed for [NEVPNProtocol](https://developer.apple.com/documentation/networkextension/nevpnprotocol).

```Swift
let keychain = DYFSwiftKeychain()
keychain.set(data, forKey: "key1")
keychain.getData("key1", asReference: true)
```


## Using DYFSwiftKeychain from Objective-C

`DYFSwiftKeychain` supports to be used in Objective-C apps.


## Demo

`DYFSwiftKeychain` is learned how to use under this [Demo](https://github.com/itenfay/DYFStore/blob/master/Classes/DYFStoreKeychainPersistence.swift).


## Feedback is welcome

If you notice any issue, got stuck to create an issue. I will be happy to help you.
