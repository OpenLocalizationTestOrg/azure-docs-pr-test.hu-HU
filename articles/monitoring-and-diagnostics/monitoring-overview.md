---
title: a Microsoft Azure-ban aaaMonitoring |} Microsoft Docs
description: "Választási lehetőségek, ha azt szeretné, toomonitor semmi a Microsoft Azure-ban. A figyelő az Azure, az Application Insights szolgáltatáshoz"
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: robb
ms.openlocfilehash: f5a4f32525c52613f01a913e09a9fe3fbcbaeb21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-monitoring-in-microsoft-azure"></a>A figyelés a Microsoft Azure-ban – áttekintés
Ez a cikk egy áttekintést nyújt az eszközök a Microsoft Azure figyeléshez kiválasztható. Túl vonatkozik
- a Microsoft Azure-ban futó alkalmazások figyelése 
- eszközök/futó szolgáltatások, amelyek az Azure-ban objektumokat figyelhetnek meg Azure-on kívüli. 

Azt ismerteti, amelyek hello különböző termékek és szolgáltatások érhető el, és hogyan működnek együtt. Ez segítséget nyújthatnak toodetermine, mely eszközök legmegfelelőbb meg, milyen esetekben.  

## <a name="why-use-monitoring-and-diagnostics"></a>Megfigyelési és diagnosztikai miért érdemes használni?

A cloud app teljesítményével kapcsolatos problémákat hatással lehet az üzleti. A több összekapcsolt összetevőkkel és gyakori kiadásokban degradations bármikor fordulhat elő. És ha az alkalmazást, a felhasználók általában felderítése a tesztjei során nem talált problémákat. Meg kell ezekről a problémákról értesülnek azonnal, eszközöket, és diagnosztizálására és hello problémák elhárításához. A Microsoft Azure tartománnyal rendelkező eszközök a problémák azonosításához.

## <a name="how-do-i-monitor-my-azure-cloud-apps"></a>Hogyan figyelhetek az Azure felhőalapú alkalmazásokat?

Nincs számos eszközt az Azure-alkalmazások és szolgáltatások figyelését. A funkciók némelyike átfedésben vannak. Ez egy korábbi okokból részben és részben miatt toohello fellazítja a fejlesztési és a művelet az alkalmazások között. 

Az alábbiakban hello egyszerű eszközök:

-   **Az Azure figyelő** alapvető eszköz Azure-on futó szolgáltatások figyelése. Biztosít egy szolgáltatás és a környezet körülvevő hello hello átviteli infrastruktúra szintű adatait. Ha kezeli az alkalmazások az összes Azure-ban, annak eldöntése, hogy tooscale felfelé vagy lefelé erőforrásokat, majd Azure a figyelő lehetővé teszi az valóban használt funkciókért toostart.

-   **Az Application Insights** fejlesztési és egy üzemi felügyeleti megoldás is használható. Működését tekintve a csomag telepítése az alkalmazásba, és így lehetővé teszi több belső nézetét jelenít meg. Az adatok közé tartoznak a függőségi, kivétel nyomkövetési adatok, pillanatképeket, végrehajtási profilok hibakeresés válaszidejét. Az hatékony intelligens olyan eszközöket biztosít a telemetriai adatok elemzéséhez mindkét egy alkalmazás és megismerte a felhasználók tevékenységeit vele toohelp debug toohelp. Megállapítható, hogy egy csúcs az igények válaszidők esedékes toosomething egy alkalmazást vagy egy külső resourcing probléma. Ha a Visual Studio használata és hello alkalmazás hibás, akkor átvihető jobb toohello probléma sor kód, hogy kijavíthassuk.  

-   **Naplófájl Analytics** éles környezetben futó alkalmazások teljesítmény- és terv karbantartási tootune igény van. Az Azure-ban alapul. Gyűjti, és számos más forrásból, bár a késéssel too15 10 perc összesíti. Körű informatikai felügyeleti megoldást nyújt az Azure-ba, a helyszíni és a külső felhőalapú infrastruktúra (például az Amazon Web Services). Az tooanalyze adatok gazdagabb eszközöket biztosít több forrás, lehetővé teszi összetett lekérdezések összes napló és a megadott feltételek proaktív riasztást küldhet.  Egyéni adatok azokat a központi tárházához lekérdezhetik és megjelenítheti azt, még akkor is gyűjtheti. 

-   **A System Center Operations Manager (SCOM)** van, felügyelheti és figyelheti a nagy felhőalapú telepítések. Valószínűleg már tisztában van, a felügyeleti eszköz a helyi Windows Server és Hyper-V alapú-felhők, de is integrálása és az Azure-alkalmazások kezelése. Többek között az Application Insights telepíthet a meglévő élő alkalmazásokra.  Ha egy alkalmazás leáll, láthatja, másodpercben. Vegye figyelembe, hogy a Naplóelemzési nem helyettesíti a SCOM. Azt is, együtt működik.  


## <a name="accessing-monitoring-in-hello-azure-portal"></a>Figyelési funkciójában hello Azure-portál elérése
Minden Azure figyelési szolgáltatásokat is elérhető a felhasználói felület egytáblás. További információt a tooaccess Ez a terület, lásd: [Ismerkedés az Azure-figyelő](monitoring-get-started.md). 

Az erőforrások figyelési funkciók kiemelése ezeket az erőforrásokat, és a figyelési lehetőségek adatlehatolás úgy is elérheti. 

## <a name="examples-of-when-toouse-which-tool"></a>Példák a toouse, amely az eszköz 

a következő szakaszok hello megjelenítése néhány alapvető forgatókönyv, mely eszközök együtt használható. 

