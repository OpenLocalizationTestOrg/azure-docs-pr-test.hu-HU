---
title: "az Azure Application Insights tarifa- és adatok kötet aaaManage |} Microsoft Docs"
description: "Telemetria kötetek kezelése, és figyelje az Application Insightsban költségeket."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: bwren
ms.openlocfilehash: 4621c989cd467735aefc48ec9547fcbe1b1ae41b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Az Application Insightsban tarifa- és adatok kötet kezelése


Az árképzés [Azure Application Insights] [ start] adatmennyiség alkalmazásonként alapul. Kevésbé aktív a fejlesztés során, vagy egy kis alkalmazás szabad, valószínűleg toobe azért, mert egy 1 GB-os havi juttatást telemetriai adatok.

Minden Application Insights-erőforrás külön szolgáltatás fel van töltve, és hozzájárul az előfizetés tooAzure a toohello számlázási.

Nincsenek két árképzési tervek. Alapszintű hello alapértelmezett terv neve. Dönthet úgy is, hello vállalati terv, amely napi díj rendelkezik, de lehetővé teszi, hogy bizonyos további szolgáltatások, mint [a folyamatos exportálás](app-insights-export-telemetry.md).

Ha árképzés működésével kapcsolatos az Application Insights kérdése van, érzi, hogy szabad toopost fel kérdést az [fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights). 

## <a name="hello-price-plans"></a>hello ár tervek

Lásd: hello [árképzést ismertető oldalra Application Insights] [ pricing] a pénznemben számított aktuális árak.

### <a name="basic-plan"></a>Alapszintű

hello alapszintű hello alapértelmezett beállítás, amikor egy új Application Insights-erőforrás jön létre, és a legtöbb felhasználó elegendő.

* Az alapszintű hello, az adatmennyiség áraihoz: a telemetriai adatok az Application Insights által fogadott bájtok száma. Hello méreteként tömörítetlen hello JSON adatcsomag Application Insights által kapott az alkalmazás adatainak mennyisége mérése történik.
A [Analytics importált adatok](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-import), hello adatmennyiség, az elküldött fájlok tooApplication Insights az uncompressed size hello mérése történik.  
* Az első 1 GB az egyes alkalmazásokhoz felszabadul, úgy, ha csak kísérletezés vagy fejlesztésével, a valószínű toohave toopay is.
* [Élő Stream metrikák](app-insights-live-stream.md) adatok díjszabási célra nem számítanak.
* [A folyamatos exportálás](app-insights-export-telemetry.md) érhető el az extra / GB-os kell fizetni az alapszintű hello.

### <a name="enterprise-plan"></a>Vállalati terv

* Hello vállalati tervben az alkalmazás az Application Insights összes hello szolgáltatását használhatja. [A folyamatos exportálás](app-insights-export-telemetry.md) és 

[Naplófájl Analytics összekötő](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) érhetők el a felesleges díjmentesen hello vállalati terv.
* Küldő alkalmazások telemetriai hello vállalati tervben csomópontonként kell fizetnie. 
 * A *csomópont* egy fizikai vagy virtuális gépet, vagy a Platform,--szolgáltatás szerepkör-példány, az alkalmazás.
 * Fejlesztési gépek, az ügyfélböngészők és a mobileszközök nem számítanak csomópontok.
 * Ha az alkalmazás még több összetevők által küldött telemetriai adatok, például egy webszolgáltatás-bővítmény és a háttér-feldolgozók ezek nem számítanak bele.
 * [Élő Stream metrikák](app-insights-live-stream.md) adatok nem számítanak purposes.* díjszabási egy előfizetésből, a díjak nem alkalmazásonkénti csomópontonként. Ha telemetriai adatokat küld a 12 alkalmazásokat, majd hello díjat számítanak öt csomópontok az öt csomóponttal rendelkezik.
