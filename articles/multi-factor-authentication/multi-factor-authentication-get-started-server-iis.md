---
title: "Hitelesítés és az Azure MFA kiszolgáló aaaIIS |} Microsoft Docs"
description: "Ez a hello Azure multi-factor authentication oldal, amely segítséget nyújt az IIS-hitelesítés és az Azure multi-factor Authentication kiszolgáló üzembe helyezése."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d1bf1c8a-2c10-4ae6-9f4b-75f0c3df43eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/16/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017,it-pro
ms.openlocfilehash: 74bd39c2644e2bca0880baea3824cad4c9215111
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-multi-factor-authentication-server-for-iis-web-apps"></a>Az Azure Multi-Factor Authentication-kiszolgáló konfigurálása IIS-webalkalmazásokhoz

Hello Azure multi-factor Authentication (MFA) kiszolgáló tooenable szakasza hello IIS-hitelesítés használatára, és az integráció az IIS-hitelesítés konfigurálása a Microsoft IIS-webalkalmazások. hello Azure MFA kiszolgáló telepíti a beépülő modul, amely szűrheti kérések toohello IIS web server tooadd Azure multi-factor Authentication hitelesítést végeznek. hello IIS beépülő modul – űrlapalapú hitelesítés és az integrált Windows HTTP-hitelesítést támogatja. Megbízható IP-címek is lehet konfigurált tooexempt belső IP-címeket a kéttényezős hitelesítés.

![IIS-hitelesítés](./media/multi-factor-authentication-get-started-server-iis/iis.png)

## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>Az űrlapalapú IIS-hitelesítés használata az Azure Multi-Factor Authentication-kiszolgálóval
toosecure webes űrlapalapú hitelesítést használó alkalmazás, hello IIS webkiszolgálón hello Azure multi-factor Authentication kiszolgáló telepítése és konfigurálása az IIS kiszolgáló hello eljárást követő hello száma:

