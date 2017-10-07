---
title: "az Azure Mobile Apps IOS hitelesítési aaaAdd"
description: "Megtudhatja, hogyan toouse Azure Mobile Apps tooauthenticate felhasználókat identitás-szolgáltatóktól, beleértve az aad-ben, a Google, a Facebook, a Twitter és a Microsoft számos az iOS-alkalmazás."
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
ms.openlocfilehash: df129e1c7517582db0e4705e0a6e98345ac8a48c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-ios-app"></a><span data-ttu-id="f860d-103">Hitelesítési tooyour iOS-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f860d-103">Add authentication tooyour iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="f860d-104">Ebben az oktatóanyagban hitelesítési toohello hozzáadása [iOS quick start] projekt támogatott identitásszolgáltató használatával.</span><span class="sxs-lookup"><span data-stu-id="f860d-104">In this tutorial, you add authentication toohello [iOS quick start] project using a supported identity provider.</span></span> <span data-ttu-id="f860d-105">Ez az oktatóanyag hello alapuló [iOS quick start] oktatóanyag, amely először el kell végeznie.</span><span class="sxs-lookup"><span data-stu-id="f860d-105">This tutorial is based on hello [iOS quick start] tutorial, which you must complete first.</span></span>

## <span data-ttu-id="f860d-106"><a name="register"></a>Regisztrálja az alkalmazást a hitelesítéshez, és az App Service hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f860d-106"><a name="register"></a>Register your app for authentication and configure hello App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="f860d-107"><a name="redirecturl"></a>A toohello engedélyezett külső átirányítási URL-címek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f860d-107"><a name="redirecturl"></a>Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="f860d-108">Biztonságos hitelesítéshez az szükséges, hogy az alkalmazás adja meg egy új URL-sémát.</span><span class="sxs-lookup"><span data-stu-id="f860d-108">Secure authentication requires that you define a new URL scheme for your app.</span></span>  <span data-ttu-id="f860d-109">Ez lehetővé teszi, hogy hello authentication rendszer tooredirect hátsó tooyour alkalmazás hello hitelesítési folyamat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="f860d-109">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span>  <span data-ttu-id="f860d-110">Ebben az oktatóanyagban az URL-séma használjuk _appname_ egész.</span><span class="sxs-lookup"><span data-stu-id="f860d-110">In this tutorial, we use the URL scheme _appname_ throughout.</span></span>  <span data-ttu-id="f860d-111">Bármely választja URL-sémát is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f860d-111">However, you can use any URL scheme you choose.</span></span>  <span data-ttu-id="f860d-112">Egyedi tooyour mobilalkalmazás kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f860d-112">It should be unique tooyour mobile application.</span></span>  <span data-ttu-id="f860d-113">tooenable hello átirányítási th kiszolgáló oldalán:</span><span class="sxs-lookup"><span data-stu-id="f860d-113">tooenable hello redirection on th server side:</span></span>

1. <span data-ttu-id="f860d-114">A hello [Azure-portálon], válassza ki az App Service.</span><span class="sxs-lookup"><span data-stu-id="f860d-114">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="f860d-115">Kattintson a hello **hitelesítési / engedélyezési** menüjét.</span><span class="sxs-lookup"><span data-stu-id="f860d-115">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="f860d-116">Kattintson a **Azure Active Directory** alatt hello **hitelesítésszolgáltatókat** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f860d-116">Click **Azure Active Directory** under hello **Authentication Providers** section.</span></span>

4. <span data-ttu-id="f860d-117">Set hello **üzemmód** túl**speciális**.</span><span class="sxs-lookup"><span data-stu-id="f860d-117">Set hello **Management mode** too**Advanced**.</span></span>

5. <span data-ttu-id="f860d-118">A hello **engedélyezett külső átirányítási URL-t**, adja meg `appname://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="f860d-118">In hello **Allowed External Redirect URLs**, enter `appname://easyauth.callback`.</span></span>  <span data-ttu-id="f860d-119">Hello _appname_ hello URL-sémát a mobilalkalmazás Ez a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="f860d-119">hello _appname_ in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="f860d-120">Akkor érdemes követnie normál URL-cím meghatározása (használata betűket és számokat csak, és betűvel kezdődik) protokoll.</span><span class="sxs-lookup"><span data-stu-id="f860d-120">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="f860d-121">Meg kell győződnie, jegyezze fel az Ön által mivel kell tooadjust hello több helyen URL-sémát a mobilalkalmazás kódot hello karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="f860d-121">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