* Bár a havi díjak korlátozott akkor van szó, csak a minden órában, amelyben egy csomópont telemetriai adatokat küld az alkalmazásokból. hello óránkénti kell fizetni az hello idézőjelek között havi kell fizetni / 744 (hello a 31 napos hónap órák száma).
* Az egyes csomópontok (az óránkénti részletességű) észlelt kap egy adatok kötet foglalási, 200 MB / nap. Nem használt adatok foglalási rendszer nem veszi át egy nap toohello mellett.
 * Ha hello vállalati árképzési beállítást választja, minden egyes előfizetés lekérdezi a napi támogatás adatok hello toohello Application Insights-erőforrások telemetriai adatok küldése az adott előfizetés csomópontok száma alapján. Ha 5 csomópontok adatküldés minden nap, hogy az 1 GB-os alkalmazott tooall hello Application Insights-erőforrások készletezett támogatás az adott előfizetés. Nem számít, ha az egyes csomópontok üzenetet küld más csomópontok-nál több adatot hello része adatok megosztott összes csomópont. Ha egy adott napon hello Application Insights-erőforrások szerepel ez az előfizetés-adatok foglalás napi hello több adatot kap, hello / GB-os keretét adatok díjak vonatkoznak. 
 * hello napi adatok támogatás a következő képlettel hello hello napra (UTC) órák száma, hogy minden csomópont küld telemetriai 24 alkalommal 200 MB hányadosa. Ezért ha 4 csomópontok telemetriai adatok küldése során 15 hello hello napi 24 óra, hello tartalmazza-e a adatok, mert a nap ((4 x 15) / 24) x 200 MB = 500 MB. Az adatok túlhasználati 2.30-as USD / GB díjakon hello hello kell fizetni az lenne, az 1,15 USD Ha hello csomópontok küldése 1 GB adatot adott napon.
 * Vegye figyelembe, hogy hello vállalati terv napi támogatás nincsenek megosztva, alkalmazások, amelynek alapszintű hello lehetőséget választotta, és használaton kívüli támogatás nem átkerülnek a napi. 
* Íme néhány példa a különböző csomópont számának meghatározása:
| Forgatókönyv                               | Teljes napi száma |
|:---------------------------------------|:----------------:|
| 1 alkalmazás használ 3 Azure App Service-példány és az 1 virtuális kiszolgáló | 4 |
| Ezek az alkalmazások: a hello ugyanahhoz az előfizetéshez, és a vállalati hello tervezi, 2 virtuális gép, és az Application Insights-erőforrások hello futó 3 alkalmazásokhoz | 2 | 
| 4 alkalmazások, amelyek alkalmazások Insights-erőforrások hello ugyanahhoz az előfizetéshez. Minden alkalmazás fut, 16 csúcsidőn 2 példányok és 8 csúcsidőben 4 példányok. | 13.33 | 
| Az 1 feldolgozói szerepkör és 1 webes szerepkör, minden futó 2 példányait cloud services csomag | 4 | 
| 5-csomópont Service Fabric-fürt minden egyes micro-szolgáltatást, futó 3 példányait futtató 50 micro-szolgáltatások, | 5|

