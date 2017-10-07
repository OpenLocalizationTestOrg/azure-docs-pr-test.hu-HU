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
# <a name="add-authentication-tooyour-ios-app"></a>Hitelesítési tooyour iOS-alkalmazás hozzáadása
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Ebben az oktatóanyagban hitelesítési toohello hozzáadása [iOS quick start] projekt támogatott identitásszolgáltató használatával. Ez az oktatóanyag hello alapuló [iOS quick start] oktatóanyag, amely először el kell végeznie.

## <a name="register"></a>Regisztrálja az alkalmazást a hitelesítéshez, és az App Service hello konfigurálása
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>A toohello engedélyezett külső átirányítási URL-címek hozzáadása

Biztonságos hitelesítéshez az szükséges, hogy az alkalmazás adja meg egy új URL-sémát.  Ez lehetővé teszi, hogy hello authentication rendszer tooredirect hátsó tooyour alkalmazás hello hitelesítési folyamat befejezése után.  Ebben az oktatóanyagban az URL-séma használjuk _appname_ egész.  Bármely választja URL-sémát is használhatja.  Egyedi tooyour mobilalkalmazás kell lennie.  tooenable hello átirányítási th kiszolgáló oldalán:

1. A hello [Azure-portálon], válassza ki az App Service.

2. Kattintson a hello **hitelesítési / engedélyezési** menüjét.

3. Kattintson a **Azure Active Directory** alatt hello **hitelesítésszolgáltatókat** szakasz.

4. Set hello **üzemmód** túl**speciális**.

5. A hello **engedélyezett külső átirányítási URL-t**, adja meg `appname://easyauth.callback`.  Hello _appname_ hello URL-sémát a mobilalkalmazás Ez a karakterlánc.  Akkor érdemes követnie normál URL-cím meghatározása (használata betűket és számokat csak, és betűvel kezdődik) protokoll.  Meg kell győződnie, jegyezze fel az Ön által mivel kell tooadjust hello több helyen URL-sémát a mobilalkalmazás kódot hello karakterlánc.

6. Kattintson az **OK** gombra.

7. Kattintson a **Save** (Mentés) gombra.

## <a name="permissions"></a>Engedélyek tooauthenticated felhasználók korlátozása
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Az xcode-ban, nyomja le az **futtatása** toostart hello alkalmazást. Egy kivétel következik be, mert hello app próbál, nem hitelesített felhasználónak háttér tooaccess, de hello *TodoItem* tábla most hitelesítést igényel.

## <a name="add-authentication"></a>Hitelesítési tooapp hozzáadása
**Objective-C**:

1. Nyissa meg a Mac *QSTodoListViewController.m* az xcode-ban, és adja hozzá a következő metódus hello:

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

    Változás *google* túl*microsoftaccount*, *twitter*, *facebook*, vagy *windowsazureactivedirectory* használatakor nem Google az identitás-szolgáltatóként. Facebook használatakor meg kell [engedélyezett Facebook tartományok] [ 1] az alkalmazásban.

    Cserélje le a hello **urlScheme** rendelkező egy egyedi nevet az alkalmazásnak.  hello urlScheme kell kell hello ugyanaz, mint hello hello megadott URL-séma protokoll **engedélyezett külső átirányítási URL-t** mező mellett hello Azure-portálon. a hitelesítési kérelem befejeződése után hello urlScheme hello hitelesítési visszahívási tooswitch hátsó tooyour alkalmazás használja.

2. Cserélje le `[self refresh]` a `viewDidLoad` a *QSTodoListViewController.m* a hello a következő kódot:

    ```Objective-C
    [self loginAndGetData];
    ```

3. Nyissa meg hello `QSAppDelegate.h` fájlt, és adja hozzá a következő kód hello:

    ```Objective-C
    #import "QSTodoService.h"

    @property (strong, nonatomic) QSTodoService *qsTodoService;
    ```

