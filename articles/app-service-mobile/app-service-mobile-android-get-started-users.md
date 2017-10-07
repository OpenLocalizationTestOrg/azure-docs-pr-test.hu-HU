---
title: "az Android a Mobile Apps aaaAdd hitelesítési |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello az Android-alkalmazás Identitásszolgáltatók, beleértve a Google, a Facebook, a Twitter és a Microsoft számos tooauthenticate felhasználói Azure App Service Mobile Apps szolgáltatásának."
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 1fc8e7c1-6c3c-40f4-9967-9cf5e21fc4e1
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 01f608f996c931c643790ed2778df11cf590c903
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-android-app"></a>Hitelesítési tooyour Android-alkalmazás hozzáadása
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Összefoglalás
Ebben az oktatóanyagban hozzáadása hitelesítési toohello todolist gyorsútmutató-projekt az Android a támogatott identitásszolgáltató használatával. Ez az oktatóanyag hello alapuló [Ismerkedés a Mobile Apps] oktatóanyag, amely először el kell végeznie.

## <a name="register"></a>Hitelesítés az alkalmazás regisztrálása és konfigurálása az Azure App Service
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>A toohello engedélyezett külső átirányítási URL-címek hozzáadása

Biztonságos hitelesítéshez az szükséges, hogy az alkalmazás adja meg egy új URL-sémát. Ez lehetővé teszi, hogy hello authentication rendszer tooredirect hátsó tooyour alkalmazás hello hitelesítési folyamat befejezése után. Ebben az oktatóanyagban hello URL-séma használjuk _appname_ egész. Bármely választja URL-sémát is használhatja. Egyedi tooyour mobilalkalmazás kell lennie. tooenable hello átirányítási hello kiszolgáló oldalán:

1. Hello [Azure-portálon] válassza ki az App Service.

2. Kattintson a hello **hitelesítési / engedélyezési** menüjét.

3. A hello **engedélyezett külső átirányítási URL-t**, adja meg `appname://easyauth.callback`.  Hello _appname_ hello URL-sémát a mobilalkalmazás Ez a karakterlánc.  Akkor érdemes követnie normál URL-cím meghatározása (használata betűket és számokat csak, és betűvel kezdődik) protokoll.  Meg kell győződnie, jegyezze fel az Ön által mivel kell tooadjust hello több helyen URL-sémát a mobilalkalmazás kódot hello karakterlánc.

4. Kattintson az **OK** gombra.

5. Kattintson a **Save** (Mentés) gombra.

## <a name="permissions"></a>Engedélyek tooauthenticated felhasználók korlátozása
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* Az Android Studióban nyissa meg elvégezte hello projekt hello oktatóanyag [Ismerkedés a Mobile Apps]. A hello **futtatása** menüben kattintson **-alkalmazás futtatása**, és győződjön meg arról, hogy a 401-es (nem engedélyezett) egy állapotkód: nem kezelt kivétel hello alkalmazás indítása után jelenik meg.

     Ez a kivétel az oka, hogy hello app kísérletek tooaccess hello vissza, nem hitelesített felhasználónak végén, de hello *TodoItem* tábla most hitelesítést igényel.

Következő lépésként frissítse a hello app tooauthenticate felhasználók erőforrások kér hello vissza a Mobile Apps befejezése előtt. 

## <a name="add-authentication-toohello-app"></a>Hitelesítési toohello alkalmazás hozzáadása
[!INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]



## <a name="cache-tokens"></a>A hitelesítési tokenek hello ügyfélen gyorsítótárazása
[!INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Következő lépések
Most, hogy elvégezte az oktatóanyag az egyszerű hitelesítés, vegye figyelembe az alábbi oktatóanyagok hello tooone a Folytatás:

* [Leküldéses értesítések tooyour Android-alkalmazás hozzáadása](app-service-mobile-android-get-started-push.md).
  Ismerje meg, hogyan a Mobile Apps biztonsági tooconfigure end toouse Azure notification hubs toosend leküldéses értesítéseket.
* [Az Android-alkalmazás kapcsolat nélküli szinkronizálásának engedélyezése](app-service-mobile-android-get-started-offline-data.md).
  Ismerje meg, hogyan tooadd offline tooyour alkalmazás használatával támogatják a Mobile Apps háttérből. Kapcsolat nélküli szinkronizálás, a felhasználók használhatják a mobilalkalmazás&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions tooauthenticated users]: #permissions
[Add authentication toohello app]: #add-authentication
[Store authentication tokens on hello client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Ismerkedés a Mobile Apps]: app-service-mobile-android-get-started.md
