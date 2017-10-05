---
title: "Profilkészítési élő webalkalmazások Azure az Application insights szolgáltatással |} Microsoft Docs"
description: "A web server kód gyors elérési út egy kis erőforrásigénnyel Profilkészítő azonosításához."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: bwren
ms.openlocfilehash: ff39f9a84b86c14859aaee50ee368643fb2848ea
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="profiling-live-azure-web-apps-with-application-insights"></a>Profilkészítési élő Azure-webalkalmazásokban az Application insights szolgáltatással

*Ez a szolgáltatás az Application Insights App Services és a számítás előzetes verziója.*

Annak megállapítása, mennyi idő telhet el az egyes módszerek élő webalkalmazásban a profilkészítési eszközzel a [Azure Application Insights](app-insights-overview.md). Bemutatja, hogy az alkalmazás által kiszolgált volt élő kérelem részletes profilokat, és kiemeli a "gyors elérési út", amely a legtöbb időt használja. Példák, amelyek különböző válaszidők automatikusan kijelöli. A Profilkészítő terhek minimalizálása érdekében különböző módszereket használ.

A Profilkészítő ASP.NET web Apps rendszeren futó Azure App Service szolgáltatások legalább a Basic, IP-címek jelenleg működik. 

<a id="installation"></a>
## <a name="enable-the-profiler"></a>A Profilkészítő engedélyezése

[Telepítse az Application Insights](app-insights-asp-net.md) a kódban. Ha már telepítve van, győződjön meg arról, hogy a legújabb verzióra. (Ehhez kattintson a jobb gombbal a projektben a Megoldáskezelőre, és válassza a kezelése NuGet-csomagok parancsra. Válassza ki a frissítéseket, és minden csomagjainak.) Hozza létre újra az alkalmazást.

