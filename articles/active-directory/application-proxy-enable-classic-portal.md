---
title: "a klasszikus portálon hello Azure AD alkalmazásproxy aaaEnable |} Microsoft Docs"
description: "A klasszikus Azure portálon hello alkalmazásproxyt kapcsolni, és az hello fordított proxy hello összekötők telepítése."
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
ms.date: 07/02/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 8be9416a61993e1b46a20152e172c5133e54c0a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-application-proxy-in-hello-classic-portal-and-download-connectors"></a>Alkalmazásproxy engedélyezése az hello klasszikus portálra, és töltse le az összekötők
Ez a cikk bemutatja, hogyan hello lépéseket tooenable Microsoft Azure AD-alkalmazásproxy a felhőalapú címtárban az Azure ad-ben.

Ha nem ismeri a mi alkalmazásproxy is segítséget nyújthat, további információ [hogyan tooprovide biztonságos távoli hozzáférés tooon helyszíni alkalmazások](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Az alkalmazásproxy használatának előfeltételei
Engedélyezi, és az alkalmazásproxy szolgáltatásainak használatára, toohave kell:

* [Microsoft Azure AD Prémium vagy Alapszintű előfizetés](active-directory-editions.md) és egy globális rendszergazdaként használt Azure AD-címtár.
* Egy Windows Server 2012 R2 vagy 2016, amelyre telepítheti az alkalmazásproxy-összekötő hello futtató kiszolgáló. hello server kérelmeket küld az alkalmazásproxy szolgáltatásainak toohello hello felhőben, ezért egy HTTP vagy HTTPS kapcsolat toohello alkalmazásokat tesz közzé az.
  * Egyszeri bejelentkezés tooyour a közzétett alkalmazások, a gép kell a tartományhoz hello ugyanazt az AD-tartomány, amely közzéteszi a hello alkalmazásokként. További információ: [egyszeri bejelentkezéshez az alkalmazásproxy](active-directory-application-proxy-sso-using-kcd.md)
* Ha a szervezet használja a proxy kiszolgálók tooconnect toohello internetes, olvasási [együttműködnek a meglévő helyszíni proxykiszolgálókat](application-proxy-working-with-proxy-servers.md) kapcsolatos részletes tudnivalókért tooconfigure őket.

## <a name="open-your-ports"></a>A portok megnyitása

tooprepare a környezet az Azure AD-alkalmazásproxy, először tooenable kommunikációs tooAzure adatközpontokban. Ha van tűzfal a hello elérési útja, győződjön meg arról, hogy meg nyitva, az összekötő el tudja küldeni HTTPS (TCP), hogy hello kérelmek toohello alkalmazásproxy.

1. Nyissa meg hello alábbi port túl**kimenő** forgalmat:

   | Portszám | Hogyan használja fel azokat |
   | --- | --- |
   | 80 | Letölti a tanúsítvány-visszavonási listákat (CRL) hello SSL-tanúsítvány érvényesítése közben |
   | 443 | Minden kimenő kommunikáció hello alkalmazásproxy-szolgáltatás |

   Ha a tűzfal szabályozza az adatforgalmat toooriginating felhasználók szerint, nyissa meg ezeket a portokat, a hálózati szolgáltatásként futó Windows-szolgáltatások adatforgalma számára.

   > [!IMPORTANT]
   > hello táblázat által adott jelentéseket tükrözik a connector verziók 1.5.132.0 hello portokra vonatkozó követelmények és újabb. Ha továbbra is összekötő régebbi verziójú, szükség tooenable hello a következő portokat: 5671, 8080-as, 9090, 9091, 9350, 9352 és 10100 – 10120.
   >
   >Az összekötők toohello legújabb verzióra frissítésével kapcsolatos információkért lásd: [megértéséhez Azure AD-alkalmazásproxy összekötők](application-proxy-understand-connectors.md#automatic-updates).

2. Ha a tűzfal vagy a proxy lehetővé teszi a DNS engedélyezett, akkor az engedélyezett kapcsolatok toomsappproxy.net és servicebus.windows.net. Ha nem, akkor kell tooallow hozzáférés toohello [Azure DataCenter IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653), amely minden héten frissítése.

3. Használjon hello [az Azure AD Application Proxy Connector portok teszt eszközét](https://aadap-portcheck.connectorporttest.msappproxy.net/) tooverify, hogy az összekötő érni hello alkalmazásproxy-szolgáltatás. Minimális győződjön meg arról, hogy hello központi US régió és hello régió legközelebbi tooyou összes zöld jelölők. Túl további zöld jelölők azt jelenti, hogy nagyobb rugalmasság.

## <a name="enable-application-proxy-in-azure-ad"></a>Az Azure AD alkalmazásproxy engedélyezése
1. Jelentkezzen be rendszergazdaként a hello [a klasszikus Azure portálon](https://manage.windowsazure.com/).
2. Nyissa meg tooActive könyvtár, és válassza ki a kívánt tooenable proxyval hello könyvtár.

    ![Active Directory – ikon](./media/active-directory-application-proxy-enable/ad_icon.png)
3. Válassza ki **konfigurálása** hello directory lapon görgessen lefelé, túl**alkalmazásproxy**.
4. Váltás **Application Proxy szolgáltatások engedélyezése ehhez a könyvtárhoz** túl**engedélyezve**.

    ![Alkalmazásproxy engedélyezése](./media/active-directory-application-proxy-enable/app_proxy_enable.png)
5. Válassza a **Download now** (Letöltés most) lehetőséget. Hello **az Azure AD Application Proxy Connector letöltése** nyílik meg. Olvassa el és fogadja el a licencfeltételeket hello, és kattintson **letöltése** toosave hello Windows Installer-fájl (.exe) hello összekötő.

## <a name="install-and-register-hello-connector"></a>Telepítse és regisztrálja a hello összekötő
1. Futtatás **AADApplicationProxyConnectorInstaller.exe** hello kiszolgálón függően toohello Előfeltételek előkészítve.
2. Kövesse a varázsló tooinstall hello hello utasításait.
3. A telepítés során az Azure AD-bérlő Alkalmazásproxyjával hello felszólító tooregister hello összekötő áll.

   * Adja meg Azure AD globális rendszergazdai hitelesítő adatait. A globális rendszergazdai bérlő adatai eltérhetnek a Microsoft Azure rendszerre vonatkozó hitelesítő adatoktól.
   * Ellenőrizze, hogy Üdvözöljük a rendszergazdákat, akik hello összekötő regiszterekben hello könyvtárába, amelyben engedélyezve hello alkalmazásproxy-szolgáltatás. Például, ha hello bérlő tartománya a contoso.com, Üdvözöljük a rendszergazdákat legyen admin@contoso.com vagy bármely más, adott tartománybeli alias.
   * Ha **Internet Explorer fokozott biztonsági beállítások** értéke túl**a** hello kiszolgálón hello regisztrációs képernyő blokkolhatja. tooallow hozzáférést, kövesse az utasításokat hello hello hibaüzenet. Győződjön meg arról, hogy az Internet Explorer fokozott biztonsági beállításai ki vannak kapcsolva.
   * Ha az összekötő regisztrálása meghiúsul, tekintse meg a [Troubleshoot Application Proxy](active-directory-application-proxy-troubleshoot.md) (Alkalmazásproxy hibaelhárítása) című anyagot.  
4. Hello telepítésének befejezése után két új szolgáltatással bővül tooyour kiszolgáló:

   * a **Microsoft AAD alkalmazásproxy-összekötő** a kapcsolódást engedélyezi;

     * **Microsoft AAD Alkalmazásproxyösszekötő** az automatikus frissítési szolgáltatása. Rendszeres időközönként ellenőrzi hello connector új verziója, és frissíti a hello összekötő, igény szerint.

     ![Az alkalmazásproxy összekötőjének szolgáltatásai – képernyőfelvétel](./media/active-directory-application-proxy-enable/app_proxy_services.png)
5. Kattintson a **Befejezés** hello telepítési ablakban.

Az összekötőkre vonatkozó további információkat az [Azure AD-alkalmazásproxy összekötőit ismertető](application-proxy-understand-connectors.md) cikk tartalmaz.

A magas szintű rendelkezésre állás érdekében legalább kettő összekötőt üzembe kell helyeznie. toodeploy további összekötők, ismételje meg a 2. és 3. Minden egyes összekötőt külön kell regisztrálni.

Ha azt szeretné, hogy toouninstall hello összekötőt, távolítsa el a hello összekötő szolgáltatást és a hello frissítési szolgáltatást. Indítsa újra a számítógépet toofully hello szolgáltatást.

## <a name="next-steps"></a>Következő lépések
Most már készen áll túl[alkalmazások közzétételére az alkalmazásproxy](active-directory-application-proxy-publish.md).

Ha olyan alkalmazásokkal rendelkezik, különálló hálózatokból vagy különböző helyeken lévő, logikai egységek összekötő csoportok tooorganize hello különböző összekötők is használhatja. További információ: [Az alkalmazásproxy-összekötők használata](active-directory-application-proxy-connectors.md).
