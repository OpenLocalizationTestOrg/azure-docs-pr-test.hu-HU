---
title: ".NET-alkalmazások Application Insights pillanatkép hibakereső aaaAzure |} Microsoft Docs"
description: "Debug pillanatképek vannak automatikusan gyűjti a program kivételek éles .NET-alkalmazásokban"
services: application-insights
documentationcenter: 
author: qubitron
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: bwren
ms.openlocfilehash: f0173a752b5795d934fbab1bd53eb077433edc90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a>A .NET-alkalmazásokban kivételek pillanatképek hibakeresése

Ha kivétel lép, a működés közbeni webes alkalmazás automatikusan begyűjtheti a hibakeresési pillanatképet. hello pillanatkép forráskód hello állapotát jeleníti meg, és változók: hello jelenleg hello kivétel lépett fel. hello pillanatkép hibakereső (előzetes verzió) a [Azure Application Insights](app-insights-overview.md) kivételtelemetria a webes alkalmazás figyeli. Pillanatképek a felső értesítő kivételek a összegyűjti az, hogy éles környezetben toodiagnose problémák szükséges hello információkat. Hello tartalmaznak [pillanatkép adatgyűjtő NuGet-csomag](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) az alkalmazásban, és szükség esetén adja meg a gyűjtemény paramétereit [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). A pillanatképek jelennek meg [kivételek](app-insights-asp-net-exceptions.md) hello Application Insights portálon.