6. <span data-ttu-id="f860d-122">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f860d-122">Click **OK**.</span></span>

7. <span data-ttu-id="f860d-123">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="f860d-123">Click **Save**.</span></span>

## <span data-ttu-id="f860d-124"><a name="permissions"></a>Engedélyek tooauthenticated felhasználók korlátozása</span><span class="sxs-lookup"><span data-stu-id="f860d-124"><a name="permissions"></a>Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="f860d-125">Az xcode-ban, nyomja le az **futtatása** toostart hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f860d-125">In Xcode, press **Run** toostart hello app.</span></span> <span data-ttu-id="f860d-126">Egy kivétel következik be, mert hello app próbál, nem hitelesített felhasználónak háttér tooaccess, de hello *TodoItem* tábla most hitelesítést igényel.</span><span class="sxs-lookup"><span data-stu-id="f860d-126">An exception is raised because hello app attempts tooaccess the backend as an unauthenticated user, but hello *TodoItem* table now requires authentication.</span></span>

## <span data-ttu-id="f860d-127"><a name="add-authentication"></a>Hitelesítési tooapp hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f860d-127"><a name="add-authentication"></a>Add authentication tooapp</span></span>
<span data-ttu-id="f860d-128">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="f860d-128">**Objective-C**:</span></span>

1. <span data-ttu-id="f860d-129">Nyissa meg a Mac *QSTodoListViewController.m* az xcode-ban, és adja hozzá a következő metódus hello:</span><span class="sxs-lookup"><span data-stu-id="f860d-129">On your Mac, open *QSTodoListViewController.m* in Xcode and add hello following method:</span></span>

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

    <span data-ttu-id="f860d-130">Változás *google* túl*microsoftaccount*, *twitter*, *facebook*, vagy *windowsazureactivedirectory* használatakor nem Google az identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="f860d-130">Change *google* too*microsoftaccount*, *twitter*, *facebook*, or *windowsazureactivedirectory* if you are not using Google as your identity provider.</span></span> <span data-ttu-id="f860d-131">Facebook használatakor meg kell [engedélyezett Facebook tartományok] [ 1] az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f860d-131">If you use Facebook, you must [whitelist Facebook domains][1] in your app.</span></span>

    <span data-ttu-id="f860d-132">Cserélje le a hello **urlScheme** rendelkező egy egyedi nevet az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="f860d-132">Replace hello **urlScheme** with a unique name for your application.</span></span>  <span data-ttu-id="f860d-133">hello urlScheme kell kell hello ugyanaz, mint hello hello megadott URL-séma protokoll **engedélyezett külső átirányítási URL-t** mező mellett hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f860d-133">hello urlScheme should be hello same as hello URL Scheme protocol that you specified in hello **Allowed External Redirect URLs** field in hello Azure portal.</span></span> <span data-ttu-id="f860d-134">a hitelesítési kérelem befejeződése után hello urlScheme hello hitelesítési visszahívási tooswitch hátsó tooyour alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="f860d-134">hello urlScheme is used by hello authentication callback tooswitch back tooyour application after the authentication request is complete.</span></span>

2. <span data-ttu-id="f860d-135">Cserélje le `[self refresh]` a `viewDidLoad` a *QSTodoListViewController.m* a hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="f860d-135">Replace `[self refresh]` in `viewDidLoad` in *QSTodoListViewController.m* with hello following code:</span></span>

    ```Objective-C
    [self loginAndGetData];
    ```

3. <span data-ttu-id="f860d-136">Nyissa meg hello `QSAppDelegate.h` fájlt, és adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="f860d-136">Open hello `QSAppDelegate.h` file and add hello following code:</span></span>

    ```Objective-C
    #import "QSTodoService.h"

    @property (strong, nonatomic) QSTodoService *qsTodoService;
    ```

