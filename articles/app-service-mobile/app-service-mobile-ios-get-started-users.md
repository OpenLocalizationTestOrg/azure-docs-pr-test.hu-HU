---
title: "Hitelesítés hozzáadása az Azure Mobile Apps IOS"
description: "Útmutató az Azure Mobile Apps segítségével hitelesíti a felhasználókat identitás-szolgáltatóktól, beleértve az aad-ben, a Google, a Facebook, a Twitter és a Microsoft számos az iOS-alkalmazás."
services: app-service\mobile
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: ef3d3cbe-e7ca-45f9-987f-80c44209dc06
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: glenga
ms.openlocfilehash: 21a2cc6c1eaf4b34cbe8c2d7c4dbb69c8730cf32
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-ios-app"></a><span data-ttu-id="abe45-103">Hitelesítés hozzáadása az iOS-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="abe45-103">Add authentication to your iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="abe45-104">Ebben az oktatóanyagban a hitelesítés hozzáadása a [iOS quick start] projekt támogatott identitásszolgáltató használatával.</span><span class="sxs-lookup"><span data-stu-id="abe45-104">In this tutorial, you add authentication to the [iOS quick start] project using a supported identity provider.</span></span> <span data-ttu-id="abe45-105">Ez az oktatóanyag alapul a [iOS quick start] oktatóanyag, amely először el kell végeznie.</span><span class="sxs-lookup"><span data-stu-id="abe45-105">This tutorial is based on the [iOS quick start] tutorial, which you must complete first.</span></span>

## <span data-ttu-id="abe45-106"><a name="register"></a>Regisztrálja az alkalmazást a hitelesítéshez, és az App Service konfigurálása</span><span class="sxs-lookup"><span data-stu-id="abe45-106"><a name="register"></a>Register your app for authentication and configure the App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="abe45-107"><a name="redirecturl"></a>Az alkalmazás hozzáadása az engedélyezett külső átirányítási URL-címek</span><span class="sxs-lookup"><span data-stu-id="abe45-107"><a name="redirecturl"></a>Add your app to the Allowed External Redirect URLs</span></span>

<span data-ttu-id="abe45-108">Biztonságos hitelesítéshez az szükséges, hogy az alkalmazás adja meg egy új URL-sémát.</span><span class="sxs-lookup"><span data-stu-id="abe45-108">Secure authentication requires that you define a new URL scheme for your app.</span></span>  <span data-ttu-id="abe45-109">Ez lehetővé teszi a hitelesítési rendszer visszairányítja az alkalmazás a hitelesítési folyamat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="abe45-109">This allows the authentication system to redirect back to your app once the authentication process is complete.</span></span>  <span data-ttu-id="abe45-110">Ebben az oktatóanyagban az URL-séma használjuk _appname_ egész.</span><span class="sxs-lookup"><span data-stu-id="abe45-110">In this tutorial, we use the URL scheme _appname_ throughout.</span></span>  <span data-ttu-id="abe45-111">Bármely választja URL-sémát is használhatja.</span><span class="sxs-lookup"><span data-stu-id="abe45-111">However, you can use any URL scheme you choose.</span></span>  <span data-ttu-id="abe45-112">A mobilalkalmazás egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="abe45-112">It should be unique to your mobile application.</span></span>  <span data-ttu-id="abe45-113">Az átirányítás th kiszolgálóoldalon engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="abe45-113">To enable the redirection on th server side:</span></span>