A portál toosee hello hívásveremben hello hibakeresési pillanatképek tekintheti meg és vizsgálja meg az egyes Hívásiverem-keret változókat. a forráskódot, a Visual Studio 2017 vállalati által megnyitott pillanatképek hatékonyabb hibakeresési élmény tooget [hello pillanatkép hibakereső bővítmény letöltése a Visual Studio](https://aka.ms/snapshotdebugger).

Pillanatkép gyűjtemény érhető el:
* .NET-keretrendszer és az ASP.NET alkalmazások futó .NET-keretrendszer 4.5-ös vagy újabb.
* Windows rendszeren futó alkalmazások .NET core 2.0 és 2.0-s ASP.NET Core.

### <a name="configure-snapshot-collection-for-aspnet-applications"></a>Pillanatkép gyűjtemény az ASP.NET-alkalmazások konfigurálása

1. [Az Application Insights engedélyezéséhez a web app alkalmazásban](app-insights-asp-net.md), ha még nincs kész.

2. Hello tartalmaznak [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-csomag az alkalmazásban. 

3. Tekintse át a csomag túl hozzáadott hello hello alapértelmezett beállításokat[ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- hello default is true, but you can disable Snapshot Debugging by setting it toofalse -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this tootrue. -->
        <!-- DeveloperMode is a property on hello active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need toosee an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>5</ThresholdForSnapshotting>
        <!-- hello maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- hello maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often tooreset problem counters. -->
        <ProblemCounterResetInterval>06:00:00</ProblemCounterResetInterval>
        <!-- hello maximum number of snapshots allowed in one minute. -->
        <SnapshotsPerMinuteLimit>2</SnapshotsPerMinuteLimit>
        <!-- hello maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>50</SnapshotsPerDayLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. A pillanatképek összegyűjtése csak a kivételek, amelyek jelentett tooApplication Insights. Bizonyos esetekben (például hello .NET platform régebbi verzióknál), akkor módosítania kell túl[kivétel gyűjtemény konfigurálása](app-insights-asp-net-exceptions.md#exceptions) toosee kivételek hello portálon pillanatfelvételekkel.


### <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a>Az ASP.NET Core 2.0 alkalmazások pillanatkép-gyűjtésének konfigurálása

1. [Az Application Insights engedélyezése az ASP.NET Core web app alkalmazásban](app-insights-asp-net-core.md), ha még nincs kész.

2. Hello tartalmaznak [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-csomag az alkalmazásban.

3. Módosítsa a hello `ConfigureServices` módszer az alkalmazás `Startup` tooadd hello pillanatkép adatgyűjtő telemetriai processzor osztály. hello kódot, hozzá kell adnia a hello hivatkozott hello Microsoft.ApplicationInsights.ASPNETCore NuGet-csomag verziójának függ.

   Microsoft.ApplicationInsights.AspNetCore 2.1.0 hozzáadása:
   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   ...
   class Startup
   {
       // This method is called by hello runtime. Use it tooadd services toohello container.
       public void ConfigureServices(IServiceCollection services)
       {
           services.AddSingleton<Func<ITelemetryProcessor, ITelemetryProcessor>>(next => new SnapshotCollectorTelemetryProcessor(next));
           // TODO: Add any other services your application needs here.
       }
   }
   ```

   Microsoft.ApplicationInsights.AspNetCore 2.1.1 hozzáadása:
   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   ...
   class Startup
   {
       private class SnapshotCollectorTelemetryProcessorFactory : ITelemetryProcessorFactory
       {
           public ITelemetryProcessor Create(ITelemetryProcessor next) =>
               new SnapshotCollectorTelemetryProcessor(next);
       }

       // This method is called by hello runtime. Use it tooadd services toohello container.
       public void ConfigureServices(IServiceCollection services)
       {
            services.AddSingleton<ITelemetryProcessorFactory>(new SnapshotCollectorTelemetryProcessorFactory());
           // TODO: Add any other services your application needs here.
       }
   }
   ```

### <a name="configure-snapshot-collection-for-other-net-applications"></a>Pillanatkép gyűjtemény más .NET-alkalmazások konfigurálása

1. Ha az alkalmazás már nem tagolva az Application insights szolgáltatással, Kezdésként [engedélyezése az Application Insights és beállítási hello instrumentation kulcs](app-insights-windows-desktop.md).

2. Adja hozzá a hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-csomag az alkalmazásban.

3. A pillanatképek összegyűjtése csak a kivételek, amelyek jelentett tooApplication Insights. Szükség lehet toomodify a kód tooreport őket. hello kivételkezelő kód hello struktúra az alkalmazás függ, de például nem éri el:
    ```C#
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle hello request.
        }
        catch (Exception ex)
        {
            // Report hello exception tooApplication Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow hello exception if desired.
        }
   }
    ```
    
## <a name="grant-permissions"></a>Engedélyek

Azure-előfizetés hello tulajdonosainak pillanatképek vizsgálhatja meg. Más felhasználók tulajdonos engedéllyel kell rendelkezni.

toogrant engedély hozzárendelése hello `Application Insights Snapshot Debugger` szerepkör toousers, akik vizsgálja a pillanatképeket. Ehhez a szerepkörhöz is hozzárendelhető, tooindividual felhasználók vagy csoportok által előfizetésnél tulajdonos hello cél Application Insights-erőforrást, vagy az erőforráscsoportba vagy előfizetésbe.

1. Hello hozzáférés-vezérlés (IAM) panel megnyitásához.
1. Kattintson a hello + Hozzáadás gombra.
1. Válassza ki az Application Insights pillanatkép hibakereső hello szerepkörök legördülő listából.
1. Keresse meg, és adjon meg egy nevet hello felhasználói tooadd.
1. Kattintson a hello Mentés gombra tooadd hello toohello szerepkörben.


[!IMPORTANT]
    A pillanatképek potenciálisan tartalmazó változó és a paraméter értékét a személyes és más bizalmas adatokat.

## <a name="debug-snapshots-in-hello-application-insights-portal"></a>Pillanatképek hello Application Insights portál hibakeresési

Pillanatkép érhető el egy adott kivétel vagy probléma azonosítója, ha egy **Debug pillanatfelvétel megnyitása** hello gomb [kivétel](app-insights-asp-net-exceptions.md) hello Application Insights portálon.

![Nyissa meg Debug pillanatkép gombra a kivételei](./media/app-insights-snapshot-debugger/snapshot-on-exception.png)

Hello pillanatkép hibakeresési nézet láthatja, a hívási verem és a változók ablaktáblán. Keretek hello hívás a verem hello hívási verem ablaktáblán, megtekintheti a helyi változókat és paraméterek függvény hívása hello változók ablaktáblán.

![Nézet Debug pillanatkép hello portálon](./media/app-insights-snapshot-debugger/open-snapshot-portal.png)

A pillanatképek kényes információt is tartalmazhat, és alapértelmezés szerint nincsenek megtekinthető. tooview pillanatképeket, rendelkeznie kell hello `Application Insights Snapshot Debugger` szerepkörrel tooyou.

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a>A Visual Studio 2017 vállalati pillanatképek hibakeresése
1. Hello kattintson **letöltése pillanatkép** gomb toodownload egy `.diagsession` fájlt, amely a Visual Studio 2017 Enterprise nyithatja meg. 

2. tooopen hello `.diagsession` fájl, először [töltse le és telepítse a hello pillanatkép hibakereső bővítményt a Visual Studio](https://aka.ms/snapshotdebugger).

3. Hello pillanatfelvétel-fájl megnyitása után hello kis memóriakép Hibakeresés lap a Visual Studio jelenik meg. Kattintson a **felügyelt kód Debug** toostart hello pillanatkép hibakeresést. hello pillanatkép megnyitja toohello kódsort ahol hello kivétel lépett fel, hogy a hibakeresést tud hello folyamat hello aktuális állapotát.

    ![A Visual Studio nézet hibakeresési pillanatkép](./media/app-insights-snapshot-debugger/open-snapshot-visualstudio.png)

hello letöltött pillanatkép bármely alkalmazás webkiszolgálón található szimbólum fájlokat tartalmazza. A szimbólum fájlok szükséges tooassociate pillanatkép-adatok a forráskódot. App Service-alkalmazásokhoz győződjön meg arról, hogy tooenable szimbólum központi telepítés, a webalkalmazások közzétételekor.

## <a name="how-snapshots-work"></a>A pillanatképek működése

Az alkalmazás indításakor egy külön pillanatkép feltöltése folyamat jön létre, amely az alkalmazás a pillanatkép-kéréseket figyeli. Ha pillanatkép van szükség, folyamatának futtatása hello árnyékmásolatát too20 körülbelül 10 perc múlva történik. hello árnyékmásolat folyamat majd elemzésnek, és egy pillanatkép jön létre, miközben hello fő folyamat tovább toorun és ki tudja szolgálni forgalom toousers. hello pillanatkép nem feltöltött tooApplication Insights, valamint bármely megfelelő szimbólum (.pdb) fájlok, amelyek szükséges tooview hello pillanatkép.

## <a name="current-limitations"></a>Aktuális korlátozások

### <a name="publish-symbols"></a>Szimbólumok közzététele
hello pillanatkép hibakereső szimbólumfájlok hello üzemi kiszolgáló toodecode változók és a Visual Studio hibakeresési élményt tooprovide igényel. Visual Studio 2017 hello 15.2 kiadásának kibocsátási buildek szimbólumait alapértelmezés szerint közzéteszi tooApp szolgáltatás teszi közzé. A korábbi verziókban tooadd hello következőkre lesz szüksége a közzétételi profil sor tooyour `.pubxml` fájlt úgy, hogy a szimbólumok közzétett kiadási módban:

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

Az Azure számítási és egyéb, győződjön meg arról, hogy hello szimbólumfájlok hello hello fő alkalmazás .dll fájl ugyanabba a mappába (általában `wwwroot/bin`) vagy hello aktuális útvonalon érhetők el.

### <a name="optimized-builds"></a>Optimalizált buildek
Bizonyos esetekben helyi változók nem lehet megtekinteni a kiadási buildek hello felépítési folyamat során alkalmazott optimalizálásokat miatt.

## <a name="troubleshooting"></a>Hibaelhárítás

Ezek a tippek hello pillanatkép hibakereső kapcsolatos problémák hibaelhárítása érdekében.

### <a name="verify-hello-instrumentation-key"></a>Hello instrumentation kulcs ellenőrzése

Győződjön meg arról, hogy a közzétett alkalmazás hello megfelelő instrumentation kulcsot használ. Általában az Application Insights olvassa be az hello instrumentation kulcs hello ApplicationInsights.config fájlban. Győződjön meg arról, hogy hello érték van hello azonos kulcsaként hello instrumentation hello Application Insights-erőforrást, hogy megjelenik-e hello portálon.

### <a name="check-hello-uploader-logs"></a>Hello feltöltése naplófájlokban

Pillanatkép létrehozása után egy kis memóriakép fájl (.dmp) jön létre a lemezen. Egy külön feltöltése folyamat veszi, hogy kis memóriakép fájl, és feltölti azt, és minden társított PDB,-tooApplication Insights pillanatkép hibakereső tárolási fájlok. Után hello kis memóriakép sikeresen fel van töltve, hanem törli a lemezen. hello naplófájlok hello kis memóriakép feltöltése megőrzi a lemezen. Egy App Service environment-környezetben található a naplók a `D:\Home\LogFiles\Uploader_*.log`. Használjon hello Kudu felügyeleti webhely az App Service toofind ezek naplófájlok.

1. Nyissa meg az App Service alkalmazás hello Azure-portálon.

2. Jelölje be hello **speciális eszközök** panelen, vagy keressen a **Kudu**.
3. Kattintson a **Ugrás**.
4. A hello **hibakereső konzol** legördülő listáján jelölje ki **CMD**.
5. Kattintson a **naplófájlok**.

Legalább egy fájlt egy nevű kezdődő kell megjelennie `Uploader_` és egy `.log` bővítmény. Kattintson a megfelelő ikonra toodownload hello minden naplófájl, vagy egy böngészőben nyissa meg ezeket.
hello fájlnév hello gép nevét tartalmazza. Az App Service-példány egynél több számítógép üzemelteti, ha nincsenek az egyes gépek külön naplófájlokat. Ha hello feltöltése egy új kis memóriakép fájlt észlel, azt hello naplófájl rögzíti. Íme egy példa a hibátlan feltöltés:

```
MinidumpUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:08.0349846Z
MinidumpUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 329.12 MB
    DateTime=2017-05-25T14:25:16.0145444Z
MinidumpUploader.exe Information: 0 : Upload successful.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2017-05-25T14:25:44.2310982Z
MinidumpUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2017-05-25T14:25:44.5435948Z
MinidumpUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:44.6095821Z
```

Hello előző példában hello instrumentation kulcsa `c12a605e73c44346a984e00000000000`. Ezt az értéket meg kell felelnie hello instrumentation kulcs az alkalmazáshoz.
hello kis memóriakép rendelve egy pillanatkép hello azonosítójú `139e411a23934dc0b9ea08a626db16c5`. Ezt az Azonosítót használható újabb toolocate hello kapcsolódó Application Insights Analytics kivételtelemetria.

hello feltöltése 15 percenként egyszer kapcsolatos új PDB-fájlok keres. Íme egy példa:

```
MinidumpUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\home\site\wwwroot\ for local PDBs.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\local\Temporary ASP.NET Files\root\a6554c94\e3ad6f22\assembly\dl3\81d5008b\00b93cc8_dec5d201 for local PDBs.
    DateTime=2017-05-25T15:11:38.8160276Z
MinidumpUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2017-05-25T15:11:38.8316450Z
MinidumpUploader.exe Information: 0 : Deleted PDB scan marker D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\.pdbscan.
    DateTime=2017-05-25T15:11:38.8316450Z
```

Az alkalmazások, amelyek _nem_ hello feltöltése naplók vannak az App Service-ben üzemel hello mappában, amelyben a tömörített memóriaképek hello: `%TEMP%\Dumps\<ikey>` (ahol `<ikey>` a rendszerállapot-kulcs).

### <a name="use-application-insights-search-toofind-exceptions-with-snapshots"></a>Használja az Application Insights toofind kivételek pillanatképekkel keresése

Egy pillanatkép jön létre, ha Kivétel kiváltása hello címkéje egy pillanatkép. Jelentett tooApplication Insights – kivételtelemetria hello esetén, hogy a pillanatkép-azonosító szerepel egy egyéni tulajdonság. Hello az összes telemetriai adat található Application Insights hello Search paneljét használva `ai.snapshot.id` egyéni tulajdonság.

1. Keresse meg az Azure-portálon hello tooyour Application Insights-erőforrást.
2. Kattintson a **keresési**.
3. Típus `ai.snapshot.id` a keresőmezőben hello, és nyomja le az ENTER billentyűt.

![Keresse meg a telemetriai adatok hello portálon egy pillanatkép-azonosító](./media/app-insights-snapshot-debugger/search-snapshot-portal.png)

Ha ez a keresés eredménytelen, majd pillanatképek nem volt jelentett tooApplication Insights az alkalmazáshoz kijelölt hello időtartományba esik.

hello feltöltése naplókból adott pillanatkép azonosító toosearch azonosító hello keresőmezőbe írja be. Ha olyan pillanatképet, amely töltött fel tudja telemetriai adatok nem találhatók, kövesse az alábbi lépéseket:

1. Ellenőrizze, hogy a hello instrumentation kulcs ellenőrzésével hello jobb Application Insights-erőforrás tartózkodik.

2. Hello időbélyeg hello feltöltése naplóból használ, állítsa be úgy hello időtartomány szűrő hello keresési toocover az adott időtartományt.

Ha még nem látja a pillanatkép Azonosítóval rendelkező kivételek, hello kivételtelemetria jelentett tooApplication Insights nem volt. Ez a helyzet akkor fordulhat elő, ha az alkalmazás összeomlott után hello pillanatkép tartott, de előtt hello kivételtelemetria jelentette. Ebben az esetben ellenőrizze az App Service naplózza a hello `Diagnose and solve problems` toosee, ha váratlan volt újraindul, vagy nem kezelt kivételeket.

## <a name="next-steps"></a>Következő lépések

* [Állítsa be a snappoints a kódban](https://azure.microsoft.com/blog/snapshot-debugger-for-azure/) tooget pillanatképek kivétel várakozás nélkül.
* [Kivételek a webalkalmazások diagnosztizálásához](app-insights-asp-net-exceptions.md) ismerteti, hogyan toomake tooApplication Insights további kivételek látható. 
* [Észlelési intelligens](app-insights-proactive-diagnostics.md) automatikusan észleli a teljesítményanomáliákat.