4. <span data-ttu-id="f860d-137">Nyissa meg hello `QSAppDelegate.m` fájlt, és adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="f860d-137">Open hello `QSAppDelegate.m` file and add hello following code:</span></span>

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

   <span data-ttu-id="f860d-138">Adja hozzá a kódot közvetlenül hello sor olvasása előtt `#pragma mark - Core Data stack`.</span><span class="sxs-lookup"><span data-stu-id="f860d-138">Add this code directly before hello line reading `#pragma mark - Core Data stack`.</span></span>  <span data-ttu-id="f860d-139">Cserélje le a _appname_ egyidejűleg hello urlScheme értéknek az 1. lépésben használt.</span><span class="sxs-lookup"><span data-stu-id="f860d-139">Replace the _appname_ wih hello urlScheme value that you used in step 1.</span></span>

5. <span data-ttu-id="f860d-140">Nyissa meg hello `AppName-Info.plist` (az alkalmazás hello nevű tagjára AppName) fájlt, és adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="f860d-140">Open hello `AppName-Info.plist` file (replacing AppName with hello name of your app), and add hello following code:</span></span>

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

    <span data-ttu-id="f860d-141">Ez a kód nem helyezhetők el hello `<dict>` elemet.</span><span class="sxs-lookup"><span data-stu-id="f860d-141">This code should be placed inside hello `<dict>` element.</span></span>  <span data-ttu-id="f860d-142">Cserélje le a hello _appname_ karakterlánc (a tömbön belüli **CFBundleURLSchemes**) az 1. lépésben kiválasztott hello alkalmazásnév.</span><span class="sxs-lookup"><span data-stu-id="f860d-142">Replace hello _appname_ string (within the array for **CFBundleURLSchemes**) with hello app name you chose in step 1.</span></span>  <span data-ttu-id="f860d-143">Is tehet módosítások hello plist-szerkesztő – kattintson a hello `AppName-Info.plist` fájlt XCode tooopen hello plist szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="f860d-143">You can also make these changes in hello plist editor - click on hello `AppName-Info.plist` file in XCode tooopen hello plist editor.</span></span>

    <span data-ttu-id="f860d-144">Cserélje le a hello `com.microsoft.azure.zumo` karakterlánc- **CFBundleURLName** az Apple csomagot azonosítója.</span><span class="sxs-lookup"><span data-stu-id="f860d-144">Replace hello `com.microsoft.azure.zumo` string for **CFBundleURLName** with your Apple bundle identifier.</span></span>

6. <span data-ttu-id="f860d-145">Nyomja le az *futtatása* toostart hello app, majd jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="f860d-145">Press *Run* toostart hello app, and then log in.</span></span> <span data-ttu-id="f860d-146">Jelentkezett be, amikor legyen képes tooview hello teendőlista és frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="f860d-146">When you are logged in, you should be able tooview hello Todo list and make updates.</span></span>

<span data-ttu-id="f860d-147">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="f860d-147">**Swift**:</span></span>

1. <span data-ttu-id="f860d-148">Nyissa meg a Mac *ToDoTableViewController.swift* az xcode-ban, és adja hozzá a következő metódus hello:</span><span class="sxs-lookup"><span data-stu-id="f860d-148">On your Mac, open *ToDoTableViewController.swift* in Xcode and add hello following method:</span></span>

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

    <span data-ttu-id="f860d-149">Változás *google* túl*microsoftaccount*, *twitter*, *facebook*, vagy *windowsazureactivedirectory* használatakor nem Google az identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="f860d-149">Change *google* too*microsoftaccount*, *twitter*, *facebook*, or *windowsazureactivedirectory* if you are not using Google as your identity provider.</span></span> <span data-ttu-id="f860d-150">Facebook használatakor meg kell [engedélyezett Facebook tartományok] [ 1] az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f860d-150">If you use Facebook, you must [whitelist Facebook domains][1] in your app.</span></span>

    <span data-ttu-id="f860d-151">Cserélje le a hello **urlScheme** rendelkező egy egyedi nevet az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="f860d-151">Replace hello **urlScheme** with a unique name for your application.</span></span>  <span data-ttu-id="f860d-152">hello urlScheme kell kell hello ugyanaz, mint hello hello megadott URL-séma protokoll **engedélyezett külső átirányítási URL-t** mező mellett hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f860d-152">hello urlScheme should be hello same as hello URL Scheme protocol that you specified in hello **Allowed External Redirect URLs** field in hello Azure portal.</span></span> <span data-ttu-id="f860d-153">a hitelesítési kérelem befejeződése után hello urlScheme hello hitelesítési visszahívási tooswitch hátsó tooyour alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="f860d-153">hello urlScheme is used by hello authentication callback tooswitch back tooyour application after the authentication request is complete.</span></span>

