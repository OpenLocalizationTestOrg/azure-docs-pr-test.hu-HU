---
title: "aaaUnderstand az Azure AD alkalmazásproxy összekötők |} Microsoft Docs"
description: "Az Azure AD-alkalmazásproxy összekötők hello alapjairól ismerteti."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 294cb26803ef7cf8be9f3af0678d6d2e64f6cc14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-ad-application-proxy-connectors"></a>Az Azure AD-alkalmazásproxy összekötők ismertetése

Összekötők a legtöbb esetben mi az Azure AD-alkalmazásproxy lehetővé teszik. Ezek egyszerű, könnyen toodeploy és karbantartása, és felügyelői hatékony. Ez a cikk ismerteti, milyen összekötők, hogyan működnek, és néhány javaslatokat arról, hogyan toooptimize a központi telepítés. 

## <a name="what-is-an-application-proxy-connector"></a>Mi az az alkalmazásproxy-összekötő

Összekötők olyan egyszerűsített helyszíni elhelyezkedik és hello kimenő kapcsolat toohello Proxy szolgáltatás lehetővé teszi. Összekötők hozzáférés toohello a háttéralkalmazás rendelkező Windows Server rendszerre telepíthető. Összekötők minden forgalom toospecific alkalmazások kezelése csoport összekötő csoportokba rendezhetők. Összekötők terheléselosztásához automatikusan, és segít a toooptimize a hálózati környezethez. 

## <a name="requirements-and-deployment"></a>Követelmények és üzembe helyezés

Alkalmazásproxy toodeploy sikeres legyen, legalább egy összekötő van szüksége, de azt javasoljuk, hogy két vagy több, a nagyobb rugalmasságot. Telepítse a Windows Server 2012 R2 vagy 2016-gépnek hello összekötőt. hello összekötő toobe képes toocommunicate hello alkalmazásproxy-szolgáltatás, valamint a közzétett hello a helyszíni alkalmazások kell. 

Hello összekötő kiszolgálója hello hálózati követelményeivel kapcsolatos további információkért lásd: [az alkalmazásproxy első lépései, és telepítse az összekötőt](active-directory-application-proxy-enable.md).

## <a name="maintenance"></a>Karbantartás
hello összekötők és hello szolgáltatás gondoskodunk hello magas rendelkezésre állású feladatok. Akkor is lehet hozzáadásakor vagy eltávolításakor dinamikusan. Minden alkalommal, amikor egy új kérelem érkezik irányított tooone hello összekötők, hogy jelenleg rendelkezésre áll. Ha egy összekötő átmenetileg nem érhető el, toothis forgalom nem működik.

hello összekötők állapot nélküli, és nincsenek konfigurációs adatok hello gépen. hello csak tárolják adata hello beállítások kapcsolódás hello szolgáltatás és -hitelesítési tanúsítványát. Amikor toohello service szolgáltatáshoz csatlakozik, lekéréses hello szükséges összes konfigurációs adatot, és frissítenie minden néhány percig.

Összekötők is kérdezze le a hello server toofind, hogy hello connector újabb verziója. Ha talál olyat, hello összekötők frissítse magát.

Az összekötők futtatnak, hello gépről hello Eseménynapló és a teljesítményszámlálók segítségével figyelheti. Vagy hello alkalmazásproxy lapján hello Azure-portálon az állapotát tekintheti meg:

 ![AzureAD Application Proxy összekötők](./media/application-proxy-understand-connectors/app-proxy-connectors.png)

Törölje az összekötőt, nem használt toomanually nincs. Összekötő futtatásakor addig marad aktív, toohello szolgáltatáshoz csatlakozik. Nem használt összekötők címkével rendelkeznek, _inaktív_ és inaktivitás 10 nap után törlődnek. Ha szeretné, hogy toouninstall összekötő, azonban eltávolítása hello összekötő szolgáltatást és a hello frissítési szolgáltatást hello kiszolgálóról. Indítsa újra a számítógépet toofully hello szolgáltatást.

## <a name="automatic-updates"></a>Automatikus frissítések

