---
title: "aaaConfiguration – gyakori kérdések az Azure web Apps |} Microsoft Docs"
description: "Kérdések a konfigurációs és felügyeleti problémák az Azure App Service Web Apps szolgáltatása hello válaszok toofrequently beolvasása."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 5aa405582813291a4aaf144d2f821ecb20a5edcb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuration-and-management-faqs-for-web-apps-in-azure"></a>Konfigurálása és kezelése – gyakori kérdések az Azure Web Apps

Ez a cikk rendelkezik kérdések (GYIK) konfigurációs és kezelésének számos hello a válaszok toofrequently [az Azure App Service Web Apps szolgáltatásának](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="are-there-limitations-i-should-be-aware-of-if-i-want-toomove-app-service-resources"></a>Vannak-e I kell ügyelnie, ha szeretnék toomove App Service erőforrások korlátai?

Ha azt tervezi, toomove App Service erőforrások tooa új erőforráscsoportba vagy előfizetésbe, nincsenek néhány korlátozások toobe tudomást. További információkért lásd: [App Service korlátozásai](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations).

## <a name="how-do-i-use-a-custom-domain-name-for-my-web-app"></a>A webalkalmazás számára egy egyéni tartománynevet használata?

A válaszok toocommon kérdések az Azure web app alkalmazásban egy egyéni tartománynevet használatával kapcsolatban lásd a hét perces videót [egy egyéni tartománynév hozzáadása](https://channel9.msdn.com/blogs/Azure-App-Service-Self-Help/Add-a-Custom-Domain-Name). videó hello kínál bemutatóért tooadd egy egyéni tartománynevet. Leírja, hogyan toouse saját URL-cím helyett hello *. azurewebsites.net az App Service webalkalmazás URL-cím. Részletes is látható [hogyan toomap egy egyéni tartománynevet](web-sites-custom-domain-name.md).


## <a name="how-do-i-purchase-a-new-custom-domain-for-my-web-app"></a>Hogyan vásárolhat egy új egyéni tartományt a webalkalmazás?

Hogyan toopurchase és az App Service webalkalmazás az egyéni tartománynév beállítása: toolearn [vásárolnia, és állítson be egy egyéni tartománynevet az App Service](custom-dns-web-site-buydomains-web-app.md).


## <a name="how-do-i-upload-and-configure-an-existing-ssl-certificate-for-my-web-app"></a>Hogyan feltöltése és konfigurálása a létező SSL-tanúsítvány a webalkalmazás?

Hogyan tooupload, és állítsa be egy meglévő egyéni SSL-tanúsítvány: toolearn [kötése egy meglévő egyéni SSL tanúsítvány tooan Azure webalkalmazás](app-service-web-tutorial-custom-ssl.md#upload).


## <a name="how-do-i-purchase-and-configure-a-new-ssl-certificate-in-azure-for-my-web-app"></a>Hogyan vásárolhat és egy új SSL-tanúsítvány konfigurálása az Azure-ban a webalkalmazás?

Hogyan toopurchase, és állítsa be az App Service web app alkalmazásban egy SSL-tanúsítványa: toolearn [hozzáadása egy SSL-tanúsítvány tooyour App Service alkalmazás](web-sites-purchase-ssl-web-site.md).


## <a name="how-do-i-move-application-insights-resources"></a>Hogyan helyezze át az Application Insights-erőforrások?

Azure Application Insights jelenleg hello áthelyezési művelet nem támogatja. Ha az eredeti erőforráscsoport Application Insights-erőforrást tartalmaz, nem helyezhető át, hogy az erőforrás. Ha hello Application Insights-erőforrást meg toomove egy App Service-alkalmazást, a teljes hello áthelyezése művelet sikertelen lesz. Azonban az Application Insights és az App Service-csomag nem szükséges a toobe hello hello ugyanazt az erőforráscsoportot a hello app toofunction hello alkalmazásként megfelelően.

További információkért lásd: [App Service korlátozásai](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations).

## <a name="where-can-i-find-a-guidance-checklist-and-learn-more-about-resource-move-operations"></a>Ahol I talál útmutatást ellenőrzőlista és további erőforrás kapcsolatos műveletek áthelyezése?

[App Service korlátozásai](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations) bemutatja, hogyan toomove erőforrások tooeither egy új előfizetés vagy tooa új erőforrás csoport hello azonos előfizetéssel. Erőforrás-áthelyezés ellenőrzőlista hello vonatkozó információt, ismerje meg, melyik szolgáltatás hello áthelyezési művelet támogatja, és további tudnivalók az App Service korlátozásai és egyéb témaköröket.

## <a name="how-do-i-set-hello-server-time-zone-for-my-web-app"></a>Hogyan állíthatom hello server időzóna a webalkalmazás?

tooset hello kiszolgáló időzónáját a webalkalmazás:

1. Hello Azure-portálon, az App Service előfizetésében lépjen toohello **Alkalmazásbeállítások** menü.
2. A **Alkalmazásbeállítások**, adja hozzá ezt a beállítást:
    * Kulcs = WEBSITE_TIME_ZONE
    * Érték = *hello kívánt időzóna*
3. Kattintson a **Mentés** gombra.

## <a name="why-do-my-continuous-webjobs-sometimes-fail"></a>Miért hajtsa végre a folyamatos webjobs-feladatok néha nem?

Alapértelmezés szerint a webalkalmazások a memóriából, ha a beállított időn üresjáratban. Ez lehetővé teszi, hogy az erőforrások megőrzése hello rendszer. A Basic és Standard csomagokban bekapcsolása hello **mindig a** mindig hello beállítása tookeep hello webalkalmazás betöltése. A webalkalmazás fut a folyamatos webjobs-feladatok, be kell kapcsolnia **mindig a**, vagy hello webjobs-feladatok nem futnak megbízhatóan. További információkért lásd: [folyamatosan futó webjobs-feladat létrehozása](web-sites-create-web-jobs.md#CreateContinuous).

## <a name="how-do-i-get-hello-outbound-ip-address-for-my-web-app"></a>Hogyan tudom működtetni a hello kimenő IP-címet a webalkalmazás?

a webalkalmazás kimenő IP-címek tooget hello listáját:

1. Hello Azure-portálon, a webes alkalmazás paneljén lépjen toohello **tulajdonságok** menü.
2. Keresse meg **kimenő IP-címek**.

kimenő IP-címek hello listája jelenik meg.

Ha a webhely az App Service Environment-környezet a PowerApps, toolearn hogyan tooget a kimenő IP-cím, lásd: [kimenő hálózati címek](app-service-app-service-environment-network-architecture-overview.md#outbound-network-addresses).

## <a name="how-do-i-get-a-reserved-or-dedicated-inbound-ip-address-for-my-web-app"></a>Hogyan szerezhetek fenntartott vagy dedikált bejövő IP-címnek a webalkalmazás?

bejövő hívások tooyour Azure alkalmazás webhely, dedikált vagy fenntartott IP-cím mentése tooset telepítse, és IP-alapú SSL-tanúsítvány konfigurálása.

Vegye figyelembe, hogy egy dedikált toouse, vagy a bejövő hívások fenntartott IP-címet, az App Service-csomag szerepelnie kell egy egyszerű vagy magasabb service-csomag.

## <a name="can-i-export-my-app-service-certificate-toouse-outside-azure-such-as-for-a-website-hosted-elsewhere"></a>I exportálhatja az App Service tanúsítvány toouse kívül Azure, például egy webhely máshol tárolt? 

App Service-tanúsítványok minősülnek Azure-erőforrások. Nincsenek tervezett toouse kívül az Azure-szolgáltatások. Nem lehet exportálni őket toouse Azure kívül. További információkért lásd: [gyakori kérdések az App Service-tanúsítványok és az egyéni tartományok](https://social.msdn.microsoft.com/Forums/azure/f3e6faeb-5ed4-435a-adaa-987d5db43b80/faq-on-app-service-certificates-and-custom-domains?forum=windowsazurewebsitespreview).

## <a name="can-i-export-my-app-service-certificate-toouse-with-other-azure-cloud-services"></a>Exportálhatja az App Service tanúsítvány toouse az egyéb Azure felhőszolgáltatások?

hello portálon az első osztályú élményt nyújt az Azure Key Vault tooApp szolgáltatás alkalmazásokkal az App Service-tanúsítvány telepítése. Azonban azt rendelkezik lett ügyfelek toouse érkező kéréseket a tanúsítványokat fogadó kívül hello App Service platformra, például Azure virtuális gépekkel. Hogyan toocreate PFX helyi másolat készítése az App Service, a más Azure-erőforrások hello tanúsítvány használható tanúsítvány toolearn lásd [PFX helyi másolatot készít egy App Service tanúsítvány](https://blogs.msdn.microsoft.com/appserviceteam/2017/02/24/creating-a-local-pfx-copy-of-app-service-certificate/).

További információkért lásd: [gyakori kérdések az App Service-tanúsítványok és az egyéni tartományok](https://social.msdn.microsoft.com/Forums/azure/f3e6faeb-5ed4-435a-adaa-987d5db43b80/faq-on-app-service-certificates-and-custom-domains?forum=windowsazurewebsitespreview).


## <a name="why-do-i-see-hello-message-partially-succeeded-when-i-try-tooback-up-my-web-app"></a>Miért látom "Részben sikeres" üdvözlőüzenetére tooback kész a webalkalmazás közben?

A közös biztonsági mentési hiba oka, hogy néhány fájl hello alkalmazás használatban van. Használt zárolva hello biztonsági mentés végrehajtása során. Ez megakadályozza, hogy ezek a fájlok biztonsági mentése folyamatban, és a "Részben sikeres" állapota esetén előfordulhat. Potenciálisan megakadályozhatja ez által a fájlok kizárása a biztonsági mentési folyamat hello lépett fel. Kiválaszthatja, hogy csak a legszükségesebbekre mentése tooback. További információkért lásd: [biztonsági mentése csak hello fontos részek a webhely, az Azure web apps](http://www.zainrizvi.io/2015/06/05/creating-partial-backups-of-your-site-with-azure-web-apps/).

## <a name="how-do-i-remove-a-header-from-hello-http-response"></a>Hogyan fejléc eltávolítása hello HTTP-válasz?

tooremove hello fejléceket a HTTP-válasz hello a webhely web.config fájl frissítése. További információkért lásd: [távolítsa el az Azure websitesban általános jogú kiszolgálói fejlécek](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/).

## <a name="is-app-service-compliant-with-pci-standard-30-and-31"></a>Megfelel az App Service PCI szabvány 3.0 és 3.1?

Hello Azure App Service Web Apps szolgáltatása jelenleg PCI adatok biztonsági szabvány (DSS) verzióját az 1. szintű 3.0-s. A terv PCI DSS 3.1-es verziója van. Tervezési már folyamatban van hogyan hello legújabb standard bevezetése folytatódik.

PCI DSS 3.1-es verziója szükséges Transport Layer Security (TLS) 1.0 letiltása. A TLS 1.0 letiltása jelenleg nem a beállítás a legtöbb App Service-csomagokról. Ha Ön App Service Environment-környezet használata vagy hajlandó toomigrate a munkaterhelés tooApp Service-környezet, kaphat nagyobb ellenőrzése a környezetben. Ez magában foglalja a TLS 1.0 letiltása lépjen kapcsolatba az Azure támogatási szolgálatához. A jövőben közelében hello tervezzük toomake ezen beállítások elérhető toousers.

További információkért lásd: [Microsoft Azure App Service web app és való megfelelés PCI szabvány 3.0 3.1](https://support.microsoft.com/help/3124528).

## <a name="how-do-i-use-hello-staging-environment-and-deployment-slots"></a>Átmeneti környezet és üzembe helyezési hello használata?

A Standard és prémium szintű App Service csomagokban a webes alkalmazás tooApp szolgáltatás telepítésekor telepítése tooa külön üzembe helyezési pont helyett toohello alapértelmezett éles tárolóhelyre. Üzembe helyezési olyan élő webes alkalmazások, amelyek a saját állomás neve. Webes alkalmazás tartalom és a konfigurációs elemek lecserélhető között két üzembe helyezési, beleértve a hello éles tárolóhelyre.

Üzembe helyezési használatával kapcsolatos további információkért lásd: [az App Service fejlesztői környezet beállítása](web-sites-staged-publishing.md).

## <a name="how-do-i-access-and-review-webjob-logs"></a>Hogyan eléréséhez és tekintse át a webjobs-feladat naplókat?

naplózza a webjobs-feladat tooreview:

1. Jelentkezzen be tooyour [Kudu webhely](https://*yourwebsitename*.scm.azurewebsites.net).
2. Válassza ki a hello webjobs-feladat.
3. Jelölje be hello **váltása kimeneti** gombra.
4. toodownload hello kimeneti fájlt, jelölje be hello **letöltése** hivatkozásra.
5. Egyes futtatja, válasszon **egyes meghívása**.
6. Jelölje be hello **váltása kimeneti** gombra.
7. Jelölje be hello letöltési hivatkozását.

## <a name="im-trying-toouse-hybrid-connections-with-sql-server-why-do-i-see-hello-message-systemoverflowexception-arithmetic-operation-resulted-in-an-overflow"></a>SQL Server-toouse hibrid kapcsolatok próbálok. Miért látom azt üdvözlőüzenetére "System.OverflowException: aritmetikai művelet túlcsordulást okozott"?

Hibrid kapcsolatok tooaccess SQL Server használatához a Microsoft .NET-frissítések 2016. május 10., kapcsolatok toofail okozhat. Előfordulhat, hogy ez az üzenet jelenik meg:

```
Exception: System.Data.Entity.Core.EntityException: hello underlying provider failed on Open. —> System.OverflowException: Arithmetic operation resulted in an overflow. or (64 bit Web app) System.OverflowException: Array dimensions exceeded supported range, at System.Data.SqlClient.TdsParser.ConsumePreLoginHandshake
```

### <a name="resolution"></a>Megoldás:

Dolgozunk ennek tooupdate Hybrid Connection Manager toofix probléma. Lehetséges megoldások, lásd: [hibrid kapcsolatok hiba történt a következő SQL Server: System.OverflowException: aritmetikai művelet túlcsordulást okozott](https://blogs.msdn.microsoft.com/waws/2016/05/17/hybrid-connection-error-with-sql-server-system-overflowexception-arithmetic-operation-resulted-in-an-overflow/).

## <a name="how-do-i-add-or-edit-a-url-rewrite-rule"></a>Hogyan hozzáadása vagy egy URL-cím átdolgozás szabály szerkesztése?

tooadd vagy egy URL-cím szerkesztése újraírási szabály:

1. Internet Information Services (IIS) kezelője beállítása, hogy az App Service webalkalmazásba tooyour csatlakozik. Hogyan tooconnect tooApp IIS-kezelő szolgáltatás: toolearn [távoli felügyeletére, Azure-webhelyek IIS-kezelő használatával](https://azure.microsoft.com/blog/remote-administration-of-windows-azure-websites-using-iis-manager/).
2. Az IIS-kezelőben szabály felvétele vagy szerkesztése egy URL-cím átdolgozás. Hogyan tooadd vagy egy URL-újraíró Szerkesztés szabály, toolearn lásd [létrehozása átdolgozás szabályok a hello URL-újraíró modul](https://www.iis.net/learn/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module).

## <a name="how-do-i-control-inbound-traffic-tooapp-service"></a>Hogyan szabályozza a bejövő forgalom tooApp szolgáltatás?

Hello hely szintjén a bejövő forgalom tooApp szolgáltatás vezérlése két lehetőség közül választhat:

* Kapcsolja be a dinamikus IP-korlátozásokat. Hogyan dinamikus IP-korlátozások, a tooturn: toolearn [IP-cím és tartomány korlátozása az Azure-webhelyek](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/).
* Kapcsolja be a modul biztonsági. Hogyan modul biztonsági tooturn: toolearn [ModSecurity webalkalmazási tűzfal Azure-webhelyeken futó](https://azure.microsoft.com/blog/modsecurity-for-azure-websites/).

App Service Environment-környezet használatakor használhatja [Barracuda tűzfal](https://azure.microsoft.com/blog/configuring-barracuda-web-application-firewall-for-azure-app-service-environment/).

## <a name="how-do-i-block-ports-in-an-app-service-web-app"></a>Hogyan tiltsa le a portokat az App Service web app alkalmazásban?

App Service hello megosztott bérlős környezet, nem lehetséges tooblock adott portok hello infrastruktúra hello jellege miatt. 4016, 4018 és 4020 TCP-portot is nyissa meg a Visual Studio távoli hibakereséshez lehet.

App Service Environment-környezet, a bejövő és kimenő forgalom teljes hozzáféréssel rendelkeznek. Hálózati biztonsági csoportok toorestrict vagy blokk adott portok is használhatja. App Service Environment-környezet kapcsolatos további információkért lásd: [App Service Environment bemutatása](https://azure.microsoft.com/blog/introducing-app-service-environment/).

## <a name="how-do-i-capture-an-f12-trace"></a>Hogyan rögzítése a az F12 nyomkövetési?

Egy F12 nyomkövetés rögzítésének két lehetőség közül választhat:

* F12 HTTP nyomkövetési
* F12 billentyű lenyomása a konzol kimeneti

### <a name="f12-http-trace"></a>F12 HTTP nyomkövetési

1. Az Internet Explorerben nyissa meg tooyour webhelyet. Fontos toosign következő lépések hello előtt. Ellenkező esetben a hello F12 nyomkövetési bizalmas bejelentkezési adatokat rögzíti.
2. Nyomja le az F12.
3. Győződjön meg arról, hogy hello **hálózati** lapon meg van jelölve, és válassza ki azt hello zöld **lejátszása** gombra.
4. Hello hello probléma reprodukálásához szükséges lépések.
5. Jelölje be hello piros **leállítása** gombra.
6. SELECT hello **mentése** (lemez ikon) gombra, és mentse a hello HAR fájlt (az Internet Explorer és a peremhálózati) *vagy* kattintson a jobb gombbal a hello HAR fájlt, majd válassza ki **HAR elmentse tartalmú** () a Chrome-ban).

### <a name="f12-console-output"></a>F12 billentyű lenyomása a konzol kimeneti

1. Jelölje be hello **konzol** fülre.
2. Az egyes lapokon, amely nagyobb, mint nulla elemet tartalmaz, válassza a hello lapot (**hiba**, **figyelmeztetés**, vagy **információk**). Ha hello lapon nincs bejelölve, hello tab ikon a szürke vagy fekete hello kurzor befejeződött, akkor helyezheti.
3. Hello üzenet területén hello ablaktáblán kattintson a jobb gombbal, majd válassza ki **másolhatja az összes**.
4. Beillesztés hello másolt szöveg egy fájlban, és mentse hello fájlt.

egy HAR fájl tooview, hello használható [HAR viewer](http://www.softwareishard.com/har/viewer/).

## <a name="why-do-i-get-an-error-when-i-try-tooconnect-an-app-service-web-app-tooa-virtual-network-that-is-connected-tooexpressroute"></a>Miért kapok hiba jelenik meg, hogy tooconnect egy App Service web app tooa virtuális hálózathoz csatlakoztatott tooExpressRoute?

Ha egy Azure web app tooa virtuális hálózatot, amely csatlakozott tooAzure ExpressRoute tooconnect, sikertelen. hello következő üzenet jelenik meg: "Átjáró nincs VPN-átjáró."

Jelenleg nem rendelkezik pont-pont VPN kapcsolatok tooa virtuális hálózathoz csatlakoztatott tooExpressRoute. A pont-pont VPN- és ExpressRoute nem lehet hello az azonos virtuális hálózaton. További információkért lásd: [ExpressRoute- és telephelyek közötti VPN kapcsolatok korlátai és korlátozásai](../expressroute/expressroute-howto-coexist-classic.md#limits-and-limitations).

## <a name="how-do-i-connect-an-app-service-web-app-tooa-virtual-network-that-has-a-static-routing-policy-based-gateway"></a>Csatlakozás egy App Service web app tooa virtuális hálózatot, amelyen egy statikus útválasztás (házirend-alapú) átjáró?

Csatlakozás egy App Service web app tooa virtuális hálózatot, amelyen egy statikus útválasztás (házirend-alapú) átjáró jelenleg nem támogatott. Ha a cél virtuális hálózat már létezik, pont-pont VPN engedélyezve van, egy dinamikus útválasztási átjáró ahhoz, azok csatlakoztatott tooan app kell rendelkeznie. Ha az átjáró toostatic útválasztási, pont-pont típusú VPN nem engedélyezhető. 

További információkért lásd: [alkalmazások integrálása az Azure virtuális hálózat](web-sites-integrate-with-vnet.md#getting-started).

## <a name="in-my-app-service-environment-why-can-i-create-only-one-app-service-plan-even-though-i-have-two-workers-available"></a>Az App Service Environment-környezet a miért hozható létre csak egy App Service-csomag, annak ellenére, hogy van két munkavállalók elérhető?

tooprovide hibatűrést, a App Service Environment-környezet megköveteli, hogy minden feldolgozókészletek kell-e legalább egy további számítási erőforrás. hello további számítási erőforrás nem rendelhető hozzá a munkaterhelés.

További információkért lásd: [hogyan toocreate az App Service-környezetek](app-service-web-how-to-create-an-app-service-environment.md).

## <a name="why-do-i-see-timeouts-when-i-try-toocreate-an-app-service-environment"></a>Miért látom időtúllépések, az App Service-környezetek toocreate meg?

Egyes esetekben az App Service Environment-környezet létrehozása sikertelen. Ebben az esetben tekintse meg a következő hiba történt a tevékenység naplózza hello hello:
```
ResourceID: /subscriptions/{SubscriptionID}/resourceGroups/Default-Networking/providers/Microsoft.Web/hostingEnvironments/{ASEname}
Error:{"error":{"code":"ResourceDeploymentFailure","message":"hello resource provision operation did not complete within hello allowed timeout period.”}}
```

tooresolve, győződjön meg arról, hogy hello a következő feltételek egyike teljesül:
* hello alhálózat mérete túl kicsi.
* hello alhálózat nem üres.
* ExpressRoute megakadályozza, hogy a hello hálózati kapcsolat egy App Service Environment-környezet követelményeinek.
* Hibás hálózati biztonsági csoport megakadályozza, hogy a hello hálózati kapcsolat egy App Service Environment-környezet követelményeinek.
* A kényszerített bújtatás be van kapcsolva.

További információkért lásd: [gyakori problémák (létrehozás) telepítésekor egy új Azure App Service Environment-környezet](https://blogs.msdn.microsoft.com/waws/2016/05/13/most-frequent-issues-when-deploying-creating-a-new-azure-app-service-environment-ase/).

## <a name="why-cant-i-delete-my-app-service-plan"></a>Miért nem lehet törölni az App Service-csomag?

Az App Service-csomag nem törölhető, ha az App Service alkalmazások társított hello App Service-csomag. Az App Service-csomag törlése előtt távolítsa el az összes társított App Service-alkalmazások hello App Service-csomag.

## <a name="how-do-i-schedule-a-webjob"></a>Hogyan ütemezni a webjobs-feladat?

Létrehozhat egy ütemezett webjobs-feladat Cron-kifejezés használatával:

1. Hozzon létre egy settings.job fájlt.
2. A JSON-fájlt tartalmaznak egy ütemezés tulajdonság Cron-kifejezés használatával: 
    ```
    { "schedule": "{second}
    {minute} {hour} {day}
    {month} {day of hello week}" }
    ```

Az ütemezett webjobs-feladatok kapcsolatos további információkért lásd: [hozzon létre egy ütemezett webjobs-feladat Cron-kifejezés](web-sites-create-web-jobs.md#CreateScheduledCRON).

## <a name="how-do-i-perform-penetration-testing-for-my-app-service-app"></a>Hogyan végezhető el az App Service-alkalmazás tesztelése behatolás?

tooperform behatolást vagy a biztonság tesztelése, [igényelnie](https://security-forms.azure.com/penetration-testing/terms).

## <a name="how-do-i-configure-a-custom-domain-name-for-an-app-service-web-app-that-uses-traffic-manager"></a>Hogyan állítson be egy egyéni tartománynevet, az App Service webalkalmazás Traffic Manager használó?

Hogyan toouse egy App Service alkalmazás által használt Azure Traffic Manager a(z) terheléselosztást, és egy egyéni tartománynevet: toolearn [egy egyéni tartománynév beállítása az Azure-webalkalmazás a Traffic Managerrel](web-sites-traffic-manager-custom-domain-name.md).

## <a name="my-app-service-certificate-is-flagged-for-fraud-how-do-i-resolve-this"></a>Az App Service tanúsítvány csalás van megjelölve. Hogyan lehet elhárítani ezt?

Hello tartomány az ellenőrzés során az App Service tanúsítvány beszerzésére hello a következő üzenetet láthatja:

"A tanúsítvány rendelkezik már meg van jelölve a lehetséges csalásról. hello kérelem teljesítése jelenleg felülvizsgálat alatt van. Ha hello tanúsítvány nem lesz használható 24 órán belül, forduljon Azure támogatási szolgálatához."

Hello az üzenet azt jelzi, mert az csalás ellenőrzési folyamat too24 óra toocomplete előfordulhat, hogy tarthat. Ebben az időszakban toosee üdvözlőüzenetére fogja folytatni.

Ha az App Service tanúsítvány továbbra is tooshow Ez az üzenet 24 óra múlva, futtassa a következő PowerShell-parancsfájl hello. hello parancsfájl névjegyek hello [tanúsítványszolgáltató](https://www.godaddy.com/) közvetlenül tooresolve hello probléma.

```
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <subId>
$actionProperties = @{
    "Name"= "<Customer Email Address>"
    };
Invoke-AzureRmResourceAction -ResourceGroupName "<App Service Certificate Resource Group Name>" -ResourceType Microsoft.CertificateRegistration/certificateOrders -ResourceName "<App Service Certificate Resource Name>" -Action resendRequestEmails -Parameters $actionProperties -ApiVersion 2015-08-01 -Force   
```

## <a name="how-do-authentication-and-authorization-work-in-app-service"></a>Hitelesítési és engedélyezési működése az App Service-ben

Részletes dokumentációt a hitelesítéshez és engedélyezéshez az App Service szolgáltatásban, lásd: [App Service biztonsági](../app-service/app-service-security-readme.md). hello dokumentáció is tartalmaz információt az App Service toouse beállításával kapcsolatos különféle azonosítása szolgáltatóhoz bejelentkezést:
* [Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md)
* [Facebook](../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md)
* [Google](../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md)
* [Microsoft-fiók](../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md)
* [Twitter](../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md)

## <a name="how-do-i-redirect-hello-default-azurewebsitesnet-domain-toomy-azure-web-apps-custom-domain"></a>Hogyan átirányítási hello alapértelmezett *. azurewebsites.net tartomány toomy Azure-webalkalmazásban tartozó egyéni tartomány?

Amikor egy új webhelyet hoz létre az alapértelmezett Azure Web Apps használatával *sitename*. azurewebsites.net tartomány tooyour helyhez van hozzárendelve. Ha egyéni állomás neve tooyour hely hozzáadása, és nem szeretné felhasználók toobe képes tooaccess az alapértelmezett *. azurewebsites.net tartományban, átirányíthatók hello alapértelmezett URL-CÍMÉT. toolearn hogyan tooredirect minden forgalom a webhely alapértelmezett tartomány tooyour egyéni tartomány, lásd: [átirányítási hello alapértelmezett tooyour egyéni tartományok az Azure web apps](http://www.zainrizvi.io/2016/04/07/block-default-azure-websites-domain/).

## <a name="how-do-i-determine-which-version-of-net-version-is-installed-in-app-service"></a>Hogyan állapítható meg, melyik verziója a .NET a verziója telepítve van az App Service-ben?

hello leggyorsabb módon toofind hello az App Service-ben telepített Microsoft .NET verziója hello Kudu konzol használatával. Hello Kudu konzol hello portálon vagy az App Service alkalmazás hello URL-cím segítségével érheti el. Részletes útmutatásért lásd: [határozza meg hello telepített .NET verzióját az App Service](https://blogs.msdn.microsoft.com/waws/2016/11/02/how-to-determine-the-installed-net-version-in-azure-app-services/).

## <a name="why-isnt-autoscale-working-as-expected"></a>Automatikus skálázás miért nem működik az elvárásoknak megfelelően?

Ha Azure automatikus skálázás nem méretezhető a vagy horizontálisan hello web app-példány megfelelő, akkor előfordulhat, hogy fut az olyan forgatókönyvekben, ahol szándékosan választjuk nem tooscale tooavoid ki végtelen ciklus miatt túl "" ugrál"." Ez általában akkor fordul elő, ha nem áll rendelkezésre megfelelő hello kibővített és a skála a küszöbértékek között. Hogyan tooavoid "váltakozó állapotú" és az automatikus skálázás gyakorlati tanácsokat, tooread: toolearn [automatikus skálázás gyakorlati tanácsok](../monitoring-and-diagnostics/insights-autoscale-best-practices.md#autoscale-best-practices).

## <a name="why-does-autoscale-sometimes-scale-only-partially"></a>Miért nem automatikus skálázás néha méretezése csak részben?

Automatikus skálázás lesz kiváltva, ha a metrikák túllépnek előre konfigurált határokat. Néha azt tapasztalhatja, hogy hello kapacitás csak részben tölti összehasonlított toowhat várt. Ez akkor fordulhat elő, ha szeretné példányok hello száma nem érhetők el. Adott esetben automatikus skálázás részben tölti be hello elérhető példányok száma. Automatikus skálázás majd futtatja az hello egyensúlyozza ki újra logika tooget további kapacitást. Példányok fennmaradó hello oszt ki. Vegye figyelembe, hogy ez eltarthat néhány percig.

Ha néhány perc múlva várt hello példányainak száma nem látható, mert hello részleges Újratöltés nem volt elég toobring hello metrikák hello határain belül lehet. Vagy az automatikus skálázás előfordulhat, hogy rendelkezik méretezhető mert hello alacsonyabb metrikák határ elérte.

Ha ezek a feltételek közül egyik sem vonatkoznak, és hello a probléma továbbra is fennáll, a támogatási kérelem elküldéséhez.

## <a name="how-do-i-turn-on-http-compression-for-my-content"></a>Hogyan bekapcsolni a HTTP-tömörítés a tartalom?

tooturn a tömörítést is biztosít a statikus és dinamikus tartalom, adja hozzá a következő kód toohello alkalmazásszintű web.config fájl hello:

```
<system.webServer>
<urlCompression doStaticCompression="true" doDynamicCompression="true" />
< /system.webServer>
```

Is megadhat hello adott dinamikus és statikus MIME-típusok, amelyet az toocompress. További információkért lásd: a válasz tooa fórum kérdést [httpCompression beállítások egy egyszerű Azure webhelyen](https://social.msdn.microsoft.com/Forums/azure/890b6d25-f7dd-4272-8970-da7798bcf25d/httpcompression-settings-on-a-simple-azure-website?forum=windowsazurewebsitespreview).

## <a name="how-do-i-migrate-from-an-on-premises-environment-tooapp-service"></a>Hogyan egy a helyszíni környezet tooApp szolgáltatás át?

a Windows és Linux webes kiszolgálók tooApp szolgáltatás toomigrate helyeket, használhat Azure App Service áttelepítési Segéd. hello áttelepítési eszköz hoz létre webes alkalmazásokat és adatbázisokat Azure igény szerint, és majd közzéteszi a hello tartalom. További információkért lásd: [Azure App Service áttelepítési Segéd](https://www.movemetothecloud.net/).