2. <span data-ttu-id="f860d-154">Hello sorok eltávolítása `self.refreshControl?.beginRefreshing()` és `self.onRefresh(self.refreshControl)` végén `viewDidLoad()` a *ToDoTableViewController.swift*.</span><span class="sxs-lookup"><span data-stu-id="f860d-154">Remove hello lines `self.refreshControl?.beginRefreshing()` and `self.onRefresh(self.refreshControl)` at the end of `viewDidLoad()` in *ToDoTableViewController.swift*.</span></span> <span data-ttu-id="f860d-155">Adjon hozzá egy túl`loginAndGetData()` azok:</span><span class="sxs-lookup"><span data-stu-id="f860d-155">Add a call too`loginAndGetData()` in their place:</span></span>

    ```swift
    loginAndGetData()
    ```

3. <span data-ttu-id="f860d-156">Nyissa meg hello `AppDelegate.swift` fájlt, és adja hozzá a következő sor toohello hello `AppDelegate` osztály:</span><span class="sxs-lookup"><span data-stu-id="f860d-156">Open hello `AppDelegate.swift` file and add hello following line toohello `AppDelegate` class:</span></span>

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

    <span data-ttu-id="f860d-157">Cserélje le a hello _appname_ egyidejűleg hello urlScheme értéknek az 1. lépésben használt.</span><span class="sxs-lookup"><span data-stu-id="f860d-157">Replace hello _appname_ wih hello urlScheme value that you used in step 1.</span></span>

4. <span data-ttu-id="f860d-158">Nyissa meg hello `AppName-Info.plist` (az alkalmazás hello nevű tagjára AppName) fájlt, és adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="f860d-158">Open hello `AppName-Info.plist` file (replacing AppName with hello name of your app), and add hello following code:</span></span>

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

    <span data-ttu-id="f860d-159">Ez a kód nem helyezhetők el hello `<dict>` elemet.</span><span class="sxs-lookup"><span data-stu-id="f860d-159">This code should be placed inside hello `<dict>` element.</span></span>  <span data-ttu-id="f860d-160">Cserélje le a hello _appname_ karakterlánc (a tömbön belüli **CFBundleURLSchemes**) az 1. lépésben kiválasztott hello alkalmazásnév.</span><span class="sxs-lookup"><span data-stu-id="f860d-160">Replace hello _appname_ string (within the array for **CFBundleURLSchemes**) with hello app name you chose in step 1.</span></span>  <span data-ttu-id="f860d-161">Is tehet módosítások hello plist-szerkesztő – kattintson a hello `AppName-Info.plist` fájlt XCode tooopen hello plist szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="f860d-161">You can also make these changes in hello plist editor - click on hello `AppName-Info.plist` file in XCode tooopen hello plist editor.</span></span>

    <span data-ttu-id="f860d-162">Cserélje le a hello `com.microsoft.azure.zumo` karakterlánc- **CFBundleURLName** az Apple csomagot azonosítója.</span><span class="sxs-lookup"><span data-stu-id="f860d-162">Replace hello `com.microsoft.azure.zumo` string for **CFBundleURLName** with your Apple bundle identifier.</span></span>

5. <span data-ttu-id="f860d-163">Nyomja le az *futtatása* toostart hello app, majd jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="f860d-163">Press *Run* toostart hello app, and then log in.</span></span> <span data-ttu-id="f860d-164">Jelentkezett be, amikor legyen képes tooview hello teendőlista és frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="f860d-164">When you are logged in, you should be able tooview hello Todo list and make updates.</span></span>

<span data-ttu-id="f860d-165">App Service hitelesítés almák Inter alkalmazáson belüli kommunikációhoz használ.</span><span class="sxs-lookup"><span data-stu-id="f860d-165">App Service Authentication uses Apples Inter-App Communication.</span></span>  <span data-ttu-id="f860d-166">A témáról további részletekért tekintse meg a toohello [Apple dokumentáció][2]</span><span class="sxs-lookup"><span data-stu-id="f860d-166">For more details on this subject, refer toohello [Apple Documentation][2]</span></span>
<!-- URLs. -->

[1]: https://developers.facebook.com/docs/ios/ios9#whitelist
[2]: https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html
[Azure-portálon]: https://portal.azure.com

[iOS quick start]: app-service-mobile-ios-get-started.md

