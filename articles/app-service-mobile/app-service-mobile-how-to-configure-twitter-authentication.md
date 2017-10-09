---
title: "aaaHow tooconfigure Twitter hitelesítési alkalmazásszolgáltatások alkalmazás"
description: "Megtudhatja, hogyan tooconfigure Twitter hitelesítési alkalmazásszolgáltatások alkalmazás."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 0d16ac44d7b54e3567b793a904059d31ab1d8911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-twitter-login"></a>Hogyan tooconfigure App Service alkalmazás toouse Twitter bejelentkezési adatait
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Ez a témakör bemutatja, hogyan tooconfigure Azure App Service toouse Twitter hitelesítési-szolgáltatóként.

toocomplete hello a jelen témakör eljárásában, rendelkeznie kell egy Twitter-fiók, amely több hitelesített e-mail címét és telefonszámát. túl nyissa meg egy új Twitter-fiók toocreate<a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"></a>Az alkalmazás regisztrálása a Twitteren
1. Jelentkezzen be toohello [Azure-portálon], és keresse meg a tooyour alkalmazás. Másolás a **URL-cím**. A tooconfigure az Twitter-alkalmazás fogja használni.
2. Keresse meg a toohello [Twitter fejlesztők] webhelyét, jelentkezzen be Twitter-fiók hitelesítő adataival, majd kattintson a **új alkalmazás létrehozása**.
3. A típushoz hello **neve** és egy **leírása** az új alkalmazás. Illessze be az alkalmazás **URL-cím** a hello **webhely** érték. Ezt követően a hello **visszahívási URL-cím**, illessze be a hello **visszahívási URL-cím** korábban kimásolt. Ez az a mobilalkalmazás átjáró hello elérési hozzáíródik */.auth/login/twitter/callback*. Például: `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Győződjön meg arról, hogy hello HTTPS protokollt használ.
4. Hello alsó hello lapon olvassa el, és fogadja el hello feltételeit. Kattintson a **az Twitter-alkalmazás létrehozása**. A regisztereket hello app hello alkalmazás részleteit jeleníti meg.
5. Hello kattintson **beállítások** lapon jelölje **engedélyezése az alkalmazás használt toobe toosign twitterrel**, majd kattintson a **frissítési beállítások**.
6. Jelölje be hello **kulcsok és a hozzáférési jogkivonatok** fülre. Jegyezze fel a hello értékének **(API-kulcs) kulcsa** és **felhasználói titok (API titkos)**.
   
   > [!NOTE]
   > hello fogyasztói titkos kulcs egy fontos biztonsági hitelesítő adatok. Ne ossza meg senkivel ezt a titkos kulcsot, és eloszthatják azt az alkalmazással.
   > 
   > 

## <a name="secrets"></a>Információk tooyour alkalmazás Twitter hozzáadása
1. Vissza a hello [Azure-portálon], keresse meg a tooyour alkalmazás. Kattintson a **beállítások**, majd **hitelesítési / engedélyezési**.
2. Ha hello hitelesítési / engedélyezési funkció nincs engedélyezve, kapcsolja be a hello kapcsoló túl**a**.
3. Kattintson a **Twitter**. Illessze be a hello alkalmazás Azonosítóját és az alkalmazás titkos kulcs értékek, amelyek korábban beszerzett. Ezután kattintson az **OK** gombra.
   
   ![][1]
   
   Alapértelmezés szerint az App Service hitelesítést nyújt, de nem korlátozza a hitelesített hozzáférést tooyour hely tartalom és API-k. Kell engedélyeznie a felhasználók az alkalmazás kódját.
4. (Választható) toorestrict hozzáférés tooyour hely tooonly által hitelesített felhasználók Twitter, állítsa be **hitelesítetlen kérés esetén a művelet tootake** túl**Twitter**. Ehhez szükséges, hogy az összes kérelem hitelesítését, és minden nem hitelesített kérelmek átirányított tooTwitter hitelesítéshez.
5. Kattintson a **Save** (Mentés) gombra.

Most már áll készen toouse Twitter-hitelesítéshez az alkalmazásban.

## <a name="related-content"></a>Kapcsolódó tartalom
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter fejlesztők]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure-portálon]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
