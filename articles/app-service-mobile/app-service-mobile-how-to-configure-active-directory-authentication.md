---
title: "aaaHow tooconfigure Azure Active Directory hitelesítési alkalmazásszolgáltatások alkalmazás"
description: "Megtudhatja, hogyan tooconfigure Azure Active Directory hitelesítési alkalmazásszolgáltatások alkalmazás."
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 5b1d73f8fdf5831a266d900cd07f4e3b0917a7f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-azure-active-directory-login"></a>Hogyan tooconfigure App Service alkalmazás toouse Azure Active Directory bejelentkezési adatait
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Ez a témakör bemutatja, hogyan tooconfigure Azure App Service szolgáltatások toouse Azure Active Directory hitelesítési-szolgáltatóként.

## <a name="express"></a>Azure Active Directory konfigurálása gyorsbeállítások használatával
1. A hello [Azure-portálon], keresse meg a tooyour alkalmazás. Kattintson a **beállítások**, majd **hitelesítési/engedélyezési**.
2. Ha hello hitelesítési / engedélyezési funkció nincs engedélyezve, kapcsolja be a hello kapcsoló túl**a**.
3. Kattintson a **Azure Active Directory**, és kattintson a **Express** alatt **üzemmód**.
4. Kattintson a **OK** tooregister hello alkalmazás Azure Active Directoryban. Ezzel létrehoz egy új regisztrációs. Ha ehelyett toochoose levő meglévő regisztrációval, kattintson **válasszon ki egy meglévő alkalmazást** majd keresse meg a korábban létrehozott regisztrációs belül a bérlő hello nevét.
   Kattintson a hello regisztrációs tooselect, azt, és kattintson **OK**. Kattintson a **OK** a hello Azure Active Directory-beállítások panelen.
   
   ![][0]
   
   Alapértelmezés szerint az App Service hitelesítést nyújt, de nem korlátozza a hitelesített hozzáférést tooyour hely tartalom és API-k. Kell engedélyeznie a felhasználók az alkalmazás kódját.
5. (Választható) toorestrict hozzáférés tooyour hely tooonly felhasználók az Azure Active Directory, hitelesített meg **hitelesítetlen kérés esetén a művelet tootake** túl**bejelentkezés Azure Active Directoryval**. Ehhez szükséges, hogy az összes kérelem hitelesítését, és minden nem hitelesített kérelmek átirányított tooAzure Active Directory-hitelesítéshez.
6. Kattintson a **Save** (Mentés) gombra.

Most már áll készen toouse Azure Active Directory-hitelesítéshez az alkalmazásban.

## <a name="advanced"></a>(Alternatív módszer) manuális konfigurálása az Azure Active Directory speciális beállításokkal
Másik lehetőségként tooprovide konfigurációs beállításokat manuálisan. Ez az előnyben részesített hello megoldás, ha a hello AAD-bérlőt toouse kívánja eltér, amellyel jelentkezzen be Azure hello bérlő. toocomplete hello konfigurációs, akkor először létre kell hoznia egy regisztrációs az Azure Active Directoryban, és meg kell adni az egyes hello regisztrációs részletek tooApp szolgáltatás.

### <a name="register"></a>Regisztrálhatja alkalmazását Azure Active Directoryban
1. Jelentkezzen be toohello [Azure-portálon], és keresse meg a tooyour alkalmazás. Másolás a **URL-cím**. A tooconfigure az Azure Active Directory-alkalmazás fogja használni.
2. Jelentkezzen be toohello [a klasszikus Azure portálon] , és keresse meg a túl**Active Directory**.
   
    ![][2]
3. Válassza ki azt a címtárat, majd válassza ki a hello **alkalmazások** hello felső fülre. Kattintson a **hozzáadása** : hello alsó toocreate egy új alkalmazás regisztrációja.
4. Kattintson a **a szerveztem által fejlesztett alkalmazás hozzáadása**.
5. A hello hozzáadása varázsló, adjon meg egy **neve** a az alkalmazás, és kattintson hello **Web Application And/Or Web API** típusa. Kattintson a toocontinue.
6. A hello **SIGN-ON URL** mezőbe illessze be a hello alkalmazás URL-címet korábban kimásolt. Adja meg, hogy azonos URL-CÍMMEL hello **App ID URI** mezőbe. Kattintson a toocontinue.
7. Ha hello alkalmazáshoz hozzáadott, kattintson a hello **konfigurálása** fülre. Hello szerkesztése **válasz URL-CÍMEN** alatt **egyszeri bejelentkezés** toobe hello alkalmazás URL-CÍMÉT a hello elérési hozzáíródik */.auth/login/aad/callback*. Például: `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Győződjön meg arról, hogy hello HTTPS protokollt használ.
   
    ![][3]
8. Kattintson a **Save** (Mentés) gombra. Ezután másolási hello **ügyfél-azonosító** hello alkalmazás. Az alkalmazás toouse konfigurálja ezt később.
9. Hello alsó parancssávon kattintson **végpontok megtekintése**, és ezután másolási hello **összevonási metaadat-dokumentum** URL-cím és dokumentálása, vagy keresse meg a böngészőben tooit letölthető.
10. Hello gyökérben **EntityDescriptor** elem, nem kell egy **entityid beállítást** hello űrlap attribútum `https://sts.windows.net/` egy GUID adott tooyour bérlő (más néven "bérlő azonosítója") követ. Másolja ezt az értéket – fog szolgálni a **kiállítójának URL-címe**. Az alkalmazás toouse konfigurálja ezt később.