Az Azure AD automatikus frissítések üzembe helyezett hello összekötők biztosít. Mindaddig, amíg hello Alkalmazásproxyösszekötő szolgáltatás fut, az összekötők automatikusan frissíteni. Ha nem lát hello összekötő frissítési szolgáltatást a kiszolgálón, akkor a túl kell[telepítse újra az összekötő](active-directory-application-proxy-enable.md) tooget frissítéseket. 

Ha egy automatikus frissítési toocome tooyour összekötő toowait nem szeretné, a kézi frissítés végezheti el. Nyissa meg toohello [összekötő letöltési oldala](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download) hello kiszolgálóra, amelyen az összekötő található, és válassza a **letöltése**. Ez a folyamat hello helyi összekötő frissítés másolattól. 

A bérlők számára a több összekötő hello automatikus frissítések célként egy összekötő környezetében minden csoport tooprevent állásidőt egyszerre. 

Állásidő problémákat tapasztalhat, amikor frissíti az összekötőt, ha:  
- Csak akkor kell egy összekötőt. tooavoid üzemszünet és javíthatja a magas rendelkezésre állás érdekében javasoljuk, hogy a második összekötő telepítéséhez és [hozzon létre egy összekötő csoportot](active-directory-application-proxy-connectors-azure-portal.md).  
- Összekötő hello közel tranzakció, mint amikor hello frissítés megkezdte. Bár hello kezdeti tranzakció nem vesztek el, a böngésző automatikusan újra kell hello műveletet, vagy a lap frissítésével. Hello kérést a rendszer újraküldi, hello forgalom esetén irányított tooa biztonsági mentési összekötő.

## <a name="creating-connector-groups"></a>Összekötő csoportok létrehozása

Összekötő csoportok lehetővé teszik a tooassign adott összekötők tooserve bizonyos alkalmazásokat. Összekötőket csoportosíthatja, és hozzárendelheti a minden egyes tooa alkalmazáscsoport. 

Összekötő csoportok révén könnyebben toomanage nagy méretű telepítéséhez. Akkor is tovább fejlesztheti várakozási ideje a bérlők számára, amelyek rendelkeznek a különböző régiókban található, mert csoportokat hozhat létre helyalapú összekötő tooserve csak a helyi alkalmazások. 

További információ az összekötő csoportok toolearn lásd [külön hálózatok és helyek összekötő csoportok használata alkalmazások közzétételére](active-directory-application-proxy-connectors-azure-portal.md).

## <a name="security-and-networking"></a>Biztonság és a hálózatkezelés

Összekötők bárhol hello hálózaton, amellyel toosend kérelmek toohello alkalmazásproxy-szolgáltatás telepíthető. A fontos hello hello-összekötőt futtató is tartozik hozzáférés tooyour alkalmazások. Összekötők telepíthető a vállalati hálózaton belül, vagy hello felhőben futó virtuális gépen. Összekötők futtathatja a demilitarizált zónában (DMZ), de nincs szükség, mert az összes akkor kimenő forgalomról beszélünk, a hálózat biztonságos marad.

Összekötők csak a kimenő kérések küldése. hello kimenő adatforgalom toohello alkalmazásproxy-szolgáltatás és toohello közzétett alkalmazásokhoz. Bejövő portot, mert a forgalom mindkét irányba után a munkamenet tooopen nincs. Nem tooset hello összekötőket közötti terheléselosztás fel van, és a tűzfalon keresztüli bejövő hozzáférés konfigurálása. 

Kimenő tűzfalszabályok konfigurálásával kapcsolatos további információkért lásd: [együttműködnek a meglévő helyszíni proxykiszolgálókat](application-proxy-working-with-proxy-servers.md).

