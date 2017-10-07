---
title: "élő webalkalmazások aaaProfiling Azure az Application insights szolgáltatással |} Microsoft Docs"
description: "Egy kis erőforrásigénnyel Profilkészítő segítségével azonosíthatók a hello a gyakran használt adatok elérési útja, a web server kódban."
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
ms.openlocfilehash: 3c7f21076f19335e0f006327932e13623ec9526b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="profiling-live-azure-web-apps-with-application-insights"></a>Profilkészítési élő Azure-webalkalmazásokban az Application insights szolgáltatással

*Ez a szolgáltatás az Application Insights App Services és a számítás előzetes verziója.*

Annak megállapítása, mennyi idő telhet el az egyes módszerek élő webalkalmazásban hello adatgyűjtési az eszköz használatával [Azure Application Insights](app-insights-overview.md). Bemutatja, hogy az alkalmazás által kiszolgált volt élő kérelem részletes profilok, és kiemeli a hello "gyakran használt adatok elérési út" által használt hello legtöbb időt. Példák, amelyek különböző válaszidők automatikusan kijelöli. hello Profilkészítő használ a különböző módszereket toominimize terhelést.

hello Profilkészítő jelenleg működik az ASP.NET webalkalmazások a Azure App Services, az IP-címek alapszintű hello legalább. 

<a id="installation"></a>
## <a name="enable-hello-profiler"></a>Hello Profilkészítő engedélyezése

[Telepítse az Application Insights](app-insights-asp-net.md) a kódban. Ha már telepítve van, győződjön meg arról hogy hello legújabb verziót. (toodo, kattintson a jobb gombbal a projektben a Megoldáskezelőre, és válassza a kezelése NuGet-csomagok. Válassza ki a frissítéseket, és minden csomagjainak.) Hozza létre újra az alkalmazást.