### <a name="secrets"></a>Azure Active Directory hozzáadása információk tooyour alkalmazás
1. Vissza a hello [Azure-portálon], keresse meg a tooyour alkalmazás. Kattintson a **beállítások**, majd **hitelesítési/engedélyezési**.
2. Ha hello hitelesítési/engedélyezési funkció nincs engedélyezve, kapcsolja be az hello kapcsoló túl**a**.
3. Kattintson a **Azure Active Directory**, és kattintson a **speciális** alatt **üzemmód**. Illessze be hello ügyfél-azonosító és a kiállítói URL-címe mely beszerzett értékét korábban. Ezután kattintson az **OK** gombra.
   
   ![][1]
   
   Alapértelmezés szerint az App Service hitelesítést nyújt, de nem korlátozza a hitelesített hozzáférést tooyour hely tartalom és API-k. Kell engedélyeznie a felhasználók az alkalmazás kódját.
4. (Választható) toorestrict hozzáférés tooyour hely tooonly felhasználók az Azure Active Directory, hitelesített meg **hitelesítetlen kérés esetén a művelet tootake** túl**bejelentkezés Azure Active Directoryval**. Ehhez szükséges, hogy az összes kérelem hitelesítését, és minden nem hitelesített kérelmek átirányított tooAzure Active Directory-hitelesítéshez.
5. Kattintson a **Save** (Mentés) gombra.

Most már áll készen toouse Azure Active Directory-hitelesítéshez az alkalmazásban.

## <a name="optional-configure-a-native-client-application"></a>(Választható) Natív ügyfélalkalmazás konfigurálása
Az Azure Active Directory is lehetővé teszi tooregister natív ügyfelek, amely nagyobb mértékben vezérelheti a leképezési engedélyeket biztosít. Ez szükséges Ha hello például a szalagtárat használó tooperform bejelentkezések **Active Directory Authentication Library**.

1. Keresse meg a túl**Active Directory** a hello [a klasszikus Azure portálon].
2. Válassza ki azt a címtárat, majd válassza ki a hello **alkalmazások** hello felső fülre. Kattintson a **hozzáadása** : hello alsó toocreate egy új alkalmazás regisztrációja.
3. Kattintson a **a szerveztem által fejlesztett alkalmazás hozzáadása**.
4. A hello hozzáadása varázsló, adjon meg egy **neve** a az alkalmazás, és kattintson hello **natív ügyfélalkalmazás** típusa. Kattintson a toocontinue.
5. A hello **átirányítási URI-** mezőbe írja be a webhely */.auth/login/done* végpont hello HTTPS protokollt használ. Ez az érték túl hasonló legyen*https://contoso.azurewebsites.net/.auth/login/done*. Ha olyan Windows-alkalmazás létrehozása, inkább hello [csomag biztonsági azonosítója](app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) , hello URI.
6. Miután hozzáadta a hello natív alkalmazás, kattintson a hello **konfigurálása** fülre. Hello található **ügyfél-azonosító** és jegyezze fel ezt az értéket.
7. Görgessen hello PgDn toohello **engedélyek tooother alkalmazások** szakaszt, és kattintson **alkalmazás hozzáadása**.
8. Keresse meg a korábban regisztrált hello webalkalmazás, majd kattintson a hello plusz ikonra. Kattintson a hello ellenőrzés tooclose hello párbeszédpanel. Ha hello webes alkalmazás nem található tooits regisztrációs váltson, és adja hozzá egy új válasz URL-cím (pl. hello HTTP verziója az aktuális URL-cím), kattintson a Mentés gombra, és ismételje meg ezeket a lépéseket – hello alkalmazás kell jelenik meg hello listában.
9. A hello új bejegyzést az előzőekben adott hozzá, nyissa meg a hello **delegált engedélyek** legördülő menüből válassza ki **Access (appName)**. Ezután kattintson a **Save** (Mentés) gombra.

Ezzel beállította a natív ügyfélalkalmazást, amely hozzáfér az App Service alkalmazás.

## <a name="related-content"></a>Kapcsolódó tartalom
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-express-settings.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-advanced-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-configure.png

<!-- URLs. -->

[Azure-portálon]: https://portal.azure.com/
[a klasszikus Azure portálon]: https://manage.windowsazure.com/
[alternative method]:#advanced
