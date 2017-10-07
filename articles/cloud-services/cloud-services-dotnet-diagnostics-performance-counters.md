---
title: "az Azure Diagnostics teljesítményszámlálók aaaUse |} Microsoft Docs"
description: "Teljesítményszámlálók Azure felhőszolgáltatások vagy a virtuális gép toofind szűk keresztmetszetei és a teljesítmény hangolására."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: fc4c61e9-d49d-4ed9-a32c-b91cdf851909
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/29/2016
ms.author: robb
ms.openlocfilehash: f3250816c01fc6e164a6aae48da5035845e6d2b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Hozzon létre és teljesítményszámlálók használata az Azure alkalmazásban
Ez a cikk ismerteti a hello előnyeit, és hogyan tooput teljesítményszámlálóinak az Azure alkalmazásba. Használhatja őket toocollect adatok, szűk keresztmetszetek található, és rendszer- és teljesítmény hangolására.

A Windows Server, az IIS és ASP.NET rendelkezésre álló teljesítményszámlálók is összegyűjthetők, és használja az Azure webes szerepkörök, a feldolgozói szerepkörök és a virtuális gépek toodetermine hello állapotát. Hozhat létre, és egyéni teljesítményszámlálók.  

Láthatja a teljesítményszámláló-adatok

1. Közvetlenül alkalmazásgazdán hello hello Teljesítményfigyelő eszközzel a távoli asztal használatával érhető el
2. A System Center Operations Manager hello Azure felügyeleti csomag használatával
3. A más hello elérő eszközök diagnosztikai adatok átvitele a tooAzure tároló. Lásd: [Store és diagnosztikai adatok megtekintése az Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) további információt.  

