---
title: "aaaSlow webes alkalmazás teljesítménye az App Service szolgáltatásban |} Microsoft Docs"
description: "Ez a cikk segít az Azure App Service lassú web app teljesítménnyel kapcsolatos problémák hibaelhárítása."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: top-support-issue
keywords: "webes alkalmazás teljesítménye, lassú app alkalmazást lassú"
ms.assetid: b8783c10-3a4a-4dd6-af8c-856baafbdde5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: cephalin
ms.openlocfilehash: 3e56b99b48db0e7baae1fac797a7fcb9eff74c9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Az Azure App Service lassú web app teljesítménnyel kapcsolatos problémák hibaelhárítása
Ez a cikk segítséget nyújt a lassú web app teljesítménnyel kapcsolatos problémák elhárítása [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure hello és fórumok Stack Overflow hello](https://azure.microsoft.com/support/forums/). Másik lehetőségként is fájl is az Azure támogatási incidens. Nyissa meg toohello [Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) , majd kattintson a **támogatás**.

## <a name="symptom"></a>Jelenség
Ha a felhasználó hello webalkalmazás, hello lapok betöltési lassan és egyes esetekben időtúllépés.

## <a name="cause"></a>Ok
Ez a probléma gyakran alkalmazás szintű problémákat, például:

* hosszú ideig tart a hálózati kérelmek
* alkalmazás éppen nem elég hatékony kód vagy az adatbázis lekérdezések
* nagy memória/CPU-t használó alkalmazások
* alkalmazás összeomló tooan kivétel miatt

## <a name="troubleshooting-steps"></a>Hibaelhárítási lépések
Hibaelhárítás három különböző feladatokat, egymás utáni sorrendben osztható:

1. [Figyelje meg, és figyelheti az alkalmazások viselkedését](#observe)
2. [Adatok gyűjtése](#collect)
3. [Hello probléma elhárítása érdekében](#mitigate)

[App Service Web Apps](/services/app-service/web/) minden lépésnél különböző lehetőséget biztosít.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Figyelje meg, és figyelheti az alkalmazások viselkedését
#### <a name="track-service-health"></a>Nyomon követése szolgáltatásának állapota
A Microsoft Azure publicizes minden alkalommal, amikor a szolgáltatás megszakadásának vagy a teljesítmény romlását van. Hello szolgáltatás állapotának hello követheti a hello [Azure-portálon](https://portal.azure.com/). További információkért lásd: [szolgáltatás állapotát nyomon](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>A webes alkalmazás figyelése
Ezzel a beállítással toofind, ha az alkalmazás problémáit. A webalkalmazás panelen kattintson a hello **kérések és hibák követése** csempére. Hello **metrika** panelen látható összes hello metrikát is hozzáadhat.

Hello metrikákat, érdemes a webalkalmazás toomonitor vannak

* Munkakészlet átlagos memória
* Átlagos válaszidő
* CPU-idő
* Memória munkakészlete
* Kérelmek

![webes alkalmazás teljesítményének figyelése](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

További információkért lásd:

* [Az Azure App Service Web-alkalmazások figyelése](web-sites-monitor.md)
* [Riasztási értesítések fogadása](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Webes végpont állapotának figyelése
Ha a webalkalmazás fut hello **szabványos** IP-címek, Web Apps lehetővé teszi, hogy két végpontot három földrajzi helyekről figyelése.

Webes tesztjeinek használatát, amely a válaszidő és a webalkalmazás URL-címeket üzemidőt tesztelése földrajzilag elosztott helyekről végpontmonitoring kijelző konfigurálása. hello teszt hello webes URL-cím toodetermine hello válaszidő és a hasznos üzemidő HTTP GET műveletet hajt az egyes helyeken. Minden egyes konfigurált helyről futtatja egy tesztet, ötpercenként.

Hasznos üzemidő rendszer figyeli a HTTP válaszkódot használatával, és válaszideje ezredmásodpercben. A figyelési teszt sikertelen, ha HTTP-válaszkód hello nagyobb vagy egyenlő too400, vagy ha hello válasz veszi több mint 30 másodperc. A végpont tekinthető érhető el, ha a figyelési tesztek futtatásához az összes sikeres hello megadott helyeken.

tooset azt, lásd: [az Azure App Service-alkalmazások figyelése](web-sites-monitor.md).

Lásd még [Azure webhelyek tartja be és a Végpontmonitoring kijelző - lengyel Schackow rendelkező](https://channel9.msdn.com/Shows/Azure-Friday/Keeping-Azure-Web-Sites-up-plus-Endpoint-Monitoring-with-Stefan-Schackow) videó a végpontmonitoring kijelző.

#### <a name="application-performance-monitoring-using-extensions"></a>Alkalmazásteljesítmény-figyelési bővítményekkel
Ugyanígy figyelheti az alkalmazások teljesítményéről használatával *bővítmények hely*.

Minden egyes App Service webalkalmazásba biztosít egy bővíthető felügyeleti végpontot, amely lehetővé teszi toouse hatékony hely kiterjesztéseket telepített eszközök. Bővítmények a következők: 

- Szerkesztők kód, például [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx). 
- Felügyeleti eszközök a kapcsolódó erőforrások, például a MySQL-adatbázis tooa webalkalmazás csatlakoztatva.

[Az Azure Application Insights](/services/application-insights/) és [New Relic](/marketplace/partners/newrelic/newrelic/) két hello teljesítményfigyelés rendelkezésre álló hely bővítmények. New Relic toouse, futási időben ügynök telepítése. Azure Application Insights toouse, újra kell építeni a kódot az SDK-val, és bővítménye által biztosított hozzáférés tooadditional adatokat is telepíthet. hello SDK lehetővé teszi írja toomonitor hello kódhasználat és az alkalmazás teljesítményével kapcsolatos további információkhoz juthat.

az Application Insights toouse lásd [a webes alkalmazások teljesítményének figyelésére](../application-insights/app-insights-web-monitor-performance.md).

Tekintse meg a New Relic toouse [új New Relic teljesítmény Alkalmazáskezelés Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />

### <a name="2-collect-data"></a>2. Adatok gyűjtése
hello webalkalmazások környezet diagnosztikai funkciók hello webkiszolgáló és a webes alkalmazás hello naplózási információkat biztosít. hello információt diagnosztikai web server és az application diagnostics van osztva.

#### <a name="enable-web-server-diagnostics"></a>Web server diagnosztika engedélyezése
Engedélyezheti vagy letilthatja a következő naplók típusú hello:

* **Részletes naplózás hiba** -részletes (állapotkód: 400 vagy nagyobb) hibát jelző HTTP-állapotkódok hiba adatait. Ez tartalmazhat, amelyek segíthetnek meghatározni, miért hello server hello hibakódot adott vissza adatokat.
* **Sikertelen kérelmek nyomkövetésére vonatkozó** – részletes információk a sikertelen kérelmek nyomkövetési hello IIS használt összetevőknek tooprocess hello kérés és hello időn belül az egyes összetevők többek között. Ez akkor lehet hasznos, ha tooimprove webes alkalmazás teljesítménye próbált vagy különítse el, mi okozza-e egy adott HTTP-hiba.
* **Webalkalmazás-kiszolgáló naplózza a** -hello W3C bővített naplófájlformátum használata HTTP tranzakciókkal kapcsolatos információkat. Ez akkor hasznos, teljes web app metrika, például a hello kezelt kérelmek száma, vagy hogy hány kérésnek egy adott IP-címről meghatározásakor.

#### <a name="enable-application-diagnostics"></a>Application diagnostics engedélyezése
Több beállítások toocollect alkalmazás teljesítményadatait Web Apps, profilhoz, a Visual Studio élő alkalmazást, vagy módosítsa az alkalmazás kódja toolog további információkat, valamint a nyomkövetési adatokat. Hello lehetőséget választhatja mennyi hozzáférést toohello alkalmazás van, és figyelési eszközök hello a megfigyelt.

##### <a name="use-application-insights-profiler"></a>Application Insights Profilkészítő használata
Engedélyezheti a hello Application Insights Profilkészítő toostart részletes teljesítmény nyomok rögzítését. Nyomkövetések mentése napja került sor, amikor szüksége tooinvestigate problémák történt múlt hello toofive rögzített érheti el. Ezt a lehetőséget választhatja, mindaddig, amíg hozzáférést toohello web app Application Insights-erőforrás rendelkezik Azure-portál.

Application Insights Profilkészítő válaszideje minden webes hívás és a nyomkövetési adatokat, amely jelzi, hogy mely kódsort okozott hello lassú válaszok nyújt információkat. Egyes esetekben hello App Service alkalmazás lassú mert bizonyos kód nem egy performant módon. Például a párhuzamos és a nemkívánatos adatbázis zárolási contentions futtató szekvenciális kódot. A szűk keresztmetszetek eltávolítása hello kódban növeli a hello alkalmazás teljesítménye, de rögzített toodetect fejlesztett ki nyomkövetéseket és a naplók beállítása nélkül. Application Insights Profilkészítő által gyűjtött hello nyomkövetéseket hello sornyi kódot, amely hello alkalmazás lelassítja azonosító segítségével, és megoldásához, a kihívás az App Service-alkalmazásokhoz.

 További információkért lásd: [profilkészítési élő Azure-webalkalmazásokban az Application insights szolgáltatással](../application-insights/app-insights-profiler.md).

##### <a name="use-remote-profiling"></a>Használja a távoli adatainak összegyűjtése
Az Azure App Service Web Apps, az API-alkalmazások és a webjobs-feladatok is távolról csatolást. Válassza ezt a beállítást, ha hozzáférési toohello webes alkalmazás-erőforrást és tudja, hogyan tooreproduce hello probléma, vagy ha tudja hello pontos idő időköz hello teljesítményprobléma történik.

Távoli profilkészítési akkor hasznos, ha hello hello folyamat CPU-használata túl magas és a folyamat a vártnál lassabban fut-e, vagy a HTTP-kérések hello késés nagyobb, mint a normál, akkor távolról a folyamat kiértékelése és hello CPU mintavételi hívási verem tooanalyze beolvasása hello folyamat tevékenység és a működés közbeni elérési utak code.

További információkért lásd: [távoli profilkészítési támogatás az Azure App Service](https://azure.microsoft.com/blog/remote-profiling-support-in-azure-app-service).

##### <a name="set-up-diagnostic-traces-manually"></a>Diagnosztikai nyomkövetési manuális beállítása
Ha hozzáférés toohello webes alkalmazás forráskódjához, a alkalmazásdiagnosztika lehetővé teszi egy webes alkalmazás által létrehozott toocapture adatok. ASP.NET alkalmazások használhatják hello `System.Diagnostics.Trace` osztály toolog információk toohello alkalmazás diagnosztikai naplófájlok. Azonban meg kell toochange hello kódot, és telepítse újra az alkalmazást. Ez a módszer akkor ajánlott, ha az alkalmazás fut egy tesztelési környezetben.

Részletes útmutatást tooconfigure az alkalmazás naplózást, lásd: [az Azure App Service web Apps diagnosztikai naplózás engedélyezése](web-sites-enable-diagnostic-log.md).

#### <a name="use-hello-azure-app-service-support-portal"></a>Hello Azure App Service támogatása a portálon
Web Apps biztosít hello képességét tootroubleshoot problémák kapcsolódó tooyour webalkalmazás HTTP naplókat, eseménynaplók, folyamat memóriaképek és több megtekintésével. Végezheti el az összes ezt az információt a támogatási portálon, a **http://&lt;az alkalmazás neve >.scm.azurewebsites.net/Support**

hello Azure App Service támogatási portal nyújt három különálló lappal toosupport hello három lépést a gyakori hibaelhárítási használatára:

1. Figyelje meg a jelenlegi viselkedése
2. Diagnosztikai adatok gyűjtése és futtató hello beépített elemzőkkel elemzése
3. Csökkentése

Ha most hello hiba történik, kattintson a **elemzés** > **diagnosztika** > **diagnosztizálása most** toocreate diagnosztikai munkamenet, amely összegyűjti a HTTP-naplókat, az Eseménynapló, memóriaképek, PHP hibanaplókat és a PHP-folyamat jelentés.

Hello adatgyűjtés, miután hello támogatási portálon hello adatok elemzése fut, és lehetővé teszi egy HTML-jelentést.

Abban az esetben, ha azt szeretné, toodownload hello adatok, alapértelmezés szerint, akkor legyen tárolva hello D:\home\data\DaaS mappában.

Hello Azure App Service-támogatás portál további információkért lásd: [új frissítések tooSupport hely bővítmény Azure webhelyekhez](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-hello-kudu-debug-console"></a>Hello Kudu hibakereső konzol használata
A hibakereső konzol hibakeresés, felfedezése, fájlok, valamint JSON végpontokat a környezettel kapcsolatos információk beolvasásakor feltöltése használható webes alkalmazásokat tartalmaz. Ez a konzol nevezik hello *Kudu konzol* vagy hello *SCM irányítópult* a webalkalmazás.

Érhető el ehhez az irányítópulthoz toohello hivatkozást fog **https://&lt;az alkalmazás neve >.scm.azurewebsites.net/**.

A Kudu biztosítja hello, amit a következők:

* az alkalmazás környezeti beállítások
* naplófolyamot
* diagnosztikai memóriakép
* a hibakeresési konzol, ahol futtathatja a Powershell-parancsmagok és alapvető DOS parancsok.

A Kudu egy másik fontos része, hogy de, abban az esetben, ha az alkalmazás első-alkalommal kivételek szűrész, használhatja a Kudu hello SysInternals eszköz Procdump toocreate memóriaképeket. Ezek memóriaképek pillanatképek hello folyamat, és gyakran segítségével bonyolultabb webalkalmazásokba elhárítása.

A Kudu szolgáltatásainak további információkért lásd: [kell tudni Azure webhelyek Team Services eszközök](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-hello-issue"></a>3. Hello probléma elhárítása érdekében
#### <a name="scale-hello-web-app"></a>Skála hello webalkalmazás
Az Azure App Service a teljesítmény növelése és a teljesítményt, módosíthatja hello méretezési, amelyen az alkalmazást futtat. A webes alkalmazás vertikális felskálázásával magában foglalja a két kapcsolódó műveletek: az App Service csomag tooa magasabb tarifacsomagra vált, és bizonyos beállítások konfigurálása után magasabb tarifacsomagra toohello rendelkezik váltott módosítása.

A méretezés további információkért lásd: [egy webalkalmazás skálázása az Azure App Service](web-sites-scale.md).

Ezenkívül lehetősége van toorun egynél több példány az alkalmazást. Méretezési out nemcsak nyújt további feldolgozási képesség, de is lehetővé teszi bizonyos hibatűrést. Ha egy példány leáll hello folyamat, hello más esetekben továbbra is tooserve kérelmek.

Hello toobe manuális vagy automatikus skálázás állíthatja be.

#### <a name="use-autoheal"></a>Elindulásáról használata
Elindulásáról hello (ilyen például a konfigurációs módosításokat, kérelmek, memória-alapú korlátok vagy hello idő szükséges tooexecute kérelmet) kiválasztott beállítások alapján az alkalmazás munkavégző folyamata újraindul. Legtöbbször ennek hello újrahasznosítást hello folyamat hello leggyorsabb módon toorecover a probléma. Indítsa újra a hello webalkalmazást az közvetlenül hello Azure-portálon belül mindig, bár elindulásáról minderre automatikusan meg. Toodo szüksége néhány eseményindítók hozzáadása a webalkalmazáshoz hello legfelső szintű web.config fájlban. Ezek a beállítások akkor működnek a hello azonos módon akkor is, ha az alkalmazás nem .net-alkalmazás.

További információkért lásd: [automatikus javítás Azure webhelyek](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-hello-web-app"></a>Hello a webalkalmazás újraindítása
Újraindítás legtöbbször hello legegyszerűbb módja toorecover az egyszeri hibák. A hello [Azure-portálon](https://portal.azure.com/), a webalkalmazás panelen hello beállítások toostop van, vagy indítsa újra az alkalmazást.

 ![Indítsa újra a webes alkalmazás toosolve teljesítménnyel kapcsolatos problémák](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

A webalkalmazás Azure Powershell használatával is kezelheti. További információ: [Az Azure PowerShell használata az Azure Resource Manager eszközzel](../powershell-azure-resource-manager.md).
