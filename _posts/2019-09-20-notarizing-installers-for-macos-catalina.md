---
title: Notarizing installers for macOS Catalina
date: 2019-09-20
author: Davide Barranca
excerpt: "With macOS 10.15 Catalina, native installers such as .pkg or .dmg require to be 'notarized' with Apple. It's less scaring than it sounds, let's see how please the Gatekeeper."
layout: post
permalink: /2019/04/notarizing-installers-for-macos-catalina/
description: "With macOS 10.5 Catalina, native installers such as .pkg or .dmg require to be 'notarized' with Apple. It's less scaring than it sounds: let's see how to pass through Gatekeeper"
image: /wp-content/uploads/2019/05/gatekeeper.png
category:
  - HTML Panels tips
tags:
  - Notarizing
  - Apple
  - macOS Catalina
---

According to Apple (read [here](https://developer.apple.com/documentation/security/notarizing_your_app_before_distribution) the whole article):

> The Apple notary service is an automated system that scans your software for malicious content, checks for code-signing issues, and returns the results to you quickly. Notarization gives users more confidence that the Developer ID-signed software you distribute has been checked by Apple for malicious components. Notarization is not App Review. 

Basically, it is some kind of whitelisting. From macOS Catalina on (late September 2019), **notarization is mandatory**: your installers won't work if you don't do your homeworks. 

## Gatekeeper

When your customers run your product's `installer.pkg`, macOS Gatekeeper connects with Apple, checking whether the file you're executing has been vetted and thus can be run safely. If this is the case, Gatekeeper pops up the usual warning anyway: 

<figure>
<img src="/wp-content/uploads/2019/05/gatekeeper.gif" srcset="/wp-content/uploads/2019/05/gatekeeper.gif 1x, /wp-content/uploads/2019/05/gatekeeper@2x.gif 2x" alt="Book Design">
</figure>  

Because users can also be offline, you can _staple_ the notary service's response ticket directly to the installer – an optional but recommended step.

## What you need

1. An active subscription to the [Apple Developer Program](https://developer.apple.com/) ($99 per year).
2. Xcode 10 or newer (free download via the App Store).
3. An app-specific password defined for your AppleID (see [here](https://support.apple.com/en-us/HT204397) for instruction).

## Notarization walkthrough

I assume that you are at least vaguely familiar with the topic of code signing. I'm going to show you how to notarize a `.pkg` installer; caveats for slightly more complex scenarios (e.g. a `.dmg` that wraps a `.pkg` and/or `.app` plus other assets) will follow in a later section.

### 1. Create a DeveloperID Installer certificate

Log in the [Apple Developer portal](https://developer.apple.com) portal and follow the "Certificates, Identifiers & Profiles" section. Create a new Certificate: when asked, select DeveloperID Installer:

<figure>
<img src="/wp-content/uploads/2019/05/DeveloperID.png" srcset="/wp-content/uploads/2019/05/DeveloperID.png 1x, /wp-content/uploads/2019/05/DeveloperID@2x.png 2x" alt="Book Design">
</figure>  

Follow the instruction to complete the process – you'll be required to open the Certificate Assistant from the Keychain Access application to create a Certificate request, then upload it to the Developer Portal. When the process is complete, you'll be presented with a download link for your certificate, to be imported via double click into the Keychain Access app.

You can verify that the certificate has been successfully installed opening the Terminal and pasting the following command:

```
security find-identity -v

1) 6A7C3D1A28FA5483E5E8857F2A12EDC71731CAED "Developer ID Installer: Davide Barranca (I2WD7PWGD9)"
  1 valid identities found
```

Keep note of the string between quotes that says `"Developer ID Installer: ..."`.

### 2. Signing the Installer

You don't just need to _notarize_ an installer, it must be _signed_ first. Assuming that the `installer.pkg` file sits in the folder you have `cd` to in the Terminal window, you **sign the installer** with the following command (the output I've got comes right below the first line):

```
productsign --sign "Developer ID Installer: Davide Barranca" ./installer.pkg ./installer_signed.pkg

  productsign: using timestamp authority for signature
  productsign: signing product with identity "Developer ID Installer: Davide Barranca (I2WD7PWGD9)" from keychain /Library/Keychains/System.keychain
  productsign: adding certificate "Developer ID Certification Authority"
  productsign: adding certificate "Apple Root CA"
  productsign: Wrote signed product archive to ./installer_signed.pkg
```

Let's break down that:

- `productsign` is the utility in charge of the signing.
- `--sign` is the argument that defines the Identity associated to the Apple issued certificated, which follows as a string.
- `./installer.pkg` is the relative path (can be absolute as well) that points to the file you want to be signed.
- `./installer_signed.pkg` is the path of the new, signed file that is going to be created by `productsign`.

Of course you should substitute the `"Developer ID Installer:"` string with the one you've noted in the previous section, as well as the files paths.

You can verify that the `installer_signed.pkg` has been really signed with:

```
pkgutil --check-signature ./installer_signed.pkg

   Status: signed by a certificate trusted by Mac OS X
   Certificate Chain:
    1. Developer ID Installer: Davide Barranca (Y4PA7PZLM9)
       SHA1 fingerprint: 3A 7C 3D 1E 28 AA 54 83 E5 E8 11 7F 2A 12 ED C7 17
       --------------------------------------------------------------------    2. Developer ID Certification Authority
       SHA1 fingerprint: 3B 16 6C 3B CC C4 B7 11 C9 FE 22 FA B9 13 56 41 E3       --------------------------------------------------------------------    3. Apple Root CA
       SHA1 fingerprint: 61 1E 5B A2 2C 59 3A 08 FF 58 D1 61 E2 24 52 D1 98       
```

### 3. Sending the notarization request

Keep handy the app-specific password you've requested from your AppleID account (if you've not done it yet, follow the instruction [here](https://support.apple.com/en-us/HT204397)). Let's say that it is `cvbs-epfg-sizx-olwd`. The command I've used to request the notarization is:

```
xcrun altool --notarize-app --primary-bundle-id "com.ccextensions.alce3" --username "YourAppleID@mail.com" --password "cvbs-epfg-sizx-olwd" --file "/full/path/to/the/installer_signed.pkg"
```

Let's break down that.

- `xcrun` is used to find and execute XCode commands.
- `altool` is the executable that performs the notarization request.
- `--notarize-app` is the flag that tells `altool` to request notarization.
- `--primary-bundle-id` links to the installer I'm submitting its unique _bundle id_ (in my case `"com.ccextensions.alce3"`, substitute it with yours)
- `--username` wants your AppleID.
- `--password` wants the app-specific password you've requested (here, `"cvbs-epfg-sizx-olwd"`)
- `--file` is the flag that tells `altool` which file to upload. Please note that I had to specify the absolute full path (not the relative that starts with `./`) between quotes

The command takes a while to upload the file, and gives you zero feedback; after a while, if the process has been successful, you get back something like:

```
2019-09-18 16:30:35.659 altool[8788:92462] No errors uploading '/full/path/to/the/installer_signed.pkg'.
RequestUUID = 181638fb-a618-2298-bff0-47fa79f01326
```

Keep note of the `RequestUUID` string.

At this point your request for notarizationhas been sent to Apple, and you have to wait for them to process the file.

Mind you: if you get **"Error: You must first sign the relevant contracts online. (1048)"** (as it has happened to me several times) don't panic. After a lot of googling finding only partially similar errors, my understanding is that it is a temporary glitch on Apple's side.

Just in case, you can launch XCode to check whether it asks you to accept its Terms and Conditions, or run:

```
sudo xcodebuild -license
```

to do it from the Terminal. I doubt it will work, but when it comes to random, not repeatable issues our brain works like the pigeon's – we tend to try random solutions and stick to the ones that may have a chance to have contributed to the desired outcome. Probably if you haven't signed XCode Terms and Conditions the process will fail anyway.

### 4. Checking the notarization status

In the Terminal, enter:

```
xcrun altool --notarization-info 181638fb-a618-2298-bff0-47fa79f01326 --username "YourAppleID@mail.com" --password "cvbs-epfg-sizx-olwd"
```

The command uses now `--notarization-info` to ping the current status, and expects the same `RequestUUID` string that has been sent you in response of the original request. The result is something like that:

```
2019-09-18 16:31:48.449 altool[9133:95654] No errors getting notarization info.

   RequestUUID: 181638fb-a618-2298-bff0-47fa79f01326
          Date: 2019-09-18 14:30:36 +0000
        Status: in progress
    LogFileURL: (null)
```

Please note the _in progress_ status. At some point the result will be different, and hopefully similar to this one:

```
   RequestUUID: 287628eb-e628-4998-b4b0-47fa79f07386
          Date: 2019-09-18 14:30:36 +0000
        Status: success
    LogFileURL: https://osxapps-ssl.itunes.apple.com/awfully/long/URL
    Status Code: 0
Status Message: Package Approved
```

**The installer has been successfully notarized!**

If, for some reason, you have to perform several requests and want to check the notarization requests history, run the following command:

```
xcrun altool --notarization-history 0 --username "YourAppleID@mail.com" --password "cvbs-epfg-sizx-olwd"

Notarization History - page 0

Date                      RequestUUID                          Status  Status Code Status Message   
------------------------- ------------------------------------ ------- ----------- ---------------- 
2019-09-18 14:30:36 +0000 181638fb-a618-2298-bff0-47fa79f01326 success 0           Package Approved 

Next page value: 1768817026000

```

### 5. Stapling the ticket to the file

As I wrote, this step is optional but highly recommended: Gatekeeper will be able to find the whitelist info in the file itself, without the need to perform an online check.

```
xcrun stapler staple "/full/path/to/the/installer_signed.pkg"

Processing: /full/path/to/the/installer_signed.pkg
The staple and validate action worked!
```

The command here is much simpler: you don't need to pass any `RequestUUID` string yourself, for `stapler` will do the call home at Apple's on its own. 

You can check stapling details with:

```
stapler validate --verbose "/full/path/to/the/installer_signed.pkg"
```

The result is too long to paste here, and frankly I've no idea what it means: as long as it ends with `"The validate action worked!"` you should be fine.

And... **you're done** 🍾

## Caveats for different scenarios

The example I've shown is for one `installer.pkg` file. Let's say that you (as I do) deliver to your customers a `product.dmg` file, that wraps the `installer.pkg`, an additional `uninstaller.app` (say, an _app-ified_ AppleScript) and some documentation as well.

In this case, you need to:

- Notarize only the outmost container (here the `.dmg`).
- Sign all the executable children elements (here the `.app` and `.pkg`) _and_ the `.dmg` as well.

Please note that in order to sign a `.dmg` you need a "Developer ID Application" certificate, instead of the "Developer ID Installer" I've used for the `.pkg`

Lastly, things may get a bit convoluted when it comes to extra libraries/bundles that may be called by your panel – if you feel like it's your case, please read [this thread](https://forums.developer.apple.com/message/383180).

## Support this site!

Please consider supporting my work with the purchase of these books and courses. You can find them all [here](https://www.davidebarranca.com/courses/), bundles available. Thanks! 🙏🏻

<figure>
<a href="https://www.davidebarranca.com/courses/" ><img src="/wp-content/uploads/2019/05/courses.jpg" srcset="/wp-content/uploads/2019/05/courses.jpg 1x, /wp-content/uploads/2019/05/courses@2x.jpg 2x" alt="Courses"></a>
</figure> 

PS. I will update the Ultimate Guide to Native Installers and Automated Build Systems as well, I just wanted you to get informed asap.