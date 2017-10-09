---
title: "aaaTroubleshooting nincs adat - .NET-keretrendszerhez készült Application Insights"
description: "Nem jelennek meg adatok az Azure Application Insights? Próbálja meg itt."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: e231569f-1b38-48f8-a744-6329f41d91d3
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 261c25c89884c46e41bbc305474ac854360221ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Adathiány hibaelhárítása – Application Insights .NET-hez
## <a name="some-of-my-telemetry-is-missing"></a>A telemetriai adatok némelyike hiányzik
*Az Application Insightsban az alkalmazás által létrehozott események hello töredéke alatt csak látható.*

* Ha egységesen azonos hello azon részét, az valószínűleg esedékes tooadaptive [mintavételi](app-insights-sampling.md). tooconfirm, nyissa meg a keresési (paneljéről hello áttekintése), és nézze meg példányt egy kérelem vagy más esemény. Hello tulajdonságok szakaszának hello alján kattintson a "..." tooget teljes tulajdonság részleteit. Ha a kérelmek száma > 1, majd mintavételi működik. 
* Ellenkező esetben, akkor előfordulhat, hogy Ön elérte van-e egy [sávszélesség-korlátjának](app-insights-pricing.md#limits-summary) a árképzési terv. Ezek a korlátozások érvényesek / perc.

## <a name="no-data-from-my-server"></a>Nincs adat a kiszolgálóról
*Saját alkalmazás a webkiszolgálón telepített, és be van kapcsolva, nem lát minden telemetriai belőle. A fejlesztői gépen eredménnyel járt OK gombra.*

* Valószínűleg a tűzfallal kapcsolatos probléma. [Állítsa be a tűzfal kivételeit az Application Insights toosend adatok](app-insights-ip-addresses.md).

*I [állapotfigyelő telepítve](app-insights-monitor-performance-live-website-now.md) web server toomonitor meglévő alkalmazásaimat a. Nem szerepel az eredmények.*

* Lásd: [állapotfigyelő hibaelhárítási](app-insights-monitor-performance-live-website-now.md#troubleshooting-runtime-configuration-of-application-insights). 

## <a name="q01"></a>Nincs "Application Insights hozzáadása" lehetőséget a Visual Studio
*Az egér jobb gombjával egy meglévő projektben a Megoldáskezelőre, nem szerepel az Application Insights beállításokat.*

* Nem minden alkalmazástípus .NET projekt hello eszközöket támogatja. Webes és WCF-projektek támogatottak. Más projekttípusok például asztali vagy service alkalmazások még [manuálisan adja hozzá az Application Insights SDK tooyour projektek](app-insights-windows-desktop.md).
* Győződjön meg arról, hogy [Visual Studio 2013 Update 3-as vagy újabb](http://go.microsoft.com/fwlink/?LinkId=397827). Előre telepített és a fejlesztői Analytics eszközök, amelyek hello Application Insights SDK származik.
* Válassza ki **eszközök**, **bővítmények és frissítések** és ellenőrizze, hogy **Analytics Fejlesztőeszközök** van telepítve és engedélyezve van. Ha igen, kattintson a **frissítések** toosee, ha egy frissítés érhető el.
* Hello új projekt párbeszédpanel megnyitásához, és válassza ki az ASP.NET-webalkalmazás. Ha megjelenik a hello nincs Application Insights-beállítást, majd hello eszközei telepítve vannak. Ha nem, akkor távolítsa el, majd újbóli telepítésével hello Application Insights-eszközökkel.

## <a name="q02"></a>Az Application Insights hozzáadása nem sikerült
*Tooadd Application Insights tooan létező projekt meg, egy hibaüzenet jelenik meg.*

Valószínű okokat:

* Sikertelen kommunikáció a hello Application Insights portál; vagy
* Probléma lép fel az Azure-fiókja;
* Csak akkor kell [olvasási hozzáférés toohello előfizetés vagy a csoport, amikor megpróbált toocreate hello új erőforrás](app-insights-resources-roles-access-control.md).

Javítás:

* Ellenőrizze, hogy hello jobb Azure-fiók bejelentkezési hitelesítő adataival megadott. 
* A böngészőben, ellenőrizze, hogy rendelkezik hozzáférési toohello [Azure-portálon](https://portal.azure.com). Nyissa meg a beállításokat, és hogy van-e korlátozás.
* [Add Application Insights tooyour létező projekt](app-insights-asp-net.md): A Megoldáskezelőben kattintson a jobb gombbal a projekt, és válassza az "Application Insights hozzáadása."
* Ha az eszköz még nem működik, hajtsa végre a hello [manuális eljárás](app-insights-windows-services.md) tooadd hello portálon erőforrás majd hello SDK tooyour projekt. 

## <a name="emptykey"></a>Hiba jelenik meg: "Instrumentation kulcs nem lehet üres"
A jelek szerint valami hiba történt, miközben telepíti a Application Insights vagy lehet, hogy a naplózás adapter.

A Megoldáskezelőben kattintson jobb gombbal a projektre, és válassza a **Application Insights > konfigurálja az Application Insights**. Kaphat olyan párbeszédpanel, amely hozzáfűzendő toosign a tooAzure, és létrehozza az Application Insights-erőforrást, vagy használja ismét egy meglévőt.

## <a name="NuGetBuild"></a>"NuGet csomag hiányoznak a" saját build kiszolgálón
*Minden buildek OK I vagyok hibakeresés a fejlesztési számítógépen, de hello build kiszolgálón jelenik meg a NuGet-hiba.*

Ellenőrizze a [NuGet-csomagok visszaállításának](http://docs.nuget.org/Consume/Package-Restore) és [automatikus csomagok visszaállításának](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-tooopen-application-insights-from-visual-studio"></a>Hiányzó menü parancs tooopen a Visual Studio Application Insights
*Az egér jobb gombjával a projekt megoldáskezelő, nem szerepel az Application Insights parancsok, vagy nem szerepel egy olyan nyissa meg az Application Insights-parancsot.*

Valószínű okokat:

* Ha manuálisan létrehozott hello Application Insights-erőforrást, vagy ha hello projekt, amely hello Application Insights-eszközök által nem támogatott típusú.
* a Visual Studio Developer elemzőeszközök hello le vannak tiltva. 
* A Visual Studio 2013 Update 3-nál régebbi.

Javítás:

* Ellenőrizze, hogy a Visual Studio verziója 2013 update 3-as vagy újabb.
* Válassza ki **eszközök**, **bővítmények és frissítések** és ellenőrizze, hogy **fejlesztői elemzőeszközök** van telepítve és engedélyezve van. Ha igen, kattintson a **frissítések** toosee, ha egy frissítés érhető el.
* Kattintson a jobb gombbal a projektben a Megoldáskezelőre. Ha megjelenik a hello parancs **Application Insights > konfigurálja az Application Insights**, tooconnect használják a projekt toohello erőforrás hello Application Insights szolgáltatás.

Ellenkező esetben a projekttípus nem közvetlenül támogatja hello Application Insights-eszközökkel. toosee a telemetriai adatokat toohello bejelentkezés [Azure-portálon](https://portal.azure.com)válassza az Application Insights hello bal oldali navigációs sávon, majd az alkalmazást.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>"A hozzáférés megtagadva" a Visual Studio Application Insights megnyitása
*hello "Nyissa meg az Application Insights" menüparancshoz veszi át, me toohello Azure-portálon, de "hozzáférés megtagadva" hiba jelenik meg.*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

a Microsoft bejelentkezés az alapértelmezett böngésző legutóbb használt hello túl nincs hozzáférése az[készült Application Insights toothis app hozzáadásakor erőforrás hello](app-insights-asp-net.md). Két oka valószínűleg: 

* Egynél több Microsoft-fiók – lehet, hogy a munkahelyi és személyes Microsoft-fiókkal van? hello bejelentkezés az alapértelmezett böngésző legutóbb használt volt egy másik fiókot, mint egy túl hozzáféréssel rendelkező hello[Application Insights toohello projekt hozzáadása](app-insights-asp-net.md). 
  
  * Javítás: Kattintson a nevére, hello böngészőablak jobb felső, és jelentkezzen ki. Jelentkezzen be hello fiókkal, amely hozzáféréssel rendelkezik. Hello bal oldali navigációs sávon, majd kattintson az Application Insights, és válassza ki az alkalmazást.
* Valaki más Application Insights toohello projekt, valamint az toogive elfelejtette meg [hozzáférés toohello erőforráscsoport](app-insights-resources-roles-access-control.md) a, amelyen létrehozták. 
  
  * Javítás: Azok a használatához szervezeti fiók azok felvehesse Önt toohello team; vagy azok adhat meg egyéni hozzáférési toohello erőforráscsoportot.

## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>"Eszköz nem található" a Visual Studio Application Insights megnyitása
*hello "Nyissa meg az Application Insights" menüparancshoz veszi át, me toohello Azure-portálon, de "az eszköz nem található" hiba jelenik meg.*

Valószínű okokat:

* Application Insights-erőforrást az alkalmazáshoz hello törölve lett; vagy
* hello instrumentation kulcs lett beállítva, vagy applicationinsights.config módosítja a közvetlen szerkesztése hello projektfájlt frissítése nélkül. 

hello instrumentation kulcs ApplicationInsights.config vezérlők, ahol hello telemetriai adatok küldése. Egy sor hello projektfájlban szabályozza, hogy melyik erőforrást meg van nyitva, ha a Visual Studio hello parancs. 

Javítás:

* A Megoldáskezelőben kattintson a jobb gombbal a projekt hello, és válassza ki az Application Insights, konfigurálja az Application Insights. Hello párbeszédpanelen válassza ki toosend telemetriai tooan meglévő erőforrást, vagy hozzon létre egy újat. Vagy:
* Nyissa meg a hello erőforrás közvetlenül. Jelentkezzen be a túl[hello Azure-portálon](https://portal.azure.com), és az Application Insights hello bal oldali navigációs sávon kattintson, majd válassza ki az alkalmazást.

## <a name="where-do-i-find-my-telemetry"></a>Hol található a telemetriai adatokat?
*A toohello aláírt [Microsoft Azure-portálon](https://portal.azure.com), és szeretnék hello Azure otthoni irányítópult vagyok megnézi. Ezért hol található az Application Insights adataimat?*

* Hello bal oldali navigációs sávon kattintson az Application Insights részére, majd az alkalmazás neve. Ha bármely projektek nincs hiba, akkor túl[hozzáadásához vagy konfigurálásához az Application Insights a webes projekt](app-insights-asp-net.md).
  
    Van néhány összefoglaló diagramok megjelenik. Kattinthat a rajtuk keresztül toosee további adatok találhatók.
* A Visual Studióban az alkalmazás hibakeresése, közben gombra hello Application insights szolgáltatással.

## <a name="q03"></a>Nincs server-adatok (vagy egyáltalán nem adatok)
*Szeretnék saját alkalmazás futott, és majd megnyitni hello Application Insights szolgáltatás a Microsoft Azure-ban, de minden hello diagramok megjelenítése "további hogyan toocollect..." vagy "Nincs beállítva."* Másik lehetőségként *csak nézet és a felhasználói adatok, de nem a server-adatok.*

* Futtassa az alkalmazást hibakeresési módban, a Visual Studio (F5). Használható hello alkalmazás így toogenerate néhány telemetriai adatokat. Ellenőrizze, hogy látja-e az események naplózása hello Visual Studio kimeneti ablakában. 
  
    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)
* Nyissa meg az Application Insights portál hello, [diagnosztikai keresési](app-insights-diagnostic-search.md). Adatok általában itt jelenik meg először.
* Hello frissítése gombra. hello panel rendszeres időközönként frissíti magát, de manuálisan is végrehajthatja. a nagyobb időtartomány hello frissítési időköze áll.
* Ellenőrizze, hogy hello instrumentation kulcsok felel meg. Hello fő panelen az alkalmazáshoz a hello Application Insights portál hello **Essentials** legördülő, nézze meg **Instrumentation kulcs**. Ezután a projekt a Visual Studio, nyissa meg az ApplicationInsights.config és hello található `<instrumentationkey>`. Ellenőrizze, hogy két kulccsal hello egyenlő. Ha nem:
  
  * Hello portálon kattintson az Application Insights, és keressen hello alkalmazás-erőforrást hello megfelelő kulcsot. vagy
  * A Visual Studio Solution Explorerben kattintson a jobb gombbal a projekt hello, és válassza az Application Insights konfigurálása. Hello app toosend telemetriai toohello jobb erőforrás visszaállítása.
  * Ha nem talál megfelelő kulcsok hello, ellenőrizze, hogy azok be hello azonos bejelentkezési hitelesítő adatok a Visual Studio toohello portal hasonlóan.
    
    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
* A hello [Microsoft Azure otthoni irányítópult](https://portal.azure.com), nézze meg hello szolgáltatás állapota térkép. Ha néhány riasztási jelzések, várjon, amíg azok vissza kellett volna tooOK majd zárja be és nyissa meg újra az Application Insights-alkalmazás paneljének.
* Ellenőrizze azt is [az állapot blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Adta meg a hello kód írása [kiszolgálóoldali SDK](app-insights-api-custom-events-metrics.md) , amelyek esetleg módosító hello instrumentation kulcs `TelemetryClient` példányok vagy a `TelemetryContext`? Vagy adott ír egy [szűrő vagy mintavételi](app-insights-api-filtering-sampling.md) , előfordulhat, hogy szűrés kimenő túl sok?
* A szerkesztett ApplicationInsights.config, gondosan ellenőrizze hello beállítását [TelemetryInitializers és TelemetryProcessors](app-insights-api-filtering-sampling.md). Egy nem megfelelően nevű típus vagy paraméter okozhat hello SDK toosend nincsenek adatok.

## <a name="q04"></a>Nincsenek a lapmegtekintések, böngészők, használati adatok
*Nézet Lapbetöltési idő, illetve hello böngésző vagy a használati paneleken kiszolgálói válaszidő és a kiszolgálói kérelmek diagramok lévő adatok, azonban adatot nem láthatók.*

hello adatok hello weblapok parancsfájlok származik. 

* Ha hozzáadta az Application Insights tooan meglévő webes projektet [tooadd hello parancsfájlok kézzel kell](app-insights-javascript.md).
* Győződjön meg arról, hogy az Internet Explorer kompatibilitási módban nem jelennek meg a helyet.
* Hello böngésző hibakeresési szolgáltatással (F12 az egyes böngészők, majd válassza a hálózati), amely adatokat küldi túl tooverify`dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Adatot nem függőségi vagy kivétel
Lásd: [– függőségi telemetria](app-insights-asp-net-dependencies.md) és [kivételtelemetria](app-insights-asp-net-exceptions.md).

## <a name="no-performance-data"></a>Nincs teljesítményadatokat
Teljesítményadatok (CPU, IO sebessége, és így tovább) érhető el [Java web services](app-insights-java-collectd.md), [Windows asztali alkalmazások](app-insights-windows-desktop.md), [IIS webes alkalmazások és szolgáltatások telepítése állapotfigyelő](app-insights-monitor-performance-live-website-now.md), és [Azure Felhőszolgáltatások](app-insights-azure.md). a beállítások kiszolgálók találhat.

Nem érhető el az Azure-webhelyek.

## <a name="no-server-data-since-i-published-hello-app-toomy-server"></a>Nincs (kiszolgáló) adat, mivel a közzétett alkalmazások toomy kiszolgálói hello
* Ellenőrizze, hogy ténylegesen másolt összes hello Microsoft. ApplicationInsights DLL-ek toohello server Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll együtt
* A tűzfal, lehetséges, hogy túl[nyissa meg az egyes TCP-portok](app-insights-ip-addresses.md#data-access-api).
* Ha rendelkezik toouse egy proxy toosend a vállalati hálózaton kívül, [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) a Web.config fájlban
* Windows Server 2008: Ellenőrizze, hogy telepítette a frissítést követő hello: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).

## <a name="i-used-toosee-data-but-it-has-stopped"></a>Használt toosee adatokat, de leállt
* Ellenőrizze a hello [állapot blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Elérte a havi kvótát, az adatokat? Nyissa meg a beállítások/kvóta hello és az árazás toofind ki. Ha igen, frissítse a csomagot, vagy további kapacitást kell fizetnie. Lásd: hello [séma árképzési](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="i-dont-see-all-hello-data-im-expecting"></a>Nem szerepel az összes hello adatok I vagyok vár
Ha az alkalmazás nagy mennyiségű adatot küld, és használja ASP.NET verzió 2.0.0-beta3 Application Insights SDK hello, vagy később, hello [adaptív mintavételi](app-insights-sampling.md) szolgáltatás működtetésében, valamint csak százalékaként a telemetriai adatok küldése. 

Bármikor letilthatja, de ez nem ajánlott. Mintavételi célja, hogy a kapcsolódó telemetriai adat megfelelően továbbítani, diagnosztikai célokra. 

## <a name="wrong-geographical-data-in-user-telemetry"></a>A felhasználó telemetriai helytelen földrajzi adatokat
hello város, régió és ország dimenziók IP-címek alapján, és nem minden esetben pontosak.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>„A metódus nem található” kivétel az Azure Cloud Servicesben futó rendszeren
A .NET 4.6-os verziójára készítette el az alkalmazást? Az Azure Cloud Services szerepkörei nem támogatják automatikusan a 4.6-os verziót. [Telepítse a 4.6-os verziót mindegyik szerepkörön](../cloud-services/cloud-services-dotnet-install-dotnet.md), mielőtt futtatná az alkalmazást.

## <a name="still-not-working"></a>Még nem folyamatban...
* [Application Insights fórum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

