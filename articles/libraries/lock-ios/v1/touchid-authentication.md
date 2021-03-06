---
section: libraries
toc_title: Touch ID Authentication
description: How to implement Touch ID authentication with Lock iOS.
---
# Lock iOS: Touch ID Authentication

::: version-warning
This feature is disabled for new tenants as of June 8th 2017. Any tenant created after that date won't have the necessary legacy [grant types](/clients/client-grant-types) to use Touch ID. This document is offered as reference for older implementations. We recommend that you [upgrade to v2](/libraries/lock-ios/v2/migration).
:::

Lock provides passwordless authentication with TouchID for your Auth0 DB connection. To start authenticating your users with TouchID please follow those steps:

1. Add `TouchID` subspec module of **Lock** to your `Podfile`
  ```ruby
  pod 'Lock/TouchID'
  ```

1. Import **Lock**'s umbrella header
  ```objc
  #import <Lock/Lock.h>
  ```
  ::: note
  If your are coding in Swift, you need to import the header in your app's [Bridging Header](https://developer.apple.com/library/ios/documentation/swift/conceptual/buildingcocoaapps/MixandMatch.html).
  :::

1. Instantiate `A0TouchIDLockViewController` and register authentication callback
  ```objc
  A0TouchIDLockViewController *controller = [[A0TouchIDLockViewController alloc] init];
  controller.onAuthenticationBlock = ^(A0UserProfile *profile, A0Token *token) {
      //Store token & profile. For example in the keychain using SimpleKeychain.
      [self dismissViewControllerAnimated:YES completion:nil];
  };
  ```
  ```swift
  let lock = A0Lock.shared()
  let controller: A0TouchIDLockViewController = lock.newTouchIDViewController()
  controller.onAuthenticationBlock = { (profile, token) in
      // Do something with token & profile. e.g.: save them.
      // Lock will not save the Token and the profile for you.
      // And dismiss the UIViewController.
      self.dismiss(animated: true, completion: nil)
  }
  ```

1. Present `A0TouchIDLockViewController` as the root controller of a `UINavigationController`
  ```objc
  UINavigationController *navController = [[UINavigationController alloc] initWithRootViewController:controller];
  if (UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPad) {
      navController.modalPresentationStyle = UIModalPresentationFormSheet;
  }
  [self presentViewController:navController animated:YES completion:nil];
  ```
  ```swift
  let navController = UINavigationController(rootViewController: controller)
  if UIDevice.current.userInterfaceIdiom == .pad {
      navController.modalPresentationStyle = .FormSheet
  }
  self.presentViewController(navController, animated: true, completion:nil)
  ```
  ::: note
  It's mandatory to present `A0TouchIDLockViewController` embedded in a `UINavigationController`.
  :::

And you'll see TouchID login screen.

![Lock Screenshot](/media/articles/libraries/lock-ios/Lock-TouchID-Screenshot.png)