Használjon hello [az Azure AD Application Proxy Connector portok teszt eszközét](https://aadap-portcheck.connectorporttest.msappproxy.net/) tooverify, hogy az összekötő érni hello alkalmazásproxy-szolgáltatás. Minimális győződjön meg arról, hogy hello központi US régió és hello régió legközelebbi tooyou összes zöld jelölők. Túl további zöld jelölők azt jelenti, hogy nagyobb rugalmasság. 

## <a name="performance-and-scalability"></a>Teljesítmény és méretezhetőség

Hello alkalmazásproxy-szolgáltatás számára is méretezhető átlátszó, de méretezési tényező az összekötők. Toohave elég összekötők toohandle csúcs forgalmat kell. Azonban nem szükséges, tooconfigure terheléselosztás mert összekötő csoportban lévő összes összekötőt automatikus terheléselosztása érdekében.

Mivel az összekötők állapot nélküli, nem érinthetik hello felhasználók vagy a munkamenetek száma. Ehelyett válaszoljanak toohello kérések számát és a terhelés méretének. A szokásos webes forgalom egy átlagos gép kezelni tud a néhány ezer kérések száma másodpercenként. hello meghatározott kapacitásra hello pontos gép jellemzők függ. 

Processzor- és hálózati hello összekötők teljesítménye kötéssel. Processzorteljesítmény szükséges SSL-titkosítás és visszafejtés, míg hálózatkezelés fontos tooget gyors kapcsolat toohello alkalmazások és hello online szolgáltatás az Azure-ban.

Ezzel szemben memória beállítás értéke kisebb a csatlakozók kapcsolatos problémát. hello online szolgáltatás hello feldolgozás jelentős részét, és az összes nem hitelesített forgalom gondoskodik. Minden, ami hello felhőben végezhető hello felhőben történik. 

Teljesítményét befolyásoló másik tényező hello minőségének hello hello összekötők, beleértve a közötti hálózati kapcsolat: 

* **online szolgáltatás hello**: lassú vagy nagy késleltetésű kapcsolat toohello Azure befolyásoló hello összekötők teljesítménye az alkalmazásproxy-szolgáltatás. Hello legjobb teljesítmény érdekében kapcsolatba a szervezet tooAzure Express Route. Ellenkező esetben van a hálózati csoport, győződjön meg arról, hogy kapcsolatok tooAzure kezeli a lehető leghatékonyabban. 
* **háttér-alkalmazások hello**: bizonyos esetekben nincsenek további proxyk közötti hello összekötő és hello háttér alkalmazások, amelyek lassú vagy megakadályozza a kapcsolódást. tootroubleshoot ebben az esetben hello összekötő kiszolgálója megnyithat egy böngészőt, majd próbálja tooaccess hello alkalmazás. Ha hello összekötők futtatja az Azure-ban, de hello alkalmazások helyszíni, hello élmény előfordulhat, a felhasználók várható.
* **tartományvezérlők hello**: hello összekötők SSO-t a Kerberos által korlátozott delegálás hajthatja végre, ha azok hello tartományvezérlőkhöz csatlakozni hello kérelem toohello háttér elküldése előtt. hello összekötők kell a Kerberos-jegyek gyorsítótárát, de foglalt környezetben hello tartományvezérlők hello válaszképességének befolyásolhatja a teljesítményt. A probléma napjainkban egyre általánosabbá összekötők, futtassa az Azure-ban, de olyan tartományvezérlőn, amely a helyszínen kommunikál. 

A hálózati optimalizálás kapcsolatos további információkért lásd: [Azure Active Directory Application Proxy használata esetén a hálózati topológia szempontjai](application-proxy-network-topology-considerations.md).

## <a name="domain-joining"></a>Tartományhoz való csatlakozás

Olyan számítógépen, amelyen a rendszer nem tartományhoz csatlakozó összekötők is futtathatók. Azonban ha azt szeretné, hogy egyszeri bejelentkezés (SSO) tooapplications integrált Windows-hitelesítéssel (IWA) használó, kell egy tartományhoz gép. Ebben az esetben hello csatlakozó gépek illesztett tooa tartományhoz által végrehajtható műveleteket kell-e [Kerberos](https://web.mit.edu/kerberos) által korlátozott delegálás a hello hello felhasználók nevében közzétett alkalmazásokhoz.

Összekötők illesztett toodomains vagy erdőkben találhatók, amelyek a részlegesen megbízható kapcsolat, vagy csak tooread tartományvezérlő is lehet.

## <a name="connector-deployments-on-hardened-environments"></a>Összekötő központi telepítések a megerősített környezetben

-Összekötő telepítési egyszerű, és semmilyen speciális konfigurációra van szükség. Van azonban néhány egyedi feltételeket, amelyeket érdemes figyelembe venni:

* A szervezeteknek, amelyek korlátozzák a kimenő forgalom hello kell [nyissa meg a szükséges portok](active-directory-application-proxy-enable.md#open-your-ports).
* A FIPS előírásainak megfelelő gépek szükséges toochange lehet, hogy a konfigurációs tooallow hello összekötő folyamatok toogenerate, és tárolja a tanúsítványt.
* A kérelmek hálózati probléma hello hello folyamatok alapján környezetükben zárolását szervezeteknek toomake meg arról, hogy mindkét összekötő-szolgáltatások engedélyezett tooaccess minden szükséges portok és IP-cím áll.
* Bizonyos esetekben a kimenő előre proxyk hello kétirányú tanúsítványhitelesítés törés és hello kommunikációs toofail vezethet.

## <a name="connector-authentication"></a>Összekötő-hitelesítés

biztonságos szolgáltatás tooprovide, összekötők rendelkezik tooauthenticate hello szolgáltatás felé, és hello szolgáltatásban van tooauthenticate hello összekötő felé. Ez a hitelesítés történik, amikor hello összekötők hello kapcsolatot kezdeményezzen ügyfél és kiszolgáló-tanúsítványok használatával. Ez úgy hello rendszergazda felhasználónevét és jelszavát nem tárolt hello összekötő gépen.

hello használt tanúsítványok olyan konkrét toohello alkalmazásproxy-szolgáltatás. Hello kezdeti regisztráció során létrehozása, és automatikusan megújítani az hello összekötők minden néhány hónappal. 

Ha az összekötő nem csatlakozik több hónapig, a tanúsítványok toohello szolgáltatás lehet, hogy elavult. Ebben az esetben távolítsa el, majd telepítse újra a hello összekötő tootrigger regisztrálása. A következő PowerShell-parancsok hello futtathatja:

```
Import-module AppProxyPSModule
Register-AppProxyConnector
```

## <a name="under-hello-hood"></a>Hello technikai részletek alapján

Összekötők a úgy, hogy a legtöbb hello azonos felügyeleti eszközeit, beleértve a Windows eseménynaplóiban keresse meg a Windows Server webalkalmazás-Proxy alapulnak

 ![Az Eseménynapló hello eseménynaplók kezelése](./media/application-proxy-understand-connectors/event-view-window.png)

és Windows-teljesítményszámlálókat. 

 ![Hello Teljesítményfigyelő-számlálók toohello összekötő hozzáadása](./media/application-proxy-understand-connectors/performance-monitor.png)

hello összekötők rendelkezik rendszergazdai és a munkamenet naplókat. hello admin naplókban szerepel a fő kapcsolódó események és a hibák. hello munkamenet naplók tartalmazni, minden hello tranzakciók és feldolgozási adataikat. 

toosee hello naplókat, nyissa meg az Eseménynapló, nyissa meg hello toohello **nézet** menüt, és lehetővé teszik **megjelenítése elemzési és hibakeresési naplókat**. Ezt követően engedélyezze azokat toostart események gyűjtése. Ezek a naplók nem jelennek meg a Windows Server 2012 R2 rendszerben a webalkalmazás-Proxy, hello összekötők alapuló újabb verziója.

Hello állapotának hello szolgáltatás hello szolgáltatások ablakban ellenőrizheti. hello összekötő magában foglalja a két központi Windows-szolgáltatások: hello tényleges összekötő és hello frissítőjének. Mindkettő kell futtatnia minden hello idő.

 ![AzureAD szolgáltatások helyi](./media/application-proxy-understand-connectors/aad-connector-services.png)

## <a name="next-steps"></a>Következő lépések


* [Külön hálózatok és helyek összekötő csoportokat használnak az alkalmazások közzététele](active-directory-application-proxy-connectors-azure-portal.md)
* [A meglévő helyszíni proxykiszolgálókkal működik](application-proxy-working-with-proxy-servers.md)
* [Proxy és összekötő hibák elhárítása](active-directory-application-proxy-troubleshoot.md)
* [Hogyan a toosilently hello Azure AD alkalmazásproxy-összekötő telepítése](active-directory-application-proxy-silent-installation.md)