További információ az alkalmazás a hello hello teljesítményének figyelésével kapcsolatos [Azure-portálon](http://portal.azure.com/), lásd: [tooMonitor Cloud Services](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).

További részletes útmutatót létrehozása a naplózás és nyomkövetés stratégia és diagnosztika és egyéb technikák tootroubleshoot problémák és optimalizálhatja az Azure-alkalmazások, lásd: [fejlesztése Azure ajánlott eljárásai hibaelhárítás Alkalmazások](https://msdn.microsoft.com/library/azure/hh771389.aspx).

## <a name="enable-performance-counter-monitoring"></a>Engedélyezze a teljesítményszámláló megfigyeléséhez
Alapértelmezés szerint a teljesítményszámlálók nem engedélyezettek. Az alkalmazás vagy egy indítási tevékenységhez módosítania kell az ügynök konfigurációs tooinclude hello specifikus teljesítményszámlálóinak, hogy kívánja-e minden szerepkörpéldányhoz toomonitor hello alapértelmezett diagnosztika.

### <a name="performance-counters-available-for-microsoft-azure"></a>A Microsoft Azure rendelkezésre álló teljesítményszámlálók
Azure található rendelkezésre álló hello teljesítményszámlálók egy részét biztosítja a Windows Server, az IIS és ASP.NET verem hello. hello alábbi táblázat néhány hello teljesítményszámlálók különösen fontos az Azure-alkalmazások.

| Teljesítményszámláló-kategória: Objektum (példány) | Számláló neve | Referencia |
| --- | --- | --- |
| .NET CLR-kivételek (*globális*) |# Kiváltott kivétel / mp |Kivétel teljesítményszámlálói |
| .NET CLR-memória (*globális*) |% Töltött idő |Memóriateljesítmény-számlálók |
| ASP.NET |Az alkalmazások |ASP.NET teljesítményszámlálók |
| ASP.NET |Kérelem végrehajtási ideje |ASP.NET teljesítményszámlálók |
| ASP.NET |Megszakadt kérések |ASP.NET teljesítményszámlálók |
| ASP.NET |Munkavégző folyamata újraindul |ASP.NET teljesítményszámlálók |
| ASP.NET-alkalmazások (**teljes**) |Az összes kérelem |ASP.NET teljesítményszámlálók |
| ASP.NET-alkalmazások (**teljes**) |Kérelmek/másodperc |ASP.NET teljesítményszámlálók |
| Az ASP.NET 4.0.30319 |Kérelem végrehajtási ideje |ASP.NET teljesítményszámlálók |
| Az ASP.NET 4.0.30319 |Kérés várakozási ideje |ASP.NET teljesítményszámlálók |
| Az ASP.NET 4.0.30319 |Aktuális |ASP.NET teljesítményszámlálók |
| Az ASP.NET 4.0.30319 |Kérései |ASP.NET teljesítményszámlálók |
| Az ASP.NET 4.0.30319 |Visszautasított kérelmek száma |ASP.NET teljesítményszámlálók |
| Memory (Memória) |Rendelkezésre álló memória (MB) |Memóriateljesítmény-számlálók |
| Memory (Memória) |Bájtok |Memóriateljesítmény-számlálók |
| Processzor(_Total) |Processzor kihasználtsága |ASP.NET teljesítményszámlálók |
| TCPv4 |Kapcsolódási hibák |TCP-objektum |
| TCPv4 |Fennálló kapcsolatok |TCP-objektum |
| TCPv4 |Kapcsolatok alaphelyzetbe állítása |TCP-objektum |
| TCPv4 |Felosztja a küldött/mp |TCP-objektum |
| Hálózati Interface(*) |Fogadott bájtok/s |Hálózati kapcsolat objektum |
| Hálózati Interface(*) |Küldött bájtok/s |Hálózati kapcsolat objektum |
| Hálózati adapter (Microsoft virtuális gép Buszán hálózati Adapter _2) |Fogadott bájtok/s |Hálózati kapcsolat objektum |
| Hálózati adapter (Microsoft virtuális gép Buszán hálózati Adapter _2) |Küldött bájtok/s |Hálózati kapcsolat objektum |
| Hálózati adapter (Microsoft virtuális gép Buszán hálózati Adapter _2) |Összes bájt/mp |Hálózati kapcsolat objektum |

## <a name="create-and-add-custom-performance-counters-tooyour-application"></a>Hozzon létre és egyéni teljesítmény-számlálói tooyour alkalmazás hozzáadása
Azure támogatás toocreate rendelkezik, és egyéni teljesítményszámlálók a webes és feldolgozói szerepkörök módosításához. hello számlálók lehet használt tootrack és a figyelő az alkalmazás-specifikus viselkedését. Hozzon létre, és törli az egyéni teljesítményszámláló-kategóriák és a kulcsszavak indítási tevékenységhez, webes vagy feldolgozói szerepkör emelt jogosultsági szintű.

> [!NOTE]
> Kód, amely módosítást hajt végre toocustom teljesítményszámlálók kell emelt szintű engedélyekkel toorun. Ha hello kód egy webes vagy feldolgozói szerepkör, szerepkör hello tartalmaznia kell hello címke <Runtime executionContext="elevated" /> hello ServiceDefinition.csdef a fájlt hello szerepkör tooinitialize megfelelően.
>
>

Egyéni teljesítmény számláló tooAzure adattárolás hello diagnosztikai ügynök használatával küldhet.

standard teljesítmény számlálóadatok hello Azure feldolgozza hello állítja elő. A webes szerepkör vagy a feldolgozói szerepkör alkalmazás egyéni teljesítmény számlálóadatok kell létrehoznia. Lásd: [teljesítmény számlálótípusok](https://msdn.microsoft.com/library/z573042h.aspx) egyéni teljesítményszámlálóit tárolt adatok hello típusú információk. Lásd: [PerformanceCounters minta](http://code.msdn.microsoft.com/azure/) által létrehozott, és beállítja az egyéni teljesítmény számlálóadatok webes szerepkörrel rendelkező példát.

## <a name="store-and-view-performance-counter-data"></a>Tároló és a nézet teljesítményszámláló-adatok
Azure gyorsítótárazza a teljesítményszámláló-adatok más diagnosztikai információkkal. Ezek az adatok érhető el távoli megfigyelési hello szerepkörpéldány távoli asztal eléréséhez tooview eszközökkel, például a Teljesítményfigyelő futása közben. toopersist hello kívül hello szerepkörpéldányt, hello diagnosztikai ügynök kell adatátvitel hello tooAzure tárolására. gyorsítótárazott hello teljesítmény számlálóadatok hello mérete konfigurálható hello diagnosztikai ügynök, vagy lehet, hogy a megosztott korlátozása toobe részét konfigurált összes hello diagnosztikai adatok. Hello pufferméret beállításával kapcsolatos további információkért lásd: [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) és [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). Lásd: [Store és diagnosztikai adatok megtekintése az Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) hello diagnosztikai ügynök tootransfer adatok tooa tárfiók beállítása áttekintését.

Minden egyes konfigurált teljesítményszámláló-példány a megadott mintavételi díj rögzítése, és mintát hello adatátvitel toohello tárfiók egy ütemezett átviteli kérelem vagy egy igény szerinti átviteli kérelmet. Automatikus átvitelek percenkénti gyakorisággal ütemezhető. Hello diagnosztikai ügynök által átadott teljesítményszámláló-adatokat egy olyan táblázatában, WADPerformanceCountersTable, hello tárfiók tárolódik. Ez a táblázat előfordulhat, hogy érhetők el, és megkérdezi a szabványos az Azure storage API-módszerekkel együtt. Lásd: [Microsoft Azure PerformanceCounters minta](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) kérdez le, és a teljesítmény számlálóadatok hello WADPerformanceCountersTable táblából megjelenítése egy példát.

> [!NOTE]
> Attól függően, hogy hello diagnosztikai ügynök átviteli gyakoriságát és várólista késés hello legutóbbi teljesítményszámláló-adatokat hello tárfiókban lévő több percet is elavult.
>
>

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Engedélyezze a teljesítményszámlálók diagnosztika konfigurációs fájl használata
A következő eljárás tooenable teljesítményszámlálók az Azure alkalmazásban hello használata.

## <a name="prerequisites"></a>Előfeltételek
Jelen szakaszban feltételezzük, hogy hello diagnosztikai figyelő importálni az alkalmazást, és hozzá hello diagnosztika konfigurációs fájl tooyour Visual Studio-megoldás (SDK 2.4 és az alacsonyabb diagnostics.wadcfg vagy diagnostics.wadcfgx az SDK 2.5-ös vagy újabb). Lásd: 1. és 2 [diagnosztika engedélyezésével az Azure Cloud Services és a virtuális gépek](cloud-services-dotnet-diagnostics.md)) olvashat.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>1. lépés: Gyűjt, és a teljesítményszámlálók adatainak tárolásához
Miután hozzáadta a hello diagnosztika fájl tooyour Visual Studio megoldás, hello gyűjtése és teljesítményszámláló-adatok tárolása konfigurálhatja az Azure alkalmazásban. Ez történik, teljesítményszámláló toohello diagnosztika fájljának hozzáadásával. Diagnosztikai adatok, beleértve a teljesítményszámlálókat, először gyűjtött hello példányon. hello adatokat, majd az Azure Table szolgáltatás hello tábla megőrzött toohello WADPerformanceCountersTable, úgy fogja is kell toospecify hello storage-fiók alkalmazásában. Ha a tesztelést az alkalmazás helyi a Compute Emulator hello, is tárolhatja diagnosztikai adatok helyi Storage Emulator hello. Diagnosztikai adatok vannak tárolva, mielőtt először kell lépnie toohello [Azure-portálon](http://portal.azure.com/) , és hozzon létre egy hagyományos tárolási fiókot. Az ajánlott eljárás az toolocate a storage-fiókot geo-és ugyanazon a helyen az Azure-alkalmazásában hello. Által tartása hello Azure-alkalmazás és a tárolási fiók vannak a hello azonos földrajzi helyhez, a külső sávszélességgel kapcsolatos költségek megfizetése alól, és a késés csökkentésére.

### <a name="add-performance-counters-toohello-diagnostics-file"></a>Teljesítményszámláló toohello diagnosztika fájljának hozzáadása
Használhatja sok számláló áll rendelkezésre. hello alábbi példában több teljesítményszámlálót mutat be, javasolt a webes és feldolgozói szerepkör figyelését.

Nyissa meg a hello diagnosztika fájl (diagnostics.wadcfg SDK 2.4 és az alatt vagy diagnostics.wadcfgx az SDK 2.5-ös vagy újabb), és adja hozzá a következő toohello DiagnosticMonitorConfiguration elem hello:

```xml
<PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
    <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use hello Process(w3wp) category counters in a web role -->

    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use hello Process(WaWorkerHost) category counters in a worker role.
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
</PerformanceCounters>
```

hello bufferQuotaInMB attribútum, amely hello fájl rendszerhez szükséges tárhelyet, amelyet hello adatok gyűjteménytípus (Azure naplóit, IIS-naplóit, stb.) a maximális időt adja meg. hello alapértelmezett érték 0. Hello kvóta elérése után hello legrégebbi adat törlődik, mivel az új adatok kerülnek. az összes hello bufferQuotaInMB tulajdonság hello összege hello hello OverallQuotaInMB attribútum értékének nagyobbnak kell lennie. Mennyi tárhelyre lesz szükség a diagnosztikai adatok gyűjtésére hello meghatározásának részletes tárgyalását lásd: a telepítő ÜVEGVATTA szakasza hello [hibaelhárítási ajánlott eljárások Azure-alkalmazások fejlesztése](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).

hello scheduledTransferPeriod attribútumot, amely meghatározza az adatok továbbítása ütemezett hello időközétől, a függvény toohello legközelebbi perc. A következő példák hello, az érték tooPT30M (30 perc). Hello átviteli időszak tooa kis beállításérték, például 1 perc, kedvezőtlen hatással van az alkalmazás teljesítményére éles környezetben, de diagnosztika gyorsan működik a tesztelés során hasznos lehet. hello ütemezett átviteli időszakot kell, hogy diagnosztikai adatok nincs-e írva hello példányon, de elég nagy ahhoz, hogy nincs hatással az alkalmazás teljesítményének hello elég kis tooensure.

hello counterSpecifier attribútum hello teljesítmény számláló toocollect határozza meg. hello sampleRate attribútum határozza meg a hello sebesség, amellyel hello teljesítményszámláló kell mintát venni, ebben az esetben 30 másodperc.

Teljesítményszámlálók hello megjeleníteni kívánt toocollect felvételét, az módosítások toohello diagnosztika fájl mentéséhez. A következő lépésben toospecify hello tárfiókot, amely hello diagnosztikai adatokat fog maradnak meg.

### <a name="specify-hello-storage-account"></a>Adja meg a tárfiók hello
a diagnosztikai információ tooyour Azure Storage fiók, meg kell adnia toopersist egy kapcsolati karakterláncot a szolgáltatás konfigurációs (ServiceConfiguration.cscfg) fájlban.

Az Azure SDK 2.5 hello Tárfiók hello diagnostics.wadcfgx fájl adható meg.

> [!NOTE]
> Ezek az utasítások csak akkor jutnak érvényre tooAzure SDK 2.4 vagy régebbi verzió. Az Azure SDK 2.5 hello Tárfiók hello diagnostics.wadcfgx fájl adható meg.
>
>

tooset hello kapcsolati karakterláncok:

1. Nyissa meg a kedvenc szövegszerkesztőjével és set hello kapcsolati karakterláncot használ a tárolási hello ServiceConfiguration.Cloud.cscfg fájlt. Hello *AccountName* és *AccountKey* értékek találhatók hello hello tárolási fiók irányítópulton, a Tárelérési kulcsok Azure-portálon.

    ```xml
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Hello ServiceConfiguration.Cloud.cscfg fájl mentéséhez.
3. Nyissa meg hello ServiceConfiguration.Local.cscfg fájlt, és ellenőrizze, hogy UseDevelopmentStorage tootrue.

    ```xml
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
   Most, hogy hello kapcsolati karakterláncok van beállítva, az alkalmazás diagnosztikai adatok tooyour tárfiók megmaradnak, az alkalmazás központi telepítésekor.
4. Mentse és a projekt buildjének elkészítéséhez, majd az alkalmazás központi telepítése.

## <a name="step-2-optional-create-custom-performance-counters"></a>2. lépés: (Nem kötelező) hozzon létre egyéni teljesítményszámlálói
Ezenkívül toohello előre meghatározott teljesítményszámlálókat, a saját egyéni teljesítményszámlálóinak toomonitor webes vagy feldolgozói szerepköröket is hozzáadhat. Egyéni teljesítményszámlálóit is használt tootrack és a figyelő az alkalmazás-specifikus viselkedését kell és létrehozhatók, illetve indítási tevékenységhez, webes vagy feldolgozói szerepkör emelt jogosultsági szintű törölt.

hello Azure diagnostics ügynök frissítése hello teljesítmény számláló konfigurációs hello .wadcfg fájlból egy percig elindítása után.  Ha egyéni teljesítményszámlálóit hello OnStart metódus hoz létre, és az indítási feladatok tovább tart, mint egy perc tooexecute, az egyéni teljesítményszámlálóit fog nem lett létrehozva amikor hello Azure diagnosztikai ügynök tooload őket.  Ebben a forgatókönyvben láthatja, hogy Azure Diagnostics megfelelően rögzíti az egyéni teljesítményszámlálóit kivételével minden diagnosztikai adatokat.  tooresolve probléma hello teljesítményszámlálók indítása feladat létrehozása, vagy helyezze át néhány a indítási tevékenységhez toohello OnStart metódus működik hello teljesítményszámlálók létrehozása után.

Hajtsa végre a következő lépéseket toocreate egy egyszerű egyéni teljesítmény "\MyCustomCounterCategory\MyButton1Counter" számláló nevű hello:

1. Nyissa meg a hello szolgáltatásdefiníciós fájl (CSDEF) az alkalmazáshoz.
2. Adja hozzá a hello futásidejű elem toohello vagy webrole típusról a WorkerRole elem tooallow végrehajtási megemelt jogosultságokkal:

    ```xml
    <runtime executioncontext="elevated"/>
    ```
3. Hello fájl mentéséhez.
4. Nyissa meg a hello diagnosztika fájl (diagnostics.wadcfg SDK 2.4 és az alatt vagy diagnostics.wadcfgx az SDK 2.5-ös vagy újabb), és adja hozzá a következő toohello DiagnosticMonitorConfiguration hello

    ```xml
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
      <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Hello fájl mentéséhez.
6. Hozzon létre egyéni teljesítményszámláló-kategória hello hello OnStart metódus a szerepkör talál meghívása előtt. OnStart. hello következő C# példa hoz létre egy egyéni kategória, ha még nem létezik:

    ```csharp
    public override bool OnStart()
    {
      if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
      {
         CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

         // add a counter tracking user button1 clicks
         CounterCreationData operationTotal1 = new CounterCreationData();
         operationTotal1.CounterName = "MyButton1Counter";
         operationTotal1.CounterHelp = "My Custom Counter for Button1";
         operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
         counterCollection.Add(operationTotal1);

         PerformanceCounterCategory.Create(
           "MyCustomCounterCategory",
           "My Custom Counter Category",
           PerformanceCounterCategoryType.SingleInstance, counterCollection);

         Trace.WriteLine("Custom counter category created.");
      }
      else {
        Trace.WriteLine("Custom counter category already exists.");
      }

    return base.OnStart();
    }
    ```
7. Frissítések hello számlálói az alkalmazáson belül. a következő példa hello Button1_Click események egyéni teljesítményszámláló frissítése:

    ```csharp
    protected void Button1_Click(object sender, EventArgs e)
    {
      PerformanceCounter button1Counter = new PerformanceCounter(
        "MyCustomCounterCategory",
        "MyButton1Counter",
        string.Empty,
        false);
      button1Counter.Increment();
      this.Button1.Text = "Button 1 count: " +
        button1Counter.RawValue.ToString();
    }
    ```
8. Hello fájl mentéséhez.  

Egyéni teljesítmény számlálóadatok most hello Azure diagnostics figyelő gyűjtenek.

## <a name="step-3-query-performance-counter-data"></a>3. lépés: A teljesítményszámláló-adatok lekérdezése
Ha az alkalmazás telepítve van és fut, hello diagnosztikai figyelő megkezdődik teljesítményszámlálók gyűjtése és megőrzése, hogy adattárolás tooAzure. Eszközök, például a Server Explorer használhatja a Visual Studio [Azure Tártallózó](http://azurestorageexplorer.codeplex.com/), vagy [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) által Cerebrata tooview hello teljesítményszámlálóinak hello adatok WADPerformanceCountersTable tábla. Szoftveresen is lekérheti hello tábla szolgáltatás használatával [C#](../cosmos-db/table-storage-how-to-use-dotnet.md), [Java](../cosmos-db/table-storage-how-to-use-java.md), [Node.js](../cosmos-db/table-storage-how-to-use-nodejs.md), [Python](../cosmos-db/table-storage-how-to-use-python.md), [Ruby](../cosmos-db/table-storage-how-to-use-ruby.md), vagy [PHP](../cosmos-db/table-storage-how-to-use-php.md).

hello C# alábbi példa mutatja egy alapszintű hello WADPerformanceCountersTable tábla lekérdezése, és menti hello diagnosztikai adatok tooa CSV-fájl. Miután hello teljesítményszámlálók tooa CSV-fájlba menti, a Microsoft Excel alkalmazást vagy valamilyen más eszköz toovisualize hello adatok képességei grafikonozás hello is használhatja. Lehet, hogy tooadd egy hivatkozási tooMicrosoft.WindowsAzure.Storage.dll, a .NET október 2012-es és újabb verziók hello Azure SDK részét képező. hello szerelvény telepített toohello % Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ könyvtárban.

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Table;
...

// Get hello connection string. When using Microsoft Azure Cloud Services, it is recommended
// you store your connection string using hello Microsoft Azure service configuration
// system (*.csdef and *.cscfg files). You can you use hello CloudConfigurationManager type
// tooretrieve your storage connection string.  If you're not using Cloud Services, it's
// recommended that you store hello connection string in your web.config or app.config file.
// Use hello ConfigurationManager type tooretrieve your storage connection string.

string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
//string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

// Get a reference toohello storage account using hello connection string.  You can also use hello development
// storage account (Storage Emulator) for local debugging.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
//CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "WADPerformanceCountersTable" table.
CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

// Create hello table query, filter on a specific CounterName, DeploymentId and RoleInstance.
TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
  .Where(
    TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
      TableOperators.And,
      TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
      TableOperators.And,
      TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
    )
  )
);

// Execute hello table query.
IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

// Process hello query results and build a CSV file.
StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

foreach (PerformanceCountersEntity entity in result)
{
  sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
    + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
}

StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
sw.Write(sb.ToString());
sw.Close();
```

Entitások leképezése tooC # objektumok egy származtatott egyéni osztály használatával **TableEntity**. hello alábbi kód meghatároz egy entitásosztályt hello teljesítményszámlálója jelölő **WADPerformanceCountersTable** tábla.

```csharp
public class PerformanceCountersEntity : TableEntity
{
  public long EventTickCount { get; set; }
  public string DeploymentId { get; set; }
  public string Role { get; set; }
  public string RoleInstance { get; set; }
  public string CounterName { get; set; }
  public double CounterValue { get; set; }
}
```

## <a name="next-steps"></a>Következő lépések
[Az Azure Diagnostics további cikkek megtekintése](../azure-diagnostics.md)