4. Nyissa meg hello `QSAppDelegate.m` fájlt, és adja hozzá a következő kód hello:

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

   Adja hozzá a kódot közvetlenül hello sor olvasása előtt `#pragma mark - Core Data stack`.  Cserélje le a _appname_ egyidejűleg hello urlScheme értéknek az 1. lépésben használt.

5. Nyissa meg hello `AppName-Info.plist` (az alkalmazás hello nevű tagjára AppName) fájlt, és adja hozzá a következő kód hello:

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

    Ez a kód nem helyezhetők el hello `<dict>` elemet.  Cserélje le a hello _appname_ karakterlánc (a tömbön belüli **CFBundleURLSchemes**) az 1. lépésben kiválasztott hello alkalmazásnév.  Is tehet módosítások hello plist-szerkesztő – kattintson a hello `AppName-Info.plist` fájlt XCode tooopen hello plist szerkesztőben.

    Cserélje le a hello `com.microsoft.azure.zumo` karakterlánc- **CFBundleURLName** az Apple csomagot azonosítója.

6. Nyomja le az *futtatása* toostart hello app, majd jelentkezzen be. Jelentkezett be, amikor legyen képes tooview hello teendőlista és frissítéseket.

**SWIFT**:

1. Nyissa meg a Mac *ToDoTableViewController.swift* az xcode-ban, és adja hozzá a következő metódus hello:

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

    Változás *google* túl*microsoftaccount*, *twitter*, *facebook*, vagy *windowsazureactivedirectory* használatakor nem Google az identitás-szolgáltatóként. Facebook használatakor meg kell [engedélyezett Facebook tartományok] [ 1] az alkalmazásban.

    Cserélje le a hello **urlScheme** rendelkező egy egyedi nevet az alkalmazásnak.  hello urlScheme kell kell hello ugyanaz, mint hello hello megadott URL-séma protokoll **engedélyezett külső átirányítási URL-t** mező mellett hello Azure-portálon. a hitelesítési kérelem befejeződése után hello urlScheme hello hitelesítési visszahívási tooswitch hátsó tooyour alkalmazás használja.

2. Hello sorok eltávolítása `self.refreshControl?.beginRefreshing()` és `self.onRefresh(self.refreshControl)` végén `viewDidLoad()` a *ToDoTableViewController.swift*. Adjon hozzá egy túl`loginAndGetData()` azok:

    ```swift
    loginAndGetData()
    ```

3. Nyissa meg hello `AppDelegate.swift` fájlt, és adja hozzá a következő sor toohello hello `AppDelegate` osztály:

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

    Cserélje le a hello _appname_ egyidejűleg hello urlScheme értéknek az 1. lépésben használt.

4. Nyissa meg hello `AppName-Info.plist` (az alkalmazás hello nevű tagjára AppName) fájlt, és adja hozzá a következő kód hello:

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

    Ez a kód nem helyezhetők el hello `<dict>` elemet.  Cserélje le a hello _appname_ karakterlánc (a tömbön belüli **CFBundleURLSchemes**) az 1. lépésben kiválasztott hello alkalmazásnév.  Is tehet módosítások hello plist-szerkesztő – kattintson a hello `AppName-Info.plist` fájlt XCode tooopen hello plist szerkesztőben.

    Cserélje le a hello `com.microsoft.azure.zumo` karakterlánc- **CFBundleURLName** az Apple csomagot azonosítója.

5. Nyomja le az *futtatása* toostart hello app, majd jelentkezzen be. Jelentkezett be, amikor legyen képes tooview hello teendőlista és frissítéseket.

App Service hitelesítés almák Inter alkalmazáson belüli kommunikációhoz használ.  A témáról további részletekért tekintse meg a toohello [Apple dokumentáció][2]
<!-- URLs. -->

[1]: https://developers.facebook.com/docs/ios/ios9#whitelist
[2]: https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html
[Azure-portálon]: https://portal.azure.com

[iOS quick start]: app-service-mobile-ios-get-started.md

