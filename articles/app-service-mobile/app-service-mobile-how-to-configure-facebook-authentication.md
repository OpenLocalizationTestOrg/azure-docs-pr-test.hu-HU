---
title: "aaaHow tooconfigure Facebook hitelesítési alkalmazásszolgáltatások alkalmazás"
description: "Megtudhatja, hogyan tooconfigure Facebook hitelesítési alkalmazásszolgáltatások alkalmazás."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 53d03445a2ad17de1d2f69f5e770d14385b48ad4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-facebook-login"></a>Hogyan tooconfigure App Service alkalmazás toouse Facebook bejelentkezési adatait
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Ez a témakör bemutatja, hogyan tooconfigure Azure App Service toouse Facebook hitelesítési-szolgáltatóként.

toocomplete hello a jelen témakör eljárásában, fiókkal kell rendelkeznie a Facebook, amelyen egy ellenőrzött és érvényes e-mail címet és egy mobiltelefon számával. toocreate új Facebook-fiókkal, Ugrás túl[Facebook.com weboldalt].

## <a name="register"></a>Regisztrálhatja alkalmazását Facebook-on
1. Jelentkezzen be toohello [Azure-portálon], és keresse meg a tooyour alkalmazás. Másolás a **URL-cím**. A Facebook-alkalmazást a tooconfigure fogja használni.
2. Egy másik böngészőablakban, keresse meg a toohello [Facebook fejlesztők] webhelyet, és jelentkezzen be a Facebook-fiók hitelesítő adatait.
3. (Választható) Ha már nem regisztrált, kattintson a **alkalmazások** > **fejlesztőként nyilvántartásba**, majd fogadja el hello szabályzatot, és hello regisztrációs lépésekkel.
4. Kattintson a **alkalmazásaimat** > **fel egy új alkalmazást** > **webhely** > **kihagyhatja, és létrehozhat Alkalmazásazonosító**. 
5. A **megjelenített név**, adjon meg egy egyedi nevet az alkalmazás típusa a **kapcsolattartó E-mail**, válasszon egy **kategória** beállítása az alkalmazáshoz, majd kattintson a **Alkalmazásazonosító létrehozása**és hello biztonsági ellenőrzés. Ezzel megnyitná toohello fejlesztői irányítópult az új Facebook-alkalmazás.
6. Kattintson a "Bejelentkezés Facebook-on," **Ismerkedés**. Adja hozzá az alkalmazást **átirányítási URI-** túl**érvényes OAuth-alapú átirányítási URI-azonosítók**, majd kattintson a **módosítások mentése**. 
   
   > [!NOTE]
   > Az átirányítási URI megadása hello URL-cím, az alkalmazás hozzáíródik hello elérési */.auth/login/facebook/callback*. Például: `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Győződjön meg arról, hogy hello HTTPS protokollt használ.
   > 
   > 
7. Kattintson a bal oldali navigációs hello, **beállítások**. A hello **alkalmazás titkos kulcs** , majd kattintson **megjelenítése**, adja meg a jelszót, ha a kért, majd jegyezze fel a hello értékének **Alkalmazásazonosító** és **alkalmazás titkos kulcs**. Használja a későbbi tooconfigure az alkalmazás az Azure-ban.
   
   > [!IMPORTANT]
   > hello alkalmazás titkos kulcs egy fontos biztonsági hitelesítő adatok. Ne ossza meg senkivel ezt a titkos kulcsot, és eloszthatják azt egy ügyfél-alkalmazással.
   > 
   > 
8. hello Facebook-fiókhoz használt tooregister hello alkalmazás lépett hello alkalmazás rendszergazda. Ezen a ponton csak a rendszergazdák jelentkezhetnek be az alkalmazás. tooauthenticate más Facebook-fiókba, kattintson a **App felülvizsgálati** , és engedélyezze **< saját-alkalmazás-neve > tegyék közzé** tooenable általános hozzáférésű Facebook-hitelesítéssel.

## <a name="secrets"></a>Hozzáadása Facebook-információk tooyour alkalmazás
1. Vissza a hello [Azure-portálon], keresse meg a tooyour alkalmazás. Kattintson a **beállítások** > **hitelesítési / engedélyezési**, és győződjön meg arról, hogy **App Service hitelesítés** van **a**.
2. Kattintson a **Facebook**, illessze be a hello Alkalmazásazonosító és alkalmazás titkos kulcs értékét, amely korábban beszerzett, nem kötelezően bármely hatókörök, az alkalmazás által igényelt engedélyezése, majd kattintson **OK**.
   
    ![][0]
   
    Alapértelmezés szerint az App Service hitelesítést nyújt, de nem korlátozza a hitelesített hozzáférést tooyour hely tartalom és API-k. Kell engedélyeznie a felhasználók az alkalmazás kódját.
3. (Választható) toorestrict hozzáférés tooyour hely tooonly által hitelesített felhasználók Facebook, állítsa be **hitelesítetlen kérés esetén a művelet tootake** túl**Facebook**. Ehhez szükséges, hogy az összes kérelem hitelesítését, és minden nem hitelesített kérelmek átirányított tooFacebook hitelesítéshez.
4. Konfigurálás hitelesítési végzett, kattintson a **mentése**.

Most már áll készen toouse Facebook-hitelesítéshez az alkalmazásban.

## <a name="related-content"></a>Kapcsolódó tartalom
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook fejlesztők]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[Facebook.com weboldalt]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure-portálon]: https://portal.azure.com/