*ASP.NET Core segítségével? [Jelölje be,](#aspnetcore).*

A [https://portal.azure.com](https://portal.azure.com), nyissa meg a webalkalmazás hello Application Insights-erőforrást. Nyissa meg **teljesítmény** kattintson **Application Insights Profilkészítő engedélyezése...** .

![Kattintson a hello Profilkészítő szalagcím engedélyezése][enable-profiler-banner]

Azt is megteheti, hogy mindig kattintva **konfigurálása** tooview állapot engedélyezheti vagy letilthatja a hello Profiler.

![Hello teljesítmény panelen kattintson a Konfigurálás gombra.][performance-blade]

Az Application insights szolgáltatással konfigurált webalkalmazások konfigurálása panelen láthatók. Kövesse az utasításokat tooinstall hello Profilkészítő ügynök szükség esetén. Ha nem a webes alkalmazás még nincs beállítva az Application insights szolgáltatással, kattintson a *csatolt alkalmazások felvétele*.

Használjon hello *engedélyezni Profilkészítő* vagy *tiltsa le a Profilkészítő* gombok hello konfigurálása panel toocontrol a Profilkészítő hello a csatolt összes webalkalmazásokban.



![Konfigurálja a panelen][linked app services]

toostop vagy újraindításával hello Profilkészítő egy egyedi App Service-példány, találhatja meg **az App Service-erőforrást hello**, a **webes feladatok**. toodelete azt, kattintson duplán a **bővítmények**.

![Egy webes feladatok Profilkészítő letiltása][disable-profiler-webjob]

Azt javasoljuk, hogy rendelkezik-e minden a web apps toodiscover minél hamarabb kapcsolatos bármilyen teljesítményproblémák engedélyezni Profilkészítő hello.

WebDeploy toodeploy módosítások tooyour webes alkalmazás használatakor győződjön meg arról, hogy kizárja hello **App_Data** mappa üzembe helyezése során kelljen törölni. Ellenkező esetben hello Profilkészítő szerepkörbővítmény-fájlokat törli hello webes alkalmazás tooAzure következő telepítésekor.

### <a name="using-profiler-with-azure-vms-and-compute-resources-preview"></a>Profilkészítő használata Azure virtuális gépek és a számítási erőforrások (előzetes verzió)

Ha Ön [Application Insights engedélyezése az Azure app Service szolgáltatások futási időben](app-insights-azure-web-apps.md#run-time-instrumentation-with-application-insights), automatikusan érhető el Profiler. (Ha már engedélyezve van az Application Insights hello erőforrás, akkor módosítania kell tooupdate toohello lates verzióból hello **konfigurálása** varázslót.)

Van egy [előzetes verzióját, az Azure számítási erőforrások Profilkészítő hello](https://go.microsoft.com/fwlink/?linkid=848155).


## <a name="limits"></a>Korlátok

hello alapértelmezett adatmegőrzés érték 5 nap. Legfeljebb 10 GB-os napi okozhatnak.

Nincs hello Profilkészítő szolgáltatás díjmentes. A webalkalmazás kell futnia az App Services szolgáltatás alapszintű rétegben legalább hello.

## <a name="viewing-profiler-data"></a>Profilkészítő adatainak megtekintése

Nyissa meg a hello teljesítmény panel megnyitásához, és görgessen lefelé toohello művelet listában.




![Application Insights teljesítmény panel példák oszlop][performance-blade-examples]

hello tábla oszlopainak hello a következők:

* **Count** -hello hello időtartománya hello panelen a kérelmek száma.
* **Medián** -hello tipikus idejét az alkalmazás toorespond tooa kérelmet. Minden válasz felének volt gyorsabb, mint ez.
* **95. percentilis** 95 %-a válaszok gyorsabb, mint ez volt. Ha ezen az ábrán nem nagyon hello medián, valószínűleg probléma az alkalmazással. (Vagy egy Tervező szolgáltatás, például a gyorsítótárazás előfordulhat, hogy viszonylag.)
* **Példák** -ikon jelzi az adott hello Profilkészítő rendelkezik captured híváslánc megjelenik az ezt a műveletet.

Kattintson a hello példák ikon tooopen hello nyomkövetési explorer. hello explorer mutat be több mintát, amely a Profilkészítő hello rendelkezik captured, minősített válaszidő alapján.

Válassza ki a minta tooshow időt töltött végrehajtó hello kérelem kód szintű részletes információkat.

![Application Insights nyomkövetési Explorer][trace-explorer]

**Működés közbeni elérési megjelenítése** legnagyobb Levélcsomópont hello, vagy valami legalább bezárása megnyitása. A legtöbb esetben ez a csomópont lesz szomszédos tooa teljesítménybeli szűk keresztmetszetek.



* **Címke**: hello függvény vagy esemény hello nevét. hello listáját jeleníti meg a kódot és az események, (például az SQL és a http-esemény). hello felső esemény jelöli hello teljes időtartam kérelem.
* **Eltelt**: hello alatt az időtartam alatt hello művelet hello kezdetét és hello vége között.
* **Amikor**: jeleníti meg, amikor hello függvény/esemény kapcsolat tooother funkciók futott.

## <a name="how-tooread-performance-data"></a>Hogyan tooread teljesítményadatokat

Microsoft szolgáltatásprofil-elemzőben metódus és instrumentation tooanalyze hello az alkalmazás teljesítményének mintavételi kombinációját alkalmazza.
Ha részletes gyűjtemény van folyamatban, a szolgáltatás Profilkészítő minták hello utasítás mutató hello gép CPU minden ezredmásodperces az egyes.
Minden minta hello teljes hívási verem hello szál jelenleg végrehajtás alatt álló, megadva a részletes és hasznos információt, hogy a szál műveletet végzett is magas és alacsony szintű absztrakció rögzíti. Más esemény, például a környezetben átváltását események, TPL eseményeket, és a szálkészlet események tootrack tevékenység korrelációs és okozatiság adatokat szolgáltatásprofil-elemzőben is gyűjti.

hello hívási verem hello idősor nézetben látható hello eredményét hello mintavételi és instrumentation felett. Minden minta hello teljes hívási verem hello szál írja le, mert ez magában foglalja a kód hello .NET-keretrendszer, valamint egyéb keretrendszerekre hivatkozik.

### <a id="jitnewobj"></a>Foglalási objektumazonosító (`clr!JIT\_New or clr!JIT\_Newarr1`)
`clr!JIT\_New and clr!JIT\_Newarr1`súgófunkciókat memóriáját is lefoglalja kezelt halommemóriában .NET-keretrendszer belül vannak. `clr!JIT\_New`nyílik meg, ha az objektum le van foglalva. `clr!JIT\_Newarr1`nyílik meg, ha egy objektum tömb le van foglalva. E két funkciók általában nagyon gyorsan és viszonylag kis mennyiségű időt kell végrehajtani. Ha látja `clr!JIT\_New` vagy `clr!JIT\_Newarr1` jelentős mennyiségű időt igénybe az ütemterven, hogy az arra utal, hogy, hogy hello kód sok objektum lefoglalása és jelentős mennyiségű memória felhasználása történhet.

### <a id="theprestub"></a>Kód betöltésekor (`clr!ThePreStub`)
`clr!ThePreStub`.NET-keretrendszer, amely hello kód tooexecute előkészíti hello először belül segítő függvény van. Ez általában tartalmaz, de nem kizárólagosan szerinti (csak az idő). Az egyes C# módszerek `clr!ThePreStub` akkor szabadna meghívni, legfeljebb egyszer folyamat hello élettartama során.

Ha látja `clr!ThePreStub` jelentős időt vesz igénybe kérelmet idő, azt jelzi, hogy a kérelem, amely végrehajtja az adott metódust, és a .NET-keretrendszer futásidejű tooload, hogy a metódus a jelentős időt hello elsőt hello. Érdemes lehet egy bemelegítési folyamat, amely végrehajtja a része, amely hello kódot, mielőtt a felhasználók hozzáférése, illetve a szerelvényekre NGen futtatja.

### <a id="lockcontention"></a>A zárolási versenyt (`clr!JITutil\_MonContention` vagy `clr!JITutil\_MonEnterWorker`)
`clr!JITutil\_MonContention`vagy `clr!JITutil\_MonEnterWorker` hello aktuális szál várakozik egy zárolás toobe, amely jelzi. Ez általában megjelenik egy C# zárolási utasítás, Monitor.Enter metódus, vagy a MethodImplOptions.Synchronized attribútummal rendelkező metódus végrehajtása közben. Zárolási versenyt általában akkor fordul elő, ha A szál zárolás, és a szál B megpróbál tooacquire hello azonos zárolása előtt A szál szabadítja.

### <a id="ngencold"></a>Kód betöltésekor (`[COLD]`)
Ha hello metódus neve tartalmaz `[COLD]`, például `mscorlib.ni![COLD]System.Reflection.CustomAttribute.IsDefined`, az azt jelenti, hogy hello .NET-keretrendszer futtatókörnyezete végrehajtja az kód által nem optimalizált <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">profil interaktív optimalizálási</a> hello első alkalommal a. Az egyes módszerek azt kell jelenik meg, legfeljebb egyszer hello folyamat hello élettartama során.

Ha jelentős mennyiségű időt a kérelmek kód betöltése időbe telik, azt jelzi, hogy a kérelem hello első egy tooexecute hello optimalizált része hello metódus. Érdemes lehet a meleg folyamatot, amely végrehajtja a része, amely hello kód, mielőtt a felhasználók hozzáférési jogosultsága.

### <a id="httpclientsend"></a>HTTP-kérelem küldése
Például módszerek `HttpClient.Send` hello kód arra vár, hogy a HTTP-kérelem toocomplete jelzi.

### <a id="sqlcommand"></a>Adatbázis-művelet
Módszer SqlCommand.Execute például azt jelzi, hogy hello kód egy adatbázis-művelet toocomplete vár.

### <a id="await"></a>Várakozás (`AWAIT\_TIME`)
`AWAIT\_TIME`azt jelzi, hogy hello kód arra vár, hogy egy másik feladat toocomplete. Ez általában akkor fordul elő a C# "await" utasítást. Amikor hello kód végez egy C# "await", hello szál unwinds és vezérlő toohello szálkészlet adja vissza, és nem szál várakozik hello "await" toofinish blokkolt van. Azonban hello logikailag szál, hogy adott hello await "blokkolva van" hello művelet toocomplete vár. A `AWAIT\_TIME` hello feladat toocomplete Várakozás blokkolva hello idejét jelzi.

### <a id="block"></a>Letiltott idő
`BLOCKED_TIME`azt jelzi, hogy hello kód arra vár, hogy egy másik erőforrás toobe érhető el, például a szinkronizálási objektumra Várakozás, Várakozás a szál toobe érhető el, vagy egy kérelem toofinish való várakozás közben.

### <a id="cpu"></a>CPU-idő
hello utasítások végrehajtása hello CPU túlterhelt.

### <a id="disk"></a>Lemezmeghajtó kihasználtsága
hello alkalmazás lemez műveleteket hajt végre.

### <a id="network"></a>Hálózati idő
hello alkalmazás hálózati műveleteket hajt végre.

### <a id="when"></a>Ha oszlop
Ez a képi megjelenítés, hogyan hello összegyűjtött egy csomópontot a határokat is beleértve minták időbeli eltérőek. hello hello kérelem teljes tartományának 32 idő gyűjtők oszlik, és ezek 32 gyűjtők felhalmozódnak hello inkluzív mintákat az adott csomópont. Minden egyes gyűjtő majd jelzi egy sávot, amelynek magasság egy méretezett értékét jelöli. Csomópontok megjelölve `CPU_TIME` vagy `BLOCKED_TIME`, vagy egy nyilvánvaló kapcsolata egy erőforrást (processzor, lemez- vagy szál) felhasználása esetén hello sáv fel ezeket az erőforrásokat egyik hello időn, hogy a gyűjtő jelöli. A metrikákat kaphat nagyobb 100 %-nál több erőforrást kötött. Például ha átlagosan két processzor arra az időtartamra, majd elérhetővé 200 %.


## <a id="troubleshooting"></a>Hibaelhárítás

### <a name="how-can-i-know-whether-application-insights-profiler-is-running"></a>Hogyan szerezhetek tudomást arról, hogy fut-e az Application Insights profiler?

hello Profilkészítő Web App alkalmazásban folyamatos webes feladatként fut. Nyissa meg a webes alkalmazás-erőforrást hello https://portal.azure.com, és a "ApplicationInsightsProfiler" állapotának hello webjobs-feladatok panelen. Ha a számítógépen nem fut, nyissa meg a **naplók** további toofind.

### <a name="why-cant-i-find-any-stack-examples-even-though-hello-profiler-is-running"></a>Miért nem található a verem példák annak ellenére, hogy fut-e hello profiler?

Az alábbiakban néhány dolgot ellenőrizheti.

1. Győződjön meg arról, hogy a Web App Service-csomag az alapszintű csomag vagy újabb verzió.
2. Ellenőrizze, hogy a webalkalmazás Application Insights SDK 2.2 Beta rendelkezik, és a fenti engedélyezve.
3. Ellenőrizze, hogy a webalkalmazás hello APPINSIGHTS_INSTRUMENTATIONKEY setting Application Insights SDK instrumentation ugyanazzal a kulccsal használt hello.
4. Győződjön meg arról, hogy a webalkalmazás működik a .net keretrendszer 4.6-os.
5. Ha az ASP.NET Core alkalmazás, azt is ellenőrizze [hello szükséges függőségek](#aspnetcore).

Hello Profilkészítő az elindítása után van egy rövid bemelegítési időszak, amikor hello Profilkészítő aktívan nyomkövetését több teljesítményét. Ezt követően hello Profilkészítő nyomkövetését teljesítmény óránként két percig.  

### <a name="i-was-using-azure-service-profiler-what-happened-tooit"></a>Azure Service Profilkészítő lett használata Milyen happened tooit?  

Ha engedélyezi az Application Insights Profilkészítő, Azure szolgáltatásprofil-elemzőben ügynök le van tiltva.

### <a id="double-counting"></a>A párhuzamos szálak számbavételi dupla

Az egyes esetekben hello teljes ideje hello verem megjelenítőben mérőszáma hello kérelem hello időtartama hosszabb.

Ez akkor fordulhat elő, ha a kérelmet, párhuzamosan működik társított két vagy több tábla. hello teljes szál ideje majd több mint hello eltelt idő. Sok esetben egy szál vár a hello más toocomplete. Ez viewer záma toodetect hello és hello érdektelen Várjon, amíg nincs megadva, de errs jelenít meg ahelyett, hogy mi lehet a kritikus információk kihagyásával túl sok hello oldalán.  

A nyomkövetések párhuzamos jelenik meg, úgy kell toodetermine, amely szálak várnak, így ellenőrizheti, hogy hello hello kérelem kritikus elérési útja. A legtöbb esetben hello szállal, amely gyorsan álljon a várakozási állapotba egyszerűen vár a szál más szálak hello. Arra összpontosíthatnék hello mások és figyelmen kívül hagyása hello várakozó szál hello idő.

### <a id="issue-loading-trace-in-viewer"></a>Profilkészítési adatok

1. Ha hello adatokat próbál tooview régebbi néhány hétre, próbálkozzon a időszűrője korlátozása és próbálja meg újból.

2. Ellenőrizze, hogy proxyk és a tűzfalon nem letiltott hozzáférés toohttps://gateway.azureserviceprofiler.net.

3. Ellenőrizze, hogy az Application Insights instrumentation kulcsot használ az alkalmazás hello azonos hello engedélyezve van a profilkészítési az Application Insights-erőforrást, hello. hello kulcs általában applicationinsights.config, de a web.config vagy az App.config fájlt is található.

### <a name="error-report-in-hello-profiling-viewer"></a>Hibajelentés a megtekintő-profilkészítés hello

A fájl egy támogatási jegy hello portálról. Tüntesse fel hello korrelációs azonosító hello hibaüzenet.

## <a name="manual-installation"></a>Manuális telepítés

Hello Profilkészítő konfigurálásakor hello következő frissítések végrehajtott toohello webes alkalmazás beállításait. Tegye őket saját kezűleg manuálisan, ha a környezet igényel, például ha az alkalmazás fut, az Azure App Service környezetben (ASE):

1. Hello webalkalmazás vezérlő panelen nyissa meg a beállításokat.
2. Állítsa be ".Net Framework version" toov4.6.
3. Állítsa be a "Mindig a" tooOn.
4. Alkalmazásbeállítás hozzáadása "__APPINSIGHTS_INSTRUMENTATIONKEY__" és a set hello érték toohello hello SDK által használt instrumentation ugyanazzal a kulccsal.
5. Nyissa meg a speciális eszközök.
6. Kattintson az "Ugrás" tooopen hello Kudu webhelyet.
7. Hello Kudu webhelyen válassza a "Hely extensions".
8. Telepítés "__az Application Insights__" gyűjteményből.
9. Hello a webalkalmazás újraindítása.

## <a id="aspnetcore"></a>Az ASP.NET Core támogatása

Az ASP.NET Core alkalmazás tooinstall Microsoft.ApplicationInsights.AspNetCore Nuget csomag 2.1.0-beta6 vagy magasabb toowork hello Profilkészítő a igényel. Hello alacsonyabb verziók 6/27/2017 után már nem támogatott.

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
