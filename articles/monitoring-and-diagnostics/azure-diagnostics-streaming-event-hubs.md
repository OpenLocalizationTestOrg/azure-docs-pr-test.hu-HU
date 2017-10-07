---
title: "aaaStreaming Azure diagnosztikai adatokat hello gyors elérési út az Event Hubs használatával |} Microsoft Docs"
description: "Az Event Hubs konfigurálása Azure Diagnostics végén tooend, beleértve a közös forgatókönyvre vonatkozó útmutatást."
services: event-hubs
documentationcenter: na
author: rboucher
manager: carmonm
editor: 
ms.assetid: edeebaac-1c47-4b43-9687-f28e7e1e446a
ms.service: monitoring-and-diagnostics
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: robb
ms.openlocfilehash: a2528ddd0688d1c23a8631e769ca016dd79e4159
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="streaming-azure-diagnostics-data-in-hello-hot-path-by-using-event-hubs"></a>Adatfolyam-továbbítási Azure diagnosztikai adatokat hello gyors elérési út az Event Hubs használatával
Az Azure Diagnostics rugalmas lehetőségeket biztosítja a toocollect metrikákat, és naplózza a felhőalapú szolgáltatások virtuális gépek (VM) és átviteli eredmények tooAzure tároló. Hello 2016. március (SDK 2.9) időkereten belül-től kezdődően küldjön diagnosztikai toocustom adatforrások, gyors útvonal adatok átviteléhez az másodpercben használatával [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).

Támogatott adattípusok a következők:

* A Windows esemény-nyomkövetés (ETW) eseményei
* Teljesítményszámlálók
* Windows-Eseménynapló
* Alkalmazás-naplók
* Az Azure Diagnostics infrastruktúra naplók

Ez a cikk bemutatja, hogyan tooconfigure Azure Diagnostics az Event Hubs az end tooend. Emellett útmutatást a következő gyakori helyzetek hello:

* Hogyan naplózza az toocustomize hello és metrikákat, küldött tooEvent hubok
* Hogyan toochange beállításai minden környezetben
* Hogyan tooview Event Hubs adatok folyamatos átviteléhez
* Hogyan tootroubleshoot hello kapcsolat  

## <a name="prerequisites"></a>Előfeltételek
Event Hubs receieving adatait Azure Diagnostics Cloud Services, a virtuális gépek, a virtuálisgép-méretezési csoportok és a Service Fabric hello Azure SDK 2.9 és a Visual Studio Azure eszközök megfelelő hello kezdve támogatott.

