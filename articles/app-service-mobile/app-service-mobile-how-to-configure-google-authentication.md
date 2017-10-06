---
title: "aaaHow tooconfigure Google hitelesítési alkalmazásszolgáltatások alkalmazás"
description: "Megtudhatja, hogyan tooconfigure Google hitelesítési alkalmazásszolgáltatások alkalmazás."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 9175c40b78c859e9e191504c41cd0bb9a3380ccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-google-login"></a>Hogyan tooconfigure App Service alkalmazás toouse Google bejelentkezési adatait
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Ez a témakör bemutatja, hogyan tooconfigure Azure App Service toouse Google hitelesítési-szolgáltatóként.

toocomplete hello a jelen témakör eljárásában, rendelkeznie kell egy hitelesített e-mail-címmel rendelkező Google-fiók. új Google-fiók toocreate lépjen túl[accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"></a>Az alkalmazás regisztrálása a Google
1. Jelentkezzen be toohello [Azure-portálon], és keresse meg a tooyour alkalmazás. Másolás a **URL-cím**, amely újabb tooconfigure a Google alkalmazás használatát.
2. Keresse meg a toohello [Google API-k](http://go.microsoft.com/fwlink/p/?LinkId=268303) webhelyét, jelentkezzen be Google fiók hitelesítő adatait, kattintson a **projekt létrehozása**, adjon meg egy **projekt neve**, kattintson a  **Hozzon létre**.
3. A **közösségi API-k** kattintson **Google + API** , majd **engedélyezése**.
4. A bal oldali navigációs, hello **hitelesítő adatok** > **OAuth-hozzájárulás képernyő**, majd válassza a **E-mail cím**, adjon meg egy **termékneve**, és kattintson a **mentése**.
5. A hello **hitelesítő adatok** lapra, majd **hitelesítő adatok létrehozása** > **OAuth-Ügyfélazonosító**, majd jelölje be **webes alkalmazás**.
6. Beillesztés hello App Service **URL-cím** másolt korábban be **jogosult JavaScript-források**, majd illessze be az átirányítási az URI **jogosult átirányítási URI-**. az átirányítási URI megadása hello URL-cím, az alkalmazás hozzáíródik hello elérési hello */.auth/login/google/callback*. Például: `https://contoso.azurewebsites.net/.auth/login/google/callback`. Győződjön meg arról, hogy hello HTTPS protokollt használ. Ezt követően kattintson a **Create** (Létrehozás) gombra.
7. A következő képernyőn hello jegyezze fel a hello értékek hello ügyfél azonosító és az ügyfél titkos kulcsot.

    > [!IMPORTANT]
    > hello ügyfélkulcs egy fontos biztonsági hitelesítő adatok. Ne ossza meg senkivel ezt a titkos kulcsot, és eloszthatják azt egy ügyfél-alkalmazással.


## <a name="secrets"></a>Hozzáadása Google információk tooyour alkalmazás
1. Vissza a hello [Azure-portálon], keresse meg a tooyour alkalmazás. Kattintson a **beállítások**, majd **hitelesítési / engedélyezési**.
2. Ha hello hitelesítési / engedélyezési funkció nincs engedélyezve, kapcsolja be a hello kapcsoló túl**a**.
3. Kattintson a **Google**. Illessze be a hello Alkalmazásazonosító és alkalmazás titkos kulcs értékét, amely korábban megszerezte, és opcionálisan engedélyezése az alkalmazás által igényelt összes hatókörök. Ezután kattintson az **OK** gombra.
   
   ![][1]
   
   Alapértelmezés szerint az App Service hitelesítést nyújt, de nem korlátozza a hitelesített hozzáférést tooyour hely tartalom és API-k. Kell engedélyeznie a felhasználók az alkalmazás kódját.
4. (Választható) toorestrict hozzáférés tooyour hely tooonly által hitelesített felhasználók a Google, állítsa be **hitelesítetlen kérés esetén a művelet tootake** túl**Google**. Ehhez szükséges, hogy az összes kérelem hitelesítését, és minden nem hitelesített kérelmek átirányított tooGoogle hitelesítéshez.
5. Kattintson a **Save** (Mentés) gombra.

Most már áll készen toouse Google-hitelesítéshez az alkalmazásban.

## <a name="related-content"></a>Kapcsolódó tartalom
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure-portálon]: https://portal.azure.com/

