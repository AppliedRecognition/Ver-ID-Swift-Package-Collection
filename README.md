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