*ASP.NET Core segítségével? [Jelölje be,](#aspnetcore).*

A [https://portal.azure.com](https://portal.azure.com), nyissa meg a webalkalmazás az Application Insights-erőforrást. Nyissa meg **teljesítmény** kattintson **Application Insights Profilkészítő engedélyezése...** .

![Kattintson az engedélyezés Profilkészítő szalagcím][enable-profiler-banner]

Azt is megteheti, hogy mindig kattintva **konfigurálása** állapotának megtekintéséhez engedélyezheti vagy letilthatja a Profiler.

![A teljesítmény paneljén kattintson a Konfigurálás gombra.][performance-blade]

Az Application insights szolgáltatással konfigurált webalkalmazások konfigurálása panelen láthatók. Kövesse az utasításokat a Profilkészítő ügynök telepítése, ha szükséges. Ha nem a webes alkalmazás még nincs beállítva az Application insights szolgáltatással, kattintson a *csatolt alkalmazások felvétele*.

Használja a *engedélyezni Profilkészítő* vagy *tiltsa le a Profilkészítő* gombok szabályozására a Profilkészítőt a csatolt összes webalkalmazásokban konfigurálás paneljén található.



![Konfigurálja a panelen][linked app services]

A leállítani, vagy indítsa újra a Profilkészítő egy egyedi App Service-példány, találhatja meg **az App Service-erőforrás**, a **webes feladatok**. Törli-e, keresse meg a **bővítmények**.

![Egy webes feladatok Profilkészítő letiltása][disable-profiler-webjob]

Azt javasoljuk, hogy rendelkezik-e a Profilkészítő engedélyezve a webalkalmazások minél hamarabb felderítéséhez a teljesítménnyel kapcsolatos problémákat.

Ha WebDeploy használatával telepíti a módosításokat a webalkalmazáshoz, győződjön meg arról, hogy kizárja a **App_Data** mappa üzembe helyezése során kelljen törölni. Ellenkező esetben a Profilkészítő szerepkörbővítmény-fájlokat törli a webalkalmazás az Azure-bA következő telepítésekor.

### <a name="using-profiler-with-azure-vms-and-compute-resources-preview"></a>Profilkészítő használata Azure virtuális gépek és a számítási erőforrások (előzetes verzió)

Ha Ön [Application Insights engedélyezése az Azure app Service szolgáltatások futási időben](app-insights-azure-web-apps.md#run-time-instrumentation-with-application-insights), automatikusan érhető el Profiler. (Ha már engedélyezve van az Application Insights az erőforrás, szükség lehet frissíteni a lates verzióra keresztül a **konfigurálása** varázslót.)

Van egy [előzetes verzióját a Profilkészítő Azure számítási erőforrások](https://go.microsoft.com/fwlink/?linkid=848155).


## <a name="limits"></a>Korlátok

Az alapértelmezett az adatmegőrzés 5 nap. Legfeljebb 10 GB-os napi okozhatnak.

Nincs a Profilkészítő szolgáltatás díjmentes. A webalkalmazás kell lennie az App Services szolgáltatás alapszintű rétegben legalább tárolva.

## <a name="viewing-profiler-data"></a>Profilkészítő adatainak megtekintése

Nyissa meg a teljesítmény panel megnyitásához, és görgessen le a művelet listában.




![Application Insights teljesítmény panel példák oszlop][performance-blade-examples]

A tábla oszlopai a következők:

* **Count** -e a panel időtartománya kérelmek száma.
* **Medián** – az alkalmazás tipikus idejét válaszol a kérelemre. Minden válasz felének volt gyorsabb, mint ez.
* **95. percentilis** 95 %-a válaszok gyorsabb, mint ez volt. Ha ez a szám nagyon eltér a középérték, az alkalmazás a probléma lehet. (Vagy egy Tervező szolgáltatás, például a gyorsítótárazás előfordulhat, hogy viszonylag.)
* **Példák** -ikon jelzi, hogy a Profilkészítő rendelkezik captured híváslánc megjelenik az ezt a műveletet.

Kattintson a példák ikonra a nyomkövetési explorer megnyitásához. A explorer több mintáit tartalmazza, hogy a Profilkészítő rendelkezik captured, minősített válaszidő alapján.

Válassza ki a megjelenítendő kód szintű információkat töltött időt a kérelem végrehajtása a minta.

![Application Insights nyomkövetési Explorer][trace-explorer]

**Működés közbeni elérési megjelenítése** megnyílik a legnagyobbnak levél csomópont, vagy valami legalább bezárása. A legtöbb esetben ez a csomópont a teljesítménybeli szűk keresztmetszetek szomszédos lesz.



* **Címke**: a függvény vagy az esemény nevét. A listáját jeleníti meg a kódot és az események (például az SQL és a http-esemény), kombinációját. Az első esemény a kérelem összesített időtartamot jelöli.
* **Eltelt**: a művelet kezdete és vége közötti időközt.
* **Amikor**: jeleníti meg, ha a függvény/esemény futott kapcsolatos egyéb funkciók.

## <a name="how-to-read-performance-data"></a>Teljesítményadatok olvasása

Microsoft szolgáltatásprofil-elemzőben mintavételi módszer és a instrumentation használja az alkalmazás teljesítményének elemzését.
Ha részletes gyűjtemény van folyamatban, a szolgáltatásprofil-elemzőben minták a gép CPU minden ezredmásodperces az egyes utasítás mutató.
A mintákat a teljes hívási verem, a szál jelenleg végrehajtás alatt álló, megadva a részletes és hasznos információt, hogy a szál műveletet végzett is magas és alacsony szintű absztrakció rögzíti. Szolgáltatásprofil-elemzőben adatokat is gyűjti, más esemény, például a környezetben átváltását események TPL események és szálkészlet események tevékenység korrelációs és okozatiság nyomon követésére.

A hívási verem, az Ütemterv nézetben látható a fenti mintavételi és instrumentation eredménye. A mintákat a teljes hívási verem, a szál írja le, mert a .NET-keretrendszer, valamint egyéb keretrendszerekre hivatkozik-kódot tartalmaz.

### <a id="jitnewobj"></a>Foglalási objektumazonosító (`clr!JIT\_New or clr!JIT\_Newarr1`)
`clr!JIT\_New and clr!JIT\_Newarr1`súgófunkciókat memóriáját is lefoglalja kezelt halommemóriában .NET-keretrendszer belül vannak. `clr!JIT\_New`nyílik meg, ha az objektum le van foglalva. `clr!JIT\_Newarr1`nyílik meg, ha egy objektum tömb le van foglalva. E két funkciók általában nagyon gyorsan és viszonylag kis mennyiségű időt kell végrehajtani. Ha látja `clr!JIT\_New` vagy `clr!JIT\_Newarr1` jelentős mennyiségű időt igénybe az ütemterven, hogy az arra utal, hogy a kód sok objektum lefoglalása is, és jelentős mennyiségű memória fel.

### <a id="theprestub"></a>Kód betöltésekor (`clr!ThePreStub`)
`clr!ThePreStub`.NET-keretrendszer, amely előkészíti a kód végrehajtásához először belül segítő függvény van. Ez általában tartalmaz, de nem kizárólagosan szerinti (csak az idő). Az egyes C# módszerek `clr!ThePreStub` akkor szabadna meghívni, legfeljebb egyszer a folyamat élettartama során.

Ha látja `clr!ThePreStub` jelentős időt vesz igénybe a kérelem idő, azt jelzi, hogy vonatkozó kérés, mert az első címtárra, amely végrehajtja az adott metódus, és a .NET-keretrendszer Ez a módszer betöltése ideje jelentős. Érdemes lehet egy bemelegítési folyamat, amely végrehajtja a része, amely a kódot, mielőtt a felhasználók hozzáférése, illetve a szerelvényekre NGen futtatja.

### <a id="lockcontention"></a>A zárolási versenyt (`clr!JITutil\_MonContention` vagy `clr!JITutil\_MonEnterWorker`)
`clr!JITutil\_MonContention`vagy `clr!JITutil\_MonEnterWorker` jelzi, hogy az aktuális szál várakozik a zárolás feloldására. Ez általában megjelenik egy C# zárolási utasítás, Monitor.Enter metódus, vagy a MethodImplOptions.Synchronized attribútummal rendelkező metódus végrehajtása közben. Zárolási versenyt általában akkor fordul elő, ha szál A zárolás, és a B szál megpróbál a azonos zárolás megszerzésére előtt A szál szabadítja.

### <a id="ngencold"></a>Kód betöltésekor (`[COLD]`)
Ha a metódus nevét tartalmazza `[COLD]`, például a `mscorlib.ni![COLD]System.Reflection.CustomAttribute.IsDefined`, az azt jelenti, hogy a .NET-keretrendszer futtatókörnyezete végrehajtja az kód által nem optimalizált <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">profil interaktív optimalizálási</a> először. Az egyes módszerek azt kell jelenik meg, legfeljebb egyszer a folyamat élettartama során.

Ha jelentős mennyiségű időt a kérelmek kód betöltése időbe telik, azt jelzi, hogy a kérelem az elsőt a nem optimalizált része a metódus végrehajtásához. Érdemes lehet a meleg folyamatot, amely végrehajtja a része, amely a kódot, mielőtt a felhasználók hozzáférési jogosultsága.

### <a id="httpclientsend"></a>HTTP-kérelem küldése
Például módszerek `HttpClient.Send` jelzi, hogy a kód arra vár, hogy a HTTP-kérelem végrehajtásához.

### <a id="sqlcommand"></a>Adatbázis-művelet
Módszer SqlCommand.Execute például azt jelzi, hogy a kód arra vár, hogy egy adatbázis-művelet befejezéséhez.

### <a id="await"></a>Várakozás (`AWAIT\_TIME`)
`AWAIT\_TIME`azt jelzi, hogy a kód arra vár, hogy egy másik feladat befejeződik. Ez általában akkor fordul elő a C# "await" utasítást. Amikor a kód végez egy C# "await", a szál unwinds, és visszaadja a szálkészlet és nem szál várakozik az "await" befejezéséhez blokkolt van. Azonban logikailag a szál a await elvégző blokkolva van,' ' vár a művelet elvégzéséhez. A `AWAIT\_TIME` Várakozás a feladat befejezésére letiltott idejét jelzi.

### <a id="block"></a>Letiltott idő
`BLOCKED_TIME`azt jelzi, hogy a kód arra vár, hogy van, például érhető el, vagy a kérelem befejezésének Várakozás a Szálra várakozó szinkronizálási objektumra Várakozás egy másik erőforrás.

### <a id="cpu"></a>CPU-idő
A Processzor nem az utasításokat.

### <a id="disk"></a>Lemezmeghajtó kihasználtsága
Az alkalmazás lemez műveleteket hajt végre.

### <a id="network"></a>Hálózati idő
Az alkalmazás hálózati műveleteket hajt végre.

### <a id="when"></a>Ha oszlop
Ez a képi megjelenítés, a határokat is beleértve mintákat egy csomópont gyűjtése hogyan eltérők lehetnek adott idő alatt. A kérelem teljes skáláját 32 idő gyűjtők oszlik, és ezek 32 gyűjtők felhalmozódnak a határokat is beleértve mintákat az adott csomópont. Minden egyes gyűjtő majd jelzi egy sávot, amelynek magasság egy méretezett értékét jelöli. Csomópontok megjelölve `CPU_TIME` vagy `BLOCKED_TIME`, vagy fel egy erőforrást (processzor, lemez- vagy szál) egy nyilvánvaló kapcsolat esetén a sáv jelöli fel ezeket az erőforrásokat egyike az, hogy a gyűjtő mennyi ideig. A metrikákat kaphat nagyobb 100 %-nál több erőforrást kötött. Például ha átlagosan két processzor arra az időtartamra, majd elérhetővé 200 %.


## <a id="troubleshooting"></a>Hibaelhárítás

### <a name="how-can-i-know-whether-application-insights-profiler-is-running"></a>Hogyan szerezhetek tudomást arról, hogy fut-e az Application Insights profiler?

A Profilkészítő Web App alkalmazásban folyamatos webes feladatként fut. Nyissa meg a webes alkalmazás-erőforrást https://portal.azure.com, és ellenőrizze a webjobs-feladatok panel "ApplicationInsightsProfiler" állapotát. Ha a számítógépen nem fut, nyissa meg a **naplók** további.

### <a name="why-cant-i-find-any-stack-examples-even-though-the-profiler-is-running"></a>Miért nem található a verem példák annak ellenére, hogy fut-e a Profilkészítő?

Az alábbiakban néhány dolgot ellenőrizheti.

1. Győződjön meg arról, hogy a Web App Service-csomag az alapszintű csomag vagy újabb verzió.
2. Ellenőrizze, hogy a webalkalmazás Application Insights SDK 2.2 Beta rendelkezik, és a fenti engedélyezve.
3. Ellenőrizze, hogy a webalkalmazás az Application Insights SDK által használt azonos instrumentation kulccsal rendelkező APPINSIGHTS_INSTRUMENTATIONKEY beállítás.
4. Győződjön meg arról, hogy a webalkalmazás működik a .net keretrendszer 4.6-os.
5. Ha az ASP.NET Core alkalmazás, azt is ellenőrizze [a szükséges függőségek](#aspnetcore).

A Profilkészítő az elindítása után van egy rövid bemelegítési időszak, amikor a Profilkészítő aktívan nyomkövetését több teljesítményét. Ezt követően a Profilkészítő nyomkövetését teljesítmény óránként két percig.  

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Azure Service Profilkészítő lett használata Mi történt azt?  

Ha engedélyezi az Application Insights Profilkészítő, Azure szolgáltatásprofil-elemzőben ügynök le van tiltva.

### <a id="double-counting"></a>A párhuzamos szálak számbavételi dupla

A teljes időmetrika a verem megjelenítőben egyes esetekben ez több, mint a tényleges időtartama a kérelmet.

Ez akkor fordulhat elő, ha a kérelmet, párhuzamosan működik társított két vagy több tábla. Az összes szál ideje majd több, mint az eltelt idő. Sok esetben egy szálat a befejeződésére vár. A megjelenítő észleli ezt, és hagyja ki ezt az érdektelen várakozási próbálkozik, de errs jelenít meg ahelyett, hogy mi lehet a kritikus információk kihagyásával túl sok oldalán.  

A nyomkövetések párhuzamos jelenik meg, amikor meg kell határoznia, melyik szálak várnak, így ellenőrizheti, hogy a kritikus fontosságú a kérelem elérési útja. A legtöbb esetben a gyorsan várakozási állapotba kerül, a szál egyszerűen a más szálak vár. A többi összpontosít, és figyelmen kívül hagyja az idő a várakozó szál.

### <a id="issue-loading-trace-in-viewer"></a>Profilkészítési adatok

1. Ha a megtekinteni kívánt adatok néhány hétre régebbi, próbálkozzon a időszűrője korlátozása, és próbálkozzon újra.

2. Ellenőrizze, hogy proxyk és a tűzfalon nem letiltott https://gateway.azureserviceprofiler.net elérésére.

3. Ellenőrizze, hogy az Application Insights instrumentation kulcsot használ az alkalmazás ugyanaz, mint az Application Insights-erőforrást az adatgyűjtés engedélyezését. A kulcs általában applicationinsights.config, de a web.config vagy az App.config fájlt is található.

### <a name="error-report-in-the-profiling-viewer"></a>Hibajelentés a profilkészítési megjelenítőben

A fájl egy támogatási jegy a portálról. A hibaüzenet a korrelációs azonosító is.

## <a name="manual-installation"></a>Manuális telepítés

A Profilkészítő konfigurálásakor az alábbi frissítések válnak, a webes alkalmazás beállításai. Tegye őket saját kezűleg manuálisan, ha a környezet igényel, például ha az alkalmazás fut, az Azure App Service környezetben (ASE):

1. A webalkalmazás vezérlő panelen nyissa meg a beállításokat.
2. Állítsa be ".Net Framework version" v4.6 való.
3. "Always On" értékre állítva.
4. Alkalmazásbeállítás hozzáadása "__APPINSIGHTS_INSTRUMENTATIONKEY__", és állítsa be az értéket az SDK által használt azonos instrumentation kulcsot.
5. Nyissa meg a speciális eszközök.
6. Kattintson a "" nyissa meg a Kudu webhelyet.
7. A Kudu webhelyen válassza a "Hely extensions".
8. Telepítés "__az Application Insights__" gyűjteményből.
9. A webalkalmazás újraindítása.

## <a id="aspnetcore"></a>Az ASP.NET Core támogatása

Az ASP.NET Core alkalmazást kell telepíteni Microsoft.ApplicationInsights.AspNetCore Nuget csomag 2.1.0-beta6 vagy magasabb a Profilkészítő együttműködni. A korábbi verziók 6/27/2017 után már nem támogatott.

## <a name="next-steps"></a>Következő lépések

* [A Visual Studio Application Insights használata](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png
