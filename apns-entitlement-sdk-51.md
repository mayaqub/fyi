# Change to APNS entitlement handling in SDK 51

Prior to SDK 51, Expo prebuild was automatically adding the Apple push notification entitlement for all iOS apps built with the Expo SDK.

In response to customers who are not using notifications, and who did not want this entitlement added, we [modified the Expo prebuild code](https://github.com/expo/expo/pull/27924) so that the APNS entitlement is only added automatically if the app has the `expo-notifications` package installed.

Since the APNS entitlement is no longer always present, this can occasionally lead to warnings or errors when submitting an app to the App Store. This can happen if there is a mismatch between the capabilities information previously sent to Apple’s developer portal, and the entitlements file in the app. Example message:

*Your app appears to register with the Apple Push Notification service, but the app signature's entitlements do not include the "aps-environment" entitlement. If your app uses the Apple Push Notification service, make sure your App ID is enabled for Push Notification in the Provisioning Portal, and resubmit after signing your app with a Distribution provisioning profile that includes the "aps-environment" entitlement.*

A mismatch can also happen in a newly created app with SDK 51 if the app is created without `expo-notifications`, a provisioning profile is created, and then the `expo-notifications` package and notifications code is added later.

Expo’s recommendations:

- If you are adding `expo-notifications` to their app after having already generated a provisioning profile for the App Store, you will need to generate a new one:
  - Run `eas credentials` then choose `iOS` -> `Build Credentials` -> `Provisioning Profile: Delete one from your project`
  - The next time you run `eas submit`, you will be prompted to create a new profile.
- If you see this error and do not want notifications in your app, you should go to the Apple developer portal and delete the APNS capability from the app there, if it is present.

More information on how to work with Apple capabilities and entitlements can be found in our [documentation](https://docs.expo.dev/build-reference/ios-capabilities/).