1. <span data-ttu-id="abe45-114">Az a [Azure-portálon], válassza ki az App Service.</span><span class="sxs-lookup"><span data-stu-id="abe45-114">In the [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="abe45-115">Kattintson a **hitelesítési / engedélyezési** menüjét.</span><span class="sxs-lookup"><span data-stu-id="abe45-115">Click the **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="abe45-116">Kattintson a **Azure Active Directory** alatt a **hitelesítésszolgáltatókat** szakasz.</span><span class="sxs-lookup"><span data-stu-id="abe45-116">Click **Azure Active Directory** under the **Authentication Providers** section.</span></span>

4. <span data-ttu-id="abe45-117">Állítsa be a **üzemmód** való **speciális**.</span><span class="sxs-lookup"><span data-stu-id="abe45-117">Set the **Management mode** to **Advanced**.</span></span>

5. <span data-ttu-id="abe45-118">Az a **engedélyezett külső átirányítási URL-t**, adja meg `appname://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="abe45-118">In the **Allowed External Redirect URLs**, enter `appname://easyauth.callback`.</span></span>  <span data-ttu-id="abe45-119">A _appname_ ezt a karakterláncot a rendszer az URL-séma, a mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="abe45-119">The _appname_ in this string is the URL Scheme for your mobile application.</span></span>  <span data-ttu-id="abe45-120">Akkor érdemes követnie normál URL-cím meghatározása (használata betűket és számokat csak, és betűvel kezdődik) protokoll.</span><span class="sxs-lookup"><span data-stu-id="abe45-120">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="abe45-121">Meg kell győződnie, jegyezze fel a karakterláncot, amely úgy dönt, mivel úgy, hogy az URL-séma több helyen a mobilalkalmazás kódot kell.</span><span class="sxs-lookup"><span data-stu-id="abe45-121">You should make a note of the string that you choose as you will need to adjust your mobile application code with the URL Scheme in several places.</span></span>

6. <span data-ttu-id="abe45-122">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="abe45-122">Click **OK**.</span></span>

7. <span data-ttu-id="abe45-123">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="abe45-123">Click **Save**.</span></span>

## <span data-ttu-id="abe45-124"><a name="permissions"></a>A hitelesített felhasználókhoz jogosultságok korlátozása</span><span class="sxs-lookup"><span data-stu-id="abe45-124"><a name="permissions"></a>Restrict permissions to authenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="abe45-125">Az xcode-ban, nyomja le az **futtatása** az alkalmazás indításához.</span><span class="sxs-lookup"><span data-stu-id="abe45-125">In Xcode, press **Run** to start the app.</span></span> <span data-ttu-id="abe45-126">Egy kivétel következik be, mert az alkalmazás megpróbál hozzáférni egy nem hitelesített felhasználóként, háttér, de a *TodoItem* tábla most hitelesítést igényel.</span><span class="sxs-lookup"><span data-stu-id="abe45-126">An exception is raised because the app attempts to access the backend as an unauthenticated user, but the *TodoItem* table now requires authentication.</span></span>

## <span data-ttu-id="abe45-127"><a name="add-authentication"></a>Hitelesítés hozzáadása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="abe45-127"><a name="add-authentication"></a>Add authentication to app</span></span>
<span data-ttu-id="abe45-128">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="abe45-128">**Objective-C**:</span></span>

1. <span data-ttu-id="abe45-129">Nyissa meg a Mac *QSTodoListViewController.m* az xcode-ban, és adja hozzá a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="abe45-129">On your Mac, open *QSTodoListViewController.m* in Xcode and add the following method:</span></span>

    ```Objective-C
    - (void)loginAndGetData
    {
        QSAppDelegate *appDelegate = (QSAppDelegate *)[UIApplication sharedApplication].delegate;
        appDelegate.qsTodoService = self.todoService;

        [self.todoService.client loginWithProvider:@"google" urlScheme:@"appname" controller:self animated:YES completion:^(MSUser * _Nullable user, NSError * _Nullable error) {
            if (error) {
                NSLog(@"Login failed with error: %@, %@", error, [error userInfo]);
            }
            else {
                self.todoService.client.currentUser = user;
                NSLog(@"User logged in: %@", user.userId);

                [self refresh];
            }
        }];
    }
    ```

    <span data-ttu-id="abe45-130">Változás *google* való *microsoftaccount*, *twitter*, *facebook*, vagy *windowsazureactivedirectory* használatakor nem Google az identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="abe45-130">Change *google* to *microsoftaccount*, *twitter*, *facebook*, or *windowsazureactivedirectory* if you are not using Google as your identity provider.</span></span> <span data-ttu-id="abe45-131">Facebook használatakor meg kell [engedélyezett Facebook tartományok] [ 1] az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="abe45-131">If you use Facebook, you must [whitelist Facebook domains][1] in your app.</span></span>

    <span data-ttu-id="abe45-132">Cserélje le a **urlScheme** rendelkező egy egyedi nevet az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="abe45-132">Replace the **urlScheme** with a unique name for your application.</span></span>  <span data-ttu-id="abe45-133">A urlScheme meg kell egyeznie a megadott URL-séma protokollként a **engedélyezett külső átirányítási URL-t** mező mellett az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="abe45-133">The urlScheme should be the same as the URL Scheme protocol that you specified in the **Allowed External Redirect URLs** field in the Azure portal.</span></span> <span data-ttu-id="abe45-134">A urlScheme váltson az alkalmazás, a hitelesítési kérelem befejeződése után a hitelesítés-visszahívás használják.</span><span class="sxs-lookup"><span data-stu-id="abe45-134">The urlScheme is used by the authentication callback to switch back to your application after the authentication request is complete.</span></span>

2. <span data-ttu-id="abe45-135">Cserélje le `[self refresh]` a `viewDidLoad` a *QSTodoListViewController.m* az alábbi kódra:</span><span class="sxs-lookup"><span data-stu-id="abe45-135">Replace `[self refresh]` in `viewDidLoad` in *QSTodoListViewController.m* with the following code:</span></span>

    ```Objective-C
    [self loginAndGetData];
    ```

3. <span data-ttu-id="abe45-136">Nyissa meg a `QSAppDelegate.h` fájlt, és adja hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="abe45-136">Open the `QSAppDelegate.h` file and add the following code:</span></span>

    ```Objective-C
    #import "QSTodoService.h"

    @property (strong, nonatomic) QSTodoService *qsTodoService;
    ```

4. <span data-ttu-id="abe45-137">Nyissa meg a `QSAppDelegate.m` fájlt, és adja hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="abe45-137">Open the `QSAppDelegate.m` file and add the following code:</span></span>

    ```Objective-C
    - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
    {
        if ([[url.scheme lowercaseString] isEqualToString:@"appname"]) {
            // Resume login flow
            return [self.qsTodoService.client resumeWithURL:url];
        }
        else {
            return NO;
        }
    }
    ```

   <span data-ttu-id="abe45-138">Adja hozzá a kódot közvetlenül a sor olvasása előtt `#pragma mark - Core Data stack`.</span><span class="sxs-lookup"><span data-stu-id="abe45-138">Add this code directly before the line reading `#pragma mark - Core Data stack`.</span></span>  <span data-ttu-id="abe45-139">Cserélje le a _appname_ egyidejűleg az 1. lépésben használt urlScheme értékét.</span><span class="sxs-lookup"><span data-stu-id="abe45-139">Replace the _appname_ wih the urlScheme value that you used in step 1.</span></span>

5. <span data-ttu-id="abe45-140">Nyissa meg a `AppName-Info.plist` (tagjára AppName nevű, az alkalmazás) fájlt, és adja hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="abe45-140">Open the `AppName-Info.plist` file (replacing AppName with the name of your app), and add the following code:</span></span>

    ```XML
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLName</key>
            <string>com.microsoft.azure.zumo</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>appname</string>
            </array>
        </dict>
    </array>
    ```

    <span data-ttu-id="abe45-141">Ezt a kódot kell elhelyezni a `<dict>` elemet.</span><span class="sxs-lookup"><span data-stu-id="abe45-141">This code should be placed inside the `<dict>` element.</span></span>  <span data-ttu-id="abe45-142">Cserélje le a _appname_ karakterlánc (a tömbön belüli **CFBundleURLSchemes**) az 1. lépésben kiválasztott alkalmazás névvel.</span><span class="sxs-lookup"><span data-stu-id="abe45-142">Replace the _appname_ string (within the array for **CFBundleURLSchemes**) with the app name you chose in step 1.</span></span>  <span data-ttu-id="abe45-143">Is tehet ezeket a módosításokat a plist-szerkesztő - kattintson arra a `AppName-Info.plist` fájlt az xcode-ban nyissa meg a plist-szerkesztőt.</span><span class="sxs-lookup"><span data-stu-id="abe45-143">You can also make these changes in the plist editor - click on the `AppName-Info.plist` file in XCode to open the plist editor.</span></span>

    <span data-ttu-id="abe45-144">Cserélje le a `com.microsoft.azure.zumo` karakterlánc- **CFBundleURLName** az Apple csomagot azonosítója.</span><span class="sxs-lookup"><span data-stu-id="abe45-144">Replace the `com.microsoft.azure.zumo` string for **CFBundleURLName** with your Apple bundle identifier.</span></span>

6. <span data-ttu-id="abe45-145">Nyomja le az *futtatása* , és indítsa el az alkalmazást, majd jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="abe45-145">Press *Run* to start the app, and then log in.</span></span> <span data-ttu-id="abe45-146">Jelentkezett be, amikor a teendőlista és módosításokat kell lennie.</span><span class="sxs-lookup"><span data-stu-id="abe45-146">When you are logged in, you should be able to view the Todo list and make updates.</span></span>

<span data-ttu-id="abe45-147">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="abe45-147">**Swift**:</span></span>

1. <span data-ttu-id="abe45-148">Nyissa meg a Mac *ToDoTableViewController.swift* az xcode-ban, és adja hozzá a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="abe45-148">On your Mac, open *ToDoTableViewController.swift* in Xcode and add the following method:</span></span>

    ```swift
    func loginAndGetData() {

        guard let client = self.table?.client, client.currentUser == nil else {
            return
        }

        let appDelegate = UIApplication.shared.delegate as! AppDelegate
        appDelegate.todoTableViewController = self

        let loginBlock: MSClientLoginBlock = {(user, error) -> Void in
            if (error != nil) {
                print("Error: \(error?.localizedDescription)")
            }
            else {
                client.currentUser = user
                print("User logged in: \(user?.userId)")
            }
        }

        client.login(withProvider:"google", urlScheme: "appname", controller: self, animated: true, completion: loginBlock)

    }
    ```

    <span data-ttu-id="abe45-149">Változás *google* való *microsoftaccount*, *twitter*, *facebook*, vagy *windowsazureactivedirectory* használatakor nem Google az identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="abe45-149">Change *google* to *microsoftaccount*, *twitter*, *facebook*, or *windowsazureactivedirectory* if you are not using Google as your identity provider.</span></span> <span data-ttu-id="abe45-150">Facebook használatakor meg kell [engedélyezett Facebook tartományok] [ 1] az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="abe45-150">If you use Facebook, you must [whitelist Facebook domains][1] in your app.</span></span>

    <span data-ttu-id="abe45-151">Cserélje le a **urlScheme** rendelkező egy egyedi nevet az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="abe45-151">Replace the **urlScheme** with a unique name for your application.</span></span>  <span data-ttu-id="abe45-152">A urlScheme meg kell egyeznie a megadott URL-séma protokollként a **engedélyezett külső átirányítási URL-t** mező mellett az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="abe45-152">The urlScheme should be the same as the URL Scheme protocol that you specified in the **Allowed External Redirect URLs** field in the Azure portal.</span></span> <span data-ttu-id="abe45-153">A urlScheme váltson az alkalmazás, a hitelesítési kérelem befejeződése után a hitelesítés-visszahívás használják.</span><span class="sxs-lookup"><span data-stu-id="abe45-153">The urlScheme is used by the authentication callback to switch back to your application after the authentication request is complete.</span></span>

2. <span data-ttu-id="abe45-154">A sorok eltávolításához `self.refreshControl?.beginRefreshing()` és `self.onRefresh(self.refreshControl)` végén `viewDidLoad()` a *ToDoTableViewController.swift*.</span><span class="sxs-lookup"><span data-stu-id="abe45-154">Remove the lines `self.refreshControl?.beginRefreshing()` and `self.onRefresh(self.refreshControl)` at the end of `viewDidLoad()` in *ToDoTableViewController.swift*.</span></span> <span data-ttu-id="abe45-155">Adjon hozzá egy `loginAndGetData()` azok:</span><span class="sxs-lookup"><span data-stu-id="abe45-155">Add a call to `loginAndGetData()` in their place:</span></span>

    ```swift
    loginAndGetData()
    ```

3. <span data-ttu-id="abe45-156">Nyissa meg a `AppDelegate.swift` fájlt, és adja hozzá a következő sort a `AppDelegate` osztály:</span><span class="sxs-lookup"><span data-stu-id="abe45-156">Open the `AppDelegate.swift` file and add the following line to the `AppDelegate` class:</span></span>

    ```swift
    var todoTableViewController: ToDoTableViewController?

    func application(_ application: UIApplication, openURL url: NSURL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
        if url.scheme?.lowercased() == "appname" {
            return (todoTableViewController!.table?.client.resume(with: url as URL))!
        }
        else {
            return false
        }
    }
    ```

    <span data-ttu-id="abe45-157">Cserélje le a _appname_ egyidejűleg az 1. lépésben használt urlScheme értékét.</span><span class="sxs-lookup"><span data-stu-id="abe45-157">Replace the _appname_ wih the urlScheme value that you used in step 1.</span></span>

4. <span data-ttu-id="abe45-158">Nyissa meg a `AppName-Info.plist` (tagjára AppName nevű, az alkalmazás) fájlt, és adja hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="abe45-158">Open the `AppName-Info.plist` file (replacing AppName with the name of your app), and add the following code:</span></span>

    ```xml
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLName</key>
            <string>com.microsoft.azure.zumo</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>appname</string>
            </array>
        </dict>
    </array>
    ```

    <span data-ttu-id="abe45-159">Ezt a kódot kell elhelyezni a `<dict>` elemet.</span><span class="sxs-lookup"><span data-stu-id="abe45-159">This code should be placed inside the `<dict>` element.</span></span>  <span data-ttu-id="abe45-160">Cserélje le a _appname_ karakterlánc (a tömbön belüli **CFBundleURLSchemes**) az 1. lépésben kiválasztott alkalmazás névvel.</span><span class="sxs-lookup"><span data-stu-id="abe45-160">Replace the _appname_ string (within the array for **CFBundleURLSchemes**) with the app name you chose in step 1.</span></span>  <span data-ttu-id="abe45-161">Is tehet ezeket a módosításokat a plist-szerkesztő - kattintson arra a `AppName-Info.plist` fájlt az xcode-ban nyissa meg a plist-szerkesztőt.</span><span class="sxs-lookup"><span data-stu-id="abe45-161">You can also make these changes in the plist editor - click on the `AppName-Info.plist` file in XCode to open the plist editor.</span></span>

    <span data-ttu-id="abe45-162">Cserélje le a `com.microsoft.azure.zumo` karakterlánc- **CFBundleURLName** az Apple csomagot azonosítója.</span><span class="sxs-lookup"><span data-stu-id="abe45-162">Replace the `com.microsoft.azure.zumo` string for **CFBundleURLName** with your Apple bundle identifier.</span></span>

5. <span data-ttu-id="abe45-163">Nyomja le az *futtatása* , és indítsa el az alkalmazást, majd jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="abe45-163">Press *Run* to start the app, and then log in.</span></span> <span data-ttu-id="abe45-164">Jelentkezett be, amikor a teendőlista és módosításokat kell lennie.</span><span class="sxs-lookup"><span data-stu-id="abe45-164">When you are logged in, you should be able to view the Todo list and make updates.</span></span>

<span data-ttu-id="abe45-165">App Service hitelesítés almák Inter alkalmazáson belüli kommunikációhoz használ.</span><span class="sxs-lookup"><span data-stu-id="abe45-165">App Service Authentication uses Apples Inter-App Communication.</span></span>  <span data-ttu-id="abe45-166">A témáról további részletekért tekintse meg a [Apple dokumentáció][2]</span><span class="sxs-lookup"><span data-stu-id="abe45-166">For more details on this subject, refer to the [Apple Documentation][2]</span></span>
<!-- URLs. -->

[1]: https://developers.facebook.com/docs/ios/ios9#whitelist
[2]: https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html
<span data-ttu-id="abe45-167">[Azure-portálon]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="abe45-167">[Azure portal]: https://portal.azure.com</span></span>

<span data-ttu-id="abe45-168">[iOS quick start]: app-service-mobile-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="abe45-168">[iOS quick start]: app-service-mobile-ios-get-started.md</span></span>

