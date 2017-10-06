---
title: "aaaAzure AD alkalmazás-Proxy – első lépések-összekötő telepítése |} Microsoft Docs"
description: "Kapcsolja be az alkalmazásproxyt hello Azure-portálon, és hello fordított proxy hello összekötők telepítése."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: c7186f98-dd80-4910-92a4-a7b8ff6272b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: ea79ffa92fa223584be04f49019fd31a0bfd8358
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-application-proxy-and-install-hello-connector"></a>Az alkalmazásproxy első lépései és hello összekötő telepítése
Ez a cikk bemutatja, hogyan hello lépéseket tooenable Microsoft Azure AD-alkalmazásproxy a felhőalapú címtárban az Azure ad-ben.

Ha Ön még nem kompatibilis hello biztonsági és a termelékenység előnyök alkalmazásproxy számos lehetőséget kínál tooyour szervezet, további információ [hogyan tooprovide biztonságos távoli hozzáférés tooon helyszíni alkalmazások](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Az alkalmazásproxy használatának előfeltételei
Engedélyezi, és az alkalmazásproxy szolgáltatásainak használatára, toohave kell:

* [Microsoft Azure AD Prémium vagy Alapszintű előfizetés](active-directory-editions.md) és egy globális rendszergazdaként használt Azure AD-címtár.
* Egy Windows Server 2012 R2 vagy 2016, amelyre telepítheti az alkalmazásproxy-összekötő hello futtató kiszolgáló. hello kiszolgálónak el kell tudnia toobe képes tooconnect toohello az alkalmazásproxy szolgáltatásainak hello felhőben, és a hello helyszíni alkalmazásokat, amelyek tesznek közzé.
  * Az egyszeri bejelentkezés tooyour közzétett alkalmazások Kerberos által korlátozott delegálás használata ezen a számítógépen kell a tartományhoz hello ugyanazt az AD-tartomány, amely közzéteszi a hello alkalmazásokként. További információ: [Kerberos által korlátozott Delegálás az egyszeri bejelentkezés az alkalmazásproxy](active-directory-application-proxy-sso-using-kcd.md).

Ha a szervezet használja a proxy kiszolgálók tooconnect toohello internet, olvassa el [együttműködnek a meglévő helyszíni proxykiszolgálókat](application-proxy-working-with-proxy-servers.md) talál részletes információt hogyan tooconfigure őket, mielőtt újra be az alkalmazásproxy elindult.

## <a name="open-your-ports"></a>A portok megnyitása

tooprepare a környezet az Azure AD-alkalmazásproxy, először tooenable kommunikációs tooAzure adatközpontokban. Ha van tűzfal a hello elérési útja, győződjön meg arról, hogy meg nyitva, az összekötő el tudja küldeni HTTPS (TCP), hogy hello kérelmek toohello alkalmazásproxy.

1. Nyissa meg hello alábbi port túl**kimenő** forgalmat:

   | Portszám | Hogyan használja fel azokat |
   | --- | --- |
   | 80 | Letölti a tanúsítvány-visszavonási listákat (CRL) hello SSL-tanúsítvány érvényesítése közben |
   | 443 | Minden kimenő kommunikáció hello alkalmazásproxy-szolgáltatás |

   Ha a tűzfal szabályozza az adatforgalmat toooriginating felhasználók szerint, nyissa meg ezeket a portokat, a forgalom hálózati szolgáltatásként futó Windows-szolgáltatások.

   > [!IMPORTANT]
   > hello táblázat által adott jelentéseket tükrözik a connector verziók 1.5.132.0 hello portokra vonatkozó követelmények és újabb. Továbbra is fennáll összekötő régebbi verziójú, is szükség van a következő portokat a hozzáadása too80 és a 443-as: 5671, a 8080-as, 9090-9091, 9350, tooenable hello 9352, 10100 – 10120.
   >
   >Az összekötők toohello legújabb verzióra frissítésével kapcsolatos információkért lásd: [megértéséhez Azure AD-alkalmazásproxy összekötők](application-proxy-understand-connectors.md#automatic-updates).

2. Ha a tűzfal vagy a proxy lehetővé teszi a DNS engedélyezett, akkor az engedélyezett kapcsolatok toomsappproxy.net és servicebus.windows.net. Ha nem, akkor kell tooallow hozzáférés toohello [Azure DataCenter IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653), amely minden héten frissítése.

3. Microsoft négy címek tooverify tanúsítványokat használ. Ha egyéb termékek esetében még nem tette a következő URL-címek hozzáférési toohello engedélyezése:
   * mscrl.microsoft.com:80
   * CRL.microsoft.com:80
   * OCSP.msocsp.com:80
   * www.microsoft.com:80

4. Az összekötő kell hozzáférést toologin.windows.net és login.microsoftonline.net hello-regisztrációs folyamathoz.

5. Használjon hello [az Azure AD Application Proxy Connector portok teszt eszközét](https://aadap-portcheck.connectorporttest.msappproxy.net/) tooverify, hogy az összekötő érni hello alkalmazásproxy-szolgáltatás. Minimális győződjön meg arról, hogy hello központi US régió és hello régió legközelebbi tooyou összes zöld jelölők. Túl további zöld jelölők azt jelenti, hogy nagyobb rugalmasság.

## <a name="install-and-register-a-connector"></a>Telepítse és regisztrálja az összekötőhöz
1. Jelentkezzen be rendszergazdaként a hello [Azure-portálon](https://portal.azure.com/).
2. A jelenlegi címtárral megjelenik a felhasználónév: hello jobb felső sarokban található. Ha toochange könyvtárak van szüksége, válassza ki az erre az ikonra.
3. Nyissa meg túl**Azure Active Directory** > **alkalmazásproxy**.

   ![Keresse meg a Proxy tooApplication](./media/active-directory-application-proxy-enable/app_proxy_navigate.png)

4. Válassza ki **összekötő letöltése**.

   ![Összekötő letöltése](./media/active-directory-application-proxy-enable/download_connector.png)

5. Futtatás **AADApplicationProxyConnectorInstaller.exe** hello kiszolgálón függően toohello Előfeltételek előkészítve.
6. Kövesse a varázsló tooinstall hello hello utasításait. A telepítés során az Azure AD-bérlő Alkalmazásproxyjával hello felszólító tooregister hello összekötő áll.

   * Adja meg Azure AD globális rendszergazdai hitelesítő adatait. A globális rendszergazdai bérlő adatai eltérhetnek a Microsoft Azure rendszerre vonatkozó hitelesítő adatoktól.
   * Ellenőrizze, hogy Üdvözöljük a rendszergazdákat, akik hello összekötő regiszterekben hello könyvtárába, amelyben engedélyezve hello alkalmazásproxy-szolgáltatás. Például, ha hello bérlő tartománya a contoso.com, Üdvözöljük a rendszergazdákat legyen admin@contoso.com vagy bármely más, adott tartománybeli alias.
   * Ha **Internet Explorer fokozott biztonsági beállítások** értéke túl**a** hello összekötő telepítéséhez hello kiszolgálón nem láthatók hello regisztrációs képernyő. tooget hozzáférést, kövesse az utasításokat hello hello hibaüzenet. Győződjön meg arról, hogy az Internet Explorer fokozott biztonsági beállításai ki vannak kapcsolva.

A magas szintű rendelkezésre állás érdekében legalább kettő összekötőt üzembe kell helyeznie. Minden egyes összekötőt külön kell regisztrálni.

## <a name="test-that-hello-connector-installed-correctly"></a>Tesztelje, hogy megfelelően telepítve hello összekötő

Ellenőrizheti, hogy egy új összekötő azzal, hogy vagy hello Azure-portálon vagy a kiszolgálón megfelelően telepítve. 

Az Azure-portálon hello, tooyour bérlői bejelentkezés, és keresse meg a túl**Azure Active Directory** > **alkalmazásproxy**. Az összes összekötőt és összekötő csoportok jelennek meg ezen a lapon. Válasszon ki egy összekötő toosee a részletek, vagy helyezze át egy másik összekötő csoportot. 

A kiszolgálón ellenőrizze a aktív szolgáltatások hello összekötő és hello összekötő frissítőjének hello listáját. két hello szolgáltatást kell azonnal futtatni, de ha nem, kapcsolhatja be azokat: 

   * a **Microsoft AAD alkalmazásproxy-összekötő** a kapcsolódást engedélyezi;

   * **Microsoft AAD Alkalmazásproxyösszekötő** az automatikus frissítési szolgáltatása. hello frissítőjének keres hello connector új verziója, és a frissítések hello összekötő igény szerint.

   ![Az alkalmazásproxy összekötőjének szolgáltatásai – képernyőfelvétel](./media/active-directory-application-proxy-enable/app_proxy_services.png)

Az összekötők, és hogyan toodate addig maradnak kapcsolatos információkért lásd: [megértéséhez Azure AD-alkalmazásproxy összekötők](application-proxy-understand-connectors.md).


## <a name="next-steps"></a>Következő lépések
Most már készen áll túl[alkalmazások közzétételére az alkalmazásproxy](application-proxy-publish-azure-portal.md).

Ha különálló hálózattal vagy különböző helyeken lévő alkalmazások, használja a összekötő csoportok tooorganize hello különböző összekötők a logikai egységeket. További információ: [Az alkalmazásproxy-összekötők használata](active-directory-application-proxy-connectors-azure-portal.md).
