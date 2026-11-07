# Ver-ID Swift Package Manager Collection

This repository contains [collection.json](./collection.json) file that can be used to discover Ver-ID SDK packages in Xcode.

A Swift Package Collection is a curated catalog of Swift packages. Instead of manually typing GitHub URLs for packages, you can point Xcode at a collection and browse all the packages it contains.

1. Open your project in Xcode
 - Launch Xcode 26.
 - Open the project or workspace where you want to use Ver-ID SDK.
2. Open the **Package Dependencies** tab
 - In the Project Navigator (the left sidebar), click the blue project icon at the very top.
 - In the editor area, select your project (not a target).
 - Across the top tabs (Info ▸ Build Settings ▸ …), click **Package Dependencies**.

	This tab lists all Swift packages currently linked to your project.
3. Add the package collection
 - At the bottom left of the Package Dependencies tab, click the “+” button.
 - Choose **Add Package Collection** from the menu.
 - Paste the collection URL: `https://github.com/AppliedRecognition/Ver-ID-Swift-Package-Collection/raw/refs/heads/main/collection.json`
 - Click **Add**.
4. Review trust information
 - If the collection is signed with a certificate Xcode recognizes, you’ll see it marked as Verified.
 - If it’s unsigned or signed with something not in Apple’s trust store, Xcode will warn you. This doesn’t prevent you from using it — click **Add** again to continue.
5. Browse and add a package
 - After adding, the collection appears in the Package Dependencies tab.
 - You can browse its packages right there.
 - To add one:
    - Select a package.
    - Choose a version rule (for example, “Up to Next Major”).
    - Confirm by clicking **Add Package**.
    - Xcode will download the package and integrate it into your project automatically.
    
Now your project has access to all the Swift packages from the Ver-ID Swift Package Collection. You only need to add the collection once per project — after that, you can reuse it anytime you add new packages.

## Deprecated packages

Two packages that used to be listed here are deprecated, and their repositories are archived:

* `Spoof-Detection-Fasnet-Apple` — a cloud FASnet model, three classes at 112×112, unrelated to the two-class 224×224 Stage 2 liveness classifier the Ver-ID 3 pipeline uses.
* `Spoof-Detection-Fusion-Apple` — that model fused with `Spoof-Device-Detection-Ver-ID-3-Apple`'s device check.

Nothing depends on either, including the legacy `Ver-ID` pod. Use `Spoof-Device-Detection-Ver-ID-3-Apple` instead. Archiving makes those repositories read-only but leaves them clonable and their tags resolvable, so a `Package.resolved` that already pins a version there still resolves.

## How this collection is built

`packages.json` is the input. `generate_collection.sh` turns it into the signed `collection.json`:

```
package-collection-generate packages.json collection_unsigned.json
package-collection-sign collection_unsigned.json collection.json key.pem swift_package.der AppleWWDRCAG3.der AppleIncRootCertificate.der
rm collection_unsigned.json
```

Neither tool is part of the Swift toolchain — `swift package-collection` offers only add, describe, list, refresh, remove and search. They are built from [`swiftlang/swift-package-collection-generator`](https://github.com/swiftlang/swift-package-collection-generator), branch `5.10`, with `swift build --configuration release`.

The signing key is not in this repository and must not be: `key.pem` and `*.p12` are in `.gitignore`. The certificate is `swift_package.der`; the private key lives in the login Keychain as the **Swift Package Collection: Applied Recognition Inc.** identity. Export it from Keychain Access when it is time to re-sign, and delete it afterwards.

Two things that cost time to find, recorded so they do not cost it twice:

* Keychain Access writes the p12 with **RC2-40-CBC**, which OpenSSL 3 refuses without `-legacy`. Use `/usr/bin/openssl`, which is LibreSSL and handles it natively.
* LibreSSL's `pkcs12 -nocerts` output begins with a **`Bag Attributes` preamble** that `package-collection-sign` cannot parse. It reports any key-loading failure as `unsupportedKeyType`, which reads like a key-type problem but is not. Strip everything before the first `-----BEGIN`.

The collection lists public packages only: it is a discovery catalogue, so an internal package has no business in it.