* Az Azure Diagnostics bővítmény 1.6 ([Azure SDK for .NET 2.9 vagy újabb](https://azure.microsoft.com/downloads/) céloz ez alapértelmezés szerint)
* [A Visual Studio 2013 vagy újabb verzió](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* Egy alkalmazás használatával az Azure Diagnostics meglévő konfigurációk egy *.wadcfgx* fájl- és hello a következő módszerek egyikét:
  * A Visual Studio: [diagnosztika konfigurálása az Azure Felhőszolgáltatások és virtuális gépek](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
  * A Windows PowerShell: [a PowerShell használata Azure Cloud Services diagnosztika engedélyezése](../cloud-services/cloud-services-diagnostics-powershell.md)
* Esemény hubok névtér hello cikkenként kiépített [Bevezetés az Event Hubs használatába](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

## <a name="connect-azure-diagnostics-tooevent-hubs-sink"></a>Csatlakozás Azure Diagnostics tooEvent hubok fogadó
Alapértelmezés szerint Azure Diagnostics mindig a küldi naplók és a metrikák tooan Azure Storage-fiók. Egy alkalmazás egy új hozzáadásával is küldhetünk adatok tooEvent hubok **fogadók esetében** hello szakasza **PublicConfig** / **WadCfg** hello eleme*.wadcfgx* fájlt. A Visual Studio hello *.wadcfgx* fájl elérési útja a következő hello tárolja: **Felhőszolgáltatás-projekt** > **szerepkörök** > **() RoleName)** > **diagnostics.wadcfgx** fájlt.

```xml
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```
```JSON
"SinksConfig": {
    "Sink": [
        {
            "name": "HotPath",
            "EventHub": {
                "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
                "SharedAccessKeyName": "SendRule"
            }
        }
    ]
}
```

Ebben a példában hello eseményközpont URL-cím értéke toohello teljesen minősített hello az event hubs névtér: az Event Hubs névtér + "/" + eseményközpont neveként.  

URL-cím jelenik meg hello hello eseményközpont [Azure-portálon](http://go.microsoft.com/fwlink/?LinkID=213885) hello Event Hubs irányítópulton.  

Hello **gyűjtése** neve tooany érvényes karakterlánc beállítható, mindaddig, amíg hello ugyanazt az értéket használja a rendszer folyamatosan keresztül hello konfigurációs fájlban.

> [!NOTE]
> Előfordulhat, további felül, például a *applicationInsights* ebben a szakaszban konfigurálni. Az Azure Diagnostics lehetővé teszi, hogy egy vagy több fogadók esetében meg, ha mindegyik fogadó is deklarálva van hello toobe **PrivateConfig** szakasz.  
>
>

hello Event Hubs fogadó is kell deklarált és definiált hello **PrivateConfig** hello szakasza *.wadcfgx* konfigurációs fájlban.

```XML
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="{account name}" key="{account key}" endpoint="{optional storage endpoint}" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
</PrivateConfig>
```
```JSON
{
    "storageAccountName": "{account name}",
    "storageAccountKey": "{account key}",
    "storageAccountEndPoint": "{optional storage endpoint}",
    "EventHub": {
        "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "{base64 encoded key}"
    }
}
```

Hello `SharedAccessKeyName` értékének meg kell felelnie egy közös hozzáférésű Jogosultságkód (SAS) és egy házirendet, amely hello definiáltak **Event Hubs** névtér. Keresse meg az Event Hubs irányítópult toohello hello [Azure-portálon](https://manage.windowsazure.com), hello kattintson **konfigurálása** lapot, és hozzon létre egy elnevezett házirendet (például "SendRule"), amely rendelkezik *küldése* engedélyek. Hello **StorageAccount** deklarálva van a **PrivateConfig**. Nincs szükség az itt toochange értékek Ha működnek. Ebben a példában a Microsoft hello értékek üresen hagyja, vagyis a jele, hogy egy alárendelt eszköz állítja be hello értékek. Például hello *ServiceConfiguration.Cloud.cscfg* környezet konfigurációs fájl beállítja hello környezet megfelelő neveket és a kulcsok.  

> [!WARNING]
> hello Event Hubs SAS-kulcsot a hello egyszerű szövegként vannak tárolva *.wadcfgx* fájlt. Gyakran ezt a kulcsot be van jelölve, a toosource ellenőrző vagy a build Server eszközként elérhető, megfelelő módon kell védeni. Azt javasoljuk, hogy egy SAS-kulcsot használ itt a *csak küldése* engedélyeit, hogy egy rosszindulatú felhasználó írhat toohello eseményközpont, de nem tooit figyelésére vagy az adatbázis felügyeletét.
>
>

## <a name="configure-azure-diagnostics-toosend-logs-and-metrics-tooevent-hubs"></a>Konfigurálja az Azure Diagnostics toosend naplók és a metrikák tooEvent hubok
Című szakaszban leírtaknak megfelelően korábban, az összes alapértelmezett és egyéni diagnosztikai adatainak, ez azt jelenti, metrikákat és a naplókat, automatikusan küldött tooAzure tárolási konfigurált hello időközök. Az Event Hubs és a további fogadó a legfelső szintű vagy levél csomópont hello hierarchia toobe toohello eseményközpont küldött adhat meg. Ez magában foglalja az ETW-események, a teljesítményszámlálók, a Windows eseménynaplóiban keresse meg és az alkalmazásnaplók.   

Fontos, hány adatpontok ténylegesen kell tooconsider tooEvent hubok át. Általában a fejlesztők adatátvitelt kis késleltetésű közbeni elérési útja, amely kell használni, és gyorsan értelmezi. Riasztások vagy az automatikus skálázási szabályok figyelő rendszerek példák. A fejlesztők is előfordulhat, hogy egy másik elemzési tároló konfigurálása és keressen rá az áruházban – például Azure Stream Analytics, Elasticsearch, egy egyéni figyelési rendszer vagy más kedvenc figyelési rendszer.

hello az alábbiakban néhány példa konfigurációkat.

```xml
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
</PerformanceCounters>
```
```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "sinks": "HotPath",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        }
    ]
}
```

A fenti példa hello, hello fogadó esetében alkalmazott toohello szülő **PerformanceCounters** hello hierarchiában, ami azt jelenti, hogy minden gyermek csomópont **PerformanceCounters** tooEvent hubok küldi.  

```xml
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```
```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Rejected",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Queued",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        }
    ]
}
```

Hello előző példában hello fogadó alkalmazott tooonly három számláló áll: **kérései**, **elutasítja kérelmeket**, és **processzoridő százalékos**.  

hello következő példa bemutatja, hogyan fejlesztők is korlátozását hello elküldött adatok toobe hello kritikus metrikákat, amelyeket a szolgáltatás állapotát.  

```XML
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```
```JSON
"Logs": {
    "scheduledTransferPeriod": "PT1M",
    "scheduledTransferLogLevelFilter": "Error",
    "sinks": "HotPath"
}
```

Ebben a példában hello fogadó alkalmazott toologs és szűrt csak tooerror szintű nyomkövetési.

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Központi telepítése és a Cloud Services alkalmazás- és diagnosztikai konfiguráció frissítése
A Visual Studio hello legegyszerűbb elérési toodeploy hello alkalmazás és az Event Hubs fogadó konfigurációs biztosít. tooview és Szerkesztés hello fájlt, nyissa meg hello *.wadcfgx* fájlt a Visual Studio, szerkesztheti és mentheti. hello elérési út **Felhőszolgáltatás-projekt** > **szerepkörök** > **(RoleName)** > **diagnostics.wadcfgx**.  

Ezen a ponton az összes központi telepítés és frissítés a Visual Studio, a Visual Studio Team System, és minden parancsokkal és parancsprogramokkal MSBuild alapulnak, és hello használó műveletek **/t: közzététele** cél tartalmaznak hello *.wadcfgx*  hello csomagolás folyamat során. Emellett központi telepítések és frissítések központi telepítését hello fájl tooAzure hello megfelelő Azure diagnosztikai ügynök bővítményt a virtuális gépeken.

Hello alkalmazás és az Azure diagnosztikai konfiguráció telepítése után hello irányítópult hello az event hubs tevékenység azonnal látni fogja. Ez azt jelzi, hogy róla, hogy készen áll a toomove adatokon tooviewing hello közbeni elérési hello figyelő ügyfél vagy elemzésük az Ön által választott eszközben.  

A következő ábra hello hello Event Hubs irányítópult jeleníti meg, megfelelő küld diagnosztikai adatok toohello event hub kiindulási némi várakozás után 23 óra. Ha ez hello alkalmazás lett telepítve, a frissített *.wadcfgx* fájlt, és hello fogadó lett megfelelően konfigurálva.

![][0]  

> [!NOTE]
> Ha frissítések toohello Azure Diagnostics konfigurációs fájljában (.wadcfgx), javasoljuk, hogy leküldéses hello frissítések toohello teljes alkalmazás, valamint a hello konfigurációs Visual Studio közzététel, vagy a Windows PowerShell-parancsfájl használatával.  
>
>

## <a name="view-hot-path-data"></a>Közbeni elérési adatok megtekintése
Korábban bemutatott, nincsenek az Event Hubs adatfeldolgozás figyelő tooand sok alkalmazási helyzetei.

Egy egyszerű módszer egy kis tesztelési konzol alkalmazás toolisten toohello eseményközpont toocreate és nyomtatási hello kimeneti adatfolyam. A következő kódra, amely részletesen kifejtett hello elhelyezheti [Bevezetés az Event Hubs használatába](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)), egy konzolalkalmazásban.  

Vegye figyelembe, hogy hello Konzolalkalmazás tartalmaznia kell hello [esemény processzor állomás NuGet-csomag](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).  

Ne feledje tooreplace hello értékek hello a csúcsos zárójelek **fő** függvény erőforrások értékekkel.   

```csharp
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key toostop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sinks"></a>Az Event Hubs mosdók hibaelhárítása
* hello event hubs nem szerepelnek a bejövő vagy kimenő eseményhez kapcsolódó tevékenység várt módon.

    Ellenőrizze, hogy az eseményközpont sikeresen lett kiépítve. A hello összes Kapcsolatinformáció **PrivateConfig** szakasza *.wadcfgx* az erőforrás hello értékének egyeznie kell hello portal látható módon. Győződjön meg arról, hogy rendelkezik-e a biztonsági Társítások házirend lett meghatározva (hello példában "SendRule" jelöli) hello portal, valamint *küldése* engedélyt.  
* Egy adott frissítés után hello event hubs nem bejövő vagy kimenő eseményhez kapcsolódó tevékenység jelenik meg.

    Először is győződjön meg arról, hogy hello eseményközpont, és a konfigurációs adatok helyességéről, amint azt korábban. Egyes esetekben hello **PrivateConfig** az egy központi telepítési alaphelyzetbe áll. hello ajánlott javítás minden változás túl toomake*.wadcfgx* a projekt hello és a teljes alkalmazás frissítés majd leküldéses. Ha ez nem lehetséges, győződjön meg arról, hogy hello diagnosztika update leküldéses értesítések teljes **PrivateConfig** , amely tartalmazza a hello SAS-kulcsot.  
* Hello javaslatok próbáltam, és hello eseményközpont továbbra sem működik.

    Nézzük meg magát Azure diagnosztikai naplók és a hibákat tartalmazó hello Azure Storage tábla: **WADDiagnosticInfrastructureLogsTable**. Egy elem toouse olyan eszköz, például [Azure Tártallózó](http://www.storageexplorer.com) tooconnect toothis tárfiókot, ebben a táblázatban megtekintheti, és adja hozzá a lekérdezés az időbélyegzési hello utolsó 24 órában. Hello eszköz tooexport egy CSV-fájlt használ, és nyissa meg a Microsoft Excel alkalmazásban. Excel segítségével könnyen toosearch-kártya karakterláncok, például a **EventHubs**, toosee milyen hibát jelez.  

## <a name="next-steps"></a>Következő lépések
• [További információ az Event Hubs](https://azure.microsoft.com/services/event-hubs/)

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>A függelék: Végezze el az Azure Diagnostics konfigurációs fájl (.wadcfgx) – Példa
```xml
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount>ACCOUNT_NAME</StorageAccount>
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="{account name}" key="{account key}" endpoint="{storage endpoint}" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

kiegészítő hello *ServiceConfiguration.Cloud.cscfg* az ebben a példában a következőhöz hasonló, hello.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

A virtuális gépek egyenértékű alapú Json-beállítások a következőképpen történik:
```JSON
"settings": {
    "WadCfg": {
        "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": 4096,
            "sinks": "applicationInsights.errors",
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "Directories": {
                "scheduledTransferPeriod": "PT1M",
                "IISLogs": {
                    "containerName": "wad-iis-logfiles"
                },
                "FailedRequestLogs": {
                    "containerName": "wad-failedrequestlogs"
                }
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "sinks": "HotPath",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Memory\\Available MBytes",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Web Service(_Total)\\Bytes Total/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Errors Total/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET\\Requests Queued",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET\\Requests Rejected",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                        "sampleRate": "PT3M"
                    }
                ]
            },
            "WindowsEventLog": {
                "scheduledTransferPeriod": "PT1M",
                "DataSource": [
                    {
                        "name": "Application!*"
                    }
                ]
            },
            "Logs": {
                "scheduledTransferPeriod": "PT1M",
                "scheduledTransferLogLevelFilter": "Error",
                "sinks": "HotPath"
            }
        },
        "SinksConfig": {
            "Sink": [
                {
                    "name": "HotPath",
                    "EventHub": {
                        "Url": "https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py",
                        "SharedAccessKeyName": "SendRule"
                    }
                },
                {
                    "name": "applicationInsights",
                    "ApplicationInsights": "",
                    "Channels": {
                        "Channel": [
                            {
                                "logLevel": "Error",
                                "name": "errors"
                            }
                        ]
                    }
                }
            ]
        }
    },
    "StorageAccount": "{account name}"
}


"protectedSettings": {
    "storageAccountName": "{account name}",
    "storageAccountKey": "{account key}",
    "storageAccountEndPoint": "{storage endpoint}",
    "EventHub": {
        "Url": "https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "YOUR_KEY_HERE"
    }
}
```

## <a name="next-steps"></a>Következő lépések
További információ az Event Hubs érhetők el a következő hivatkozások hello:

* [Event Hubs – áttekintés](../event-hubs/event-hubs-what-is-event-hubs.md)
* [Eseményközpont létrehozása](../event-hubs/event-hubs-create.md)
* [Event Hubs – gyakori kérdések](../event-hubs/event-hubs-faq.md)

<!-- Images. -->
[0]: ../event-hubs/media/event-hubs-streaming-azure-diags-data/dashboard.png
