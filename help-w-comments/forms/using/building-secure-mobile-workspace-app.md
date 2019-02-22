---
title: Building a secure AEM Forms app for iOS
seo-title: Building a secure AEM Forms app for iOS
description: Steps to build a secure AEM Forms app.
seo-description: Steps to build a secure AEM Forms app.
uuid: ee0175f0-72aa-4350-aa09-ea6a893b63dd
content-type: reference
products: SG_EXPERIENCEMANAGER/6.4/FORMS
topic-tags: forms-app
discoiquuid: 424f6662-044e-4194-870b-cf0a5c79c730
index: y
internal: n
snippet: y
---

# Building a secure AEM Forms app for iOS{#building-a-secure-aem-forms-app-for-ios}

You need to archive the Xcode project for AEM Forms app to build the installer (an .ipa file) and a property list (a .plist file) file. The property list file contains configuration information of the hosted in-house app, such as the name and the hosting location of the app. For more information about property list file, see [About Information Property List Files](http://developer.apple.com/library/ios/#documentation/general/Reference/InfoPlistKeyReference/Articles/AboutInformationPropertyListFiles.html).

1. Log in to the following website:
1. Create an App ID. For detailed steps to create an App ID, see [Creating and Configuring App IDs](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingProfiles/MaintainingProfiles.html).
1. To configure the bundle identifier for the iOS application for your app, click **Configure App ID**.
1. At the bottom of the web page, select **Enable for Data Protection**. Specify the data protection options.

   Click **Done**.

1. Navigate to Provisioning-&gt;Distribution and create a New Profile using the App ID configured in step 3.
1. Download and add the provisioning profile to the Xcode and the iPad. 
1. Log in to your Mac machine that has Xcode, and iOS SDK installed and configured.
1. Open the `AEM Forms.xcodeproj` project in Xcode.
1. Click **AEM Forms**, under **TARGETS**, select **AEM Forms**. Select the **Build Settings **tab, locate the **Code Signing Entitlement** section and in the Entitlements dropdown, select the **LC Enterprise** option.
1. Locate and open the `LC Enterprise.entitlements` file in the Xcode for editing. Under the **XCode entitlements, **add the same key-value pair as present in your provisioning profile. 
1. In the **Build Settings** tab, click **All** and then click **Combined**.
1. From the **Settings** list, expand **Code Signing**.
1. For **Code Signing Identity**, select the appropriate signature. Ensure that the same signature is selected for **Debug**, **Release**, and **Any iOS SDK**.
1. Under **PROJECT**, select **AEM Forms** and ensure that the appropriate signature is selected for **Code Signing Identity**, **Debug**, **Release** and **Any iOS SDK**. 
1. Build and Distribute AEM Forms app. For detailed instructions to build and distribute AEM Forms app, see [Build the installer for AEM Forms app](../../forms/using/setup-xcode-project-build-installer.md#main-pars-text-12).

[**Contact Support**](https://www.adobe.com/account/sign-in.supportportal.html)

<!--
<related-links>
<a href="../../forms/using/setup-environment-mobile-workspace.md" target="_blank">Set up your environment</a>
<a href="../../forms/using/setup-xcode-project-build-installer.md" target="_blank">Set up the Xcode project and build the iOS app</a>
<a href="../../forms/using/building-secure-mobile-workspace-app.md" target="_blank">Set up the Eclipse project and build the Android app</a>
<a href="../../forms/using/setup-visual-studio-project-build-installer.md" target="_blank">Set up the Visual Studio project and build the Windows app</a>
<a href="../../forms/using/distribute-mobile-workspace-app.md" target="_blank">Distribute the AEM Forms app</a>
<a href="../../forms/using/building-secure-mobile-workspace-app.md" target="_blank">Building a secure AEM Forms app for iOS</a>
</related-links>
-->
