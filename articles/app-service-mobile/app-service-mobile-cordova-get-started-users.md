---
title: "a Mobile Apps Apache Cordova-aaaAdd hitelesítés |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Mobile Apps az Azure App Service tooauthenticate felhasználókat identitás-szolgáltatóktól, beleértve a Google, a Facebook, a Twitter és a Microsoft számos az Apache Cordova-alkalmazás."
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 10dd6dc9-ddf5-423d-8205-00ad74929f0d
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 61a05c5ac67fd0f0bc4c9d6920954a9b464a0a8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-apache-cordova-app"></a>Hitelesítési tooyour Apache Cordova-alkalmazás hozzáadása
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Összefoglalás
Ebben az oktatóanyagban a hitelesítési toohello todolist gyorsútmutató-projekt az Apache Cordova segítségével egy támogatott identitásszolgáltató hozzá. Ez az oktatóanyag hello alapuló [Ismerkedés a Mobile Apps] oktatóanyag, amely először el kell végeznie.

## <a name="register"></a>Regisztrálja az alkalmazást a hitelesítéshez, és az App Service hello konfigurálása
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Tekintse meg hasonló lépéseket ismertető videót](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

## <a name="permissions"></a>Engedélyek tooauthenticated felhasználók korlátozása
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Most ellenőrizheti, hogy a névtelen hozzáférés tooyour háttér le van tiltva. A Visual Studio:

* Nyissa meg hello projekt hello oktatóanyag befejezésekor létrehozott [Ismerkedés a Mobile Apps].
* Futtassa az alkalmazást a hello **Google Android Emulator**.
* Ellenőrizze, hogy egy váratlan csatlakozási hiba hello alkalmazás indítása után.

Következő lépésként frissítse hello app tooauthenticate felhasználók erőforrások kér hello Mobile Apps-háttéralkalmazás előtt.

## <a name="add-authentication"></a>Hitelesítési toohello alkalmazás hozzáadása
1. Nyissa meg a projektet a **Visual Studio**, majd nyissa meg hello `www/index.html` fájlt szerkesztésre.
2. Keresse meg a hello `Content-Security-Policy` metaadatként hello központi szakaszban.  Adja hozzá a hello OAuth állomás toohello engedélyezett adatforrások listáját.

   | Szolgáltató | SDK-szolgáltató neve | OAuth-állomás |
   |:--- |:--- |:--- |
   | Azure Active Directory | aad-ben | https://login.microsoftonline.com |
   | Facebook | Facebook-on | https://www.Facebook.com |
   | Google | Google | https://Accounts.google.com |
   | Microsoft | MicrosoftAccount | https://Login.live.com |
   | Twitter | Twitter | https://API.Twitter.com |

    Tartalom-biztonsági-Policy (az Azure Active Directory megvalósítva) például a következőképpen történik:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.microsoftonline.com https://yourapp.azurewebsites.net; style-src 'self'">

    Cserélje le `https://login.microsoftonline.com` a tábla megelőző hello hello OAuth állomással.  Hello tartalom-security-házirend metaadatként kapcsolatos további információkért lásd: hello [tartalom-Security-házirend dokumentációját].

    Néhány hitelesítésszolgáltatókat tartalom-Security-házirend módosítja a megfelelő mobileszközök használatakor nem szükséges.  Például nem tartalom-Security-házirend módosításai szükségesek, ha a Google-hitelesítést használó Android-eszközön.

3. Nyissa meg hello `www/js/index.js` szerkesztésre fájlt, keresse meg a hello `onDeviceReady()` metódust, és hello ügyfél létrehozása alatt kódot adja hozzá a következő kód hello:

        // Login toohello service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh hello todoItems
                refreshDisplay();

                // Wire up hello UI Event Handler for hello Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Ez a kód hello meglévő kódot, amely hello táblahivatkozás hoz létre, és frissíti a felhasználói felület hello váltja fel.

    hello login() metódus elindítja a hitelesítési hello szolgáltatónál. hello login() metódus JavaScript ígéret visszaadó aszinkron függvényre.  hello inicializálási hello részeinek hello ígéret válasz belül kerül, hogy nem végre, amíg hello login() metódus befejeződik.

4. Hello kódot, amely az előzőekben adott hozzá, cserélje le `SDK_Provider_Name` hello nevet, a bejelentkezés-szolgáltató. Például az Azure Active Directory használata `client.login('aad')`.
5. Futtatja a projektet.  Hello projekt inicializálása befejeződött, amikor az alkalmazás a kiválasztott hitelesítési szolgáltató hello hello OAuth bejelentkezési lapját jeleníti meg.

## <a name="next-steps"></a>Következő lépések
* További [vonatkozó hitelesítési] Azure App Service szolgáltatással.
* Folytassa a hello oktatóanyag hozzáadásával [leküldéses értesítések] tooyour Apache Cordova-alkalmazáshoz.

Ismerje meg, hogyan toouse hello SDK-k.

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

<!-- URLs. -->
[Ismerkedés a Mobile Apps]: app-service-mobile-cordova-get-started.md
[tartalom-Security-házirend dokumentációját]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[leküldéses értesítések]: app-service-mobile-cordova-get-started-push.md
[vonatkozó hitelesítési]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