### <a name="scenario-1--fix-errors-in-an-azure-application-under-development"></a>A forgatókönyv 1 – javítás hibák fejlesztés alatt az Azure alkalmazásban   

**hello legjobb lehetőség toouse Application Insights, az Azure-figyelő és a Visual Studio együtt**

Azure most hello Visual Studio hibakereső hello felhőben teljes hatványa hello biztosít. Azure-figyelő toosend telemetriai tooApplication Insights konfigurálása. Visual Studio tooinclude hello Application Insights SDK az alkalmazás engedélyezése. Application Insights használhat hello alkalmazás-hozzárendelés toodiscover kijelölése után a futó alkalmazások részeket sérült állapotban-e. Ezen részein, amelyek nem működik megfelelően hibákat és kivételeket már rendelkezésre állnak feltárási. Használhatja az Application Insights toogo mélyebb különböző analytics hello. Ha nem biztos hello hiba, hello Visual Studio hibakereső tootrace kód és a PIN-kód pont további probléma is használhatja. 

További információkért lásd: [webalkalmazások figyelése](../application-insights/app-insights-azure-web-apps.md) , majd tekintse át a toohello tartalomjegyzék hello bal oldali különböző típusú alkalmazások és nyelvek kapcsolatos utasításokat.  

### <a name="scenario-2--debug-an-azure-net-web-application-for-errors-that-only-show-in-production"></a>2 –. forgatókönyv hibakeresése az Azure .NET webalkalmazás, amelyek csak a termelési megjelenítése 

> [!NOTE]
> Ezek a funkciók még csak előzetes verziójúak. 

**ajánlott beállítás toouse Application Insights, és ha lehetséges hello a Visual Studio teljes hibakeresési élmény hello.**

Hello Application Insights pillanatkép hibakereső toodebug használja az alkalmazást. A meghatározott küszöbérték hiba esetén éles összetevőkkel hello rendszer automatikusan rögzíti az idő "pillanatképek." nevű windows telemetriai hello rögzített összeg egy éles felhő biztonságos, mert kisméretű elég nem tooaffect teljesítmény, de elég jelentős tooallow nyomkövetés.  hello rendszer több pillanatkép is rögzítheti. Egy adott időpont hello Azure-portálon keresse meg, vagy a Visual Studio hello teljes élmény használjon. A Visual Studio a fejlesztők is végezze el, hogy a pillanatkép, mintha csak azok a valós idejű volt hibakeresését. Helyi változók, a paraméterek, a memória és a keretek is elérheti. A fejlesztők hozzáférés toothis termelési adatok RBAC szerepet keresztül kell biztosítani.  

További információkért lásd: [pillanatkép hibakeresés](../application-insights/app-insights-snapshot-debugger.md). 

### <a name="scenario-3--debug-an-azure-application-that-uses-containers-or-microservices"></a>3 –. forgatókönyv-tárolók és mikroszolgáltatások használó Azure alkalmazás hibakeresése 

**Ugyanaz, mint 1. forgatókönyv. Az Application Insights, az Azure-figyelő és a Visual Studio együtt használja** Application Insights is támogatja a összegyűjtése telemetriai tárolókba futó folyamatok és mikroszolgáltatások (Kubernetes, Docker, Azure Service Fabric). További információ [tekintse meg ezt a videót a tárolók és mikroszolgáltatások hibakeresés](https://go.microsoft.com/fwlink/?linkid=848184). 


### <a name="scenario-4--fix-performance-issues-in-your-azure-application"></a>Az Azure alkalmazásban kapcsolatos teljesítményproblémák javítás 4 –. forgatókönyv

Hello [Application Insights Profilkészítő](../application-insights/app-insights-profiler.md) van tervezett toohelp ilyen jellegű problémák elhárításához. Azonosítsa, és alkalmazásszolgáltatások (a Logic Apps a webalkalmazások Mobile Apps, az API Apps) és egyéb számítási erőforrásokat, például a virtuális gépek, virtuális gép méretezési csoportok (VMSS), a Cloud Services és a Service Fabric futó alkalmazások teljesítményével kapcsolatos hibaelhárítást. 

> [!NOTE]
> Virtuális gépek, virtuális gép méretezési csoportok (VMSS) felhőalapú szolgáltatások és szolgáltatások háló képességét tooprofile jelenleg előzetes verzióban érhető.   

Emellett proaktív értesítést kap e-mailben kapcsolatos hibákat, például a lassú lapbetöltési idők, bizonyos típusú hello intelligens Kártevőészlelő eszköz által.  Toodo ezt az eszközt a beállításra nincs szükség. További információkért lásd: [intelligens észlelési - Teljesítményanomáliákat](../application-insights/app-insights-proactive-performance-diagnostics.md) és [intelligens észlelési - Teljesítményanomáliákat](https://azure.microsoft.com/blog/Enhancments-ApplicationInsights-SmartDetection/preview).



## <a name="next-steps"></a>Következő lépések
További információ

* [Az ignite-on 2016 videó az Azure figyelője](https://myignite.microsoft.com/videos/4977)
* [Ismerkedés az Azure-figyelő](monitoring-get-started.md)
* [Az Azure Diagnostics](../azure-diagnostics.md) Ha kívánt toodiagnose problémák a felhőalapú szolgáltatás, a virtuális gépet, a virtuális gép skálázása állítsa be, vagy a Service Fabric-alkalmazás.
* [Az Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) Ha toodiagnostic problémák az App Service Web app alkalmazásban.
* [Naplófájl Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) és hello [Operations Management Suite](https://www.microsoft.com/oms/) üzemi felügyeleti megoldás