1. Hello Azure multi-factor Authentication kiszolgáló kattintson hello IIS-hitelesítés ikonra a bal oldali menü hello.
2. Kattintson a hello **űrlapalapú** fülre.
3. Kattintson az **Add** (Hozzáadás) parancsra.
4. toodetect felhasználónév, jelszó és tartomány változók automatikusan, hello bejelentkezési URL-címe (például https://localhost/contoso/auth/login.aspx) hello űrlapalapú webhely párbeszédpanelen adja meg, majd kattintson **OK**.
5. Ellenőrizze a hello **multi-factor Authentication felhasználói egyeztetés megkövetelése** mezőben, ha az összes felhasználót törölték, vagy importálja a hello kiszolgáló és a tulajdonos toomulti kétfaktoros hitelesítést. Ha a felhasználók jelentős számú nem még importálva lettek hello kiszolgálón és/vagy a multi-factor Authentication hitelesítés alól, hagyja bejelölve hello mezőt.
6. Ha hello oldalváltozók automatikusan nem észlelhető, kattintson a **adja meg manuálisan** hello űrlapalapú webhely párbeszédpanelen.
7. Hello űrlapalapú webhely párbeszédpanelen adja meg a hello URL-cím toohello bejelentkezési oldal hello Eseménybeküldési URL-cím mezőben, és adja meg az alkalmazás nevét (nem kötelező). hello alkalmazásnév Azure multi-factor Authentication-jelentésekben jelenik meg, és előfordulhat, hogy SMS vagy mobilalkalmazás hitelesítési üzenet jelenjen meg.
8. Válassza ki a hello megfelelő kérés formátumát. A beállított érték túl**POST vagy GET** legtöbb webes alkalmazásokhoz.
9. Adja meg a hello felhasználónév változót, a jelszó változót és a tartomány változó (Ha megjelenik hello bejelentkezési oldal). hello toofind hello nevei beviteli mezőbe, keresse meg a toohello bejelentkezési oldalt egy webböngészőben, hello lapon kattintson a jobb gombbal, és válassza ki **forrás megtekintése**.
10. Ellenőrizze a hello **Azure multi-factor Authentication felhasználói egyeztetés megkövetelése** mezőben, ha az összes felhasználót törölték, vagy importálja a hello kiszolgáló és a tulajdonos toomulti kétfaktoros hitelesítést. Ha a felhasználók jelentős számú nem még importálva lettek hello kiszolgálón és/vagy a multi-factor Authentication hitelesítés alól, hagyja bejelölve hello mezőt.
11. Kattintson a **speciális** tooreview speciális beállításai, többek között:

  - Egyéni elutasítási oldalfájl kiválasztása
  - Gyorsítótár sikeres hitelesítések toohello webhelye számára egy adott időn belül cookie-k használata
  - Adja meg, hogy tooauthenticate hello elsődleges hitelesítő adatok egy Windows-tartomány, LDAP-címtárhoz. vagy RADIUS-kiszolgálón hitelesíti.

12. Kattintson a **OK** tooreturn toohello űrlapalapú webhely párbeszédpanelen.
13. Kattintson az **OK** gombra.
14. Egyszer hello URL-cím és oldalváltozók észlelt vagy a megadott, hello űrlapalapú panel hello webhelyek adatainak megjelenítése.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Integrált Windows IIS-hitelesítés használata az Azure Multi-Factor Authentication-kiszolgálóval
toosecure az IIS webes integrált Windows-HTTP-hitelesítést használó alkalmazás, hello Azure MFA kiszolgáló hello IIS webkiszolgálóra telepítse, majd konfigurálja az alábbi lépésekkel hello Server hello:

1. Hello Azure multi-factor Authentication kiszolgáló kattintson hello IIS-hitelesítés ikonra a bal oldali menü hello.
2. Kattintson a hello **HTTP** fülre.
3. Kattintson az **Add** (Hozzáadás) parancsra.
4. Hello alap URL-cím hozzáadása párbeszédpanelt adja meg a hello URL-cím hello webhely, ahol a HTTP-hitelesítést (például http://localhost/owa) történik, és adja meg az alkalmazás neve (nem kötelező). hello alkalmazásnév Azure multi-factor Authentication-jelentésekben jelenik meg, és előfordulhat, hogy SMS vagy mobilalkalmazás hitelesítési üzenet jelenjen meg.
5. Állítsa be úgy hello időtúllépést és a maximális munkamenet időtúllépés Ha hello alapértelmezett nem elegendő.
6. Ellenőrizze a hello **multi-factor Authentication felhasználói egyeztetés megkövetelése** mezőben, ha az összes felhasználót törölték, vagy importálja a hello kiszolgáló és a tulajdonos toomulti kétfaktoros hitelesítést. Ha a felhasználók jelentős számú nem még importálva lettek hello kiszolgálón és/vagy a multi-factor Authentication hitelesítés alól, hagyja bejelölve hello mezőt.
7. Ellenőrizze a hello **Cookie-gyorsítótár** mezőben, ha szükséges.
8. Kattintson az **OK** gombra.

## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>IIS beépülő modulok engedélyezése az Azure Multi-Factor Authentication-kiszolgálón
Hello konfigurálása után a űrlapalapú vagy HTTP-hitelesítési URL-címeket és beállításokat, jelölje be a hello helyét, ahol hello Azure multi-factor Authentication IIS beépülő modulok kell betölteni és az IIS Szolgáltatásban. A következő eljárás hello használata:

1. Ha fut az IIS 6-os, kattintson a hello **ISAPI** fülre. Jelölje be hello webhelyet, ahol a webes alkalmazás hello fut (pl. Default Web Site) tooenable hello Azure multi-factor Authentication ISAPI-szűrő beépülő modul az adott helyhez.
2. Ha az IIS 7-es vagy újabb rendszer fut, kattintson a hello **natív modul** fülre. Válassza ki a hello server, a webhelyek vagy alkalmazások tooenable hello IIS beépülő modul szükséges hello szinten.
3. Kattintson a hello **engedélyezése az IIS-hitelesítés** mezőt üdvözlő képernyőt hello tetején. Az Azure multi-factor Authentication most biztonságossá tétele a kiválasztott hello IIS-alkalmazás. Győződjön meg arról, hogy felhasználók importálva lettek hello kiszolgáló.

## <a name="trusted-ips"></a>Megbízható IP-címek
hello megbízható IP-címek lehetővé teszi a felhasználók toobypass Azure multi-factor Authentication az adott IP-címek és alhálózatok címekről érkező webhelykérelmek esetén. Például érdemes lehet tooexempt felhasználók a Azure multi-factor Authentication hello irodából a bejelentkezés során. Ehhez a hello irodai alhálózatot megbízható IP-címek bejegyzésének kellene megadnia. tooconfigure megbízható IP-címek, a következő eljárás hello használata:

1. Az IIS-hitelesítés szakasz hello, kattintson a hello **megbízható IP-címek** fülre.
2. Kattintson az **Add** (Hozzáadás) parancsra.
3. Hello megbízható IP hozzáadása párbeszédpanel megjelenésekor, válassza ki a hello **egyetlen IP-cím**, **IP-címtartomány**, vagy **alhálózati** választógombot.
4. Adja meg a hello IP-cím, az IP-címet vagy alhálózatot, amely szerepel az engedélyezési listán kell lennie. Ha egy alhálózat, jelölje be hello megadása a megfelelő hálózati maszkot, majd kattintson **OK**. hello engedélyezett most már hozzá lett adva.