* hello pontos csomópont számlálási viselkedés függ, amelyen az Application Insights SDK az alkalmazás használatával. 
  * SDK verzióiban 2.2-es és újabb verziók esetében, mindkét hello Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) vagy [webes SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) fog jelentést minden egyes csomópontja gazda, például a számítógép nevét, a fizikai kiszolgáló és a Virtuálisgép-gazdák vagy hello hello a felhőszolgáltatások hello eset példány nevét.  egyetlen kivételt az alkalmazások csak a hello [.NET Core](https://dotnet.github.io/) és hello Application Insights Core SDK, amelyben eset csak egy csomópont lesz jelentve állomások hello állomás neve nem érhető el. 
  * Hello SDK korábbi verzióiban hello [webes SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) működik ugyanúgy, mint a hello SDK újabb verziók, azonban hello [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) függetlenül a tényleges alkalmazásgazdája hello száma csak egy csomópont tartozik. 
  * Megjegyzés: Ha az alkalmazás hello SDK tooset roleInstance tooa egyéni értéket használ, alapértelmezés szerint ugyanezt az értéket fog használni a csomópontok száma toodetermine hello. 
  * Ügyfélgépek vagy mobil eszközökhöz futó alkalmazáshoz egy új SDK verzióját használja, akkor lehetséges, hogy a csomópontok száma hello előfordulhat, hogy vissza egy szám, amely túl nagy (a hello nagy mennyiségű ügyfél gépek vagy mobil eszközöket). 

### <a name="multi-step-web-tests"></a>Többlépéses webes tesztek

Nincs olyan kiegészítő díjat [többlépéses webteszt](app-insights-monitor-web-app-availability.md#multi-step-web-tests). Hajtsa végre egy műveletsorozatot tooweb teszteket utal. 

Nincs "ping-vizsgálatok" egy oldal külön díjmentes. A többlépéses tesztek és ping-vizsgálatok telemetriai fel van töltve, és egyéb telemetriai az alkalmazásból.
 
## <a name="operations-management-suite-subscription-entitlement"></a>Az Operations Management Suite előfizetés jogosultság

Mint [nemrég jelentette be](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/), felhasználókat, akik megvásárolják a Microsoft Operations Management Suite E1 és E2 képes tooget Application Insights vállalati, minden további költség nélkül további összetevőként. Az Operations Management Suite E1 és E2 tárolóegységekhez konkrétan egy jogosultság too1 csomópont hello vállalati tervek az Application insights szolgáltatással. A fentiek szerint minden Application Insights csomópont too200 MB naponkénti (elkülönül Naplóelemzési adatfeldolgozást), az adatok 90 napos megőrzési további költségek nélkül okozhatnak adatok tartalmazza. 

> [!NOTE]
> tooensure, hogy ezt a jogosultságot kap, rendelkeznie kell az Application Insights-erőforrások hello vállalati árképzési terv. Ez a jogosultság lesz csak csomópontokat, így alapszintű hello az Application Insights-erőforrások nem valósíthat meg hasznos. Vegye figyelembe, hogy ez a jogosultság nem láthatók lesznek a hello becsült költségei hello funkciók és árképzési panelen látható. 
>
 
## <a name="review-pricing-plans-and-estimate-costs"></a>Tekintse át a csomagok és a becsült költség díjszabása

Applicaition Insights segítségével könnyen toounderstand hello díjszabások érhető el, és milyen költségek várhatóan hello legutóbbi használati minták alapján kell. Kezdő által megnyitása hello **szolgáltatások + árazás** panel az Application Insights-erőforrást az Azure-portálon hello hello:

![Tarifacsomag kiválasztása](./media/app-insights-pricing/01-pricing.png)

**a.** Tekintse át a adatmennyiség hello hónapig. Ez magában foglalja a fogadott és megőrzi minden hello adatok (után bármely [mintavételi](app-insights-sampling.md) a kiszolgáló és az ügyfélalkalmazások, és rendelkezésreállás figyelésére szolgáló tesztek.

**b.** Egy külön kell fizetni történik [többlépéses webteszt](app-insights-monitor-web-app-availability.md#multi-step-web-tests). (Ez nem tartalmazza a egyszerű rendelkezésreállás figyelésére szolgáló tesztek, szereplő hello adatok kötet kell fizetni.)

**c.** Hello vállalati terv engedélyezése.

**d.** Kattintson a toodata felügyeleti beállítások tooview adatainak mennyisége az elmúlt hónapban hello, napi kap, vagy adatfeldolgozást mintavételi.

Application Insights díjak tooyour Azure számlázásának kerülnek. Láthatja a részletes adatait az Azure a számlázási szakasz hello Azure-portálon, vagy hello hello kiszámlázni [Azure Billing Portal](https://account.windowsazure.com/Subscriptions). 

![A hello oldalsó menüben válassza ki a számlázási.](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Átviteli sebesség
Háromféleképpen mely hello küldött adatok kötet korlátozva:

* **Mintavételi:** Ez az eljárás használható a kiszolgáló és az ügyfél alkalmazásoknak, a minimális torzulásának metrikák küldött telemetriai hello csökkentheti. Ez az adatok tootune hello mennyiségű hello elsődleges eszköz. További információ [szolgáltatások mintavétel](app-insights-sampling.md). 
* **Napi cap:** Ha létrehozása az Application Insights-erőforrás hello Azure-portálon ez beállítása too500 GB/nap. alapértelmezett hello Visual Studio Application Insights-erőforrás létrehozásakor, a kis (csak 32,3 megabájt MB/nap), amely szánt csak toofaciliate tesztelése. Ebben az esetben célja, hogy a felhasználó hello emeli hello napi cap éles környezetben hello alkalmazás telepítése előtt. hello maximális cap kért nagy forgalmú alkalmazások magasabb legfeljebb 500 GB/nap esetén. Körültekintően hello napi kap, beállításakor a leképezés kell lennie **soha nem toohit hello napi maximális**, mert először majd hello nap hello hátralévő adatok elvesznek, nem toomonitor kell az alkalmazás. toochange azt használja hello napi kötet cap panelen kapcsolódó hello adatok kötetkezelés paneljén (lásd alább). Ne feledje, hogy néhány előfizetéstípusok jóváírás, amely nem használható az Application insights szolgáltatással. Ha az előfizetésnek hello egy költségeik korlátozza, naponta hello cap panel hogyan lesz utasításokat, és lehetővé teszik hello kiváltott túl 32,3 megabájt MB/nap napi cap toobe tooremove.  
* **Sávszélesség-szabályozás:** ezen korlátok hello adatok sebessége too32 k események másodpercenkénti, átlagosan több mint 1 perc. 


*Mi történik, ha saját alkalmazás meghaladja a sávszélesség-szabályozás arány hello?*

* hello mennyiségű adat, amelyet az alkalmazás küld percenként megfelelőségét ellenőrizni kell. Ha az érték meghaladja a hello másodpercenkénti aránya hello percre átlagolva, hello kiszolgáló megtagadja bizonyos kérelmek. hello SDK hello adatok puffereli, és megpróbálja tooresend megugrását terjednek több perc. Ha az alkalmazás rendszeresen küld adatokat a fent hello sebesség szabályozása, bizonyos adatok ki lesznek hagyva. (hello ASP.NET, Java és JavaScript SDK-k így tooresend próbálja; Csomagjától előfordulhat, hogy egyszerűen az vetett szabályozott adatokat.) Sávszélesség-szabályozás történik, akkor megjelenik egy értesítés, figyelmeztetés, hogy ez történik.

*Honnan tudhatom, hogy mennyi adatot küld alkalmazásom?*

* Nyissa meg hello **adatok kötetkezelés** panel toosee hello napi adatok kötet diagram. 
* A Metrikaböngészőben, új diagram hozzáadása, és válassza ki **adatpontok mennyisége** a metrika szerint. Csoportosítás váltson, és szerint kell csoportosítani a **adattípus**.

## <a name="tooreduce-your-data-rate"></a>tooreduce az arány
Az alábbiakban néhány módszert megismerhet tooreduce a adatmennyiség:

* Használjon [mintavételi](app-insights-sampling.md). Ez a technológia csökkenti az átviteli sebesség döntés a metrikákat, és hello képességét toonavigate közötti kapcsolódó elemeket a Keresés megszakítása nélkül. Kiszolgálói alkalmazások esetében működik automatikusan.
* [Hello jelenthetők Ajax-hívások száma](app-insights-javascript.md#detailed-configuration) minden lap nézetben vagy kapcsoló Ajax reporting ki.
* Kapcsolja ki a gyűjtemény modulok által a felesleges [ApplicationInsights.config szerkesztése](app-insights-configuration-with-applicationinsights-config.md). Például dönthet úgy, hogy teljesítményszámlálókkal és a függőségi adatokat inessential.
* Ossza fel a telemetriai adatok tooseparate instrumentation kulcsokat. 
* Előre összesített metrikákat. Az alkalmazás rendelkezik hívások tooTrackMetric be, ha forgalom hello többszörös definíciót, amely elfogadja a hello átlagának kiszámítása és a mérési köteg szórása használatával csökkentheti. Vagy használhat egy [előre összesítése csomag](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="managing-hello-maximum-daily-data-volume"></a>Hello maximális napi adatmennyiség kezelése

Hello napi kötet cap toolimit hello gyűjtött adatokat is használhat, de ha hello cap teljesül, az alkalmazás hello hátralévő hello nap által küldött összes telemetery adatvesztést okoz. Az **nem ajánlott** toohave az alkalmazás toohit hello napi cap óta követően a rendszer nem tootrack hello állapotának és teljesítményének az alkalmazás azt talált. 

Ehelyett használjon [mintavételi](app-insights-sampling.md) tootune hello kötet toohello szintnek kívánja, majd használja csak "utolsó lehetőségként" hello napi kap, abban az esetben, ha az alkalmazás indítása telemetery sokkal nagyobb mennyiségű váratlanul küldése. 

toochange hello napi kap, a konfigurálási csoportban az alkalmazás Insihgts erőforrás hello kattintson **adatok kötetkezelés** majd **napi maximális**.

![Hello napi telemetriai kötet cap beállítása](./media/app-insights-pricing/daily-cap.png) 

## <a name="sampling"></a>Mintavételezés
[A mintavételi](app-insights-sampling.md) hello telemetriai van küldésének sebessége tooyour app megmaradnak helyreállításra a hello képességét toofind kapcsolatos események diagnosztikai keresések során közben csökkentése módot, és továbbra is írásmódja megfelelő esemény száma. 

Mintavételi keresztül hatékonyan tooreduce költségek, és a havi kvótán belül maradnak. hello mintavételi algoritmus megtartja a kapcsolódó elemek telemetriai adatot, így például a kereséssel, található hello kérelem kapcsolódó tooa adott kivétel. hello algoritmus is megőrzi megfelelő száma, úgy, hogy hello tartozó helyes értékeket metrika Explorer kérelem sebességét, a kivétel díjszabás és egyéb számok.

Nincsenek mintavételi több űrlap.

* [Adaptív mintavételi](app-insights-sampling.md) hello alapértelmezett hello ASP.NET SDK csomagot, amely automatikusan beállítja a telemetriai adatokból, hogy az alkalmazás küld toohello mennyiségét. Működést automatikusan hello SDK a web app alkalmazásban, hogy hello telemetriai hello hálózati forgalom csökken. 
* *Adatfeldolgozást mintavételi* alternatív megoldás, ahol az alkalmazásból telemetriai belép hello Application Insights szolgáltatás hello pontján működik. Az alkalmazásból küldött telemetriai hello mennyisége nem érinti, de csökkenti a hello kötet hello szolgáltatás megőrzi. Használat tooreduce hello kvóta telemetriai böngészők és egyéb SDK-k használják.

mintavételi, tooset adatfeldolgozást hello vezérlő hello árazás panelen állítsa be:

![Hello kvóta és árképzési panelen kattintson hello minták csempéjére, és válassza a mintavételi töredéke alatt.](./media/app-insights-pricing/04.png)

> [!WARNING]
> hello adat-mintavételezésre panel csak adatfeldolgozást mintavételi hello értéke határozza meg. Hello mintavételi ráta hello Application Insights SDK az alkalmazás által alkalmazott nem is. Ha a bejövő hello telemetriai már mintavételezett: hello SDK, adatfeldolgozást mintavételi nem lesz alkalmazva.
> 

a mintavételi arány nem számít, ha az telepítve van, használja a tényleges toodiscover hello egy [Analytics lekérdezési](app-insights-analytics.md) Ez például:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

Az egyes megőrzi a rekord, `itemCount` hello eredeti rekordok száma, amely azt jelöli, annál too1 + hello előző elvetett rekordok számát jelzi. 


## <a name="automation"></a>Automatizálás

Írhat egy parancsfájl tooset hello ár tervet, Azure Resource Manager használatával. [Megtudhatja, hogyan](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Korlátozások összegzése
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="next-steps"></a>Következő lépések

* [Mintavételezés](app-insights-sampling.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/

