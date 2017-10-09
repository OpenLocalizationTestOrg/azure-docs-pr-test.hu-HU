---
title: "aaaHow tooconfigure Microsoft Account hitelesítése alkalmazás App Service szolgáltatások"
description: "Megtudhatja, hogyan tooconfigure Microsoft Account hitelesítése az App Service szolgáltatások alkalmazáshoz."
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: d86d8dab26a189f4454082fc18e44e3fb6e0a01d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-microsoft-account-login"></a>Hogyan tooconfigure App Service alkalmazás toouse Microsoft Account bejelentkezési adatait
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Ez a témakör bemutatja, hogyan tooconfigure Azure App Service toouse Microsoft Account egy hitelesítésszolgáltatót. 

## <a name="register-microsoft-account"></a>Regisztrálja az alkalmazást a Microsoft-fiók
1. Jelentkezzen be toohello [Azure-portálon], és keresse meg a tooyour alkalmazás. Másolás a **URL-cím**, mely később tooconfigure az alkalmazás használatát a Microsoft Account.
2. Keresse meg a toohello [saját alkalmazások] hello Microsoft Account Developer Center weblapjára, és jelentkezzen be Microsoft-fiókjával, ha szükséges.
3. Kattintson a **hozzáadhat egy alkalmazást**, majd írja be az alkalmazás nevét, majd kattintson **alkalmazás létrehozása**.
4. Jegyezze fel a hello **Alkalmazásazonosító**, mert később szüksége lesz az. 
5. Kattintson a "Platformok," **hozzáadása Platform** válassza ki a "Web".
6. A "Átirányítási URI-k" ellátási hello végpont az alkalmazáshoz, majd kattintson **mentése**. 
   
   > [!NOTE]
   > Az átirányítási URI megadása hello URL-cím, az alkalmazás hozzáíródik hello elérési */.auth/login/microsoftaccount/callback*. Például: `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
   > Győződjön meg arról, hogy hello HTTPS protokollt használ.
   
7. Kattintson a "Titkos kulcsok alkalmazás," **új jelszó létrehozása**. Jegyezze fel a hello érték, amely akkor jelenik meg. Hello lapon adja meg, ha az nem jelenik meg újra.

    > [!IMPORTANT]
    > hello jelszava egy fontos biztonsági hitelesítő adatok. Ne ossza meg senkivel hello jelszó vagy eloszthatják azt egy ügyfél-alkalmazással.

## <a name="secrets"></a>Microsoft-fiók hozzáadása információk tooyour App Service-alkalmazás
1. Vissza a hello [Azure-portálon]tooyour alkalmazás lépjen, kattintson **beállítások** > **hitelesítési / engedélyezési**.
2. Ha hello hitelesítési / engedélyezési funkció nincs engedélyezve, kapcsolja **a**.
3. Kattintson a **Microsoft-fiók**. Illessze be a hello alkalmazás Azonosítóját és jelszavát értékek, amelyek korábban beszerzett, és opcionálisan engedélyezése az alkalmazás által igényelt összes hatókörök. Ezután kattintson az **OK** gombra.
   
    ![][1]
   
    Alapértelmezés szerint az App Service hitelesítést nyújt, de nem korlátozza a hitelesített hozzáférést tooyour hely tartalom és API-k. Kell engedélyeznie a felhasználók az alkalmazás kódját.
4. (Választható) toorestrict hozzáférés tooyour hely tooonly által hitelesített felhasználók Microsoft-fiók beállítása **hitelesítetlen kérés esetén a művelet tootake** túl**Microsoft Account**. Ehhez szükséges, hogy az összes kérelem hitelesítését, és a rendszer az összes nem hitelesített kérelmek hitelesítés tooMicrosoft fiókját.
5. Kattintson a **Save** (Mentés) gombra.

Most már áll készen toouse Microsoft Account hitelesítéshez az alkalmazásban.

## <a name="related-content"></a>Kapcsolódó tartalom
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[saját alkalmazások]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure-portálon]: https://portal.azure.com/
